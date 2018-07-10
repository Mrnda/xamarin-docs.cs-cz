---
title: Výkon ListView
description: I když ListView je výkonný zobrazení pro zobrazení dat, má určitá omezení. Tento článek vysvětluje, jak zajistit skvělý výkon s Xamarin.Forms ListView v aplikaci.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 906fd60954b18064467e665295dba8bb75ed5a45
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935586"
---
# <a name="listview-performance"></a>Výkon ListView

Při vytváření mobilních aplikací, záleží na výkonu. Uživatelé zvykli očekávat plynulé posouvání a rychlé načítání. Služeb při selhání podle očekávání uživatelů bude stát hodnocení obchodu s aplikacemi nebo v případě – obchodní aplikace, náklady, vaše organizace čas a peníze.

I když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je výkonný zobrazení pro zobrazení dat, má určitá omezení. Posouvání výkonu může být negativně při použití vlastního buňky, zejména v případě, že obsahují zobrazení hluboce vnořené hierarchie nebo použití některých rozložení, které vyžadují hodně měření. Naštěstí jsou postupy, které vám umožní vyhnout se špatným výkonem.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Strategie ukládání do mezipaměti

Zobrazení se často používají k zobrazení mnohem většímu množství dat, než můžete přizpůsobit na obrazovce. Zvažte například app Hudba. Knihovna skladeb pravděpodobně tisíce položek. Jednoduchým přístupem, které by bylo vytvořit řádek pro každý skladby, by dojít ke snížení výkonu. Tento přístup zabírají cenný paměti a může zpomalit posouvání k procházení. Další možností je vytvořit a zničit řádky, protože data je přesunut do oblasti zobrazení. To vyžaduje konstantní vytváření instancí a vyčištění zobrazit objekty, které mohou být velmi pomalé.

Pro konzervaci paměti, nativní [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ekvivalenty pro jednotlivé platformy, mají integrované funkce pro opětovné použití řádky. Pouze v buňkách viditelný na obrazovce jsou načtena do paměti a **obsah** je načteno do existující buňky. Díky tomu aplikace nepotřebuje k vytvoření instance tisíce objektů, šetří čas a paměti.

Xamarin.Forms umožňuje [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buňky znovu pomocí [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) výčet, který má následující hodnoty:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Ignoruje univerzální platformu Windows (UPW) [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) ukládání do mezipaměti strategii, protože vždy používá ukládání do mezipaměti ke zlepšení výkonu. Proto ve výchozím nastavení se chová jako by [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) strategie ukládání do mezipaměti se použije.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) Ukládání do mezipaměti strategie Určuje, že [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) vygeneruje buňky pro každou položku v seznamu a výchozí nastavení je `ListView` chování. Obecně byste měli použít v následujících případech:

- Pokud každá buňka má velký počet vazeb (20-30 a více).
- Pokud šablona buňky často mění.
- Pokud testování odhalí, `RecycleElement` ukládání do mezipaměti strategie za následek sníženou provádění rychlost.

Je důležité uvědomit důsledky [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) ukládání do mezipaměti strategie při práci s vlastní buňky. Všechny buňky inicializační kód potřebuje ke spuštění pro každou buňku vytváření, což může být více než jednou za sekundu. V této situaci rozložení techniky, které byly jemné na stránce, jako je používání několika vnořené [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) instance, se stávají problémových míst výkonu, když se instalační program a zničen v reálném čase jako uživatel posune.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Ukládání do mezipaměti strategie Určuje, že [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) se snaží minimalizovat nároky na místo a provádění rychlosti jeho paměti recyklací seznamu buněk. Tento režim vždy neposkytuje zlepšení výkonu a testování se provádí, chcete-li zjistit případná zlepšení. Ale to je obecně upřednostňované volbou a by měl používat v následujících případech:

- Pokud má každá buňka malá až středně velkým počtem vazeb.
- Když každá buňka [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) definuje všechna data buňky.
- Pokud každá buňka je do značné míry podobná s neměnnými šablonu buňky.

Při virtualizaci buňky budou mít jeho kontextu vazby, aktualizovat a proto pokud aplikace používá tento režim musíte zajistit, že aktualizace kontextu vazby jsou zpracovávány. Všechna data o buňky musí pocházet z kontextu vazby nebo může dojít k chybám konzistence. To lze provést pomocí vazby dat k zobrazení dat buňky. Můžete také data buňky musí být nastaveno v `OnBindingContextChanged` přepsat, spíše než v konstruktoru vlastní buňky, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class CustomCell : ViewCell
{
  Image image = null;

  public CustomCell ()
  {
    image = new Image();
    View = image;
  }

  protected override void OnBindingContextChanged ()
  {
    base.OnBindingContextChanged ();

    var item = BindingContext as ImageItem;
    if (item != null) {
      image.Source = item.ImageUrl;
    }
  }
}
```

Další informace najdete v tématu [změny kontextu vazby](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

V Iosu a Androidu používáte-li buňky vlastní renderery, musí zajistit, že je správná implementace oznámení změn vlastností. Když údaje znovu použijí buňky hodnot vlastností se změní při aktualizaci kontextu vazby, který k dispozici buňky, s `PropertyChanged` událostí vyvolaných. Další informace najdete v tématu [přizpůsobení ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement s DataTemplateSelector

Když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) vyberte [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) ukládání do mezipaměti strategie neukládá do mezipaměti `DataTemplate`s. Místo toho `DataTemplate` je vybrán pro každou položku dat v seznamu.

> [!NOTE]
> [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Strategie ukládání do mezipaměti je jako nezbytné komponenty, počínaje Xamarin.Forms 2.4, že [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) se výzva k výběru [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), které každý `DataTemplate` musí vracet stejný [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) typu. Mějme například [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) s `DataTemplateSelector` , který může vrátit `MyDataTemplateA` (kde `MyDataTemplateA` vrátí `ViewCell` typu `MyViewCellA`), nebo `MyDataTemplateB` (kde `MyDataTemplateB`vrátí `ViewCell` typu `MyViewCellB`), kdy `MyDataTemplateA` je vrácena, musí vracet `MyViewCellA` nebo bude vyvolána výjimka.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Ukládání do mezipaměti strategie vychází [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) kromě zajištěním, že ukládání do mezipaměti strategie [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) k výběru [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`jsou ukládány do mezipaměti s typem položky v seznamu. Proto `DataTemplate`s jsou vybrány jednou za typ položky, namísto jednou za instance položky.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) Strategie ukládání do mezipaměti je jako nezbytné komponenty, která `DataTemplate`s vráceným [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) musí používat [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) konstruktor, který přijímá `Type`.

### <a name="setting-the-caching-strategy"></a>Nastavení strategií ukládání do mezipaměti

[ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) Hodnota výčtu není zadán s [ `ListView` ](xref:Xamarin.Forms.ListView) přetížení konstruktoru, jak je znázorněno v následujícím příkladu kódu:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

V XAML, nastavte `CachingStrategy` atributu, jak je znázorněno v následujícím kódu:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

To má stejný účinek jako argument strategie ukládání do mezipaměti v konstruktoru v C#. Všimněte si, že neexistuje žádná `CachingStrategy` vlastnost `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Nastavení ukládání do mezipaměti strategie v rozčleněných do podtříd ListView

Nastavení `CachingStrategy` atribut z XAML rozčleněných do podtříd [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nevytvoří požadované chování, protože neexistuje žádný `CachingStrategy` vlastnost `ListView`. Kromě toho pokud [XAMLC](~/xamarin-forms/xaml/xamlc.md) je povoleno, budou vytvořeny následující chybová zpráva: **žádná vlastnost, vázanou vlastnost nebo událost pro 'CachingStrategy' nalezen**

Řešení tohoto problému je zadání konstruktoru na rozčleněných do podtříd [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , který přijme [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) parametr a předá ji do základní třídy:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Pak bude [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) hodnota výčtu je zadat v XAML pomocí `x:Arguments` syntaxi:

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>Zlepšení výkonu ListView

Existuje mnoho postupů pro zlepšení výkonu `ListView`:

-  Vytvoření vazby `ItemsSource` vlastnost `IList<T>` kolekce místo `IEnumerable<T>` kolekce, protože `IEnumerable<T>` kolekce nepodporují náhodný přístup.
-  Použít integrované buňky (jako je `TextCell`  /  `SwitchCell` ) namísto `ViewCell` vždy, když je možné.
-  Použití menší počet elementů. Představme si třeba pomocí jediného `FormattedString` popisek místo více popisky.
-  Nahradit `ListView` s `TableView` při zobrazení nehomogenní data – to znamená, že data různých typů.
-  Omezit použití [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) metody. Pokud Nepromyšlené, způsobí snížení výkonu.
-  V Androidu, nenastavujte `ListView`na řádek oddělovač viditelnost nebo Barva po jeho vytvoření instance, jak to má za následek snížení výkonu velké.
-  Neměňte rozložení buněk, na základě [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Tím se spojují velké náklady na rozložení a inicializace.
-  Vyhněte se rozložení hluboce vnořené hierarchie. Použití `AbsoluteLayout` nebo `Grid` snížit vnoření.
-  Vyhněte se konkrétní `LayoutOptions` jiné než `Fill` (výplň je tím nejlevnější compute).
-  Předejde `ListView` uvnitř `ScrollView` z následujících důvodů:
    - `ListView` Implementuje vlastní posouvání.
    - `ListView` Nebude přijímat žádné gesta, jak se bude zpracován adresou nadřazené `ScrollView`.
    - `ListView` Ale může představovat vlastní záhlaví a zápatí, umožňuje posouvání prvky seznamu potenciálně nabízí funkce, která `ScrollView` byl použit pro. Další informace najdete v části [záhlaví a zápatí](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Pokud budete potřebovat velmi specifický, komplexní návrh uvedené ve vašich buňky vezměte v úvahu vlastní zobrazovací jednotky.

`AbsoluteLayout` má schopnost provádět rozložení bez volání jedné míry. To umožňuje velmi efektivní při výkonu. Pokud `AbsoluteLayout` nelze použít, zvažte [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Pokud používáte `RelativeLayout`, předávání omezení přímo bude výrazně rychlejší než použití výrazu rozhraní API. Důvodem je skutečnost, že výraz rozhraní API používá JIT, a v Iosu stromu má interpretovat, což je pomalejší. Výraz rozhraní API je vhodný pro rozložení stránky kde pouze vyžadovala počáteční rozložení a otočení, ale v `ListView`, kde je spuštěn neustále při posouvání, to bolí výkonu.

Vytváření vlastního rendereru pro [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nebo jeho buněk je jedním z přístupů k tím je omezen vliv rozložení výpočty na posouvání výkonu. Další informace najdete v tématu [přizpůsobení ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) a [přizpůsobení ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>Související odkazy

- [Vlastní nástroj pro vykreslování zobrazení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Vlastní nástroj pro vykreslování ViewCell (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
