---
title: Xamarin.Forms vlastními Renderery
description: Vlastní Renderery umožňují vývojářům přepsat vykreslování nativní ovládací prvky na jednotlivých platformách, chcete-li upravit vzhled a chování ovládacích prvků Xamarin.Forms.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c7ae25688b2f8635a9a89318e0b307e58add7a5a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998740"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms vlastními Renderery

_Xamarin.Forms uživatelského rozhraní jsou vykreslovány pomocí nativní ovládací prvky cílové platformy, povolení aplikace Xamarin.Forms pro zachování odpovídající vzhled a chování pro každou platformu. Vlastní Renderery umožňují vývojářům přepsání tohoto procesu upravit vzhled a chování Xamarin.Forms ovládací prvky na jednotlivých platformách._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Úvod do vlastní renderery](introduction.md)

Vlastní renderery poskytují výkonný přístup pro přizpůsobení vzhledu a chování ovládacích prvků Xamarin.Forms. Je možné použít pro používání stylů pro malé změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování. Tento článek obsahuje úvod do vlastní renderery a popisuje proces pro vytvoření vlastní zobrazovací jednotky.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Nástroj pro vykreslování základní třídy a nativní ovládací prvky](renderers.md)

Každý ovládací prvek Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Tento článek uvádí nástroj pro vykreslování a nativní ovládací prvek třídy, které implementují každé stránky Xamarin.Forms, rozložení, zobrazení a buňky.

## <a name="customizing-an-entryentrymd"></a>[Přizpůsobení položky](entry.md)

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) ovládací prvek umožňuje jeden řádek textu má být upraven. Tento článek ukazuje, jak vytvořit vlastního rendereru pro `Entry` ovládacího prvku, umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Přizpůsobení ContentPage](contentpage.md)

A [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) je vizuální prvek, který zobrazí jedno zobrazení a zabírá většinu obrazovky. Tento článek ukazuje, jak vytvořit vlastního rendereru pro `ContentPage` stránce, který umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu.

## <a name="customizing-a-mapmapindexmd"></a>[Přizpůsobení mapy](map/index.md)

Xamarin.Forms.Maps není úspěšný kvůli poskytuje abstrakce napříč platformami pro zobrazení mapy, které používají nativní mapování rozhraní API na každou platformu, k poskytování rychlého a důvěrně známé mapování prostředí pro uživatele. Toto téma ukazuje, jak vytvořit vlastní renderery pro `Map` ovládacího prvku, umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu.

## <a name="customizing-a-listviewlistviewmd"></a>[Přizpůsobení ListView](listview.md)

Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) je zobrazení, která zobrazuje kolekce dat jako svislý seznam. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky, který zapouzdřuje ovládacích prvcích seznam specifické pro platformu a nativní buňky rozložení, umožní větší kontrolu nad nativní seznamu řízení výkonu.

## <a name="customizing-a-viewcellviewcellmd"></a>[Přizpůsobení ViewCell](viewcell.md)

Xamarin.Forms [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) je přidaný do buňky [ `ListView` ](xref:Xamarin.Forms.ListView) nebo [ `TableView` ](xref:Xamarin.Forms.TableView), který obsahuje zobrazení definované uživatelem pro vývojáře. Tento článek ukazuje, jak vytvořit vlastního rendereru pro `ViewCell` , která je hostována v Xamarin.Forms `ListView` ovládacího prvku. Tím se zastaví výpočty rozložení Xamarin.Forms byla volána opakovaně během `ListView` posouvání.

## <a name="implementing-a-viewviewmd"></a>[Implementace zobrazení](view.md)

Xamarin.Forms vlastního uživatelského rozhraní ovládacích prvků musí být odvozený z: [ `View` ](xref:Xamarin.Forms.View) třídu, která se používá k umístění rozložení a ovládací prvky na obrazovce. Tento článek ukazuje, jak vytvořit vlastního rendereru pro Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení videa zobrazit náhled streamu z fotoaparátu zařízení.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementace HybridWebView](hybridwebview.md)

Tento článek ukazuje, jak vytvořit vlastního rendereru pro `HybridWebView` vlastní ovládací prvek, který ukazuje, jak vylepšit specifické pro platformu webové ovládací prvky umožňující kódu jazyka C# má být volána z jazyka JavaScript.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementace přehrávače videa](video-player/index.md)

Tento článek popisuje, jak psát renderery implementovat vlastní `VideoPlayer` ovládací prvek, který můžete přehrávat videa z webu, videa vloženy jako prostředky aplikace nebo videa, které jsou uložené v knihovně videí na zařízení uživatele. Je ukázán několik postupů, včetně implementace metody a vlastnosti jen pro čtení s možností vazby.


## <a name="related-links"></a>Související odkazy

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Vlastní Renderery (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Ukázková vlastní Renderery (Xamarin University Video)](http://bit.ly/xf-customrenderer)
