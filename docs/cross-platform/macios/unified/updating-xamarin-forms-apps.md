---
title: "Aktualizace stávající aplikace Xamarin.Forms"
description: "Postupujte podle těchto kroků provedete aktualizaci existující aplikaci Xamarin.Forms pomocí unifikované API a aktualizovat verzi 1.3.1"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 48b8d1cf8e6242fde632bceec5d482f53037a954
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="updating-existing-xamarinforms-apps"></a>Aktualizace stávající aplikace Xamarin.Forms

_Postupujte podle těchto kroků provedete aktualizaci existující aplikaci Xamarin.Forms pomocí unifikované API a aktualizovat verzi 1.3.1_

> [!IMPORTANT]
> Xamarin.Forms 1.3.1 je první verzi, který podporuje unifikované API, a proto by se používat nejnovější verzi ve stejnou dobu jako migrace aplikace pro iOS na Unified aktualizovat celé řešení. To znamená, že kromě aktualizace projektu iOS pro podporu Unified, také musíte upravit kód v _všechny_ projekty v řešení.

Aktualizace se provádí ve dvou krocích:

1. Migrujte aplikaci pro iOS unifikované API pomocí sady Visual Studio pro Mac na sestavení v nástroj pro migraci.

    - Použijte nástroj pro migraci k automatické aktualizaci projektu.

    - Nativní iOS aktualizace rozhraní API, jak je uvedeno v pokynech k [aktualizace aplikací iOS](~/cross-platform/macios/unified/updating-ios-apps.md) (konkrétně v vlastní zobrazovací jednotky nebo závislost kódu služby).

2. Aktualizujte celé řešení na platformě Xamarin.Forms verze 1.3.

    1. Nainstalujte balíček NuGet Xamarin.Forms 1.3.1.

    2. Aktualizace `App` – třída v sdíleného kódu.

    3. Aktualizace `AppDelegate` v projektu pro iOS.

    4. Aktualizace `MainActivity` v projektu pro Android.

    5. Aktualizace `MainPage` v projektu Windows Phone.

## <a name="1-ios-app-unified-migration"></a>1. aplikace pro iOS (Unified migrace)

Při migraci vyžaduje upgrade na verzi 1.3, která podporuje rozhraní API Unified Xamarin.Forms. Aby odkazy na sestavení správný má být vytvořen musíme nejdřív aktualizovat iOS projekt, který používá rozhraní API Unified.

### <a name="migration-tool"></a>Nástroj pro migraci

Klikněte na projekt pro iOS tak, že je vybraná, pak zvolte **Projekt > migrace do Xamarin.iOS unifikované API...**  a souhlasíte s nimi upozornění, které se zobrazí.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Zvolte projekt > migrovat do Xamarin.iOS unifikované API... a souhlasíte s nimi upozornění, které se zobrazí")

To bude automaticky:

 - Změníte typ projektu pro podporu rozhraní API Unified 64 bitů.
 - Změňte framework odkaz na **Xamarin.iOS** (nahraďte starý **monotouch** reference).
 - Změnit odkazy na obor názvů v kódu k odebrání `MonoTouch` předponu.
 - Aktualizace **csproj** souboru k použití pro rozhraní API Unified cíle správná sestavení.

**Vyčištění** a **sestavení** projekt, který má zajistit nejsou žádné chyby opravit. Žádná další akce by měla být potřeba. Tyto kroky jsou podrobně podrobněji [unifikované API dokumentace](~/cross-platform/macios/unified/updating-ios-apps.md).

### <a name="update-native-ios-apis-if-required"></a>Aktualizovat nativní aplikace pro iOS rozhraní API (v případě potřeby)

Pokud jste přidali další iOS nativního kódu (například vlastní nástroji pro vykreslování nebo závislostí services) musíte provést další ruční kód opravy. Znovu kompilovat vaší aplikace a odkazovat na [iOS aktualizaci existující aplikace pokyny](~/cross-platform/macios/unified/updating-ios-apps.md) Další informace o změnách, které mohou být vyžadovány. [Tyto tipy](~/cross-platform/macios/unified/updating-tips.md) také pomůže identifikovat změny, které jsou požadovány.

## <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 Update

Jakmile je aplikace pro iOS je aktualizovaná tak, aby unifikované API, je třeba aktualizovat na platformě Xamarin.Forms verze 1.3.1 zbytek řešení. Sem patří:

 - Aktualizuje balíček Xamarin.Forms NuGet v každém projektu.
 - Změna kódu pro použití nového Xamarin.Forms `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android), a `FormsApplicationPage` třídy (Windows Phone).

Tyto kroky jsou vysvětleny níže:

### <a name="21-update-nuget-in-all-projects"></a>2.1 NuGet aktualizace ve všech projektech

Aktualizovat Xamarin.Forms 1.3.1 předběžné verze pomocí Správce balíčků NuGet pro všechny projekty v řešení: PCL (pokud existuje), iOS, Android a Windows Phone. Je doporučeno, které **odstranit a znovu přidejte** balíček Xamarin.Forms NuGet aktualizace na verzi 1.3.

**Poznámka:** Xamarin.Forms verze 1.3.1 je aktuálně v *předběžné verze*. To znamená, že je nutné vybrat **předběžné verze** možnost NuGet prostřednictvím (značek – pole v sadě Visual Studio pro Mac) nebo vyřaďte nižší – seznam v sadě Visual Studio zobrazíte nejnovější verzi předběžné verze.

> [!IMPORTANT]
> Pokud používáte Visual Studio, ověřte, že je nainstalovaná nejnovější verze Správce balíčků NuGet. Starší verze NuGet v sadě Visual Studio není správně nainstalován Unified verzi Xamarin.Forms 1.3.1. Přejděte na **nástroje > rozšíření a aktualizace...**  a klikněte na **nainstalovaná** seznamu zkontroluje, jestli **Správce balíčků NuGet pro Visual Studio** je minimálně verze 2.8.5. Pokud je starší, klikněte na **aktualizace** seznamu ke stažení nejnovější verze.

Jakmile na platformě Xamarin.Forms 1.3.1 aktualizujete balíček NuGet, proveďte následující změny v každém projektu pro upgrade na novou `Xamarin.Forms.Application` třídy.

### <a name="22-portable-class-library-or-shared-project"></a>2.2 Přenosná knihovna tříd (nebo sdílený projekt)

Změna **App.cs** souboru tak, aby:

 - `App` Třídy dědí vlastnosti z `Application`.
 - `MainPage` Je nastavena na první obsahu stránce si přejete zobrazit.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Jsme úplně odebrat `GetMainPage` metoda a místo toho nastavte `MainPage` *vlastnost* na `Application` podtřídy.

Tento nový `Application` základní třída podporuje také `OnStart`, `OnSleep`, a `OnResume` přepsání, které vám pomohou spravovat životní cyklus aplikace.

`App` Třída je předána na nový `LoadApplication` metoda do každého projektu aplikace, jak je popsáno níže:

### <a name="23-ios-app"></a>2.3 aplikace pro iOS

Změna **AppDelegate.cs** souboru tak, aby:

 - Třídy dědí z `FormsApplicationDelegate` (místo `UIApplicationDelegate` dříve).
 - `LoadApplication` je volána s novou instanci třídy `App`.

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

### <a name="23-android-app"></a>2.3 aplikace pro android

Změna **MainActivity.cs** souboru tak, aby:

 - Třídy dědí z `FormsApplicationActivity` (místo `FormsActivity` dříve).
 - `LoadApplication` je volána s novou instanci třídy `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
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

### <a name="24-windows-phone-app"></a>2.4 Windows Phone aplikace

Je potřeba aktualizovat **MainPage** -XAML a codebehind.

Změna **MainPage.xaml** souboru tak, aby:

 - Kořenový element XAML by měl být `winPhone:FormsApplicationPage`.
 - `xmlns:phone` Atribut by měl mít *změnit* na `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Níže je uveden aktualizované příklad – byste měli mít jenom upravit tyto věci (zbytek atributy by měla zůstat stejné):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Změna **MainPage.xaml.cs** souboru tak, aby:

 - Třídy dědí z `FormsApplicationPage` (místo `PhoneApplicationPage` dříve).
 - `LoadApplication` je volána s novou instanci třídy platformě Xamarin.Forms `App` třídy. Musíte k plnému určení tento odkaz, protože Windows Phone má vlastní `App` třídy již definována.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### <a name="troubleshooting"></a>Poradce při potížích

Někdy se zobrazí chyba podobná této po aktualizaci balíček Xamarin.Forms NuGet. Ho dochází, pokud aktualizační NuGet úplně odstraní odkazy na starší verze z vaší **csproj** soubory.

>VAŠE\_PROJECT.csproj: Chyba: Tento projekt odkazuje na balíčky NuGet, které nejsou v tomto počítači. Povolte obnovení balíčků NuGet je chcete stáhnout.  Další informace najdete v tématu http://go.microsoft.com/fwlink/?LinkID=322105. Chybí soubor je... /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-Win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.TARGETS. (VÁŠ\_PROJEKTU)

Chcete-li opravte tyto chyby, otevřete **csproj** soubor v textovém editoru a vyhledejte `<Target` elementy, které odkazují na starší verze Xamarin.Forms, jako je například níže uvedeného prvku. Měli byste ručně odstranit tento celý element z **csproj** souboru a uložte změny.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Projekt má sestavit úspěšně odebraný tyto staré odkazy.

## <a name="considerations"></a>Důležité informace

Následující aspekty měli vzít v úvahu při převodu existujícího projektu Xamarin.Forms klasické rozhraní API na nové rozhraní API Unified Pokud tuto aplikaci závisí na jeden nebo více součástí nebo balíček NuGet.

### <a name="components"></a>Součásti

Všechny součásti, které se mají zahrnout do vaší aplikace bude také nutné aktualizovat tak, aby unifikované API nebo konflikt obdržíte při pokusu o zkompilovat. U všech součástí komponent nahradí aktuální verzi s novou verzí z úložišti součástí Xamarin, který podporuje unifikované API a proveďte nové čisté sestavení. Všechny součásti, který nebyl převeden ještě autorem, se zobrazí 32bitové pouze upozornění v úložišti součástí.

### <a name="nuget-support"></a>Podpora NuGet

Když jsme podílí změny NuGet pro práci s podporou unifikované API, nepřišla novou verzi balíčku nuget, takže jsme hodnocení jak získat NuGet rozpoznat nových rozhraní API.

Do té doby, stejně jako komponenty budete potřebovat přepnout libovolný balíček NuGet jste zahrnuli ve vašem projektu a na verzi podporující rozhraní API Unified a provádět nové čisté sestavení později.

> [!IMPORTANT]
> **Poznámka:** Pokud máte chybu ve formě _"Chyba 3 nesmí obsahovat 'monotouch.dll' a"Xamarin.iOS.dll"ve stejném projektu Xamarin.iOS – 'Xamarin.iOS.dll' odkazuje explicitně, zatímco 'monotouch.dll' odkazuje ' xxx, Verze = 0.0.000, Culture = neutral, PublicKeyToken = null. "_ po převedení aplikace jednotné rozhraní API, je obvykle kvůli s komponenta nebo balíček NuGet do projektu, která nebyla aktualizována jednotné rozhraní API. Budete muset odebrat existující součásti nebo NuGet, aktualizujte na verzi podporující rozhraní API Unified a provést čisté sestavení.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Povolení 64bitové sestavení aplikace Xamarin.iOS

Pro mobilní aplikace pro Xamarin.iOS, byl převeden na unifikované API musí vývojář stále umožňující vytvářet aplikace pro 64bitové počítače z možností aplikace. Najdete v tématu **povolení 64 Bit sestavení z aplikace na platformě Xamarin.iOS** z [32 nebo 64bitový platformy aspekty](~/cross-platform/macios/32-and-64/index.md#enable-64) dokumentu podrobné pokyny k povolení 64bitové sestavení.

## <a name="summary"></a>Souhrn

Aplikaci Xamarin.Forms se musí nyní aktualizovat na verze 1.3.1 a migrovat aplikace pro iOS na unifikované API (které podporuje 64bitové architektury na platformě iOS).

Jak jsme uvedli výše, pokud vaše aplikace Xamarin.Forms zahrnuje nativního kódu, například vlastní nástroji pro vykreslování nebo závislosti služeb, pak tyto mohou také vyžadovat aktualizaci používat nové typy [byla zavedená v unifikované API](~/cross-platform/macios/index.md).

## <a name="related-links"></a>Související odkazy

- [Aktualizace aplikací iOS](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualizace aplikací iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Práce s nativní typy v multiplatformních aplikacích](~/cross-platform/macios/native-types-cross-platform.md)
- [Aktualizace tipy](~/cross-platform/macios/unified/updating-tips.md)
- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
