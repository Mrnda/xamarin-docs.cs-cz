---
title: Na kartách rozložení s nadřízených členů.
description: Tento průvodce uvádí a vysvětluje, jak vytvořit záložkách uživatelské rozhraní v aplikaci Xamarin.Android pomocí rozhraní API nadřízených členů.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3e96ce2064391d585943f4d79453f8b4f8c6f583
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
ms.locfileid: "32020124"
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Na kartách rozložení s nadřízených členů.

_Tento průvodce uvádí a vysvětluje, jak vytvořit záložkách uživatelské rozhraní v aplikaci Xamarin.Android pomocí rozhraní API nadřízených členů._


## <a name="overview"></a>Přehled

Na panelu akcí je Android uživatelského rozhraní vzor, který slouží k poskytování konzistentní uživatelské rozhraní pro klíčové funkce, jako jsou karty, identita aplikace, nabídky a vyhledávání. V Android 3.0 (API úrovně 11) Google uvedla nadřízených členů rozhraní API pro platformu Android. Rozhraní API nadřízených členů zavést motivy uživatelského rozhraní zajistit konzistentní vzhled a chování a třídy, které umožňují záložkách uživatelská rozhraní. Tato příručka popisuje postup přidání kartách panelu akcí do aplikace pro Xamarin.Android. Také popisuje, jak používat podporu knihovna pro Android v7 na karty backport nadřízených členů pro Xamarin.Android aplikací pro Android 2.1, aby Android 2.3. 

Všimněte si, že `Toolbar` je součástí panelu novější a více zobecněný akce, která byste měli používat místo z `ActionBar` (`Toolbar` byla navržená tak, aby nahradit `ActionBar`). Další informace najdete v tématu [nástrojů](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="requirements"></a>Požadavky

Jakékoli aplikace Xamarin.Android, která cílí úroveň rozhraní API 11 (Android 3.0) nebo vyšší má přístup k rozhraní API nadřízených členů jako součást nativních rozhraní API systému Android. 

Některé z rozhraní API nadřízených členů zpět přenesené úroveň rozhraní API 7 (Android 2.1) a jsou dostupné prostřednictvím [V7 kompatibility aplikace knihovny](http://developer.android.com/tools/support-library/features.html#v7-appcompat), který je k dispozici aplikace Xamarin.Android přes [Xamarin Android Support Library - V7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) balíčku.



## <a name="introducing-tabs-in-the-actionbar"></a>Představení karty v nadřízených členů.

Na panelu akcí se pokusí zobrazíte všechny jeho karty souběžně a proveďte všechny karty stejnou velikost jako na základě šířka nejširší popisek karty. Tento koncept je znázorněn podle následující snímek obrazovky: 

![Příklad snímek obrazovky nadřízených členů vidět všechny karty rovná velikosti](with-action-bar-images/image1.png)

Když nadřízených členů. nelze zobrazit všechny karty, bude nastavení na karty v vodorovně posouvatelným zobrazení. Uživatel může prstem doleva nebo doprava zobrazíte zbývající karty. Tento snímek obrazovky na webu Google Play znázorňuje příklad tohoto: 

![Příklad snímek obrazovky karty v vodorovně posouvatelným zobrazení](with-action-bar-images/image2.png)

Každé kartě na panelu akcí by měl být přidružen [ *fragment*](~/android/platform/fragments/index.md). Když uživatel vybere na kartě, aplikace se zobrazí fragment, který je přidružen kartě. Nadřízených členů není zodpovědná za zobrazení příslušné fragment uživateli. Místo toho nadřízených členů oznámí aplikace o změny stavu na kartě prostřednictvím třídu, která implementuje rozhraní ActionBar.ITabListener. Toto rozhraní obsahuje tři metody zpětného volání, které Android bude vyvolán při změně stavu na kartě: 

-  **OnTabSelected** -tato metoda je volána, když uživatel vybere kartě. Mělo by se zobrazit fragment.

-  **OnTabReselected** -tato metoda je volána, když na kartě je již vybrána, ale je znovu vybraný uživatelem. Tato zpětné volání se obvykle používá k aktualizaci nebo aktualizace zobrazených fragment.

-  **OnTabUnselected** -tato metoda je volána, když uživatel vybere jinou kartu. Tato zpětné volání se používá pro uložení stavu v zobrazených fragmentu před dané zařízení zmizí.

Zabalí Xamarin.Android `ActionBar.ITabListener` s událostmi na `ActionBar.Tab` třídy. Aplikace může přiřadit obslužné rutiny událostí na jeden nebo více z těchto událostí. Existují tři události (jeden pro každou metodu v `ActionBar.ITabListener`), bude vyvolána panelu karta akce: 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>Přidání karet do nadřízených členů.

Nadřízených členů je nativní Android 3.0 (API úrovně 11) a vyšší a je k dispozici pro všechny aplikace Xamarin.Android, která cílí toto rozhraní API jako minimální. 

Následující kroky ukazují, jak přidat karty nadřízených členů do Android aktivity: 

1. V `OnCreate` metoda aktivity &ndash; *před inicializace žádné uživatelské rozhraní pomůcek* &ndash; aplikace musíte nastavit `NavigationMode` na `ActionBar` k `ActionBar.NavigationModeTabs` jak je znázorněno v následujícím kódu fragment kódu:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Vytvořte novou kartu pomocí `ActionBar.NewTab()`.

3. Přiřadit obslužné rutiny událostí nebo zadejte vlastní `ActionBar.ITabListener` implementace, která bude reagovat na události, které se vyvolá, když uživatel pracuje s kartami nadřízených členů.

4. Přidat na kartě, který byl vytvořen v předchozím kroku do `ActionBar`.


Následující kód je příkladem přidání karet do aplikace, která používá obslužné rutiny událostí reagovat na změny stavu pomocí těchto kroků: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>Vs obslužné rutiny události ActionBar.ITabListener

Aplikace by měly používat obslužné rutiny událostí a `ActionBar.ITabListener` pro různé scénáře. Obslužné rutiny událostí nabízejí určité množství syntaktické pohodlí; ukládají je nutné vytvořit třídu a implementovat `ActionBar.ITabListener`. Tato výhoda pocházet za cenu &ndash; Xamarin.Android provede Tato transformace pro vás, jednu třídu vytváření a implementaci `ActionBar.ITabListener` za vás. To je vhodná, když má omezený počet karty aplikace. 

Při plánování práce s mnoha karty, nebo sdílení běžné funkce mezi kartami nadřízených členů, může být efektivnější z hlediska paměti a výkon pro vytvoření vlastní třídy, který implementuje `ActionBar.ITabListener`a sdílení jedné instance třídy. Tím se sníží počet GREF společnosti, který používá aplikace pro Xamarin.Android. 



### <a name="backwards-compatibility-for-older-devices"></a>Zpětné kompatibility pro starší zařízení

[Kompatibility aplikace podporu knihovna pro Android v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) zpět porty nadřízených členů karet Android 2.1 (API úrovně 7). Karty jsou dostupné v aplikaci Xamarin.Android, jakmile tato součást byl přidán do projektu.

Pokud chcete použít nadřízených členů, aktivity musí podtřídami `ActionBarActivity` a používat motiv kompatibility aplikace, jak je znázorněno v následující fragment kódu:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Aktivita může získat odkaz na jeho nadřízených členů z `ActionBarActivity.SupportingActionBar` vlastnost. Následující fragment kódu ukazuje příklad nastavení nadřízených členů v aktivitě:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```


## <a name="summary"></a>Souhrn

V této příručce jsme probrali vytvoření záložkách uživatelské rozhraní v Xamarin.Android, pomocí nadřízených členů. Jsme zahrnutých postup přidání karet do nadřízených členů a jak aktivity mohou komunikovat s karta událostí prostřednictvím `ActionBar.ITabListener` rozhraní. Také jsme viděli, jak podporu knihovna pro Android v7 kompatibility aplikace balíčku backports nadřízených členů karty na starší verze systému Android. 


## <a name="related-links"></a>Související odkazy

- [ActionBarTabs (ukázka)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [Panel nástrojů](~/android/user-interface/controls/tool-bar/index.md)
- [Fragmenty](~/android/platform/fragments/index.md)
- [Panel akcí](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Vzor panelu akcí](http://developer.android.com/design/patterns/actionbar.html)
- [Kompatibilita aplikace Android v7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Balíček NuGet Xamarin.Android podpory knihovny v7 kompatibility aplikace](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
