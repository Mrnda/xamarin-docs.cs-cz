---
title: Ovládací prvky vlastní video přenosu
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 5463a91dba5840ebe655aa1509d9f98e73643d26
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="custom-video-transport-controls"></a>Ovládací prvky vlastní video přenosu

Ovládací prvky přenos z přehrávání videa zahrnují tlačítka, které provádějí funkce **přehrání**, **pozastavení**, a **Zastavit**. Tyto přepínače jsou obecně označeny známé ikony spíše než text a **přehrání** a **pozastavení** funkce jsou obecně zkombinované do jednoho tlačítka.

Ve výchozím nastavení `VideoPlayer` zobrazí přenosu ovládací prvky na každé platformě podporováno. Když nastavíte `AreTransportControlsEnabled` vlastnost `false`, jsou potlačovány tyto ovládací prvky. Můžete pak řídit `VideoPlayer` prostřednictvím kódu programu nebo zadat své vlastní ovládací prvky přenosu.

## <a name="the-play-pause-and-stop-methods"></a>Metody Play, pozastavení a ukončení

`VideoPlayer` Třída definuje tři metody s názvem `Play`, `Pause`, a `Stop` , jsou implementované aktivaci událostí:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

Obslužné rutiny události pro tyto události jsou nastavena `VideoPlayerRenderer` třídy u různých platforem, jak je uvedeno níže:

### <a name="ios-transport-implementations"></a>iOS přenosu implementace

Verze iOS `VideoPlayerRenderer` používá `OnElementChanged` metodu a nastavit obslužné rutiny pro tyto tři události při `NewElement` vlastnost není `null` a odpojí obslužné rutiny událostí při `OldElement` není `null`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

Obslužné rutiny událostí jsou implementované pomocí volání metody na `AVPlayer` objektu. Neexistuje žádné `Stop` metodu pro `AVPlayer`, takže se simuluje pozastavení videa a přesunutí pozici na začátku.

### <a name="android-transport-implementations"></a>Implementace Android přenosu

Android implementace je podobná implementace iOS. Obslužné rutiny pro tři funkce jsou nastaveny, jakmile `NewElement` není `null` a kdy odpojit `OldElement` není `null`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

Tři funkce volat metody definované `VideoView`.

### <a name="uwp-transport-implementations"></a>Implementace přenos UWP

Implementace UWP funkce tři přenosu je velmi podobné iOS a Android implementace:

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>Stav přehrávání videa

Implementace **přehrání**, **pozastavení**, a **Zastavit** funkce není dostatečná pro podporu ovládacích prvků. Často **přehrání** a **pozastavení** příkazy jsou implementované pomocí stejné tlačítka, která se mění její vzhled indikující, zda je aktuálně přehrává nebo je pozastaveno. Kromě toho by nemělo být tlačítko povoleno i pokud video nebyl dosud načteny.

Tyto požadavky neznamená, že je potřeba zpřístupnit aktuální stav označující, pokud je přehrávání nebo pozastavena, nebo pokud ještě není připraven k přehrávání videa přehrávání videa. (Tři platformy také podporují označují, pokud na video může být pozastavena, nebo lze přesunout do nového umístění, ale tyto vlastnosti jsou k dispozici pro streamování videa, nikoli videosoubory, takže některé funkce nejsou podporovány v `VideoPlayer` zde popsané.)

**VideoPlayerDemos** projekt zahrnuje `VideoStatus` výčet s tři členy:

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

`VideoPlayer` Třída definuje jen reálné vazbu vlastnost s názvem `Status` typu `VideoStatus`. Tato vlastnost je definován jako jen pro čtení, protože by měl nastavit jenom z platformy zobrazovací jednotky:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

Obvykle vazbu vlastnosti jen pro čtení by mít privátního `set` přistupujícího objektu na `Status` vlastnost tak, aby ji z nastavení v rámci třídy. Pro `View` odvozených nepodporuje nástroji pro vykreslování, ale vlastnost musí být nastavená z mimo třídu, ale pouze pomocí zobrazovací jednotky platformy.

Z tohoto důvodu je definována jinou vlastnost s názvem `IVideoPlayerController.Status`. To je explicitní implementace rozhraní a je možné pomocí `IVideoPlayerController` rozhraní `VideoPlayer` implementuje třída:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Toto je podobným způsobem [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) používá ovládací prvek [ `IWebViewController` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IWebViewController/) rozhraní k implementaci `CanGoBack` a `CanGoForward` vlastnosti. (Zobrazit zdrojový kód [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) a jeho nástroji pro vykreslování podrobnosti.)

To umožňuje pro třídu mimo `VideoPlayer` nastavit `Status` vlastnost odkazem `IVideoPlayerController` rozhraní. (Se zobrazí kód za chvíli.) Vlastnost lze nastavit z také další třídy, ale je nepravděpodobné, že nechtěně nastavit. Co je nejdůležitější `Status` prostřednictvím vazby dat nelze nastavit vlastnost.

Pomůže nástroji pro vykreslování při ochraně to `Status` vlastnost aktualizována, `VideoPlayer` třída definuje `UpdateStatus` událost, která se spustí každý desetinu sekundy:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>Nastavení stavu iOS

IOS `VideoPlayerRenderer` nastaví obslužnou rutinu pro `UpdateStatus` událostí (a odpojí této obslužné rutiny při základní `VideoPlayer` chybí element) a používá obslužná rutina nastavit `Status` vlastnost:

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
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

Dvě vlastnosti `AVPlayer` , musí se přístup: [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) vlastnost typu `AVPlayerStatus` a [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) vlastnost typu `AVPlayerTimeControlStatus`. Všimněte si, že `Element` vlastnost (což je `VideoPlayer`) musí být převedena `IVideoPlayerController` nastavit `Status` vlastnost.

### <a name="the-android-status-setting"></a>Nastavení Android stavu

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Vlastnost Android `VideoView` je logická hodnota určující, pouze pokud je přehrává nebo je pozastaveno. K určení, zda `VideoView` můžete ani play ani pozastavit video ale, `Prepared` události `VideoView` musí být zpracován. Tyto dvě obslužné rutiny jsou nastaveny v `OnElementChanged` metody a odpojené během `Dispose` přepsání:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`UpdateStatus` Obslužná rutina používá `isPrepared` pole (nastavit `Prepared` obslužné rutiny) a `IsPlaying` vlastnost nastavit `Status` vlastnost:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>Nastavení stavu UWP

UWP `VideoPlayerRenderer` využívá `UpdateStatus` událostí, ale nemusí jej pro nastavení `Status` vlastnost. `MediaElement` Definuje [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) událost, která umožňuje zobrazovací jednotky při upozornění [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) došlo ke změně vlastností. Vlastnost odpojení na severu `Dispose` přepsání:

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`CurrentState` Vlastnost je typu [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)a snadno mapy do `VideoStatus`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>Přehrání, pozastavení a zastavit tlačítka

Pomocí znaků Unicode symbolický **přehrání**, **pozastavení**, a **Zastavit** bitové kopie je problematické. [Různé technické](https://unicode-table.com/en/blocks/miscellaneous-technical/) části standardu Unicode definuje tři symbolů zdánlivě vhodné pro tento účel. Jsou to:

- 0x23F5 (černý střední šipkou trojúhelník) nebo &#x23F5; pro **přehrávání**
- 0x23F8 (dvojité svislá čára) nebo &#x23F8; pro **pozastavení**
- 0x23F9 (černý čtvereček) nebo &#x23F9; pro **zastavit**

Bez ohledu na to jak v prohlížeči se zobrazí tyto symboly (a různé prohlížeče zpracovávají různými způsoby), nejsou zobrazeny konzistentně na platformách podporovaných aplikací Xamarin.Forms. IOS a zařízení UWP **pozastavení** a **Zastavit** znaky mají grafické vzhled, modré 3D pozadí a bílé popředí. Tato akce není případ v systému Android, kde je jednoduše blue symbolu. Ale codepoint 0x23F5 pro **přehrání** nemá, že stejný vzhled na UPW a dokonce ani podporovaná na iOS a Android.

Z tohoto důvodu nelze použít 0x23F5 codepoint pro **přehrání**. Je dobré náhrada:

- 0x25B6 (černý trojúhelník směřující doprava) nebo &#x25B6; pro **přehrávání**

To je podporováno všemi tři platformami s tím rozdílem, že je prostý černý trojúhelník, který není vypadat 3D vzhled **pozastavení** a **Zastavit**. Jednou z možností je postupujte podle codepoint 0x25B6 variant kódem:

- 0x25B6 následuje 0xFE0F (typ variant 16) nebo &#x25B6; &#xFE0F; pro **přehrávání**

Je to, co se používá v kódu vidíte níže. V systému iOS, nabízí **přehrání** symbolů stejné 3D vzhled jako **pozastavení** a **Zastavit** tlačítka, ale varianta nefunguje pro Android a UWP.

**Vlastní přenos** stránka sady **AreTransportControlsEnabled** vlastnost, která má **false** i `ActivityIndicator` zobrazit při načítání videa a dva tlačítka. `DataTrigger` objekty se používají k povolení a zákaz `ActivityIndicator` a tlačítka a přejděte na první tlačítko mezi **přehrání** a **pozastavení**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

Data aktivační události jsou podrobně popsané v článku [Data aktivační události](~/xamarin-forms/app-fundamentals/triggers.md#data).

Soubor kódu má obslužné rutiny pro tlačítko `Clicked` události:

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

Protože `AutoPlay` je nastaven na `false` v **CustomTransport.xaml** soubor, budete muset stiskněte **přehrání** když nepovolíte zahájíte videa. Tlačítka jsou definovány tak, aby znaky znakové sady Unicode výše popsané se předěl doprovází jejich ekvivalenty u textu. Při přehrávání videa tlačítka mají Konzistentnějšího vzhledu na jednotlivých platformách:

[![Přehrávání vlastní přenos](custom-transport-images/customtransportplaying-small.png "vlastní přenos přehrávání")](custom-transport-images/customtransportplaying-large.png#lightbox "vlastní přenos přehrávání")

Ale pro Android a UPW **přehrání** tlačítko vypadá příliš neliší, když je pozastaven na video:

[![Vlastní přenos pozastaven](custom-transport-images/customtransportpaused-small.png "vlastní přenos pozastaven")](custom-transport-images/customtransportpaused-large.png#lightbox "vlastní přenos pozastaven")

V případě produkční aplikace budete pravděpodobně chtít používat vlastní Image rastrový obrázek pro tlačítka zajistit jednotnost visual.


## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
