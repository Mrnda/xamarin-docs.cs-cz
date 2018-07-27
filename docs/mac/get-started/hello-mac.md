---
title: Dobrý den, Mac – názorný postup
description: Tento dokument ukazuje, jak vytvořit aplikace Xamarin.Mac a přináší Visual Studio pro Mac, Xcode a Interface Tvůrce. Tento článek popisuje zpřístupňuje ovládacích prvků uživatelského rozhraní do kódu přes výstupy a akcí a ukazuje, jak vytvářet, spouštět a testovat aplikace Xamarin.Mac.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: f06bf6736b427a4d77ac34957d75cd321f3dae3a
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275934"
---
# <a name="hello-mac--walkthrough"></a>Dobrý den, Mac – názorný postup

Xamarin.Mac umožňuje vývoj plně nativní aplikace pro Mac v jazyce C# a .NET pomocí stejné knihovny OS X a ovládací prvky rozhraní, které se používají při vývoji v *Objective-C* a *Xcode*. Vzhledem k tomu, že Xamarin.Mac se integruje přímo s Xcode, vývojáři použít Xcode _Tvůrce rozhraní_ vytvářet uživatelské rozhraní vaší aplikace (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Kromě toho od aplikace Xamarin.Mac jsou napsané v C# a .NET, běžné back-end kód mohou sdílet pomocí mobilní aplikace Xamarin.iOS a Xamarin.Android vše při současném dodávání nativního prostředí na jednotlivých platformách.

Tento článek vás seznámí klíčové koncepty potřebné k vytváření aplikací pro Mac pomocí Xamarin.Mac, Visual Studio pro Mac a Xcode Tvůrce rozhraní návod provede postupem vytvoříte jednoduchou **Hello, Mac** aplikaci, která vrátí počet časy tlačítka se kliklo:

[![](hello-mac-images/run02.png "Příklad Hello spuštěnou aplikaci Mac")](hello-mac-images/run02.png#lightbox)

Následující koncepty se budeme:

-  **Visual Studio pro Mac** – Úvod do sady Visual Studio pro Mac a vytváření aplikací Xamarin.Mac s ním.
-  **Anatomie aplikace Xamarin.Mac** – co Xamarin.Mac aplikace se skládá.
-  **Tvůrce rozhraní Xcode** – jak používat Xcode je Tvůrce rozhraní pro definování uživatelského rozhraní aplikace.
-  **Výstupy a akce** – jak nastavit ovládací prvky v uživatelském rozhraní pomocí výstupy a akce.
-  **Nasazení a testování** – spuštění a testování aplikací pro Xamarin.Mac.


<a name="Requirements" />

## <a name="requirements"></a>Požadavky

K vývoji aplikace macOS se Xamarin.macem jsou vyžadovány následující položky:

- Počítač Mac se systémem macOS Yosemite(10.10) nebo vyšší.
- Xcode 7 a vyšší (i když se doporučuje nainstalovat nejnovější stabilní verzi z [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).)
- Nejnovější verze Xamarin.Mac a Visual Studio pro Mac.

Spuštěné aplikací systému Mac, které jsou vytvořené pomocí Xamarin.Mac mají následující požadavky:

- Počítač Mac se systémem Mac OS X 10.7 nebo vyšší.

<a name="Starting_a_new_Xamarin.Mac_App_in_Xamarin_Studio" />

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Spuštění nové aplikace Xamarin.Mac v sadě Visual Studio pro Mac

Jak je uvedeno výše, tento průvodce provede postupem vytvoření aplikace pro Mac volá `Hello_Mac` do hlavního okna, která přidá jedno tlačítko a popisek. Po kliknutí na tlačítko se zobrazí popisek počet pokusů, které se kliklo.

Abyste mohli začít, postupujte takto:

1. Spusťte sadu Visual Studio pro Mac:

    [![](hello-mac-images/setup01.png "Hlavní Visual Studio for Mac rozhraní")](hello-mac-images/setup01.png#lightbox)

2. Klikněte na **nové řešení...**  odkaz v levém horním rohu obrazovky, chcete-li otevřít **nový projekt** dialogové okno:

    [![](hello-mac-images/setup03.png "Vytvoření nového řešení v sadě Visual Studio pro Mac")](hello-mac-images/setup02.png#lightbox)

3. Vyberte **Mac** > **aplikace** > **aplikace Cocoa** a klikněte na tlačítko **Další** tlačítka:

    [![](hello-mac-images/setup03.png "Výběr aplikace Cocoa")](hello-mac-images/setup03.png#lightbox)

4. Zadejte `Hello_Mac` pro **název aplikace**a všechno ostatní jako výchozí. Klikněte na tlačítko **Další**:

    [![](hello-mac-images/setup05.png "Nastavení názvu aplikace")](hello-mac-images/setup05.png#lightbox)

4. Při vytváření řešení, které by organizace několik různých projektech, vývojář chtít nastavit jiný **název řešení** tady, ale pro účely tohoto příkladu, ponechejte tuto položku nastavena na výchozí nastavení je stejné jako  **Název projektu**:

    [![](hello-mac-images/setup04.png "Ověřuje se podrobnosti o novém řešení")](hello-mac-images/setup04.png#lightbox)

5. Klikněte na tlačítko **vytvořit** tlačítko.

Visual Studio pro Mac se nová aplikace Xamarin.Mac umožňuje vytvořit a zobrazit výchozí soubory, které se přidají do řešení aplikace:

 [![](hello-mac-images/project01.png "Nové výchozí zobrazení řešení")](hello-mac-images/project01.png#lightbox)

Visual Studio pro Mac používá **řešení** a **projekty**, přesně stejným způsobem, který sada Visual Studio. Řešení je kontejner, který může obsahovat jeden nebo více projektů; projekty lze zahrnout aplikací, podpora knihovny, testovací aplikace atd. V tomto případě sady Visual Studio pro Mac se vytvoří řešení a projekt aplikace automaticky.

V případě potřeby vývojář vytvořit kód jeden nebo více projektů knihovny, které obsahují běžnou a sdílený kód. Tyto projekty knihovny můžete využívat projektem aplikace nebo sdíleny s jinými projekty Xamarin.Mac aplikace (nebo Xamarin.iOS a Xamarin.Android založené na typu kódu), stejným způsobem jako standardní aplikace .NET.

<a name="The_Project" />

## <a name="anatomy-of-a-xamarinmac-application"></a>Anatomie aplikace Xamarin.Mac

Pokud jste obeznámeni s Iosem programování, existuje mnoho podobností. Ve skutečnosti používá iOS CocoaTouch rozhraní, což je slimmed nižší verzi Cocoa používá Mac, tak se překřížila mnoho konceptů.

Podívejte se na soubory v projektu:

-   `Main.cs` – Obsahuje hlavní vstupní bod aplikace. Když je aplikace spuštěná, tato položka obsahuje velmi první třídy a metody, která se spustí.
-   `AppDelegate.cs` – Tento soubor obsahuje třídu hlavní aplikaci, která zodpovídá za příjem událostí z operačního systému.
-   `Info.plist` – Tento soubor obsahuje vlastnosti aplikace, jako je například název aplikace, ikony, atd.
-   `Entitlements.plist` – Tento soubory obsahuje oprávnění pro aplikaci a umožňuje přístup k objektům, jako třeba podporu izolace (sandbox) a na Icloudu.
-  `Main.storyboard` – Definuje uživatelské rozhraní (Windows a nabídek) pro aplikace a nastaví rozložení propojení mezi Windows prostřednictvím přechody. Scénáře jsou soubory formátu XML, které obsahují definice zobrazení (prvky uživatelského rozhraní). Tento soubor můžete vytvářené a udržované pomocí Tvůrce rozhraní v Xcode.
-   `ViewController.cs` – To je ovladač pro hlavní okno. Kontrolery se budeme podrobně v jiném článku, ale prozatím kontroler si lze představit hlavní modul žádné konkrétní zobrazení.
-   `ViewController.designer.cs` – Tento soubor obsahuje kód zajistí funkčnost systému, který pomáhá integrovat s uživatelským rozhraním na hlavní obrazovce.

Následující části, bude trvat krátce podívat prostřednictvím některé z těchto souborů. Později budeme se zabývat podrobněji, ale je vhodné pochopit jejich základy nyní.

<a name="Main_cs" />

### <a name="maincs"></a>Main.cs

**Main.cs** souborů je velmi jednoduché. Obsahuje statický `Main` metodu, která vytvoří novou instanci aplikace Xamarin.Mac a předá název třídy, která bude zpracovávat události operačního systému, což je v tomto případě `AppDelegate` třídy:

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

`AppDelegate.cs` Obsahuje soubor `AppDelegate` třídy, který je zodpovědný za vytváření oken a naslouchá událostem operačního systému:

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

Tento kód je pravděpodobně obeznámeni, pokud vývojář má integrované aplikace pro iOS před, ale je to poměrně jednoduché.

`DidFinishLaunching` Metoda se spouští po vytvoření instance aplikace, a je zodpovědný za ve skutečnosti vytváří okno aplikace a od proces zobrazení v ní.

`WillTerminate` Metoda bude volána, když uživatel nebo systém je vytvořena instance vypnutí aplikace. Vývojáři musí tuto metodu použijte k dokončení aplikace předtím, než ho ukončí (jako je například ukládání uživatelských předvoleb nebo okna velikost a umístění).

<a name="ViewController_cs" />

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa (a při odvození CocoaTouch) používá, která se označuje jako *Model View Controller* vzor (MVC). `ViewController` Deklarace představuje objekt, který řídí okna aplikace skutečný. Obecně platí každé okno vytvořené (a mnoha dalším věcem v systému windows), je kontroler, který je zodpovědný za životního cyklu, v okně, například zobrazení, přidání nové zobrazení (ovládací prvky) ji atd.

`ViewController` Třída je kontroler hlavního okna. To znamená, že je zodpovědná za životní cyklus hlavního okna. To prozkoumá podrobněji později pro nyní přebírá rychlý přehled:

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

Soubor návrháře pro třídy hlavního okna je teď prázdný, ale se automaticky naplní se sadou Visual Studio pro Mac při vytváření uživatelského rozhraní pomocí Tvůrce rozhraní v Xcode:

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

Vývojář není obvykle obeznámeni s soubory návrháře, protože jste automaticky prováděna nástrojem Visual Studio pro Mac a pak poskytnete kód požadavku vložení, který umožňuje přístup k ovládacím prvkům, které byly přidány do jakékoli okno nebo zobrazení v aplikaci.

Vytvoření projektu aplikace Xamarin.Mac a základní znalosti o komponentách přejdete na Xcode k vytvoření uživatelského rozhraní pomocí Tvůrce rozhraní.

<a name="Info_plist" />

### <a name="infoplist"></a>Info.plist

`Info.plist` Soubor obsahuje informace o aplikaci Xamarin.Mac, jako jeho **název** a **identifikátor sady prostředků**:

[![](hello-mac-images/infoplist01.png "Visual Studio for Mac plist editor")](hello-mac-images/infoplist01.png#lightbox)

Definuje také _scénáře_ , který se použije k zobrazení uživatelského rozhraní pro Xamarin.Mac aplikaci v rámci **hlavní rozhraní** rozevíracího seznamu. V případě výše uvedeného příkladu `Main` v rozevírací nabídce má vztah k `Main.storyboard` ve stromu zdrojového kódu projektu v **Průzkumníka řešení**. Definuje také nevýhody aplikace tak, že zadáte *katalog Assetů* , který je obsahuje (**AppIcon** v tomto případě).

### <a name="entitlementsplist"></a>Do souboru Entitlements.plist

Aplikace `Entitlements.plist` soubor nároků, které aplikace Xamarin.Mac má například ovládací prvky, **Sandboxing** a **Icloudu**:

[![](hello-mac-images/entitlements01.png "Visual Studio for Mac editor oprávnění")](hello-mac-images/entitlements01.png#lightbox)

Například Hello World bude se vyžadovat žádná oprávnění. V další části ukazuje, jak upravit pomocí Tvůrce rozhraní Xcode `Main.storyboard` souborů a definování uživatelského rozhraní aplikace Xamarin.Mac.

<a name="Introduction_to_Xcode_and_Interface_Builder" />

## <a name="introduction-to-xcode-and-interface-builder"></a>Úvod do Xcode a Interface Builder

Jako součást Xcode Apple vytvořil nástroj zvaný Tvůrce rozhraní, která umožňuje vývojářům vizuálně vytvářet uživatelské rozhraní v návrháři. Xamarin.Mac plynule integruje Tvůrce rozhraní, což uživatelského rozhraní mají být vytvořeny pomocí stejných nástrojů jako uživatelé Objective-C.

Abyste mohli začít, klikněte dvakrát na `Main.storyboard` ve **Průzkumníka řešení** otevřete pro úpravy v Xcode a Interface Tvůrce:

[![](hello-mac-images/xcode01.png "Main.storyboard souborů v Průzkumníkovi řešení")](hello-mac-images/xcode01.png#lightbox)

To by měl spustit Xcode a vypadat přibližně takto:

[![](hello-mac-images/xcode02.png "Výchozí zobrazení Tvůrce rozhraní Xcode")](hello-mac-images/xcode02.png#lightbox)

Než začnete navrhovat rozhraní, trvat rychlý přehled Xcode orientujete hlavní funkce, které se použijí.

> [!NOTE]
> Použití Xcode a Tvůrce rozhraní pro vytváření uživatelského rozhraní pro aplikace Xamarin.Mac nemá vývojář, uživatelské rozhraní lze vytvořit přímo z kódu jazyka C#, který je ale nad rámec tohoto článku. Z důvodu zjednodušení ji budou používat Tvůrce rozhraní pro vytváření uživatelského rozhraní v celé zbývající části tohoto kurzu.


<a name="Components_of_Xcode" />

### <a name="components-of-xcode"></a>Součástí Xcode

Při otevírání `.storyboard` souborů v Xcode ze sady Visual Studio pro Mac, otevře se **navigátoru projektů** na levé straně, **hierarchii rozhraní** a **rozhraní editoru**uprostřed a **vlastnosti a technické vybavení budov** části na pravé straně:

[![](hello-mac-images/xcode03.png "Různé části Tvůrce rozhraní v Xcode")](hello-mac-images/xcode03.png#lightbox)

V dalších částech podívejte se na co jednotlivé funkce dělat tyto Xcode a jak se dají použít k vytvoření rozhraní pro aplikace Xamarin.Mac.

<a name="Project_Navigation" />

### <a name="project-navigation"></a>Navigace v projektu

Při otevírání `.storyboard` soubor pro úpravy v Xcode, Visual Studio pro Mac vytvoří *soubor projektu Xcode* Oznamovat změny mezi samostatně a Xcode na pozadí. Později když vývojář přepne zpět do sady Visual Studio pro Mac z nástroje Xcode, všechny změny provedené v tomto projektu jsou synchronizovány s projektem Xamarin.Mac pomocí sady Visual Studio pro Mac.

**Navigace v projektu** část umožňuje vývojářům procházet mezi všechny soubory, které tvoří to _překrytí_ projektu Xcode. Obvykle jsou pouze bude zajímat `.storyboard` jako soubory v tomto seznamu `Main.storyboard`.

<a name="Interface_Hierarchy" />

### <a name="interface-hierarchy"></a>Rozhraní hierarchie

**Hierarchii rozhraní** část umožňuje vývojářům snadný přístup k několika klíčové vlastnosti uživatelského rozhraní, jako jeho **zástupné symboly** a hlavní **okno**. V této části slouží pro přístup k jednotlivým prvkům (zobrazení), která tvoří uživatelské rozhraní a upravit způsob, jakým jsou vnořené přetažením kolem v rámci hierarchie.

<a name="Interface_Editor" />

### <a name="interface-editor"></a>Editor rozhraní

**Rozhraní editoru** čísti povrch, na kterém je uživatelské rozhraní graficky rozloženy. Prvky z přetáhnout **knihovny** část **vlastnosti a technické vybavení budov** oddílu pro vytvoření návrhu. Prvky uživatelského rozhraní (zobrazení) přidávání do návrhové plochy, se přidají do **hierarchii rozhraní** v pořadí, ve kterém jsou uvedeny v části **rozhraní editoru**.

<a name="Properties_Utilities" />

### <a name="properties--utilities"></a>Vlastnosti a technické vybavení budov

**Vlastnosti a technické vybavení budov** část je rozdělena na dvě hlavní části **vlastnosti** (také označované jako inspektory) a **knihovny**:

[![](hello-mac-images/xcode04.png "Kontrola vlastností")](hello-mac-images/xcode04.png#lightbox)

Zpočátku je v této části téměř prázdná, ale pokud vývojář je prvek v **rozhraní editoru** nebo **hierarchii rozhraní**, **vlastnosti** bude oddíl naplní informacemi o daný prvek a vlastnosti, které můžete upravit.

V rámci **vlastnosti** části, existují osm různé *inspektoru karty*, jak je znázorněno na následujícím obrázku:

[![](hello-mac-images/xcode05.png "Přehled o všech kontroly")](hello-mac-images/xcode05.png#lightbox)

<a name="Properties_Utility_Types" />

### <a name="properties--utility-types"></a>Vlastnosti & nástroje typy

Z zleva doprava jsou tyto karty:

-   **Soubor inspektoru** – The inspektoru souboru zobrazuje informace o souborech, jako je například název souboru a umístění souboru Xib, který se právě upravuje.
-   **Rychlá Nápověda** – karta rychlý nápovědy obsahuje kontextové nápovědy podle vybrané v Xcode.
-   **Kontrola identity** – The inspektoru Identity obsahuje informace o vybrané zobrazení/ovládání.
-   **Atributy inspektoru** – The inspektoru atributy umožňuje vývojářům přizpůsobit různé atributy vybraného zobrazení nebo ovládací prvek.
-   **Velikost inspektoru** – The inspektoru velikost umožňuje vývojářům řídit velikost a změna velikosti chování vybraného ovládacího prvku nebo zobrazení.
-   **Kontrola připojení** – The Kontrola připojení zobrazí **výstupu** a **akce** připojení z vybraných ovládacích prvků. Výstupy a akce probereme podrobnější rozpis naleznete níže.
-   **Kontrola vazby** – The inspektoru vazby umožňuje vývojářům pro konfiguraci ovládacích prvků tak, aby jejich hodnoty jsou automaticky svázán s datovými modely.
-   **Zobrazení inspektoru účinky** – The inspektoru účinky zobrazení umožňuje vývojářům zadat dopady na ovládací prvky, jako je například animace.

Použití **knihovny** části ovládacích prvků a objekty umístí do návrháře k vytvoření grafické uživatelské rozhraní:

[![](hello-mac-images/xcode06.png "Inspektor knihovny Xcode")](hello-mac-images/xcode06.png#lightbox)

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Vytvoření rozhraní

Se základními funkcemi integrované vývojové prostředí Xcode a Interface Builder zahrnutých Vývojář můžete vytvořit uživatelské rozhraní pro hlavní zobrazení.

Postupujte následovně:

1. V Xcode, přetáhněte **tlačítka** z **části knihovny**:

    [![](hello-mac-images/xcode07.png "Výběr NSButton z knihovny inspektoru")](hello-mac-images/xcode07.png#lightbox)

2. Přetáhněte tlačítko na **zobrazení** (v části **kontroler okna**) v **rozhraní editoru**:

    [![](hello-mac-images/xcode08.png "Přidání tlačítka pro návrh rozhraní")](hello-mac-images/xcode08.png#lightbox)

3. Klikněte na **název** vlastnost **inspektoru atribut** a změnit název tlačítka na `Click Me`:

    [![](hello-mac-images/xcode09.png "Nastavení vlastností tlačítka.")](hello-mac-images/xcode09.png#lightbox)

4. Přetáhněte **popisek** z **části knihovny**:

    [![](hello-mac-images/xcode10.png "Výběr popisku z knihovny inspektoru")](hello-mac-images/xcode10.png#lightbox)

5. Přetáhněte popisek do **okno** vedle tlačítka v **rozhraní editoru**:

    [![](hello-mac-images/xcode11.png "Přidání popisku návrh rozhraní")](hello-mac-images/xcode11.png#lightbox)

6. Získejte pravý úchyt na popisek a přetáhněte ho, dokud není u okraje okna:

    [![](hello-mac-images/xcode12.png "Změna velikosti popisek")](hello-mac-images/xcode12.png#lightbox)

7. Klikněte na tlačítko v právě přidali **rozhraní editoru**a klikněte na tlačítko **Editor omezení** ikonu a dolní části okna:

    [![](hello-mac-images/xcode13.png "Přidání omezení k tlačítku")](hello-mac-images/xcode13.png#lightbox)

8. V horní části stránky editoru, klikněte na tlačítko **Red můžu nosníky** nahoře a vlevo. Při změně velikosti okna to budete mít na tlačítko ve stejném umístění, v levém horním rohu obrazovky.

9. Dalším krokem je kontrola **výška** a **šířka** dialogových oken a použijte výchozí velikosti. Tím zůstane na tlačítko na stejnou velikost při změní velikost okna.

10. Klikněte na tlačítko **přidat omezení 4** tlačítko Přidat omezení a ukončete editor.

11. Vyberte popisek a klikněte na tlačítko **Editor omezení** znovu na ikonu:

    [![](hello-mac-images/xcode14.png "Přidání omezení do popisku")](hello-mac-images/xcode14.png#lightbox)

12. Kliknutím na **Red můžu nosníky** horní, pravé a levá strana **Editor omezení**, říká tento popisek se zablokuje dané X a Y a umístění a zvětšení a zmenšení při změně velikosti okna v běžícím aplikace.

13. Znovu zkontrolovat **výška** a použít výchozí velikost a potom klikněte na tlačítko **přidat omezení 4** tlačítko Přidat omezení a ukončete editor.

14. Uložte změny do uživatelského rozhraní.

Při změně velikosti a přesunutí kontrolních mechanismů kolem, Všimněte si, že tvůrce rozhraní poskytuje užitečné snap pokynů, které jsou založeny na [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Tyto pokyny vám umožní vývojářům vytvářet vysoce kvalitní aplikace, které se mají známé vzhled a chování pro uživatele počítačů Mac.

Podívejte se **hierarchii rozhraní** část a zobrazit, jak se zobrazí rozložení a hierarchie prvků, které tvoří uživatelského rozhraní:

[![](hello-mac-images/xcode15.png "Výběr elementu v hierarchii rozhraní")](hello-mac-images/xcode15.png#lightbox)

Odtud můžete vybrat vývojář položky, které chcete upravit nebo přetažením přeuspořádat prvky uživatelského rozhraní v případě potřeby. Například pokud jiný element se zkušebním prvku uživatelského rozhraní, že byste ho přetáhnout myší do dolní části seznamu k němu položky nejvrchnější okna.

S uživatelským rozhraním vytvořili vývojář bude muset vystavit položky uživatelského rozhraní tak, aby Xamarin.Mac může přistupovat a pracovat s nimi v kódu jazyka C#. Další části **výstupy a akce**, ukazuje, jak to provést.

<a name="Outlets_and_Actions" />

### <a name="outlets-and-actions"></a>Výstupy a akce

Tak co jsou **výstupy** a **akce**? V tradičních programování rozhraní .NET uživatele, je ovládací prvek v uživatelském rozhraní automaticky vystavena jako vlastnost při přidání. Věci pracují jinak než v systému Mac, jednoduše přidání ovládacího prvku zobrazení není usnadňují kódu. Vývojář musí vystavit explicitně prvek uživatelského rozhraní do kódu. Aby to udělat, Apple nabízí dvě možnosti:

-   **Výstupy** – výstupy jsou podobná vlastnosti. Pokud vývojář sváže ovládacího prvku do zásuvky, to je vystavená pro kód prostřednictvím vlastnosti, tak dělají kroky, jako je připojení obslužných rutin událostí, volání metod, atd.
-   **Akce** – akce jsou obdobou příkazu vzoru v subsystému WPF. Například při akci provádí na ovládací prvek, Dejme tomu, že klikněte na tlačítko, ovládací prvek automaticky volání metody v kódu. Akce jsou efektivní a pohodlný, protože vývojář, můžete nastavit mnoho ovládacích prvků na stejnou akci.

V prostředí Xcode **výstupy** a **akce** přidávají přímo do kódu přes *přetažením ovládací prvek*. Přesněji řečeno, to znamená, že k vytvoření **výstupu** nebo **akce**, vývojář bude Zvolte ovládací prvek a přidat **výstupu** nebo **akce** pro přidržení **ovládací prvek** klávesu na klávesnici a přetáhněte ovládací prvek přímo do kódu.

Pro vývojáře Xamarin.Mac, to znamená, že vývojář bude přetáhněte do souborů se zakázaným inzerováním Objective-C, které odpovídají kde chtějí vytvořit soubor C# **výstupu** nebo **akce**. Visual Studio pro Mac vytvoří soubor s názvem `ViewController.h` jako součást překrytí projektu Xcode vygenerovat použít Tvůrce rozhraní:

[![](hello-mac-images/xcode16.png "Zobrazení zdroje v Xcode")](hello-mac-images/xcode16.png#lightbox)

Toto se zakázaným inzerováním `.h` souboru zrcadlení `ViewController.designer.cs` , který je automaticky přidán do projektu Xamarin.Mac, když je nový `NSWindow` se vytvoří. Tento soubor se použije k synchronizaci změn provedených nástrojem Tvůrce rozhraní a je tam, kde **výstupy** a **akce** jsou vytvořeny tak, aby prvky uživatelského rozhraní jsou vystavená pro kód jazyka C#.

<a name="Adding_an_Outlet" />

#### <a name="adding-an-outlet"></a>Přidání výstupu

S základní znalosti o co **výstupy** a **akce** se vytvoření **výstupu** vystavit popisek vytvořený do našeho kódu C#.

Postupujte následovně:

1. V prostředí Xcode na nejvíce vpravo nahoře dolním rohu obrazovky klikněte na tlačítko **Double kruh** tlačítko Otevřít **Pomocníka s nastavením Editor**:

    [![](hello-mac-images/outlet01.png "Zobrazování editoru Pomocníka s nastavením")](hello-mac-images/outlet01.png#lightbox)

2. Xcode se přepnout na režim rozděleného zobrazení s **rozhraní editoru** na jedné straně a **Editor kódu** na druhé.

3. Všimněte si, že má automaticky rozpoznat Xcode **ViewController.m** soubor **Editor kódu**, což není správně. Z diskuse o co **výstupy** a **akce** jsou výš, vývojář bude muset mít **ViewController.h** vybrané.

4. V horní části **Editor kódu** klikněte na **automatické propojení** a vyberte `ViewController.h` souboru:

    [![](hello-mac-images/outlet02.png "Výběr správného souboru")](hello-mac-images/outlet02.png#lightbox)

5. Xcode by měl mít nyní vybraný správný soubor:

    [![](hello-mac-images/outlet03.png "Zobrazení soubor ViewController.h")](hello-mac-images/outlet03.png#lightbox)

6. **Posledním krokem je velmi důležité!** Pokud vývojář neměli vybraný správný soubor, nebudou moct vytvořit **výstupy** a **akce** nebo budou přístupné nesprávné třídy v jazyce C#!

7. V **rozhraní editoru**, podržte stisknutou klávesu **ovládací prvek** klávesu na klávesnici a klikněte na tlačítko přetáhněte popisek vytvořené výše do editoru kódu hned pod `@interface ViewController : NSViewController {}` kódu:

    [![](hello-mac-images/outlet04.png "Vytvoření zásuvky tažením")](hello-mac-images/outlet04.png#lightbox)

8. Zobrazí se dialogové okno. Nechte **připojení** nastavena na **výstupu** a zadejte `ClickedLabel` pro **název**:

    [![](hello-mac-images/outlet05.png "Definování výstupu")](hello-mac-images/outlet05.png#lightbox)

9. Klikněte na tlačítko **připojit** tlačítko vytvoříte **výstupu**:

    [![](hello-mac-images/outlet06.png "Zobrazení konečného výstupu")](hello-mac-images/outlet06.png#lightbox)

10. Uložte změny do souboru.

<a name="Adding_an_Action" />

#### <a name="adding-an-action"></a>Přidání akce

V dalším kroku vystavit na tlačítko pro kód jazyka C#. Stejně jako popisek výše, vývojář může propojit tlačítka až **výstupu**. Protože chceme reagovat na tlačítko klepnutí, použijte **akce** místo.

Postupujte následovně:

1. Ujistěte se, že je stále v Xcode **pomocníka s nastavením Editor** a **ViewController.h** je viditelný v souboru **Editor kódu**.
2. V **rozhraní editoru**, podržte stisknutou klávesu **ovládací prvek** klávesu na klávesnici a přetažením klikněte na tlačítko vytvořené výše do editoru kódu hned pod `@property (assign) IBOutlet NSTextField *ClickedLabel;` kódu:

    [![](hello-mac-images/action01.png "Vytvoření akce přetažení")](hello-mac-images/action01.png#lightbox)

3. Změnit **připojení** typ, který **akce**:

    [![](hello-mac-images/action02.png "Definování akce")](hello-mac-images/action02.png#lightbox)

4. Zadejte `ClickedButton` jako **název**:

    [![](hello-mac-images/action03.png "Pojmenování nové akce")](hello-mac-images/action03.png#lightbox)

5. Klikněte na tlačítko **připojit** pro vytvoření **akce**:

    [![](hello-mac-images/action04.png "Zobrazit poslední akce")](hello-mac-images/action04.png#lightbox)

6. Uložte změny do souboru.

S uživatelským rozhraním, přes drátové sítě nahoru a vystavená pro kód jazyka C# přepněte zpět do sady Visual Studio pro Mac a nechat ho synchronizovat změny provedené v Xcode a Interface Tvůrce.

> [!NOTE]
> Pravděpodobně trvalo dlouhou dobu na vytváření uživatelského rozhraní a **výstupy** a **akce** pro tento první aplikace a může se zdají být spoustu práce, ale byly zavedeny spoustu nových konceptů a velké množství času byl vynaložen na zahrnuje nové základu. Po nějakou dobu ocení prostředků a práci s Tvůrce rozhraní, toto rozhraní a všechny jeho **výstupy** a **akce** lze vytvořit pouze jednu minutu nebo dvě.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronizace změn s Xcode

Když vývojář přepne zpět do sady Visual Studio pro Mac z nástroje Xcode, všechny změny provedené v Xcode automaticky synchronizovat s projektem Xamarin.Mac.

Vyberte **ViewController.designer.cs** v **Průzkumníka řešení** zobrazíte jak **výstupu** a **akce** byla drátové nahoru v jazyce C# kód:

[![](hello-mac-images/sync01.png "Synchronizace změn s Xcode")](hello-mac-images/sync01.png#lightbox)

Všimněte si, že jak dvě definice v **ViewController.designer.cs** souboru:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Řádek nahoru s definicemi v `ViewController.h` souborů v Xcode:

```csharp
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Visual Studio pro Mac čeká na změny **.h** souborů a tyto změny v příslušné automaticky synchronizuje **. designer.cs** souboru vystavit do aplikace. Všimněte si, že **ViewController.designer.cs** je částečná třída, tak, aby Visual Studio for Mac nebude muset upravit **ViewController.cs** která by přepsala změnách, které vývojář provedl Třída.

Za normálních okolností vývojář nikdy muset otevřít **ViewController.designer.cs**, se zobrazí jako sem pro vzdělávací účely.

> [!NOTE]
> Ve většině případů bude Visual Studio for Mac automaticky zobrazí všechny změny provedené v Xcode a synchronizovat je se do projektu Xamarin.Mac. Vypnuto výskytů, který automaticky nestane synchronizace přepněte zpět do Xcode a pak zpátky do sady Visual Studio pro Mac znovu. Tím se spustí normálně řízený synchronizační cyklus.

<a name="Writing_the_Code" />

## <a name="writing-the-code"></a>Psaní kódu

Vytvoření uživatelského rozhraní a jeho prvky uživatelského rozhraní vystavená pro kód prostřednictvím **výstupy** a **akce**, budeme připravení nakonec napsat kód k oživení program.

Této ukázkové aplikace pokaždé, když dojde ke kliknutí na první tlačítko, popisek se aktualizují na tom, kolikrát se kliklo na tlačítko zobrazit. Chcete-li to provést, otevřete `ViewController.cs` pro úpravu poklepáním v soubor **Průzkumníka řešení**:

[![](hello-mac-images/code01.png "Zobrazení ViewController.cs souboru v sadě Visual Studio pro Mac")](hello-mac-images/code01.png#lightbox)

Nejprve vytvořte proměnnou úroveň třídy `ViewController` třídy můžete sledovat počet kliknutí, ke kterým došlo. Upravit definici třídy a nastavte ji vypadat nějak takto:

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Další ve stejné třídě (`ViewController`), přepsat `ViewDidLoad` metoda a přidejte nějaký kód nastavení počáteční zprávy popisku:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Použití `ViewDidLoad`, namísto jiné metody, jako `Initialize`, protože `ViewDidLoad` nazývá *po* načetlo a vytvoření instance uživatelského rozhraní z operačního systému **.storyboard** souboru. Pokud vývojář se pokusili získat přístup na ovládací prvek popisku před **.storyboard** soubor má plně načten a instance, mají `NullReferenceException` chyby vzhledem k tomu, že ovládací prvek popisku by ještě neexistuje.

Dále přidejte kód, reagovat na uživatele, klikněte na tlačítko. Přidejte částečnou metodu `ViewController` třídy:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Tento kód se připojí k **akce** v Xcode a Interface tvůrce vytvoří a bude volán vždy, když uživatel klikne na tlačítko.

<a name="Testing_the_Application" />

## <a name="testing-the-application"></a>Testování aplikace

Je čas sestavíte a spustíte aplikaci, abyste měli jistotu, že pracuje podle očekávání. Vývojář můžete sestavit a spustit vše v jednom kroku, nebo ji mohl sestavit bez nutnosti jeho spuštění.

Pokaždé, když je aplikace sestavená, můžete zvolit vývojáři chtějí jaký typ sestavení:

-   **Ladění** – je zkompilován sestavení pro ladění **.app** soubor (aplikace) s spoustu další metadata, která umožňuje vývojářům ladit, co se děje, když aplikace běží.
-   **Uvolnění** – také vytvoří sestavení pro vydání **.app** soubor, ale neobsahuje informace o ladění, proto je menší a rychlejší spustí.

Vývojář můžete vybrat typ sestavení z **selektor konfigurace** v levém horním rohu sady Visual Studio pro Mac obrazovky:

[![](hello-mac-images/run01.png "Výběr sestavení pro ladění")](hello-mac-images/run01.png#lightbox)

<a name="Building_the_Application" />

## <a name="building-the-application"></a>Sestavení aplikace

V tomto příkladu jsme chcete jenom sestavení pro ladění, tak zajistěte, aby **ladění** zaškrtnuto. Nejdřív sestavit aplikaci stisknutím **⌘B**, nebo **sestavení** nabídce zvolte **sestavení všechny**.

Pokud nebyly žádné chyby **sestavení proběhlo úspěšně** se zobrazí zpráva v sadě Visual Studio pro Mac stavový řádek. Pokud byly nějaké chyby, zkontrolujte projektu a ujistěte se, že se výše uvedené kroky provedli správně. Začněte tak, že zkontrolujete, že kód (i v prostředí Xcode a v sadě Visual Studio pro Mac) neodpovídá kódu v tomto kurzu.

<a name="Running_the_Application" />

## <a name="running-the-application"></a>Spuštění aplikace

Existují tři způsoby, jak spustit aplikaci:

-  Stisknutím klávesy **⌘ + Enter**.
-  Z **spustit** nabídce zvolte **ladění**.
-  Klikněte na tlačítko **Přehrát** tlačítko v sadě Visual Studio pro Mac nástrojů (přímo nad **Průzkumníka řešení**).

Aplikace sestavení (Pokud není již vytvořená), spuštění v režimu ladění a zobrazení jeho hlavní rozhraní okna:

[![](hello-mac-images/run02.png "Spuštění aplikace")](hello-mac-images/run02.png#lightbox)

Pokud je stisknuto tlačítko několikrát, je třeba aktualizovat popisek počet:

[![](hello-mac-images/run03.png "Zobrazuje výsledky kliknutím na tlačítko")](hello-mac-images/run03.png#lightbox)

<a name="Where_to_Next" />

## <a name="where-to-next"></a>Pokud na další

Se základy práce s aplikací Xamarin.Mac dolů podívejte se na následující dokumenty, chcete-li získat podrobnější vysvětlení:

- [Úvod do scénářů](~/mac/platform/storyboards/index.md) – Tento článek obsahuje úvod k práci s použitím scénářů v aplikaci pro Xamarin.Mac. Popisuje vytvoření a údržbu Uživatelském rozhraní aplikace pomocí scénářů a Tvůrce rozhraní Xcode.
- [Windows](~/mac/user-interface/window.md) – Tento článek popisuje práci s Windows a panelů aplikace Xamarin.Mac. Popisuje vytvoření a údržbu Windows a panelů v Xcode a Interface Tvůrce načtení Windows a panelů z soubory .xib Windows pomocí a reagovat na Windows v kódu jazyka C#.
- [Dialogová okna](~/mac/user-interface/dialog.md) – Tento článek popisuje práci s dialogových oken a modální Windows aplikace Xamarin.Mac. Popisuje vytvoření a údržbu modální Windows v Xcode a Interface Tvůrce práce s standardní dialogová okna, zobrazení a reakce na Windows v kódu jazyka C#.
- [Výstrahy](~/mac/user-interface/alert.md) – Tento článek se týká práce s výstrahami v aplikace Xamarin.Mac. Popisuje vytváření a zobrazování oznámení z kódu jazyka C# a reagovat na výstrahy.
- [Nabídky](~/mac/user-interface/menu.md) -nabídky se používají v různých částí uživatelského rozhraní pro aplikace systému Mac; z hlavní nabídky aplikace v horní části obrazovky, místní a kontextové nabídky, které může vyskytovat kdekoli v okně. Nabídky jsou nedílnou součástí aplikace systému Mac uživatelské prostředí. Tento článek se týká práce s nabídkami Cocoa aplikace Xamarin.Mac.
- [Panely nástrojů](~/mac/user-interface/toolbar.md) – Tento článek popisuje práci s panelů nástrojů v aplikaci Xamarin.Mac. Popisuje vytvoření a údržbu panelů nástrojů v Xcode a Interface tvůrce, jak vystavit položky panelu nástrojů do kódu pomocí výstupy a akcí, povolení a zakázání položky panelu nástrojů a nakonec zpracování položky panelu nástrojů v kódu jazyka C#.
- [Zobrazení tabulky](~/mac/user-interface/table-view.md) – Tento článek popisuje práci se zobrazení tabulky do aplikace Xamarin.Mac. Popisuje vytvoření a údržbu zobrazení tabulek v Xcode a Interface tvůrce, jak vystavit zobrazit položky, které mají kód pomocí výstupy a akce, naplnění položky tabulky a nakonec zpracování tabulky zobrazení položek v kódu jazyka C#.
- [Zobrazení osnovy](~/mac/user-interface/outline-view.md) – Tento článek popisuje práci s zobrazení osnov aplikace Xamarin.Mac. Popisuje vytvoření a údržbu zobrazení osnov v Xcode a Interface tvůrce, jak vystavit položky zobrazení osnovy kódu pomocí výstupy a akcí, naplnění položek osnovy a nakonec zpracování osnovy zobrazení položek v kódu jazyka C#.
- [Zdroj seznamy](~/mac/user-interface/source-list.md) – Tento článek popisuje práci se seznamy zdroj aplikace Xamarin.Mac. Popisuje vytvoření a údržbu zdroje jsou uvedeny v Xcode a Interface tvůrce, jak vystavit zdroj obsahuje seznam položek s kódem pomocí výstupy a akcí, naplnění položek seznamu zdroje a nakonec reakce na zdroj položky seznamu v kódu jazyka C#.
- [Zobrazení kolekcí](~/mac/user-interface/collection-view.md) – Tento článek popisuje práci s zobrazení kolekcí aplikace Xamarin.Mac. Popisuje vytvoření a údržbu zobrazení kolekcí v Xcode a Interface tvůrce, jak vystavit prvky zobrazení kolekcí, které mají kód pomocí výstupy a akcí, naplnění zobrazení kolekcí a nakonec reakce na zobrazení kolekcí v kódu jazyka C#.
- [Práce s obrázky](~/mac/app-fundamentals/image.md) – Tento článek se týká práce s obrázky a ikony aplikace Xamarin.Mac. Popisuje vytvoření a správa imagí musí provést k vytvoření ikonu a použití Imagí v kódu jazyka C# a Tvůrce rozhraní Xcode vaší aplikace.

Doporučujeme také a podívejte se na [Galerie ukázek na Mac](https://developer.xamarin.com/samples/mac/all/), zahrnuje celou řadu připravených k použití kódu, která pomůže vývojáři rychle získat projekt Xamarin.Mac firmu.

Příklad vytvoří kompletní Xamarin.Mac aplikaci, která zahrnuje celou řadu funkcí, které uživatel byste očekávali v typické aplikaci systému Mac, najdete v tématu [SourceWriter ukázkovou aplikaci](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter je jednoduché zdroj editor kódu, který poskytuje podporu pro doplňování kódu a zvýrazňování syntaxe jednoduché.

Kód SourceWriter byly plně nezadal komentář. a, pokud je k dispozici, odkazy mají být k dispozici z klíčových technologií nebo metody relevantní informace v dokumentaci k Průvodci Xamarin.Mac.

## <a name="summary"></a>Souhrn

Tento článek popisuje základní informace o standardní aplikace Xamarin.Mac. Zahrnuté vytvořit novou aplikaci v sadě Visual Studio pro Mac, návrh uživatelského rozhraní v Xcode a Interface tvůrce, vystavení prvky uživatelského rozhraní pro C# kód pomocí **výstupy** a **akce**, přidání kódu pro práci s Prvky uživatelského rozhraní a nakonec, sestavování a testování aplikací pro Xamarin.Mac.

## <a name="related-links"></a>Související odkazy

- [Dobrý den, Mac (ukázka)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
