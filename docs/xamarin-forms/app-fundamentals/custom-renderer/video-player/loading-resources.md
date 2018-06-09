---
title: Načítání videa prostředků aplikace
description: Tento článek vysvětluje, jak načíst videa ukládají jako prostředky aplikace v aplikaci přehrávání videa pomocí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: f28b0dc8e25cb2e498f4101175005f05a5c5a6ef
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241029"
---
# <a name="loading-application-resource-videos"></a>Načítání videa prostředků aplikace

Vlastní nástroji pro vykreslování pro `VideoPlayer` zobrazení podporují přehrávání videa soubory, které byly vloženy do projektů jednotlivé platformy jako prostředky aplikace. Ale aktuální verze `VideoPlayer` nemají přístup k prostředkům v rozhraní .NET standardní knihovny.

K načtení těchto prostředků, vytvořte instanci `ResourceVideoSource` nastavením `Path` vlastnost název souboru (nebo na složku a název souboru) prostředku. Alternativně můžete volat statických `VideoSource.FromResource` metoda tak, aby odkazovaly na prostředek. Potom nastavte `ResourceVideoSource` do objektu `Source` vlastnost `VideoPlayer`.

## <a name="storing-the-video-files"></a>Ukládání souborů videa

Ukládání souboru videa v projektu platformy se liší pro tři platformy:

### <a name="ios-video-resources"></a>iOS grafických prostředků

V projektu iOS můžete uložit video v **prostředky** složku nebo podsložku **prostředky** složky. Video soubor musí mít `Build Action` z `BundleResource`. Nastavte `Path` vlastnost `ResourceVideoSource` k názvu souboru, například **MyFile.mp4** pro soubor v **prostředky** složky, nebo **MyFolder/MyFile.mp4**, kde **Moje_složka** je podsložkou **prostředky**.

V **VideoPlayerDemos** řešení, **VideoPlayerDemos.iOS** projekt obsahuje podsložky **prostředky** s názvem **videa** obsahující soubor s názvem **iOSApiVideo.mp4**. Toto je krátké video, které ukazuje, jak vyhledat dokumentaci pro iOS pomocí webu Xamarin `AVPlayerViewController` třídy.

### <a name="android-video-resources"></a>Android grafických prostředků

V projektu Android videa musí být uložen v podsložce **prostředky** s názvem **nezpracovaná**. **Nezpracovaná** složka nemůže obsahovat podsložky. Poskytnout soubor videa `Build Action` z `AndroidResource`. Nastavte `Path` vlastnost `ResourceVideoSource` k názvu souboru, například **MyFile.mp4**.

**VideoPlayerDemos.Android** projekt obsahuje podsložky **prostředky** s názvem **nezpracovaná**, která obsahuje soubor s názvem **AndroidApiVideo.mp4**.

### <a name="uwp-video-resources"></a>UWP grafických prostředků

V projektu univerzální platformu Windows můžete uložit videa v libovolné složky v projektu. Zadejte soubor `Build Action` z `Content`. Nastavte `Path` vlastnost `ResourceVideoSource` na složku a název souboru, například **MyFolder/MyVideo.mp4**.

**VideoPlayerDemos.UWP** projekt obsahuje složku s názvem **videa** souborem **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Načítání souborů videa

Všechny třídy zobrazovací jednotky platformy obsahuje kód v jeho `SetSource` metodu pro načítání video soubory uložené jako prostředky.

### <a name="ios-resource-loading"></a>načítání prostředků iOS

Verze iOS `VideoPlayerRenderer` používá `GetUrlForResource` metodu `NSBundle` načítání prostředku. Úplná cesta musí být rozdělen do filename, rozšíření a adresáře. Kód používá `Path` – třída v rozhraní .NET `System.IO` obor názvů pro dělení cesta k souboru do těchto součástí:

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
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhitespace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Načítání prostředků Android

Android `VideoPlayerRenderer` používá název a název souboru a balíček můžete vytvořit `Uri` objektu. Název balíčku je název aplikace, v takovém případě **VideoPlayerDemos.Android**, který můžete získat z statických `Context.PackageName` vlastnost. Výsledné `Uri` objekt se pak předá do `SetVideoURI` metodu `VideoView`:

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
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>Načítání prostředků UWP

UWP `VideoPlayerRenderer` vytvoří `Uri` objektu pro cestu a nastaví na `Source` vlastnost `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>Přehrávání souboru prostředků

**Přehrávání videa prostředků** stránku **VideoPlayerDemos** řešení používá `OnPlatform` třídu k určení video soubor pro každou platformu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

Pokud uloží se prostředek iOS **prostředky** složku, a pokud uloží se prostředek do UWP v kořenové složce projektu, můžete použít stejný název souboru pro tři platformy. Pokud tomu tak je, pak můžete nastavit přímo na tento název `Source` vlastnost `VideoPlayer`.

Tady je této stránce běžící na třech platformách:

[![Přehrávání videa prostředků](loading-resources-images/playvideoresource-small.png "přehrávání videa prostředků")](loading-resources-images/playvideoresource-large.png#lightbox "přehrávání videa prostředků")

Nyní jste viděli postup [načíst videa z identifikátoru URI webové](web-videos.md) a jak přehrát vložené prostředky. Kromě toho můžete [načíst videa z knihovny video zařízení](accessing-library.md).


## <a name="related-links"></a>Související odkazy

- [Přehrávač videoukázky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
