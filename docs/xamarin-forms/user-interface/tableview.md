---
title: Zobrazení Xamarin.Forms tabulka
description: Tento článek vysvětluje, jak použít třídu zobrazení Xamarin.Forms tabulka prezentovat posouvání nabídek, nastavení a vstupních formulářů v aplikacích.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 47cd79611cfeaf48c0422772d8f3e75eb57ba771
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996050"
---
# <a name="xamarinforms-tableview"></a>Zobrazení Xamarin.Forms tabulka

[Zobrazení Tabulka](xref:Xamarin.Forms.TableView) je zobrazení pro posuvný seznam data nebo možnosti tam, kde existují řádky, které nechcete sdílet stejnou šablonu. Na rozdíl od [ListView](~/xamarin-forms/user-interface/listview/index.md), zobrazení tabulka neobsahuje koncept `ItemsSource`, takže položky musí být přidány jako podřízené objekty ručně.

Tato příručka se skládá z následujících částí:

- **[Případy použití](#Use_Cases)**  &ndash; kdy použít zobrazení Tabulka místo ListView nebo vlastní zobrazení.
- **[Struktura zobrazení Tabulka](#TableView_Structure)**  &ndash; hierarchii zobrazení, která je potřeba v rámci zobrazení Tabulka.
- **[Vzhled zobrazení Tabulka](#TableView_Appearance)**  &ndash; možnosti vlastního nastavení pro zobrazení Tabulka.
- **[Integrované buňky](#Built-In_Cells)**  &ndash; buňky integrované možnosti, včetně [EntryCell](#EntryCell) a [SwitchCell](#SwitchCell).
- **[Vlastní buňky](#Custom_Cells)**  &ndash; jak vytvořit vlastní vlastní buňky.

![](tableview-images/tableview-all-sml.png "Příklad zobrazení Tabulka")

<a name="Use_Cases" />

## <a name="use-cases"></a>Případy použití

Zobrazení Tabulka je užitečné, když:

- prezentace seznamu nastavení,
- shromažďování dat ve formuláři, nebo
- zobrazení dat, která se zobrazí odlišně od řádků na řádek (např. čísla, procenta a bitové kopie).

Zobrazení Tabulka zpracovává posouvání a rozložení řádků v atraktivní oddíly, potřebují společné pro výše zmíněné situace umožnili. `TableView` Ovládací prvek používá jednotlivé platformy základní ekvivalentní zobrazení, pokud je k dispozici, vytvoření nativní vzhledu pro každou platformu.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>Struktura zobrazení Tabulka

Prvky `TableView` jsou uspořádány do oddílů. V kořenovém adresáři `TableView` je `TableRoot`, což je rodiče, aby jeden nebo více `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Každý `TableSection` se skládá ze záhlaví a jeden nebo více ViewCells. Tady vidíme `TableSection`společnosti `Title` vlastnost nastavena na hodnotu *"Kanál"* v konstruktoru:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

K provedení stejné rozložení, jak je uvedeno výše v XAML:

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

Zobrazení tabulka uvádí `Intent` vlastnost, která je výčet z následujících možností:

- **Data** &ndash; pro použití při zobrazení datových položek. Všimněte si, že [ListView](~/xamarin-forms/user-interface/listview/index.md) , může být lepší možností pro posouvání seznamy data.
- **Formulář** &ndash; k použití pro zobrazení Tabulka funguje jako formulář.
- **Nabídka** &ndash; pro použití při zobrazení nabídky možností.
- **Nastavení** &ndash; pro použití při zobrazování seznamu nastavení konfigurace.

`TableIntent` Zvolíte, může mít vliv na způsob, jakým `TableView` se zobrazí na jednotlivých platformách. I když nejsou, nerušte zaškrtnutí políčka rozdíly, je osvědčeným postupem je vybrat `TableIntent` , která nejlépe odpovídá způsobu použití v tabulce.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Integrované buňky

Xamarin.Forms se dodává s integrovanou buňky pro shromažďování a zobrazování informací. I když ListView a zobrazení Tabulka umožňují používat všechny funkce stejných buňkách, tady jsou nejrelevantnější pro zobrazení Tabulka scénář:

- **SwitchCell** &ndash; pro prezentaci a zaznamenávání stavu true nebo false, spolu s textový popisek.
- **EntryCell** &ndash; pro prezentaci a zachycení textu.

Zobrazit [vzhled buňky ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) podrobný popis [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) a [funkce ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) ovládací prvek určený pro prezentaci a zaznamenávání zapnuto/vypnuto nebo `true` / `false` stavu.

SwitchCells mít jeden řádek textu k úpravě a vlastnost zapnout nebo vypnout. Obě tyto vlastnosti jsou s možností vazby.

- `Text` &ndash; text zobrazený vedle přepínač.
- `On` &ndash; Určuje, zda přepínač je zobrazena jako zapnuto nebo vypnuto.

Všimněte si, že `SwitchCell` zpřístupňuje `OnChanged` událost, abyste mohli reagovat na změny stavu buňky.

![](tableview-images/switch-cell.png "Příklad SwitchCell")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](xref:Xamarin.Forms.EntryCell) je užitečné, když budete chtít zobrazit textová data, která uživatel může upravovat. `EntryCell`s nabídky, které můžete přizpůsobit následující vlastnosti:

- `Keyboard` &ndash; Klávesnice pro zobrazení při úpravách. Existují možnosti pro takové věci, jako jsou číselné hodnoty, e-mailu, telefonních čísel atd. [Viz dokumentace rozhraní API](xref:Xamarin.Forms.Keyboard).
- `Label` &ndash; Text popisku zobrazíte napravo od polem pro zadání textu.
- `LabelColor` &ndash; Barva textu popisku.
- `Placeholder` &ndash; Text, který se zobrazí v poli položky, když má hodnotu null nebo prázdný. Tento text zmizí při zahájení zadání textu.
- `Text` &ndash; Text v polem pro zadání.
- `HorizontalTextAlignment` &ndash; Vodorovné zarovnání textu. Může být center vlevo nebo vpravo zarovnaný. [Viz dokumentace rozhraní API](xref:Xamarin.Forms.TextAlignment).

Všimněte si, že `EntryCell` zpřístupňuje `Completed` událost, která se aktivuje, když uživatel stiskne "Hotovo" na klávesnici během úprav textu.

![](tableview-images/entry-cell.png "Příklad EntryCell")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Vlastní buňky
Pokud integrované buňky nestačí, vlastní buňky je možné k prezentaci a sbírat data způsobem, který dává smysl pro vaši aplikaci. Můžete například k dispozici posuvník umožňující uživateli vybrat míra průhlednosti obrázku.

Všechny vlastní buňky musí být odvozen od [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), je stejná základní třída, že všechny buňky vestavěné typy použití.

Toto je příklad vlastního buňky:

![](tableview-images/custom-cell.png "Příklad vlastní buňky")

### <a name="xaml"></a>XAML
XAML vytvořit výše rozložení je nižší než:

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

Výše uvedené XAML je to mnohem. To Pojďme rozdělit:

- Kořenový element v rámci `TableView` je `TableRoot`.
- Je `TableSection` bezprostředně pod `TableRoot`.
- `ViewCell` Definovaný přímo v oddíl tabulky. Na rozdíl od `ListView`, `TableView` nevyžaduje, aby tuto vlastní (nebo všechny) buňky jsou definovány v `ItemTemplate`.
- StackLayout slouží ke správě rozložení vlastní buňky. Tady můžete použít libovolného rozložení.

### <a name="cnum"></a>C&num;

Protože `TableView` funguje s statická data nebo data, která se ručně změní, nemá koncept šablony položky. Místo toho vlastní buňky lze ručně vytvořit a vložit do tabulky. Všimněte si, že postup vytvoření vlastní buňky, která dědí z `ViewCell`, pak jejím přidáním na `TableView` jako jste vy byste integrované buňky, je také podporována.
Tady je kód jazyka c# k dosažení vyšší rozložení:

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

C# výše je to mnohem. To Pojďme rozdělit:

- Kořenový element v rámci `TableView` je `TableRoot`.
- Je `TableSection` bezprostředně pod `TableRoot`.
- `ViewCell` Definovaný přímo v oddíl tabulky. Na rozdíl od `ListView`, `TableView` nevyžaduje, aby tuto vlastní (nebo všechny) buňky jsou definovány v `ItemTemplate`.
- StackLayout slouží ke správě rozložení vlastní buňky. Tady můžete použít libovolného rozložení.

Všimněte si, že třída pro buňku vlastní nikdy definovaný. Místo toho `ViewCell`jeho zobrazení je nastavena pro konkrétní instanci `ViewCell`.



## <a name="related-links"></a>Související odkazy

- [Zobrazení Tabulka (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
