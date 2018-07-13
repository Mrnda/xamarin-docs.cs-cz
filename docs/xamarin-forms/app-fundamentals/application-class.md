---
title: Třídu aplikace Xamarin.Forms
description: Tento článek vysvětluje funkce výchozí třídu aplikace, která zahrnuje vlastnosti nastavit na úvodní stránku pro aplikaci, a trvalé slovník pro ukládání jednoduchých hodnot mezi změnami stavu životního cyklu.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 6de4380f2ce2d19df4ff912b7c86b75ca9e7821b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999026"
---
# <a name="xamarinforms-app-class"></a>Třídu aplikace Xamarin.Forms

`Application` Základní třídy nabízí následující funkce, které jsou přístupné na výchozí projekty `App` podtřídy:

* A `MainPage` vlastnost, která je tam, kde nastavit počáteční stránku pro aplikaci.
* Trvalé [ `Properties` slovníku](#Properties_Dictionary) k ukládání jednoduchých hodnot mezi změnami stavu životního cyklu.
* Statický `Current` vlastnost, která obsahuje odkaz na aktuální objekt aplikace.

Také poskytuje [životního cyklu metody](~/xamarin-forms/app-fundamentals/app-lifecycle.md) například `OnStart`, `OnSleep`, a `OnResume` a také modální navigační události.

V závislosti na šablonu, kterou jste zvolili, `App` třídy může být definován v jednom ze dvou způsobů:

* **C#**, nebo
* **XAML A C#**

Vytvoření **aplikace** pomocí XAML, výchozí **aplikace** třídy je nutné nahradit XAML **aplikace** třídy a přidružený kód na pozadí, jak je znázorněno v následujícím příkladu kódu:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

Následující příklad kódu ukazuje související kódu:

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

A také nastavení [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) vlastnost modelu code-behind musíte také zavolat `InitializeComponent` metoda načíst a analyzovat související XAML.

## <a name="mainpage-property"></a>Vlastnost MainPage

`MainPage` Vlastnost `Application` třída nastaví kořenové stránky aplikace.

Můžete například vytvořit logiku v vaše `App` třídy zobrazení jiné stránky v závislosti na tom, jestli je uživatel přihlášen či nikoli.

`MainPage` Vlastnost by měla být nastavena v `App` konstruktoru,

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

`Application` Podtřídy má statickou `Properties` slovník, který slouží k ukládání dat, zejména pro použití v `OnStart`, `OnSleep`, a `OnResume` metody. To je přístupná z libovolné místo v kódu Xamarin.Forms pomocí `Application.Current.Properties`.

`Properties` Používá slovníku `string` klíče a ukládá `object` hodnotu.

Například můžete nastavit trvalá `"id"` vlastnost kdekoli ve vašem kódu (když je položka vybrána na stránce `OnDisappearing` metodu, nebo v `OnSleep` – metoda) tímto způsobem:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

V `OnStart` nebo `OnResume` metody, které pak můžete tuto hodnotu znovu vytvořit uživatelské prostředí nějakým způsobem. `Properties` Úložiště slovníku `object`s, takže je třeba přetypovat jeho hodnotu před jeho použitím.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Vždy zkontrolujte přítomnost klíče před přístupem k zabránili neočekávaným chybám.

> [!NOTE]
> `Properties` Slovník může serializovat pouze primitivní typy pro úložiště. Pokusu o uložení jiných typů (například `List<string>`) může selhat, bezobslužně.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Trvalost

`Properties` Slovníku se ukládá do zařízení automaticky.
Data přidaná do slovníku bude k dispozici, pokud aplikace vrací z na pozadí nebo i po restartování.

Xamarin.Forms 1.4 představen další metodu `Application` class - `SavePropertiesAsync()` – který dá zavolat, aby proaktivně zachování `Properties` slovníku. Toto je bylo možné uložit vlastnosti po důležité aktualizace, spíše než rizika je neprovedení serializovat si kvůli chybovému ukončení nebo ukončuje podle operačního systému.

Můžete najít odkazy na použití `Properties` slovníku **vytváření mobilních aplikací pomocí Xamarin.Forms** kapitoly knihy [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), a [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)a v souvisejících [ukázky](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Třída aplikace

Kompletní `Application` implementace třídy je uveden níže pro referenci:

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

Tato třída se pak vytvořit instanci v každém projektu konkrétní platformy a předat `LoadApplication` metodu, která je tam, kde `MainPage` se načte a zobrazí uživateli.
V následujících částech se zobrazí kód pro každou platformu. Nejnovější šablony řešení Xamarin.Forms již obsahují celý tento kód, předem nakonfigurovat pro vaši aplikaci.


### <a name="ios-project"></a>iOS projektu

IOS `AppDelegate` třída dědí z `FormsApplicationDelegate`. Má následující vlastnosti:

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

Android `MainActivity` dědí z `FormsAppCompatActivity`. V `OnCreate` přepsat `LoadApplication` metoda je volána s instancí `App` třídy.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Projekt univerzální Windows (UPW) pro Windows 10

Zobrazit [projekty instalace Windows](~/xamarin-forms/platform/windows/installation/index.md) informace o podpoře UWP v Xamarin.Forms.

Hlavní stránka v projektu UWP by měla dědit z `WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Konstrukce codebehind C# musí volat `LoadApplication` vytvořit instanci vaší Xamarin.Forms `App`. Všimněte si, že je dobrým zvykem explicitně používání oboru názvů aplikací k získání způsobilosti `App` vzhledem k tomu, že aplikace UPW také mají svůj vlastní `App` třídy, které nesouvisí se Xamarin.Forms.

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

Všimněte si, že `Forms.Init()` musí být volána **App.xaml.cs** kolem řádku 63.
