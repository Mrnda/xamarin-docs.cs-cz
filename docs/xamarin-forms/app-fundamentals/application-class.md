---
title: Aplikace – třída
description: Funkce výchozí třídy aplikace, které může být buď C# nebo XAML
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 5c9eed8f48a40bc7feaadd0c644610f691713e9b
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="app-class"></a>Aplikace – třída

`Application` Základní třída nabízí následující funkce, které jsou zveřejněné v výchozí projekty `App` podtřídami:

* A `MainPage` vlastnost, která je, kde se má nastavit úvodní stránku pro aplikaci.
* Trvalé [ `Properties` slovník](#Properties_Dictionary) pro uložení jednoduché hodnot mezi změny stavu životního cyklu.
* Statického `Current` vlastnost, která obsahuje odkaz na objekt aktuální aplikace.

Taky zpřístupňuje [životního cyklu metody](~/xamarin-forms/app-fundamentals/app-lifecycle.md) například `OnStart`, `OnSleep`, a `OnResume` a také události modální navigace.

V závislosti na šablonu, kterou jste zvolili, `App` třídy mohou být definovány v jedním ze dvou způsobů:

* **C#**, nebo
* **XAML A C#**

Chcete-li vytvořit **aplikace** pomocí XAML, výchozí hodnota **aplikace** třídy je nutné nahradit XAML **aplikace** třídy a přidružené kódu, jak je uvedené v následujícím příkladu kódu:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

Následující příklad kódu ukazuje přidružené kódu:

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

I nastavení [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) vlastnost modelu code-behind musíte také zavolat `InitializeComponent` metoda se načíst a analyzovat přidružené XAML.

## <a name="mainpage-property"></a>Vlastnost MainPage

`MainPage` Vlastnost `Application` třída nastaví kořenovou stránku aplikace.

Například můžete vytvořit logiku v vaší `App` třída pro zobrazení jiné stránky v závislosti na tom, zda uživatel je přihlášen nebo ne.

`MainPage` Musí být vlastnost nastavena `App` konstruktoru,

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Slovník vlastností

`Application` Podtřídami má statického `Properties` slovník, který můžete použít k ukládání dat, zejména pro použití v `OnStart`, `OnSleep`, a `OnResume` metody. To lze přistupovat z kdekoli v kódu Xamarin.Forms pomocí `Application.Current.Properties`.

`Properties` Používá slovník `string` klíče a ukládá `object` hodnotu.

Například můžete nastavit trvalá `"id"` vlastnost kdekoli v kódu (Pokud je položka vybrána na stránce `OnDisappearing` metoda, nebo v `OnSleep` metoda) podobné výjimky:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

V `OnStart` nebo `OnResume` tuto hodnotu pak můžete použít k opětovnému vytvoření možnosti pro uživatele nějakým způsobem metod. `Properties` Úložiště slovníku `object`s proto musíte přetypovat jeho hodnotu před jeho použitím.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Vždy zkontrolujte přítomnost klíče před přístupem k, abychom zabránili jeho neočekávaným chybám.

> [!NOTE]
> `Properties` Slovníku může serializovat jenom primitivní typy pro úložiště. Pokusu o uložení jiné typy (například `List<string>`) může selhat bezobslužně.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Trvalost

`Properties` Slovníku se automaticky uloží do zařízení.
Po návratu aplikace ze na pozadí, nebo i po jeho restartování, bude k dispozici data přidat do slovníku.

Xamarin.Forms 1.4 zavedená další způsob na `Application` třída - `SavePropertiesAsync()` – které lze volat pro proaktivní zachovat `Properties` slovníku. Toto je vám umožní uložit vlastnosti po důležité aktualizace a nikoli riziko je aplikace serializovat se kvůli chybě nebo se ukončená operačního systému.

Můžete najít odkazy na pomocí `Properties` slovníku v **vytváření mobilních aplikací s Xamarin.Forms** sešit kapitolám [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), a [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)a v přidruženém [ukázky](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Třída aplikace

Úplná `Application` implementaci třídy jsou uvedeny níže pro referenci:

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}

```

Tato třída je pak instanci v každém projektu specifické pro platformu a předán `LoadApplication` metodu, která je tam, kde `MainPage` načíst a zobrazit uživateli.
V následujících částech se zobrazí kód pro každou platformu. Nejnovější šablony řešení Xamarin.Forms již obsahovat všechny tento kód předem nakonfigurovat pro vaši aplikaci.


### <a name="ios-project"></a>iOS projektu

IOS `AppDelegate` třídy dědí vlastnosti z `FormsApplicationDelegate`. Má následující vlastnosti:

* Volání `LoadApplication` s instancí `App` třídy.

* Vždy vrátí `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Projekt pro Android

Android `MainActivity` nyní dědí z `FormsApplicationActivity`. V `OnCreate` přepsat `LoadApplication` metoda je volána s instancí `App` třídy.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> Je novější [ `FormsAppCompatActivity` ](~/xamarin-forms/platform/android/appcompat.md) základní třídu, která umožňuje lepší podpory Android materiálu návrhu.
> To se stane výchozí šablonu pro Android v budoucnosti, ale můžete postupovat podle [tyto pokyny](~/xamarin-forms/platform/android/appcompat.md) k aktualizaci existující aplikace pro Android.

### <a name="universal-windows-project-uwp-for-windows-10"></a>Univerzální projekt pro Windows (UWP) pro Windows 10

V tématu [projekty instalace Windows](~/xamarin-forms/platform/windows/installation/index.md) informace o podpoře UWP v Xamarin.Forms.

Na hlavní stránku projektu UPW musí dědit z `WindowsPage`. To znamená XAML a C# pro `MainPage` odkaz `FormsApplicationPage` třídy, jak je vidět.

XAML používá vlastní obor názvů tak, aby kořenový element odráží `FormsApplicationPage` třídy:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Konstrukce codebehind C# musí volat `LoadApplication` k vytvoření instance vaše Xamarin.Forms `App`. Všimněte si, že je dobrým zvykem explicitně ke kvalifikaci použít obor názvů aplikace `App` vzhledem k tomu, že aplikace UWP také mají svůj vlastní `App` nezávislé na platformě Xamarin.Forms třídy.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Všimněte si, že `Forms.Init()` musí být volán v **App.xaml.cs** kolem řádku 63.
