---
title: Zobrazení kolekce
description: Zobrazení kolekce povolit obsah, který se má zobrazit pomocí libovolného rozložení. Umožňují snadno vytváření rozložení mřížky mimo pole a zároveň také podporuje vlastní rozložení.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 75ad331a265c14892f101b1aa7956d2cde3beec8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="collection-views"></a>Zobrazení kolekce

_Zobrazení kolekce povolit obsah, který se má zobrazit pomocí libovolného rozložení. Umožňují snadno vytváření rozložení mřížky mimo pole a zároveň také podporuje vlastní rozložení._

Zobrazení kolekce, k dispozici v `UICollectionView` třídy, jsou nový koncept iOS 6, která zavést nabízí více položek na obrazovce pomocí rozložení. Vzory pro poskytování dat `UICollectionView` vytvoření položky a interakci s tyto položky podle stejné delegování a data zdroje vzory pro běžně používané v iOS vývoj.

Zobrazení kolekce však pracovat subsystému rozložení, která je nezávislá `UICollectionView` sám sebe. Proto jednoduše poskytování rozložení můžete snadno změnit prezentaci zobrazení kolekce.

iOS poskytuje rozložení třídy s názvem `UICollectionViewFlowLayout` umožňuje na základě řádku rozložení například mřížky se vytvoří s žádné další kroky. Navíc vlastní rozložení můžete také vytvořit umožňujících všechny prezentace, které si lze představit.

## <a name="uicollectionview-basics"></a>Základy UICollectionView

`UICollectionView` Třída je tvořen tři různé položky:

-  **Buněk** – datové zobrazení pro každou položku
-  **Doplňující zobrazení** – řízené daty zobrazení přidružená k oddílu.
-  **Zobrazení decoration** – Non-zobrazení vytvořené rozložení řízených daty

## <a name="cells"></a>Buněk

Buňky jsou objekty, které představují jednu položku v sadě dat, která má problém v kolekci zobrazení. Každá buňka představuje instanci `UICollectionViewCell` třídy, která se skládá ze tří různých zobrazení, jak je znázorněno na obrázku níže:

 [![](uicollectionview-images/01-uicollectionviewcell.png "Každá buňka se skládá ze tří různých zobrazení, jak je vidět tady")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

`UICollectionViewCell` Třída má následující vlastnosti pro každou z těchto zobrazení:

-   `ContentView` – Toto zobrazení obsahuje obsah, který představuje buňky. Je vykreslen v nejhornější pořadí na obrazovce.
-   `SelectedBackgroundView` – Buněk mít integrovanou podporu pro výběr. Toto zobrazení se používá k vizuální označení, že je vybraný buňku. Je vykreslen pod `ContentView` při výběru buňky.
-   `BackgroundView` – Buněk lze také zobrazit pozadí, který je předkládán `BackgroundView` . Toto zobrazení se zobrazí pod `SelectedBackgroundView` .


Nastavením `ContentView` tak, aby byla menší než `BackgroundView` a `SelectedBackgroundView`, `BackgroundView` slouží k vizuálně rámce obsahu, než `SelectedBackgroundView` se zobrazí, když je vybrána buňka, jak je uvedeno níže:

 [![](uicollectionview-images/02-cells.png "Elementy jinou buňku")](uicollectionview-images/02-cells.png#lightbox)

V buňkách v výše uvedený snímek obrazovky jsou vytvořené pomocí dědění z `UICollectionViewCell` a nastavení `ContentView`, `SelectedBackgroundView` a `BackgroundView` vlastnosti, v uvedeném pořadí, jak je znázorněno v následujícím kódu:

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />


## <a name="supplementary-views"></a>Doplňující zobrazení

Zobrazení, která představují informace spojené s každou část jsou doplňující zobrazení `UICollectionView`. Stejně jako u buněk jsou doplňující zobrazení řízené daty. Kde buněk prezentovat položky data ze zdroje dat, doplňující zobrazení k dispozici část dat, například kategorie adresáře na poličce nebo genre hudby v knihovně hudby.

Například doplňující zobrazení může představovat hlavičku pro konkrétní část, jak je znázorněno na obrázku níže:

 [![](uicollectionview-images/02a-supplementary-view.png "Doplňující zobrazení používá se k předložení hlavičku pro konkrétní část, jak je vidět tady")](uicollectionview-images/02a-supplementary-view.png#lightbox)

Použití doplňující zobrazení, nejprve musí být zaregistrovaný v `ViewDidLoad` metoda:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Potom je nutné vrátit pomocí zobrazení `GetViewForSupplementaryElement`, vytvořené pomocí `DequeueReusableSupplementaryView`a dědí z `UICollectionReusableView`. Následující fragment kódu vytvoří SupplementaryView ukazuje výše uvedený snímek obrazovky:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Doplňující zobrazení jsou obecné víc než jenom záhlaví a zápatí stránky.
Tyto můžete umístit kamkoli v zobrazení kolekce a může skládat z jakékoli zobrazení, provedení jejich vzhled plně přizpůsobit.

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>Dekorace zobrazení

Jsou čistě visual zobrazení, které lze zobrazit v zobrazení decoration `UICollectionView`. Na rozdíl od buněk a doplňující zobrazení že nejsou řízené daty. Tyto jsou vždy vytvořeny v rámci podtřídami a rozložení a následně můžete změnit tak, jako obsah rozložení. Například Decoration zobrazení může představovat zobrazení pozadí se posouvá společně s obsahem v `UICollectionView`, jak je uvedeno níže:

 [![](uicollectionview-images/02c-decoration-view.png "Zobrazení decoration s červenou pozadí")](uicollectionview-images/02c-decoration-view.png#lightbox)

 Následující fragment kódu změní na pozadí na červený v ukázkách `CircleLayout` třídy:

 ```csharp
 public class MyDecorationView : UICollectionReusableView
    {
        [Export ("initWithFrame:")]
        public MyDecorationView (CGRect frame) : base (frame)
        {
            BackgroundColor = UIColor.Red;
        }
    }
 ```


## <a name="data-source"></a>Zdroj dat

Jako s dalšími částmi iOS, například `UITableView` a `MKMapView`, `UICollectionView` získává data ze *zdroj dat*, který je zveřejněný v Xamarin.iOS prostřednictvím **`UICollectionViewDataSource`** – třída. Tato třída zodpovídá za poskytování obsahu `UICollectionView` například:

-  **Buněk** – vrácená z `GetCell` metoda.
-  **Doplňující zobrazení** – vrácená z `GetViewForSupplementaryElement` metoda.
-  **Počet oddílů** – vrácená z `NumberOfSections` metoda. Výchozí hodnota je 1, pokud není implementováno.
-  **Počet položek za část** – vrácená z `GetItemsCount` metoda.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Pro usnadnění práce `UICollectionViewController` třída je k dispozici. To je automaticky nakonfigurován pro obě delegáta, která je popsána v další části, a zdroj dat pro jeho `UICollectionView` zobrazení.

Stejně jako u `UITableView`, `UICollectionView` třídu bude volat pouze zdrojem dat získat buněk pro položky, které jsou na obrazovce.
Buněk, které posouvat z obrazovky jsou umístěny do fronty pro opakované použití, jak ukazuje následující obrázek:

 [![](uicollectionview-images/03-cell-reuse.png "Buněk, které posouvat z obrazovky jsou umístěny v do fronty pro opakované použití, jak je vidět tady")](uicollectionview-images/03-cell-reuse.png#lightbox)

Opakované použití buněk je jednodušší s `UICollectionView` a `UITableView`. Už musíte vytvořit buňku přímo ve zdroji dat, pokud jeden není k dispozici ve frontě opakované použití a jak buněk jsou zaregistrována v systému. Pokud buňku není k dispozici při volání zrušte fronty buňky z fronty opakované použití, vytvoří se automaticky na základě typu nebo nib, který byl zaregistrován iOS.
Stejný postup je také k dispozici pro doplňující zobrazení.

Zvažte například následující kód, který registruje `AnimalCell` třídy:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Když `UICollectionView` musí buňku, protože jeho položka je na obrazovce `UICollectionView` volá zdrojem dat `GetCell` metoda. Podobně jako jak to funguje s UITableView, tato metoda je zodpovědná za konfiguraci buňku ze základní data, která bude `AnimalCell` třídy v tomto případě.

Následující kód ukazuje implementaci `GetCell` , který vrací `AnimalCell` instance:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

Volání `DequeReusableCell` kde buňky bude buď zrušte zařadit do fronty z fronty opakované použití, nebo pokud není k dispozici ve frontě a vytvoření závislosti na rozsahu typ registrovaný ve volání buňku `CollectionView.RegisterClassForCell`.

V takovém případě tím, že zaregistrujete `AnimalCell` třída, iOS se vytvoří nové `AnimalCell` interně a obnoví v něm při volání zrušte fronty buňku, po který je nakonfigurován s bitovou kopii obsažené ve třídě zvířat a vrátí k zobrazení na `UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>Delegát

`UICollectionView` Třída používá delegáta typu `UICollectionViewDelegate` podporuje interakci se obsah `UICollectionView`. To umožňuje řízení:

-  **Výběr buňky** – Určuje, zda je vybrána buňku.
-  **Buňka zvýraznění** – Určuje, pokud buňku je aktuálně probíhá změněny.
-  **Buňka nabídky** – nabídky zobrazí pro buňku v reakci na gesto dlouho stiskněte.


Stejně jako u zdroje dat, `UICollectionViewController` je ve výchozím nastavení jako delegáta pro nakonfigurované `UICollectionView`.

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>Zvýraznění buněk

Při stisknutí buňku, přechody buňky do zvýrazněná stavu, a není vybraná dokud uživatel zruší jejich prstem z buňky. To umožňuje dočasných změn v vzhled buňky předtím, než je ve skutečnosti vybraná. Při výběru, buňky `SelectedBackgroundView` se zobrazí. Následující obrázek znázorňuje zvýrazněná stavu těsně před dojde k výběru:

 [![](uicollectionview-images/04-cell-highlight.png "Tento obrázek ukazuje stav zvýrazněná těsně před dojde k výběru")](uicollectionview-images/04-cell-highlight.png#lightbox)

K implementaci zvýraznění, `ItemHighlighted` a `ItemUnhighlighted` metody `UICollectionViewDelegate` lze použít. Například následující kód použije žlutý pozadí `ContentView` při buňky zvýrazní a bílé pozadí při zrušení zvýrazněná, jak je znázorněno na obrázku výše:

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />


#### <a name="disabling-selection"></a>Zakázání výběr

Ve výchozím nastavení je povolen výběr `UICollectionView`. Chcete-li zakázat výběr, přepište `ShouldHighlightItem` a vrátí hodnotu false, jak je uvedeno níže:

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Pokud zvýraznění zakázáno, je proces výběru buňku také zakázána. Navíc k dispozici je také `ShouldSelectItem` metoda, která řídí výběr přímo, ale pokud `ShouldHighlightItem` je implementována a vrátí hodnotu false, `ShouldSelectItem` není volán.

 `ShouldSelectItem` Umožňuje výběr zapnout nebo vypnout na základě jednotlivých položek, když `ShouldHighlightItem` není implementována. Umožňuje také zvýraznění bez výběru, pokud `ShouldHighlightItem` je implementována a vrátí hodnotu true, při `ShouldSelectItem` vrací hodnotu false.

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>Buňky nabídky

Každá buňka `UICollectionView` umožňuje zobrazení nabídky, která umožňuje vyjmutí, kopírování a vložení volitelně podporovaná. K vytvoření nabídky Upravit na buňku:

1.  Přepsání `ShouldShowMenu` a vrátí hodnotu PRAVDA, pokud položka by měl zobrazit nabídku.
1.  Přepsání `CanPerformAction` a vrátí hodnotu PRAVDA pro všechny akce, které může provádět položka, která bude vyjmutí, kopírování nebo vložení.
1.  Přepsání `PerformAction` provést úpravy, kopírování operace vložení.


Následující snímek obrazovky zobrazit v nabídce při dlouho stisknutí buňku:

 [![](uicollectionview-images/04a-menu.png "Tento snímek obrazovky zobrazit v nabídce při dlouho stisknutí buňku")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />


## <a name="layout"></a>Rozložení

`UICollectionView` podporuje systém rozložení, který umožňuje umístění všech jejích elementů, buněk, doplňující zobrazení a zobrazení Decoration spravovat nezávisle `UICollectionView` sám sebe.
Pomocí systému rozložení, aplikace může podporovat rozložení, jako je třeba mřížky jsme jste viděli v tomto článku, zadejte vlastní rozložení.

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>Základní informace o rozložení

Rozložení v `UICollectionView` jsou definovány v třídu, která dědí z `UICollectionViewLayout`. Implementace rozložení zodpovídá za vytvoření atributy rozložení pro každou položku v `UICollectionView`. Existují dva způsoby, jak vytvořit rozložení:

-  Použijte předdefinovaný `UICollectionViewFlowLayout` .
-  Zadejte vlastní rozložení dědění ze `UICollectionViewLayout` .


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>Plovoucí rozložení

`UICollectionViewFlowLayout` Třída poskytuje to vhodný pro uspořádání obsahu v mřížce buněk, jak jste viděli na základě řádku rozložení.

Použití plovoucí rozložení:

-  Vytvoření instance `UICollectionViewFlowLayout` :


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  Předat konstruktoru instance `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

To je vše, který je potřebný pro rozložení obsahu v mřížce. Navíc pokud orientaci změní, `UICollectionViewFlowLayout` zpracovává Změna uspořádání obsahu odpovídajícím způsobem, jak je uvedeno níže:

 [![](uicollectionview-images/05-layout-orientation.png "Příklad změny orientace")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>Část Inset

K poskytování nějaké místo kolem `UIContentView`, rozložení mají `SectionInset` vlastnost typu `UIEdgeInsets`. Například následující kód poskytuje vyrovnávací paměť 50 pixelů kolem každá část `UIContentView` při nastíněny `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Výsledkem je mezery okolo části, jak je uvedeno níže:

 [![](uicollectionview-images/06-sectioninset.png "Jak je vidět tady mezer kolem oddílu")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>Vytvoření podtřídy UICollectionViewFlowLayout

V edici pomocí `UICollectionViewFlowLayout` přímo, ho můžete také být rozčlenění k dalšímu přizpůsobení rozložení obsahu podél řádku. Například může sloužit k vytvoření rozložení, který není zalomen buněk v mřížce, ale místo toho vytvoří jeden řádek s vodorovné posouvání platit, jak je uvedeno níže:

 [![](uicollectionview-images/07-line-layout.png "Jeden řádek s vodorovné efekt posouvání")](uicollectionview-images/07-line-layout.png#lightbox)

Chcete-li tuto funkci implementovat pomocí vytváření podtříd `UICollectionViewFlowLayout` vyžaduje:

-  Inicializuje všechny vlastnosti rozložení, které platí pro rozložení sám sebe nebo všechny položky v rozložení v konstruktoru.
-  Přepsání `ShouldInvalidateLayoutForBoundsChange` , vracejících hodnotu true, který po mezí `UICollectionView` bude přepočítat změny, rozložení buněk. Používá se v takovém případě zkontrolujte kód pro transformaci buňky centermost se použijí při posouvání.
-  Přepsání `TargetContentOffset` aby centermost buňka snap na střed `UICollectionView` jako posouvání zastaví.
-  Přepsání `LayoutAttributesForElementsInRect` vrátit pole `UICollectionViewLayoutAttributes` . Každý `UICollectionViewLayoutAttribute` obsahuje informace o tom, jak rozložení konkrétní položky, včetně vlastnosti, jako například jeho `Center` , `Size` , `ZIndex` a `Transform3D` .


Následující kód ukazuje takové implementace:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
    public class LineLayout : UICollectionViewFlowLayout
    {
        public const float ITEM_SIZE = 200.0f;
        public const int ACTIVE_DISTANCE = 200;
        public const float ZOOM_FACTOR = 0.3f;

        public LineLayout ()
        {
            ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
            ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
            MinimumLineSpacing = 50.0f;
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            return true;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

            foreach (var attributes in array) {
                if (attributes.Frame.IntersectsWith (rect)) {
                    float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
                    float normalizedDistance = distance / ACTIVE_DISTANCE;
                    if (Math.Abs (distance) < ACTIVE_DISTANCE) {
                        float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
                        attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
                        attributes.ZIndex = 1;
                    }
                }
            }
            return array;
        }

        public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
        {
            float offSetAdjustment = float.MaxValue;
            float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
            CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
            var array = base.LayoutAttributesForElementsInRect (targetRect);
            foreach (var layoutAttributes in array) {
                float itemHorizontalCenter = (float)layoutAttributes.Center.X;
                if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
                    offSetAdjustment = itemHorizontalCenter - horizontalCenter;
                }
            }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
        }

    }
}
```

 <a name="Custom_Layout" />


### <a name="custom-layout"></a>Vlastní rozložení

Kromě používání `UICollectionViewFlowLayout`, rozložení se dá plně přizpůsobit taky dědění přímo ze `UICollectionViewLayout`.

Klíče metody k přepsání jsou následující:

-   `PrepareLayout` – Používá k provedení počáteční geometrickou výpočty, které budou použity v celém procesu rozložení.
-   `CollectionViewContentSize` – Vrátí velikost oblasti slouží k zobrazení obsahu.
-   `LayoutAttributesForElementsInRect` – Jak se z UICollectionViewFlowLayout příkladu výše, tato metoda se používá k poskytují informace, které `UICollectionView` o tom, jak rozložení jednotlivých položek. Ale na rozdíl od `UICollectionViewFlowLayout` , při vytváření vlastní rozložení, ale zvolíte můžete umístit položky.


Například stejný obsah mohou být uvedeny v cyklické rozložení, jak je uvedeno níže:

 [![](uicollectionview-images/08-circle-layout.png "Cyklické vlastní rozložení, jak je vidět tady")](uicollectionview-images/08-circle-layout.png#lightbox)

Výkonné věc o rozložení je, že změna rozložení mřížky na vodorovném rozložení posouvání a následně do tohoto rozvržení cyklické vyžaduje pouze třída rozložení poskytnuté `UICollectionView` změnit. Nic v `UICollectionView`, jeho delegáta nebo zdroje dat změn kódu vůbec.


## <a name="changes-in-ios-9"></a>Změny v iOS 9


V systému iOS 9, zobrazení kolekce (`UICollectionView`) teď podporuje přetáhněte změny pořadí položek mimo pole přidáním nové rozpoznávání gesto výchozí a několik nových podpůrné metod.

Pomocí těchto nových metod, můžete snadno implementovat přetáhněte chcete změnit pořadí v zobrazení kolekce tak, aby možnost přizpůsobení vzhledu položky během jakékoli fázi způsob procesu.

[![](uicollectionview-images/intro01.png "Příklad způsob procesu")](uicollectionview-images/intro01.png#lightbox)

V tomto článku provedeme podívejte se na implementace změnit přetáhněte pořadí v aplikace pro Xamarin.iOS a také některé změny, provedena iOS 9 do ovládacího prvku zobrazení kolekce:

- [Snadno změny pořadí položek](#Easy-Reordering-of-Items)
    - [Jednoduchý způsob příklad](#Simple-Reordering-Example)
    - [Použití funkce rozpoznávání vlastní gesto](#Using-a-Custom-Gesture-Recognizer)
    - [Vlastní rozložení a změna pořadí](#Custom-Layouts-and-Reording)
- [Zobrazení změn v kolekci](#Collection-View-Changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Změny pořadí položek

Jak jsme uvedli výše, jednou z nejvýznamnějších změn zobrazení kolekce v iOS 9 se přidání snadno změnit přetáhněte pořadí funkcí mimo pole.

V systému iOS 9, je použití nejrychlejší způsob, jak přidat Změna pořadí zobrazení kolekce `UICollectionViewController`.
Teď má řadiče zobrazení kolekce `InstallsStandardGestureForInteractiveMovement` vlastností, která se přidá standardní *gesty při rozpoznávání rukopisu* podporující přetahování chcete změnit pořadí položek v kolekci.
Vzhledem k tomu, že výchozí hodnota je `true`, budete muset implementovat `MoveItem` metodu `UICollectionViewDataSource` třídy pro podporu změnit přetáhněte pořadí. Příklad:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Jednoduchý způsob příklad

Jako v příkladu rychlé spuštění nového projektu Xamarin.iOS a upravit **Main.storyboard** souboru. Přetažení `UICollectionViewController` na návrhovou plochu:

[![](uicollectionview-images/quick01.png "Přidání UICollectionViewController")](uicollectionview-images/quick01.png#lightbox)

Vyberte zobrazení kolekce (může být nejjednodušší provést ze Osnova dokumentu). Na kartě Rozložení panelu vlastností pro nastavení následující velikostí, jak ukazuje následující snímek obrazovky:

- **Buňka velikost**: šířka – 60 | Výška – 60
- **Velikost záhlaví**: šířka – 0 | Výška – 0
- **Velikost zápatí**: šířka – 0 | Výška – 0
- **Minimální mezera**: pro buněk – 8 | Řádky – 8
- **Část vsazení**: horní – 16 | Dolní – 16 | Vlevo – 16 | Pravé – 16

[![](uicollectionview-images/quick04.png "Nastavení velikosti zobrazení kolekce")](uicollectionview-images/quick04.png#lightbox)

Potom upravte výchozí buňky:
    - Změnit barvu pozadí na modrou
    - Přidání popisku tak, aby fungoval jako název buňky
    - Nastavit identifikátor opakované použití na **buňky**

[![](uicollectionview-images/quick02.png "Upravit výchozí buňky")](uicollectionview-images/quick02.png#lightbox)

Přidáte omezení zachovat popisek zarovnaný na střed uvnitř buňky změnách velikost:

V **Pad vlastnost** pro _CollectionViewCell_ a nastavte **třída** k `TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "Nastavit třídy na TextCollectionViewCell")](uicollectionview-images/quick05.png#lightbox)

Nastavte **opakovaně použitelné zobrazení kolekce** k `Cell`:

[![](uicollectionview-images/quick06.png "Nastavení zobrazení opakovaně použitelné kolekce do buňky")](uicollectionview-images/quick06.png#lightbox)

Nakonec vyberte štítek a pojmenujte ji `TextLabel`:

[![](uicollectionview-images/quick07.png "Popisek názvu TextLabel")](uicollectionview-images/quick07.png#lightbox)

Upravit `TextCollectionViewCell` třídu a přidejte následující vlastnosti.:

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
    public partial class TextCollectionViewCell : UICollectionViewCell
    {
        #region Computed Properties
        public string Title {
            get { return TextLabel.Text; }
            set { TextLabel.Text = value; }
        }
        #endregion

        #region Constructors
        public TextCollectionViewCell (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Zde `Text` popisku je vlastnost k dispozici v záhlaví buňky, takže je můžete nastavit z kódu.

Přidejte novou třídu C# do projektu a pojmenujte ji `WaterfallCollectionSource`. Upravte soubor a nastavit jej vypadat následovně:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionSource : UICollectionViewDataSource
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        public List<int> Numbers { get; set; } = new List<int> ();
        #endregion

        #region Constructors
        public WaterfallCollectionSource (WaterfallCollectionView collectionView)
        {
            // Initialize
            CollectionView = collectionView;

            // Init numbers collection
            for (int n = 0; n < 100; ++n) {
                Numbers.Add (n);
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView) {
            // We only have one section
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section) {
            // Return the number of items
            return Numbers.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a reusable cell and set {~~it's~>its~~} title from the item
            var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
            cell.Title = Numbers [(int)indexPath.Item].ToString();

            return cell;
        }

        public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // We can always move items
            return true;
        }

        public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
        {
            // Reorder our list of items
            var item = Numbers [(int)sourceIndexPath.Item];
            Numbers.RemoveAt ((int)sourceIndexPath.Item);
            Numbers.Insert ((int)destinationIndexPath.Item, item);
        }
        #endregion
    }
}
```

Tato třída bude sloužit jako zdroj dat pro naše zobrazení kolekce a zadejte informace pro každé pole v kolekci.
Všimněte si, že `MoveItem` povolit přetáhněte položky v kolekci být uspořádaný je implementována metoda.

Přidejte další novou C# třídu do projektu a pojmenujte ji `WaterfallCollectionDelegate`. Upravit tento soubor a nastavit jej vypadat následovně:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionDelegate : UICollectionViewDelegate
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        #endregion

        #region Constructors
        public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
        {

            // Initialize
            CollectionView = collectionView;

        }
        #endregion

        #region Overrides Methods
        public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // Always allow for highlighting
            return true;
        }

        public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and change to green background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
        }

        public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and return to blue background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
        }
        #endregion
    }
}
```

To bude fungovat jako delegáta pro naše zobrazení kolekce. Abyste měli na očích buňku jako uživatel pracuje s ním v zobrazení kolekce přepsána metody.

Přidejte jednu poslední C# třídu do projektu a pojmenujte ji `WaterfallCollectionView`. Upravit tento soubor a nastavit jej vypadat následovně:

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
    [Register("WaterfallCollectionView")]
    public class WaterfallCollectionView : UICollectionView
    {

        #region Constructors
        public WaterfallCollectionView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Initialize
            DataSource = new WaterfallCollectionSource(this);
            Delegate = new WaterfallCollectionDelegate(this);

        }
        #endregion
    }
}
```

Všimněte si, že `DataSource` a `Delegate` které jsme vytvořili výše jsou nastaveny při zobrazení kolekce se vytvářejí na základě jeho storyboard (nebo **.xib** souboru).

Upravit **Main.storyboard** znovu a vyberte zobrazení kolekce a přepnout **vlastnosti**. Nastavit **třída** na vlastní `WaterfallCollectionView` třídu, která jsme definovali výše:

Uložit změny provedené u uživatelského rozhraní a spuštění aplikace.
Pokud uživatel vybere položka ze seznamu a nastavuje tažením ho do nového umístění, bude automaticky animace další položky, jako se přesouvají z cesty položky.
Jakmile uživatel přesune položku do nového umístění, bude přilepit do tohoto umístění. Příklad:

[![](uicollectionview-images/intro01.png "Příklad přetažení položky do nového umístění")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>Použití funkce rozpoznávání vlastní gesto

V případech, kdy nelze používat `UICollectionViewController` a musí používat běžný `UIViewController`, nebo pokud budete chtít využívat větší kontrolu nad gesto přetahování myší, můžete vytvořit vlastní vlastní rozpoznávání rukopisu gesto a přidat do kolekce zobrazení při načtení zobrazení. Příklad:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Create a custom gesture recognizer
    var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

        // Take action based on state
        switch(gesture.State) {
        case UIGestureRecognizerState.Began:
            var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
            if (selectedIndexPath !=null) {
                CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
            }
            break;
        case UIGestureRecognizerState.Changed:
            CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
            break;
        case UIGestureRecognizerState.Ended:
            CollectionView.EndInteractiveMovement();
            break;
        default:
            CollectionView.CancelInteractiveMovement();
            break;
        }

    });

    // Add the custom recognizer to the collection view
    CollectionView.AddGestureRecognizer(longPressGesture);
}
```

K implementaci a řídit operaci přetažení se používá několik nových metod, které jsou přidány do zobrazení kolekce:

 - `BeginInteractiveMovementForItem` -Označuje začátek operaci přesunutí.
 - `UpdateInteractiveMovementTargetPosition` -Je odeslána jako umístění položky se aktualizuje.
 - `EndInteractiveMovement` -Označí přesunu položky.
 - `CancelInteractiveMovement` -Označí uživatele zrušení operaci přesunutí.

Když se aplikace spustí, operace tažení bude fungovat stejně, jako výchozí přetáhněte pro rozpoznávání gesta, která se dodává s zobrazení kolekce.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Vlastní rozložení a změna pořadí

V systému iOS 9 byly přidány několik nové metody pro práci s změnit přetáhněte pořadí a vlastní rozložení v zobrazení kolekce. Pokud chcete prozkoumat tuto funkci, přidejte umožňuje vlastní rozložení do kolekce.

Nejprve přidejte novou třídu C# s názvem `WaterfallCollectionLayout` do projektu. Upravit a nastavit jej vypadat následovně:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
    [Register("WaterfallCollectionLayout")]
    public class WaterfallCollectionLayout : UICollectionViewLayout
    {
        #region Private Variables
        private int columnCount = 2;
        private nfloat minimumColumnSpacing = 10;
        private nfloat minimumInterItemSpacing = 10;
        private nfloat headerHeight = 0.0f;
        private nfloat footerHeight = 0.0f;
        private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
        private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
        private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private List<CGRect> unionRects = new List<CGRect>();
        private List<nfloat> columnHeights = new List<nfloat>();
        private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
        private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
        private nfloat unionSize = 20;
        #endregion

        #region Computed Properties
        [Export("ColumnCount")]
        public int ColumnCount {
            get { return columnCount; }
            set {
                WillChangeValue ("ColumnCount");
                columnCount = value;
                DidChangeValue ("ColumnCount");

                InvalidateLayout ();
            }
        }

        [Export("MinimumColumnSpacing")]
        public nfloat MinimumColumnSpacing {
            get { return minimumColumnSpacing; }
            set {
                WillChangeValue ("MinimumColumnSpacing");
                minimumColumnSpacing = value;
                DidChangeValue ("MinimumColumnSpacing");

                InvalidateLayout ();
            }
        }

        [Export("MinimumInterItemSpacing")]
        public nfloat MinimumInterItemSpacing {
            get { return minimumInterItemSpacing; }
            set {
                WillChangeValue ("MinimumInterItemSpacing");
                minimumInterItemSpacing = value;
                DidChangeValue ("MinimumInterItemSpacing");

                InvalidateLayout ();
            }
        }

        [Export("HeaderHeight")]
        public nfloat HeaderHeight {
            get { return headerHeight; }
            set {
                WillChangeValue ("HeaderHeight");
                headerHeight = value;
                DidChangeValue ("HeaderHeight");

                InvalidateLayout ();
            }
        }

        [Export("FooterHeight")]
        public nfloat FooterHeight {
            get { return footerHeight; }
            set {
                WillChangeValue ("FooterHeight");
                footerHeight = value;
                DidChangeValue ("FooterHeight");

                InvalidateLayout ();
            }
        }

        [Export("SectionInset")]
        public UIEdgeInsets SectionInset {
            get { return sectionInset; }
            set {
                WillChangeValue ("SectionInset");
                sectionInset = value;
                DidChangeValue ("SectionInset");

                InvalidateLayout ();
            }
        }

        [Export("ItemRenderDirection")]
        public WaterfallCollectionRenderDirection ItemRenderDirection {
            get { return itemRenderDirection; }
            set {
                WillChangeValue ("ItemRenderDirection");
                itemRenderDirection = value;
                DidChangeValue ("ItemRenderDirection");

                InvalidateLayout ();
            }
        }
        #endregion

        #region Constructors
        public WaterfallCollectionLayout ()
        {
        }

        public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

        }
        #endregion

        #region Public Methods
        public nfloat ItemWidthInSectionAtIndex(int section) {

            var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
            return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
        }
        #endregion

        #region Override Methods
        public override void PrepareLayout ()
        {
            base.PrepareLayout ();

            // Get the number of sections
            var numberofSections = CollectionView.NumberOfSections();
            if (numberofSections == 0)
                return;

            // Reset collections
            headersAttributes.Clear ();
            footersAttributes.Clear ();
            unionRects.Clear ();
            columnHeights.Clear ();
            allItemAttributes.Clear ();
            sectionItemAttributes.Clear ();

            // Initialize column heights
            for (int n = 0; n < ColumnCount; n++) {
                columnHeights.Add ((nfloat)0);
            }

            // Process all sections
            nfloat top = 0.0f;
            var attributes = new UICollectionViewLayoutAttributes ();
            var columnIndex = 0;
            for (nint section = 0; section < numberofSections; ++section) {
                // Calculate section specific metrics
                var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
                    MinimumInterItemSpacingForSection (CollectionView, this, section);

                // Calculate widths
                var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
                var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

                // Calculate section header
                var heightHeader = (HeightForHeader == null) ? HeaderHeight :
                    HeightForHeader (CollectionView, this, section);

                if (heightHeader > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
                    headersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);

                    top = attributes.Frame.GetMaxY ();
                }

                top += SectionInset.Top;
                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }

                // Calculate Section Items
                var itemCount = CollectionView.NumberOfItemsInSection(section);
                List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

                for (nint n = 0; n < itemCount; n++) {
                    var indexPath = NSIndexPath.FromRowSection (n, section);
                    columnIndex = NextColumnIndexForItem (n);
                    var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
                    var yOffset = columnHeights [columnIndex];
                    var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
                    nfloat itemHeight = 0.0f;

                    if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
                        itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
                    }

                    attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
                    attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
                    itemAttributes.Add (attributes);
                    allItemAttributes.Add (attributes);
                    columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
                }
                sectionItemAttributes.Add (itemAttributes);

                // Calculate Section Footer
                nfloat footerHeight = 0.0f;
                columnIndex = LongestColumnIndex();
                top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
                footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

                if (footerHeight > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
                    footersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);
                    top = attributes.Frame.GetMaxY ();
                }

                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }
            }

            var i =0;
            var attrs = allItemAttributes.Count;
            while(i < attrs) {
                var rect1 = allItemAttributes [i].Frame;
                i = (int)Math.Min (i + unionSize, attrs) - 1;
                var rect2 = allItemAttributes [i].Frame;
                unionRects.Add (CGRect.Union (rect1, rect2));
                i++;
            }

        }

        public override CGSize CollectionViewContentSize {
            get {
                if (CollectionView.NumberOfSections () == 0) {
                    return new CGSize (0, 0);
                }

                var contentSize = CollectionView.Bounds.Size;
                contentSize.Height = columnHeights [0];
                return contentSize;
            }
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
        {
            if (indexPath.Section >= sectionItemAttributes.Count) {
                return null;
            }

            if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
                return null;
            }

            var list = sectionItemAttributes [indexPath.Section];
            return list [(int)indexPath.Item];
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
        {
            var attributes = new UICollectionViewLayoutAttributes ();

            switch (kind) {
            case "header":
                attributes = headersAttributes [indexPath.Section];
                break;
            case "footer":
                attributes = footersAttributes [indexPath.Section];
                break;
            }

            return attributes;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var begin = 0;
            var end = unionRects.Count;
            List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();


            for (int i = 0; i < end; i++) {
                if (rect.IntersectsWith(unionRects[i])) {
                    begin = i * (int)unionSize;
                }
            }

            for (int i = end - 1; i >= 0; i--) {
                if (rect.IntersectsWith (unionRects [i])) {
                    end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
                    break;
                }
            }

            for (int i = begin; i < end; i++) {
                var attr = allItemAttributes [i];
                if (rect.IntersectsWith (attr.Frame)) {
                    attrs.Add (attr);
                }
            }

            return attrs.ToArray();
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            var oldBounds = CollectionView.Bounds;
            return (newBounds.Width != oldBounds.Width);
        }
        #endregion

        #region Private Methods
        private int ShortestColumnIndex() {
            var index = 0;
            var shortestHeight = nfloat.MaxValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height < shortestHeight) {
                    shortestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int LongestColumnIndex() {
            var index = 0;
            var longestHeight = nfloat.MinValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height > longestHeight) {
                    longestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int NextColumnIndexForItem(nint item) {
            var index = 0;

            switch (ItemRenderDirection) {
            case WaterfallCollectionRenderDirection.ShortestFirst:
                index = ShortestColumnIndex ();
                break;
            case WaterfallCollectionRenderDirection.LeftToRight:
                index = ColumnCount;
                break;
            case WaterfallCollectionRenderDirection.RightToLeft:
                index = (ColumnCount - 1) - ((int)item / ColumnCount);
                break;
            }

            return index;
        }
        #endregion

        #region Events
        public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
        public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
        public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

        public event WaterfallCollectionSizeDelegate SizeForItem;
        public event WaterfallCollectionFloatDelegate HeightForHeader;
        public event WaterfallCollectionFloatDelegate HeightForFooter;
        public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
        public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
        #endregion
    }
}
```

To může být použitý k poskytnutí vlastního dva sloupce, vodopádu typ rozložení do kolekce zobrazení třídy.
Kód používá klíč-hodnota kódování (prostřednictvím `WillChangeValue` a `DidChangeValue` metody) má být poskytnuta vazba dat pro naše počítané vlastnosti v této třídě.

Potom upravte `WaterfallCollectionSource` a proveďte následující změny a dodatky:

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
    // Initialize
    CollectionView = collectionView;

    // Init numbers collection
    for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
        Heights.Add (rnd.Next (0, 100) + 40.0f);
    }
}
```

Tím se vytvoří náhodný výšku pro všechny položky, které se zobrazí v seznamu.

Potom upravte `WaterfallCollectionView` třídu a přidejte následující vlastnost pomocné rutiny:

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

To bude usnadňují získat v našem zdroje dat (a výšky položky) z vlastní rozložení.

Nakonec upravte řadiče zobrazení a přidejte následující kód:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    var waterfallLayout = new WaterfallCollectionLayout ();

    // Wireup events
    waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
        var collection = collectionView as WaterfallCollectionView;
        return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
    };

    // Attach the custom layout to the collection
    CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

To vytvoří instanci našeho vlastní rozložení, nastaví událost a poskytuje velikost každé položky a nové rozložení k naší zobrazení kolekce.

Pokud jsme spuštění aplikace Xamarin.iOS zobrazení kolekce bude nyní vypadat následovně:

[![](uicollectionview-images/custom01.png "Zobrazení kolekce bude nyní vypadat například takto")](uicollectionview-images/custom01.png#lightbox)

Můžeme stále přetáhněte na-změnit pořadí položek jako před, ale položky bude nyní změnit velikost tak, aby vyhovoval jejich nové umístění, když budou vynechána.

## <a name="collection-view-changes"></a>Zobrazení změn v kolekci

V následujících částech provedeme podrobné podívejte se na změny provedené iOS 9 pro různé třídy v zobrazení kolekce.

### <a name="uicollectionview"></a>UICollectionView

Následující změny nebo přidání byly provedeny `UICollectionView` třídu pro iOS 9:

 - `BeginInteractiveMovementForItem` – Označuje začátek operaci přetažení.
 - `CancelInteractiveMovement` – Informuje kolekce zobrazení, že uživatel zrušil operaci přetažení.
 - `EndInteractiveMovement` – Informuje kolekce zobrazení, že uživatel dokončí operaci přetažení.
 - `GetIndexPathsForVisibleSupplementaryElements` – Vrátí `indexPath` záhlaví nebo zápatí stránky v části zobrazení kolekce.
 - `GetSupplementaryView` – Vrátí daný záhlaví nebo zápatí stránky.
 - `GetVisibleSupplementaryViews` – Vrátí seznam všech viditelné záhlaví a zápatí stránky.
 - `UpdateInteractiveMovementTargetPosition` – Informuje kolekce zobrazení, aby uživatel přesunul nebo přechází položku během operace přetažení.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Následující změny nebo přidání byly provedeny `UICollectionViewController` třídy v iOS 9:

 - `InstallsStandardGestureForInteractiveMovement` – Pokud `true` se použije pro nové rozpoznávání gesto automaticky podporující přetahování chcete změnit pořadí.
 - `CanMoveItem` – Upozorní zobrazení kolekce, pokud daná položka může být přetáhněte přeuspořádány.
 - `GetTargetContentOffset` – Použije k získání posun položku zobrazení danou kolekci.
 - `GetTargetIndexPathForMove` – Získá `indexPath` u dané položky pro operaci přetažení.
 - `MoveItem` – Přesune pořadí danou položku v seznamu.


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Následující změny nebo přidání byly provedeny `UICollectionViewDataSource` třídy v iOS 9:

 - `CanMoveItem` – Upozorní zobrazení kolekce, pokud daná položka může být přetáhněte přeuspořádány.
 - `MoveItem` – Přesune pořadí danou položku v seznamu.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Následující změny nebo přidání byly provedeny `UICollectionViewDelegate` třídy v iOS 9:

 - `GetTargetContentOffset` – Použije k získání posun položku zobrazení danou kolekci.
 - `GetTargetIndexPathForMove` – Získá `indexPath` u dané položky pro operaci přetažení.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Následující změny nebo přidání byly provedeny `UICollectionViewFlowLayout` třídy v iOS 9:

 - `SectionFootersPinToVisibleBounds` – Přilepí zápatí části k zobrazení hranice viditelné kolekce.
 - `SectionHeadersPinToVisibleBounds` – Přilepí část hlavičky k zobrazení hranice viditelné kolekce.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Následující změny nebo přidání byly provedeny `UICollectionViewLayout` třídy v iOS 9:

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – Vrátí kontext zneplatnění na konci operace přetažení, pokud uživatel dokončí přetahování nebo zruší ho.
 - `GetInvalidationContextForInteractivelyMovingItems` – Vrátí kontext zneplatnění při spuštění operace přetažení.
 - `GetLayoutAttributesForInteractivelyMovingItem` – Získá atributy rozložení pro danou položku při přetažení položky.
 - `GetTargetIndexPathForInteractivelyMovingItem` – Vrátí `indexPath` položky, které v daném okamžiku při přetažení položky.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Následující změny nebo přidání byly provedeny `UICollectionViewLayoutAttributes` třídy v iOS 9:

 - `CollisionBoundingPath` – Vrátí cestu kolizí dvě položky během operace přetažení.
 - `CollisionBoundsType` – Vrátí typ kolizí (jako `UIDynamicItemCollisionBoundsType`), došlo k během operace přetažení.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Následující změny nebo přidání byly provedeny `UICollectionViewLayoutInvalidationContext` třídy v iOS 9:

 - `InteractiveMovementTarget` – Vrátí cílová položka operace přetažení.
 - `PreviousIndexPathsForInteractivelyMovingItems` – Vrátí `indexPaths` jiných položek součástí přetáhněte ke změně pořadí operaci.
 - `TargetIndexPathsForInteractivelyMovingItems` – Vrátí `indexPaths` položek, které bude přeuspořádány jako výsledek operace přetažení chcete změnit pořadí.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Následující změny nebo přidání byly provedeny `UICollectionViewSource` třídy v iOS 9:

 - `CanMoveItem` – Upozorní zobrazení kolekce, pokud daná položka může být přetáhněte přeuspořádány.
 - `GetTargetContentOffset` – Vrátí posunutí položky, které budou přesunuty pomocí operace přetažení chcete změnit pořadí.
 - `GetTargetIndexPathForMove` – Vrátí `indexPath` položky, kterou bude přesunut během operace přetažení chcete změnit pořadí.
 - `MoveItem` – Přesune pořadí danou položku v seznamu.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých změny zobrazení kolekcí v iOS 9 a popsané postupy pro jejich implementaci v Xamarin.iOS.
O něm zmínka implementaci jednoduché akce přetažení chcete změnit pořadí zobrazení kolekce; vlastní rozpoznávání rukopisu gesto pomocí přetahování na-změnit pořadí; a jak změnit přetáhněte pořadí ovlivňuje rozložení zobrazení vlastní kolekce.

## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Ukázkové zobrazení kolekce](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (ukázka)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [Události, protokoly a delegáti](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Práce s tabulkami a buněk](~/ios/user-interface/controls/tables/index.md)
