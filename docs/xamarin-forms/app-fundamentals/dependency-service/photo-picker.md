---
title: Výběr z knihovny obrázků fotografie
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms DependencyService vybrat fotografii z knihovny obrázků v telefonu.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: dafa60ff57f34bd4169af48e380079d9637d8d26
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241104"
---
# <a name="picking-a-photo-from-the-picture-library"></a>Výběr z knihovny obrázků fotografie

Tento článek vás provede vytvořením aplikace, která umožňuje uživateli vybrat fotografii z knihovny obrázků v telefonu. Protože Xamarin.Forms nezahrnuje tuto funkci, je nutné použít [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) pro přístup k nativním rozhraním API na každou platformu.  Tento článek se zabývá následující pokyny pro použití `DependencyService` pro tuto úlohu:

- **[Vytvoření rozhraní](#Creating_the_Interface)**  &ndash; pochopit, jak se v sdílený kód vytvoří rozhraní.
- **[iOS implementace](#iOS_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro iOS.
- **[Android implementace](#Android_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro Android.
- **[Implementace pro univerzální platformu Windows](#UWP_Implementation)**  &ndash; zjistěte, jak implementovat rozhraní v nativním kódu pro univerzální platformu Windows (UPW).
- **[Implementace v sdíleným kódem](#Implementing_in_Shared_Code)**  &ndash; Další informace o použití `DependencyService` Chcete-li volat nativní implementaci ze sdíleného kódu.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytvoření rozhraní

Nejprve vytvořte rozhraní v sdíleného kódu, který vyjadřuje požadované funkce. V případě aplikace fotky vybere pouze jedním ze způsobů je požadovaná. Toto je definováno v [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) rozhraní v knihovně .NET Standard ukázkového kódu:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` Metoda je definována jako asynchronní, protože metoda musí vracet rychle, ale nemůže vrátit `Stream` pro vybrané fotografie, dokud uživatel procházet knihovnu obrázků a vybrali jeden objekt.

Toto rozhraní je implementováno ve všech platformách pomocí kódu pro konkrétní platformu.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS implementace

Implementace iOS `IPicturePicker` rozhraní používá [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) jak je popsáno v [ **vybírat fotografii do Galerie** ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) recept a [ukázkový kód](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Je součástí implementace iOS [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) třídu v projektu pro iOS ukázkového kódu. Aby tato třída viditelná pro `DependencyService` správce, musí být určen třídy s [`assembly`] atribut typu `Dependency`, a musí být veřejné a explicitní implementace třídy `IPicturePicker` rozhraní:

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

`GetImageStreamAsync` Metoda vytvoří `UIImagePickerController` a inicializuje ji vybrat Image z knihovny fotografií. Dvě obslužné rutiny událostí jsou povinné: jeden pro když uživatel vybere fotografii a druhou pro když uživatel zruší zobrazení knihovny fotografií. `PresentModalViewController` Uživateli zobrazí knihovny fotografií.

V tomto okamžiku `GetImageStreamAsync` metoda musí vracet `Task<Stream>` objekt kódu, který je volání. Tento úkol je dokončen, pouze v případě, že uživatel dokončil interakci s knihovny fotografií a jeden z obslužné rutiny událostí je volána. V situacích, jako je to [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) třída je základní. Poskytuje třídy `Task` objekt správné obecného typu má vrátit z `GetImageStreamAsync` metody a třídy lze později být signalizován při dokončení úlohy.

`FinishedPickingMedia` Obslužná rutina události je volána, když uživatel vybral obrázku. Však poskytuje obslužné rutiny `UIImage` objektu a `Task` musí vracet .NET `Stream` objektu. To se provádí ve dvou krocích: `UIImage` objekt nejprve převeden na soubor JPEG v uložených v paměti `NSData` objektu a pak `NSData` objekt je převést na .NET `Stream` objektu. Volání `SetResult` metodu `TaskCompletionSource` objektu se dokončí úkol zadáním `Stream` objektu:

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

Aplikace pro iOS vyžaduje oprávnění uživatele pro přístup k telefonu knihovny fotografií. Přidejte následující text do `dict` část souboru Info.plist:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementace s androidem

Android implementace používá techniky popsané v [ **vyberte bitovou kopii** ](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) recept a [ukázkový kód](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Metoda, která je volána, když uživatel vybral bitovou kopii z knihovny obrázků ale `OnActivityResult` přepsat ve třídě, která je odvozena z `Activity`. Z tohoto důvodu normální [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) třídu v projektu pro Android má byla doplněna pole, vlastnosti a přepsání `OnActivityResult` metody:

```csharp
public class MainActivity : FormsAppCompatActivity
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

`OnActivityResult`Přepsání označuje vybraný obrázek souboru pomocí aplikace pro Android `Uri` objektu, ale toto je možné převést do .NET `Stream` objektu voláním `OpenInputStream` metodu `ContentResolver` objekt, který byl získán z aktivity `ContentResolver` vlastnost.

Stejně jako implementace iOS, Android implementace používá `TaskCompletionSource` který signalizuje, že po dokončení úlohy. To `TaskCompletionSource` objekt je definovaný jako veřejné vlastnosti v `MainActivity` třídy. To umožňuje vlastnost, která má odkazovat [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) třídu v projektu pro Android. Toto je třída s `GetImageStreamAsync` metody:

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

Tato metoda umožňuje přístup `MainActivity` třídy několik důvodů: pro `Instance` vlastnost, pro `PickImageId` pole pro `TaskCompletionSource` vlastnost a k volání `StartActivityForResult`. Tato metoda je definována `FormsAppCompatActivity` třídu, která je základní třídou z `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>Implementace UPW

Na rozdíl od implementace Android a iOS se nevyžaduje implementace nástroje pro výběr fotografií pro univerzální platformu Windows `TaskCompletionSource` třídy. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) Třídy používá [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) třídy pro získání přístupu do knihovny fotografií. Protože `PickSingleFileAsync` metoda `FileOpenPicker` samotného je asynchronní, `GetImageStreamAsync` můžete jednoduše použít metoda `await` pomocí dané metody (a jiné asynchronní metody) a vraťte se `Stream` objektu:


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

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementace v sdíleným kódem

Teď, když rozhraní implementován pro každou platformu, aplikaci knihovny .NET Standard můžete jej využít.

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) Vytvoří třídu `Button` vybrat fotografii:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` Obslužná rutina používá `DependencyService` třídy volání `GetImageStreamAsync`. Výsledkem volání projektu platformy. Pokud metoda vrátí `Stream` objektu, pak vytvoří obslužnou rutinu `Image` – element pro daný obrázek s `TabGestureRecognizer`a nahradí `StackLayout` na stránce, která `Image`:

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

Klepnutím `Image` element na stránce se vrátí do normálu.


## <a name="related-links"></a>Související odkazy

- [Zvolte fotografii v galerii (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [Vyberte bitovou kopii (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
- [DependencyService (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
