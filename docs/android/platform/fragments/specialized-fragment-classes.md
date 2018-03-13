---
title: "Specializované Fragment třídy"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: f962f4619352dbaaed8c8ffcf5d8c8305cb6ad62
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="specialized-fragment-classes"></a>Specializované Fragment třídy

Rozhraní API fragmenty poskytuje další podtříd zapouzdření některé z běžnějších funkce dostupné v aplikacích. Tyto podtřídy jsou:

-   **ListFragment** &ndash; tento Fragment slouží k zobrazení seznamu položek, které jsou vázány na zdroj dat, jako je například pole nebo kurzoru.

-   **DialogFragment** &ndash; tento Fragment slouží jako obálku kolem dialogové okno. Fragment zobrazí dialogové okno nad jeho aktivity.

-   **PreferenceFragment** &ndash; tento Fragment se používá k zobrazení předvoleb objekty seznamy.



## <a name="the-listfragment"></a>ListFragment

`ListFragment` Je velmi podobný koncept a funkce `ListActivity`; je obálky, který je hostitelem `ListView` v Fragment. Obrázek níže znázorňuje `ListFragment` systémem tablet a telefonního čísla:

[![Snímky obrazovky z ListFragment na tablet a na telefon](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>Vazba dat s ListAdapter

`ListFragment` Třída již poskytuje o výchozí rozložení, takže není nutné přepsat `OnCreateView` zobrazit obsah `ListFragment`. `ListView` Vázané na data pomocí `ListAdapter` implementace. Následující příklad ukazuje, jak to může provést pomocí jednoduchého pole řetězců:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

Při nastavení `ListAdapter`, je důležité použít `ListFragment.ListAdapter` vlastnost a ne `ListView.ListAdapter` vlastnost. Pomocí `ListView.ListAdapter` způsobí kód důležité inicializace přeskočen.



### <a name="responding-to-user-selection"></a>Reakce na výběr uživatele

Reagovat na výběr uživatelů, musí přepsat aplikaci `OnListItemClick` metoda. Následující příklad ukazuje, jednou z těchto možností:

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

V kódu výše, když uživatel vybere položku v `ListFragment`, Fragment nové se zobrazí v hostitelských aktivity, zobrazuje podrobnosti o položce, který byl vybrán.



## <a name="dialogfragment"></a>DialogFragment

*DialogFragment* je Fragment, který se používá k zobrazení objektu dialogového okna uvnitř Fragment, který bude float nad okno aktivity. Smyslem je nahradit dialogu spravované rozhraní API (od verze Android 3.0). Následující snímek obrazovky ukazuje příklad `DialogFragment`:

[![Snímek obrazovky DialogFragment zobrazení přidat textové pole nový Vehicle](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

A `DialogFragment` zajistí, aby zůstaly konzistentní stav mezi Fragment a dialogové okno. Všechny interakce a kontrolu nad objektu dialogového okna by měl nastat pomocí `DialogFragment` rozhraní API a nesmí být vytvářeny pomocí přímého volání na objektu dialogového okna. `DialogFragment` Rozhraní API poskytuje každá instance s `Show()` metoda, která se používá k zobrazení Fragment. Existují dva způsoby, jak se zbavit Fragment:

-  Volání `DialogFragment.Dismiss()` na `DialogFragment` instance. 

-  Zobrazit jiné `DialogFragment`.

Chcete-li vytvořit `DialogFragment`, třídy dědí z `Android.App.DialogFragment,` a pak přepíše jednu z následujících dvou metod:

- **OnCreateView** &ndash; tím se vytvoří a vrátí zobrazení.

- **OnCreateDialog** &ndash; tím se vytvoří vlastní dialogové okno. Obvykle se používá k zobrazení *AlertDialog*. Při přepisování tuto metodu, není nutné přepsat `OnCreateView` .



### <a name="a-simple-dialogfragment"></a>Jednoduché DialogFragment

Následující snímek obrazovky ukazuje jednoduchý `DialogFragment` má `TextView` a dvě `Button`s:

[![Příklad DialogFragment s TextView a dvě tlačítka](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView` Zobrazí počet pokusů, které uživatel klepne na jednu tlačítko `DialogFragment`, zatímco klepnutím na tlačítko Další se zavře Fragment. Kód pro `DialogFragment` je:

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```


### <a name="displaying-a-fragment"></a>Zobrazení Fragment

Všechny fragmenty, například `DialogFragment` se zobrazí v kontextu `FragmentTransaction`.

`Show()` Metodu `DialogFragment` trvá `FragmentTransaction` a `string` jako vstup. Dialogové okno bude přidána do aktivity a `FragmentTransaction` potvrdit.

Následující kód ukazuje jeden možný způsob aktivity mohou používat `Show()` metoda zobrazíte `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>Zavření Fragment

Volání metody `Dismiss()` na instanci systému `DialogFragment` způsobí, že Fragment odeberou z aktivity a potvrdí, že transakce.
Standardní metody Fragment životního cyklu, které se podílejí s zničení Fragment bude mít název.


### <a name="alert-dialog"></a>Dialogového okna výstrah

Místo přepsání `OnCreateView`, `DialogFragment` může místo přepsání `OnCreateDialog`. To umožňuje aplikaci vytvářet [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) , který je spravován nástrojem Fragment. Následující kód představuje příklad, který používá `AlertDialog.Builder` k vytvoření `Dialog`:

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```



## <a name="preferencefragment"></a>PreferenceFragment

Ke správě předvoleb, poskytuje rozhraní API fragmenty `PreferenceFragment` podtřídy. `PreferenceFragment` Je podobná [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/
) &ndash; hierarchie předvolby uživateli se zobrazí v Fragment. Jako uživatel pracuje s předvolby, se budou automaticky uloženy do [SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html).
V systému Android 3.0 nebo vyšší aplikace, použijte `PreferenceFragment` jak nakládat s předvoleb v aplikacích. Následující obrázek ukazuje příklad `PreferenceFragment`:

[![Příklad PreferencesFragment vložené, dialogové okno a předvolby spuštění](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>Vytvořit Fragment předvoleb z prostředku

Předvolba Fragment může zvětšený ze souboru XML prostředků pomocí [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) metoda. Logické místo, kde můžete volat tuto metodu v průběhu životního cyklu fragmentu by v `OnCreate` metoda.

`PreferenceFragment` Rozevíracích vyšší byl vytvořen načítání prostředku ze souboru XML. Soubor prostředků je:

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

Kód pro předvoleb Fragment vypadá takto:

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```



### <a name="querying-activities-to-create-a-preference-fragment"></a>Dotazování aktivit za účelem vytvoření Fragment předvoleb

Další postup pro vytváření `PreferenceFragment` zahrnuje dotazování aktivity. Každá aktivita můžete použít [METADATA\_klíč\_PŘEDVOLEB](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) atribut, který bude odkazovat na soubor XML prostředků. V Xamarin.Android, k tomu je potřeba adorning aktivitu `MetaDataAttribute`a potom zadáte tento soubor prostředků. `PreferenceFragment` Třída poskytuje metody [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)), lze použít k dotazování aktivitu najít tento prostředek XML a zvýšilo hierarchie předvoleb pro ni.

Příkladem tohoto procesu je součástí následující fragment kódu, který používá `AddPreferencesFromIntent` k vytvoření `PreferenceFragment`:

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

Android se podívejte se na třídu `MyActivityWithPreference`. Třída musí být ozdobené s `MetaDataAttribute,` jak je znázorněno v následující fragment kódu:

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

`MetaDataAttribute` Deklaruje souboru prostředků jazyka XML, který `PreferenceFragment` použije k zvýšilo předvoleb hierarchii. Pokud `MetatDataAttribute` není k dispozici, pak bude vyvolána výjimka za běhu. Po spuštění tohoto kódu `PreferenceFragment` se zobrazí jako na následujícím snímku obrazovky:

[![Snímek obrazovky aplikace příklad s PreferenceFragment zobrazí](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
