---
title: Vlastní nástroji pro vykreslování
description: Uživatelská rozhraní Xamarin.Forms jsou vykreslovány pomocí nativní ovládací prvky typu cílovou platformu, povolíte Xamarin.Forms aplikací zachovat odpovídající vzhledu a chování pro každou platformu. Vlastní nástroji pro vykreslování umožňují vývojářům přepsání tohoto procesu, chcete-li přizpůsobit vzhled a chování Xamarin.Forms ovládacích prvků na každou platformu.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 6a2ee4b09426e6b4ff6dac7e1fd5221fc5b6d750
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="custom-renderers"></a>Vlastní nástroji pro vykreslování

_Uživatelská rozhraní Xamarin.Forms jsou vykreslovány pomocí nativní ovládací prvky typu cílovou platformu, povolíte Xamarin.Forms aplikací zachovat odpovídající vzhledu a chování pro každou platformu. Vlastní nástroji pro vykreslování umožňují vývojářům přepsání tohoto procesu, chcete-li přizpůsobit vzhled a chování Xamarin.Forms ovládacích prvků na každou platformu._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Úvod do vlastní nástroji pro vykreslování](introduction.md)

Přizpůsobení vzhledu a chování ovládacích prvků Xamarin.Forms zadejte vlastní nástroji pro vykreslování efektivní přístup. Mohou být použity pro malé stylů změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování. Tento článek obsahuje úvod do vlastní nástroji pro vykreslování a popisuje proces pro vytvoření vlastní zobrazovací jednotky.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Základní třídy zobrazovací jednotky a nativní ovládací prvky](renderers.md)

Každý prvek Xamarin.Forms musí doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. V tomto článku jsou uvedené zobrazovací jednotky a nativní řízení třídy, které implementují každou stránku Xamarin.Forms, rozložení, zobrazení a buňky.

## <a name="customizing-an-entryentrymd"></a>[Přizpůsobení položky](entry.md)

Platformě Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení umožňuje jeden řádek textu k provádění úprav. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro `Entry` řízení, umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Přizpůsobení ContentPage](contentpage.md)

A [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) je visual element, který zobrazí jedno zobrazení a zabírá Většina obrazovky. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro `ContentPage` stránce, který umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu.

## <a name="customizing-a-mapmapindexmd"></a>[Přizpůsobení mapy](map/index.md)

Xamarin.Forms.Maps poskytuje abstrakci a platformy pro zobrazení mapy, které používají nativní mapy rozhraní API na jednotlivých platformách zajistit mapu rychlého a známým prostředí pro uživatele. Toto téma ukazuje, jak vytvořit vlastní nástroji pro vykreslování pro `Map` řízení, umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu.

## <a name="customizing-a-listviewlistviewmd"></a>[Přizpůsobení ListView](listview.md)

Platformě Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) zobrazení, které zobrazí kolekce dat jako svislý seznam. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky zapouzdřující ovládací prvky seznamu specifické pro platformu a nativní buňky rozložení umožňuje větší kontrolu nad nativní seznamu řízení výkonu.

## <a name="customizing-a-viewcellviewcellmd"></a>[Přizpůsobení ViewCell](viewcell.md)

Platformě Xamarin.Forms [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) je buňku, která mohou být přidány do [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nebo [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/), která obsahuje zobrazení definované developer. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro `ViewCell` hostující uvnitř platformě Xamarin.Forms `ListView` ovládacího prvku. To zastaví výpočty rozložení Xamarin.Forms z opakovaně volané během `ListView` posouvání.

## <a name="implementing-a-viewviewmd"></a>[Implementace zobrazení](view.md)

Xamarin.Forms vlastní uživatelské rozhraní ovládací prvky by měl být odvozen z [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třídy, která se používá k umístění rozložení a ovládacích prvků na obrazovce. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky Xamarin.Forms vlastního ovládacího prvku, který se používá k zobrazení náhledu datový proud videa z fotoaparátu zařízení.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementace HybridWebView](hybridwebview.md)

Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro `HybridWebView` vlastní ovládací prvek, který ukazuje, jak zvýšit ovládací prvky specifické pro platformu webového povolit C# – kód má být volána z jazyka JavaScript.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementace přehrávání videa](video-player/index.md)

Tento článek ukazuje, jak napsat nástroji pro vykreslování implementovat vlastní `VideoPlayer` ovládací prvek, který můžete přehrát videa z webových, videa vložených jako prostředky aplikace nebo videa uložený v knihovně videa na zařízení uživatele. Je ukázán několik postupů, včetně implementace metod a vlastností, vázat jen pro čtení. 


## <a name="related-links"></a>Související odkazy

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Vlastní nástroji pro vykreslování (Xamarin univerzity Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Ukázka vlastních nástroji pro vykreslování (Xamarin univerzity Video)](http://bit.ly/xf-customrenderer)
