---
title: ListView
description: "ListView je důležitou součástí uživatelského rozhraní aplikace pro Android; Umožňuje everywhere z krátké seznamy možností v nabídce dlouhými seznamy kontaktů nebo Oblíbené položky. Poskytuje jednoduchý způsob, jak posouvání seznam řádků, které může být naformátovaný předdefinovaný styl, nebo podob, k dispozici."
ms.topic: article
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 70a7abb186c102fb803c0ab6fa38c7b2d8222292
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="listview"></a>ListView

_ListView je důležitou součástí uživatelského rozhraní aplikace pro Android; Umožňuje everywhere z krátké seznamy možností v nabídce dlouhými seznamy kontaktů nebo Oblíbené položky. Poskytuje jednoduchý způsob, jak posouvání seznam řádků, které může být naformátovaný předdefinovaný styl, nebo podob, k dispozici._

<a name="overview" />

## <a name="overview"></a>Přehled

Zobrazení seznamu a adaptéry jsou součástí nejvíce základní stavební kameny aplikací pro Android. `ListView` Třída poskytuje flexibilní způsob, jak prezentaci dat toho, jestli je krátký nabídky nebo seznam dlouho posouvání. Poskytuje použitelnost funkcí, jako je rychlé posouvání, indexy a výběr jednoho nebo více pomoc při vytváření mobilní zařízení uživatelského rozhraní pro vaše aplikace. A `ListView` instance vyžaduje *adaptér* ke kanálu s daty obsaženými v řádku zobrazení.

Tato příručka vysvětluje, jak implementovat `ListView` a různé `Adapter` třídy v Xamarin.Android. Také ukazuje, jak přizpůsobit vzhled `ListView`, a popisuje význam řádek znovu použít, a snížit využití paměti. Je také některé diskuzi o jak ovlivňuje životního cyklu aktivity `ListView` a `Adapter` použít. Pokud pracujete v aplikacích pro různé platformy s Xamarin.iOS, `ListView` ovládací prvek je strukturálně podobný iOS `UITableView` (a pro Android `Adapter` je podobná `UITableViewSource`).

Nejprve krátký kurz představuje `ListView` s příklad základní kódu. Dále jsou uvedeny odkazy na pokročilejší témata vám pomohou používat `ListView` ve skutečných aplikacích.


> [!NOTE]
> **Poznámka:**: `RecyclerView` pomůcky je verze více pokročilé a flexibilní `ListView`. Protože `RecyclerView` je navržený jako nástupcem `ListView` (a `GridView`), doporučujeme vám, že používáte `RecyclerView` místo `ListView` pro nový vývoj aplikací. Další informace najdete v tématu [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).


<a name="tutorial" />

## <a name="listview-tutorial"></a>Kurz ListView

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) je [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) vytvářející posouvatelného položek seznamu. Položky seznamu jsou automaticky vloženy do seznamu pomocí [ `IListAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/).

V tomto kurzu vytvoříte posouvatelného seznam názvů země, které se načítají z pole řetězců. Pokud je vybrána položka seznamu, informační zpráva se zobrazí pozici položky v seznamu.


Spuštění nového projektu s názvem **HelloListView**.

Vytvořte soubor XML s názvem **list_item.xml** a uložit v rámci **prostředky, rozvržení nebo** složky. Vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

Tento soubor definuje rozložení pro každou položku, která bude uložena v umístění [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Otevřete `HelloListView.cs` a ujistěte se, třída rozšířit [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (místo [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)):

```csharp
public class HelloListView : ListActivity
{
```

Vložte následující kód pro [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metoda:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, ItemEventArgs args) {
        // When clicked, show a toast with the TextView text
        Toast.MakeText (Application, ((TextView)args.View).Text, ToastLength.Short).Show ();
    };
}
```

Všimněte si to nenačte soubor rozložení pro aktivitu (což obvykle provést s [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))).
Místo toho nastavení [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) vlastnost automaticky přidá [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) k vyplnění na celé obrazovce [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/).
Tato metoda přebírá [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), který spravuje pole položek seznamu, které budou umístěny do [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).
[ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/) Konstruktor trvá aplikace [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/), popis rozložení pro každou položku seznamu (vytvořený v předchozím kroku) a `T[]` nebo [ `Java.Util.IList<T>` ](https://developer.xamarin.com/api/type/Java.Util.IList/) pole objektů, která se přidá [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) (definovanou Další).

[ `TextFilterEnabled` ](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/) Vlastnost zapne text pro filtrování [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/)tak, že když uživatel zahájí zadáte, bude filtrováno seznamu.

[ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) Události lze použít k odběru obslužné rutiny pro kliknutí. Když je položka v [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) je klikli, se nazývá obslužná rutina a [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) se zobrazí zpráva, pomocí textu z kliknutelnou položky.

Můžete použít návrhy položky seznamu poskytované platformou místo definování vlastní rozložení souboru [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).
Například použijte `Android.Resource.Layout.SimpleListItem1` místo `Resource.Layout.list_item`.

Po [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metody přidat pole řetězců:

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

Toto je pole řetězce, které budou umístěny do [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Spusťte aplikaci. Můžete posuňte se v seznamu nebo zadejte vyfiltrujete ho a pak klikněte na položku Zobrazit zprávu. Měli byste vidět zhruba takhle:

[ ![Příklad snímek obrazovky ListView s názvy země](images/helloviews6.png)](images/helloviews6.png)

Všimněte si, že pomocí pole řetězců pevně není doporučený postup návrhu. V tomto kurzu pro jednoduchost, jeden se používá k předvedení [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) pomůcky. Lepší postupem je odkazovat na pole řetězců definované externí zdroj, například s `string-array` prostředků ve vašem projektu **Resources/Values/Strings.xml** souboru. Příklad:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="countries_array">
        <item>Bahrain</item>
        <item>Bangladesh</item>
        <item>Barbados</item>
        <item>Belarus</item>
        <item>Belgium</item>
        <item>Belize</item>
        <item>Benin</item>
    </string-array>
</resources>
```

Použít tyto řetězce prostředků pro [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), nahradí původní [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) řádek následujícím kódem:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

<a name="going_further" />

## <a name="going-further-with-listview"></a>Budete pokračovat s ListView

Zbývající témata (vedou následující odkazy) prohlédněte komplexní práce `ListView` třídy a různé typy typy adaptéru můžete použít s ním. Struktura je následující:

-   **Vzhled** &ndash; části `ListView` řízení a jak pracují.

-   **Třídy** &ndash; přehled tříd, které slouží k zobrazení `ListView`.

-   **Zobrazení dat v prvku ListView** &ndash; postupy: zobrazení jednoduchý seznam dat; jak implementovat `ListView's` funkce použitelnost; jak používat různé integrované řádek rozložení; a jak adaptéry ukládání paměti znovu pomocí zobrazení řádek.

-   **Vlastní vzhled** &ndash; změna styl `ListView` pro vlastní rozložení, písma a barvy.

-   **Pomocí SQLite** &ndash; jak zobrazit data z databáze SQLite s `CursorAdapter`.

-   **Životní cyklus aktivity** &ndash; aspekty návrhu při implementaci `ListView` aktivit, včetně, kde by měl v životním cyklu vyplňte vaše data a kdy k uvolnění prostředků.

Diskusní (rozdělen do šesti částí) začíná přehled `ListView` třídy samotné před představení progresivně složitější příklady způsobu jeho použití.

-   [Části a funkce ListView](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [Naplnění ListView s daty](~/android/user-interface/layouts/list-view/populating.md)
-   [Přizpůsobení vzhledu ListView](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [Používání CursorAdapterů](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [Používání ContentProvideru](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView a životní cyklus aktivity](~/android/user-interface/layouts/list-view/activity-lifecycle.md)

<a name="summary" />

## <a name="summary"></a>Souhrn

Témata zavedená `ListView` a poskytuje některé příklady, jak používat integrované funkce `ListActivity`. Je popsané vlastní implementace `ListView` povolené pro barevné rozložení a použití databáze SQLite a stručně dotýkal k závažnosti životního cyklu aktivity vaše `ListView` implementace.


## <a name="related-links"></a>Související odkazy

- [AccessoryViews (ukázka)](https://developer.xamarin.com/samples/AccessoryViews/)
- [BasicTableAndroid (ukázka)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (ukázka)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [BuiltInViews (ukázka)](https://developer.xamarin.com/samples/BuiltInViews/)
- [CustomRowView (ukázka)](https://developer.xamarin.com/samples/CustomRowView/)
- [FastScroll (ukázka)](https://developer.xamarin.com/samples/FastScroll/)
- [SectionIndex (ukázka)](https://developer.xamarin.com/samples/SectionIndex/)
- [SimpleCursorTableAdapter (ukázka)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (ukázka)](https://developer.xamarin.com/samples/CursorTableAdapter/)
- [Kurz životního cyklu aktivity](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Práce s tabulkami a buněk (v Xamarin.iOS)](~/ios/user-interface/controls/tables/index.md)
- [Referenční třída ListView](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [Odkaz na ListActivity – třída](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [Odkaz na BaseAdapter – třída](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [Odkaz na ArrayAdapter – třída](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [Odkaz na CursorAdapter – třída](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
