---
title: Hello, Mac
description: "Tento průvodce vás provede kroky k vytvoření první aplikace Xamarin.Mac a v procesu zavádí vývoj nástrojů, včetně sady Visual Studio pro Mac, Xcode a rozhraní tvůrce. Také zavádí výstupy a akcí, které zveřejňují ovládacích prvků uživatelského rozhraní na kód, a nakonec ho ukazuje, jak pro vytvoření, spuštění a testování Xamarin.Mac aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: fdf5d1236c0d8f797bc53d01eada1777b1d92373
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="hello-mac"></a>Hello, Mac

Xamarin.Mac umožňuje vývoj aplikace plně nativní Mac v C# a .NET pomocí stejné knihovny OS X a ovládací prvky rozhraní, které se používají při vývoji v *jazyka Objective-C* a *Xcode*. Vzhledem k tomu, že Xamarin.Mac integruje přímo s Xcode, vývojáři použít na Xcode _rozhraní tvůrce_ vytvořit uživatelské rozhraní aplikace (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Navíc vzhledem k tomu, že Xamarin.Mac aplikace jsou napsané v C# a rozhraní .NET, běžné, back-end kód je možné sdílet s Xamarin.iOS a Xamarin.Android mobilní aplikace; všechny při doručování nativním prostředím na každou platformu.

Tento článek vás seznámí klíčové koncepty jsou potřeba k vytvoření aplikace pro Mac pomocí Xamarin.Mac, Visual Studio pro Mac a na Xcode rozhraní tvůrce rámci prostřednictvím procesu vytváření jednoduše **Hello, Mac** aplikaci, která vrátí počet časy tlačítko bylo stisknuto:

[![](hello-mac-images/run02.png "Příklad Hello, Mac aplikaci spuštěnou")](hello-mac-images/run02.png#lightbox)

Následující koncepty se budeme:

-  **Visual Studio pro Mac** – Úvod do sady Visual Studio pro Mac a vytváření aplikací Xamarin.Mac s ním.
-  **Anatomie aplikace Xamarin.Mac** – co Xamarin.Mac aplikace se skládá z.
-  **Xcode je rozhraní tvůrce** – jak používat Xcode je rozhraní tvůrce definovat uživatelské rozhraní aplikace.
-  **Výstupy a akce** – jak pomocí akce a výstupy propojit se ovládací prvky v uživatelském rozhraní.
-  **Nasazení nebo testování** – spuštění a testování Xamarin.Mac aplikace.


<a name="Requirements" />

## <a name="requirements"></a>Požadavky

Toto je požadováno pro vývoj aplikací systému macOS s Xamarin.Mac:

- Počítač Mac se systémem systému macOS Yosemite(10.10) nebo vyšší.
- Xcode 7 a vyšší (i když se doporučuje nainstalovat nejnovější stabilní verzi [obchod](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).)
- Nejnovější verzi Xamarin.Mac a Visual Studio for Mac.

Spuštěné aplikace systému Mac, které jsou vytvořené pomocí Xamarin.Mac mají následující požadavky:

- Počítač Mac se systémem Mac OS X 10,7 nebo vyšší.

<a name="Starting_a_new_Xamarin.Mac_App_in_Xamarin_Studio" />

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Spuštění nové aplikace Xamarin.Mac v sadě Visual Studio pro Mac

Jak jsme uvedli výše, tento průvodce provede kroky k vytvoření aplikace pro Mac názvem `Hello_Mac` doplňuje jednoho tlačítka a popisek do hlavního okna. Při kliknutí na tlačítko se zobrazí popisek počet časy, kdy bylo stisknuto.

Abyste mohli začít, postupujte takto:

1. Spuštění sady Visual Studio pro Mac:

    [![](hello-mac-images/setup01.png "Hlavní sady Visual Studio pro Mac rozhraní")](hello-mac-images/setup01.png#lightbox)

2. Klikněte na **nové řešení...**  odkaz v levém horním rohu obrazovky, otevřete **nový projekt** dialogové okno:

    [![](hello-mac-images/setup03.png "Vytvoření nové řešení v sadě Visual Studio pro Mac")](hello-mac-images/setup02.png#lightbox)

3. Vyberte **Mac** > **aplikace** > **kakao aplikace** a klikněte na **Další** tlačítko:

    [![](hello-mac-images/setup03.png "Výběr aplikace kakao")](hello-mac-images/setup03.png#lightbox)

4. Zadejte `Hello_Mac` pro **název aplikace**a udržovat všem ostatním jako výchozí. Klikněte na tlačítko **Další**:

    [![](hello-mac-images/setup05.png "Nastavení názvu aplikace")](hello-mac-images/setup05.png#lightbox)

4. Při vytváření řešení, která bude obsahovat několik různých projektů, vývojář může chtít nastavit jiný **název řešení** zde, ale z důvodu tento příklad, nechte nastavení na výchozí je stejný jako  **Název projektu**:

    [![](hello-mac-images/setup04.png "Ověření nové podrobnosti řešení")](hello-mac-images/setup04.png#lightbox)

5. Klikněte **vytvořit** tlačítko.

Visual Studio pro Mac se vytvořit novou aplikaci Xamarin.Mac a zobrazit výchozí soubory, které se přidají do řešení aplikace:

 [![](hello-mac-images/project01.png "Nové výchozí zobrazení řešení")](hello-mac-images/project01.png#lightbox)

Visual Studio pro Mac používá **řešení** a **projekty**, přesný stejným způsobem, který Visual Studio. Řešení je kontejner, který může obsahovat jeden nebo více projektů; projekty může zahrnovat aplikací, podpora knihovny, testovací aplikace atd. V takovém případě Visual Studio pro Mac se vytvořil řešení a projekt aplikace automaticky.

V případě potřeby vývojář vytvořit jeden nebo více kód projektů knihovny obsahující běžné, sdíleného kódu. Tyto projektů knihovny můžou spotřebovávají projektu aplikace nebo sdíleny s jinými projekty Xamarin.Mac aplikace (nebo Xamarin.iOS a Xamarin.Android na základě typu kódu), stejným způsobem jako u standardní aplikace .NET.

<a name="The_Project" />

## <a name="anatomy-of-a-xamarinmac-application"></a>Anatomie Xamarin.Mac aplikace

Pokud znáte iOS programování, je celá řada podobnosti sem. Ve skutečnosti iOS používá CocoaTouch rozhraní, které je slimmed nižší verze kakao, používá Mac, tak bude překřížila mnoho konceptů.

Podívejte se na soubory v projektu:

-   `Main.cs` – Tato položka obsahuje hlavní vstupní bod aplikace. Když je aplikace spuštěná, tato položka obsahuje velmi první třídy a metody, která se spustí.
-   `AppDelegate.cs` – Tento soubor obsahuje třídu hlavní aplikace, která je zodpovědná za naslouchá událostem z operačního systému.
-   `Info.plist` – Tento soubor obsahuje vlastnosti aplikace, jako je například název aplikace, ikony, atd.
-   `Entitlements.plist` – Tato soubory obsahuje oprávnění pro aplikaci a umožňuje přístup k objektům, jako je podpora Sandboxing a na Icloudu.
-  `Main.storyboard` – Definuje uživatelské rozhraní (Windows a nabídky) pro aplikaci a rozložen propojení mezi Windows prostřednictvím Segues. Scénářů jsou soubory formátu XML, které obsahují definice zobrazení (elementy uživatelského rozhraní). Tento soubor můžete vytvářené a udržované pomocí Tvůrce rozhraní v Xcode.
-   `ViewController.cs` – To je ovladač pro hlavní okno. Řadiče se budeme podrobně v jiném článku, ale prozatím se může považovat řadič modul hlavní žádné konkrétní zobrazení.
-   `ViewController.designer.cs` – Tento soubor obsahuje kód pro vložení, který pomáhá integrovat s uživatelským rozhraním hlavní obrazovky.

Následující části, bude trvat rychle zkontrolovat pomocí některé z těchto souborů. Později bude být zkoumána podrobněji, ale je vhodné pochopit jejich základy teď.

<a name="Main_cs" />

### <a name="maincs"></a>Main.cs

**Main.cs** souborů je velmi jednoduchý. Obsahuje statický `Main` metodu, která vytvoří novou instanci aplikace Xamarin.Mac a předá název třídy, která bude zpracovávat události operačního systému, který v tomto případě je `AppDelegate` třídy:

```csharp
using System;
using System.Drawing;
using Foundation;
using AppKit;
using ObjCRuntime;

namespace Hello_Mac
{
        class MainClass
        {
                static void Main (string[] args)
                {
                        NSApplication.Init ();
                        NSApplication.Main (args);
                }
        }
}
```

<a name="AppDelegate_cs" />

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Soubor obsahuje `AppDelegate` třídy, která je odpovědná za vytváření windows a naslouchá událostem operačního systému:

```csharp
using AppKit;
using Foundation;

namespace Hello_Mac
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Tento kód je pravděpodobně obeznámeni, pokud vývojář aplikace pro iOS před vytvořila, ale je velmi jednoduché.

`FinishedLaunching` Metoda se spouští po po vytvoření instance aplikace a je odpovědná za ve skutečnosti vytváření okna aplikace a od proces zobrazení zobrazení v ní.

`WillTerminate` Metoda bude volána, když uživatel nebo systém má vytvořena instance vypnutí aplikace. Vývojář musí tuto metodu použijte pro dokončení aplikace předtím, než ho ukončí (například ukládání předvoleb uživatelů nebo velikost okna a umístění).

<a name="ViewController_cs" />

### <a name="viewcontrollercs"></a>ViewController.cs

Kakao (a odvození, CocoaTouch) používá, která se označuje jako *Model View Controller* vzor (MVC). `ViewController` Deklarace představuje ovládací prvky objektu okna skutečné aplikace. Obecně platí pro každý okna vytvořeného (a pro mnoho dalších položek v rámci systému windows), není kontroler, který je zodpovědný za okna životní cyklus, například zobrazení, přidání nové zobrazení (ovládací prvky) Chcete-li ji, atd.

`ViewController` Třída je hlavním okně řadiče. To znamená, že je zodpovědná za životní cyklus hlavního okna. To bude podrobně později, pro provést nyní rychle zobrazit ho:

```csharp
using System;

using AppKit;
using Foundation;

namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }
    }
}
```

<a name="ViewController_Designer_cs" />

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Návrháře soubor pro třídu hlavní okno je teď prázdný, ale jej bude automaticky vyplňovat Visual Studio pro Mac jako uživatelské rozhraní je vytvořena s rozhraní tvůrce uvnitř Xcode:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac to store outlets and
// actions made in the UI designer. If it is removed, they will be lost.
// Manual changes to this file may not be handled correctly.
//
using Foundation;

namespace Hello_Mac
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Vývojáři obvykle není nevadí návrháře souborů, protože jste automaticky prováděna nástrojem Visual Studio pro Mac a zadejte požadavků pluming kód, který umožňuje přístup k ovládacím prvkům, které jsou přidané do jakékoli okno nebo zobrazení v aplikaci.

Projekt aplikace Xamarin.Mac vytvořen a základní znalosti o jeho komponenty přepněte do Xcode k vytvoření uživatelského rozhraní pomocí rozhraní tvůrce.

<a name="Info_plist" />

### <a name="infoplist"></a>Info.plist

`Info.plist` Soubor obsahuje informace o aplikaci Xamarin.Mac jako jeho **název** a **identifikátor svazku**:

[![](hello-mac-images/infoplist01.png "Visual Studio pro Mac editor plist.")](hello-mac-images/infoplist01.png#lightbox)

A definuje _Storyboard_ který se použije k zobrazení uživatelského rozhraní pro aplikaci Xamarin.Mac pod **hlavní rozhraní** rozevíracího seznamu. V případě příkladu nahoře `Main` v rozevírací nabídce má vztah k `Main.storyboard` ve stromu zdroje projektu v **Průzkumníku řešení**. Také definuje ikon aplikace tak, že zadáte *katalog Asset* obsahující je (v tomto případě AppIcons).

### <a name="entitlementsplist"></a>Entitlements.plist

Aplikace `Entitlements.plist` soubor řídí oprávnění, které má aplikace Xamarin.Mac jako **Sandboxing** a **Icloudu**:

[![](hello-mac-images/entitlements01.png "Visual Studio pro Mac oprávnění editor")](hello-mac-images/entitlements01.png#lightbox)

Například Hello World se bude vyžadovat žádná oprávnění. V další části ukazuje, jak na Xcode rozhraní tvůrce použít k úpravě `Main.storyboard` souboru a definovat Xamarin.Mac uživatelském rozhraní aplikace.

<a name="Introduction_to_Xcode_and_Interface_Builder" />

## <a name="introduction-to-xcode-and-interface-builder"></a>Úvod do Xcode a Tvůrce rozhraní

V rámci Xcodu Apple vytvořil nástroj volat rozhraní tvůrce, který umožňuje vývojáři vizuálně vytváření uživatelského rozhraní v návrháři. Xamarin.Mac fluently integruje s Tvůrce rozhraní, což uživatelského rozhraní mají být vytvořeny s stejné nástroje jako jazyka Objective-C uživatele.

Chcete-li začít, dvakrát klikněte na `Main.storyboard` souboru v **Průzkumníku řešení** otevřete pro úpravy v Xcode a Tvůrce rozhraní:

[![](hello-mac-images/xcode01.png "Main.storyboard soubor v Průzkumníku řešení")](hello-mac-images/xcode01.png#lightbox)

To by měl spusťte Xcode a vypadat podobně jako následující:

[![](hello-mac-images/xcode02.png "Výchozí zobrazení rozhraní tvůrce Xcode")](hello-mac-images/xcode02.png#lightbox)

Před zahájením návrhu rozhraní, trvat rychlý přehled o Xcode pro orientaci s hlavní funkce, které se budou používat.

> [!NOTE]
> Vývojář nemá používat Xcode a Tvůrce rozhraní k vytváření uživatelského rozhraní pro aplikaci Xamarin.Mac, uživatelského rozhraní lze vytvořit přímo z kódu jazyka C#, ale který je nad rámec tohoto článku. Z důvodu zjednodušení se budou používat rozhraní tvůrce vytvořit uživatelské rozhraní ve zbývající části tohoto kurzu.


<a name="Components_of_Xcode" />

### <a name="components-of-xcode"></a>Součástí Xcode

Při otevírání `.storyboard` souboru v Xcode ze sady Visual Studio pro Mac, otevře se **navigátoru projektů** na levé straně **rozhraní hierarchie** a **rozhraní editoru**uprostřed a **vlastnosti & Nástroje** části na pravé straně:

[![](hello-mac-images/xcode03.png "Různé části Tvůrce rozhraní v Xcode")](hello-mac-images/xcode03.png#lightbox)

V následujících částech prohlédněte si co každý z těchto funkcí proveďte Xcode a jakým způsobem je použít k vytvoření rozhraní pro aplikaci Xamarin.Mac.

<a name="Project_Navigation" />

### <a name="project-navigation"></a>Navigace projektu

Při otevírání `.storyboard` souboru pro úpravy v Xcode, Visual Studio pro Mac vytvoří *soubor projektu Xcode* na pozadí pro komunikaci změny mezi samostatně a Xcode. Později když vývojáři přejde zpět do Visual Studio pro Mac z Xcode, změny provedené v tomto projektu jsou synchronizovány s projektem Xamarin.Mac pomocí sady Visual Studio for Mac.

**Projektu navigační** části umožňuje vývojáři přecházet mezi všechny soubory, které tvoří to _shim_ projektu Xcode. Obvykle se budou jenom zájem o `.storyboard` soubory v tomto seznamu, jako `Main.storyboard`.

<a name="Interface_Hierarchy" />

### <a name="interface-hierarchy"></a>Rozhraní hierarchie

**Rozhraní hierarchie** části umožňuje vývojáři snadný přístup k několika klíčové vlastnosti uživatelského rozhraní, jako má **zástupné symboly** a hlavní **okno**. V této části můžete použít pro přístup k jednotlivé elementy (zobrazení), které tvoří uživatelské rozhraní a upravit tak, že se nejedná o vnořené jejich kolem přetažením v rámci hierarchie.

<a name="Interface_Editor" />

### <a name="interface-editor"></a>Editor rozhraní

**Rozhraní editoru** část obsahuje prostor, na kterém je uživatelské rozhraní graficky nastíněny. Přetáhněte elementy z **knihovny** části **vlastnosti & Nástroje** oddílu pro vytvoření návrhu. Prvky uživatelského rozhraní (zobrazení) při přidávání na návrhovou plochu, budou přidány do **rozhraní hierarchie** v pořadí, ve kterém se zobrazují v části **rozhraní editoru**.

<a name="Properties_Utilities" />

### <a name="properties--utilities"></a>Vlastnosti & Nástroje

**Vlastnosti & Nástroje** část je rozdělena na dvě hlavní části **vlastnosti** (také nazývané inspektoři) a **knihovny**:

[![](hello-mac-images/xcode04.png "Vlastnosti Inspector")](hello-mac-images/xcode04.png#lightbox)

Zpočátku je v této části téměř prázdný, ale pokud vývojář vybere element v **rozhraní editoru** nebo **rozhraní hierarchie**, **vlastnosti** bude oddíl obsahuje informace o daného elementu a vlastnosti, které se můžete upravit.

V rámci **vlastnosti** části, se liší 8 *Inspector karty*, jak je znázorněno na následujícím obrázku:

[![](hello-mac-images/xcode05.png "Přehled všechny kontroly")](hello-mac-images/xcode05.png#lightbox)

<a name="Properties_Utility_Types" />

### <a name="properties--utility-types"></a>Vlastnosti & nástroj typy

Z zleva doprava jsou těchto karet:

-   **Soubor Inspector** – v souboru Inspector zobrazuje informace o souborech, jako je název souboru a umístění souboru Xib, který je zpracováván.
-   **Rychlé nápovědy** – karta rychlé pomoci poskytuje kontextové nápovědy podle položky vybrané v Xcode.
-   **Identity Inspector** – The Identity Inspector obsahuje informace o vybrané zobrazení/ovládacího prvku.
-   **Atributy Inspector** – atributy Inspector umožňuje vývojáři přizpůsobit různé atributy vybrané zobrazení/ovládacího prvku.
-   **Velikost Inspector** – velikost Inspector umožňuje vývojáři řídit velikost a změna velikosti chování vybrané zobrazení/ovládacího prvku.
-   **Připojení Inspector** – zobrazuje Inspector připojení **výstupu** a **akce** připojení vybrané ovládacích prvků. Výstupy a akce se bude zabývat rozpis naleznete níže.
-   **Vazby Inspector** – vazby Inspector umožňuje vývojáři ke konfiguraci ovládacích prvků tak, aby jejich hodnoty jsou automaticky vázány na datové modely.
-   **Zobrazit důsledky Inspector** – v zobrazení důsledky Inspector umožňuje vývojáři zadejte důsledky pro ovládací prvky, jako je například animace.

Použití **knihovny** části najít ovládací prvky a objekty, které chcete umístit do návrháře graficky sestavit uživatelské rozhraní:

[![](hello-mac-images/xcode06.png "Knihovna Xcode Inspector")](hello-mac-images/xcode06.png#lightbox)

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytváření rozhraní

Se základy Xcode IDE a zahrnutých Tvůrce rozhraní můžete vytvořit vývojář uživatelské rozhraní pro hlavní zobrazení.

Postupujte takto:

1. V Xcode, přetáhněte ji **tlačítko** z **knihovny části**:

    [![](hello-mac-images/xcode07.png "Výběr NSButton z knihovny Inspector")](hello-mac-images/xcode07.png#lightbox)

2. Vyřaďte tlačítko na **zobrazení** (v části **okno řadiče**) v **rozhraní editoru**:

    [![](hello-mac-images/xcode08.png "Přidání tlačítka na návrh rozhraní")](hello-mac-images/xcode08.png#lightbox)

3. Klikněte na **název** vlastnost **atribut Inspector** a změňte název na tlačítko pro `Click Me`:

    [![](hello-mac-images/xcode09.png "Nastavení vlastností tlačítka")](hello-mac-images/xcode09.png#lightbox)

4. Přetáhněte **popisek** z **knihovny části**:

    [![](hello-mac-images/xcode10.png "Výběr štítek z knihovny Inspector")](hello-mac-images/xcode10.png#lightbox)

5. Vyřaďte popisek na **okno** vedle tlačítka na **rozhraní editoru**:

    [![](hello-mac-images/xcode11.png "Přidávání do rozhraní návrhu štítek")](hello-mac-images/xcode11.png#lightbox)

6. Získat popisovač vpravo na štítek a přetáhněte ji, dokud nebude u okraje okna:

    [![](hello-mac-images/xcode12.png "Změna velikosti popisku")](hello-mac-images/xcode12.png#lightbox)

7. Kliknutím na tlačítko právě přidali v **rozhraní editoru**a klikněte na tlačítko **omezení Editor** ikonu a dolní části okna:

    [![](hello-mac-images/xcode13.png "Přidání omezení pro tlačítko")](hello-mac-images/xcode13.png#lightbox)

8. V horní části editoru, klikněte **Red I světla** v horní a levé straně. Při změně velikosti okna to ponechá tlačítko ve stejném umístění, v levém horním rohu obrazovky.

9. Dále zkontrolujte **výška** a **šířka** oknech a použít výchozí velikosti. Díky tomu tlačítko stejné velikosti, když se změní velikost okna.

10. Klikněte **přidat omezení 4** tlačítko Přidat omezení a zavřete editor.

11. Vyberte štítek a klikněte na **omezení Editor** ikonu znovu:

    [![](hello-mac-images/xcode14.png "Přidání omezení do popisku")](hello-mac-images/xcode14.png#lightbox)

12. Kliknutím na **Red I světla** v horní, pravé a levé z **omezení Editor**, informuje štítek, který k jeho zablokování jeho dané X a Y umístění a chcete zvýšit nebo snížit při změně velikosti okna v provozu aplikace.

13. Znovu, zkontrolujte **výška** pole a použít výchozí velikost a pak klikněte na **přidat omezení 4** tlačítko Přidat omezení a zavřete editor.

14. Uložte změny do uživatelského rozhraní.

Při změně velikosti a přesouvání ovládacích prvků kolem, Všimněte si, že rozhraní tvůrce poskytuje užitečné snap pomocné parametry, které jsou založeny na [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Tyto pokyny vám umožní vývojáři vytvářet vysoké kvality aplikace, které budou mít známých vzhled a chování pro uživatele Mac.

Oblast hledání **rozhraní hierarchie** části Jak se zobrazují rozložení a hierarchii elementů, které tvoří uživatelské rozhraní:

[![](hello-mac-images/xcode15.png "Výběr elementu v hierarchii rozhraní")](hello-mac-images/xcode15.png#lightbox)

Tady můžete vybrat vývojář položky, které chcete upravit nebo přetáhněte v případě potřeby změňte pořadí prvky uživatelského rozhraní. Například pokud prvku uživatelského rozhraní se vztahuje jiný element, se může přetáhněte jej do dolní části seznamu, chcete-li nejvyšší položku v okně.

S uživatelským rozhraním, vytvořit bude třeba vývojář vystavit položky uživatelského rozhraní, aby mohli Xamarin.Mac přístup a s nimi pracovat v kódu jazyka C#. V další části **výstupy a akce**, ukazuje, jak to udělat.

<a name="Outlets_and_Actions" />

### <a name="outlets-and-actions"></a>Výstupy a akcí

Proto co jsou **výstupy** a **akce**? Na tradiční programování uživatelské rozhraní .NET, je ovládací prvek v uživatelském rozhraní automaticky přístup jako vlastnost při jejím přidání. Věcí pracují různě v systému Mac, jednoduše přidání ovládacího prvku zobrazení není usnadňují kódu. Vývojář musí explicitně vystavit element uživatelského rozhraní pro kód. Aby to udělat, Apple nabízí dvě možnosti:

-   **Výstupy** – výstupy jsou podobná vlastnosti. Pokud vývojář sváže ovládacího prvku výstupu, je vystaven na kód prostřednictvím vlastnosti, tak mohou provádět akce, jako je připojení obslužné rutiny událostí volat metody pro jeho atd.
-   **Akce** – akce jsou obdobou příkazu vzor v grafickém subsystému WPF. Například při provádění akce v ovládacím prvku, například klikněte na tlačítko, ovládacího prvku automaticky volání metody v kódu. Akce jsou výkonný a pohodlný, protože vývojář se může připojit až mnoho ovládacích prvků na stejné akce.

V Xcode **výstupy** a **akce** přidají přímo v kódu pomocí *přetahování řízení*. Přesněji řečeno, to znamená, že k vytvoření **výstupu** nebo **akce**, vývojář vybere elementu ovládacího prvku, který chcete přidat **výstupu** nebo **akce** pro podržení **řízení** klíče na klávesnici a přetáhněte ji tuto kontrolu přímo do kódu.

Pro vývojáře Xamarin.Mac, to znamená, že vývojář bude přetáhněte do souborů se zakázaným inzerováním jazyka Objective-C, které odpovídají požadované k vytvoření souboru C# **výstupu** nebo **akce**. Visual Studio pro Mac vytvořit soubor s názvem `ViewController.h` jako součást shim projektu Xcode vygeneroval Tvůrce rozhraní:

[![](hello-mac-images/xcode16.png "Zobrazení zdroje v Xcode")](hello-mac-images/xcode16.png#lightbox)

Toto se zakázaným inzerováním `.h` souboru zrcadlení `ViewController.designer.cs` , se automaticky přidá do projektu Xamarin.Mac při novou `NSWindow` je vytvořena. Tento soubor se použije k synchronizovat změny provedené při Tvůrce rozhraní a je tam, kde **výstupy** a **akce** jsou vytvořeny tak, aby se zveřejňují prvky uživatelského rozhraní pro kód C#.

<a name="Adding_an_Outlet" />

#### <a name="adding-an-outlet"></a>Přidání výstupu

S základní znalosti o **výstupy** a **akce** se vytvořit **výstupu** vystavit štítek vytvořený kódu C#.

Postupujte takto:

1. V Xcode na nejvíce vpravo nahoře dolním rohu obrazovky klikněte na tlačítko **dvojitý kroužek** tlačítko Otevřít **pomocníka Editor**:

    [![](hello-mac-images/outlet01.png "Zobrazování editoru pomocníka")](hello-mac-images/outlet01.png#lightbox)

2. Xcode dojde k přepnutí do režimu zobrazení rozdělení s **rozhraní editoru** na jedné straně a **Editor kódu** na straně druhé.

3. Všimněte si, že automaticky vybral Xcode **ViewController.m** v soubor **Editor kódu**, což je nesprávný. Z diskusi k tomu, co **výstupy** a **akce** jsou nad, potřebovat Vývojář **ViewController.h** vybrané.

4. V horní části **Editor kódu** klikněte na **automatické propojení** a vyberte `ViewController.h` souboru:

    [![](hello-mac-images/outlet02.png "Výběr správný soubor")](hello-mac-images/outlet02.png#lightbox)

5. Xcode by měl mít nyní vybrán správný soubor:

    [![](hello-mac-images/outlet03.png "Zobrazení soubor ViewController.h")](hello-mac-images/outlet03.png#lightbox)

6. **Poslední krok je velmi důležité!** Pokud vývojář neměly správný soubor vybrána, nebudou moci vytvořit **výstupy** a **akce** nebo zveřejní nesprávný třídy v jazyce C#!

7. V **rozhraní editoru**, podržte klávesu **řízení** klíče na klávesnici a klepnutím přetažením popisek vytvořili výše do editoru kódu právě níže `@interface ViewController : NSViewController {}` kódu:

    [![](hello-mac-images/outlet04.png "Vytvoření výstupu tažením")](hello-mac-images/outlet04.png#lightbox)

8. Zobrazí se dialogové okno. Ponechte **připojení** nastavena na **výstupu** a zadejte `ClickedLabel` pro **název**:

    [![](hello-mac-images/outlet05.png "Definování výstupu")](hello-mac-images/outlet05.png#lightbox)

9. Klikněte **připojit** tlačítko vytvořte **výstupu**:

    [![](hello-mac-images/outlet06.png "Zobrazení poslední výstupu")](hello-mac-images/outlet06.png#lightbox)

10. Uložte změny do souboru.

<a name="Adding_an_Action" />

#### <a name="adding-an-action"></a>Přidání akce

V dalším kroku vystavit tlačítko pro kód C#. Stejně jako popisek výše vývojář může propojit tlačítko až **výstupu**. Vzhledem k tomu, že chceme reagovat na tlačítko se klikli, použijte **akce** místo.

Postupujte takto:

1. Zkontrolujte, zda je pořád ještě v Xcode **pomocníka Editor** a **ViewController.h** souboru se zobrazí na **Editor kódu**.
2. V **rozhraní editoru**, podržte klávesu **řízení** klíče na klávesnici a přetáhněte klikněte na tlačítko vytvořili výše do editoru kódu právě níže `@property (assign) IBOutlet NSTextField *ClickedLabel;` kódu:

    [![](hello-mac-images/action01.png "Vytvoření akce tažením")](hello-mac-images/action01.png#lightbox)

3. Změna **připojení** typ **akce**:

    [![](hello-mac-images/action02.png "Definování akce")](hello-mac-images/action02.png#lightbox)

4. Zadejte `ClickedButton` jako **název**:

    [![](hello-mac-images/action03.png "Pojmenování nové akce")](hello-mac-images/action03.png#lightbox)

5. Klikněte **připojit** tlačítko vytvořte **akce**:

    [![](hello-mac-images/action04.png "Zobrazení poslední akce")](hello-mac-images/action04.png#lightbox)

6. Uložte změny do souboru.

S uživatelským rozhraním přes drátové sítě up a viditelné na kód C# přepněte zpět na Visual Studio pro Mac a nechat ji synchronizovat změny provedené v Xcode a rozhraní tvůrce.

> [!NOTE]
> Pravděpodobně trvalo dlouhou dobu na vytváření uživatelského rozhraní a **výstupy** a **akce** pro tento první aplikaci ale může jevit jako velké množství práce, ale byly zavedeny mnoho nových konceptů a byl stráven mnoho času pokrývá nové základů. Po určitou dobu cvičení práce rozhraní tvůrce, toto rozhraní a všechny jeho **výstupy** a **akce** lze vytvořit v právě minutu nebo dvě.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronizace změn s Xcode

Když vývojáři přejde zpět do Visual Studio pro Mac z Xcode, všechny změny provedené v Xcode automaticky synchronizovat s projektem Xamarin.Mac.

Vybere **ViewController.designer.cs** v **Průzkumníku řešení** zobrazíte jak **výstupu** a **akce** byla drátové nahoru v C # kód:

[![](hello-mac-images/sync01.png "Synchronizace změn s Xcode")](hello-mac-images/sync01.png#lightbox)

Všimněte si jak dvě definice v **ViewController.designer.cs** souboru:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Zarovnání pomocí definice v `ViewController.h` souboru v Xcode:

```csharp
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Visual Studio pro Mac čeká na změny **.h** souboru a poté tyto změny v příslušné automaticky synchronizuje **. designer.cs** soubor umístěte je do aplikace. Všimněte si, že **ViewController.designer.cs** je konkrétní třídu, takže nemá Visual Studio pro Mac k úpravě **ViewController.cs** který by přepsala všechny změny provedené na vývojáře k Třída.

Za normálních okolností vývojář nikdy muset otevřít **ViewController.designer.cs**, se zobrazí jako sem pro vzdělávací účely.

> [!NOTE]
> Ve většině případů se Visual Studio pro Mac automaticky najdete v části veškeré změny provedené v Xcode a synchronizovat je do projektu Xamarin.Mac. Ve vypnutém výskyt, který se synchronizace nedojde automaticky přepněte zpět na Xcode a je zpět do Visual Studio pro Mac znovu. To se obvykle ji synchronizační cyklus.

<a name="Writing_the_Code" />

## <a name="writing-the-code"></a>Psaní kódu

Uživatelské rozhraní pro vytvoření a je prvky uživatelského rozhraní, které jsou zpřístupněny kódu prostřednictvím **výstupy** a **akce**, jsme připraveni nakonec napsat kód pro Oživte program.

V této ukázkové aplikaci pokaždé, když se po kliknutí na první tlačítko, popisek zaktualizuje a zobrazí počet kliknutí na tlačítko. Chcete-li dosáhnout, otevřete `ViewController.cs` soubor pro úpravy poklepáním v **Průzkumníku řešení**:

[![](hello-mac-images/code01.png "Prohlížení souboru ViewController.cs v sadě Visual Studio pro Mac")](hello-mac-images/code01.png#lightbox)

Nejprve vytvořte proměnnou úrovni třídy v `ViewController` třída sledovat počet kliknutí, které došlo. Upravit definici třídy a nastavit jej vypadat následovně:

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Další ve stejné třídě (`ViewController`), přepsat `ViewDidLoad` metoda a přidejte kód, který počáteční zprávu pro popisek nastavit:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Použití `ViewDidLoad`, místo jiné metody, jako `Initialize`, protože `ViewDidLoad` nazývá *po* má načíst a vytvoření instance uživatelské rozhraní z operačního systému **.storyboard** souboru. Pokud vývojář se pokusila přistoupit k prvku popisek před **.storyboard** soubor má plně načíst a vytvořit instance, bude `NullReferenceException` chyba protože ovládací prvek popisek by dosud vytvořena.

Dál přidejte kód reagovat na uživatel klepnutím na tlačítko. Přidejte následující třídu k `ViewController` třídy:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Tento kód se připojí k **akce** vytvořili v Xcode a Tvůrce rozhraní a bude volána vždy, když uživatel klikne na tlačítko.

<a name="Testing_the_Application" />

## <a name="testing-the-application"></a>Testování aplikace

Je čas sestavení a spuštění aplikace a ujistěte se, že pracuje podle očekávání. Vývojář můžete sestavit a spustit v jediném kroku, nebo se mohou vytvářet bez jeho spuštění.

Vždy, když je integrovaná aplikace, můžete zvolit vývojář, jaký druh sestavení chtějí:

-   **Ladění** – zkompilován sestavení ladicí verze **.app** soubor (aplikace) s spoustu doplňující metadata, která umožňuje vývojáři k ladění, co se děje, když aplikace běží.
-   **Verze** – sestavení pro vydání vytvoří také **.app** soubor, ale neobsahuje informace o ladění, takže je menší a rychleji spustí.

Vývojář můžete vybrat typ sestavení z **konfigurace selektor** v levém horním rohu sady Visual Studio pro Mac obrazovky:

[![](hello-mac-images/run01.png "Výběr sestavení ladicí verze")](hello-mac-images/run01.png#lightbox)

<a name="Building_the_Application" />

## <a name="building-the-application"></a>Sestavení aplikace

V případě tohoto příkladu jsme právě má sestavení ladicí verze, tak zajistěte, aby **ladění** je vybrána. Sestavení aplikace nejprve stisknutím **⌘B**, nebo z **sestavení** nabídce zvolte **sestavení všechny**.

Pokud nebyly žádné chyby **sestavení úspěšné** zpráva se zobrazí v sadě Visual Studio pro Mac na stavovém řádku. Pokud došlo k chybám, zkontrolujte projektu a ujistěte se, zda byly správně provedeny výše uvedených kroků. Spustit tak, že zkontrolujete, že kód (i v prostředí Xcode a v sadě Visual Studio pro Mac) odpovídá kód v tomto kurzu.

<a name="Running_the_Application" />

## <a name="running-the-application"></a>Spuštění aplikace

Existují tři způsoby, jak spustit aplikaci:

-  Stiskněte klávesu **⌘ + Enter**.
-  Z **spustit** nabídce zvolte **ladění**.
-  Klikněte na tlačítko **přehrání** tlačítko v sadě Visual Studio pro Mac panelu nástrojů (právě vyšší **Průzkumníku řešení**).

Aplikace bude sestavení (pokud nebyl již vytvořen), spusťte v režimu ladění a zobrazí rozhraní hlavní okno:

[![](hello-mac-images/run02.png "Spuštění aplikace")](hello-mac-images/run02.png#lightbox)

Pokud je stisknuto tlačítko několikrát, je třeba aktualizovat popisek počet:

[![](hello-mac-images/run03.png "Zobrazuje výsledky klepnutím na tlačítko")](hello-mac-images/run03.png#lightbox)

<a name="Where_to_Next" />

## <a name="where-to-next"></a>Kde další

S základní informace o práci s aplikací Xamarin.Mac dolů prohlédněte si následující dokumenty, chcete-li získat podrobnější vysvětlení:

- [Úvod do scénářů](~/mac/platform/storyboards/index.md) – Tento článek obsahuje úvod do práce s scénářů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu uživatelském rozhraní aplikace pomocí scénářů a Tvůrce rozhraní pro Xcode.
- [Windows](~/mac/user-interface/window.md) – Tento článek se zabývá práci s Windows a panelů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu systému Windows a panelů v Xcode a rozhraní tvůrci načtení systému Windows a panelů z .xib soubory pomocí systému Windows a reagovat na systém Windows v kódu jazyka C#.
- [V dialogových oknech](~/mac/user-interface/dialog.md) – Tento článek se zabývá práce se dialogová okna a modální okna v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu modální systému Windows v Xcode a rozhraní tvůrci práce Standardní dialogová okna, zobrazení a odpovídá na požadavky systému Windows v kódu jazyka C#.
- [Výstrahy](~/mac/user-interface/alert.md) – Tento článek se zabývá práce s výstrahami v aplikaci Xamarin.Mac. Vysvětluje vytváření a zobrazování výstrah z kódu jazyka C# a zpracování výstrah.
- [Nabídky](~/mac/user-interface/menu.md) -nabídky se používají v různých částí uživatelského rozhraní pro aplikace systému Mac; z hlavní nabídky v horní části obrazovky, automaticky otevírané okno a kontextové nabídky, které může vyskytovat kdekoli v okně. Nabídky jsou nedílnou součástí aplikace systému Mac uživatelské prostředí. Tento článek se zabývá práce s nabídkami kakao v aplikaci Xamarin.Mac.
- [Panely nástrojů](~/mac/user-interface/toolbar.md) – Tento článek se zabývá práci s panely nástrojů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu panely nástrojů v Tvůrci Xcode a rozhraní, jak vystavit položky panelu nástrojů, které chcete kódu pomocí akce a výstupy, povolování a zakazování položky panelu nástrojů a nakonec neodpovídá na požadavky položky panelu nástrojů v kódu jazyka C#.
- [Zobrazení tabulky](~/mac/user-interface/table-view.md) – Tento článek se zabývá práci s zobrazení tabulek v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení tabulek v Tvůrci Xcode a rozhraní, jak vystavit zobrazit položky, které mají kód pomocí akce a výstupy, vyplní položky tabulky a nakonec zpracování tabulky zobrazit položky v kódu jazyka C#.
- [Zobrazení osnovy](~/mac/user-interface/outline-view.md) – Tento článek se zabývá práci s zobrazení osnovy v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení osnovy v Tvůrci Xcode a rozhraní, jak vystavit zobrazit položky osnova kódu pomocí akce a výstupy, vyplní Outline položky a nakonec neodpovídá na požadavky Outline zobrazit položky v kódu jazyka C#.
- [Zdroj seznamy](~/mac/user-interface/source-list.md) – Tento článek se zabývá práce se uvádí zdroje v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu uvádí zdroje v Tvůrci Xcode a rozhraní, jak vystavit položky uvádí zdroje kódu pomocí akce a výstupy, vyplní položky seznamu zdroje a nakonec neodpovídá na požadavky položky seznamu zdroje v kódu jazyka C#.
- [Zobrazení kolekce](~/mac/user-interface/collection-view.md) – Tento článek se zabývá práci s zobrazení kolekcí v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení kolekcí v Tvůrci Xcode a rozhraní, jak vystavit zobrazení kolekce elementy kódu pomocí akce a výstupy, vyplní zobrazení kolekcí a nakonec neodpovídá na požadavky zobrazení kolekcí v kódu jazyka C#.
- [Práce s obrázky](~/mac/app-fundamentals/image.md) – Tento článek se zabývá práce s obrázky a ikony v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu Image nutné k vytvoření aplikace ikonu a pomocí bitové kopie v kódu jazyka C# a rozhraní tvůrce na Xcode.

Doporučujeme také trvá podívejte se na [Mac ukázky Galerie](https://developer.xamarin.com/samples/mac/all/), obsahuje širokou řadu připravené k použití kódu, který vám pomůže rychle získat projektu Xamarin.Mac vypnout základů vývojář.

Příklad kompletní Xamarin.Mac aplikace, která obsahuje řadu funkcí, uživatel by uživatel očekával najít v typické aplikaci systému Mac, najdete v tématu [SourceWriter ukázkovou aplikaci](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter je editor jednoduché zdrojového kódu, který poskytuje podporu pro doplňování kódu a zvýraznění syntaxe jednoduché.

Kód SourceWriter má plně komentář, pokud je k dispozici odkazy být zadané a z klíčové technologie nebo metody na příslušné informace v dokumentaci k Xamarin.Mac příručky.

## <a name="summary"></a>Souhrn

Tento článek zahrnutých základní informace o standardní Xamarin.Mac aplikace. Vytvoření nové aplikace v sadě Visual Studio pro Mac, návrh uživatelského rozhraní v Xcode a rozhraní tvůrce, vystavení prvky uživatelského rozhraní pro C# kódu pomocí o něm zmínka **výstupy** a **akce**, přidání kódu pro práci s Prvky uživatelského rozhraní a nakonec, vytváření a testování Xamarin.Mac aplikace.

## <a name="related-links"></a>Související odkazy

- [Hello, Mac (ukázka)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
