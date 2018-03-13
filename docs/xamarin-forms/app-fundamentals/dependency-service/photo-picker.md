---
title: "Výběr fotografie z knihovny obrázků"
description: "Použijte DependencyService a vyberte fotografie z knihovny obrázků telefonu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: db7f5058983195c0dcea9505f5adcd0fd03f905d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="picking-a-photo-from-the-picture-library"></a>Výběr fotografie z knihovny obrázků

Tento článek vás provede vytváření aplikace, která umožňuje uživateli vybrat fotografie z knihovny obrázků telefonu. Protože Xamarin.Forms nezahrnuje tuto funkci, je nutné použít [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) pro přístup k nativních rozhraní API na každou platformu.  Tento článek se zabývá následující kroky pro používání `DependencyService` k této úloze:

- **[Vytváření rozhraní](#Creating_the_Interface)**  &ndash; pochopit vytváření rozhraní ve sdílené kódu.
- **[iOS implementace](#iOS_Implementation)**  &ndash; zjistěte, jak toto rozhraní implementovat v nativním kódu pro iOS.
- **[Android implementace](#Android_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Android.
- **[Universal Windows Platform implementace](#UWP_Implementation)**  &ndash; zjistěte, jak toto rozhraní implementovat v nativním kódu pro univerzální platformu Windows (UWP).
- **[Windows Phone implementace](#Windows_Phone_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Windows Phone 8.1.
- **[Implementace v sdíleného kódu](#Implementing_in_Shared_Code)**  &ndash; Naučte se používat `DependencyService` provést volání do nativního implementace ze sdíleného kódu.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytváření rozhraní

Nejprve vytvořte rozhraní v sdíleného kódu, která vyjadřuje požadované funkce. V případě fotografií výdej aplikace je vyžaduje právě jednu metodu. To je definována v [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) rozhraní v knihovny přenosných tříd ukázkového kódu:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` Metoda je definován jako asynchronní, protože metoda musí vracet rychle, ale nemůže vrátit `Stream` objektu pro vybrané fotografie, dokud se uživatel má procházet knihovny obrázků a vybrali jednu.

Toto rozhraní je implementováno ve všech platformách pomocí kódu pro konkrétní platformu.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS implementace

Implementace iOS `IPicturePicker` rozhraní používá [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) jak je popsáno v [ **zvolte fotografie z Galerie** ](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) recepturách a [ukázkový kód](https://github.com/xamarin/recipes/tree/master/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Implementace iOS je součástí [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) třídu do projektu iOS ukázkový kód. Zpřístupněte Tato třída `DependencyService` správci zjistit třídu, musí se [`assembly`] atribut typu `Dependency`, musí být veřejné a explicitní implementace třídy `IPicturePicker` rozhraní:

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

`GetImageStreamAsync` Metoda vytvoří `UIImagePickerController` a inicializuje ho vybrat obrázky z knihovny fotografii. Jsou požadovány dva obslužné rutiny událostí: jeden pro když uživatel vybere fotografie a druhou pro Pokud uživatel zruší zobrazení fotografií knihovny. `PresentModalViewController` Pak zobrazí knihovně fotografií pro uživatele.

V tomto okamžiku `GetImageStreamAsync` metoda musí vrátit `Task<Stream>` objekt, který má kód, který je volání. Tato úloha byla dokončena pouze v případě, že uživatel dokončil interakci s knihovnou fotografie a jeden z obslužné rutiny událostí je volána. Pro situace, jako to [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) třída je nezbytné. Poskytuje třídy `Task` správné obecného typu vrácení z objektu `GetImageStreamAsync` metoda a třídu můžete později být signál, když je úloha dokončena.

`FinishedPickingMedia` Obslužné rutiny události je volána, když uživatel vybral obrázku. Však poskytuje obslužné rutiny `UIImage` objektu a `Task` musí vracet .NET `Stream` objektu. To se provádí ve dvou krocích: `UIImage` objekt nejprve převeden do souboru JPEG v uložené v paměti `NSData` objekt a potom `NSData` objekt je převést na .NET `Stream` objektu. Volání `SetResult` metodu `TaskComkpletionSource` objekt dokončí úlohu zadáním `Stream` objektu:

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data = image.AsJPEG(1);
                Stream stream = data.AsStream();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}

```

Aplikace pro iOS vyžaduje oprávnění od uživatele pro přístup k telefonu fotografií knihovny. Přidejte následující `dict` v souboru Info.plist:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android implementace

Android implementace používá podle postupu popsaného v [ **Vybrat bitovou kopii** ](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) recepturách a [ukázkový kód](https://github.com/xamarin/recipes/tree/master/android/other_ux/pick_image). Je však metoda, která je volána, když uživatel vybral bitovou kopii z knihovny obrázků `OnActivityResult` potlačení v třídu odvozenou z `Activity`. Z tohoto důvodu normální [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) třídy v projektu pro Android má byla doplněná pole, vlastnosti a přepsání `OnActivityResult` metoda:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
{
    ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}

```

`OnActivityResult`Přepsání Určuje soubor vybraný obrázek se Android `Uri` objektu, ale to může být převedeny do .NET `Stream` objekt voláním `OpenInputStream` metodu `ContentResolver` objekt, který byl získán z aktivity `ContentResolver` vlastnost.

Jako implementace iOS, Android implementace používá `TaskCompletionSource` signál po dokončení úlohy. To `TaskCompletionSource` objektu je definována jako veřejné vlastnosti v `MainActivity` třídy. To umožňuje vlastnost odkazovat ve [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) třídy v projektu pro Android. Toto je třída, která se `GetImageStreamAsync` metoda:

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Tato metoda umožňuje přístup `MainActivity` třída několik důvodů: pro `Instance` vlastnost, pro `PickImageId` pole pro `TaskCompletionSource` vlastnost a k volání `StartActivityForResult`. Tato metoda je definována `FormsApplicationActivity` třídy tedy základní třídu `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>Implementace UWP

Na rozdíl od iOS a Android implementace nevyžaduje implementace nástroje pro výběr fotografií pro univerzální platformu Windows `TaskCompletionSource` třídy. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) Třídy používá [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) třída získat přístup ke knihovně fotografii. Protože `PickSingleFileAsync` metodu `FileOpenPicker` je sám asynchronní, `GetImageStreamAsync` můžete jednoduše použít metoda `await` pomocí dané metody (a další asynchronní metody) a vrátit `Stream` objektu:


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-81-implementation"></a>Windows Phone 8.1 implementace

Windows Phone 8.1 implementace je podobná UWP implementace s výjimkou jeden velký rozdíl, který má významný vliv: `PickSingleFileAsync` metodu `FileOpenPicker` není k dispozici. Místo toho `GetImageStreamAsync` musí volat metoda `PickSingleFileAndContinue`. Tato metoda vrátí hodnotu, pokud fotografie knihovny se zobrazí uživateli, ale výběr uživatele fotografie signalizace voláním `OnActivated` metodu `App` třídy.

Z tohoto důvodu [ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/App.xaml.cs) vytvořené v projektu Windows Phone 8.1 v šabloně projektů Xamarin.Forms třídy byla doplněná vlastnost typu `TaskCompletionSource` a přepsání `OnActivated` metoda:

```csharp
namespace DependencyServiceSample.WinPhone
{
    public sealed partial class App : Application
    {
        ...
        public TaskCompletionSource<Stream> TaskCompletionSource { set; get; }

        protected async override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            IContinuationActivatedEventArgs continuationArgs = args as IContinuationActivatedEventArgs;

            if (continuationArgs != null &&
                continuationArgs.Kind == ActivationKind.PickFileContinuation)
            {
                FileOpenPickerContinuationEventArgs pickerArgs = args as FileOpenPickerContinuationEventArgs;

                if (pickerArgs.Files.Count > 0)
                {
                    // Get the file and a Stream
                    StorageFile storageFile = pickerArgs.Files[0];
                    IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
                    Stream stream = raStream.AsStreamForRead();

                    // Set the completion of the Task
                    TaskCompletionSource.SetResult(stream);
                }
                else
                {
                    TaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnActivated` Metoda může být volána pro několik příčin, například spuštění aplikace. Kód používat pouze pouze ty volání při otevření souboru výběr dokončilo a pak získá `Stream` objektu z `StorageFile`.

[ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/PicturePickerImplementation.cs) Třída obsahuje `GetImageStreamAsync` metodu, která vytvoří `FileOpenPicker` a volání `PickSingleFileAndContainue`:

```csharp
[assembly: Xamarin.Forms.Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.WinPhone
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Display the picker for a single file (resumes in OnActivated in App.xaml.cs)
            openPicker.PickSingleFileAndContinue();

            // Create a TaskCompletionSource stored in App.xaml.cs
            TaskCompletionSource<Stream> taskCompletionSource = new TaskCompletionSource<Stream>();
            (Application.Current as App).TaskCompletionSource = taskCompletionSource;

            // Return the Task object
            return taskCompletionSource.Task;
        }
    }
}

```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementace v sdíleného kódu

Teď, když byl implementován rozhraní pro každou platformu, aplikaci v běžné knihovny přenosných tříd můžete využít výhod ho.

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) Třída vytvoří `Button` vybrat fotografie:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` Obslužná rutina používá `DependencyService` třída volat `GetImageStreamAsync`. Výsledkem volání v projektu platformy. Pokud metoda vrátí `Stream` objektu, pak obslužná rutina vytvoří `Image` element pro tento obrázek s `TabGestureRecognizer`a nahradí `StackLayout` na stránce s třídou `Image`:

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

Klepnutím `Image` element vrátí stránku na normální.


## <a name="related-links"></a>Související odkazy

- [Zvolte fotografie z Galerie (iOS)](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [Vyberte bitovou kopii (Android)](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
