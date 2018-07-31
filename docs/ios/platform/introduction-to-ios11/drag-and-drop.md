---
title: Přetáhnout myší v Xamarin.iosu
description: Tento dokument popisuje, jak implementovat přetáhněte a umístěte do aplikace Xamarin.iOS pomocí rozhraní API zavedené v Iosu 11. Především se zabývá povolení přetažení v UITableView.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2017
ms.openlocfilehash: bc58c866a4a754bccea8d851f79e73fe5a415eed
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351132"
---
# <a name="drag-and-drop-in-xamarinios"></a>Přetáhnout myší v Xamarin.iosu

_Implementace přetažení pro iOS 11_

iOS 11 zahrnuje přetažení myší podporu pro kopírování dat mezi aplikacemi na Ipadu. Uživatelé můžou vyberte a přetáhněte všechny typy obsahu z aplikace umístěné na straně sebe nebo přetažením přes ikonu aplikace, která se aktivuje aplikaci otevřít a data, která mají vyřadit, povolit:

![Příklad přetažení z vlastních aplikací do poznámky aplikace](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Přetažení je k dispozici pouze v rámci stejné aplikace na zařízeních iPhone.

Zvažte podporu přetáhněte a umístěte operace kdekoli obsah můžou vytvořit nebo upravit:

- Textové ovládací prvky přetažení podporu všech aplikací založených na iOS 11, bez jakékoli další práce.
- Zobrazení tabulek a zobrazení kolekce zahrnují vylepšení v Iosu 11, která zjednodušuje přidávání přetáhněte a umístěte chování.
- Všech ostatních zobrazeních provádět podporují přetáhněte a umístěte pomocí dalších úprav.

Při přidávání přetažení podporu pro vaše aplikace, můžete poskytovat různé úrovně přesnosti obsahu; Například můžete zadat formátovaný text a verze ve formátu prostého textu dat, aby mohli vybrat přijímající aplikace, které je nejvhodnější do cíl přetažení. Je také možné přizpůsobení vizualizace přetažením a také povolit přetahování více položek najednou.

## <a name="drag-and-drop-with-text-controls"></a>Přetáhnout myší s textových ovládacích prvků

`UITextView` a `UITextField` automaticky podporují vybraný text navýšení kapacity, přetahování textu v obsahu.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Přetáhnout myší s UITableView

`UITableView` má integrovanou zpracování pro přetažení interakce s řádky tabulky, které vyžadují pouze několik metod, chcete-li povolit chování výchozí.

Zahrnuté jsou dvě rozhraní:

- `IUITableViewDragDelegate` – Balíčky informace při přetáhněte je zahájeno v tabulkovém zobrazení.
- `IUITableViewDropDelegate` – Procesy informace, když se pokles Probíhá pokus o a dokončeny.

V [DragAndDropTableView ukázka](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) obě tato dvě rozhraní jsou implementovány v `UITableViewController` třídě spolu se zdroji delegáta a data. Jsou v přiřazení `ViewDidLoad` metody:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Minimální požadovaný kód pro tato dvě rozhraní je popsán níže.

### <a name="table-view-drag-delegate"></a>Delegát přetáhněte zobrazení tabulky

Jedinou metodou _požadované_ pro podporu přetažením řádek z tabulky zobrazení je `GetItemsForBeginningDragSession`. Pokud uživatel spustí přetáhněte řádek, tato metoda bude volána.

Implementace je uveden níže. Načte data přidružená k řádku Přetahované, kóduje ho a nakonfiguruje `NSItemProvider` který určuje, jak aplikace bude zpracovávat "rozevírací" součástí operace (například, jestli se dokáže zpracovat datový typ `PlainText`, v příkladu):

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

Způsoby mnoho volitelné na přetáhněte delegát, který je možné implementovat přizpůsobení přetáhněte chování, jako je například poskytuje více reprezentací dat, které je možné provést výhod cílové aplikace (jako je například formátovaný text jako prostý text nebo vektoru a rastrový obrázek verze kresby). Můžete také zadat vlastní data reprezentace má být použit při přetahování a vkládání ve stejné aplikaci.

### <a name="table-view-drop-delegate"></a>Delegát rozevírací zobrazení tabulky

Metody delegáta rozevírací jsou volány při operaci přetažení probíhá přes zobrazení tabulky, nebo dokončí nad ním. Požadované metody určit, zda data smí vyřadit, a jaké akce pocházejí po dokončení rozevírací:

- `CanHandleDropSession` – Když probíhá přetáhněte a potenciálně probíhá vyřazování na aplikaci, tato metoda určuje, jestli data jsou kvůli usnadnění použití vypsány smí vyřadit.
- `DropSessionDidUpdate` – Když probíhá přetažení, tato metoda je volána k určení, jaké akce je určen. Všechny informace ze zobrazení tabulky se přesune, přetáhněte relace a možné index cestu lze použít k určení chování a vizuální zpětnou vazbu k dispozici pro uživatele.
- `PerformDrop` – Pokud uživatel dokončí rozevírací (přenesením svých prstem), tato metoda extrahuje data jsou kvůli usnadnění použití vypsány a upravuje zobrazení tabulky můžete přidat data do nového řádku (nebo řádků).

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Určuje, zda zobrazení tabulky může přijmout data právě přetáhli. V tomto fragmentu kódu `CanLoadObjects` se používá pro potvrzení, že toto zobrazení tabulky může přijmout data řetězce.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` Metoda je volána opakovaně během operace přetažení, poskytnout uživateli vizuální prvky.

V níže uvedený kód `HasActiveDrag` slouží k určení, zda operace vytvoří se v aktuálním zobrazení tabulky. Pokud ano, jsou povoleny pouze jednoho sloupce přesunout.
Při přetažení z jiného zdroje se zobrazuje operace kopírování:

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

Záměr rozevírací lze vložit nový řádek, nebo přidat/připojovat data do stávající řádek.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` Metoda se volá, když uživatel dokončí operaci a upraví zdroje, tak, aby odrážely vynechaných dat zobrazení a data tabulky.

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

Další kód může být přidán do asynchronní načítání velkých datových objektů.

### <a name="testing-drag-and-drop"></a>Testovací operace přetažení

IPad musí používat k testování [ukázka](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Otevřete ukázkový společně s jinou aplikací (jako jsou poznámky) a přetáhněte řádků a text mezi nimi:

![snímek obrazovky probíhá operace přetažení](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Související odkazy

- [Přetažení, lidských Interface Guidelines (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Přetáhnout myší příklad zobrazení tabulky](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Přetáhnout myší ukázkové zobrazení kolekce](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Představujeme přetáhněte a umístěte (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Přetáhnout myší kolekci a zobrazení tabulky (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/223/)
