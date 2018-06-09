---
title: Navigační Xamarin.Forms
description: Tato příručka vysvětluje, jak provádět navigace v Xamarin.Forms aplikace. Xamarin.Forms poskytuje několik možností, navigace různé stránky, v závislosti na typu stránka používá.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 90aedee42af7ed1788110e832fb3b435d870ee77
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241953"
---
# <a name="xamarinforms-navigation"></a>Navigační Xamarin.Forms

_Xamarin.Forms poskytuje několik možností, navigace různé stránky, v závislosti na typu stránka používá._

![](images/page-types.png "Typy Xamarin.Forms stránky")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Hierarchická navigace](hierarchical.md)

[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Třída poskytuje hierarchické navigační prostředí, kde je možné procházet stránky, dopředný a podle potřeby zpětné uživatele. Třída implementuje navigační jako zásobník ven (LIFO) last-in služby [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objekty.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Platformě Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) se skládá z seznam karty a větší oblast podrobností, s každé kartě načítání obsahu do oblasti podrobností.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Platformě Xamarin.Forms [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) je stránka, která uživatelé mohou prstem stranu procházet stránky obsahu, jako je galerie.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Platformě Xamarin.Forms [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) je stránka, která spravuje dvou stránkách související informace – hlavní stránky, který představuje položky a podrobnosti o stránku, která zobrazí podrobné informace o položky na hlavní stránce.

## <a name="modal-pagesmodalmd"></a>[Modální stránky](modal.md)

Xamarin.Forms taky poskytuje podporu pro modální stránky. Modální stránky doporučuje uživatelům k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena.

## <a name="displaying-pop-upspop-upsmd"></a>[Zobrazování automaticky otevíraných oken](pop-ups.md)

Xamarin.Forms poskytuje dva elementy pop množství jako uživatelského rozhraní: výstrahu a akci listu. Tyto prvky rozhraní lze použít k dotazu na uživatele jednoduchých dotazů a můžete uživatele prostřednictvím úlohy.
