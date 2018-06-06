---
title: Přetáhnout myší v Xamarin.iOS
description: Tento dokument popisuje, jak implementovat přetažení a vyřadit v aplikace Xamarin.iOS pomocí rozhraní API byla zavedená v iOS 11. Konkrétně popisuje povolení přetažení v UITableView.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: 7c41f96dae88047e64ec1e74838e3efab55958cc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786961"
---
# <a name="drag-and-drop-in-xamarinios"></a>Přetáhnout myší v Xamarin.iOS

_Implementace přetažení pro iOS 11_

iOS 11 zahrnuje přetažení a podpora ke kopírování dat mezi aplikacemi na iPad vyřadit. Uživatele můžete vybrat a přetáhněte všechny typy obsahu z aplikací umístěný-souběžného nebo přetažením přes ikonu pro aplikace, které budou aktivovat aplikaci otevřete a aby bylo možné data chcete vyřadit:

![Příklad přetažení z vlastní aplikace do aplikace poznámky](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Přetažení je k dispozici pouze v rámci stejné aplikace na zařízení iPhone.

Vezměte v úvahu podpora přetažení a operací myší kdekoli obsahu můžou vytvořit nebo upravit:

- Ovládacích prvků textu podpora přetažení pro všechny aplikace vytvořené s iOS 11, bez další zátěže.
- Zobrazení tabulek a zobrazení kolekce je vylepšení v iOS 11, která přidává přetažení zjednodušit a vyřadit chování.
- Ostatní zobrazení můžete provést pro podporu přetažení a vyřadit pomocí dalších úprav.

Při přidávání přetažení podporovat pro vaše aplikace, můžete poskytovat různé úrovně obsahu věrnosti; Například můžete zadat formátovaný text i prostý text verzi dat tak, aby přijímající aplikace můžete vybrat, který nejlépe vyhovuje do cílové přetažení. Je také možné přizpůsobit vizualizace přetažení a také povolit přetahování více položek najednou.

## <a name="drag-and-drop-with-text-controls"></a>Přetáhnout myší pomocí ovládacích prvků textu

`UITextView` a `UITextField` automaticky podporují přetahování vybraný text out a vkládání textu v obsahu.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Přetáhnout myší s UITableView

`UITableView` má integrovanou zpracování pro přetažení interakce s řádky tabulky, které vyžadují jenom několik metod povolit výchozí chování.

Existují dvě rozhraní související se situací:

- `IUITableViewDragDelegate` – Balíčky informace při zahájení přetáhněte v tabulkovém zobrazení.
- `IUITableViewDropDelegate` – Zpracovává informace, když se pokles pokus a dokončit.

V [DragAndDropTableView ukázka](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) obě tyto dvě rozhraní jsou implementované v `UITableViewController` třída, společně s zdroji delegáta a data. Je přiřazený v `ViewDidLoad` metoda:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Minimální požadovaná kód pro tato dvě rozhraní je popsáno níže.

### <a name="table-view-drag-delegate"></a>Tabulka zobrazení přetažení delegáta

Jedinou metodou _požadované_ pro podporu přetahování řádek ze zobrazení tabulky je `GetItemsForBeginningDragSession`. Pokud uživatel spustí přetáhněte řádek, bude tato metoda volána.

Implementace je uveden níže. Načte data související s taženou řádek, kóduje ho a nakonfiguruje `NSItemProvider` který určuje, jak bude aplikace zpracovávat část "rozevírací" operace (například, zda se může zpracovat datového typu, `PlainText`, v příkladu):

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

Existuje mnoho volitelné metod na přetáhněte delegáta, který se dají implementovat přizpůsobení chování přetažení, jako je například poskytuje více reprezentace dat, které můžete provést výhod cílové aplikace (například formátovaný text jako také jako prostý text nebo vektor a rastrový obrázek verze výkresu). Můžete také zadat reprezentace vlastních dat má být použit při přetahování a vkládání v rámci stejné aplikaci.

### <a name="table-view-drop-delegate"></a>Delegát vyřaďte zobrazení tabulky

V rozevírací delegát metody jsou volány při operaci přetažení probíhá přes zobrazení tabulky nebo dokončení nad ním. Požadované metody určují, jestli je dovoleno data vyřadit a jaké akce jsou provedena v případě, že dokončení rozevíracího:

- `CanHandleDropSession` – Když probíhá přetažení a potenciálně probíhá vyřazování na aplikaci, tato metoda určuje, zda data přetažen povolena chcete vyřadit.
- `DropSessionDidUpdate` – Když probíhá přetahování, tato metoda je volána k určení, jaké akce slouží. Všechny informace z tabulky zobrazení přetažen přes, přetáhněte relace a cestu indexů můžete použít k určení chování a vizuální zpětnou vazbu uživateli poskytnutý.
- `PerformDrop` – Pokud uživatel dokončí rozevíracího (zrušení jejich prstem), tato metoda extrahuje data přetažen a upraví tabulka zobrazení přidat data v nový řádek (nebo řádky).

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Určuje, zda tabulka zobrazení můžete přijímat data přetažen. V tento fragment kódu `CanLoadObjects` se používá pro potvrzení, že tato tabulka zobrazení může přijmout data řetězce.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` Metoda je volána opakovaně probíhá operace přetahování, k poskytování vizuální upozornění uživatele.

V kódu níže `HasActiveDrag` slouží k určení, zda pochází operaci v aktuálním zobrazení tabulky. Pokud ano, je povoleno pouze jednoho sloupce přesunout.
Pokud přetažení z jiného zdroje, budou uvedené operace kopírování:

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Operace odstranění může být jedna z `Cancel`, `Move`, nebo `Copy`.

Vyřaďte záměr lze vložit nový řádek, nebo přidat/připojovat data do existující řádek.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` Metoda je volána, když uživatel dokončí operaci a upraví zdroje, tak, aby odrážela vynechaných dat zobrazení a data tabulky.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

Asynchronně načíst objekty velkých objemů dat lze přidat další kód.

### <a name="testing-drag-and-drop"></a>Testování – přetažení

IPad musíte použít k testování [ukázka](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Otevřete ukázku spolu s jinou aplikaci (například poznámky) a přetáhněte řádky a text mezi nimi:

![snímek obrazovky probíhá operace přetažení](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Související odkazy

- [Přetáhnout myší lidského Interface Guidelines (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Přetáhnout myší ukázková tabulka zobrazení](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Přetáhnout myší ukázkové zobrazení kolekce](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Představení přetažení a Drop (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Přetáhnout myší s kolekce a zobrazení tabulky (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/223/)
