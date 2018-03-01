---
title: "Výška řádku automatickou"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c5deb294aac679d60535f3f3bd6c9745e8bff358
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="auto-sizing-row-height"></a>Výška řádku automatickou

Od verze iOS 8, Apple přidat schopnost vytvářet zobrazení tabulky (`UITableView`), automaticky zvýšit nebo snížit výšku daného řádku založenou na velikosti jeho obsah pomocí automatického rozložení, velikost třídy a omezení.

iOS 11 přidala možnost pro řádky automaticky rozšiřovat. Záhlaví, zápatí a buněk velikost lze nyní automaticky přizpůsobit na základě jejich obsahu. Ale pokud tabulka je vytvořen v iOS Designer rozhraní tvůrce, nebo pokud se má pevnou výšku řádků je nutné ručně povolit vlastní Změna velikosti buněk, jak je popsáno v této příručce.

## <a name="cell-layout-in-the-ios-designer"></a>Rozložení buněk v iOS návrháře

Otevřete scénáře pro zobrazení tabulky, že budete chtít mít automaticky velikosti řádku pro v iOS Designer, vyberte z buňky *prototypu* a navrhněte rozložení buňky. Příklad:

[ ![](autosizing-row-height-images/table01.png "Návrh prototypu buňky")](autosizing-row-height-images/table01.png)

Pro každý prvek v prototyp přidáte omezení zachovat elementy ve správné pozici při změně velikosti zobrazení tabulky pro otočení nebo jiné iOS velikost obrazovky zařízení. Například Připnutí `Title` nahoře vlevo a vpravo od buňky *zobrazení obsahu*:

[ ![](autosizing-row-height-images/table02.png "Připnutí nadpis v horní, vlevo a vpravo od zobrazení obsahu buněk")](autosizing-row-height-images/table02.png)

V případě naší příklad tabulky malým `Label` (v části `Title`) je pole, které můžete zmenšit a dosáhnout zvýšení nebo snížení výšku řádku. K dosažení tohoto efektu, přidejte následující omezení připnete vlevo, vpravo, horní a dolní popisku:

[ ![](autosizing-row-height-images/table03.png "Těchto omezení pro Připnutí vlevo, vpravo, horní a dolní popisku")](autosizing-row-height-images/table03.png)

Teď, když jsme plně mít omezené elementů v buňky, musíme vysvětlení by měl být roztažen tak, který element. Chcete-li to provést, nastavte **obsahu s prioritou Tvá objetí** a **obsahu s prioritou komprese odporu** podle potřeby v **rozložení** části panelu Vlastnosti pro:

[ ![](autosizing-row-height-images/table03a.png "Části panelu Vlastnosti pro rozložení")](autosizing-row-height-images/table03a.png)

Nastavit elementu, který chcete rozšířit tak, aby měl **nižší** hodnota Tvá objetí Priority a **nižší** hodnota Priority odporu komprese.

V dalším kroku musíme vyberte buňku prototypu a pojmenujte ho jedinečný **identifikátor**:

[ ![](autosizing-row-height-images/table04.png "Poskytnutí prototypu buňky jedinečný identifikátor")](autosizing-row-height-images/table04.png)

V našem příkladu případě `GrowCell`. Tato hodnota jsme budete používat později při naplníme v tabulce.

> [!IMPORTANT]
> **Poznámka:** Pokud tabulka obsahuje více než jeden typ buňky (**prototypu**), je potřeba zajistit, každý typ má svůj vlastní jedinečný `Identifier` pro změnu velikosti řádku automaticky pracovat.

Pro každý prvek naše buňky prototypu přiřadit **název** vystavit kódu C#. Příklad:

[ ![](autosizing-row-height-images/table05.png "Přiřadit název, který umístěte do kódu jazyka C#")](autosizing-row-height-images/table05.png)

V dalším kroku přidejte vlastní třídu pro `UITableViewController`, `UITableView` a `UITableCell` (prototyp). Příklad: 

[ ![](autosizing-row-height-images/table06.png "Přidání vlastní třídy UITableViewController, UITableView a UITableCell")](autosizing-row-height-images/table06.png)

Nakonec pokud chcete mít jistotu, že všechny očekávaný obsah se zobrazí v našem popisek, nastavte **řádky** vlastnost `0`:

[ ![](autosizing-row-height-images/table06.png "Vlastnost nastavena na hodnotu 0 na řádky")](autosizing-row-height-images/table06a.png)

Pomocí uživatelského rozhraní definované přidejme kód umožňující změnu velikosti výšku řádku automaticky.

##<a name="enabling-auto-resizing-height"></a>Povolení výška automatickou změnu velikosti

Buď naše tabulka zobrazení zdroje dat (`UITableViewDatasource`) nebo zdroje (`UITableViewSource`), když jsme dequeue – buňku je potřeba použít `Identifier` , definovaného v návrháři. Příklad:

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

Tabulka zobrazení se ve výchozím nastavení pro automatickou změnu velikosti výšku řádku. To, aby `RowHeight` vlastnost by měla být nastavená na `UITableView.AutomaticDimension`. Také je potřeba nastavit `EstimatedRowHeight` vlastnost v našem `UITableViewController`. Příklad:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Tento odhad nemusí být přesné, právě odhad průměrná výška každý řádek v tabulce zobrazení.

S tímto kódem na místě když se aplikace spustí, každý řádek se zmenšit a růst v závislosti na výšku poslední popisek v buňce prototypu. Příklad:

[ ![](autosizing-row-height-images/table07.png "Spuštění ukázkové tabulky")](autosizing-row-height-images/table07.png)


## <a name="related-links"></a>Související odkazy

- [GrowRowTable (ukázka)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
