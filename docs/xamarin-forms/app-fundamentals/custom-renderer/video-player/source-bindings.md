---
title: "Vytvoření vazby zdroje videa na player"
ms.topic: article
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 209b301c44da9bbb52ad8bf7fe867811a9b7617f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="binding-video-sources-to-the-player"></a>Vytvoření vazby zdroje videa na player

Když `Source` vlastnost `VideoPlayer` je pro zobrazení nastavena do nového souboru videa, existující video zastaví přehrávání a začne nové video. Toto prokazuje **vyberte Video webové** stránky [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) ukázka. Stránka obsahuje `ListView` s názvy tři videa na něj odkazovat z **App.xaml** souboru:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Pokud je vybraná video, `ItemSelected` obslužné rutiny událostí v souboru kódu na pozadí se spustí. Obslužná rutina odebere všechny prázdné znaky a apostrofy z názvu a použije tento jako klíč získat jeden z prostředků, které jsou definované v **App.xaml** souboru. Že `UriVideoSource` je pak nastavena `Source` vlastnost `VideoPlayer`.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

Při prvním načtení stránky, není vybrána žádná položka v `ListView`, takže je třeba vybrat jednu zahájíte přehrávání videa:

[![Vyberte Web Video](source-bindings-images/selectwebvideo-small.png "vyberte Web Video")](source-bindings-images/selectwebvideo-large.png#lightbox "vyberte Web Video")

`Source` Vlastnost `VideoPlayer` je zálohovaný díky vazbu vlastnosti, která znamená, že může být cílem datová vazba. Tento postup je znázorněn pomocí **vytvořit vazbu na VideoPlayer** stránky. Kód v **BindToVideoPlayer.xaml** podporuje následující třídy, který zapouzdřuje název video a odpovídající soubor `VideoSource` objektu:

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

`ListView` v **BindToVideoPlayer.xaml** soubor obsahuje pole tyto `VideoInfo` objektů a každý z nich inicializován s název videa a `UriVideoSource` objekt ze slovníku prostředků v  **App.XAML**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

`Source` Vlastnost `VideoPlayer` je vázána `ListView`. `Path` Vazby je zadán jako `SelectedItem.VideoSource`, což je složené cesty, který se skládá ze dvou vlastností: `SelectedItem` je vlastností `ListView`. Vybraná položka je typu `VideoInfo`, který má `VideoSource` vlastnost.

Stejně jako u první **vyberte Video webové** stránky, není vybrána žádná položka zpočátku z `ListView`, proto musíte vybrat jednu z videí předtím, než začne přehrávání.


## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
