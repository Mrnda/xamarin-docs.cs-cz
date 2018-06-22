---
title: Pomocí Android prostředky
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 337a1ed82010658adce40e8946ed1b0361fb6094
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762994"
---
# <a name="using-android-assets"></a>Pomocí Android prostředky

_Prostředky_ poskytnout způsob, jak do aplikace zahrnout libovolné soubory jako text, xml, písma, Hudba a video. Pokud se pokusíte zahrnují tyto soubory jako "zdroje", Android jejich zpracování do jeho prostředků systému a nebude možné získat nezpracovaná data. Pokud chcete získat přístup k datům nezměněný, prostředky jsou jedním ze způsobů ho.

Prostředky přidat do projektu se zobrazí stejně jako systém souborů, který může číst z vaší aplikace pomocí [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/).
V této jednoduchý ukázkový jsme se chystáte přidat asset soubor textu do našich projektu, přečtěte si ho pomocí `AssetManager`a zobrazit ji v TextView.


## <a name="add-asset-to-project"></a>Přidat prostředek do projektu

Prostředky v přejděte `Assets` složky vašeho projektu. Přidat nový textový soubor pro tuto složku s názvem `read_asset.txt`. Umístěte text v něm jako "I pochází prostředek!".

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio by měl nastavili **akce sestavení** pro tento soubor do **AndroidAsset**:

![Nastavení akce sestavení na AndroidAsset](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio pro Mac musí mít nastavená **akce sestavení** pro tento soubor do **AndroidAsset**:

[![Nastavení akce sestavení na AndroidAsset](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

Výběr správného **BuildAction** zajistí, že soubor budou zabalené do APK v době kompilace.


## <a name="reading-assets"></a>Načítání prostředků

Prostředky se načítají pomocí [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/). Instance `AssetManager` je k dispozici přístup k [prostředky](https://developer.xamarin.com/api/property/Android.Content.Context.Assets/) vlastnost `Android.Content.Context`, například aktivitu.
V následujícím kódu, jsme otevřít naše **read_asset.txt** asset, přečíst obsah a zobrazit ji pomocí TextView.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```


## <a name="running-the-application"></a>Spuštění aplikace

Spusťte aplikaci a měli byste vidět následující:

![Příklad – snímek obrazovky](android-assets-images/screenshot.png)


## <a name="related-links"></a>Související odkazy

- [AssetManager](https://developer.xamarin.com/api/type/Android.Content.Res.AssetManager/)
- [Kontext](https://developer.xamarin.com/api/type/Android.Content.Context/)
