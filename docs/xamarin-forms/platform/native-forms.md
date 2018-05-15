---
title: Nativní formulářů
description: Nativní Forms povolí na stránkách Xamarin.Forms ContentPage odvozené využívat nativní projekty Xamarin.iOS, Xamarin.Android a univerzální platformu Windows (UWP). Nativní projekty spotřebovat odvozené ContentPage stránek, které jsou přímo přidat do projektu, nebo z knihovny .NET standardní standardní knihovny .NET, nebo sdílený projekt. Tento článek vysvětluje, jak využívat odvozené ContentPage stránek, které jsou přímo přidat do nativní projekty a jak se orientovat mezi nimi.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: bb7aa9a7071f9ac7bef0dce5790a3fe74302cfb4
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="native-forms"></a>Nativní formulářů

_Nativní Forms povolí na stránkách Xamarin.Forms ContentPage odvozené využívat nativní projekty Xamarin.iOS, Xamarin.Android a univerzální platformu Windows (UWP). Nativní projekty spotřebovat odvozené ContentPage stránek, které jsou přímo přidat do projektu, nebo z knihovny .NET standardní standardní knihovny .NET, nebo sdílený projekt. Tento článek vysvětluje, jak využívat odvozené ContentPage stránek, které jsou přímo přidat do nativní projekty a jak se orientovat mezi nimi._

Obvykle Xamarin.Forms aplikace obsahuje jednu nebo více stránek, které jsou odvozeny od [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), a tyto stránek jsou sdíleny ve všech platformách v rozhraní .NET standardní projektu knihovny nebo sdílený projekt. Však umožňuje nativní Forms `ContentPage`-odvozené stránky, které chcete přidat do nativní aplikace Xamarin.iOS, Xamarin.Android a UWP přímo. Ve srovnání s s nativní projektem využívat `ContentPage`-odvozené stránky z .NET Standard projektu knihovny nebo sdílené projektu výhod přidání stránky přímo do nativní projektů je, že stránky lze rozšířit pomocí nativní zobrazení. Nativní zobrazení může být název pak v jazyce XAML s `x:Name` a odkazované z modelu code-behind. Další informace o nativní zobrazení najdete v tématu [nativní zobrazení](~/xamarin-forms/platform/native-views/index.md).

Proces pro použití Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránku k nativnímu projektu je následující:

1. Přidejte balíček Xamarin.Forms NuGet k nativnímu projektu.
1. Přidat [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránku a všechny závislosti, k nativnímu projektu.
1. Volání `Forms.Init` metoda.
1. Vytvořit instanci [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky a ho převést na typ odpovídající nativní pomocí jedné z následujících metod rozšíření: `CreateViewController` pro iOS, `CreateFragment` nebo `CreateSupportFragment` pro Android, nebo `CreateFrameworkElement` pro UPW.
1. Přejděte k reprezentaci nativního typu [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránku pomocí nativní navigační rozhraní API.

Xamarin.Forms se musí inicializovat voláním `Forms.Init` metoda předtím, než můžete vytvořit k nativnímu projektu [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky. Výběr, pokud se jedná především závisí na při je nejvýhodnější pro vaše aplikace toku – ho můžete provést při spuštění aplikace, nebo jednoduše před `ContentPage`-odvozené stránky je vytvořený. V tomto článku a doprovodné ukázkové aplikace `Forms.Init` metoda je volána při spuštění aplikace.

> [!NOTE]
> **NativeForms** ukázkové aplikace řešení neobsahuje žádné projekty Xamarin.Forms. Místo toho sestává z projektu Xamarin.iOS, projektu Xamarin.Android a projektu UPW. Každý projekt je nativní projekt, který používá nativní Forms využívat [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky. Neexistuje však žádný důvod, proč nelze využívat nativní projekty `ContentPage`-stránky odvozen od standardní rozhraní .NET projektu knihovny nebo sdílené projektu.

Pokud používáte nativní formulářů, Xamarin.Forms funkce, jako [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/), [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)a modul vazby dat, všechny stále práci.

## <a name="ios"></a>iOS

V systému iOS `FinishedLaunching` potlačení v `AppDelegate` třída je obvykle místní aplikace provést spuštění související úlohy. Je volána poté, co aplikace je spuštěna a je obvykle potlačena za účelem konfigurace hlavní okno a zobrazit řadiče. Následující příklad kódu ukazuje `AppDelegate` – třída v ukázkové aplikaci:

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

- Xamarin.Forms se inicializuje pomocí volání `Forms.Init` metoda.
- Odkaz na `AppDelegate` třída je uložena v `static` `Instance` pole. Toto nastavení slouží k poskytují mechanismus pro jiné třídy pro volání metody definované v `AppDelegate` třídy.
- `UIWindow`, Což je hlavní kontejner pro zobrazení v aplikacích nativní aplikace pro iOS, se vytvoří.
- `PhonewordPage` Třída, která je platformě Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, které jsou definované v jazyce XAML, je vytvořená a převést na `UIViewController` pomocí `CreateViewController` metoda rozšíření.
- `Title` Vlastnost `UIViewController` nastavena, bude se zobrazovat v `UINavigationBar`.
- A `UINavigationController` se vytvoří pro správu hierarchické navigace. `UINavigationController` Třída spravuje zásobníku zobrazení řadičů a `UIViewController` předaný do konstruktoru bude uvedeno původně při `UINavigationController` je načteno.
- `UINavigationController` Instance je nastaven jako nejvyšší úrovně `UIViewController` pro `UIWindow`a `UIWindow` je nastaven jako okno klíče pro aplikace a jsou dostupná.

Jednou `FinishedLaunching` metoda provedlo, uživatelského rozhraní definované v platformě Xamarin.Forms `PhonewordPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

Interakci s uživatelským rozhraním, například klepnutím na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), bude mít za následek obslužné rutiny událostí v `PhonewordPage` provádění kódu. Když uživatel například klepne **historie volání** tlačítko následující obslužné rutiny události se spustí:

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

`NavigateToCallHistoryPage` Metoda převede platformě Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky `UIViewController` s `CreateViewController` metoda rozšíření a nastaví `Title` vlastnost `UIViewController`. `UIViewController` Se pak posune do `UINavigationController` pomocí `PushViewController` metoda. Proto definováno uživatelské rozhraní v platformě Xamarin.Forms `CallHistoryPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

Při `CallHistoryPage` se zobrazí, klepnutím na zadní bude pop šipku `UIViewController` pro `CallHistoryPage` třídy z `UINavigationController`, vrácení uživateli `UIViewController` pro `PhonewordPage` – třída.

## <a name="android"></a>Android

V systému Android `OnCreate` potlačení v `MainActivity` třída je obvykle místní aplikace provést spuštění související úlohy. Následující příklad kódu ukazuje `MainActivity` – třída v ukázkové aplikaci:

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

- Xamarin.Forms se inicializuje pomocí volání `Forms.Init` metoda.
- Odkaz na `MainActivity` třída je uložena v `static` `Instance` pole. Toto nastavení slouží k poskytují mechanismus pro jiné třídy pro volání metody definované v `MainActivity` třídy.
- `Activity` Obsah je nastavený z prostředku rozložení. V ukázkové aplikaci rozložení se skládá z `LinearLayout` obsahující `Toolbar`a `FrameLayout` tak, aby fungoval jako kontejner fragment.
- `Toolbar` Načíst a nastavená na panelu akcí pro `Activity`, a je nastavená název panelu akcí.
- `PhonewordPage` Třída, která je platformě Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, které jsou definované v jazyce XAML, je vytvořená a převést na `Fragment` pomocí `CreateFragment` metoda rozšíření.
- `FragmentManager` Třídy vytvoří a potvrdí transakci, která nahrazuje `FrameLayout` instanci s `Fragment` pro `PhonewordPage` třídy.

Další informace o fragmenty najdete v tématu [fragmenty](~/android/platform/fragments/index.md).

> [!NOTE]
> Kromě `CreateFragment` metoda rozšíření Xamarin.Forms také zahrnuje `CreateSupportFragment` metoda. `CreateFragment` Metoda vytvoří `Android.App.Fragment` , mohou být použity v aplikacích, které cílí 11 rozhraní API a větší. `CreateSupportFragment` Metoda vytvoří `Android.Support.V4.App.Fragment` který lze použít v aplikacích, které cílí na rozhraní API verze starší než 11.

Jednou `OnCreate` metoda provedlo, uživatelského rozhraní definované v platformě Xamarin.Forms `PhonewordPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

Interakci s uživatelským rozhraním, například klepnutím na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), bude mít za následek obslužné rutiny událostí v `PhonewordPage` provádění kódu. Když uživatel například klepne **historie volání** tlačítko následující obslužné rutiny události se spustí:

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

`NavigateToCallHistoryPage` Metoda převede platformě Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky `Fragment` s `CreateFragment` metoda rozšíření a přidá `Fragment` fragment zpět zásobníku. Proto definováno uživatelské rozhraní v platformě Xamarin.Forms `CallHistoryPage` se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

Při `CallHistoryPage` se zobrazí, klepnutím na zadní bude pop šipku `Fragment` pro `CallHistoryPage` v zásobníku back fragment vrácení uživateli `Fragment` pro `PhonewordPage` – třída.

### <a name="enabling-back-navigation-support"></a>Povolení podpory Back navigace

`FragmentManager` Třída má `BackStackChanged` událost, která aktivuje vždy, když se změní obsah fragment back zásobníku. `OnCreate` Metoda v `MainActivity` třída obsahuje anonymní obslužnou rutinu pro tuto událost:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Tuto obslužnou rutinu události zobrazí tlačítko Zpět na panelu akcí za předpokladu, že je jeden nebo více `Fragment` instancí na fragment zpět zásobníku. Odpověď na klepnutím na tlačítko Zpět se zpracovává souborem `OnOptionsItemSelected` přepsání:

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

`OnOptionsItemSelected` Přepsání je volána, když vyberete položku v nabídce Možnosti. Tato implementace automaticky otevře aktuální fragment z back zásobník fragment, za předpokladu, že byla vybrána tlačítko Zpět a existují jeden nebo více `Fragment` instancí na fragment zpět zásobníku.

### <a name="multiple-activities"></a>Více aktivit

Když se aplikace skládá z více aktivit, [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky lze jej vkládat do jednotlivých aktivit. V tomto scénáři `Forms.Init` metoda musí být volán jen v `OnCreate` přepsat prvního `Activity` , vloží platformě Xamarin.Forms `ContentPage`. To však má dopad na následující:

- Hodnota `Xamarin.Forms.Color.Accent` se provedou z `Activity` která volána `Forms.Init` metoda.
- Hodnota `Xamarin.Forms.Application.Current` bude přidružen `Activity` která volána `Forms.Init` metoda.

### <a name="choosing-a-file"></a>Výběr souboru

Při vložení [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, která používá [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) , musí pro podporu HTML "Zvolit soubor" tlačítko, `Activity` muset přepsat `OnActivityResult` Metoda:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

Na UPW, nativního `App` třída je obvykle místní aplikace provést spuštění související úlohy. Xamarin.Forms je obvykle inicializován, v aplikacích pro UPW Xamarin.Forms v `OnLaunched` override v nativní `App` tříd, předat `LaunchActivatedEventArgs` argument `Forms.Init` metoda. Z tohoto důvodu nativních aplikací UWP, které využívají platformě Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránka snadno volat `Forms.Init` metoda z `App.OnLaunched` metoda.

Ve výchozím nastavení nativního `App` třídy spustí `MainPage` třída jako první stránka aplikace. Následující příklad kódu ukazuje `MainPage` – třída v ukázkové aplikaci:

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

`MainPage` Konstruktor provede následující úlohy:

- Je povoleno ukládání do mezipaměti pro stránku, tak, aby nový `MainPage` není vytvořená, když uživatel přejde zpět na stránku.
- Odkaz na `MainPage` třída je uložena v `static` `Instance` pole. Toto nastavení slouží k poskytují mechanismus pro jiné třídy pro volání metody definované v `MainPage` třídy.
- `PhonewordPage` Třída, která je platformě Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, které jsou definované v jazyce XAML, je vytvořená a převést na `FrameworkElement` pomocí `CreateFrameworkElement` metoda rozšíření a potom nastavte jako obsah `MainPage` třídy.

Jednou `MainPage` konstruktor provedlo, uživatelského rozhraní definované v platformě Xamarin.Forms `PhonewordPage` třídy se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

Interakci s uživatelským rozhraním, například klepnutím na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), bude mít za následek obslužné rutiny událostí v `PhonewordPage` provádění kódu. Když uživatel například klepne **historie volání** tlačítko následující obslužné rutiny události se spustí:

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

Navigace ve UWP se obvykle provádí pomocí `Frame.Navigate` metoda, která přebírá `Page` argument. Definuje Xamarin.Forms `Frame.Navigate` metody rozšíření, která přijímá [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené instance stránky. Proto když `NavigateToCallHistoryPage` metoda provádí, uživatelského rozhraní definované v platformě Xamarin.Forms `CallHistoryPage` se zobrazí, jak je znázorněno na následujícím snímku obrazovky:

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

Při `CallHistoryPage` se zobrazí, klepnutím na zadní bude pop šipku `FrameworkElement` pro `CallHistoryPage` z back zásobníku v aplikaci, vrácení uživateli `FrameworkElement` pro `PhonewordPage` – třída.

### <a name="enabling-back-navigation-support"></a>Povolení podpory Back navigace

Na UPW musí aplikace povolit back navigace pro všechny hardwarové a softwarové back tlačítka, mezi velikostem jiné zařízení. To lze provést tak, že zaregistrujete obslužné rutiny události pro `BackRequested` událost, která mohou být prováděny v `OnLaunched` metoda v nativního `App` třídy:

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

Při spuštění aplikace `GetForCurrentView` metoda načte `SystemNavigationManager` přidružené k aktuální zobrazení objektu a pak zaregistruje obslužné rutiny události pro `BackRequested` událostí. Aplikaci jenom obdrží tuto událost, pokud je aplikace popředí a v reakci, volá `OnBackRequested` obslužné rutiny události:

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

`OnBackRequested` Volání obslužné rutiny událostí `GoBack` metoda rámec kořenové aplikace a nastaví `BackRequestedEventArgs.Handled` vlastnost `true` označit událost jako zpracovává. Označit událost jako zpracování může dojít v systému navigace mimo aplikaci (v dané rodině mobilních zařízení) nebo události (na rodiny stolního počítače) je ignorována.

Tato aplikace spoléhá na tlačítko zpět systému podle na telefon, ale rozhodne, zda má být zobrazeno tlačítko Zpět v záhlaví okna na ploše zařízení. Toho dosáhnete pomocí nastavení `AppViewBackButtonVisibility` vlastnost na jednu z `AppViewBackButtonVisibility` hodnot výčtu:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Obslužná rutina události, která se spustí v reakci `Navigated` událost, která iniciovala, aktualizuje viditelnost záhlaví tlačítko Zpět, když dojde k navigaci na stránce. Zajistíte tak, že tlačítko Zpět panelu název se zobrazí, pokud back zásobníku v aplikaci není prázdná, nebo odebrat ze záhlaví, pokud back zásobníku v aplikaci je prázdná.

Další informace o podpoře back navigace na UWP najdete v tématu [navigační historie a zpětně navigace pro aplikace UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Souhrn

Nativní formuláře umožňují Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, které se spotřebovávají nativní projekty Xamarin.iOS, Xamarin.Android a univerzální platformu Windows (UWP). Můžete využívat nativní projekty `ContentPage`-odvozené stránek, které jsou přímo přidat do projektu, nebo z .NET Standard projektu knihovny nebo sdílené projektu. Tento článek vysvětlené využívání `ContentPage`-odvozené stránek, které jsou přímo přidat do nativní projekty a jak se orientovat mezi nimi.


## <a name="related-links"></a>Související odkazy

- [NativeForms (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Nativní zobrazení](~/xamarin-forms/platform/native-views/index.md)
