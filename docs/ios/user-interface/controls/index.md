---
title: Ovládacích prvků uživatelského rozhraní v Xamarin.iOS
description: Tento dokument obsahuje odkazy na příručky, které popisují různých iOS ovládacích prvků uživatelského rozhraní pro Xamarin.iOS vývojáře k dispozici. Odkazovaný obsah popisuje výstrahy, tlačítka, zobrazení kolekcí, bitové kopie, ruční ovládací prvky, mapy, popisky, výběr, výběr data a další.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 477cb4e479224b9e795f3460e0f59e0aa2038630
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790039"
---
# <a name="user-interface-controls-in-xamarinios"></a>Ovládacích prvků uživatelského rozhraní v Xamarin.iOS

Tento dokument uvádí některé z nejběžnějších ovládacích prvků uživatelského rozhraní iOS a způsob jejich použití.

## <a name="alertsalertsmd"></a>[Upozornění](alerts.md)

Od verze iOS 8, UIAlertController má dokončené nahrazené UIActionSheet a UIAlertView z nich jsou nyní zastaralé.

## <a name="buttonsbuttonsmd"></a>[Tlačítka](buttons.md)

UIButton třída se používá k reprezentaci různých různé styly tlačítka na iOS obrazovky. Tato část představuje různé možnosti pro práci s tlačítka v iOS.

## <a name="collection-viewsuicollectionviewmd"></a>[Zobrazení kolekcí](uicollectionview.md)

Zobrazení kolekce, k dispozici v `UICollectionView` třídy, jsou nový koncept iOS 6, která zavést nabízí více položek na obrazovce pomocí rozložení. Vzory pro poskytování dat `UICollectionView` vytvoření položky a interakci s tyto položky podle stejné delegování a data zdroje vzory pro běžně používané v iOS vývoj.

## <a name="imagesimagemd"></a>[Obrázky](image.md)

Přidání bitové kopie do vaší aplikace vyžaduje dva kroky: nejprve přidejte bitové kopie do projektu; pak přidejte ovládacími prvky a kódem jejich zobrazení na obrazovce. Odkazovat [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) článek podrobnější pokrytí bitové kopie v Xamarin.iOS.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[Ruční ovládací prvky kamery](intro-to-manual-camera-controls.md)

Ovládací prvky fotoaparát ruční poskytované `AVFoundation Framework` v iOS 8, povolení mobilní aplikace, abyste mohli plnou kontrolu nad fotoaparát zařízení s iOS. Tato přesné úrovni řízení lze vytvořit profesionální úrovni fotoaparát aplikace a poskytnout umělcem složení tím, že postupně je upravujte parametry fotoaparát při pořizování stále bitovou kopii nebo video.

## <a name="mapsios-mapsindexmd"></a>[Mapy](ios-maps/index.md)

Mapování jsou běžnou funkcí ve všech operačních systémech mobilních moderní. iOS nabízí podporu mapování nativně prostřednictvím rozhraní Kit mapy. Pomocí mapy Kit aplikace snadno přidat bohatý a interaktivní mapy. Tyto mapy lze přizpůsobit různými způsoby, například přidání poznámek k označení umístění na mapě a Překrytí grafiky libovolný tvarů. Mapa Kit i má integrovanou podporu pro zobrazení aktuálního umístění zařízení.

## <a name="labelslabelsmd"></a>[Popisky](labels.md)

`UILabel` Řízení se používá pro zobrazení jeden a více řádků číst pouze text.

## <a name="pickers-and-date-pickerspickermd"></a>[Výběr a výběr data](picker.md)

Ovládací prvek výběru zobrazí wheel jako ovládací prvek obsahující posuvného seznam hodnot s se zvýrazněnou vybrané hodnoty. Uživatelé otočit kolečka a vyberte možnost, který jim vyhovuje.

Jeden konkrétní uživatel případu pro výběr ji nastavit datum nebo čas. Zajistit pro toto Apple vytvořil vlastní podtřídou třídy UIPickerView názvem UIDatePicker.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[Indikátory průběhu a aktivit](progress-activity-indicator.md)

iOS nabízí dva způsoby hlavní k určení průběhu ve vaší aplikaci: indikátory aktivitu (včetně konkrétní _sítě_ ukazatel aktivity) a indikátory průběhu.

## <a name="search-barssearchbarmd"></a>[Hledání pruhy](searchbar.md)

UISearchBar se používá k hledání v seznamu hodnot. 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[Posuvníky, Steppers a Segmentovaným ovládací prvky](slider-switch-segmented-controls.md)

V ovládacím prvku posuvník umožňuje jednoduché výběr číselnou hodnotu v rozsahu. používá iOS `UISwitch` jako logická hodnota vstupu, který může být reprezentována přepínač na jiných platformách. Segmentovaným řízení je lze snadno povolit uživatelům interakci s malý počet možností.

## <a name="stack-viewuistackviewmd"></a>[Zobrazení za sebou](uistackview.md)

Ovládací prvek zobrazení zásobníku (`UIStackView`) využívá sílu automatického rozložení a velikost třídy, které slouží ke správě více dílčích zobrazení, vodorovně nebo svisle, která dynamicky reaguje na velikost orientaci a obrazovky zařízení s iOS.

## <a name="tables-and-cellstablesindexmd"></a>[Tabulky a buňky](tables/index.md)

jeho část představuje třídy používané k vytváření a zobrazování tabulky pak poskytuje příklady, jak je používat v Xamarin.iOS. Bude se vztahovat pomocí výchozí vzhled pro tabulky, přizpůsobení rozložení, implementace úpravy a použití Xamarin iOS Designer pro vizuální návrh tabulky. Někdy zobrazení je samozřejmě seznam řádků (například aplikace Hudba) a dalších akcí, kdy je obtížné rozpoznat ovládacího prvku tabulky (např. úpravy v aplikaci kontakty nebo konverzace v aplikaci zprávy).

## <a name="text-inputtext-inputmd"></a>[Zadávání textu](text-input.md)

Přijetí zadávání textu uživatele se provádí pomocí `UITextField` pro jeden řádek vstupy a UITextView Víceřádkový upravovat text. Můžete buď z těchto prvků přetažením na obrazovce a dvakrát klikněte na nastavit počáteční text.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[Panely karet a jejich kontrolery](creating-tabbed-applications.md)

aplikace pro iOS pomocí uživatelského rozhraní navigace na kartě jsou vytvořené pomocí třídy UITabBarController. V tomto článku budeme zabývat postup nastavení na kartách aplikaci, která obsahuje několik kontrolery a zobrazení. Potom podíváme jak načíst UITabBarController, pokud není kořenový řadič, například po přihlašovací obrazovku.

## <a name="web-viewsuiwebviewmd"></a>[Webová zobrazení](uiwebview.md)

V tomto článku se podíváme na každé tři webových zobrazení poskytovaných společností Apple: `UIWebView`, `WKWebview`, a `SFSafariViewController`, jejich podobnosti a rozdíly a jejich použití.

## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
