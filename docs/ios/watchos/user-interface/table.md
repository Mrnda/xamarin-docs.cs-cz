---
title: "Ovládací prvek tabulky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4887b9a42c5a855353b5a4e422559aafcdc68173
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="table-control"></a>Ovládací prvek tabulky

WatchOS `WKInterfaceTable` ovládací prvek je mnohem jednodušší než jeho protějšku iOS, ale provádí podobnou roli. Vytvoří seznam řádků, který může mít vlastní rozložení a které reagují na touch události posouvání.

![](table-images/table-list-sml.png "Seznam sledování tabulky") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Přidání tabulky

Přetáhněte **tabulky** ovládacího prvku do scény. Ve výchozím nastavení bude vypadat jako tento (zobrazující rozložení neurčené jeden řádek):

[ ![](table-images/add-table-sml.png "Přidání tabulky")](table-images/add-table.png)

Zadejte název tabulky **vlastnosti** pad na **název** pole, takže může být uvedené v kódu.

## <a name="add-a-row-controller"></a>Přidat řadič řádek

Tabulka automaticky obsahuje jeden řádek, reprezentována řadič řádek, který obsahuje **skupiny** ovládacího prvku ve výchozím nastavení.

Nastavit **třída** pro řadič řádek, vyberte řádek v **Osnova dokumentu** a zadejte název třídy v **vlastnosti** odsazení:

[ ![](table-images/add-row-controller-sml.png "V poli vlastnosti pro zadávání názvu – třída")](table-images/add-row-controller.png)

Po nastavení třídy pro řádku řadič IDE vytvoří odpovídající soubor C# v projektu. Přetáhněte ovládací prvky (například popisky) na řádku a přiřaďte jim názvy, takže se může být uvedené v kódu.




## <a name="create-and-populate-rows"></a>Vytvořit a naplnit řádků

`SetNumberOfRows` vytvoří třídy controller řádek pro každý řádek, pomocí `Identifier` vybrat tu správnou. Pokud dáte řadiči řádek vlastní `Identifier`, změňte **výchozí** ve fragmentu kódu níže na identifikátor, který jste použili. `RowController` *Pro každý řádek* se vytvoří při `SetNumberOfRows` nazývá a zobrazí tabulku.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> **Poznámka:**: řádky tabulky nejsou Virtualizovat, jako jsou v iOS. Pokuste se omezit počet řádků (Apple doporučuje méně než 20).
Po vytvoření řádky potřebujete k naplnění jednotlivých buněk (jako je `GetCell` by se v iOS). Tento fragment kódu z [WatchTables příklad](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) aktualizuje popisek v jednotlivých řádcích

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> **Poznámka:** pomocí `SetNumberOfRows` a potom ve smyčce přes pomocí `GetRowController` způsobí, že k odeslání do hodinek celou tabulku. Na další zobrazení tabulky, pokud potřebujete přidat nebo odebrat pomocí konkrétní řádky `InsertRowsAt` a `RemoveRowsAt` pro dosažení vyššího výkonu.


## <a name="respond-to-taps"></a>Reakce na odposlouchávání

Reagovat na výběr řádku dvěma různými způsoby:

- implementace `DidSelectRow` metoda v rozhraní řadiči, nebo
- Vytvořit segue ve scénáři a implementovat `GetContextForSegue` Pokud chcete, aby výběr řádku otevřete jiné scény.

### <a name="didselectrow"></a>DidSelectRow

Prostřednictvím kódu programu zpracování výběr řádků, implementovat `DidSelectRow` metoda. Chcete-li otevřít nové scény, použijte `PushController` a předejte scény identifikátor a kontext dat, který má použít:

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

Přetáhněte segue ve scénáři z vaší řádku tabulky do jiné scény (podržte stisknutou **řízení** klíče při přetahování).
Nezapomeňte vybrat segue a přidělte mu identifikátor. **vlastnosti** pad (například `secondLevel` v následujícím příkladu).

V kontroleru rozhraní implementovat `GetContextForSegue` metoda a vraťte se kontextu dat, který by měl být zadán do scény, který je předkládán segue.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Tato data jsou předána scény storyboard cíl v jeho `Awake` metoda.

## <a name="multiple-row-types"></a>Více typů řádků

Ve výchozím nastavení ovládacího prvku tabulka má typ jeden řádek, který můžete navrhnout. Chcete-li přidat další řádek, šablony, použijte **řádky** pole **vlastnosti** pad vytvoření další řádek řadičů:

![](table-images/prototype-rows1.png "Nastavení počtu řádků prototypu")

Nastavení **řádky** vlastnost **3** vytvoří další řádek zástupné symboly pro vás přetáhněte ovládací prvky do. Pro každý řádek nastavit **třída** v **vlastnosti** odsazení a ověřte třídy kontroleru řádek je vytvořen.

![](table-images/prototype-rows2.png "Prototyp řádky v Návrháři")

Vyplnění tabulky s použitím typů jiný řádek `SetRowTypes` metoda zadat typ řadiče řádek, který se má použít pro každý řádek v tabulce. Použijte identifikátory řádku k určení řadič řádek, který má být použit pro každý řádek.

Počet elementů v toto pole by měl odpovídat počet řádků, které chcete být v tabulce:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Při naplňování tabulky s více řadiči řádek, budete potřebovat k udržování přehledu o jaký typ očekáváte, že jako naplnit uživatelského rozhraní:

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>Svislé podrobností stránkování

watchOS 3 zavedla nová funkce pro tabulky: možnost Procházet stránky podrobností související na každý řádek bez nutnosti přejít zpět k tabulce a zvolte jiný řádek. K načtení nahoru a dolů, nebo pomocí digitální Crown posunout obrazovky podrobností.

![](table-images/table-scroll-sml.png "Příklad svislé podrobností stránkování") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> **Upozornění:** tato funkce je aktuálně k dispozici pouze úpravou storyboard v Xcode rozhraní tvůrce.

Chcete-li povolit tuto funkci, vyberte `WKInterfaceTable` na návrhovou plochu a značek **svislé stránkování podrobností** možnost:

![](table-images/vertical-detail-paging-sml.png "Výběrem možnosti svislé stránkování podrobností")

Jako [vysvětlené Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) navigační tabulka musí používat segues pro funkci stránkování pracovat. Znovu zapsat existující kód, který používá `PushController` používat segues místo.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Dodatek: Příklad kódu řadiče řádek

Prostředí IDE automaticky vytvoří dva soubory kódu při vytvoření řadič řádek v návrháři. Kód na tyto soubory generované jsou uvedené dole pro referenci.

První bude mít název pro třídu, například **RowController.cs**, podobné výjimky:

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

Druhá **. designer.cs** soubor je definicí třídu, která obsahuje výstupy a akce, které jsou vytvořené na plochu návrháře, jako je například v tomto příkladu s jedním `WKInterfaceLabel` ovládacího prvku:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

Výstupy a akcí, které jsou deklarované v tomto poli může být odkazováno pak v kód – ale **. designer.cs** soubor není lze upravit přímo.



## <a name="related-links"></a>Související odkazy

- [WatchTables (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Tabulka dokumentace společnosti Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable)
