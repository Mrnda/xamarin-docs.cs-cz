---
title: "Vlastní umístění video"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d4446d30491ee796ca93eadf2e107fc9d74748df
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="custom-video-positioning"></a>Vlastní umístění video

Ovládací prvky přenosu implementované každou platformu zahrnují pozici panelu. Tento panel zobrazuje aktuální umístění videa v rámci jeho celková doba trvání a vypadá takto: jezdce nebo posuvníku. Kromě toho uživatel můžete upravit na pozici panelu přesunout dopředný nebo zpětné na jiné místo v videu.

Tento článek ukazuje, jak můžete implementovat vlastní vlastní pozici panelu.

## <a name="the-duration-property"></a>Vlastnost doba trvání

Jednu položku informací, `VideoPlayer` musí podporovat vlastní pozici panelu je doba, videa. `VideoPlayer` Definuje jen pro čtení `Duration` vlastnost typu `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

Podobně jako `Status` vlastnost popsané v [předchozí článek](custom-transport.md)tento `Duration` vlastnost je jen pro čtení. Je definovaná pomocí privátního `BindablePropertyKey` a lze nastavit pouze odkazem `IVideoPlayerController` rozhraní, což zahrnuje `Duration` vlastnost:

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

Všimněte si také obslužná rutina změnit vlastnost, která volá metodu s názvem `SetTimeToEnd` popsané dále v tomto článku.

Doba trvání video je *není* k dispozici ihned po `Source` vlastnost `VideoPlayer` nastavena. Soubor videa musí částečně stažen před základní přehrávání videa můžete určit jeho trvání.

Zde je, jak každý z platformy pro vykreslování získá délku videa:

### <a name="video-duration-in-ios"></a>Video doba trvání v iOS

V iOS, trvání video získaný `Duration` vlastnost `AVPlayerItem`, ale okamžitě po `AVPlayerItem` je vytvořena. Je možné nastavit pozorovatele iOS pro `Duration` vlastnost, ale `VideoPlayerRenderer` získá doba trvání v `UpdateStatus` metoda, která se nazývá 10krát druhý:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

`ConvertTime` Metoda převede `CMTime` do objektu `TimeSpan` hodnotu.

### <a name="video-duration-in-android"></a>Video doba trvání v Android

`Duration` Vlastnost Android `VideoView` sestav platná doba v milisekundách při `Prepared` události `VideoView` je aktivováno. Android `VideoPlayerRenderer` třída používá k získání této obslužné rutiny `Duration` vlastnost:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>Video doba trvání v UWP

`NaturalDuration` Vlastnost `MediaElement` je `TimeSpan` hodnotu a začne platit, kdy `MediaElement` aktivuje `MediaOpened` událostí:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Vlastnost umístění

`VideoPlayer` také je nutné `Position` vlastnost, která zvyšuje od nuly do `Duration` jako přehrávání videa. `VideoPlayer` implementuje tuto vlastnost jako `Position` vlastnost UWP `MediaElement`, což je normální vazbu vlastnost s veřejné `set` a `get` přistupující objekty:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

`get` Přistupujícího objektu vrátí aktuální pozici videa jako jeho vysílání, ale `set` přistupujícího objektu je určený reagovat na uživatele zpracování na pozici panelu přesunutím video pozice dopředný nebo zpětné.

V iOS a Android, má pouze vlastnosti, který získá aktuální pozice `get` přistupujícího objektu a `Seek` metoda je k dispozici k provedení této druhé úlohy. Pokud si myslíte o něm samostatné `Seek` metoda zdá se, že se více rozumný přístup než jedné `Position` vlastnost. Jediný `Position` vlastnost má problém s vyplývajících: během přehrávání videa `Position` vlastnost musí být průběžně aktualizovat tak, aby odrážely novou pozici. Ale nechcete, aby většinu změn `Position` vlastnost způsobí přehrávání videa pro přesun do nového pozice ve videu. Pokud k tomu dojde, přehrávání videa by odpovídat tak, že vyhledávání na poslední hodnotu `Position` vlastnost a video nebude zálohy.

Bez ohledu problémy implementace `Position` vlastnost s `set` a `get` přístupových objektů, tento přístup jste vybrali, protože je v souladu s UWP `MediaElement`, a má zásadní výhodu s datové vazby: `Position` Vlastnost `VideoPlayer` mohou být vázány na posuvníku, který se používá pro zobrazení pozice i k vyhledání do nového umístění. Však několik opatření jsou nezbytné při implementaci to `Position` vlastnosti tak, aby se zabránilo smyčky zpětné vazby.

### <a name="setting-and-getting-ios-position"></a>Nastavení a získávání iOS pozice

V iOS `CurrentTime` vlastnost `AVPlayerItem` objekt označuje aktuální pozici přehrávání videa. IOS `VideoPlayerRenderer` nastaví `Position` vlastnost `UpdateStatus` obslužné rutiny ve stejnou dobu, která nastaví `Duration` vlastnost:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

Zobrazovací jednotky zjistí při `Position` vlastností nastavenou z `VideoPlayer` došlo ke změně v `OnElementPropertyChanged` přepsat a používá tuto novou hodnotu pro volání `Seek` metodu `AVPlayer` objektu:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

Mějte na paměti, že pokaždé, když `Position` vlastnost v `VideoPlayer` nastavena z `OnUpdateStatus` obslužnou rutinu, `Position` vlastnost aktivuje `PropertyChanged` událost, která je zjištěn během `OnElementPropertyChanged` přepsat. Pro většinu těchto změn `OnElementPropertyChanged` metoda by měla Neprovádět žádnou akci. Jinak hodnota s každé změně v pozici na video, by se přesune do stejné pozici, které bylo dosaženo!

Předejdete této zpětné vazby `OnElementPropertyChanged` metoda pouze volá `Seek` při rozdíl mezi `Position` vlastnost a aktuální pozici `AVPlayer` je větší než jedna sekunda.

### <a name="setting-and-getting-android-position"></a>Nastavení a získávání Android pozice

Stejně jako zobrazovací jednotky iOS, Android `VideoPlayerRenderer` Nastaví novou hodnotu `Position` vlastnost v `OnUpdateStatus` obslužné rutiny. `CurrentPosition` Vlastnost `VideoView` obsahuje nové pozici v jednotkách milisekundách:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

Také jako v případě zobrazovací jednotky iOS, Android zobrazovací jednotky volá `SeekTo` metodu `VideoView` při `Position` vlastnost změněna, ale jenom v případě, že tato změna se liší od více než jedna sekunda `CurrentPosition` hodnotu `VideoView`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>Nastavení a získávání UWP pozice

UWP `VideoPlayerRenderer` obslužné rutiny `Position` stejným způsobem jako iOS a Android nástroji pro vykreslování, ale protože `Position` vlastnost UWP `MediaElement` je také `TimeSpan` hodnotu, je nutné žádný převod:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>Výpočet TimeToEnd vlastnost

Někdy video přehrávače zobrazit zbývající ve videu čas. Tato hodnota začíná na délku videa při přehrávání videa začne a snižuje dolů nula při ukončení videa.

`VideoPlayer` zahrnuje jen pro čtení `TimeToEnd` vlastnost, která zpracovává zcela v rámci třídy na základě změn `Duration` a `Position` vlastnosti:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

`SetTimeToEnd` Metoda je volána z obslužné rutiny vlastnost změnit i `Duration` a `Position`.

## <a name="a-custom-slider-for-video"></a>Vlastní posuvník pro video

Je možné zapisovat vlastní ovládací prvek pro pozici panelu nebo používat platformě Xamarin.Forms `Slider` nebo třídu, která pochází z `Slider`, jako je například následující `PositionSlider` třídy. Třída definuje dvě nové vlastnosti s názvem `Duration` a `Position` typu `TimeSpan` , by měla být vázané na data na dvě vlastnosti se stejným názvem v `VideoPlayer`. Všimněte si, že výchozí režim vytvoření vazby `Position` vlastnost je obousměrný:

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

Vlastnost změnit obslužnou rutinu pro `Duration` sady vlastností `Maximum` vlastnost základní `Slider` k `TotalSeconds` vlastnost `TimeSpan` hodnotu. Podobně vlastnost změnit obslužnou rutinu pro `Position` nastaví `Value` vlastnost `Slider`. Tímto způsobem, základní `Slider` sleduje pozici `PositionSlider`.

`PositionSlider` Je aktualizována z základní `Slider` v pouze jednu instanci: když uživatel manipuluje `Slider` že video měli advanced nebo vrátit zpět do nového umístění. To je zjištěn během `PropertyChanged` obslužné rutiny v konstruktoru `PositionSlider`. Obslužná rutina kontroluje změny v `Value` vlastnost, a pokud se liší od `Position` vlastnost, pak se `Position` z je hodnota nastavena `Value` vlastnost.

Teoreticky vnitřní `if` příkaz může zapsat takto:

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Ale Android implementace `Slider` má jenom 1000 oddělených kroků bez ohledu na to `Minimum` a `Maximum` nastavení. Délka video je větší než 1 000 sekund, pak dva různé `Position` hodnoty by odpovídat na stejnou `Value` nastavit `Slider`a to `if` příkaz by aktivovat falešně pozitivní pro manipulaci s uživatele z `Slider`. Je bezpečnější místo toho zkontrolujte, zda jsou větší než jedné setině celkové doby trvání nové umístění a stávající umístění.

## <a name="using-the-positionslider"></a>Pomocí PositionSlider

Dokumentaci UWP [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) varuje o vazbě na `Position` vlastnost proto, že vlastnost často aktualizuje. V dokumentaci doporučuje použít časovač pro dotaz `Position` vlastnost.

To je dobrý doporučení, ale tří `VideoPlayerRenderer` třídy jsou už nepřímo pomocí časovače aktualizace `Position` vlastnost. `Position` Vlastnost se změnilo v obslužnou rutinu pro `UpdateStatus` událost, která je aktivována pouze 10krát na druhou.

Proto `Position` vlastnost `VideoPlayer` mohou být vázány na `Position` vlastnost `PositionSlider` bez problémů s výkonem, jak je předvedeno v **vlastní pozici panelu** stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

Skryje první třemi tečkami (···) `ActivityIndicator`; je stejný jako předchozí **vlastní přenos** stránky. Všimněte si, dva `Label` prvky zobrazení `Position` a `TimeToEnd` vlastnosti. Se třemi tečkami mezi těmito dvěma `Label` skryje dva elementy `Button` prvky zobrazené v **vlastní přenos** stránky pro Play, pozastavení a zastavit. Logika kódu je také stejné jako **vlastní přenos** stránky.

[![Vlastní umístění](custom-positioning-images/custompositioning-small.png "vlastní umístění")](custom-positioning-images/custompositioning-large.png "vlastní umístění")

Tím končí diskuzi o `VideoPlayer`.

## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
