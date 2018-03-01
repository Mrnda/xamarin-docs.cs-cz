---
title: TableView
description: "K dispozici posouvání nabídek, nastavení a vstupní formuláře."
ms.topic: article
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 2dff3ca311341ffe1e30145ad436820b715973b0
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="tableview"></a>TableView

[Zobrazení Tabulka](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) je zobrazení pro posouvatelného seznamy dat nebo volby tam, kde existují řádky, které Nesdílejte stejné šablony. Na rozdíl od [ListView](~/xamarin-forms/user-interface/listview/index.md), zobrazení tabulka neobsahuje koncept `ItemsSource`, takže položky je nutné přidat jako podřízené objekty ručně.

Tato příručka se skládá z následujících částí:

- **[Případy použití](#Use_Cases)**  &ndash; kdy použít zobrazení Tabulka místo ListView nebo vlastní zobrazení.
- **[Struktura zobrazení Tabulka](#TableView_Structure)**  &ndash; hierarchie zobrazení, která je vyžadována během zobrazení Tabulka.
- **[Vzhled zobrazení Tabulka](#TableView_Appearance)**  &ndash; možnosti přizpůsobení zobrazení Tabulka.
- **[Předdefinované buněk](#Built-In_Cells)**  &ndash; buňky integrované možnosti, včetně [EntryCell](#EntryCell) a [SwitchCell](#SwitchCell).
- **[Vlastní buněk](#Custom_Cells)**  &ndash; jak provádět vlastní vlastní buněk.

![](tableview-images/tableview-all-sml.png "Příklad zobrazení Tabulka")

<a name="Use_Cases" />

## <a name="use-cases"></a>Případy použití

Zobrazení Tabulka je užitečné, když:

- prezentace seznamu nastavení
- shromažďování dat ve formuláři, nebo
- Zobrazuje data, která se zobrazí jinak z řádku řádek (např. čísla, procenta a bitové kopie).

Zobrazení Tabulka zpracovává posouvání a rozložení řádků v atraktivní částech, potřebují společné výše uvedených scénářů. `TableView` Řízení používá každou platformu základní ekvivalentní zobrazení, pokud je k dispozici, vytváření nativní vzhled pro každou platformu.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>Struktura zobrazení Tabulka

Elementy v `TableView` jsou uspořádány do oddílů. V kořenovém adresáři `TableView` je `TableRoot`, což je nadřazená na jeden nebo více `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Každý `TableSection` se skládá z nadpisu a jeden nebo více ViewCells. Tady vidíte `TableSection`na `Title` vlastnost nastavena na hodnotu *"Okruh"* v konstruktoru:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

K provedení se stejné rozvržení jako výše v jazyce XAML:

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
      <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>Vzhled zobrazení Tabulka

Zobrazení Tabulka zpřístupní `Intent` vlastnost, která je výčet z následujících možností:

- **Data** &ndash; pro použití při zobrazení datové položky. Všimněte si, že [ListView](~/xamarin-forms/user-interface/listview/index.md) může být lepším řešením pro posouvání seznam data.
- **Formulář** &ndash; pro použití při zobrazení Tabulka funguje jako formulář.
- **Nabídky** &ndash; pro použití při prezentací nabídky výběry.
- **Nastavení** &ndash; pro použití při zobrazení seznamu nastavení konfigurace.

`TableIntent` Zvolíte, může mít vliv jak `TableView` se zobrazí na každé platformě. I když existují, nerušte zaškrtnutí rozdíly, je osvědčeným postupem vyberte `TableIntent` která nejvíce odpovídá jak máte v úmyslu použít v tabulce.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Předdefinované buněk

Xamarin.Forms se dodává s integrovanou buňky shromažďování a zobrazení informací. I když ListView a zobrazení Tabulka můžete použít všechny stejné buňky, tady jsou nejdůležitější pro zobrazení Tabulka scénář:

- **SwitchCell** &ndash; pro prezentování a zaznamenávání stavu hodnotu true nebo false, společně s textový popisek.
- **EntryCell** &ndash; pro prezentování a zachycení textu.

V tématu [vzhledu buněk ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) podrobný popis [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) a [funkce ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) ovládací prvek, který chcete použít pro prezentování a zachycení zapnutí nebo vypnutí nebo `true` / `false` stavu.

SwitchCells mít jeden řádek textu upravit a vlastnost zapnout nebo vypnout. Obě tyto vlastnosti jsou vazbu.

- `Text` &ndash; text, který se zobrazí vedle přepínač.
- `On` &ndash; zda přepínač se zobrazuje jako zapnuto nebo vypnuto.

Všimněte si, že `SwitchCell` zpřístupní `OnChanged` událostí, umožňuje reagovat na změny ve stavu na buňku.

![](tableview-images/switch-cell.png "Příklad SwitchCell")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) je užitečné, když je třeba zobrazit text data, která může uživatel upravit. `EntryCell`s nabízejí následující vlastnosti, které se dají přizpůsobit:

- `Keyboard` &ndash; Klávesnice zobrazíte při úpravách. Existují možnosti pro takové věci, jako číselné hodnoty, e-mailu, telefonních čísel atd. [Najdete v dokumentaci rozhraní API](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/).
- `Label` &ndash; Text popisku, který chcete zobrazit napravo od textového pole.
- `LabelColor` &ndash; Barva textu popisku.
- `Placeholder` &ndash; Text, který se zobrazí v poli položku, když je null nebo prázdný. Tento text zmizí při zahájení zadávání textu.
- `Text` &ndash; Text v poli položku.
- `HorizontalTextAlignment` &ndash; Vodorovné zarovnání textu. Může být center doleva nebo doprava zarovnaný. [Najdete v dokumentaci rozhraní API](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/).

Všimněte si, že `EntryCell` zpřístupní `Completed` událost, která je aktivována, pokud uživatel narazí 'v' na klávesnici při úpravách textu.

![](tableview-images/entry-cell.png "Příklad EntryCell")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Vlastní buněk
Pokud integrované buněk nejsou dost, vlastní buněk slouží k zaznamenání dat takovým způsobem, který dává smysl pro vaši aplikaci a k dispozici. Můžete například jezdce chcete umožnit uživatelům zvolit krytí bitové kopie k dispozici.

Všechny vlastní buňky musí být odvozeny od [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), že všechny buňky předdefinované typy použijte stejnou základní třídu.

Jedná se o příklad vlastní buňky:

![](tableview-images/custom-cell.png "Příklad vlastní buňky")

### <a name="xaml"></a>XAML
Zde je XAML pro vytvoření rozložení výše:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
        <TableView Intent="Settings">
            <TableRoot>
                <TableSection Title="Getting Started">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Image Source="bulb.png" />
                            <Label Text="left"
                              TextColor="#f35e20" />
                            <Label Text="right"
                              HorizontalOptions="EndAndExpand"
                              TextColor="#503026" />
                        </StackLayout>
                    </ViewCell>
                </TableSection>
            </TableRoot>
        </TableView>
    </ContentPage.Content>
</ContentPage>

```

Výše uvedené XAML provádí mnoho. Můžeme rozdělit ho:

- Kořenový element v části `TableView` je `TableRoot`.
- Je `TableSection` bezprostředně pod `TableRoot`.
- `ViewCell` Je definována přímo pod oddíl tabulky. Na rozdíl od `ListView`, `TableView` nevyžaduje, aby tento vlastní (nebo některého) buněk jsou definovány v `ItemTemplate`.
- StackLayout slouží ke správě rozložení vlastní buňky. Všechny rozložení může zde.

### <a name="cnum"></a>C&num;

Protože `TableView` funguje s statických dat nebo dat, která je ručně změnit, nemá koncept šablony položky. Místo toho vlastní buněk ručně vytvořit a umístí do tabulky. Poznámka: postup vytvoření vlastní buňky, dědí z `ViewCell`, pak jejím přidáním do `TableView` jako jste by předdefinované buňku, je také podporována.
Tady je kód c# k provádění rozložení výše:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

C# výše provádí mnoho. Můžeme rozdělit ho:

- Kořenový element v části `TableView` je `TableRoot`.
- Je `TableSection` bezprostředně pod `TableRoot`.
- `ViewCell` Je definována přímo pod oddíl tabulky. Na rozdíl od `ListView`, `TableView` nevyžaduje, aby tento vlastní (nebo některého) buněk jsou definovány v `ItemTemplate`.
- StackLayout slouží ke správě rozložení vlastní buňky. Všechny rozložení může zde.

Všimněte si, že třída pro vlastní buňky nikdy definovaná. Místo toho `ViewCell`na zobrazení je nastavena pro konkrétní instanci `ViewCell`.



## <a name="related-links"></a>Související odkazy

- [Zobrazení Tabulka (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
