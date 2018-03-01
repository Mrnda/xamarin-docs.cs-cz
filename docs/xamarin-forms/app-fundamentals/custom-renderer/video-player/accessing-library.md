---
title: "Přístup ke knihovně videí zařízení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 95f992c2f3ca976c1e02ff3043ceef0a4a81e4f6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="accessing-the-devices-video-library"></a>Přístup ke knihovně videí zařízení

Většina moderních mobilních zařízení a stolních počítačů mít možnost záznam videa pomocí fotoaparátu zařízení. Videa, která uživatel vytvoří jsou pak uloženy jako soubory v zařízení. Tyto soubory můžete načíst z bitové kopie knihovny a kterou hraje `VideoPlayer` třída stejně jako ostatní video.

## <a name="the-photo-picker-dependency-service"></a>Službu fotografií výběr závislostí

Všechny tři platformy obsahuje funkci, která umožňuje uživateli vybrat fotku nebo video z bitové kopie knihovny zařízení. Prvním krokem při přehrávání videa z knihovna obrázků v zařízení vytváří služba závislost, která volá výběr image na každou platformu. Je velmi podobné definovaná v závislostí služby popsané níže [ **výběr fotografie z knihovny obrázků** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) článku, s tím rozdílem, že video výběr vrátí název souboru, nikoli `Stream`objektu.

Definuje rozhraní s názvem projektu PCL `IVideoPicker` pro službu závislost:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Všechny tři platformy obsahuje třídu s názvem `VideoPicker` které toto rozhraní implementuje.

### <a name="the-ios-video-picker"></a>Nástroje pro výběr video iOS

IOS `VideoPicker` používá iOS [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) přístup do bitové kopie knihovny určující, zda by mělo být omezeno videa (označované jako "filmy") na webu iOS `MediaType` vlastnost. Všimněte si, že `VideoPicker` explicitně implementuje `IVideoPicker` rozhraní. Všimněte si také `Dependency` atribut, který identifikuje tuto třídu jako závislé služby. Toto jsou dva požadavky, které umožňují Xamarin.Forms hledání závislostí služby v projektu platformy:

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
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

### <a name="the-android-video-picker"></a>Android video výběr.

Android implementace `IVideoPicker` vyžaduje metoda zpětného volání, která je součástí aktivitě dané aplikace. Z tohoto důvodu `MainActivity` třída definuje dvě vlastnosti a pole, metody zpětného volání:

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnCreate` Metoda v `MainActivity` ukládá svou vlastní instanci statických `Current` vlastnost. To umožňuje implementace `IVideoPicker` získat `MainActivity` instance pro spuštění **vyberte Video** výběru:

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Doplňky k `MainActivity` objekt jsou pouze kód [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) řešení, kde kód normální aplikace musí být změněn na podporu `FormsVideoLibrary` třídy.

### <a name="the-uwp-video-picker"></a>Nástroje pro výběr video UWP

Implementace UWP `IVideoPicker` rozhraní používá UWP [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Začne hledat soubor s knihovnou obrázky a omezuje typy souborů MP4 a WMV (Windows Media Video):

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>Vyvolání závislostí služby

**Přehrávání videa knihovny** stránky [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) program ukazuje, jak používat službu video výběr závislostí. Obsahuje soubor XAML `VideoPlayer` instance a `Button` s názvem bez přípony **zobrazit knihovna videa**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

Obsahuje souboru kódu `Clicked` obslužné rutiny pro `Button`. Vyvolání závislostí služby vyžaduje volání `DependencyService.Get` získat implementace `IVideoPicker` rozhraní v projektu platformy. `GetVideoFileAsync` Metoda je volána poté v dané instanci:

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

`Clicked` Obslužná rutina pak používá tento název souboru k vytvoření `FileVideoSource` objektu a nastavte ji na `Source` vlastnost `VideoPlayer`.

Každý z `VideoPlayerRenderer` třídy obsahuje kód v jeho `SetSource` metoda pro objekty typu `FileVideoSource`. Níže jsou uvedeny tyto:

### <a name="handling-ios-files"></a>Zpracování souborů iOS

Verze iOS `VideoPlayerRenderer` procesy `FileVideoSource` objekty pomocí statické `Asset.FromUrl` metodu se název souboru. Tím vytvoří `AVAsset` objekt představující soubor bitové kopie knihovny zařízení:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Zpracování Android soubory

Při zpracování objekty typu `FileVideoSource`, Android implementace `VideoPlayerRenderer` používá `SetVideoPath` metodu `VideoView` a zadejte soubor bitové kopie knihovny zařízení:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>Zpracování souborů UWP

Při zpracování objekty typu `FileVideoSource`, implementace UWP `SetSource` Metoda potřebuje k vytvoření `StorageFile` objektu, otevřete tento soubor pro čtení a předat objekt datový proud, který má `SetSource` metodu `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

Pro všechny tři platformy, na video začne přehrávání téměř okamžitě po video zdroj je nastavit, protože soubor je na zařízení a není nutné stáhnout.



## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [Výběr fotografie z knihovny obrázků](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
