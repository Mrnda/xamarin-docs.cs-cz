---
title: Přehrávání webového videa
description: Tento článek vysvětluje, jak přehrávání webového videa do aplikace přehrávače videa pomocí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 566f056bd616c918ce274b9c7406d94fdc265ea2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994556"
---
# <a name="playing-a-web-video"></a>Přehrávání webového videa

`VideoPlayer` Definuje třídu `Source` vlastnost použít k určení zdrojového souboru videa, ale i `AutoPlay` vlastnost. `AutoPlay` má výchozí nastavení `true`, což znamená, že by měl video začne přehrávat automaticky po `Source` byla nastavena:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }

        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

`Source` Vlastnost je typu `VideoSource`, což je vzorované po Xamarin.Forms [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) abstraktní třída a odvozené tři, [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource), [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), a [ `StreamImageSource` ](xref:Xamarin.Forms.StreamImageSource). Žádný datový proud možnost je dostupná pro `VideoPlayer` však, protože zařízení s iOS a Android nepodporují přehrávání videa z datového proudu.

## <a name="video-sources"></a>Zdroje videa

Abstraktní `VideoSource` tvořen výhradně tři statické metody, které instanci tři třídy, které jsou odvozeny z třídy `VideoSource`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

`UriVideoSource` Třída se používá k určení souboru ke stažení videa s identifikátorem URI. Definuje jedinou vlastnost typ `string`:

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

Zpracování objektů typu `UriVideoSource` je popsána níže.

`ResourceVideoSource` Třída se používá pro přístup k videosoubory, které jsou uloženy jako vložené prostředky v aplikaci platformy, také se zadaným `string` vlastnost:

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

Zpracování objektů typu `ResourceVideoSource` je popsaný v článku [načítání videí prostředků aplikace](loading-resources.md). `VideoPlayer` Třída nemá žádné zařízení se načíst soubor videa uložené jako prostředek v knihovně .NET Standard.

`FileVideoSource` Třída se používá pro přístup k souborům videa z knihovny videí zařízení. Je také jedinou vlastnost typu `string`:

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

Zpracování objektů typu `FileVideoSource` je popsaný v článku [přístup k zařízení Videoknihovny](accessing-library.md).

`VideoSource` Obsahuje třídy `TypeConverter` atribut, který odkazuje na `VideoSourceConverter`:

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

Tento převaděč typu, je vyvolána při `Source` je nastavena na řetězec v XAML. Tady je `VideoSourceConverter` třídy:

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ?
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

`ConvertFromInvariantString` Metoda se pokusí převést řetězec, který má `Uri` objektu. Pokud tato operace úspěšná, a schéma není `file:`, vrátí metoda `UriVideoSource`. V opačném případě vrátí `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Nastavení zdroj videa

Všechny další logiku zahrnující zdroje videa je implementované v renderery jednotlivé platformy. Následující části vysvětlují, jak platforma renderery přehrávání videa při `Source` je nastavena na `UriVideoSource` objektu.

### <a name="the-ios-video-source"></a>Zdroj videa s Iosem

Dva oddíly `VideoPlayerRenderer` se podílejí nastavení zdroj videa přehrávače videa. Když Xamarin.Forms nejprve vytvoří objekt typu `VideoPlayer`, `OnElementChanged` metoda je volána `NewElement` vlastnost objekt arguments nastavena na hodnotu, která `VideoPlayer`. `OnElementChanged` Volání metody `SetSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

Později na, kdy `Source` změněna vlastnost `OnElementPropertyChanged` metoda je volána s `PropertyName` vlastnost "Zdroj", a `SetSource` volána znovu.

K přehrání videa souboru v Iosu, objekt typu [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) prvním vytvoření k zapouzdření souboru videa, která slouží k vytvoření [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), který je pak předán `AVPlayer`objektu. Tady je způsob, jakým `SetSource` metoda obslužné rutiny `Source` vlastnost je typu `UriVideoSource`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

`AutoPlay` Vlastnost nemá žádné analogové ve třídách videa s Iosem, na konci je zkontrolován vlastnost `SetSource` metoda se má volat `Play` metodu `AVPlayer` objektu.

V některých případech pokračování přehrávání za stránku s videa `VideoPlayer` přejde zpět na domovskou stránku. Zastavit na video `ReplaceCurrentItemWithPlayerItem` je také nastavena `Dispose` přepsat:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>Android zdroj videa

Android `VideoPlayerRenderer` nastavit zdroj přehrávače videa při `VideoPlayer` nejprve je vytvořená a v novějších při `Source` změny vlastností:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

`SetSource` Metoda zpracovává objekty typu `UriVideoSource` voláním `SetVideoUri` na `VideoView` s Androidem `Uri` objekt vytvořený z řetězce identifikátoru URI. `Uri` Třídy je plně kvalifikovaný tady ho odlišuje od .NET `Uri` třídy:

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

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

Android `VideoView` nemá odpovídající `AutoPlay` vlastnosti, takže `Start` metoda je volána, pokud byla nastavena nové video.

Pokud je rozdíl mezi chování systému iOS a Android renderery `Source` vlastnost `VideoPlayer` je nastavena na `null`, nebo pokud `Uri` vlastnost `UriVideoSource` je nastavena na `null` nebo prázdný řetězec. Pokud přehrávač videa iOS je momentálně přehrávání videa, a `Source` je nastavena na `null` (nebo je řetězec `null` nebo prázdné), `ReplaceCurrentItemWithPlayerItem` volána s `null` hodnotu. Aktuální videa se nahradí a zastaví přehrávání.

Podobně jako zařízení Android nejsou podporovány. Pokud `Source` je nastavena na `null`, `SetSource` metoda jednoduše ignoruje a aktuální videa i nadále přehrát.

### <a name="the-uwp-video-source"></a>Zdroj videa UPW

Na UPW `MediaElement` definuje `AutoPlay` vlastnosti, které se zpracovávají v renderer stejně jako jakoukoli jinou vlastnosti:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

`SetSource` Popisovačů vlastností `UriVideoSource` objektu tak, že nastavíte `Source` vlastnost `MediaElement` k .NET `Uri` hodnotu, nebo `null` Pokud `Source` vlastnost `VideoPlayer` je nastavena na `null`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>Nastavení adresy URL zdroje

K provádění těchto vlastností v tři renderery je možné k přehrání videa ze zdroje adresy URL. **Přehrávání webového videa** stránku [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) programu je definován v následujícím souboru XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` Převede řetězec na třídy `UriVideoSource`. Když přejdete **přehrávání webového videa** stránce videa začíná načítání a spustí přehrávání dostatečné množství dat po stažení a uložená do vyrovnávací paměti. Video je přibližně 10 minut:

[![Přehrávání webového videa](web-videos-images/playwebvideo-small.png "přehrávání webového videa")](web-videos-images/playwebvideo-large.png#lightbox "přehrávání webového videa")

Na všech třech platformách ovládací prvky pro přenos zesvětlit Pokud nejsou použity, ale je možné obnovit zobrazíte klepnutím na video.

Video můžete zabránit automatickému spuštění tak, že nastavíte `AutoPlay` vlastnost `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Bude nutné stisknout klávesu **Přehrát** tlačítko, na kterém se video spustí.

Podobně můžete potlačit zobrazení ovládací prvky pro přenos tak, že nastavíte `AreTransportControlsEnabled` vlastnost `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Pokud nastavíte na obě vlastnosti `false`, video se začne přehrávat a bude existovat žádný způsob, jak začít ho! Je třeba volat `Play` ze souboru kódu na pozadí nebo vytvořit vlastní ovládací prvky pro přenos, jak je popsáno v článku [implementace vlastní videa ovládací prvky pro přenos](custom-transport.md).

**App.xaml** soubor obsahuje prostředky pro další dvě videa:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />

        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Odkaz na některou z těchto filmy, můžete nahradit explicitní adresa URL v **PlayWebVideo.xaml** soubor s `StaticResource` – rozšíření značek, v takovém případě `VideoSourceConverter` není potřeba vytvořit `UriVideoSource` objektu:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternativně můžete nastavit `Source` vlastnost z videosouboru v `ListView`, jak je popsáno v následujícím článku [vazby zdroje videa k přehrávači](source-bindings.md).





## <a name="related-links"></a>Související odkazy

- [Ukázky přehrávače videa (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
