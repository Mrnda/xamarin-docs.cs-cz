---
title: Vytvoření vlastního rozložení
description: Tento článek vysvětluje, jak napsat vlastní rozložení třídu a ukazuje třídu WrapLayout orientaci-velká a malá písmena, který uspořádá podřízené vodorovně na stránce a potom zabalí zobrazení následné podřízené objekty další řádky.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: f225a80a1c386668b71d1cdd47409eb017f19033
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244932"
---
# <a name="creating-a-custom-layout"></a>Vytvoření vlastního rozložení

_Definuje Xamarin.Forms třídy čtyři rozložení – StackLayout, AbsoluteLayout, RelativeLayout a mřížky, a všechny její podřízené položky uspořádá jiným způsobem. Ale někdy je nezbytné k uspořádání obsahu stránce pomocí rozložení neposkytuje Xamarin.Forms. Tento článek vysvětluje, jak napsat vlastní rozložení třídu a ukazuje třídu WrapLayout orientaci-velká a malá písmena, který uspořádá podřízené vodorovně na stránce a potom zabalí zobrazení následné podřízené objekty další řádky._

## <a name="overview"></a>Přehled

Všechny třídy rozložení v Xamarin.Forms, jsou odvozeny od [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) třídy a omezit obecný typ, který má [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) a jeho odvozených typů. Pak `Layout<T>` třída odvozená z [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) třídy, která poskytuje mechanismus pro umístění a velikost podřízené elementy.

Každý element, visual je zodpovědná za určení vlastní upřednostňovanou velikost, která se označuje jako *požadovaný* velikost. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), a [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) odvozené typy jsou zodpovědní za určení umístění a velikost jejich podřízené, nebo relativně k samotné podřízené objekty. Proto rozložení zahrnuje relaci nadřazený podřízený, kde nadřazený Určuje, co by měla být velikost podřízené, ale pokusí se dokázala pojmout velikost požadovaný podřízený objekt.

Důkladné znalosti týkající se Xamarin.Forms rozložení a zneplatnění cykly je potřeba vytvořit vlastní rozložení. Tyto cykly teď probereme.

## <a name="layout"></a>Rozložení

Rozložení začne v horní části stromu visual se stránkou, a pokračuje prostřednictvím všechny větve vizuálním stromu zahrnovat každý visual element na stránce. Prvky, které jsou rodičů na další elementy jsou zodpovědní za změny velikosti a umístění jejich podřízené relativně k sami.

[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Třída definuje [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metoda, která měří element pro operace rozložení a [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metoda, která určuje v rámci bude vykreslen obdélníkovou oblast elementu. Při spuštění aplikace, a zobrazí se první stránka, *rozložení cyklus* první skládající se z `Measure` volání a potom `Layout` volání, spustí na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objektu:

1. Během cyklu rozložení každý nadřazený element zodpovídá za volání `Measure` metodu na jeho podřízených položek.
1. Po měří podřízené objekty, každý nadřazený element zodpovídá za volání `Layout` metodu na jeho podřízených položek.

Tento cyklus zajistí, že každý visual element na stránce přijímá volání `Measure` a `Layout` metody. Na následujícím obrázku je znázorněn proces:

![](custom-images/layout-cycle.png "Cyklus Xamarin.Forms rozložení")

> [!NOTE]
> Všimněte si, že rozložení cyklů může dojít také v podmnožině vizuálním stromu něco změny ovlivňují rozložení. To zahrnuje položky se přidat nebo odebrat z kolekce, jako [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), změnu [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) vlastnost elementu nebo změnit velikost elementu.

Každá třída Xamarin.Forms, který má `Content` nebo `Children` vlastnost má přepisovatelným [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) metoda. Vlastní rozložení třídy, které jsou odvozeny od [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) musí přepsat tuto metodu a ujistěte se, že [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) a [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metody volá se na všechny element podřízené objekty, zadejte požadované vlastní rozložení.

Kromě toho každá třída odvozený z [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) nebo [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) musí přepsat [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metodu, která je, kde třídu rozložení Určuje velikost, musí se jednat o tím, že volání [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metody jeho podřízených položek.

> [!NOTE]
> Elementy určit jejich velikost na základě *omezení*, který udává, kolik místa je k dispozici pro nějaký element v rámci nadřazeného elementu. Předaný omezení [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) a [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metody mohou být v rozsahu od 0 do `Double.PositiveInfinity`. Element *omezené*, nebo *plně omezené*, když obdrží volání jeho [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metoda s argumenty bez nekonečné - je omezené elementu konkrétní velikost. Element *neomezeným*, nebo *částečně omezené*, když obdrží volání jeho `Measure` metoda s alespoň jedním argumentem rovna `Double.PositiveInfinity` – může být nekonečné omezení představit jako označující Automatická změna velikosti.

## <a name="invalidation"></a>Zneplatnění

Zneplatnění je proces, pomocí kterého změnu element na stránce spustí cyklus nové rozložení. Elementy jsou považovány za neplatné, když už nebude mít správnou velikost nebo pozice. Například pokud [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/) vlastnost [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) změny, `Button` říká, že je pravděpodobně neplatný, protože se již nebude mít správnou velikost. Změna velikosti `Button` může pak mít vliv ripple změny v rozložení procházení zbývající části stránky.

Elementy zneplatnit sami vyvoláním [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) metoda, obecně až vlastnost elementu se změní, může mít za následek novou velikost elementu. Aktivuje se tato metoda [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) událost, která zpracovává nadřazeného elementu se spustit nový cyklus rozložení.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Třída nastaví obslužnou rutinu pro [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) událost v každé podřízené přidán do jeho `Content` vlastnost nebo `Children` kolekce a umožňuje odpojit obslužná rutina při podřízené se odebere. Každý element ve vizuálním stromu, který nemá podřízené objekty je proto upozornění pokaždé, když jednu z jejích podřízených změní velikost. Následující diagram znázorňuje, jak změnit velikost elementu ve vizuální strojové struktuře může způsobit změny, které ripple nahoru k:

![](custom-images/invalidation.png "Zneplatnění ve vizuálním stromu")

Ale `Layout` třída pokusí omezit dopad změny podřízené velikost na rozložení stránky. Pokud je velikost omezené rozložení, pak změnu velikosti podřízené neovlivní položky vyšší než rozložení nadřazené ve vizuálním stromu. Však obvykle změnu velikosti rozložení ovlivňuje způsob rozložení uspořádá své podřízené objekty. Proto všechny změny velikosti a rozložení se spustí cyklus rozložení pro rozložení a rozložení obdrží volání jeho [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) a [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) metody.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Třída definuje také [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) metoda, která má podobné účel k [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) metoda. `InvalidateLayout` Metoda by měla být volána vždy, když dojde ke změně, ovlivňuje způsob rozložení umisťuje a velikosti své podřízené objekty. Například `Layout` – třída volá `InvalidateLayout` metoda vždy, když je podřízená přidat nebo odebrat z rozložení.

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Může být potlačena za účelem implementace mezipaměti, aby se minimalizoval opakovaných volání [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metody pro rozložení podřízených prvků. Přepsání `InvalidateLayout` metoda zajistí upozornění při jsou podřízené objekty přidat nebo odebrat z rozložení. Podobně [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) metoda může být přepsáno za oznámení při jednu z podřízených na rozložení se změní velikost. U obou přepsání metoda by měla odpovídat vlastní rozložení tak, že vymazání mezipaměti. Další informace najdete v tématu [výpočet a ukládání do mezipaměti Data](#caching).

## <a name="creating-a-custom-layout"></a>Vytvoření vlastního rozložení

Proces vytvoření vlastního rozložení vypadá takto:

1. Vytvořte třídu, která je odvozena od třídy `Layout<View>`. Další informace najdete v tématu [vytváření WrapLayout](#creating).
1. [*volitelné*] přidávat vlastnosti, zajišťoval vazbu vlastnosti pro všechny parametry, které musí být nastavená na třídě rozložení. Další informace najdete v tématu [přidání zajišťoval vlastnosti vazbu vlastnosti](#adding_properties).
1. Přepsání [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metoda k vyvolání [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metodu podřízené objekty všechny rozložení a vraťte se požadovanou velikost pro rozložení. Další informace najdete v tématu [přepsání metody OnMeasure](#onmeasure).
1. Přepsání [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) metoda k vyvolání [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metodu na všechny rozložení podřízené objekty. Nepodařilo se vyvolat [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) podřízené nikdy přijetí správnou velikost nebo pozice bude výsledkem metody na jednotlivých podřízených v rozložení, a proto nebude stane podřízená na stránce vidět. Další informace najdete v tématu [přepsání metody LayoutChildren](#layoutchildren).

  > [!NOTE]
>  Při vytváření výčtu dětech do [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) a [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) přepsání, přeskočit všechny podřízené jejichž [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) je nastavena na `false`. Tím bude zajištěno, že vlastní rozložení nebude ponechte místa pro neviditelná podřízené objekty.

1. [*volitelné*] přepsat [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) metoda chcete být upozorněni, když jsou podřízené objekty přidat nebo odebrat z rozložení. Další informace najdete v tématu [přepsání metody InvalidateLayout](#invalidatelayout).
1. [*volitelné*] přepsat [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) metoda oznámení o jednu z podřízených na rozložení se změní velikost. Další informace najdete v tématu [přepsání metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Všimněte si, že [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) přepsání nebude volána, pokud velikost rozložení podléhá nadřazené, nikoli jeho podřízených položek. Však bude přepsání volána, pokud jedna nebo obě omezení nekonečné, nebo pokud třída rozložení má nevýchozí [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) nebo [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) hodnot vlastností. Z tohoto důvodu [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) nelze přepsat závisí na velikosti podřízené získané při [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) volání metody. Místo toho `LayoutChildren` musí volat [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) metodu pro rozložení, děti, před vyvoláním [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metoda. Alternativně velikost podřízených získané v `OnMeasure` přepsání můžete do mezipaměti, aby se zabránilo později `Measure` volání v `LayoutChildren` přepsání, ale třída rozložení budou potřebovat vědět, kdy je potřeba znovu získat velikosti. Další informace najdete v tématu [výpočet a ukládání do mezipaměti Data rozložení](#caching).

Třída rozložení může být potom používán přidáním jeho [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)a přidáním podřízené objekty do rozložení. Další informace najdete v tématu [využívání WrapLayout](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Vytváření WrapLayout

Ukázkovou aplikaci ukazuje orientaci citlivé `WrapLayout` třídu, která uspořádá podřízené vodorovně na stránce a potom zabalí zobrazení následné podřízené objekty další řádky.

`WrapLayout` Třída přidělí stejné množství místa na jednotlivých podřízených, označuje jako *buňka velikost*, podle maximální velikost podřízené objekty. Menší než velikost buňky můžete ocitne ve buňky podřízené objekty na základě jejich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) hodnot vlastností.

`WrapLayout` Definice třídy je znázorněno v následujícím příkladu kódu:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>Výpočet a ukládání do mezipaměti rozložení dat

`LayoutData` Struktura ukládá data o kolekci podřízených prvků v určité vlastnosti:

- `VisibleChildCount` – počet podřízených prvků, které jsou viditelné v rozložení.
- `CellSize` – maximální velikost všech podřízených upravený tak, aby velikost rozložení.
- `Rows` – počet řádků.
- `Columns` – počet sloupců.

`layoutDataCache` Pole se používá k ukládání více `LayoutData` hodnoty. Při spuštění aplikace, dva `LayoutData` objekty budou uložené v mezipaměti do `layoutDataCache` slovník pro aktuální orientaci – jednu pro omezení argumenty, které mají `OnMeasure` přepsání a jeden pro `width` a `height` argumenty na `LayoutChildren` přepsat. Při otočení zařízení do orientaci na šířku `OnMeasure` přepsat a `LayoutChildren` přepsání bude znovu vyvolán, bude výsledkem jiného dva `LayoutData` objektů do mezipaměti do slovníku. Při vracení zařízení na orientaci na výšku, žádné další výpočty jsou však nutná vzhledem k tomu `layoutDataCache` již požadovaná data.

Následující příklad kódu ukazuje `GetLayoutData` metodu, která vypočítá vlastnosti `LayoutData` strukturovaných podle určité velikosti:

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

`GetLayoutData` Metoda provede následující operace:

- Určuje, zda počítané `LayoutData` hodnota je již v mezipaměti a vrátí ji, pokud je k dispozici.
- Jinak hodnota zobrazí prostřednictvím všechny podřízené objekty, volání `Measure` metodu na všechny podřízené s neomezenou šířku a výšku a určuje podřízené maximální velikost.
- Za předpokladu, že existuje alespoň jeden podřízený viditelné, vypočítá počet řádků a sloupců, které jsou potřebné a pak vypočítá velikost buňky pro děti podle rozměry `WrapLayout`. Všimněte si, že velikost buňky je obvykle mírně širší než podřízené maximální velikost, ale, mohou být také menší Pokud `WrapLayout` není dost široké, aby pro nejširší talovat dostatečně nebo podřízená pro podřízený objekt nejvyšší.
- Ukládá nové `LayoutData` hodnota v mezipaměti.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Přidání vlastnosti používající vazbu vlastnosti

`WrapLayout` Třída definuje `ColumnSpacing` a `RowSpacing` vlastnosti, jejichž hodnoty se používají k oddělení řádků a sloupců v rozložení a který je zálohovaný díky vazbu vlastnosti. Vazbu vlastnosti jsou uvedeny v následující příklad kódu:

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

Vyvolá obslužnou rutinu vlastnost změnit každé vazbu vlastnosti `InvalidateLayout` přepsání metody k aktivaci nové rozložení předat na `WrapLayout`. Další informace najdete v tématu [přepsání metody InvalidateLayout](#invalidatelayout) a [přepsání metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>Přepsání metody OnMeasure

`OnMeasure` Přepsání je znázorněno v následujícím příkladu kódu:

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

Vyvolá přepsání `GetLayoutData` metoda a konstrukce `SizeRequest` objekt z vrácená data, ale rovněž berou v úvahu `RowSpacing` a `ColumnSpacing` hodnot vlastností. Další informace o `GetLayoutData` metodu, najdete v části [výpočet a ukládání do mezipaměti Data](#caching).

> [!IMPORTANT]
> [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) a [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) metody by nikdy požadavku nekonečné dimenze vrácením [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/) s vlastností nastavenou na hodnotu `Double.PositiveInfinity`. Ale alespoň jeden z argumentů omezení pro `OnMeasure` může být `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>Přepsání metody LayoutChildren

`LayoutChildren` Přepsání je znázorněno v následujícím příkladu kódu:

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

Přepsání začíná volání `GetLayoutData` metoda a potom zobrazí všechny podřízené objekty k určení velikosti a umístit je v rámci jednotlivých podřízených buněk. Toho se dosáhne vyvolání [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/) metodu, která se používá na pozici podřízenou v obdélníku na základě jeho [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)hodnoty vlastností. Jde o ekvivalent že zavoláte podřízené [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) metoda.

> [!NOTE]
> Všimněte si, že předaný rámeček `LayoutChildIntoBoundingRegion` metoda zahrnuje celou oblast, ve kterém mohou být uloženy podřízené.

Další informace o `GetLayoutData` metodu, najdete v části [výpočet a ukládání do mezipaměti Data](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>Přepsání metody InvalidateLayout

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Přepsání je volána, když jsou podřízené objekty přidat nebo odebrat z rozložení, nebo pokud jeden z `WrapLayout` změny vlastnosti hodnoty, jak je znázorněno v následujícím příkladu kódu:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Přepsání by způsobila neplatnost rozložení a odstraní všechny informace uložené v mezipaměti rozložení.

> [!NOTE]
> Zastavit [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) vyvolání třídy [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) metoda vždy, když je server přidat nebo odebrat z rozložení, podřízená přepsat [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/) a [ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/) metody a vraťte se `false`. Třída rozložení poté můžete implementovat vlastní proces, při přidávání nebo odebírání podřízené objekty.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>Přepsání metody OnChildMeasureInvalidated

[ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) Přepsání je volána, když jednu z podřízených na rozložení změny velikosti a je znázorněno v následujícím příkladu kódu:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Přepsání by způsobila neplatnost podřízené rozložení a zahodí se všechny informace uložené v mezipaměti rozložení.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>Použití WrapLayout

`WrapLayout` Třídy mohou být spotřebovávána jeho umístění na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) odvozeného typu, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Ekvivalentní kódu C# je zobrazena níže:

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

Podřízené položky můžete přidat do `WrapLayout` podle potřeby. Následující příklad kódu ukazuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) elementy, který se přidává do `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

Když na stránku obsahující `WrapLayout` se zobrazí, asynchronně přistupuje ke vzdálenému souboru JSON obsahující seznam fotografie, vytvoří ukázkovou aplikaci [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) element pro každou fotografii a přidá ho `WrapLayout`. Výsledkem je vidět na následujících snímcích obrazovky vzhled:

![](custom-images/portait-screenshots.png "Snímky obrazovky na výšku ukázkové aplikace")

Tyto snímky obrazovky zobrazit `WrapLayout` po byl otočen na šířku:

![](custom-images/landscape-ios.png "Ukázkové iOS – snímek obrazovky na šířku aplikace")
![](custom-images/landscape-android.png "ukázka Android snímek obrazovky na šířku aplikace") 
 ![ ] (custom-images/landscape-uwp.png " Ukázka snímek obrazovky na šířku aplikace UWP")

Počet sloupců v jednotlivých řádcích závisí na velikosti fotografií, šířka obrazovky a počet pixelů na jednotku nezávislé na zařízení. [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Elementy asynchronně načíst fotografie a proto `WrapLayout` třída obdrží častých volání jeho [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) jako každou metodu `Image` Element obdrží novou velikost podle načíst fotografie.

## <a name="summary"></a>Souhrn

Tento článek vysvětlené tom, jak psát vlastní rozložení třídy a ukázán orientaci citlivé `WrapLayout` třídu, která uspořádá podřízené vodorovně na stránce a potom zabalí zobrazení následné podřízené objekty další řádky.


## <a name="related-links"></a>Související odkazy

- [WrapLayout (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Vlastní rozložení](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Vytváření vlastní rozložení v Xamarin.Forms (video)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Rozložení<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [Rozložení](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
