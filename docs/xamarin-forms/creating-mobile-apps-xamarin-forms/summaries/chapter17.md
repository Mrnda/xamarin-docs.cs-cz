---
title: Souhrn kapitole 17. Ovládnutí mřížky
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitole 17. Ovládnutí mřížky'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5f5b934b5f828bf6f5e8d4a0f0738c7db633aefb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995837"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Souhrn kapitole 17. Ovládnutí mřížky

[ `Grid` ](xref:Xamarin.Forms.Grid) Virtuálních sítí je mechanismus výkonné rozložení, který uspořádá podřízené do řádků a sloupců. Na rozdíl od podobně jako HTML `table` elementu, `Grid` slouží výhradně pro účely rozložení spíše než prezentace.

## <a name="the-basic-grid"></a>Základní mřížky

`Grid` je odvozen od [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1), která definuje [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) vlastnost, která `Grid` dědí. Můžete přejít k vyplnění tuto kolekci na XAML nebo kódu.

### <a name="the-grid-in-xaml"></a>Mřížky v XAML

Definice `Grid` v XAML obvykle začíná naplnění [ `RowDefinitions` ](xref:Xamarin.Forms.Grid.RowDefinitions) a [ `ColumnDefinitions` ](xref:Xamarin.Forms.Grid.ColumnDefinitions) kolekce `Grid` s [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) a [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition) objekty. To je, jak vytvořit počet řádků a sloupců `Grid`a jejich vlastnosti.

`RowDefinition` má [ `Height` ](xref:Xamarin.Forms.RowDefinition.Height) vlastnost a `ColumnDefinition` má [ `Width` ](xref:Xamarin.Forms.ColumnDefinition.Width) vlastnost, oba typu [ `GridLength` ](xref:Xamarin.Forms.GridLength), strukturu.

V XAML [ `GridLengthTypeConverter` ](xref:Xamarin.Forms.GridLengthTypeConverter) převede jednoduché textové řetězce do `GridLength` hodnoty. Na pozadí [ `GridLength` konstruktor](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) vytvoří `GridLength` hodnoty na základě čísla a hodnota typu [ `GridUnitType` ](xref:Xamarin.Forms.GridUnitType), výčet pomocí tří členů:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; Šířka nebo výška je zadán v jednotkách nezávislých na zařízení (číslo v XAML)
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; na výšku nebo šířku je autosized na základě obsahu buňky ("Auto" v XAML)
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; zbylé výšce nebo šířce je přidělen proporcionálně (číslo s "\*", označované jako *hvězdičky*, v XAML)

Každý podřízený `Grid` musíte být také přiřazeni řádků a sloupců (explicitně nebo implicitně). Překlenuje řádek a sloupec rozsahy jsou volitelné. Tyto jsou všechny zadané pomocí připojené vlastnosti umožňující vazbu &mdash; vlastnosti, které jsou definovány `Grid` ale nastavit na podřízené objekty `Grid`. `Grid` definuje čtyři statické připojené vlastnosti umožňující vazbu:

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; řádek založený na nule. Výchozí hodnota je 0
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; sloupec založený na nule. Výchozí hodnota je 0
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; počet řádků zahrnuje podřízené; Výchozí hodnota je 1
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; počet sloupců zahrnuje podřízené; Výchozí hodnota je 1

V kódu můžete použít program osm statické metody nastavit a získat tyto hodnoty:

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) a [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) a [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) a [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) a [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

V XAML pro nastavení tyto hodnoty použijete následující atributy:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) ukázce vytváření a inicializaci `Grid` v XAML.

`Grid` Dědí [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) vlastnost z `Layout` a definuje dvě další vlastnosti, které poskytují mezery mezi řádky a sloupce:

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) má výchozí hodnotu 6
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) má výchozí hodnotu 6

`RowDefinitions` a `ColumnDefinitions` kolekce nejsou nezbytně nutné. Pokud tomu tak není, `Grid` vytvoří řádků a sloupců pro `Grid` podřízené položky a poskytuje všechny výchozí `GridLength` z "\*" (hvězdička).

### <a name="the-grid-in-code"></a>Mřížky v kódu

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) Ukázka předvádí, jak vytvořit a naplnit `Grid` v kódu. Připojené vlastnosti můžete nastavit pro každý podřízený přímo nebo nepřímo pomocí volání Další `Add` metody jako [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) určené [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) rozhraní.

### <a name="the-grid-bar-chart"></a>Pruhový graf mřížky

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) příklad ukazuje, jak přidat více `BoxView` prvků, které mají `Grid` pomocí hromadného [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) metody. Ve výchozím nastavení tyto `BoxView` elementy mají stejnou šířku. Výška jednotlivých `BoxView` lze ovládat tak, aby připomínaly pruhový graf.

`Grid` v **GridBarChart** ukázkový sdílených složek `AbsoluteLayout` nadřazená zpočátku neviditelné `Frame`. Program také nastaví `TapGestureRecognizer` na každém `BoxView` používat `Frame` k zobrazení informací o které jste klepli panelu.

### <a name="alignment-in-the-grid"></a>Zarovnání v mřížce

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) Ukázka předvádí, jak používat `VerticalOptions` a `HorizontalOptions` vlastnosti podřízené položky v souladu `Grid` buňky.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) ukázkový stejně prostory `Button` elementy na střed v `Grid` buňky.

### <a name="cell-dividers-and-borders"></a>Rozdělení buňky a ohraničení

`Grid` Neobsahuje funkce, který vykreslí rozdělení buňky nebo ohraničení. Ale můžete si vytvořit svoje vlastní.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) ukazuje, jak definovat další řádky a sloupce speciálně pro dynamické `BoxView` prvky tak, aby napodoboval dělicí řádky.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) program nevytváří žádné další buňky, ale místo toho zarovná `BoxView` prvky v každé buňce tak, aby napodoboval ohraničení buňky.

## <a name="almost-real-life-grid-examples"></a>Téměř reálné příkladech mřížky

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) ukázkový používá `Grid` zobrazíte klávesnici:

[![Trojitá snímek klávesnici mřížky](images/ch17fg12-small.png "klávesnici mřížky")](images/ch17fg12-large.png#lightbox "klávesnici mřížky")

### <a name="responding-to-orientation-changes"></a>Reagování na změny orientace

`Grid` Pomůže strukturovat program reakce na změny orientace. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) ukázce technika, která se přesune mezi druhého řádku telefonu orientovaný na výšku a druhý sloupec telefonu orientované na šířku elementu.

Inicializuje program `Slider` prvků, které mají rozsah 0 až 255 a datové vazby používá pro zobrazení hodnoty jezdce v šestnáctkové soustavě. Protože `Slider` hodnoty jsou plovoucí desetinná čárka a .NET pro šestnáctkové funguje jenom s celými čísly, formátovací řetězec [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna pomáhá navýšení kapacity.



## <a name="related-links"></a>Související odkazy

- [Kapitola 17 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Ukázky kapitole 17](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Mřížka](~/xamarin-forms/user-interface/layouts/grid.md)
