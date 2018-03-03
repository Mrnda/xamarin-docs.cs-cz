---
title: "Návod – vytvoření s TabHost záložkách uživatelského rozhraní"
description: "Tento článek provede procesem vytvoření záložkách uživatelského rozhraní v Xamarin.Android pomocí rozhraní API TabHost."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 99a35705c408d16f5b4b0e71e53dd453ae377341
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>Návod – vytvoření s TabHost záložkách uživatelského rozhraní

_Tento článek provede procesem vytvoření záložkách uživatelského rozhraní v Xamarin.Android pomocí rozhraní API TabHost._

> [!NOTE]
> **Poznámka:** `TabHost` je starý rozhraní API, které se už nepoužívá Google. Vývojáři se doporučuje vytvořit záložkách aplikací pomocí [nadřízených členů](~/android/user-interface/controls/action-bar.md). `ActionBar` Je k dispozici ve všech verzí systému Android. To bylo poprvé dostupné ve Android 3.0 (API úrovně 11) a byl přesně zpět do Android 2.2 (úroveň rozhraní API 8) a Android 2.3 (API úrovně 10) v [V7 kompatibility aplikace knihovny](http://developer.android.com/tools/support-library/features.html#v7-appcompat), což je k dispozici pro Xamarin.Android prostřednictvím [Xamarin Knihovna pro Android podporu - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) balíčku.

Tento článek provede procesem vytvoření záložkách uživatelského rozhraní v Xamarin.Android pomocí `TabHost` rozhraní API. Toto je starší rozhraní API, která je k dispozici ve všech verzích systémů Android. Tento příklad vytvoří aplikace s tři karty s logiku pro každé kartě se zapouzdřené v aktivitě.
Na následujícím snímku obrazovky je příklad aplikace, která vytvoříme:

![Příklad snímek obrazovky aplikace s více kartami](creating-a-tabbed-ui-images/image02.png)

<a name="Creating_the_Application" />

## <a name="creating-the-application"></a>Vytvoření aplikace

Stáhněte a rozbalte [TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/).
Tento projekt slouží jako výchozí bod pro naši aplikaci a obsahuje některé bitových kopií. Pokud si projdete tento projekt, zobrazí se, že jsme jste už vytvořili prostředky drawable ikony kartě.

První můžeme aktualizovat soubor rozložení **Resources/Layout/Main.axml** který bude hostitelem karty. Upravit **Resources/Layout/Main.axml** a vložte následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

Následující snímek obrazovky ukazuje rozložení v Návrháři Xamarin:

[![Snímek obrazovky TabHost rozložení v Návrháři Xamarin](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png)

TabHost musí mít dva podřízené zobrazení je uvnitř: `TabWidget` a `FrameLayout`. Na pozici `TabWidget` a `FrameLayout` svisle uvnitř `TabHost`, `LinearLayout` se používá. FrameLayout je, kde obsah pro každé kartě proběhne, což je prázdný protože `TabHost` bude automaticky vložení každou aktivitu v době běhu. Existuje několik pravidel, která musí být dodržen, pokud jde o vytváření rozložení pro záložkách uživatelského rozhraní:

-   `TabHost` Musí mít id `@android:id/tabhost`.

-   `TabWidget` Musí mít id `@android:id/tabs`.

-   `FrameLayout` Musí mít id `@android:id/tabcontent`.

-   `TabHost` vyžaduje aktivit, které spravuje dědění z `TabActivity`. Proto je důležité, abyste podtřídami `TabActivity` sem &ndash; standardní aktivity nebude fungovat.

Upravte soubor **MainActivity.cs** tak, aby třída `MainActivity` podtřídy `TabActivity` jak je znázorněno v následující fragment kódu:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

Vytvořit čtyři samostatné třídy aktivity ve vašem projektu: `MyScheduleActivity`, `SessionsActivity`, `SpeakersActivity`, a `WhatsOnActivity`. Každá aktivita budou použity k vytvoření uživatelského rozhraní na kartě. Tyto aktivity teď bude zástupnou proceduru, která zobrazuje `TextView` jednoduché zprávou. Upravit kód v každé aktivity obsahuje následující `OnCreate` implementace:

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

Všimněte si, že ve výše uvedeném kódu nepoužívá soubor rozložení. Právě vytvoří `TextView` textem a který nastaví `TextView` jako zobrazení obsahu. Duplicitní to pro každý zbývající tři aktivit.

Potom jsme se přiřadit ikony na každé kartě. Každé kartě vyžaduje dvě ikony &ndash; jeden pro vybraném stavu a jeden pro stav nezaškrtnuté. Příkladem tyto dvě různé ikony najdete v následující dvě bitové kopie (nezbytné ikony pro tuto aplikaci již byly přidány do ukázkový projekt):

![Snímek obrazovky s vybraném stavu a stavu nezaškrtnuté ikony](creating-a-tabbed-ui-images/tab-icons.png)


Jsme přidělí drawable prostředky na ikonu karty definováním *stavu-List Drawable*. Stav seznamu drawables jsou speciální drawable prostředky, které jsou definovány v XML a umožňují určit různé bitové kopie, které jsou specifické pro stav této položky. V tomto příkladu je jednu image, která se používá, pokud je vybrána na kartě a druhý, který se používá, pokud není vybrána na kartě. K ušetřit čas, nezbytné drawables stavu seznamu byly přidány do projektu. V následujícím seznamu jsou soubory a XML každý obsahuje:

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

Karty jsou přidány do `TabHost` prostřednictvím kódu programu, který je velmi opakovaných úloh. Usnadní to, přidejte následující metodu do třídy `MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

Každé kartě v `TabHost` představuje instanci z `TabHost.TabSpec` třídy. Tato instance obsahuje meta-data potřebné k vykreslení kartě, konkrétně:

-   **Text a ikona** &ndash; v zobrazí `TabWidget`.

-   **Kartě Obsah** &ndash; to může být buď `Activity` nebo `View` a se zobrazí, pokud je vybrána na kartě.

-   **Jedinečný kód** &ndash; každé kartě musí mít jedinečný kód přiřazen.

Nyní musíte přidat `TabHost.TabSpec` instance pro každé kartě v naší aplikaci. Umožňuje to udělat v dalším kroku.

Aktualizujte metodu `OnCreate` v `MainActivity` tak, aby je podobná následující kód:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

Spusťte aplikaci. Aplikace by měla vypadat podobně jako snímek obrazovky ukazuje na začátku tohoto návodu.

Je to! Vytvořili jsme záložkách aplikaci, která umožňuje uživateli snadný způsob přejděte do různých částí aplikace.


<a name="Summary" />

## <a name="summary"></a>Souhrn

Tato kapitola popsané záložkách rozložení a celým procesem vytvoření záložkách aplikace. Průvodce ukázal, jak lze použít `TabActivity` k zvýšilo rozložení souboru, že hostování `TabHost` a `TabWidget`. `TabHost` Se pak naplněný kolekce `TabHost.TabSpec` objekty, které by být používána `TabHost` za běhu k vytváření instancí aktivity, které se použije na jednotlivých kartách.



## <a name="related-links"></a>Související odkazy

- [TabHostWalkthrough (ukázka)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [Panel akcí](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Balíček NuGet Android, podporují knihovny v7 kompatibility aplikace](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Knihovna v7 kompatibility aplikace](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
