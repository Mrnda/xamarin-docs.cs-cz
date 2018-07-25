---
title: Podrobně o infrastruktuře Xamarin.Forms
description: Tento článek zkoumá základy vývoje aplikací pomocí Xamarin.Forms. Probíraná témata zahrnuté anatomie aplikace Xamarin.Forms, architektury a základní informace o aplikaci a uživatelské rozhraní.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 7eff7f4413b533caadcf2aa8b5eed8c4ab65449d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242222"
---
# <a name="xamarinforms-deep-dive"></a>Podrobně o infrastruktuře Xamarin.Forms

V [Xamarin.Forms Quickstart](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md), kterou Phoneword byla aplikace vytvořena. Tento článek obsahuje přehled co byl sestaven získáte informace o základní informace o fungování aplikací Xamarin.Forms.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Úvod do sady Visual Studio

Visual Studio je výkonné integrované vývojové prostředí společnosti Microsoft. Nabízí plně integrovaná vizuálního návrháře, kompletní nástroje pro refaktoring textového editoru, prohlížeči sestavení, integrace zdrojového kódu a další. Tento článek se zaměřuje na některé základní funkce sady Visual Studio pomocí modulu plug-in Xamarin.

Visual Studio slouží k uspořádání kódu do *řešení* a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace, podpůrné knihovny, testovací aplikace a další. Phoneword aplikace se skládá z jednoho řešení, který obsahuje čtyři projekty, jak je znázorněno na následujícím snímku obrazovky.

![](deepdive-images/vs/solution.png "Průzkumník řešení sady Visual Studio")

Projekty jsou:

- Phoneword – tento projekt je projekt .NET Standard knihovny, který obsahuje všechny sdílenému kódu a sdílené uživatelské rozhraní.
- Phoneword.Android – tento projekt obsahuje konkrétní kódu pro Android a je vstupním bodem pro aplikaci pro Android.
- Phoneword.iOS – tento projekt obsahuje iOS konkrétního kódu a je vstupním bodem pro aplikace pro iOS.
- Phoneword.UWP – tento projekt obsahuje určitý kód pro univerzální platformu Windows (UPW) a je vstupním bodem pro aplikaci pro UPW.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomie aplikace Xamarin.Forms

Následující snímek obrazovky ukazuje Phoneword .NET Standard projektu knihovny obsahu v sadě Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Obsah Phoneword .NET Standard projektu")

Projekt má **závislosti** uzel, který obsahuje **NuGet** a **SDK** uzly. **NuGet** uzel obsahuje balíček Xamarin.Forms NuGet, která byla přidána do projektu, a **SDK** obsahuje uzel `NETStandard.Library` Microsoft.aspnetcore.all odkazující na kompletní sadu balíčků NuGet které definují .NET Standard.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Úvod do sady Visual Studio pro Mac

Visual Studio for Mac je zdarma, open source prostředí IDE podobný sady Visual Studio. Nabízí plně integrovaná vizuálního návrháře, kompletní nástroje pro refaktoring textového editoru, prohlížeči sestavení, integrace zdrojového kódu a další. Další informace o sadě Visual Studio pro Mac najdete v tématu [představení sady Visual Studio pro Mac](/visualstudio/mac/).

Visual Studio pro Mac odpovídá uspořádání kódu do sady Visual Studio praxe *řešení* a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace, podpůrné knihovny, testovací aplikace a další. Phoneword aplikace se skládá z jednoho řešení, který obsahuje tři projekty, jak je znázorněno na následujícím snímku obrazovky.

![](deepdive-images/xs/solution.png "Visual Studio pro Mac podokno řešení")

Projekty jsou:

- Phoneword – tento projekt je projekt .NET Standard knihovny, který obsahuje všechny sdílenému kódu a sdílené uživatelské rozhraní.
- Phoneword.Droid – tento projekt obsahuje konkrétní kódu pro Android a je vstupním bodem pro aplikace pro Android.
- Phoneword.iOS – tento projekt obsahuje iOS konkrétního kódu a je vstupním bodem pro aplikace pro iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomie aplikace Xamarin.Forms

Následující snímek obrazovky ukazuje Phoneword .NET Standard projektu knihovny obsahu v sadě Visual Studio pro Mac:

![](deepdive-images/xs/library-project.png "Obsah projektu standardní knihovny .NET Phoneword")

Projekt má **závislosti** uzel, který obsahuje **NuGet** a **SDK** uzly. **NuGet** uzel obsahuje balíček Xamarin.Forms NuGet, která byla přidána do projektu, a **SDK** obsahuje uzel `NETStandard.Library` Microsoft.aspnetcore.all odkazující na kompletní sadu balíčků NuGet které definují .NET Standard.

-----

Projekt se také skládá počet souborů:

- **App.XAML** – značky XAML `App` třídu, která definuje slovník prostředků pro aplikaci.
- **App.XAML.cs** – kódu pro `App` třídu, která odpovídá za vytvoření instance první stránka, která se zobrazí v aplikaci na jednotlivých platformách a pro zpracování událostí životního cyklu aplikace.
- **IDialer.cs** – `IDialer` rozhraní, které určuje, že `Dial` v jakékoli implementaci tříd musí být zadaná metoda.
- **MainPage.xaml** – značky XAML `MainPage` třídu, která definuje uživatelské rozhraní pro stránky zobrazené při spuštění aplikace.
- **MainPage.xaml.cs** – kódu pro `MainPage` třídu, která obsahuje obchodní logiku, která se spustí, až uživatel pracuje se na stránce.
- **PhoneTranslator.cs** – obchodní logiky, který je zodpovědný za převod aplikace word telefonní na telefonní číslo, které se vyvolá z **MainPage.xaml.cs**.

Další informace o anatomií aplikaci Xamarin.iOS, naleznete v tématu [anatomie aplikace pro Xamarin.iOS](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Další informace o anatomií aplikace pro Xamarin.Android, naleznete v tématu [anatomie aplikace Xamarin.Android](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Architektura a principy aplikací

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aplikace Xamarin.Forms je navržený stejným způsobem jako tradiční aplikace pro víc platforem. Sdílený kód je obvykle umístěn v knihovně .NET Standard a specifické pro platformu aplikace spotřebovávat sdílený kód. Následující diagram znázorňuje základní informace o této relace pro aplikaci Phoneword:

![](deepdive-images/vs/architecture.png "Architektura Phoneword")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aplikace Xamarin.Forms je navržený stejným způsobem jako tradiční aplikace pro víc platforem. Sdílený kód je obvykle umístěn v knihovně .NET Standard a specifické pro platformu aplikace spotřebovávat sdílený kód. Následující diagram znázorňuje základní informace o této relace pro aplikaci Phoneword:

![](deepdive-images/xs/architecture.png "Architektura Phoneword")

-----

Pokud chcete maximalizovat opětovné použití kódu po spuštění, aplikace Xamarin.Forms mají jednu třídu s názvem `App` , který je zodpovědný za vytváření instancí první stránka, která se zobrazí v aplikaci na jednotlivé platformy, jak je znázorněno v následujícím příkladu kódu:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Phoneword
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new MainPage();
        }
        ...
    }
}
```

Tento kód nastaví `MainPage` vlastnost `App` novou instanci třídy [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) třídy. Kromě toho [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) atribut zapne kompilátor XAML tak, aby přímo do jazyka intermediate language se zkompilovat XAML. Další informace najdete v tématu [kompilace XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Spouštění aplikací na jednotlivých platformách

### <a name="ios"></a>iOS

Ke spuštění úvodní stránku Xamarin.Forms v Iosu, projekt Phoneword.iOS zahrnuje `AppDelegate` třídu odvozenou od `FormsApplicationDelegate` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
namespace Phoneword.iOS
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init ();
            LoadApplication (new App ());
            return base.FinishedLaunching (app, options);
        }
    }
}
```

`FinishedLaunching` Přepsání inicializuje rozhraní Xamarin.Forms pomocí volání `Init` metody. To způsobí, že implementace Xamarin.Forms, které mají být načteny v aplikaci předtím, než je ve volání nastavení kontroleru zobrazení kořenové iOS `LoadApplication` metody.

### <a name="android"></a>Android

Ke spuštění úvodní stránku Xamarin.Forms v Androidu, Phoneword.Droid projekt zahrnuje kód, který vytvoří `Activity` s `MainLauncher` atribut s aktivitou dědění z `FormsAppCompatActivity` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword", 
              Icon = "@mipmap/icon", 
              Theme = "@style/MainTheme", 
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(bundle);
            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` Přepsání inicializuje rozhraní Xamarin.Forms pomocí volání `Init` metody. To způsobí, že implementace Xamarin.Forms, které mají být načteny v aplikaci před načtením aplikace Xamarin.Forms s Androidem. Kromě toho `MainActivity` třída uchovává odkaz na sebe sama v `Instance` vlastnost. `Instance` Vlastnost se označuje jako místní kontext a odkazuje `PhoneDialer` třídy.

## <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

V aplikacích pro univerzální platformu Windows (UPW) `Init` vyvolat metodu, která inicializuje rozhraní Xamarin.Forms pomocí `App` třídy:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

To způsobí, že implementace Xamarin.Forms, které mají být načteny v aplikaci UWP. Úvodní stránka Xamarin.Forms je spouštěn `MainPage` třídy, jak je ukázáno v následujícím příkladu kódu:

```csharp
namespace Phoneword.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Phoneword.App());
        }
    }
}
```

Načte se aplikace Xamarin.Forms s `LoadApplication` metody.

> [!NOTE]
> Univerzální aplikace pro platformu Windows (UPW) mohou být vytvořené s Xamarin.Forms, ale pouze pomocí sady Visual Studio na Windows.

## <a name="user-interface"></a>Uživatelské rozhraní

Existují čtyři hlavní ovládací prvek skupiny použité k vytvoření uživatelského rozhraní aplikace Xamarin.Forms.

1. **Stránky** – stránky Xamarin.Forms představují obrazovky multiplatformní mobilní aplikace. Aplikace Phoneword používá [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) třídy zobrazíte na jedné obrazovce. Další informace o stránkách naleznete v tématu [Xamarin.Forms stránky](~/xamarin-forms/user-interface/controls/pages.md).
1. **Rozložení** – rozložení Xamarin.Forms jsou kontejnery, které umožňuje sestavit zobrazení do logické struktury. Aplikace Phoneword používá [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) třídy k uspořádání ovládacích prvků ve vodorovné zásobníku. Další informace o rozložení najdete v tématu [rozložení Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Zobrazení** – zobrazení Xamarin.Forms jsou ovládací prvky zobrazí v uživatelském rozhraní, jako je například popisky, tlačítka a textová vstupní pole. Aplikace Phoneword používá [ `Label` ](xref:Xamarin.Forms.Label), [ `Entry` ](xref:Xamarin.Forms.Entry), a [ `Button` ](xref:Xamarin.Forms.Button) ovládacích prvků. Další informace o zobrazeních najdete v tématu [zobrazení Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Buňky** – Xamarin.Forms buňky jsou specializované prvky použitá pro položky v seznamu a popisují, jak má být vykreslena každou položku v seznamu. Všechny buňky použití Phoneword, které se aplikace nepoužívá. Další informace o buňky, naleznete v tématu [Xamarin.Forms buňky](~/xamarin-forms/user-interface/controls/cells.md).

Za běhu každý ovládací prvek se namapují na ekvivalentní nativní, což je, co bude vykreslen.

Když aplikace Phoneword je spouštěn na libovolné platformě, zobrazí se na jedné obrazovce, která odpovídá [ `Page` ](xref:Xamarin.Forms.Page) v Xamarin.Forms. A `Page` představuje *skupinu ViewGroup* v Androidu, *kontroler zobrazení* v Iosu, nebo *stránky* na univerzální platformu Windows. Phoneword aplikace také vytvoří instanci [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objekt, který reprezentuje `MainPage` třídy, jehož značky XAML je znázorněno v následujícím příkladu kódu:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                         xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                         x:Class="Phoneword.MainPage">
        ...
        <StackLayout>
            <Label Text="Enter a Phoneword:" />
            <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
            <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
            <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
</ContentPage>
```

`MainPage` Třídy používá [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ovládací prvek automaticky uspořádat ovládací prvky na obrazovce bez ohledu na velikost obrazovky. Každý podřízený prvek je umístěný za druhým v pořadí, ve kterém byly přidány svisle. `StackLayout` Obsahuje ovládací prvek [ `Label` ](xref:Xamarin.Forms.Label) ovládacího prvku k zobrazení textu na stránce [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku tak, aby přijímal textový vstup a dva [ `Button` ](xref:Xamarin.Forms.Button) používají ke spouštění kódu v reakci na události dotyku ovládací prvky.

Další informace o v Xamarin.Forms XAML najdete v tématu [Xamarin.Forms XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Reagovat na interakci uživatele

Objekt definovaný v XAML může vyvolat událost, kterou provádí v souboru kódu na pozadí. Následující příklad kódu ukazuje `OnTranslate` metody v kódu pro `MainPage` třídu, která je provést v reakci na [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) událost na *přeložit* tlačítko.

```csharp
void OnTranslate(object sender, EventArgs e)
{
    translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
    if (!string.IsNullOrWhiteSpace (translatedNumber)) {
        callButton.IsEnabled = true;
        callButton.Text = "Call " + translatedNumber;
    } else {
        callButton.IsEnabled = false;
        callButton.Text = "Call";
    }
}
```

`OnTranslate` Metoda překládá phoneword do jeho odpovídající telefonní číslo a v odpovědi, nastaví vlastnosti pro tlačítko volání. Použití modelu code-behind soubor pro třídu XAML dostanete objekt definovaný v XAML pomocí název přiřazený k němu pod `x:Name` atribut. Hodnota přiřazená k tomuto atributu má stejná pravidla jako C# proměnné, v, musí začínat písmenem nebo podtržítkem a obsahovat žádné mezery.

Vedení tlačítka přeložit `OnTranslate` metoda vyvolá se v kódu XAML pro `MainPage` třídy:

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty Představenými v Phoneword

Phoneword aplikaci pro Xamarin.Forms přináší několik konceptů, které nejsou zahrnuta v tomto článku. Tyto koncepty patří:

- Povolení a zakázání tlačítka. A [ `Button` ](xref:Xamarin.Forms.Button) můžete zapnout nebo vypnout tak, že změníte jeho [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) vlastnost. Například následující kód například zakáže `callButton`:

    ```csharp
    callButton.IsEnabled = false;
    ```

- Zobrazení dialogového okna výstrah. Když uživatel stiskne volání **tlačítko** u aplikace zobrazí Phoneword *dialogového okna výstrah* s možností umístit nebo zrušení volání. [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) Metoda se používá k vytvoření dialogového okna, jak je znázorněno v následujícím příkladu kódu:

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- Přístup k nativním funkcím prostřednictvím [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) třídy. Aplikace Phoneword používá `DependencyService` třídy přeložit `IDialer` rozhraní při volání implementace phone specifické pro platformu, jak je znázorněno v následujícím příkladu kódu z projektu Phoneword:

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  Další informace o [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) najdete v tématu [přístup k nativním funkce prostřednictvím DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Uvedení telefonního hovoru s adresou URL. Aplikace Phoneword používá `OpenURL` ke spuštění systému telefonní aplikace. Adresa URL se skládá z `tel:` předponou následuje telefonní číslo, která se má volat, jak je znázorněno v následujícím příkladu kódu z projektu pro iOS:

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- Úprava rozložení platformy. [ `Device` ](xref:Xamarin.Forms.Device) Třída umožňuje vývojářům k přizpůsobení rozložení aplikace a funkce na základě podle platformy, jak je znázorněno v následujícím příkladu kódu, který používá jiný [ `Padding` ](xref:Xamarin.Forms.Layout.Padding)hodnoty na různých platformách pro správné zobrazení jednotlivých stránkách:

    ```xaml
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms" ... >
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        ...
    </ContentPage>
    ```

  Další informace o vylepšení platformy najdete v tématu [třídu zařízení](~/xamarin-forms/platform/device.md).

## <a name="testing-and-deployment"></a>Testování a nasazení

Visual Studio pro Mac a Visual Studio poskytují mnoho možností pro testování a nasazení aplikace. Ladění aplikací je běžné součástí životního cyklu vývoje aplikací a pomáhá diagnostikovat problémy v kódu. Další informace najdete v tématu [nastavte zarážku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [kód prostřednictvím kroku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), a [výstupní informace do okna protokolu](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Simulátorů jsou vhodné místo pro spuštění nasazení a testování aplikace a funkce užitečných funkcí pro testování aplikací. Uživatelé však nebude využívat konečné aplikaci v simulátoru, tak aplikace by měl být testován na skutečných zařízeních již v rané fázi a často. Další informace o zřizování zařízení s Iosem, najdete v části [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md). Další informace o zřizování zařízení s Androidem, najdete v části [nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Souhrn

Tento článek má prověřit, základní informace o vývoji aplikací pomocí Xamarin.Forms. Probíraná témata zahrnuté anatomie aplikace Xamarin.Forms, architektury a základní informace o aplikaci a uživatelské rozhraní.

V další části této příručky aplikace prodlouží se mají zahrnout více obrazovek, a prozkoumejte pokročilejší architektura Xamarin.Forms a koncepty.
