---
title: Přehrávání videa Web
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 8326c142207e90f9b7d4bced7effd88ec88d8fa8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="playing-a-web-video"></a>Přehrávání videa Web

`VideoPlayer` Třída definuje `Source` vlastnost slouží k zadání zdrojového souboru videa, a také `AutoPlay` vlastnost. `AutoPlay` má výchozí nastavení pro `true`, což znamená, že by měl začínat na video přehrávání automaticky po `Source` byla nastavena:

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

`Source` Vlastnost je typu `VideoSource`, což je vzorované po platformě Xamarin.Forms [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) abstraktní třída a odvozené tři, [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/), [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/), a [ `StreamImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/). Žádná možnost datového proudu je k dispozici pro `VideoPlayer` ale protože iOS a Android nepodporují přehrávání videa z datového proudu.

## <a name="video-sources"></a>Video zdroje

Abstraktní `VideoSource` třídy se skládá pouze z tři statické metody, které doložit tří tříd, které jsou odvozeny od `VideoSource`:

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

`UriVideoSource` Třída se používá k určení soubor ke stažení video s identifikátorem URI. Definuje vlastnosti jediné typu `string`:

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

Zpracování objektů typu `UriVideoSource` je popsáno níže.

`ResourceVideoSource` Třída se používá pro přístup k video soubory, které jsou uloženy jako vložené prostředky v aplikaci platformy také zadaným `string` vlastnost:

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

Zpracování objektů typu `ResourceVideoSource` je popsána v článku [načítání videa prostředků aplikace](loading-resources.md). `VideoPlayer` Třída nemá žádné zařízení se načíst soubor videa uložený jako prostředek v knihovny přenosných tříd.

`FileVideoSource` Třída se používá pro přístup k video soubory z knihovny video zařízení. Je také jedinou vlastnost typu `string`:

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

Zpracování objektů typu `FileVideoSource` je popsána v článku [přístup k zařízení Video knihovny](accessing-library.md).

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

Tento převaděč typ je voláno, když `Source` je nastavena na řetězec v jazyce XAML. Tady je `VideoSourceConverter` třídy:

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

`ConvertFromInvariantString` Metoda se pokusí převést řetězec, který má `Uri` objektu. Pokud tato operace úspěšná, a schéma není `file:`, potom metoda vrátí `UriVideoSource`. Funkce `ResourceVideoSource`.

## <a name="setting-the-video-source"></a>Nastavení zdroj videa

Všechny ostatní logiku zahrnující video zdroje je implementovaná v nástroji pro vykreslování jednotlivé platformy. Následující části vysvětlují, jak pro vykreslování platformy přehrávání videa při `Source` je nastavena na `UriVideoSource` objektu.

### <a name="the-ios-video-source"></a>Zdroj videa iOS

Dva oddíly `VideoPlayerRenderer` se účastní nastavení video zdroj přehrávání videa. Když Xamarin.Forms nejprve vytvoří objekt typu `VideoPlayer`, `OnElementChanged` metoda je volána s `NewElement` vlastnost objektu argumenty nastavena na který `VideoPlayer`. `OnElementChanged` Volání metod `SetSource`:

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

Později na, kdy `Source` vlastnost je změnit, `OnElementPropertyChanged` metoda je volána s `PropertyName` vlastnost "Zdroje" a `SetSource` nebude volán znovu.

Přehrávání videa soubor v iOS, objekt typu [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) je poprvé vytvořen pro zapouzdření souboru videa, a který se používá k vytvoření [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), který je pak předávána `AVPlayer`objektu. Tady je jak `SetSource` metoda zpracovává `Source` vlastnost po typu `UriVideoSource`:

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

`AutoPlay` Vlastnost nemá žádné analogovým v iOS video třídy, tak vlastnost je zkontrolován na konci `SetSource` metoda k volání `Play` metodu `AVPlayer` objektu.

V některých případech dál přehrávání po stránku s videa `VideoPlayer` přešli zpět na domovskou stránku. Zastavit video, `ReplaceCurrentItemWithPlayerItem` nastavena v `Dispose` přepsání:

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

Android `VideoPlayerRenderer` je třeba nastavit zdroj videa přehrávače při `VideoPlayer` nejprve je vytvořený a novějším při `Source` změny vlastností:

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

`SetSource` Metoda zpracovává objekty typu `UriVideoSource` voláním `SetVideoUri` na `VideoView` s Android `Uri` objekt vytvořený z řetězce identifikátoru URI. `Uri` Třída je plně kvalifikovaný sem ho odlišuje od .NET `Uri` třídy:

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

Android `VideoView` nemá odpovídající `AutoPlay` vlastnost, proto `Start` metoda je volána, pokud byla nastavena nové video.

Pokud je rozdíl mezi chování iOS a Android nástroji pro vykreslování `Source` vlastnost `VideoPlayer` je nastaven na `null`, nebo pokud `Uri` vlastnost `UriVideoSource` je nastaven na `null` nebo je prázdný. Pokud přehrávání videa iOS je momentálně přehrávání videa, a `Source` je nastaven na `null` (nebo je řetězec `null` nebo je prázdný), `ReplaceCurrentItemWithPlayerItem` je volán s `null` hodnotu. Aktuální video se nahradí a zastaví přehrávání.

Android nepodporuje budovy podobné. Pokud `Source` je nastavena na `null`, `SetSource` metoda jednoduše ignoruje a aktuální video pokračuje přehrávání.

### <a name="the-uwp-video-source"></a>Zdroj videa UWP

UWP `MediaElement` definuje `AutoPlay` vlastnosti, které se zpracovávají v zobrazovací jednotky jako ostatní vlastnosti:

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

`SetSource` Popisovače vlastností `UriVideoSource` objekt nastavením `Source` vlastnost `MediaElement` pro .NET `Uri` hodnotu, nebo `null` Pokud `Source` vlastnost `VideoPlayer` je nastaven na `null`:

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

S implementací těchto vlastností v tři nástroji pro vykreslování je možné přehrát video z adresy URL zdroje. **Přehrát Video webové** stránku [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) programu je definován v následujícím souboru XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` Třída převede řetězec na `UriVideoSource`. Když přejdete **přehrávání videa webové** stránky, na video začíná načítání a spustí přehrávání při dostatečné množství dat po stažení a uložená do vyrovnávací paměti. Je přibližně 10 minut, délka:

[![Přehrávání videa webové](web-videos-images/playwebvideo-small.png "přehrát Video webové")](web-videos-images/playwebvideo-large.png#lightbox "přehrát Web Video")

Na každé tři platformy objevovat přenosu ovládací prvky, pokud se nepoužívají, ale může být obnovena zobrazíte klepnutím na video.

Video můžete zabránit automatickému spuštění nastavením `AutoPlay` vlastnost `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

Budete muset stiskněte **přehrání** tlačítko video spustíte.

Podobně můžete potlačit zobrazení ovládacích prvků přenosu nastavením `AreTransportControlsEnabled` vlastnost `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

Pokud nastavíte obě vlastnosti na `false`, videa nezahájí, přehrávání a budou existovat žádný způsob, jak jej spustit! Je třeba volat `Play` ze souboru kódu na pozadí, nebo vytvořte vlastní ovládací prvky přenosu, jak je popsáno v článku [implementace vlastní přenos ovládací prvky Video](custom-transport.md). 

**App.xaml** soubor obsahuje prostředky pro dva další videa:

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

Odkaz na některou z těchto dalších filmy, můžete nahradit explicitní adresa URL v **PlayWebVideo.xaml** soubor s `StaticResource` – rozšíření značek, v takovém případě `VideoSourceConverter` není potřeba vytvořit `UriVideoSource` objektu:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

Alternativně můžete nastavit `Source` vlastnost ze souboru videa v `ListView`, jak je popsáno v článku na další [vazby Video zdroje přehrávači](source-bindings.md).





## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
