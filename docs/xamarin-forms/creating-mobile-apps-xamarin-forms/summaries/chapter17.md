---
title: "Shrnutí kapitoly 17. Ovládnutí koncepcí mřížky"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 4f76b1060ee8a672319683525470aee00e3db001
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Shrnutí kapitoly 17. Ovládnutí koncepcí mřížky

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Je výkonný rozložení mechanismus, který uspořádá své podřízené objekty do řádků a sloupců buněk. Na rozdíl od podobné HTML `table` elementu, `Grid` je výhradně pro účely rozložení místo prezentace.

## <a name="the-basic-grid"></a>Základní mřížky

`Grid` odvozená z [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), která definuje [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) vlastnost který `Grid` dědí. Tuto kolekci na XAML nebo kódu, můžete vyplnit.

### <a name="the-grid-in-xaml"></a>Mřížky v jazyce XAML

Definice `Grid` v jazyce XAML obvykle začíná naplnění [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) a [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) kolekce `Grid` s [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) a [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) objekty. Toto je, jak vytvořit počet řádků a sloupců `Grid`a jejich vlastnosti.

`RowDefinition` má [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) vlastnost a `ColumnDefinition` má [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) vlastnost, obě typu [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), struktury.

V jazyce XAML [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) převede jednoduché textové řetězce do `GridLength` hodnoty. Na pozadí [ `GridLength` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) vytvoří `GridLength` hodnota založená na číslo a hodnota typu [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), výčet se tři členy:

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) &mdash; šířky nebo výšky je zadána v jednotkách nezávislé na zařízení (číslo v jazyce XAML)
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) &mdash; výška nebo šířka je autosized podle obsahu buňky ("Auto" v jazyce XAML)
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) &mdash; velikost zbývajícího výšky a šířky je přidělena úměrně (číslo s "\*", volané *hvězdičkami*, v jazyce XAML)

Každou podřízenou `Grid` zapotřebí také přiřazení řádků a sloupců (explicitně nebo implicitně). Zahrnuje řádek a sloupec rozsahy jsou volitelné. Tyto jsou určeny všechny připojené vlastnosti vazbu pomocí &mdash; vlastnosti, které jsou definovány `Grid` ale nastavený na podřízené objekty `Grid`. `Grid` definuje čtyři statické připojené vazbu vlastnosti:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) &mdash; počítaný od nuly řádek; Výchozí hodnota je 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) &mdash; počítaný od nuly sloupec; Výchozí hodnota je 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) &mdash; počet řádků podřízená zahrnuje; Výchozí hodnota je 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) &mdash; počet sloupců podřízených zahrnuje; Výchozí hodnota je 1

V kódu programu, můžete použít osm statických metod nastavit a získat tyto hodnoty:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) A [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) A [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) A [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) A [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

V jazyce XAML použijte následující atributy pro nastavení tyto hodnoty:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) příklad ukazuje vytvoření a inicializace `Grid` v jazyce XAML.

`Grid` Dědí [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) vlastnost z `Layout` a definuje dva další vlastnosti, které obsahují mezery mezi řádky a sloupce:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) nemá výchozí hodnotu 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) nemá výchozí hodnotu 6

`RowDefinitions` a `ColumnDefinitions` kolekce nejsou nezbytně nutné. Pokud tomu tak není, `Grid` vytvoří řádků a sloupců pro `Grid` podřízené objekty a poskytne jim všechny výchozí `GridLength` z "\*" (hvězdička).

### <a name="the-grid-in-code"></a>Mřížky v kódu

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) ukázka ukazuje, jak vytvořit a naplnit `Grid` v kódu. Můžete nastavit přidružené vlastnosti pro všechny podřízené přímo nebo nepřímo voláním Další `Add` metody, jako [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) definované [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) rozhraní.

### <a name="the-grid-bar-chart"></a>Pruhový graf mřížky

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) příklad ukazuje, jak přidat více `BoxView` elementů `Grid` pomocí hromadného [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) metoda. Ve výchozím nastavení tyto `BoxView` prvky mít stejnou šířku. Výška jednotlivých `BoxView` lze řídit tak, aby připomínaly pruhový graf.

`Grid` v **GridBarChart** ukázkové sdílené složky `AbsoluteLayout` nadřazené s původně neviditelná `Frame`. Program nastaví také `TapGestureRecognizer` na každém `BoxView` používat `Frame` zobrazíte informace o tapped panelu.

### <a name="alignment-in-the-grid"></a>Zarovnání v mřížce

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) příklad ukazuje způsob použití `VerticalOptions` a `HorizontalOptions` vlastnosti, které chcete-li zarovnat dětech do `Grid` buňky.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) ukázkové stejně prostory `Button` elementy zarovnaný na střed v `Grid` buněk.

### <a name="cell-dividers-and-borders"></a>Rozdělení buňky a ohraničení

`Grid` Neobsahuje funkce, která nevykresluje rozdělení buňky nebo ohraničení. Však můžete nastavit vlastní.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) ukazuje, jak definovat další řádky a sloupce speciálně pro dynamické `BoxView` elementy tak, aby napodoboval dělicí čáry.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) program nevytvoří žádné další buňky, ale místo toho zarovnává `BoxView` elementy v každé buňce tak, aby napodoboval ohraničení buňky.

## <a name="almost-real-life-grid-examples"></a>Příklady téměř reálnými mřížky

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) ukázkové používá `Grid` zobrazíte klávesnici:

[![Trojitá snímek obrazovky klávesnici mřížky](images/ch17fg12-small.png "klávesnici mřížky")](images/ch17fg12-large.png#lightbox "klávesnici mřížky")

### <a name="responding-to-orientation-changes"></a>Reagovat na změny orientace

`Grid` Může pomoci struktury program reagovat na změny orientace. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) příklad ukazuje postup, který přesouvá element mezi druhý řádek telefon orientované na výšku a druhý sloupec Telefon orientované na šířku.

Inicializuje program `Slider` elementy do rozsahu 0 až 255 a používá vazby na data zobrazit hodnotu posuvníků v šestnáctkové soustavě. Protože `Slider` hodnoty jsou plovoucí desetinná čárka a .NET formátování řetězce pro hexadecimální funguje pouze s celými čísly, [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny pomáhá.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 17 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Ukázky kapitoly 17](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Mřížka](~/xamarin-forms/user-interface/layouts/grid.md)
