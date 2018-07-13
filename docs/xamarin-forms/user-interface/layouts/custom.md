---
title: Vytvoření vlastního rozložení
description: Tento článek vysvětluje, jak psát vlastní rozložení třídy a ukazuje třídu WrapLayout citlivé na orientaci, který uspořádá podřízené vodorovně na stránce a tento algoritmus pak zabalí zobrazení další podřízené objekty další řádky.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 0c16fd3930926a05ed7796391962d0fc8996dc96
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995372"
---
# <a name="creating-a-custom-layout"></a>Vytvoření vlastního rozložení

_Xamarin.Forms definuje čtyři rozložení třídy – StackLayout, AbsoluteLayout, RelativeLayout a mřížky, a každý uspořádá podřízené jiným způsobem. Někdy je však nutné uspořádat obsah stránky rozložení neposkytuje Xamarin.Forms. Tento článek vysvětluje, jak psát vlastní rozložení třídy a ukazuje třídu WrapLayout citlivé na orientaci, který uspořádá podřízené vodorovně na stránce a tento algoritmus pak zabalí zobrazení další podřízené objekty další řádky._

## <a name="overview"></a>Přehled

V Xamarin.Forms, všechny rozložení třídy odvozovat [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) třídy a jeho omezení obecného typu [ `View` ](xref:Xamarin.Forms.View) a jeho odvozených typů. Pak `Layout<T>` třída odvozena z [ `Layout` ](xref:Xamarin.Forms.Layout) třídu, která poskytuje mechanismus pro umístění a velikosti podřízené prvky.

Každý prvek visual je zodpovědný za určení vlastní upřednostňovanou velikost, která se nazývá *požadovaný* velikost. [`Page`](xref:Xamarin.Forms.Page), [ `Layout` ](xref:Xamarin.Forms.Layout), a [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) odvozené typy odpovědností zjistit umístění a velikosti jejich podřízené nebo relativní k samotné podřízené položky. Proto rozložení zahrnuje relaci nadřazený podřízený, nadřazené kde určí, co by měl být velikost jejích potomků, ale pokusí s ohledem na požadovanou velikost podřízeného.

K vytvoření vlastního rozložení je potřeba důkladné znalosti Xamarin.Forms rozložení a zneplatnění cyklů. Tyto cykly nyní probereme.

## <a name="layout"></a>Rozložení

Rozložení začíná v horní části stránky z vizuálního stromu se stránkou, a pokračuje přes všechny větve ve vizuálním stromu rozšiřovat a zahrnovat každou vizuální prvek na stránce. Prvky, které jsou rodičům, aby ostatní prvky zodpovídají za změně velikosti a umístění jejich podřízených vzhledem k sami.

[ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Definuje třídu [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metodu, která měří element pro operace rozložení a [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metodu, která určuje Zobrazí se oblasti obdélníkový element v rámci. Při spuštění aplikace a zobrazí se první stránka, *rozložení cyklu* první skládající se z `Measure` volání a potom `Layout` vyzývá, spustí [ `Page` ](xref:Xamarin.Forms.Page) objektu:

1. Během cyklu rozložení, každý nadřazený element zodpovídá za volání `Measure` metodu na jeho podřízené položky.
1. Po změření podřízené objekty, každý nadřazený prvek je zodpovědná za volání `Layout` metodu na jeho podřízené položky.

Tento cyklus se zajistí, že každý vizuální prvek na stránce přijímá volání `Measure` a `Layout` metody. Proces můžete vidět v následujícím diagramu:

![](custom-images/layout-cycle.png "Cyklus rozložení Xamarin.Forms")

> [!NOTE]
> Všimněte si, že rozložení cyklů může vzniknout také v podmnožině vizuálního stromu něco změní ovlivnit rozložení. To zahrnuje položky nebo jejímu odebrána z kolekce, například [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), změnu [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) vlastnost elementu nebo změnu velikosti prvku.

Každý Xamarin.Forms třídu, která má `Content` nebo `Children` vlastnost má přepisovatelným [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metody. Vlastní rozložení třídy, které jsou odvozeny z [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) musí tuto metodu přepsat a ujistěte se, že [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) a [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) jsou metody volá se na podřízené všechny elementy, k poskytování požadované vlastní rozložení.

Kromě toho každá třída, která je odvozena z [ `Layout` ](xref:Xamarin.Forms.Layout) nebo [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) musí přepsat [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metodu, která je tam, kde rozložení třídy Určuje velikost, která musí to být tím, že volání [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metody své podřízené objekty.

> [!NOTE]
> Prvky určují jeho velikost na základě *omezení*, která označuje, kolik místa je k dispozici pro prvek v rámci nadřazeného elementu. Omezení předán [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) a [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metody může být v rozsahu od 0 do `Double.PositiveInfinity`. Je prvek *omezené*, nebo *plně omezené*, když přijme volání jeho [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metoda s argumenty jiných nekonečné – element omezené do určité velikosti. Je prvek *neomezeným*, nebo *částečně omezené*, když přijme volání jeho `Measure` metoda s alespoň jedním argumentem rovna `Double.PositiveInfinity` – může být nekonečné omezení představit jako označující automatické přizpůsobení velikosti.

## <a name="invalidation"></a>Zrušení

Je proces, pomocí kterého změna v elementu na stránce spustí nový cyklus rozložení. Prvky jsou považovány za neplatné, když už nebude mít správnou velikost nebo pozice. Například pokud [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize) vlastnost [ `Button` ](xref:Xamarin.Forms.Button) změny, `Button` je označen jako neplatný, protože se už nebude mít správnou velikost. Změna velikosti `Button` pak pravděpodobně zvlněný efekt změn v rozložení procházení zbývající části stránky.

Prvky sami zneplatnit vyvoláním [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) metoda, obvykle vlastnost elementu změny, který může mít za následek novou velikost prvku. Tato metoda je vyvoláno [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) událost, která zpracovává nadřazeného prvku spustit nový cyklus rozložení.

[ `Layout` ](xref:Xamarin.Forms.Layout) Třída nastaví obslužnou rutinu pro [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) události u každé podřízené přidán do jeho `Content` vlastnost nebo `Children` kolekce a odpojí obslužné rutiny při podřízený se odebere. Proto je každý prvek ve vizuálním stromu, který má podřízené položky výstraha pokaždé, když jedna z jejích podřízených změní velikost. Následující diagram znázorňuje, jak změnit velikost elementu ve vizuálním stromu může způsobit změny, které ripple směrem nahoru:

![](custom-images/invalidation.png "Neplatnost ve vizuálním stromu")

Ale `Layout` třídy pokusí omezit dopad změnu velikosti dítěte v rozložení stránky. Pokud rozložení je velikost omezena, potom změnit velikost podřízeného neovlivní vyšší než nadřazené rozložení ve vizuálním stromu. Však obvykle změnu velikosti rozložení má vliv na způsob rozložení uspořádá podřízené. Proto všechny změny velikosti rozložení se spustí cyklus rozložení pro rozložení a rozložení obdrží volání jeho [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) a [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metody.

[ `Layout` ](xref:Xamarin.Forms.Layout) Také definuje třídu [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) metodu, která má k podobnému účelu k [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) metoda. `InvalidateLayout` Metoda má být volána v pokaždé, když dojde ke změně, která má vliv na způsob rozložení umístění a velikosti své podřízené objekty. Například `Layout` třída vyvolá `InvalidateLayout` metoda pokaždé, když je přidán či odebrán z rozložení podřízený.

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Může být potlačena za účelem implementace mezipaměti minimalizovat opakující volání [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metody rozložení pro podřízené položky. Přepsání `InvalidateLayout` metoda poskytne se upozornění, když jsou podřízené objekty přidán či odebrán z rozložení. Podobně platí [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) metoda může být potlačena za účelem poskytují oznámení, když se jeden z podřízených prvků na rozložení změní velikost. Pro obě přepsání metody by měly odpovídat vlastní rozložení vymazáním mezipaměti. Další informace najdete v tématu [výpočet a ukládání dat do mezipaměti](#caching).

## <a name="creating-a-custom-layout"></a>Vytvoření vlastního rozložení

Proces pro vytvoření vlastního rozložení je následujícím způsobem:

1. Vytvořte třídu, která je odvozena od třídy `Layout<View>`. Další informace najdete v tématu [vytváření WrapLayout](#creating).
1. [*volitelné*] vlastnosti, s možností vazby vlastnosti pro všechny parametry, které by měla být nastavena na rozložení třídy se opírá o přidat. Další informace najdete v tématu [přidání vlastnosti se opírá o vlastnosti umožňující vazbu](#adding_properties).
1. Přepsat [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metoda k vyvolání [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metodu na podřízené objekty všechny rozložení a vraťte se požadovaná velikost pro rozložení. Další informace najdete v tématu [přepsání metody OnMeasure](#onmeasure).
1. Přepsat [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metoda k vyvolání [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metodu na podřízené položky všechna rozložení. Nepodařilo se vyvolat [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metodu na jednotlivých podřízených v rozložení způsobí podřízené nikdy příjem správné velikosti nebo pozice, a proto nebude podřízené budou zobrazeny na stránce. Další informace najdete v tématu [přepsání metody LayoutChildren](#layoutchildren).

  > [!NOTE]
>  Při vytváření výčtu podřízených položek v [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) a [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) přepsání, přeskočit všechny podřízené jehož [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) je nastavena na `false`. Tím se zajistí, že vlastní rozložení neopustí místa pro děti jako neviditelné.

1. [*volitelné*] přepsat [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) metoda, která vás upozorní, když je přidán či odebrán z rozložení podřízené položky. Další informace najdete v tématu [přepsání metody InvalidateLayout](#invalidatelayout).
1. [*volitelné*] přepsat [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) metoda, která vás upozorní, když se jeden z podřízených prvků na rozložení změní velikost. Další informace najdete v tématu [přepsání metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Všimněte si, že [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) přepsání nebude zavolán, pokud velikost rozložení se řídí svého nadřazeného objektu, nikoli jeho podřízené položky. Však vyvolána přepsání Pokud jedna nebo obě omezení jsou nekonečné nebo pokud třída rozložení má nevýchozí [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) nebo [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) hodnot vlastností. Z tohoto důvodu [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) přepsání nelze spoléhat na podřízené velikosti zjišťovala během [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) volání metody. Místo toho `LayoutChildren` musí vyvolat [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) metodu na podřízené položky na rozložení, před vyvoláním [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody. Můžete také velikost podřízenou položku získanou v `OnMeasure` přepsání můžete do mezipaměti, aby se zabránilo později `Measure` volání v `LayoutChildren` přepsání, ale rozložení třídy budou potřebovat vědět, kdy velikosti muset získat znovu. Další informace najdete v tématu [výpočet a ukládání dat rozložení](#caching).

Rozložení třídy může být potom používán tak, že ji přidáte [ `Page` ](xref:Xamarin.Forms.Page)a přidáním podřízené prvky do rozložení. Další informace najdete v tématu [využívání WrapLayout](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Vytváření WrapLayout

Ukázková aplikace ukazuje orientace citlivé `WrapLayout` třídu, která uspořádá podřízené vodorovně na stránce a tento algoritmus pak zabalí zobrazení další podřízené objekty další řádky.

`WrapLayout` Třídy přiděluje stejné množství místa pro každý podřízený prvek, označované jako *buňky velikost*, v závislosti na maximální velikosti podřízené objekty. Menší než velikost buňky, může být umístěné v buňce podřízené položky na základě jejich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) hodnot vlastností.

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

`LayoutData` Struktura ukládá data o soubor děti počet vlastností:

- `VisibleChildCount` – počet podřízených objektů, které jsou viditelné v rozložení.
- `CellSize` – maximální velikost všech podřízených upraví velikost zobrazení.
- `Rows` – počet řádků.
- `Columns` – počet sloupců.

`layoutDataCache` Pole se používá k ukládání více `LayoutData` hodnoty. Při spuštění aplikace, dva `LayoutData` objekty budou uložené v mezipaměti do `layoutDataCache` slovník pro aktuální orientaci – jednu pro omezení argumenty, které mají `OnMeasure` přepsání a jeden pro `width` a `height` argumenty Chcete `LayoutChildren` přepsat. Při obměně zařízení v orientaci na šířku `OnMeasure` přepsat a `LayoutChildren` přepsání bude znovu vyvolat, jejímž výsledkem bude dva `LayoutData` objektů do mezipaměti do slovníku. Ale při návratu zařízení pro orientaci na výšku, žádné další výpočty jsou nutná kvůli tomu `layoutDataCache` již má požadovaná data.

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

`GetLayoutData` Metoda provádí následující operace:

- Určuje, zda výpočtová `LayoutData` hodnotu je již v mezipaměti a vrátí jej, pokud je k dispozici.
- V opačném případě se týká přes všechny podřízené objekty, volání `Measure` metodu na jednotlivých podřízených s neomezenou šířku a výšku a určuje podřízený maximální velikost.
- Za předpokladu, že existuje nejméně jeden podřízený prvek viditelný, vypočítá počet řádků a sloupců, které jsou požadované a pak vypočítá velikost buňky pro podřízené položky podle velikosti `WrapLayout`. Vezměte na vědomí, že velikost buňky je obvykle o něco větší než velikost maximální podřízené, ale, že mohou být také menší Pokud `WrapLayout` není dost široký, nejširší podřízený nebo dostatečně vysoký má nejvyšší podřízené.
- Uloží nový `LayoutData` hodnota v mezipaměti.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Přidání vlastností se opírá o vlastnosti umožňující vazbu

`WrapLayout` Třída definuje `ColumnSpacing` a `RowSpacing` vlastnosti, jejichž hodnoty se používají k oddělení řádků a sloupců v rozložení a které se zálohují na vlastnosti umožňující vazbu. Vlastnosti umožňující vazbu jsou uvedeny v následujícím příkladu kódu:

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

Vyvolá obslužnou rutinu změny vlastnosti každé umožňujících vazbu vlastnosti `InvalidateLayout` předat jí přepsání metody k aktivaci nového rozložení `WrapLayout`. Další informace najdete v tématu [přepsání metody InvalidateLayout](#invalidatelayout) a [přepsání metody OnChildMeasureInvalidated](#onchildmeasureinvalidated).

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

Vyvolá přepsání `GetLayoutData` metody a konstruktory `SizeRequest` objekt z vrácená data při taky s ohledem `RowSpacing` a `ColumnSpacing` hodnoty vlastností. Další informace o `GetLayoutData` metodu, najdete v článku [výpočet a ukládání dat do mezipaměti](#caching).

> [!IMPORTANT]
> [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) a [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) metody byste nikdy požadovat nekonečné dimenze vrácením [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest) s vlastností nastavenou na hodnotu `Double.PositiveInfinity`. Ale nejméně jeden z argumentů omezení na `OnMeasure` může být `Double.PositiveInfinity`.

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

Přepsání začíná volání `GetLayoutData` metoda a potom vytvoří výčet všech podřízených velikosti a umístění v rámci jednotlivých podřízených buňky. Toho dosáhnete pomocí volání [ `LayoutChildIntoBoundingRegion` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)) metodu, která se používá k umístění podřízených v rámci obdélník na základě jeho [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)hodnot vlastností. Jedná se o ekvivalent volání podřízeného [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) metody.

> [!NOTE]
> Všimněte si, že obdélník předán `LayoutChildIntoBoundingRegion` metoda zahrnuje celou oblast, ve kterém může být podřízená.

Další informace o `GetLayoutData` metodu, najdete v článku [výpočet a ukládání dat do mezipaměti](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>Přepsání metody InvalidateLayout

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Přepsání je voláno, když jsou podřízené objekty přidán či odebrán z rozložení, nebo pokud jeden z `WrapLayout` změny vlastností hodnoty, jak je znázorněno v následujícím příkladu kódu:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Přepsání zruší platnost rozložení a zahodí všechny informace uložené v mezipaměti rozložení.

> [!NOTE]
> Zastavit [ `Layout` ](xref:Xamarin.Forms.Layout) třídy volání [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) metoda pokaždé, když je přidán či odebrán z rozložení, podřízený přepsat [ `ShouldInvalidateOnChildAdded` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded(Xamarin.Forms.View)) a [ `ShouldInvalidateOnChildRemoved` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved(Xamarin.Forms.View)) metody a vrátit `false`. Rozložení třídy pak mohou implementovat vlastní proces, který při přidání či odebrání podřízených.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>Přepsání metody OnChildMeasureInvalidated

[ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) Přepsání je voláno, když jeden z podřízených prvků na rozložení se změní velikost a je znázorněno v následujícím příkladu kódu:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Přepsání zruší platnost podřízené rozložení a zahodí všechny informace uložené v mezipaměti rozložení.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>Využívání WrapLayout

`WrapLayout` Třídy mohou být spotřebovány jeho umístění na [ `Page` ](xref:Xamarin.Forms.Page) odvozeného typu, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Ekvivalentní kód jazyka C# je uveden níže:

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

Podřízené položky lze přidat do `WrapLayout` podle potřeby. Následující příklad kódu ukazuje [ `Image` ](xref:Xamarin.Forms.Image) prvky, které se přidávají do `WrapLayout`:

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

Když stránku obsahující `WrapLayout` se zobrazí, asynchronně přistupuje k vzdáleného souboru JSON obsahující seznam fotografie, vytvoří ukázkovou aplikaci [ `Image` ](xref:Xamarin.Forms.Image) – element pro každou fotografii a přidá jej do `WrapLayout`. Výsledkem je vzhled je znázorněno na následujících snímcích obrazovky:

![](custom-images/portait-screenshots.png "Snímky obrazovky ukázkové aplikace na výšku")

Následující snímky obrazovky zobrazit `WrapLayout` po je otočený k šířku:

![](custom-images/landscape-ios.png "Ukázkový snímek obrazovky aplikace na šířku iOS")
![](custom-images/landscape-android.png "ukázka Android snímek obrazovky na šířku aplikace") 
 ![ ] (custom-images/landscape-uwp.png " Ukázkový snímek obrazovky na šířku aplikace UPW")

Počet sloupců v jednotlivých řádcích závisí na velikosti fotografií, šířka obrazovky a počet pixelů na jednotku nezávislých na zařízení. [ `Image` ](xref:Xamarin.Forms.Image) Prvky asynchronně načíst fotografie a proto `WrapLayout` třídy obdrží častých volání jeho [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) metody jako každý `Image` Element obdrží novou velikost podle načíst fotografií.

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak psát vlastní rozložení třídy a jsme vám ukázali orientace citlivé `WrapLayout` třídu, která uspořádá podřízené vodorovně na stránce a tento algoritmus pak zabalí zobrazení další podřízené objekty další řádky.


## <a name="related-links"></a>Související odkazy

- [WrapLayout (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Vlastní rozložení](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Vytvoření vlastního rozložení v Xamarin.Forms (video)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Rozložení<T>](xref:Xamarin.Forms.Layout`1)
- [Rozložení](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)
