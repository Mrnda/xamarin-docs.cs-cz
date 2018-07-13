---
title: Xamarin.Forms v Xamarin nativní projekty
description: Tento článek vysvětluje, jak využívat odvozené ContentPage stránek, které jsou přímo přidány do nativní projekty Xamarin a jak k navigaci mezi nimi.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 65bb3fa070c082fa6c6c489e326a870a80fb9502
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997509"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.Forms v Xamarin nativní projekty

_Nativní formuláře povolit odvozené Xamarin.Forms ContentPage stránkám využívat nativní projekty Xamarin.iOS, Xamarin.Android a univerzální platformu Windows (UPW). Nativní projekty mohou využívat odvozené ContentPage stránek, které jsou přímo přidat do projektu, nebo z knihovny .NET Standard, .NET Standard knihovny nebo sdíleného projektu. Tento článek vysvětluje, jak využívat odvozené ContentPage stránky, které jsou přímo přidat do nativních projektů a jak k navigaci mezi nimi._

Obvykle obsahuje jeden nebo více stránek, které jsou odvozeny z aplikace Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), a tyto stránky jsou sdíleny ve všech platformách ve sdíleném projektu a projekt knihovny .NET Standard. Nicméně umožňuje nativní formuláře `ContentPage`-odvozené stránky, které se přidávají přímo do nativní aplikace pro Xamarin.iOS, Xamarin.Android a UPW. Ve srovnání s tím, že nativní projekt využívat `ContentPage`-odvozené stránky z projekt knihovny .NET Standard nebo sdíleného projektu výhod přidávání stránek přímo do nativních projektů je, že je možné rozšířit na stránkách s nativní zobrazení. Nativní zobrazení může být název pak v XAML s `x:Name` a odkazované z modelu code-behind. Další informace o nativní zobrazení najdete v tématu [nativní zobrazení](~/xamarin-forms/platform/native-views/index.md).

Proces určená pro Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky v nativní projekt vypadá takto:

1. K nativnímu projektu přidejte balíček Xamarin.Forms NuGet.
1. Přidat [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránku a všechny závislosti, do nativního projektu.
1. Volání `Forms.Init` metody.
1. Vytvořit instanci [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky a převést na odpovídající nativní typ pomocí jedné z následujících metod rozšíření: `CreateViewController` pro iOS, `CreateFragment` nebo `CreateSupportFragment` pro Android, nebo `CreateFrameworkElement` pro UPW.
1. Přejděte na nativní typ reprezentace [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky pomocí navigace nativní rozhraní API.

Xamarin.Forms musí být inicializován pomocí volání `Forms.Init` metoda před nativní projekt můžete vytvořit [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky. Výběr, pokud se jedná hlavně při je nejpohodlnější ve svém toku aplikace závisí na – to lze provést při spuštění aplikace, nebo jen před `ContentPage`-odvozené stránky je vytvořený. V tomto článku a související ukázkové aplikace `Forms.Init` metoda je volána při spuštění aplikace.

> [!NOTE]
> **NativeForms** ukázkové aplikace řešení neobsahuje žádné projekty Xamarin.Forms. Místo toho ji se skládá z projektu Xamarin.iOS, Xamarin.Android projekt a projekt UPW. Každý projekt je nativní projekt, který používá nativní formuláře využívat [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky. Neexistuje však žádný důvod, proč nelze využívat nativní projekty `ContentPage`-stránky odvozen ze sdíleného projektu a projekt knihovny .NET Standard.

Pokud používáte nativní formuláře, Xamarin.Forms funkce, jako [ `DependencyService` ](xref:Xamarin.Forms.DependencyService), [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)a modul vazby dat, všechny funkční.

## <a name="ios"></a>iOS

V Iosu `FinishedLaunching` přepsat v `AppDelegate` třída je obvykle místem, kde můžete provádět aplikace po spuštění úlohy související se službou. Je volána poté, co aplikace byla spuštěna a je obvykle potlačena za účelem konfigurace hlavní okno a zobrazení kontroleru. Následující příklad kódu ukazuje `AppDelegate` třídy v ukázkové aplikaci:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

`FinishedLaunching` Metoda provádí následující úlohy:

- Xamarin.Forms je inicializována pomocí volání `Forms.Init` metody.
- Odkaz na `AppDelegate` třída je uložena v `static` `Instance` pole. Toto je mechanismus pro jiné třídy k volání metody definované v `AppDelegate` třídy.
- `UIWindow`, Což je hlavní kontejner pro zobrazení v nativních aplikací pro iOS aplikací, je vytvořen.
- `PhonewordPage` Třídy, která je Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky, které jsou definovány v XAML, je vytvořen a převést na `UIViewController` pomocí `CreateViewController` – metoda rozšíření.
- `Title` Vlastnost `UIViewController` nastavena, který bude zobrazen na `UINavigationBar`.
- A `UINavigationController` se vytvoří pro správu hierarchická navigace. `UINavigationController` Třída spravuje zásobníku kontrolery zobrazení a `UIViewController` předaná do konstruktoru se zobrazí nejprve při `UINavigationController` je načteno.
- `UINavigationController` Instance je nastavená na nejvyšší úrovni `UIViewController` pro `UIWindow`a `UIWindow` je nastaven jako klíče v okně aplikace a jsou dostupná.

Jednou `FinishedLaunching` po provedení metody, uživatelského rozhraní definované v Xamarin.Forms `PhonewordPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

Interakce s uživatelským rozhraním, například po klepnutí na [ `Button` ](xref:Xamarin.Forms.Button), bude mít za následek obslužné rutiny událostí v `PhonewordPage` provádění kódu. Například po klepnutí **historie volání** spustit tlačítko, následující obslužnou rutinu události:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance` Pole `AppDelegate.NavigateToCallHistoryPage` metoda k vyvolání, které je znázorněno v následujícím příkladu kódu:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage` Metoda převede Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránku `UIViewController` s `CreateViewController` – metoda rozšíření a sady `Title` vlastnost `UIViewController`. `UIViewController` Poté vloženo do `UINavigationController` podle `PushViewController` metody. Proto uživatelského rozhraní definované v Xamarin.Forms `CallHistoryPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

Když `CallHistoryPage` se zobrazí, klepněte back šipku zobrazte `UIViewController` pro `CallHistoryPage` třídy z `UINavigationController`, stávající uživatel k `UIViewController` pro `PhonewordPage` třídy.

## <a name="android"></a>Android

V systému Android `OnCreate` přepsat v `MainActivity` třída je obvykle místem, kde můžete provádět aplikace po spuštění úlohy související se službou. Následující příklad kódu ukazuje `MainActivity` třídy v ukázkové aplikaci:

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

`OnCreate` Metoda provádí následující úlohy:

- Xamarin.Forms je inicializována pomocí volání `Forms.Init` metody.
- Odkaz na `MainActivity` třída je uložena v `static` `Instance` pole. Toto je mechanismus pro jiné třídy k volání metody definované v `MainActivity` třídy.
- `Activity` Obsah je nastavený z prostředku rozložení. V ukázkové aplikaci rozložení se skládá z `LinearLayout` , která obsahuje `Toolbar`a `FrameLayout` tak, aby fungoval jako kontejner fragment.
- `Toolbar` Načíst a nastavená na panelu akcí pro `Activity`, a nastavte název panelu akcí.
- `PhonewordPage` Třídy, která je Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky, které jsou definovány v XAML, je vytvořen a převést na `Fragment` pomocí `CreateFragment` – metoda rozšíření.
- `FragmentManager` Třídy vytvoří a potvrzení transakce, který nahrazuje `FrameLayout` instanci s `Fragment` pro `PhonewordPage` třídy.

Další informace o fragmenty najdete v tématu [fragmenty](~/android/platform/fragments/index.md).

> [!NOTE]
> Kromě `CreateFragment` rozšiřující metoda Xamarin.Forms také zahrnuje `CreateSupportFragment` metoda. `CreateFragment` Metoda vytvoří `Android.App.Fragment` , které mohou být použity v aplikacích, které jsou cíleny na rozhraní API 11 a vyšší. `CreateSupportFragment` Metoda vytvoří `Android.Support.V4.App.Fragment` , který můžete použít v aplikacích, které jsou cíleny na rozhraní API verze starší než 11.

Jednou `OnCreate` po provedení metody, uživatelského rozhraní definované v Xamarin.Forms `PhonewordPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

Interakce s uživatelským rozhraním, například po klepnutí na [ `Button` ](xref:Xamarin.Forms.Button), bude mít za následek obslužné rutiny událostí v `PhonewordPage` provádění kódu. Například po klepnutí **historie volání** spustit tlačítko, následující obslužnou rutinu události:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance` Pole `MainActivity.NavigateToCallHistoryPage` metoda k vyvolání, které je znázorněno v následujícím příkladu kódu:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

`NavigateToCallHistoryPage` Metoda převede Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránku `Fragment` s `CreateFragment` metodu rozšíření a přidá `Fragment` fragment zpět zásobník. Proto uživatelského rozhraní definované v Xamarin.Forms `CallHistoryPage` se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

Když `CallHistoryPage` se zobrazí, klepněte back šipku zobrazte `Fragment` pro `CallHistoryPage` ze zadní zásobníku fragment vrátí uživateli `Fragment` pro `PhonewordPage` třídy.

### <a name="enabling-back-navigation-support"></a>Povolení podpory zpětné navigace

`FragmentManager` Třída nemá `BackStackChanged` událostí, který se aktivuje pokaždé, když se změní obsah zpět zásobník fragment. `OnCreate` Metodu `MainActivity` třída obsahuje obslužnou rutinu anonymní události pro tuto událost:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Tato obslužná rutina události se zobrazí tlačítko Zpět na panelu akcí za předpokladu, že je jeden nebo více `Fragment` instancí na fragment zpět zásobník. Zpracovává odpověď klepnutím na tlačítko Zpět `OnOptionsItemSelected` přepsat:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

`OnOptionsItemSelected` Přepsání je volána, když je vybraná položka v nabídce Možnosti. Tato implementace zobrazí aktuální fragment ze zásobníku back fragment, za předpokladu, že vybral tlačítko Zpět a jsou jeden nebo více `Fragment` instancí na fragment zpět zásobník.

### <a name="multiple-activities"></a>Více aktivit

Když se aplikace skládá z více aktivit [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky může být vložen do každé aktivity. V tomto scénáři `Forms.Init` metoda musí být volán jen v `OnCreate` přepsat prvního `Activity` , která vloží Xamarin.Forms `ContentPage`. Ale tato akce nemá vliv následující:

- Hodnota `Xamarin.Forms.Color.Accent` vytvářena z `Activity` , který volá `Forms.Init` metody.
- Hodnota `Xamarin.Forms.Application.Current` přidruží `Activity` , který volá `Forms.Init` metoda.

### <a name="choosing-a-file"></a>Výběr souboru

Při vkládání [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránka, která používá [ `WebView` ](xref:Xamarin.Forms.WebView) , který potřebuje k podpoře HTML "Zvolte soubor" tlačítko, `Activity` bude nutné přepsat `OnActivityResult` Metoda:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

Na UPW, nativní `App` třída je obvykle místem, kde můžete provádět aplikace po spuštění úlohy související se službou. Xamarin.Forms je obvykle inicializován, v aplikacích UPW Xamarin.Forms v `OnLaunched` override v nativní `App` třídy k předání `LaunchActivatedEventArgs` argument `Forms.Init` metoda. Z tohoto důvodu nativních aplikací UWP, které využívají Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránka nejsnadněji volat `Forms.Init` metodu z `App.OnLaunched` metody.

Ve výchozím nastavení, nativní `App` třídy spustí `MainPage` třídu jako první stránka aplikace. Následující příklad kódu ukazuje `MainPage` třídy v ukázkové aplikaci:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

`MainPage` Konstruktor provádí následující úlohy:

- Je povoleno ukládání do mezipaměti pro stránku, aby nový `MainPage` není vytvořen, když uživatel přejde zpět na stránku.
- Odkaz na `MainPage` třída je uložena v `static` `Instance` pole. Toto je mechanismus pro jiné třídy k volání metody definované v `MainPage` třídy.
- `PhonewordPage` Třídy, která je Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky, které jsou definovány v XAML, je vytvořen a převést na `FrameworkElement` pomocí `CreateFrameworkElement` – metoda rozšíření a pak nastavte jako obsah `MainPage` třídy.

Jednou `MainPage` po provedení konstruktoru, uživatelského rozhraní definované v Xamarin.Forms `PhonewordPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/uwp-phonewordpage.png "UPW PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "PhonewordPage UPW")

Interakce s uživatelským rozhraním, například po klepnutí na [ `Button` ](xref:Xamarin.Forms.Button), bude mít za následek obslužné rutiny událostí v `PhonewordPage` provádění kódu. Například po klepnutí **historie volání** spustit tlačítko, následující obslužnou rutinu události:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance` Pole `MainPage.NavigateToCallHistoryPage` metoda k vyvolání, které je znázorněno v následujícím příkladu kódu:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

Navigace v UPW se obvykle provádí pomocí `Frame.Navigate` metoda, která přijímá `Page` argument. Definuje Xamarin.Forms `Frame.Navigate` rozšiřující metoda, která přijímá [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené instance stránky. Proto, když `NavigateToCallHistoryPage` metoda spustí, uživatelského rozhraní definované v Xamarin.Forms `CallHistoryPage` se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/uwp-callhistorypage.png "UPW CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "CallHistoryPage UPW")

Když `CallHistoryPage` se zobrazí, klepněte back šipku zobrazte `FrameworkElement` pro `CallHistoryPage` ze zásobníku zpět v aplikaci, vrací uživateli `FrameworkElement` pro `PhonewordPage` třídy.

### <a name="enabling-back-navigation-support"></a>Povolení podpory zpětné navigace

Na UPW musí aplikace umožňují zpětné navigace pro všechny hardwarové a softwarové back tlačítka, v různých zařízeních. Toho můžete docílit tak, že zaregistrujete obslužná rutina události `BackRequested` událost, která je možné provádět `OnLaunched` metoda nativního `App` třídy:

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

Při spuštění aplikace `GetForCurrentView` načte metodu `SystemNavigationManager` spojené s aktuálním zobrazením objektu a pak zaregistruje obslužnou rutinu události pro `BackRequested` událostí. Aplikace obdrží tuto událost pouze, pokud je aplikace popředí a v odpovědi, volá `OnBackRequested` obslužné rutiny události:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

`OnBackRequested` Volání obsluhy události `GoBack` metoda u kořenového rámce aplikace a nastaví `BackRequestedEventArgs.Handled` vlastnost `true` k označení události jako zpracování. K označení události jako zpracované může vést systému navigaci pryč z aplikace (v řadě mobilní zařízení) nebo ignorování událostí (v desktopové zařízení řady).

Aplikace závisí na systému k dispozici tlačítko Zpět na telefonu, ale rozhodne, zda se má zobrazit tlačítko Zpět v záhlaví okna na desktopových zařízeních. Toho dosáhnete pomocí nastavení `AppViewBackButtonVisibility` vlastnost na jednu z `AppViewBackButtonVisibility` hodnot výčtu:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Obslužnou rutinu události, která je provést v reakci `Navigated` událost, když dojde k navigaci na stránce aktualizuje viditelnost záhlaví tlačítko Zpět. Tím se zajistí, že tlačítko Zpět. název panelu je viditelná v případě zásobníku zpět v aplikaci není prázdný, nebo odebrán z záhlaví okna, je-li v aplikaci zpět zásobník je prázdný.

Další informace o podpoře zpětné navigace na UPW, naleznete v tématu [historii pro navigaci a zpětné navigace pro aplikace pro UPW](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Souhrn

Nativní formuláře umožňují Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky, které využívat nativní projekty Xamarin.iOS, Xamarin.Android a univerzální platformu Windows (UPW). Můžete využívat nativní projekty `ContentPage`-odvozené stránky, které jsou přímo přidat do projektu, nebo ze sdíleného projektu a projekt knihovny .NET Standard. Tento článek vysvětlil, jak využívat `ContentPage`-odvozené stránky, které jsou přímo přidat do nativní projekty a jak k navigaci mezi nimi.


## <a name="related-links"></a>Související odkazy

- [NativeForms (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Nativní zobrazení](~/xamarin-forms/platform/native-views/index.md)
