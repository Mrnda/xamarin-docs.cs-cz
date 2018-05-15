---
title: Vytváření přehrávačů videa platformy
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 9efdc8376d9970cb429654e3d3aa2eef75ac2996
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="creating-the-platform-video-players"></a>Vytváření přehrávačů videa platformy

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) řešení obsahuje všechny kód pro implementaci přehrávání videa pro Xamarin.Forms. Zahrnuje také řadu stránek, které ukazuje, jak používat přehrávání videa v rámci aplikace. Všechny `VideoPlayer` kód a jeho nástroji pro vykreslování platformy nacházejí v projektu složky s názvem `FormsVideoLibrary`a také použít obor názvů `FormsVideoLibrary`. To by měla zajistit snadno kopírovat soubory do vlastní aplikace a referenční třídy.

## <a name="the-video-player"></a>Přehrávání videa

[ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) Třída je součástí **VideoPlayerDemos** .NET standardní knihovny, která je sdílena mezi platformami. Je odvozena z `View`:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

Členy této třídy (a `IVideoPlayerController` rozhraní) jsou popsané v článcích, které následují.

Všechny tři platformy obsahuje třídu s názvem `VideoPlayerRenderer` obsahující kód specifický pro platformu pro implementaci přehrávání videa. Primární úlohy tento objekt pro vykreslení je vytvoření přehrávání videa pro platformu.

### <a name="the-ios-player-view-controller"></a>Řadiče zobrazení player iOS

Několik tříd se podílejí při implementaci přehrávání videa v iOS. Aplikace vytvoří nejprve [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) a nastaví [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) vlastností pro objekt typu [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/). Další třídy jsou potřeba při přehrávač je přiřazen zdroj videa.

Jako všemi nástroji pro vykreslování iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) obsahuje `ExportRenderer` atribut, který identifikuje `VideoPlayer` zobrazení s zobrazovací jednotky:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

Obecně vykreslovacího modulu, který nastaví ovládacího prvku platformy je odvozena z [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) třídy, kde `View` je platformě Xamarin.Forms `View` odvozených (v tomto případě `VideoPlayer`) a `NativeView` je pro iOS `UIView` odvozené třídy zobrazovací jednotky. Pro tento objekt pro vykreslení, že obecné argument jednoduše nastaven na hodnotu `UIView`, se krátce zobrazí z důvodů.

Pokud je na základě vykreslovací modul `UIViewController` odvozených (jako tato je), pak by měly přepsat třídy `ViewController` vlastností a vraťte se řadiče zobrazení, v tomto případě `AVPlayerViewController`. To znamená účel `_playerViewController` pole:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

Přímá odpovědnost `OnElementChanged` přepsání je potřeba zkontrolovat, když `Control` vlastnost je `null` a pokud ano, vytvoření ovládacího prvku platformy a předejte ji do `SetNativeControl` metoda. V takovém případě tento objekt je k dispozici pouze z `View` vlastnost `AVPlayerViewController`. Aby `UIView` odvozených se stane být privátní třídy s názvem `AVPlayerView`, ale protože je privátní, nemůže být zadaný explicitně jako druhý argument Obecné `ViewRenderer`.

Obecně `Control` vlastnost třídy zobrazovací jednotky následně odkazuje `UIView` použít k implementaci zobrazovací jednotky, ale v takovém případě `Control` vlastnost nepoužívá jinde.

### <a name="the-android-video-view"></a>Zobrazení Android video

Android zobrazovací jednotky pro `VideoPlayer` je založena na Android [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) třídy. Ale pokud `VideoView` se používá samostatně k přehrávání videa v aplikaci Xamarin.Forms, videa výplněmi přiděleném oblasti pro `VideoPlayer` bez zachování poměru stran správné. Pro tento důvodu (jak se krátce zobrazí), `VideoView` přišla podřízenou Android `RelativeLayout`. A `using` tato direktiva definuje `ARelativeLayout` ho odlišuje od platformě Xamarin.Forms `RelativeLayout`, a druhý argument Obecné `ViewRenderer`:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Od verze Xamarin.Forms 2.5, Android nástroji pro vykreslování by měla obsahovat konstruktor s `Context` argument.

`OnElementChanged` Přepsání vytvoří i `VideoView` a `RelativeLayout` a nastaví rozložení parametry `VideoView` na střed v rámci `RelativeLayout`.


```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

Obslužná rutina pro `Prepared` událostí je připojený v této metodě a odpojit v `Dispose` metoda. Tato událost je aktivována, pokud `VideoView` má dost informací zahájíte přehrávání videa souboru.

### <a name="the-uwp-media-element"></a>Element média UWP

V univerzální platformu Windows (UWP), je většina běžných přehrávání videa [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/). Tuto dokumentaci `MediaElement` znamená, že [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) /t místo toho je nutné pro podporu verzích Windows 10 počínaje sestavení 1607 pouze.

`OnElementChanged` Přepsání musí vytvořit `MediaElement`, nastavit několik obslužné rutiny událostí a předat `MediaElement` do objektu `SetNativeControl`:

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

Obslužné rutiny dvou událostí jsou odpojené v `Dispose` událost pro zobrazovací jednotky.

## <a name="showing-the-transport-controls"></a>Zobrazení ovládacích prvků přenosu

Video přehrávače součástí tři platformy podporují výchozí sadu ovládacích prvků přenosu, které zahrnují tlačítka pro přehrávání a pozastavení a řádku k označení na aktuální pozici v videa a přesunout do nového umístění.

`VideoPlayer` Třída definuje vlastnost s názvem `AreTransportControlsEnabled` a nastaví výchozí hodnota `true`:


```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

I když tato vlastnost má oba `set` a `get` přístupových objektů, má zobrazovací jednotky pro zpracování případů jenom v případě, že je nastavena vlastnost. `get` Přistupujícího objektu jednoduše vrací aktuální hodnotu vlastnosti.

Vlastnosti, jako `AreTransportControlsEnabled` jsou zpracovávány v nástroji pro vykreslování platformy dvěma způsoby:

- První je, když vytvoří Xamarin.Forms `VideoPlayer` elementu. To je uvedené v `OnElementChanged` přepsat zobrazovací jednotky při `NewElement` vlastnost není `null`. V tomto okamžiku můžete nastavit zobrazovací jednotky je vlastní platformy přehrávání videa z počáteční hodnota vlastnosti podle `VideoPlayer`.

- Pokud vlastnost v `VideoPlayer` později změní, pak se `OnElementPropertyChanged` je volána metoda v zobrazovací jednotky. To umožňuje zobrazovací jednotky k aktualizaci přehrávání videa platformy založené na nové nastavení vlastnosti.

Tady je jak `AreTransportControlsEnabled` vlastnost používá tři platformy:

### <a name="ios-playback-controls"></a>ovládací prvky přehrávání iOS

Vlastnost IOS `AVPlayerViewController` , řídí zobrazení přenosu ovládací prvky je [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/). Tady je nastavení této vlastnosti ve iOS `VideoViewRenderer`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

`Element` Vlastnost zobrazovací jednotky odkazuje `VideoPlayer` třídy.

### <a name="the-android-media-controller"></a>Android médium řadiče

V systému Android, zobrazení ovládacích prvků přenosu vyžaduje vytvoření [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) objekt a jeho s přidružením `VideoView` objektu. Mechanismů je ukázán v `SetAreTransportControlsEnabled` metoda:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>Vlastnost UWP přenosu ovládací prvky

UWP `MediaElement` definuje vlastnost s názvem [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled)tak, aby z je hodnota nastavena `VideoPlayer` vlastnost se stejným názvem:

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
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Jeden další vlastnost je nutné začít přehrávání videa: Toto je zásadní `Source` vlastnost, která odkazuje na soubor videa. Implementace `Source` vlastnost je popsaný v článku na další [přehrávání videa webové](web-videos.md).


## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
