---
title: Informace přímým Xamarin.Forms
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: e254aa14f5889cee6b5bee452f5275fd579eb8fc
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="xamarinforms-deep-dive"></a>Informace přímým Xamarin.Forms

V [rychlý start Xamarin.Forms](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md), Phoneword byla vytvořena. Tento článek zkontroluje, co byla vytvořena získat představu o základní informace o fungování Xamarin.Forms aplikace.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Úvod k sadě Visual Studio

Visual Studio je výkonný IDE od společnosti Microsoft. Jeho součástí jsou plně integrované vizuálního návrháře, textový editor, který je kompletní s refaktoringu nástroje, sestavení prohlížeče, integrace zdrojového kódu a další. Tento článek se zaměřuje na některé základní funkce sady Visual Studio pomocí modulu plug-in Xamarin.

Visual Studio organizuje kód do *řešení* a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace, do pomocné knihovny, testovací aplikace a další. Phoneword aplikace se skládá z jedné řešení obsahující čtyři projekty, jak je znázorněno na následujícím snímku obrazovky.

![](deepdive-images/vs/solution.png "Průzkumník řešení sady Visual Studio")

Projekty jsou:

- Phoneword – tento projekt je .NET Standard projektu knihovny, který obsahuje všechny sdílené kód a sdíleného uživatelského rozhraní.
- Phoneword.Android – tento projekt obsahuje konkrétní kódu pro systém Android a vstupní bod pro aplikace pro Android.
- Phoneword.iOS – tento projekt obsahuje iOS konkrétního kódu a vstupní bod pro aplikace systému iOS.
- Phoneword.UWP – tento projekt obsahuje konkrétní kód univerzální platformu Windows (UWP) a je vstupní bod pro aplikace pro UPW.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomie aplikaci na platformě Xamarin.Forms

Následující snímek obrazovky ukazuje Phoneword .NET Standard projektu knihovny obsahu v sadě Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Obsah standardní projektu Phoneword rozhraní .NET")

Tento projekt **závislosti** uzlu, který obsahuje **NuGet** a **SDK** uzlů. **NuGet** uzel obsahuje balíček Xamarin.Forms NuGet, který byl přidán do projektu a **SDK** uzel obsahuje `NETStandard.Library` metapackage, který odkazuje na kompletní sadu balíčků NuGet které definují .NET Standard.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Úvod k sadě Visual Studio pro Mac

Visual Studio pro Mac je zdarma, který open-source IDE podobná Visual Studio. Jeho součástí jsou plně integrované vizuálního návrháře, textový editor, který je kompletní s refaktoringu nástroje, sestavení prohlížeče, integrace zdrojového kódu a další. Další informace o sadě Visual Studio pro Mac najdete v tématu [představení Visual Studio pro Mac](/visualstudio/mac/).

Visual Studio pro Mac následuje postup sadě Visual Studio uspořádání kód do *řešení* a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace, do pomocné knihovny, testovací aplikace a další. Phoneword aplikace se skládá z jedné řešení obsahující tři projekty, jak je znázorněno na následujícím snímku obrazovky.

![](deepdive-images/xs/solution.png "Visual Studio pro Mac podokno řešení")

Projekty jsou:

- Phoneword – tento projekt je projektu knihovny (PCL) přenosných tříd, který obsahuje všechny sdílené kód a sdíleného uživatelského rozhraní.
- Phoneword.Droid – tento projekt obsahuje konkrétní kódu pro systém Android a vstupní bod pro aplikace pro Android.
- Phoneword.iOS – tento projekt obsahuje iOS konkrétního kódu a je vstupní bod pro aplikace pro iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomie aplikaci na platformě Xamarin.Forms

Následující snímek obrazovky ukazuje obsah Phoneword PCL projektu v sadě Visual Studio pro Mac:

![](deepdive-images/xs/pcl-project.png "Obsah Phoneword PCL projektu")

Projekt se skládá ze tří složek:

- **Odkazy na** – obsahuje sestavení požadovaná sestavení a spuštění aplikace. Rozšiřování složce přenosném dílčím .NET zjistí odkazy na sestavení .NET, jako [systému](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx), System.Core a [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx). Rozšíření **z balíčků** složku zjistí odkazy na sestavení Xamarin.Forms.
- **Balíčky** – balíčky adresáři Domů [NuGet](https://www.nuget.org) balíčky, které zjednodušují proces použití knihoven jiných dodavatelů ve vaší aplikaci. Tyto balíčky lze aktualizovat na nejnovější vydání kliknutím pravým tlačítkem na složku a výběrem možnosti aktualizace v místní nabídce.
- **Vlastnosti** – obsahuje **AssemblyInfo.cs**, soubor metadat sestavení rozhraní .NET. Je dobrým zvykem vyplníte tento soubor některých základních informací o vaší aplikaci. Další informace o tento soubor najdete v tématu [AssemblyInfo třída](http://msdn.microsoft.com/library/microsoft.visualbasic.applicationservices.assemblyinfo(v=vs.110).aspx) na webu MSDN.

-----

Projekt se skládá z několika souborů:

- **App.XAML** – kód jazyce XAML pro `App` třídy, která definuje slovník prostředků pro aplikaci.
- **App.XAML.cs** – kódu pro `App` třídy, která odpovídá za vytváření instancí na první stránku, která se zobrazí v aplikaci na každou platformu a pro zpracování události životního cyklu aplikace.
- **IDialer.cs** – `IDialer` rozhraní, které určuje, že `Dial` metoda musí být poskytnuta žádnou implementující třídu.
- **MainPage.xaml** – kód jazyce XAML pro `MainPage` třídy, která definuje uživatelské rozhraní pro stránky zobrazí při spuštění aplikace.
- **MainPage.xaml.cs** – kódu pro `MainPage` třídy, která obsahuje obchodní logiky, která se spustí, až uživatel pracuje se stránkou.
- **soubor Packages.config** – soubor ve formátu XML (Visual Studio pro Mac pouze), který obsahuje informace o balíčcích NuGet používá projektu, sledovat požadované balíčky a příslušné verze. Visual Studio pro Mac a Visual Studio může být nakonfigurován pro automatické obnovení všech chybějících balíčků NuGet při sdílení s jinými uživateli ve zdrojovém kódu. Obsah tohoto souboru se řídí správce balíčků NuGet a by neměla být upravována ručně.
- **PhoneTranslator.cs** – obchodní logiky, která je zodpovědná za převod phone word na telefonní číslo, který lze vyvolat pomocí **MainPage.xaml.cs**.

Další informace o anatomy aplikace pro Xamarin.iOS najdete v tématu [anatomie aplikace pro Xamarin.iOS](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy). Další informace o anatomy aplikace pro Xamarin.Android, najdete v části [anatomie aplikace pro Xamarin.Android](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Architektura a základní informace o aplikaci

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aplikaci Xamarin.Forms je navržen stejným způsobem jako tradiční aplikace napříč platformami. Sdílené kódu je obvykle umístěna v rozhraní .NET standardní knihovna a specifické pro platformu aplikace využívat sdíleného kódu. Následující diagram ukazuje přehled této relace pro aplikaci Phoneword:

![](deepdive-images/vs/architecture.png "Architektura Phoneword")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aplikaci Xamarin.Forms je navržen stejným způsobem jako tradiční aplikace napříč platformami. Sdílené kódu je obvykle umístěna v přenosných třída knihovny PCL () a specifické pro platformu aplikace využívat sdíleného kódu. Následující diagram ukazuje přehled této relace pro aplikaci Phoneword:

![](deepdive-images/xs/architecture.png "Architektura Phoneword")

Další informace o PCLs najdete v tématu [Úvod do knihovny přenosných tříd](~/cross-platform/app-fundamentals/pcl.md).

-----

Pokud chcete maximalizovat opakované použití kódu spuštění, Xamarin.Forms aplikací mít jednu třídu s názvem `App` který zodpovídá za vytvoření instance na první stránku, která se zobrazí v aplikaci na každou platformu, jak je znázorněno v následujícím příkladu kódu:

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

Nastaví tento kód `MainPage` vlastnost `App` novou instanci třídy [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) třídy. Kromě toho [ `XamlCompilation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/) atribut zapne kompilátoru XAML, tak, aby XAML zkompilován přímo do převodní jazyk. Další informace najdete v tématu [XAML kompilace](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Spuštění aplikace na každou platformu

### <a name="ios"></a>iOS

Spusťte úvodní stránky Xamarin.Forms v iOS, tento projekt Phoneword.iOS zahrnuje `AppDelegate` třídu, která dědí z `FormsApplicationDelegate` třídy, jak je znázorněno v následujícím příkladu kódu:

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

`FinishedLaunching` Přepsání inicializuje pomocí volání rozhraní Xamarin.Forms `Init` metoda. To způsobí, že implementace specifické pro iOS Xamarin.Forms má být načten v aplikaci, než je ve volání nastavení řadiče zobrazení kořenové `LoadApplication` metoda.

### <a name="android"></a>Android

Spusťte úvodní stránky Xamarin.Forms v Android, tento projekt Phoneword.Droid zahrnuje kód, který vytvoří `Activity` s `MainLauncher` atribut s aktivitou, která dědí z `FormsApplicationActivity` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword",
              Icon = "@drawable/icon",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` Přepsání inicializuje pomocí volání rozhraní Xamarin.Forms `Init` metoda. To způsobí, že implementace specifické pro Android Xamarin.Forms má být načten v aplikaci, než je aplikace Xamarin.Forms načtena. Kromě toho `MainActivity` třída ukládá odkaz sám na sebe v `Instance` vlastnost. `Instance` Vlastnost se označuje jako lokální kontext a je na něj odkazovat z `PhoneDialer` třídy.

## <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

V aplikacích pro univerzální platformu Windows (UWP) `Init` metoda, která inicializuje rozhraní Xamarin.Forms se volá z `App` třídy:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

To způsobí, že implementace specifické pro UPW Xamarin.Forms načíst v aplikaci. Úvodní stránky Xamarin.Forms je spuštěn `MainPage` třídy, jak je ukázáno v následujícím příkladu kódu:

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

Xamarin.Forms aplikace je načtena s `LoadApplication` metoda.

> [!NOTE]
> Univerzální aplikace pro platformu Windows (UWP) může být vytvořené s Xamarin.Forms, ale pouze pomocí sady Visual Studio v systému Windows.

## <a name="user-interface"></a>Uživatelské rozhraní

Existují čtyři hlavní řízení skupiny použít k vytvoření uživatelského rozhraní aplikace Xamarin.Forms.

1. **Stránky** – Xamarin.Forms stránky představují obrazovky napříč platformami mobilních aplikací. Aplikace Phoneword používá [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) třída zobrazíte jedné obrazovce. Další informace o stránky najdete v tématu [Xamarin.Forms stránky](~/xamarin-forms/user-interface/controls/pages.md).
1. **Rozložení** – Xamarin.Forms rozložení jsou kontejnery použitý k sestavení zobrazení do logické struktury. Aplikace Phoneword používá [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) třída k uspořádání ovládacích prvků v vodorovné zásobníku. Další informace o rozložení najdete v tématu [Xamarin.Forms rozložení](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Zobrazení** – Xamarin.Forms zobrazení se zobrazí v uživatelském rozhraní, jako je například popisky, tlačítek a textová vstupní pole ovládacích prvků. Aplikace Phoneword používá [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), a [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ovládací prvky. Další informace o zobrazení najdete v tématu [Xamarin.Forms zobrazení](~/xamarin-forms/user-interface/controls/views.md).
1. **Buněk** – Xamarin.Forms buněk jsou specializované prvky používané pro položky v seznamu a popisují, jak mají být vykresleny každou položku v seznamu. Použití Phoneword, které aplikace neprovede žádné buňky. Další informace o buněk najdete v tématu [Xamarin.Forms buněk](~/xamarin-forms/user-interface/controls/cells.md).

Za běhu budou každý ovládací prvek mapována na ekvivalentní nativní, což je co bude vykreslen.

Pokud je aplikace Phoneword je spouštěn na libovolné platformě, zobrazuje jedné obrazovce, která odpovídá [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) v Xamarin.Forms. A `Page` představuje *skupinu ViewGroup* v systému Android, *View Controller* v iOS, nebo *stránky* na univerzální platformu Windows. Phoneword aplikace také vytvoří [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objekt, který reprezentuje `MainPage` třídy, jejichž XAML poznámky je vidět v následujícím příkladu kódu:

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

`MainPage` Třídy používá [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) řízení automaticky uspořádání ovládacích prvků na obrazovce bez ohledu na velikost obrazovky. Každý podřízený element je umístěného jedna po druhé, svisle v pořadí, ve kterém jsou přidány. `StackLayout` Obsahuje ovládací prvek [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ovládací prvek zobrazí text na stránce [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládacího prvku tak, aby přijímal textovou uživatelský vstup a dvě [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ovládací prvky, které používají ke spouštění kódu v reakci na touch události.

Další informace o XAML v Xamarin.Forms najdete v tématu [Xamarin.Forms XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Neodpovídá na požadavky interakci s uživatelem

Objekt definovaný v jazyce XAML, můžete aktivovat událost, která zpracovává v souboru kódu na pozadí. Následující příklad kódu ukazuje `OnTranslate` metodu v kódu pro `MainPage` třídy, která se spustí v reakci na [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) událost, která iniciovala na *přeložit* tlačítko.

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

`OnTranslate` Metoda přeloží phoneword do její odpovídající telefonní číslo a v odpovědi, nastaví vlastnosti pro tlačítko volání. K souboru kódu pro třídu XAML přístup objekt definovaný v jazyce XAML, pomocí názvu přiřazen s `x:Name` atribut. Hodnota přiřazená k tento atribut má stejná pravidla jako C# proměnné, v tom musí začínat písmenem nebo podtržítkem a obsahovat žádné mezery.

Vedení tlačítka přeložit `OnTranslate` metoda dojde v kód XAML pro `MainPage` třídy:

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty představené v Phoneword

Aplikace Phoneword pro Xamarin.Forms obsahuje zavedla několik konceptů, které nejsou zahrnuté v tomto článku. Tyto koncepty patří:

- Povolení a zákaz tlačítek. A [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) můžete zapnout nebo vypnout tak, že změníte jeho [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) vlastnost. Například následující kód například zakáže `callButton`:

    ```csharp
    callButton.IsEnabled = false;
    ```

- Zobrazení dialogového okna výstrah. Když uživatel stiskne volání **tlačítko** ukazuje aplikace Phoneword *dialogového okna výstrah* s možností umístit nebo zrušit volání. [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Metoda se používá k vytvoření tohoto dialogového okna, jak je znázorněno v následujícím příkladu kódu:

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- Přístup k nativní funkce prostřednictvím [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) třídy. Aplikace Phoneword používá `DependencyService` třída přeložit `IDialer` rozhraní pro vytáčení implementace phone specifické pro platformu, jak je znázorněno v následujícím příkladu kódu z projektu Phoneword:

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  Další informace o [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) třídy najdete v tématu [přístup k nativní funkce prostřednictvím DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Umístění telefonního hovoru s adresou URL. Aplikace Phoneword používá `OpenURL` spustit aplikaci na telefonu systému. Adresa URL se skládá z `tel:` předponou následuje telefonní číslo, která se má volat, jak je znázorněno v následujícím příkladu kódu z projektu pro iOS:

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- Postupně je upravujte rozložení platformy. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Třída umožňuje vývojářům přizpůsobit rozložení aplikace a funkce na základě podle platformy, jak je znázorněno v následujícím příkladu kódu, který používá jiný [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)hodnoty na různých platformách správně zobrazíte každé stránce:

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

Visual Studio pro Mac a Visual Studio poskytují mnoho možností pro testování a nasazení aplikace. Ladění aplikace je běžné součástí životního cyklu aplikace a pomáhá diagnostikovat problémy kódu. Další informace najdete v tématu [zarážku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [kód prostřednictvím kroku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), a [výstupní informace do okna protokolu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

Simulátorů jsou místo pro spuštění nasazení a testování aplikace a funkce užitečné funkce pro testování aplikací. Uživatelé však nebude využívat konečné aplikaci v simulátoru, tak aplikace by měla být testována na skutečné zařízení včas a často. Další informace o zřizování zařízení iOS najdete v tématu [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md). Další informace o zřizování zařízení se systémem Android, najdete v části [nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Souhrn

Tento článek má zkontrolován základní informace o vývoj aplikací pomocí Xamarin.Forms. Témata popsaná zahrnuty anatomie Xamarin.Forms aplikace, architekturu a základy aplikaci a uživatelské rozhraní.

V další části této příručky aplikace bude rozšířeno zahrnout více obrazovek a prozkoumejte pokročilejší Xamarin.Forms architektura a koncepty.
