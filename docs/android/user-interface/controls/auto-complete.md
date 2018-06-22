---
title: Automatické dokončování
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 333264e65e814ad73eabeb7be55abb320400a59a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764447"
---
# <a name="auto-complete"></a>Automatické dokončování


## <a name="overview"></a>Přehled

K vytvoření textu položky pomůcku, která poskytuje automatické doplňování návrzích, použijte [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) pomůcky. Návrhy byly přijaty z kolekce řetězce přidružené k widgetu prostřednictvím [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/).

V tomto kurzu vytvoříte [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) pomůcku, která poskytuje název země.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

[ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) Je štítek, který představuje [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) pomůcky.


## <a name="tutorial"></a>Tutoriál

Spuštění nového projektu s názvem *HelloAutoComplete*.

Vytvořte soubor XML s názvem `list_item.xml` a uložit v rámci **prostředky, rozvržení** složky. Nastavte akce sestavení tohoto souboru do `AndroidResource`. Upravte soubor vypadat takto:

```xml
<?xml version="1.0" encoding="utf-8"?>

<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp"
    android:textColor="#000">
</TextView>
```

Tento soubor definuje jednoduchou [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) který se použije pro každou položku, která se zobrazí v seznamu návrhů.

Otevřete **Resources/Layout/Main.axml** a vložte následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

Otevřete **MainActivity.cs** a vložte následující kód pro [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metoda:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    AutoCompleteTextView textView = FindViewById<AutoCompleteTextView> (Resource.Id.autocomplete_country);
    var adapter = new ArrayAdapter<String> (this, Resource.Layout.list_item, COUNTRIES);

    textView.Adapter = adapter;
}
```

Po zobrazení obsahu na hodnotu `main.xml` rozložení [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) pomůcky je zachycená z rozložení s [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). Nový [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) je potom inicializován pro vazbu `list_item.xml` rozložení na jednotlivé položky seznamu `COUNTRIES` pole řetězců (definovanou v dalším kroku). Nakonec `SetAdapter()` se nazývá přidružení [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) s [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) pomůcky tak, aby pole řetězců bude naplnění seznamu návrhů.

Uvnitř `MainActivity` třídy, přidejte pole řetězců:

```csharp
static string[] COUNTRIES = new string[] {
  "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra",
  "Angola", "Anguilla", "Antarctica", "Antigua and Barbuda", "Argentina",
  "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium",
  "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia",
  "Bosnia and Herzegovina", "Botswana", "Bouvet Island", "Brazil", "British Indian Ocean Territory",
  "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi",
  "Cote d'Ivoire", "Cambodia", "Cameroon", "Canada", "Cape Verde",
  "Cayman Islands", "Central African Republic", "Chad", "Chile", "China",
  "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo",
  "Cook Islands", "Costa Rica", "Croatia", "Cuba", "Cyprus", "Czech Republic",
  "Democratic Republic of the Congo", "Denmark", "Djibouti", "Dominica", "Dominican Republic",
  "East Timor", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Ethiopia", "Faeroe Islands", "Falkland Islands", "Fiji", "Finland",
  "Former Yugoslav Republic of Macedonia", "France", "French Guiana", "French Polynesia",
  "French Southern Territories", "Gabon", "Georgia", "Germany", "Ghana", "Gibraltar",
  "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau",
  "Guyana", "Haiti", "Heard Island and McDonald Islands", "Honduras", "Hong Kong", "Hungary",
  "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
  "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
  "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg",
  "Macau", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands",
  "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia",
  "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
  "Nicaragua", "Niger", "Nigeria", "Niue", "Norfolk Island", "North Korea", "Northern Marianas",
  "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay", "Peru",
  "Philippines", "Pitcairn Islands", "Poland", "Portugal", "Puerto Rico", "Qatar",
  "Reunion", "Romania", "Russia", "Rwanda", "Sqo Tome and Principe", "Saint Helena",
  "Saint Kitts and Nevis", "Saint Lucia", "Saint Pierre and Miquelon",
  "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Saudi Arabia", "Senegal",
  "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands",
  "Somalia", "South Africa", "South Georgia and the South Sandwich Islands", "South Korea",
  "Spain", "Sri Lanka", "Sudan", "Suriname", "Svalbard and Jan Mayen", "Swaziland", "Sweden",
  "Switzerland", "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "The Bahamas",
  "The Gambia", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey",
  "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", "Virgin Islands", "Uganda",
  "Ukraine", "United Arab Emirates", "United Kingdom",
  "United States", "United States Minor Outlying Islands", "Uruguay", "Uzbekistan",
  "Vanuatu", "Vatican City", "Venezuela", "Vietnam", "Wallis and Futuna", "Western Sahara",
  "Yemen", "Yugoslavia", "Zambia", "Zimbabwe"
};
```

Toto je seznam návrhů, které se mají poskytnout v rozevíracím seznamu, když uživatel zadá do [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) pomůcky.

Spusťte aplikaci. Jak budete zadávat, byste měli vidět přibližně takto:

[![Automatické dokončování snímku obrazovky s příkladem výpis názvy, které obsahují "ca"](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>Další informace

Všimněte si, že pomocí pole řetězců pevně není postup doporučený návrh, protože kód aplikace by měla soustředit na chování, není obsahu. Obsah aplikace, jako jsou třeba řetězce by měl být externalized z kódu pro úpravy obsahu snadněji a usnadnění lokalizaci obsahu. Pevně řetězce se používají v tomto kurzu pouze k zjednodušují a zaměřit se na [ `AutoCompleteTextView` ](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/) pomůcky. Místo toho vaše aplikace by měly deklarovat takovými poli řetězec v souboru XML. To lze provést pomocí `<string-array>` prostředků ve vašem projektu `res/values/strings.xml` souboru. Příklad:

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

Použít tyto řetězce prostředků pro [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), nahradí původní [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) konstruktoru řádku následujícím kódem:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>Odkazy

-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
-   [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)

*Úpravy, které jsou na základě práce vytvořen a sdílí projektu pro Android otevřít zdroje a používají podle podmínek, které jsou popsané v části této stránky jsou* 
 [ *Creative Commons 2.5 porušení licence* ](http://creativecommons.org/licenses/by/2.5/) *. V tomto kurzu vychází z* 
 [ *Android automaticky dokončení kurzu*](http://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
 *.*
