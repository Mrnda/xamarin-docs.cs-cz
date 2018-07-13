---
title: Navigace Xamarin.Forms
description: Tato příručka vysvětluje, jak k provádění navigace u aplikací Xamarin.Forms. Xamarin.Forms poskytuje řadu jiných stránek navigační prostředí, v závislosti na typu stránky se používají.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994725"
---
# <a name="xamarinforms-navigation"></a>Navigace Xamarin.Forms

_Xamarin.Forms poskytuje řadu jiných stránek navigační prostředí, v závislosti na typu stránky se používají._

![](images/page-types.png "Typy stránek Xamarin.Forms")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Hierarchická navigace](hierarchical.md)

[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Třída poskytuje hierarchické navigační prostředí, kde uživatel je možné procházet stránkách vpřed a zpět, podle potřeby. Třída implementuje navigace jako poslední dovnitř, první (ven LIFO) zásobníku [ `Page` ](xref:Xamarin.Forms.Page) objekty.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) se skládá ze seznamu karty a větší oblasti podrobností, každá karta načítání obsahu do oblasti podrobností.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) je stránka, která uživatelé můžou potažením prstem přejděte na stranu pro navigaci mezi stránkami obsahu, jako je galerie.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) je stránka, která spravuje dvě stránky související informace – stránku předlohy, která uvede počet položek a podrobnosti o stránku, která uvede podrobnosti o položkách ve stránce předlohy.

## <a name="modal-pagesmodalmd"></a>[Modální stránky](modal.md)

Xamarin.Forms také poskytuje podporu pro modální stránky. Modální stránky vyzývá uživatele k dokončení samostatná úloha, která nemůže být opuštění dokud je úloha dokončena nebo zrušena.

## <a name="displaying-pop-upspop-upsmd"></a>[Zobrazování automaticky otevíraných oken](pop-ups.md)

Xamarin.Forms poskytuje dva prvky pop registrace jako uživatelského rozhraní: upozornění a listu akce. Tyto prvky rozhraní je možné, požádejte uživatele jednoduché otázky a provedou uživatele úkoly.
