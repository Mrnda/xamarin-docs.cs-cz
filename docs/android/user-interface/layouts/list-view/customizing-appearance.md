---
title: Přizpůsobení vzhledu prvku ListView
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: d1b6c663be5745455f332afc11c185869579fde3
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732902"
---
# <a name="customizing-a-listviews-appearance"></a>Přizpůsobení vzhledu prvku ListView


## <a name="overview"></a>Přehled

Vzhledu ovládacího prvku ListView je určen rozložení řádků, které se zobrazí. Chcete-li změnit vzhled `ListView`, použijte jiný řádek rozložení.


## <a name="built-in-row-views"></a>Předdefinované řádek zobrazení

Existují dvanáct předdefinovaných zobrazení, které mohou odkazovat pomocí **Android.Resource.Layout**:

- **TestListItem** &ndash; jeden řádek textu s minimálním formátováním.

- **SimpleListItem1** &ndash; jeden řádek textu.

- **SimpleListItem2** &ndash; dva řádky textu.

- **SimpleSelectableListItem** &ndash; jeden řádek textu, který podporuje výběr jednoho nebo více položek (přidáno v úroveň rozhraní API 11).

- **SimpleListItemActivated1** &ndash; podobný SimpleListItem1, ale barvu pozadí označuje, když je vybraný řádek (přidáno v úroveň rozhraní API 11).

- **SimpleListItemActivated2** &ndash; podobný SimpleListItem2, ale barvu pozadí označuje, když je vybraný řádek (přidáno v úroveň rozhraní API 11).

- **SimpleListItemChecked** &ndash; zobrazí značky zaškrtnutí označující výběr.

- **SimpleListItemMultipleChoice** &ndash; zobrazí políčka označující uchovává výběr.

- **SimpleListItemSingleChoice** &ndash; zobrazí přepínač tlačítka určete výběr vzájemně vylučují.

- **TwoLineListItem** &ndash; dva řádky textu.

- **ActivityListItem** &ndash; jeden řádek textu s bitovou kopií.

- **SimpleExpandableListItem** &ndash; řádky skupin podle kategorií a každou skupinu můžete rozbalit nebo sbalit.

Každý řádek předdefinovaných zobrazení má předdefinovaný styl s ním spojená. Tyto snímky obrazovky ukazují, jak se zobrazuje jednotlivých zobrazení:

[![Snímky obrazovky TestListItem, SimpleSelectableListItem, SimpleListitem1 a SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![Snímky obrazovky SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked a SimpleListItemMultipleChecked](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![Snímky obrazovky SimpleListItemSingleChoice, TwoLineListItem, ActivityListItem a SimpleExpandableListItem](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

**BuiltInViews/HomeScreenAdapter.cs** ukázkového souboru (v **BuiltInViews** řešení) obsahuje kód k vytvoření obrazovky položka seznamu – rozšíření. Zobrazení je nastavena v `GetView` metoda takto:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Zobrazení vlastností pak můžete nastavit odkazem identifikátory standardního ovládacího prvku `Text1`, `Text2` a `Icon` pod `Android.Resource.Id` (nenastavujte vlastnosti, které neobsahuje zobrazení nebo bude vyvolána výjimka):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter.cs** ukázkového souboru (v **BuiltInViews** řešení) obsahuje kód k vytvoření SimpleExpandableListItem obrazovky. Zobrazení skupiny je nastavena v `GetGroupView` metoda takto:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Podřízené zobrazení je nastavena v `GetChildView` metoda takto:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Vlastnosti pro zobrazení skupiny a podřízené zobrazení pak můžete nastavit odkazem standardní `Text1` a `Text2` identifikátory řídit, jak je uvedeno výše. Na snímku obrazovky SimpleExpandableListItem (viz výše) poskytuje příklad skupiny jednořádkové zobrazení (SimpleExpandableListItem1) a zobrazení podřízených dvou řádcích (SimpleExpandableListItem2). Alternativně zobrazení skupiny mohou být konfigurovány pro dva řádky (SimpleExpandableListItem2) a podřízené zobrazení mohou být konfigurovány pro jeden řádek (SimpleExpandableListItem1), nebo obě skupiny, zobrazení a podřízené zobrazení může mít stejný počet řádků. 



## <a name="accessories"></a>Příslušenství

Řádky mohou být příslušenství přidat napravo od zobrazení k označení stavu výběr:

- **SimpleListItemChecked** &ndash; vytvoří seznam jedním výběrem s kontrolou jako indikátor.

- **SimpleListItemSingleChoice** &ndash; vytvoří rádiové tlačítko zobrazí, kde je možné pouze jednu možnost.

- **SimpleListItemMultipleChoice** &ndash; vytvoří seznamy typu zaškrtávací políčko, kde je možné, více možností.

Zmíněnými příslušenství jsou zobrazené v následující obrazovky v pořadí podle jejich odpovídajících:

[![Snímky obrazovky SimpleListItemChecked, SimpleListItemSingleChoice a SimpleListItemMultipleChoice s příslušenství](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Zobrazit jednu z těchto příslušenství průchodu ID prostředku požadované rozložení adaptéru pak ručně nastavte stav výběru požadované řádků. Tento řádek kódu ukazuje, jak vytvořit a přiřadit `Adapter` pomocí těchto rozložení:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView` Samotné podporuje jiný výběr režimy, bez ohledu na to příslušenství zobrazily. Chcete-li předejít nejasnostem, použijte `Single` režim výběru s `SingleChoice` příslušenství a `Checked` nebo `Multiple` režimu se `MultipleChoice` stylu. Režim výběru řídí `ChoiceMode` vlastnost `ListView`.


### <a name="handling-api-level"></a>Zpracování úroveň rozhraní API

Starší verze Xamarin.Android implementovaný výčty jako vlastnosti celé číslo. Nejnovější verzi zavedla správné výčtové typy .NET, který umožní snazší zjistit potenciální možnosti.

V závislosti na tom, kterou úroveň rozhraní API cílíte, `ChoiceMode` je celé číslo nebo výčet. Ukázkový soubor **AccessoryViews/HomeScreen.cs** má blok komentované Pokud chcete cílové rozhraní API Perník:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = 1; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```


### <a name="selecting-items-programmatically"></a>Výběr položek prostřednictvím kódu programu

Ruční nastavení položky, které jsou 'vybrané, se provádí pomocí `SetItemChecked` – metoda (může být volána vícekrát pro vícenásobný výběr):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Kód také potřebuje ke zjištění jeden výběr jinak než více výběrů. Chcete-li zjistit, který řádek byla vybrána v `Single` použití režimu `CheckedItemPosition` vlastnosti integer.:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Chcete-li zjistit, které řádky byly vybrány v `Multiple` režimu, budete muset projít `CheckedItemPositions` `SparseBooleanArray`. Zhuštěný pole je jako slovník, který obsahuje pouze položky které byla hodnota změněna, takže musí procházet celou pole hledání `true` hodnoty vědět, co bylo vybráno v seznamu podle pokynů v následující fragment kódu:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>Vytváření vlastní řádek rozložení

Čtyři integrované řádek zobrazení jsou velmi jednoduché. K zobrazení složitější rozložení (například seznam e-mailů, nebo tweetů nebo kontaktní údaje) je požadovaná vlastní zobrazení. Vlastní zobrazení jsou obecně deklarované jako AXML soubory v **prostředky, rozvržení** adresář a potom načíst pomocí jejich Id vlastní adaptérem prostředku. Zobrazení může obsahovat libovolný počet třídy zobrazení (například TextViews ImageViews a jiných ovládacích prvků) s vlastní barvy, písma a rozložení.

V tomto příkladu se liší od předchozích příkladech v mnoha různými způsoby:

-  Dědí z `Activity` , nikoli `ListActivity` . Řádků můžete přizpůsobit pro žádné `ListView` , ale další ovládací prvky můžete také zahrnou `Activity` rozložení (například na záhlaví, tlačítka nebo další prvky uživatelského rozhraní). Tento příklad přidá záhlaví výše `ListView` pro ilustraci.

-  Vyžaduje soubor rozložení AXML obrazovky; v předchozích příkladech `ListActivity` nevyžaduje soubor rozložení. Obsahuje tato AXML `ListView` deklaraci ovládacího prvku.

-  Vyžaduje soubor AXML rozložení vykreslit každý řádek. Tento soubor AXML obsahuje ovládací prvky textových a obrázkových s vlastní písma a barev.

-  Soubor XML volitelné vlastní selektor používá k nastavení vzhledu řádků, pokud je vybrána.

-  `Adapter` Implementace vrátí vlastní rozložení z `GetView` přepsat.

-  `ItemClick` musí být deklarován jinak (obslužné rutiny události je připojen k `ListView.ItemClick` místo přepsání `OnListItemClick` v `ListActivity`).


Tyto změny jsou podrobně popsány níže, počínaje vytváření zobrazení aktivity a vlastní řádek zobrazení a potom pokrývajících úpravy adaptéru a aktivity k vykreslení je.


### <a name="adding-a-listview-to-an-activity-layout"></a>Přidání prvku ListView do aktivity rozložení

Protože `HomeScreen` už nebude dědit z `ListActivity` nemá výchozí zobrazení, takže pro zobrazení HomeScreen musí vytvořit soubor AXML rozložení. V tomto příkladu bude mít zobrazení nadpisu (pomocí `TextView`) a `ListView` zobrazujících data. Rozložení je definována v **Resources/Layout/HomeScreen.axml** souboru, který je znázorněno zde:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

Výhodou použití `Activity` vlastní rozložení (místo `ListActivity`) spočívá v je možné přidat další ovládací prvky na obrazovce, například záhlaví `TextView` v tomto příkladu.


### <a name="creating-a-custom-row-layout"></a>Vytváření vlastní řádek rozložení

Jiný soubor rozložení AXML musí obsahovat vlastní rozložení pro každý řádek, který se zobrazí v zobrazení seznamu. V tomto příkladu bude mít řádek zelenou pozadí, hnědá textových a obrázkových zarovnaný doprava. Kód Android XML deklarovat toto rozložení je popsaná v **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

Při vlastní řádek rozložení může obsahovat mnoho různých ovládacích prvků, posouvání výkon může být ovlivněno komplexních návrhů a pomocí bitové kopie (zejména v případě, že mají být načtená přes síť). Najdete v článku Google informace na adresování posouvání problémy s výkonem.


### <a name="referencing-a-custom-row-view"></a>Odkazování na řádek vlastní zobrazení

Implementace vlastního adaptéru příklad je v `HomeScreenAdapter.cs`. Metoda klíče je `GetView` kde ho načte vlastní AXML pomocí ID prostředku `Resource.Layout.CustomView`a poté nastaví vlastnosti pro každý ovládací prvky v zobrazení před vrácením. Je zobrazena třída dokončení adaptéru:

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```


### <a name="referencing-the-custom-listview-in-the-activity"></a>Odkazování na vlastní ListView v aktivitě

Protože `HomeScreen` třídy dědí vlastnosti z `Activity`, `ListView` pole je deklarovaná ve třídě k uložení odkazu na ovládací prvek deklarován v AXML:

```csharp
ListView listView;
```

Třída musí pak můžete načíst aktivity vlastní rozložení AXML pomocí `SetContentView` metoda. Potom můžete najít `ListView` ovládací prvek v rozložení potom vytvoří a přiřadí adaptéru a přiřadí obslužná rutina kliknutí na. Kód pro metodu OnCreate je uveden zde:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Nakonec `ItemClick` obslužné rutiny musí být definován; v takovém případě zobrazí pouze `Toast` zpráva:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Výsledný obrazovky vypadá takto:

[![Snímek obrazovky výsledné CustomRowView](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>Přizpůsobení barev pro výběr řádků

Když je dotýkal řádek by měl mít zvýrazněná pro zpětnou vazbu od uživatelů. Určuje, kdy vlastní zobrazení jako barvu pozadí jako **CustomView.axml** nemá, potlačí také výběr zvýraznění. Tento řádek kódu v **CustomView.axml** nastaví na pozadí na světle zelená, ale také znamená se žádné visual indikátoru, pokud je dotýkal řádek:

```xml
android:background="#FFDAFF7F"
```

Znovu zapnout zvýraznění chování a také k přizpůsobení barvu, která se používá, nastavte atribut pozadí místo na vlastních selektor. Selektor bude deklarovat výchozí barvu pozadí jak barva zvýraznění. Soubor **Resources/Drawable/CustomSelector.xml** obsahuje následující prohlášení:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

Chcete-li vlastní selektor, změnit atribut pozadí v **CustomView.axml** na:

```xml
android:background="@drawable/CustomSelector"
```

Vybraný řádek a odpovídající `Toast` zprávy teď vypadá takto:

[![Vybraný řádek v oranžová s informační zprávou zobrazení název vybraného řádku](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>Brání blikání na vlastní rozložení

Android pokusí zlepšit výkon `ListView` posouvání pomocí ukládání do mezipaměti informace o rozložení. Pokud máte dlouhý posouvání seznam dat je třeba také nastavit `android:cacheColorHint` vlastnost `ListView` deklarace v definici aktivity AXML (na stejnou hodnotu barva jako vaše vlastní řádek rozložení pozadí). Zahrnout tento pomocný může dojít, blikání' jako viditelné uživatele prostřednictvím seznam s barvy pozadí vlastní řádek.



## <a name="related-links"></a>Související odkazy

- [BuiltInViews (ukázka)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (ukázka)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (ukázka)](https://developer.xamarin.com/samples/CustomRowView/)
