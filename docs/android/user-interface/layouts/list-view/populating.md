---
title: "Naplnění ListView s daty"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 12197d238ddc6ddc2bd8f48f77aa15f5eff22a0a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="populating-a-listview-with-data"></a>Naplnění ListView s daty


## <a name="overview"></a>Přehled

Přidání řádků, které mají `ListView` je nutné přidat rozložení a implementujte `IListAdapter` pomocí metod, `ListView` volání k naplnění sám sebe. Android zahrnuje integrovanou `ListActivity` a `ArrayAdapter` třídy, které můžete použít bez definování jakékoli vlastní rozložení XML nebo kódu. `ListActivity` Třída automaticky vytvoří `ListView` a zveřejňuje `ListAdapter` vlastnost k poskytování řádek pohledů pro zobrazení prostřednictvím adaptér.

Předdefinované adaptéry trvat ID prostředku zobrazení jako parametr, který se používá pro každý řádek. Můžete použít předdefinované prostředkům, například těch, které v `Android.Resource.Layout` , nemusíte psát vlastní.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Pomocí ListActivity a ArrayAdapter&lt;řetězec&gt;

V příkladu **BasicTable/HomeScreen.cs** ukazuje, jak používat tyto třídy k zobrazení `ListView` jenom pár řádků kódu:

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
   protected override void OnListItemClick(ListView l, View v, int position, long id)
}
```


### <a name="handling-row-clicks"></a>Zpracování řádek kliknutím na

Obvykle `ListView` vám také umožní uživatelům touch na řádek pro provedení několika akcí (například přehrávat skladbu nebo volání a obraťte se na zobrazení další obrazovky). Reakce na úpravy uživatele, je potřeba jeden další metoda implementována v `ListActivity` &ndash; `OnListItemClick` &ndash; podobné výjimky:

[![Snímek obrazovky SimpleListItem](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

Teď uživatel může touch řádek a `Toast` upozornění se zobrazí:

[![Snímek obrazovky z informační, který se zobrazí, když je dotýkal řádek](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>Implementace ListAdapter

`ArrayAdapter<string>` je skvělým z důvodu jeho jednoduchost, ale jeho velmi omezené. Ale často máte kolekci obchodní entity, nikoli jen řetězce, které chcete vytvořit vazbu.
Například pokud vaše data se skládá z kolekce tříd zaměstnanců, pak můžete v seznamu jenom zobrazit názvy jednotlivých zaměstnanců. Chcete-li přizpůsobit chování `ListView` k řízení, jaká data se zobrazí musí implementovat podtřídou třídy `BaseAdapter` přepsání následující čtyři položky:

-   **Počet** &ndash; s oznámením ovládacího prvku, kolik řádků se v datech.

-   **GetView** &ndash; lze vrátit zobrazení pro každý řádek naplněný daty.
    Tato metoda má parametr pro `ListView` předávat existující, nepoužívané řádek pro nové použití.

-   **GetItemId** &ndash; vrátí identifikátor řádku (obvykle řádek číslo, i když může být dlouhou hodnotu, která chcete).

-   **Tento [int]** indexer &ndash; vrátit data související s číslem konkrétního řádku.

Příklad kódu v **BasicTableAdapter/HomeScreenAdapter.cs** ukazuje, jak podtřídami `BaseAdapter`:

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```


### <a name="using-a-custom-adapter"></a>Pomocí vlastního adaptéru

Pomocí vlastního adaptéru je podobná integrované `ArrayAdapter`a předejte `context` a `string[]` hodnot pro zobrazení:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Protože v tomto příkladu se stejné rozvržení řádek (`SimpleListItem1`) výsledné aplikace bude vypadají stejně jako v předchozím příkladu.


### <a name="row-view-re-use"></a>Řádek zobrazení znovu použít.

V tomto příkladu jsou pouze šest položky. Vzhledem k tomu, že na obrazovce můžete začlenit osm, žádný řádek znovu použít vyžaduje. Při zobrazení stovkami nebo tisíci řádků, ale bylo by plýtvání paměti k vytvoření stovkami nebo tisíci `View` objekty, když pouze osm vejít na obrazovce najednou. Abyste předešli této situaci, když řádek dané zařízení zmizí z obrazovky, kterou jeho zobrazení je umístěn ve frontě pro nové použití. Jako uživatel posune, `ListView` volání `GetView` požádat o nový pohledů pro zobrazení &ndash; -li k dispozici předává nepoužívané zobrazení v `convertView` parametr. Pokud váš kód by měl vytvořit novou instanci zobrazení, je tato hodnota null, jinak můžete znovu nastavit vlastnosti daného objektu a znovu ho použít.

`GetView` Metoda postupujte podle tohoto vzoru k opětovnému použití zobrazení řádek:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Implementace vlastního adaptéru by *vždy* znovu použít `convertView` objekt před vytvořením nového zobrazení zajistit, že nemají systém nedostatek paměti při zobrazení dlouhými seznamy.

Některé implementace adaptér (, jako `CursorAdapter`) nemají `GetView` metoda, místo vyžadují dvě různé metody `NewView` a `BindView` který vynutit řádek znovu použít oddělením odpovědnosti `GetView` do dvou metody. Došlo `CursorAdapter` příklad později v dokumentu.


## <a name="enabling-fast-scrolling"></a>Povolení rychlého posouvání

Rychlé posouvání pomáhá uživatele nemuseli probírat dlouhými seznamy tím, že poskytuje další 'popisovač, který funguje jako posuvníku přímý přístup k části seznamu. Tento snímek obrazovky ukazuje rychlé posuvníku:

[![Snímek obrazovky fast posouvání s popisovačem posuvníku](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

Způsobuje rychlé posouvání popisovač zobrazí je jednoduché, nastavení `FastScrollEnabled` vlastnost `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>Přidání Index oddílu

Index oddílu poskytují další zpětnou vazbu pro uživatele, když jsou fast posouvání v seznamu &ndash; zobrazuje které 'oddíl' jste přešli do. Způsobíte index oddílu zobrazí podtřídy adaptéru musí implementovat `ISectionIndexer` rozhraní k poskytování text indexu v závislosti na řádky se zobrazí:

[![Snímek obrazovky H zobrazovaných výše oddíl, který začíná H](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

K implementaci `ISectionIndexer` je třeba přidat tři metody pro adaptér:

-   **GetSections** &ndash; poskytuje úplný seznam oddílu indexu produkty, které může se zobrazit. Tato metoda vyžaduje pole objektů Java, takže kód potřebuje k vytvoření `Java.Lang.Object[]` typu .NET Collection. V našem příkladu se vrátí seznam hodnot v seznamu jako počáteční znaky `Java.Lang.String` .

-   **GetPositionForSection** &ndash; vrátí první pozici řádek pro danou část index.

-   **GetSectionForPosition** &ndash; vrátí index oddílu, který se má zobrazit pro daný řádek.


V příkladu `SectionIndex/HomeScreenAdapter.cs` souboru implementuje tyto metody a některé další kód v konstruktoru. V konstruktoru vytvoří index oddílu tak, že každý řádek ve smyčce a extrahování první znak názvu (položky musí již být seřazeny toto provést).

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

S datové struktury vytvořili `ISectionIndexer` metody jsou velmi jednoduché:

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

Vaše názvů oddílů indexu není nutné mapování 1:1 na skutečné oddíly. To je důvod, proč `GetPositionForSection` metoda existuje.
`GetPositionForSection` vám dává příležitost k mapování, ať indexy, které jsou v seznamu indexu na jakémkoli oddíly jsou v zobrazení seznamu. Například "z" může mít v indexu, ale nemusí mít část tabulky pro každý písmeno, tak místo "z" mapování 26, může mapovat by měl namapovat 25 nebo 24 nebo libovolnou část index "z".



## <a name="related-links"></a>Související odkazy

- [BasicTableAndroid (ukázka)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (ukázka)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (ukázka)](https://developer.xamarin.com/samples/FastScroll/)
