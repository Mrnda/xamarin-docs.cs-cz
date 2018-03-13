---
title: "Číselník"
ms.topic: article
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b7c850d0ea06d69c3601081c1e9cde193903eb27
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="spinner"></a>Číselník

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) je pomůcku, která uvede rozevíracího seznamu pro výběr položek. Tato příručka vysvětluje, jak vytvořit jednoduchou aplikaci, která zobrazí seznam možností v číselník, následuje úpravy zobrazit další hodnoty přidružené k vybrané volby.

## <a name="basic-spinner"></a>Základní číselník

V první části tohoto kurzu vytvoříte jednoduchý číselník pomůcku, která zobrazuje seznam Planet. Pokud je vybraná planetu, zobrazí se zpráva informační vybrané položky:

[![Příklad snímky obrazovky HelloSpinner aplikace](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Spuštění nového projektu s názvem **HelloSpinner**.

Otevřete **Resources/Layout/Main.axml** a vložte následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

Všimněte si, že [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)na `android:text` atribut a [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)na `android:prompt` atribut obou odkazovat na stejný zdroj řetězec. Tento text se chová jako název dané pomůcky. Při použití [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/), zobrazí se v dialogovém okně Výběr, který se zobrazí při výběru widgetu textu nadpisu.

Upravit **Resources/Values/Strings.xml** a upravte soubor vypadat takto:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

Druhý `<string>` element definuje string title odkazuje [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) a [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) ve výše uvedené zobrazení.
`<string-array>` Element definuje seznam řetězců, které se zobrazí jako seznam v [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) pomůcky.

Nyní otevřete **MainActivity.cs** a přidejte následující `using` příkaz:

```csharp
using System;
```

Potom vložte následující kód pro [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metoda:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

Po `Main.axml` rozložení je nastaven jako zobrazení obsahu [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) pomůcky je zachycená z rozložení s [ `FindViewById<>(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
[ `CreateFromResource()` ](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/) Metoda vytvoří novou [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), který zajišťuje vazbu každou položku v poli řetězců pro počáteční vzhled [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) (která je, jak se objeví každou položku v číselník při výběru). `Resource.Array.planets_array` ID odkazy `string-array` definovaný nad a `Android.Resource.Layout.SimpleSpinnerItem` ID odkazuje rozložení pro standardní číselník vzhled, definované platformu.
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/) je volána po otevření widgetu definujte vzhled pro každou položku. Nakonec [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) nastavená přidružení všechny jeho položky s [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) nastavením [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter) vlastnost.

Teď poskytuje metodu zpětného volání, která notifys aplikace, když se vybere položka ze [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/). Zde je, co by měl vypadat takto:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Pokud je vybrána položka, odesílatel přetypovat [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) tak, aby položkám lze získat přístup. Pomocí `Position` vlastnost `ItemEventArgs`, můžete zjistit text vybraný objekt a použít ho k zobrazení [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/).

Spusťte aplikaci; ho by měl vypadat takto:

[![Příklad – snímek obrazovky s Mars vybrán jako planety číselník](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Číselník pomocí párů klíč hodnota

Často je nutné použít `Spinner` zobrazíte hodnoty klíče, které jsou spojené s určitého druhu dat používaných ve vaší aplikace. Protože `Spinner` nefunguje přímo s páry klíč/hodnota, musíte uložit dvojice klíč/hodnota samostatně, naplnit `Spinner` se hodnoty klíče, pak používat pozice vybraný klíč v číselníku k vyhledání hodnota přidružená data. 

V následujících krocích **HelloSpinner** aplikace je upravit tak, aby zobrazení střední teploty pro vybrané planety:

Přidejte následující `using` příkaz, který má **MainActivity.cs**:

```csharp
using System.Collections.Generic;
```

Přidat následující proměnnou instance `MainActivity` třídy.
Tento seznam bude obsahovat dvojice klíč/hodnota pro Planet a jejich střední teploty:

```csharp
private List<KeyValuePair<string, string>> planets;
```

V `OnCreate` metoda, přidejte následující kód před `adapter` je deklarovaná:

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

Tento kód vytvoří jednoduchý úložiště pro Planet a jejich přidružené střední teploty. (Ve skutečných aplikaci, databázi se obvykle používá k ukládání klíčů a jejich přidružených dat..)

Ihned po výše uvedeném kódu přidejte následující řádky k extrahování klíče a uložte je do seznamu (v pořadí):

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

Předat tohoto seznamu, `ArrayAdapter` – konstruktor (místo `planets_array` prostředků):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Upravit `spinner_ItemSelected` tak, aby vybrané pozice se používá k vyhledávání spojené s vybranou planety hodnoty (teplotní):

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Spusťte aplikaci; informační by měl vypadat takto:

[![Příklad výběru planetu zobrazení teploty](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)
   
  

## <a name="resources"></a>Prostředky

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou*
[*Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/).
