---
title: Výběr Xamarin.Forms
description: Nástroje pro výběr Xamarin.Forms zobrazí zkrácený seznam položek, ze kterých si uživatel může vybrat položku. Tento článek vysvětluje způsob použití třídy Výběr a vyberte položku text ze seznamu data.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 82ae36a7be139e2a93d0e5c43c4bad355c49f217
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245036"
---
# <a name="xamarinforms-picker"></a>Výběr Xamarin.Forms

_Výběr zobrazení je ovládací prvek pro výběr textu položky ze seznamu data._

Platformě Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) zobrazí zkrácený seznam položek, ze kterých si uživatel může vybrat položku. `Picker` definuje vlastnosti osm:

- [`Title`](xref:Xamarin.Forms.Picker.Title) typu `string`, což výchozí nastavení `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) typu `IList`, seznamu zdrojů položky k zobrazení, kde je použit výchozí `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) typu `int`, index vybrané položky, které bude jako výchozí nastavena na hodnotu -1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) typu `object`, vybrané položky, výchozí nastavení je `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) typu [ `Color` ](xref:Xamarin.Forms.Color), barvu použitou k zobrazení textu, který se standardně [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) typu [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), což výchozí nastavení [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) typu `string`, což výchozí nastavení `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) typu `double`, což výchozí nastavení-1.0.

Všechny vlastnosti osm jsou zajišťované [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objekty, které znamená, že může být ve vlastnostech lze cíle datové vazby. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) a [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) vlastnosti mají výchozí režim vazba z [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), což znamená, že mohou být cíle vazby dat v aplikaci, která používá [Model-View-ViewModel (modelem MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektura. Informace o nastavení písma vlastnostech najdete v tématu [písem](~/xamarin-forms/user-interface/text/fonts.md).

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) nezobrazí žádná data, jakmile se nejprve zobrazí. Místo toho hodnotu jeho [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) vlastnosti se zobrazí jako zástupný znak na iOS a Android platformách:

[![](images/picker-initial.png "Počáteční výběr zobrazení")](images/picker-initial-large.png#lightbox "počáteční výběr zobrazení")

Když [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) zvýšení fokus, jeho data se zobrazí a uživatele můžete vybrat položku:

[![](images/picker-selection.png "Výběr výběrem položky")](images/picker-selection-large.png#lightbox "výběr výběrem položky")

[ `Picker` ](xref:Xamarin.Forms.Picker) Aktivuje [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) událost, když uživatel vybere položku. Následující výběr, je ve vybrané položky `Picker`:

![](images/picker-after-selection.png "Výběr po výběru")

Existují dvě metody pro sestavování [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) s daty:

- Nastavení [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) vlastnost k datům, který se má zobrazit. Toto je doporučená techniku, která byla představena v Xamarin.Forms 2.3.4. Další informace najdete v tématu [nastavení vlastnost ItemsSource výběr](populating-itemssource.md).
- Přidání dat, který se má zobrazit na [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekce. Tento postup se původní proces pro sestavování [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) s daty. Další informace najdete v tématu [přidání dat do kolekce položky ovládacího prvku Výběr](populating-items.md).

## <a name="related-links"></a>Související odkazy

- [Výběr](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
