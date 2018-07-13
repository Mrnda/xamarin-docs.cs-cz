---
title: Výběr Xamarin.Forms
description: Výběr Xamarin.Forms zobrazuje krátký seznam položek, ze kterého může uživatel vybrat položku. Tento článek vysvětluje, jak použít třídu pro výběr vybrat text položku ze seznamu data.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: c852cd29197b000ed1ff53853d64cfa25fb699e7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996594"
---
# <a name="xamarinforms-picker"></a>Výběr Xamarin.Forms

_Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data._

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) zobrazuje krátký seznam položek, ze kterého může uživatel vybrat položku. `Picker` definuje vlastnosti osm:

- [`Title`](xref:Xamarin.Forms.Picker.Title) typu `string`, která má výchozí hodnotu `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) typu `IList`, seznamu zdroj položek, které chcete zobrazit, kde je použit výchozí `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) typ `int`, index vybrané položky, které výchozí hodnota -1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) typu `object`, vybrané položky, výchozí nastavení je `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) typu [ `Color` ](xref:Xamarin.Forms.Color), barva použitá k zobrazení textu, kde je použit výchozí [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) typu [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), která má výchozí hodnotu [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) typu `string`, která má výchozí hodnotu `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) typ `double`, která má výchozí hodnotu-1.0.

Všechny vlastnosti osm využívají [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objekty, které znamená, že může být styl a vlastnosti může být terčem datové vazby. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) a [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) vlastnosti mají výchozí režim vazby [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), což znamená, že můžou být cíle datové vazby v aplikaci, která se používá [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektury. Informace o nastavení vlastností písma, naleznete v tématu [písma](~/xamarin-forms/user-interface/text/fonts.md).

A [ `Picker` ](xref:Xamarin.Forms.Picker) nezobrazuje žádná data, při prvním zobrazení. Místo toho hodnota jeho [ `Title` ](xref:Xamarin.Forms.Picker.Title) vlastnost se zobrazí jako zástupný symbol v Iosu a Androidu platformy:

[![](images/picker-initial.png "Počáteční výběr zobrazení")](images/picker-initial-large.png#lightbox "počáteční výběr zobrazení")

Když [ `Picker` ](xref:Xamarin.Forms.Picker) zisky fokus, data se zobrazí a uživatel může vybrat položku:

[![](images/picker-selection.png "Výběr výběru nějaké položky")](images/picker-selection-large.png#lightbox "výběr výběru nějaké položky")

[ `Picker` ](xref:Xamarin.Forms.Picker) Aktivuje [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) událost, když uživatel vybere položku. Po výběru, vybrané položky se zobrazí při `Picker`:

![](images/picker-after-selection.png "Výběr po výběru")

Existují dva postupy pro sestavování [ `Picker` ](xref:Xamarin.Forms.Picker) s daty:

- Nastavení [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) vlastnost k datům, který se má zobrazit. Toto je doporučený postup, která byla zavedena v Xamarin.Forms 2.3.4. Další informace najdete v tématu [nastavení vlastnosti ItemsSource výběru](populating-itemssource.md).
- Přidání dat, který se zobrazí [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekce. Tato technika se původní proces pro sestavování [ `Picker` ](xref:Xamarin.Forms.Picker) s daty. Další informace najdete v tématu [přidání dat do kolekce položek ovládacího prvku Výběr](populating-items.md).

## <a name="related-links"></a>Související odkazy

- [Výběr](xref:Xamarin.Forms.Picker)
