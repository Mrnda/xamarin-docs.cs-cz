---
title: Ovládací prvky pro přenos vlastní videa
description: Tento článek vysvětluje, jak implementovat vlastní přenos ovládacích prvků do aplikace přehrávače videa pomocí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 84870de28ffd30b2d29fb5d8fbea815e1fd0d9c4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996435"
---
# <a name="custom-video-transport-controls"></a>Ovládací prvky pro přenos vlastní videa

Přenos ovládací prvky přehrávače videa zahrnout tlačítka, které provádějí funkce **Přehrát**, **pozastavení**, a **Zastavit**. Tato tlačítka jsou obecně označeny známé ikony spíše než textovém a **Přehrát** a **pozastavení** funkcí jsou obecně zkombinovány do jedno tlačítko.

Ve výchozím nastavení `VideoPlayer` zobrazí přenosu ovládacích prvků podporuje jednotlivé platformy. Při nastavení `AreTransportControlsEnabled` vlastnost `false`, tyto ovládací prvky jsou potlačeny. Můžete pak řídit `VideoPlayer` prostřednictvím kódu programu nebo zadat vlastní ovládací prvky pro přenos.

## <a name="the-play-pause-and-stop-methods"></a>Metody, pozastavení, spouštění a zastavování

`VideoPlayer` Třída definuje tři metody s názvem `Play`, `Pause`, a `Stop` , které jsou implementované vyvolává události:

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

Obslužné rutiny událostí pro tyto události jsou nastaveny pomocí `VideoPlayerRenderer` třídy u různých platforem, jak je znázorněno níže:

### <a name="ios-transport-implementations"></a>iOS přenosu implementace

Verze iOS `VideoPlayerRenderer` používá `OnElementChanged` metody nastavte obslužné rutiny pro tyto tři události při `NewElement` jedná o vlastnost neumožňující `null` a odpojí obslužné rutiny událostí při `OldElement` není `null`:

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

Obslužné rutiny událostí jsou implementovány pomocí volání metody na `AVPlayer` objektu. Neexistuje žádná `Stop` metodu `AVPlayer`, takže se simuluje pozastavení video a přesunutím pozice na začátku.

### <a name="android-transport-implementations"></a>Implementace Android přenosu

Android implementace se podobá implementace iOS. Obslužné rutiny pro tři funkce jsou nastaveny, jakmile `NewElement` není `null` a odpojit, kdy `OldElement` není `null`:

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

Tři funkce volání metody definované `VideoView`.

### <a name="uwp-transport-implementations"></a>Implementace přenosu UPW

UPW provádění funkce tři přenosu je velmi podobný iOS i Android implementace:

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

## <a name="the-video-player-status"></a>Stav přehrávače videa

Implementace **Přehrát**, **pozastavení**, a **Zastavit** funkce není dostatečná pro podporu ovládací prvky pro přenos. Často **Přehrát** a **pozastavení** příkazy jsou implementovány pomocí stejného tlačítka, která se mění její vzhled k určení, zda video je aktuálně přehrávání nebo pozastaveno. Kromě toho by nemělo být tlačítko povoleno i pokud video ještě nezavedl.

Tyto požadavky znamenají, že je potřeba zpřístupnit aktuální stav označující, pokud je přehrávání nebo pozastavena, nebo pokud ještě není připravené k přehrání videa přehrávač videa. (Tři platformy podporují také vlastnosti, které Pokud video můžete pozastavit nebo je přesunout do nového umístění, ale tyto vlastnosti jsou k dispozici pro streamování videa, spíše než videosoubory, proto nejsou podporovány v `VideoPlayer` je popsáno zde.)

**VideoPlayerDemos** obsahuje projekt `VideoStatus` výčet pomocí tří členů:

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

`VideoPlayer` Třídy definuje pouze reálném vlastnost umožňujících vazbu s názvem `Status` typu `VideoStatus`. Tato vlastnost je definován jako jen pro čtení, protože by měl nastavit pouze od rendereru platformy:

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

Obvykle vázanou vlastnost jen pro čtení by mít privátní `set` přístupového objektu na `Status` vlastnost, aby mělo být nastavené v třídě. Pro `View` odvozených děl na základě podporovaných zobrazovacím jednotkám, ale vlastnost musí nastavit z vně třídy, ale pouze pomocí zobrazovací jednotky platformy.

Z tohoto důvodu je definována jinou vlastnost s názvem `IVideoPlayerController.Status`. Toto je explicitní implementací rozhraní a je možné podle `IVideoPlayerController` rozhraní, která `VideoPlayer` implementuje třída:

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

Podobá se to jak [ `WebView` ](xref:Xamarin.Forms.WebView) používá ovládací prvek [ `IWebViewController` ](xref:Xamarin.Forms.IWebViewController) rozhraní k implementaci `CanGoBack` a `CanGoForward` vlastnosti. (Zobrazit zdrojový kód [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) a jeho renderery podrobnosti.)

To umožňuje pro třídu pro externí `VideoPlayer` nastavit `Status` vlastnost odkazováním `IVideoPlayerController` rozhraní. (Zobrazí se vám kód za chvíli.) Vlastnost lze nastavit z jiné třídy stejně, ale je nepravděpodobné, že neúmyslně nastavit. Nejdůležitější ale je `Status` vlastnost nelze nastavit prostřednictvím datové vazby.

Pomáhat renderery ochraně to `Status` vlastnost aktualizována, `VideoPlayer` třída definuje `UpdateStatus` události, která se aktivuje každou desetinu sekundy:

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

### <a name="the-ios-status-setting"></a>Stav nastavení iOS

IOS `VideoPlayerRenderer` nastaví obslužnou rutinu pro `UpdateStatus` událostí (a odpojí tuto obslužnou rutinu při základní `VideoPlayer` chybí element) a používá obslužnou rutinu k nastavení `Status` vlastnost:

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

Dvě vlastnosti `AVPlayer` je možný: [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) vlastnost typu `AVPlayerStatus` a [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) vlastnost typu `AVPlayerTimeControlStatus`. Všimněte si, že `Element` vlastnosti (což je `VideoPlayer`) musí být přetypovat na `IVideoPlayerController` nastavit `Status` vlastnost.

### <a name="the-android-status-setting"></a>Nastavení Androidu stavu

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Vlastnost Android `VideoView` je logická hodnota označující, pouze pokud je video přehrávání nebo pozastavena. Určete, jestli `VideoView` můžete ani play ani je pozastavit video ještě, `Prepared` událost `VideoView` musí být zpracován. Tyto dvě obslužné rutiny jsou nastaveny `OnElementChanged` metody a odpojená během `Dispose` přepsat:

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

`UpdateStatus` Obslužná rutina používá `isPrepared` pole (nastavit `Prepared` obslužné rutiny) a `IsPlaying` vlastnosti chcete nastavit `Status` vlastnost:

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

### <a name="the-uwp-status-setting"></a>Nastavení stavu UPW

Na UPW `VideoPlayerRenderer` využívá `UpdateStatus` události, ale nemusí jej pro nastavení `Status` vlastnost. `MediaElement` Definuje [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) událost, která umožňuje zobrazovací jednotky která vás upozorní, když [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) je změněna vlastnost. Vlastnost je odpojená v `Dispose` přepsat:

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

`CurrentState` Vlastnost je typu [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)a snadno map `VideoStatus`:

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

## <a name="play-pause-and-stop-buttons"></a>Přehrát, pozastavit a zastavit tlačítka

Pomocí znaků Unicode pro symbolické **Přehrát**, **pozastavení**, a **Zastavit** Image jen těžko. [Různé technické](https://unicode-table.com/en/blocks/miscellaneous-technical/) části Unicode standard definuje tří znaků symbolu zdánlivě vhodné pro tento účel. Toto jsou:

- 0x23F5 (černý střední vpravo trojúhelník) nebo &#x23F5; pro **přehrávání**
- 0x23F8 (double svislá čára) nebo &#x23F8; pro **pozastavení**
- 0x23F9 (černé čtverec) nebo &#x23F9; pro **zastavit**

Bez ohledu na to jak v prohlížeči se zobrazí tyto symboly (a různé prohlížeče zpracovávají různými způsoby), se nezobrazují konzistentně na platformách podporovaných aplikací Xamarin.Forms. V iOS a UPW zařízení **pozastavení** a **Zastavit** mají grafické vzhledu, pomocí modré 3D pozadí a popředí bílé znaky. To není případ v Androidu, pokud symbol je jednoduše modrá. Ale 0x23F5 bod kódu pro **Přehrát** nemá, že je stejný vzhled na UPW a dokonce ani podporováno v Iosu a Androidu.

Z tohoto důvodu 0x23F5 bod kódu nelze použít pro **Přehrát**. Je dobrou náhradou:

- 0x25B6 (černý trojúhelník vpravo) nebo &#x25B6; pro **přehrávání**

To je podporováno ve všech třech platformách, s tím rozdílem, že je prostý černý trojúhelník, který není vypadat podobně jako na 3D vzhled **pozastavení** a **Zastavit**. Jednou z možností se řídit 0x25B6 bod kódu s kódem varianty:

- 0x25B6 následovaný 0xFE0F (typ variant 16) nebo &#x25B6; &#xFE0F; pro **přehrávání**

To se používá v kódu je uvedeno níže. V systémech iOS, poskytuje **Přehrát** symbol stejný 3D vzhled jako **pozastavení** a **Zastavit** tlačítka, ale varianty nefunguje na Android a UPW.

**Přenosu vlastní** stránce sady **AreTransportControlsEnabled** vlastnost **false** a zahrnuje `ActivityIndicator` zobrazí, když se načítá videa a dvě tlačítka. `DataTrigger` objekty se používají k povolení a zakázání `ActivityIndicator` a tlačítka a přepnout na první tlačítko mezi **Přehrát** a **pozastavení**:

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

Data aktivační události jsou popsány podrobně v článku [Data aktivace](~/xamarin-forms/app-fundamentals/triggers.md#data).

Použití modelu code-behind soubor má obslužné rutiny pro tlačítko `Clicked` události:

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

Protože `AutoPlay` je nastavena na `false` v **CustomTransport.xaml** souboru, bude nutné stisknout klávesu **Přehrát** tlačítko, pokud jej nepovolíte zahájíte videa. Tlačítka jsou definovány, tak, aby znaky znakové sady Unicode probírali výše jsou doplněny ekvivalenty text. Při přehrávání videa tlačítek mít konzistentní vzhled na jednotlivých platformách:

[![Vlastní přenosu přehrávání](custom-transport-images/customtransportplaying-small.png "přehrávání vlastní přenosu")](custom-transport-images/customtransportplaying-large.png#lightbox "přehrávání vlastní přenosu")

Ale na Android a UPW **Přehrát** tlačítko vypadá velmi odlišné při pozastavení videa:

[![Vlastní přenos pozastaven](custom-transport-images/customtransportpaused-small.png "vlastní přenos pozastaven")](custom-transport-images/customtransportpaused-large.png#lightbox "vlastní přenos pozastaven")

V produkční aplikace budete pravděpodobně chtít použít vlastní rastrový obrázek Image pro tlačítka k dosažení visual sjednocení.


## <a name="related-links"></a>Související odkazy

- [Ukázky přehrávače videa (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
