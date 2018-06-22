---
title: Výkon ListView
description: I když ListView je výkonný zobrazení pro zobrazení dat, má určitá omezení. Tento článek vysvětluje, jak zajistit vysoký výkon s Xamarin.Forms ListView v aplikaci.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: f1707a6b2a1dc03ae1346520bf29ff83f0fe74fb
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309808"
---
# <a name="listview-performance"></a>Výkon ListView

Při zápisu mobilních aplikací, záleží na výkon. Uživatelé mají dřívější očekávat plynulé posouvání a časů rychlé načítání. Nemůže splnit očekávání uživatelů bude náklady můžete hodnocení do obchodu s aplikacemi nebo v případě-obchodní aplikace, náklady organizace čas a peníze.

I když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je výkonný zobrazení pro zobrazení dat, má určitá omezení. Posouvání výkonu může dojít, pokud používáte vlastní buněk, zejména v případě, že budou obsahovat hluboko vložené zobrazení hierarchie, nebo použijte určité rozložení, které vyžadují mnoho měření. Naštěstí existují postupů, které můžete použít, aby se zabránilo snížení výkonu.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Ukládání do mezipaměti strategie

ListViews se často používají k zobrazení mnohem víc dat, než můžete umístit na obrazovce. Zvažte Hudba aplikace, třeba. Knihovna skladeb může mít tisíce položek. Jednoduchý přístup, kde bude vytvořením řádek pro každou skladbu, by měla mít snížený výkon. Tento přístup zbytečně plýtvá cenné paměti a může zpomalit posouvání k procházení. Jiná možnost je vytvořit a zrušení řádky jako dat je přesunut do zobrazení oblasti. To vyžaduje konstantní vytváření instancí a čištění zobrazení objektů, které může být velmi pomalé.

Pro konzervaci paměti, nativního [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ekvivalenty pro každou platformu mají integrované funkce pro opakované použití řádků. Pouze v buňkách viditelný na obrazovce jsou načtena do paměti a **obsah** je načten do existující buňky. To brání aplikaci v museli vytvořit instanci tisíc objektů, ukládání času a paměti.

Umožňuje Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buňky znovu použijte pomocí [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) výčtu, který má následující hodnoty:

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Ignoruje univerzální platformu Windows (UWP) [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) ukládání do mezipaměti strategie, protože vždy používá ukládání do mezipaměti ke zlepšení výkonu. Proto ve výchozím nastavení se chová jako kdyby [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) ukládání do mezipaměti strategie platí.

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) Ukládání do mezipaměti strategie Určuje, že [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) vygeneruje buňky pro každou položku v seznamu a výchozí nastavení je `ListView` chování. Obecně by být používána v následujících případech:

- Když každá buňka má velký počet vazby (20-30 +).
- Pokud šablona buňky často mění.
- Při testování zjistí, že `RecycleElement` ukládání do mezipaměti strategie má za následek menší provádění rychlost.

Je důležité rozpoznat důsledky [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) ukládání do mezipaměti strategii, při práci s vlastní buněk. Žádný kód inicializace buňky bude muset spustit pro vytvoření jednotlivých buněk, což může být více než jednou za sekundu. V této situaci rozložení techniky, které byly dobře na stránce, jako jsou pomocí více vnořených [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) instancí, začnou být kritické body po instalaci a zničen v reálném čase jako viditelné pro uživatele.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Ukládání do mezipaměti strategie Určuje, že [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) se pokusí o minimalizovat její nároky a provádění rychlost paměti podle recyklace seznamu buněk. Tento režim vždy nenabízí zlepšování výkonu a testování by měla provést, chcete-li zjistit případná zlepšení. Ale ho je obecně upřednostňovanou volbou a je třeba používat v následujících případech:

- Když každá buňka má malé a středně velkým počtem vazeb.
- Při každé buňce [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) definuje všechna data buňky.
- Pokud je do značné míry podobné, s neměnné buňky šablony jednotlivých buněk.

Při virtualizaci buňky budou mít jeho kontext vazby, aktualizovat, a tak pokud aplikace používá tento režim se musí zajistit správně zpracovává aktualizace kontextu vazby. Všechna data o buňky musí pocházet z kontextu vazby, nebo může dojít k chybám konzistence. To můžete udělat pomocí datové vazby k zobrazení dat buňky. Alternativně data buňky musí být nastaveno v `OnBindingContextChanged` přepsat, a nikoli v konstruktoru vlastní buňky, jak je ukázáno v následujícím příkladu kódu:

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

Další informace najdete v tématu [vazby kontextové změny](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

Na iOS a Android když buněk použít vlastní nástroji pro vykreslování, se musí zajistit, že je správně implementovaný oznámení o změně vlastností. Když jsou opakovaně buněk jejich hodnoty vlastností se změní při aktualizaci kontextu vazby na k dispozici buňky, který se `PropertyChanged` událostí vyvolaných. Další informace najdete v tématu [přizpůsobení ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement s DataTemplateSelector

Když [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) vybrat [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) ukládání do mezipaměti strategie neukládá do mezipaměti `DataTemplate`s. Místo toho `DataTemplate` pro každou položku dat v seznamu je vybrána.

> [!NOTE]
> [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Ukládání do mezipaměti strategie je nezbytné, počínaje Xamarin.Forms 2.4, který po [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) se zobrazí výzva k výběru [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)že každý `DataTemplate` musí vracet stejné [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) typu. Pokud například chcete, [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) s `DataTemplateSelector` , může vrátit buď `MyDataTemplateA` (kde `MyDataTemplateA` vrátí `ViewCell` typu `MyViewCellA`), nebo `MyDataTemplateB` (kde `MyDataTemplateB`vrátí `ViewCell` typu `MyViewCellB`), když `MyDataTemplateA` je vrácen musí vracet `MyViewCellA` nebo k výjimce.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) Ukládání do mezipaměti strategie vychází [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) ukládání do mezipaměti strategie podle kromě zajistit, že pokud [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) používá [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) vybrat [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`v mezipaměti s typem položky v seznamu. Proto `DataTemplate`s jsou vybrané jednou za typ položky, namísto jednou za instance položky.

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) Ukládání do mezipaměti strategie má předpoklad, `DataTemplate`s vrácený [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) musí používat [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) konstruktor, který přebírá `Type`.

### <a name="setting-the-caching-strategy"></a>Nastavení ukládání do mezipaměti strategie

[ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) Hodnota výčtu je definován s [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) přetížení konstruktoru, jak je znázorněno v následujícím příkladu kódu:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

V jazyce XAML, nastavte `CachingStrategy` atributu, jak je znázorněno v následujícím kódu:

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

Tato akce nemá stejný účinek jako nastavení ukládání do mezipaměti argument strategie v konstruktoru v C#. Všimněte si, že neexistuje žádná `CachingStrategy` vlastnost `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Nastavení ukládání do mezipaměti strategie v rozčleněné ListView

Nastavení `CachingStrategy` atribut z XAML rozčleněné [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) požadované chování, nebude vytvořit, protože neexistuje žádné `CachingStrategy` vlastnost `ListView`. Kromě toho pokud [XAMLC](~/xamarin-forms/xaml/xamlc.md) je povoleno, budou vytvořeny následující chybová zpráva: **žádná vlastnost, vazbu vlastnosti nebo události nalezené pro 'CachingStrategy.**

Řešení tohoto problému je zadání konstruktor na rozčleněné [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) který přijme [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) parametr a předá ji do základní třídy:

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Pak se [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) z XAML můžete zadat hodnotu výčtu pomocí `x:Arguments` syntaxe:

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

-  Vytvoření vazby `ItemsSource` vlastnost, která má `IList<T>` kolekce místo `IEnumerable<T>` kolekce, protože `IEnumerable<T>` kolekce nepodporují náhodný přístup.
-  Použít předdefinované buňky (jako je `TextCell`  /  `SwitchCell` ) místo `ViewCell` vždy, když je možné.
-  Použijte méně elementů. Například zvažte použití jedné `FormattedString` popisek místo několika popisky.
-  Nahraďte `ListView` s `TableView` při zobrazení-homogenního dat – to znamená, dat z různých typů.
-  Omezit použití [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) metoda. Pokud Nepromyšlené, způsobí snížení výkonu.
-  V systému Android, vyhněte se nastavení `ListView`na řádek oddělovače viditelnost nebo barvu po má po vytvoření instance, jako je výsledkem snížení výkonu velké.
-  Vyhnout změně rozložení buněk, na základě [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). To způsobuje náklady na velké rozložení a inicializace.
-  Nepoužívejte hluboko vložené rozložení hierarchie. Použití `AbsoluteLayout` nebo `Grid` snížit vnoření.
-  Vyhněte se konkrétní `LayoutOptions` jiné než `Fill` (výplně je cheapest k výpočtu).
-  Neumísťujte `ListView` uvnitř `ScrollView` z následujících důvodů:
    - `ListView` Implementuje vlastní posouvání.
    - `ListView` Nebude přijímat žádné gesta, jak se budou zpracovávat nadřazené `ScrollView`.
    - `ListView` Může být vlastní záhlaví a zápatí stránky, která se posouvá společně s prvky v seznamu, potenciálně nabídky funkce `ScrollView` byl použit pro. Další informace najdete v části [záhlaví a zápatí](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Pokud potřebujete velmi určité a složité návrh uvedené ve vašem buněk, zvažte vlastní zobrazovací jednotky.

`AbsoluteLayout` se může provést rozložení bez volání jedné míry. Díky tomu je velice mocný výkonu. Pokud `AbsoluteLayout` nelze použít, zvažte [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Pokud používáte `RelativeLayout`, předávání omezení přímo bude podstatně rychleji než pomocí výrazu rozhraní API. Je to způsobeno výrazu rozhraní API používá JIT, a v systému iOS stromu se budou interpretovat, což je pomalejší. Ve výrazu rozhraní API je vhodný pro rozložení stránek kde pouze vyžadovala na počáteční rozložení a oběh, ale v `ListView`, kde je spuštěn neustále při posouvání, ho škodí jak výkonu.

Vytváření vlastní zobrazovací jednotky pro [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) nebo jeho buněk je jeden ze způsobů snižuje účinek rozložení výpočty v posouvání výkonu. Další informace najdete v tématu [přizpůsobení prvku ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) a [přizpůsobení ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>Související odkazy

- [Vlastní vykreslení zobrazení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Vlastní zobrazovací jednotky ViewCell (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
