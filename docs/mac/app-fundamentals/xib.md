---
title: .XIb soubory
description: Tento článek se zabývá práci s .xib soubory vytvořené v Xcode na tvůrce rozhraní k vytváření a údržbě uživatelského rozhraní pro aplikaci Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c1f575f5d3d5f0fbe82d5e0d08103b9261944602
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xib-files"></a>.XIb soubory

_Tento článek se zabývá práci s .xib soubory vytvořené v Xcode na tvůrce rozhraní k vytváření a údržbě uživatelského rozhraní pro aplikaci Xamarin.Mac._

> [!NOTE]
> Upřednostňovaný způsob, jak vytvořit uživatelské rozhraní pro aplikaci Xamarin.Mac je s scénářů. Tato dokumentace byla ponechána v místě historických důvodů a pro práci s starší Xamarin.Mac projekty. Další informace najdete v tématu naše [Úvod do scénářů](~/mac/platform/storyboards/index.md) dokumentaci.

## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup do stejné prvky uživatelského rozhraní a nástroje, které vývojář pracující *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a Udržovat uživatelská rozhraní (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Soubor .xib se používá v systému macOS k definování prvky vaší aplikace uživatelské rozhraní (například nabídky, Windows, zobrazení, popisky, textových polí), které se vytvářejí a spravují graficky v Xcode na rozhraní tvůrce.

[![Příkladem spuštěné aplikaci](xib-images/intro01.png "příklad spuštěné aplikaci")](xib-images/intro01-large.png#lightbox)

V tomto článku vám nabídneme základní informace o práci se soubory .xib v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, jak vysvětluje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` atributy Umožňuje propojit až tříd jazyka C# a uživatelského rozhraní jazyka Objective-C objekty elementy.


## <a name="introduction-to-xcode-and-interface-builder"></a>Úvod do Xcode a Tvůrce rozhraní

V rámci Xcodu Apple vytvořil nástroj volat rozhraní tvůrce, který vám umožní vytvořit uživatelské rozhraní vizuálně v návrháři. Xamarin.Mac fluently integruje s rozhraní tvůrce, umožňuje vytvářet uživatelské rozhraní pomocí stejných nástrojů, které provést uživatelé jazyka Objective-C.


### <a name="components-of-xcode"></a>Součástí Xcode

Když otevřete soubor .xib v Xcode ze sady Visual Studio pro Mac, otevře se **navigátoru projektů** na levé straně, **rozhraní hierarchie** a **rozhraní editoru** uprostřed a **vlastnosti & Nástroje** části na pravé straně:

[![Komponenty rozhraní Xcode](xib-images/xcode03.png "součásti uživatelského rozhraní Xcode")](xib-images/xcode03-large.png#lightbox)

Podívejme se na co každý z těchto částech Xcode nepodporuje a jak je bude používat k vytvoření rozhraní pro vaši aplikaci Xamarin.Mac.


#### <a name="project-navigation"></a>Navigace projektu

Otevřete soubor .xib pro úpravy v Xcode, Visual Studio pro Mac vytvoří soubor projektu Xcode na pozadí pro komunikaci změny mezi samostatně a Xcode. Později když přepnete zpět do Visual Studio pro Mac od Xcode, změny provedené v tomto projektu jsou synchronizovány s Xamarin.Mac projekt Visual Studio for Mac.

**Projektu navigační** část vám pomůže přecházet mezi všechny soubory, které tvoří to _shim_ projektu Xcode. Obvykle bude pouze zájem o .xib soubory v tomto seznamu, jako **MainMenu.xib** a **MainWindow.xib**.


#### <a name="interface-hierarchy"></a>Rozhraní hierarchie

**Rozhraní hierarchie** část vám pomůže snadno přístupu několik klíčových vlastností uživatelského rozhraní, jako například jeho **zástupné symboly** a hlavní **okno**. V této části můžete použít také k jednotlivé elementy (zobrazení), které uživatelské rozhraní a upravit tak, že se nejedná o vnořené jejich kolem přetažením v rámci hierarchie.


#### <a name="interface-editor"></a>Editor rozhraní

**Rozhraní editoru** část obsahuje povrchu, na kterém jste graficky rozložení vaše uživatelské rozhraní. Budete přetáhněte elementy z **knihovny** části **vlastnosti & Nástroje** oddílu pro vytvoření návrhu. Při přidávání prvky uživatelského rozhraní (zobrazení) na návrhovou plochu, budou přidány do **rozhraní hierarchie** v pořadí, ve kterém se zobrazují v části **rozhraní editoru**.


#### <a name="properties--utilities"></a>Vlastnosti & Nástroje

**Vlastnosti & Nástroje** část je rozdělena na dvě hlavní části, které pracujeme, **vlastnosti** (také nazývané inspektoři) a **knihovny**:

![Vlastnost Inspector](xib-images/xcode04.png "Inspector vlastnost")

Zpočátku je v této části téměř prázdný, ale pokud vyberete element v **rozhraní editoru** nebo **rozhraní hierarchie**, **vlastnosti** vyplní část informace o daného elementu a vlastnosti, které můžete upravit.

V rámci **vlastnosti** části, se liší 8 *Inspector karty*, jak je znázorněno na následujícím obrázku:

[![Přehled všech inspektoři](xib-images/xcode05.png "přehled všechny kontroly")](xib-images/xcode05-large.png#lightbox)

Z zleva doprava jsou těchto karet:

-   **Soubor Inspector** – v souboru Inspector zobrazuje informace o souborech, jako je název souboru a umístění souboru Xib, který je zpracováván.
-   **Rychlé nápovědy** – karta rychlé pomoci poskytuje kontextové nápovědy podle položky vybrané v Xcode.
-   **Identity Inspector** – The Identity Inspector obsahuje informace o vybrané zobrazení/ovládacího prvku.
-   **Atributy Inspector** – atributy Inspector umožňuje přizpůsobit různé atributy vybrané zobrazení/ovládacího prvku.
-   **Velikost Inspector** – velikost Inspector umožňuje řídit velikost a změna velikosti chování vybrané zobrazení/ovládacího prvku.
-   **Připojení Inspector** – The Inspector připojení znázorňuje připojení výstupu a akce vybrané ovládacích prvků. Podíváme výstupy a akce jenom za chvíli.
-   **Vazby Inspector** – vazby Inspector umožňuje konfiguraci ovládacích prvků tak, aby jejich hodnoty jsou automaticky vázány na datové modely.
-   **Zobrazit důsledky Inspector** – v zobrazení důsledky Inspector umožňuje určit důsledky pro ovládací prvky, jako je například animace.

V **knihovny** části, můžete najít ovládací prvky a objekty, které chcete umístit do návrháře k graficky sestavení uživatelské rozhraní:

![Příkladem Inspector knihovny](xib-images/xcode06.png "příklad Inspector knihovny")

Teď, když se seznámíte s Xcode IDE a rozhraní tvůrce, podíváme se na jej použijete k vytvoření uživatelského rozhraní.


## <a name="creating-and-maintaining-windows-in-xcode"></a>Vytváření a údržbu systému windows v Xcode

Preferovaná metoda pro vytvoření aplikace na Xamarin.Mac uživatelské rozhraní je s scénářů (najdete v tématu naše [Úvod do scénářů](~/mac/platform/storyboards/index.md) Další informace naleznete v dokumentaci) a v důsledku toho spuštění jakékoli nový projekt v Xamarin.Mac bude ve výchozím nastavení používá scénářů.

Přepnout na používání .xib na základě uživatelského rozhraní, postupujte takto:

1. Otevřete Visual Studio pro Mac a spusťte nový projekt Xamarin.Mac.
2. V **řešení Pad**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...**
3. Vyberte **Mac** > **řadiče Windows**:

    ![Přidávání nového řadiče okno](xib-images/setup00.png "přidávání nového řadiče okna")
4. Zadejte `MainWindow` pro název a klikněte **nový** tlačítko:

    ![Přidání nové hlavní okno](xib-images/setup01.png "přidáním nového okna Main")
5. Znovu klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...**
6. Vyberte **Mac** > **hlavní nabídky**:

    ![Přidání nové hlavní nabídky](xib-images/setup02.png "přidání nové hlavní nabídky")
7. Ponechejte pole název jako `MainMenu` a klikněte na **nový** tlačítko.
8. V **řešení Pad** vyberte **Main.storyboard** souborů klikněte pravým tlačítkem a vyberte **odebrat**:

    ![Výběr hlavní storyboard](xib-images/setup03.png "výběr hlavní storyboard")
9. V dialogovém okně odebrat, klikněte **odstranit** tlačítko:

    ![Potvrzení odstranění](xib-images/setup04.png "potvrzení odstranění")
10. V **řešení Pad**, dvakrát klikněte **Info.plist** soubor otevřete pro úpravy.
11. Vyberte `MainMenu` z **hlavní rozhraní** rozevíracího seznamu:

    [![Nastavení v hlavní nabídce](xib-images/setup05.png "nastavení z hlavní nabídky")](xib-images/setup05-large.png#lightbox)
12. V **Pad řešení**, dvakrát klikněte **MainMenu.xib** soubor otevřete pro úpravy v Xcode na rozhraní tvůrce.
13. V **knihovny Inspector**, typ `object` do pole hledání přetáhněte novou **objekt** na návrhovou plochu:

    [![Úpravy v hlavní nabídce](xib-images/setup06.png "úpravy v hlavní nabídce")](xib-images/setup06-large.png#lightbox)
14. V **Identity Inspector**, zadejte `AppDelegate` pro **třída**:

    [![Výběr aplikace delegáta](xib-images/setup07.png "výběr delegáta aplikace")](xib-images/setup07-large.png#lightbox)
15. Vyberte **vlastník souboru** z **rozhraní hierarchie**, přepněte do **Inspector připojení** a přetáhněte ji na řádku z delegátovi, aby se `AppDelegate` **Objekt** právě přidali do projektu:

    [![Připojení aplikace delegáta](xib-images/setup08.png "připojení delegáta aplikace")](xib-images/setup08-large.png#lightbox)
16. Uložte změny a vrátit k sadě Visual Studio for Mac.

Všechny tyto změny zavedené upravit **AppDelegate.cs** souboru a nastavit jej vypadat třeba takto:

```csharp
using AppKit;
using Foundation;

namespace MacXib
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public MainWindowController mainWindowController { get; set; }

        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
            mainWindowController = new MainWindowController ();
            mainWindowController.Window.MakeKeyAndOrderFront (this);
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Teď hlavní okno aplikace je definována v **.xib** automaticky zahrnutý v projektu při přidávání řadič okno. Chcete-li upravit návrh vašeho systému windows v **Pad řešení**, dvakrát klikněte na tlačítko **MainWindow.xib** souboru:

![Vyberte soubor MainWindow.xib](xib-images/edit01.png "výběru MainWindow.xib souboru")

Otevře se okno návrhu v Xcode na rozhraní Tvůrce:

[![Úpravy MainWindow.xib](xib-images/edit02.png "úpravy MainWindow.xib")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>Standardní okno pracovního postupu

Pro jakékoli okno, které vytvoříte a pracovat v aplikaci Xamarin.Mac proces je v podstatě stejný:

1. Pro nové windows, které nejsou výchozím nastavení automaticky přidat do projektu přidejte do projektu novou definici okna.
2. Poklikejte na soubor .xib otevřete okno návrh pro úpravy v Xcode na rozhraní tvůrce.
3. Nastavte všechny vlastnosti požadované okno **atribut Inspector** a **velikost Inspector**.
4. Přetáhněte v ovládacích prvcích potřebné k sestavení vaše rozhraní a je v konfiguraci **atribut Inspector**.
5. Použití **velikost Inspector** zpracovat změny velikosti pro prvky vaši uživatelského rozhraní.
6. Vystavení okna prvky uživatelského rozhraní pro kód C# prostřednictvím výstupy a akce.
7. Uložte změny a přepněte zpět na Visual Studio pro Mac k synchronizaci s Xcode.


### <a name="designing-a-window-layout"></a>Navrhování rozložení okna

Proces pro vytvoření rozložení uživatelské rozhraní v Tvůrci rozhraní je v podstatě stejný pro každý element, který je přidat:

1. Najít požadované ovládacího prvku **knihovny Inspector** a přetáhněte ji do **rozhraní editoru** a umístěte ho.
2. Nastavte všechny vlastnosti požadované okno **atribut Inspector**.
3. Použití **velikost Inspector** zpracovat změny velikosti pro prvky vaši uživatelského rozhraní.
4. Pokud používáte vlastní třídy, nastavte ji **Identity Inspector**.
5. Vystavení elementy uživatelského rozhraní pro kód C# prostřednictvím výstupy a akce.
6. Uložte změny a přepněte zpět na Visual Studio pro Mac k synchronizaci s Xcode.

Příklad:

1. V Xcode, přetáhněte ji **tlačítko** z **knihovny části**:

    [![Výběrem tlačítka z knihovny](xib-images/xcode07.png "výběr tlačítka z knihovny")](xib-images/xcode07-large.png#lightbox)
2. Vyřaďte tlačítko na **okno** v **rozhraní editoru**:

    [![Přidání tlačítka do okna](xib-images/xcode08.png "přidání tlačítka do okna")](xib-images/xcode08-large.png#lightbox)
3. Klikněte na **název** vlastnost **atribut Inspector** a změňte název na tlačítko pro `Click Me`:

    ![Nastavení atributů tlačítko](xib-images/xcode09.png "nastavení atributů tlačítko")
4. Přetáhněte **popisek** z **knihovny části**:

    [![Výběr štítek v knihovně](xib-images/xcode10.png "výběru štítku v knihovně")](xib-images/xcode10-large.png#lightbox)
5. Vyřaďte popisek na **okno** vedle tlačítka na **rozhraní editoru**:

    [![Přidání do okna štítek](xib-images/xcode11.png "přidání štítek do okna")](xib-images/xcode11-large.png#lightbox)
6. Získat popisovač vpravo na štítek a přetáhněte ji, dokud nebude u okraje okna:

    [![Změna velikosti popisek](xib-images/xcode12.png "Změna velikosti popisku")](xib-images/xcode12-large.png#lightbox)
7. S popiskem, vyberte ji v **rozhraní editoru**, přepnout **velikost Inspector**:

    ![Výběr Inspector velikost](xib-images/xcode13.png "výběr Inspector velikost")
8. V **Automatická změna velikosti pole** klikněte na tlačítko **Dim závorky Red** vpravo a **Dim Red vodorovné šipku** v centru:

    ![Úprava vlastností Automatická změna velikosti](xib-images/xcode14.png "úpravě vlastností Automatická změna velikosti")
9. Tím se zajistí, že bude popisek roztáhnou tak, aby zvětšovat a zmenšovat při změně velikosti okna v běžící aplikaci. **Závorky Red** a horní a levé straně **Automatická změna velikosti pole** pole říct štítek, který chcete zastavit jeho dané X a Y umístění.
10. Uloží změny do uživatelského rozhraní

Jako byly změny velikosti a přesouvání ovládacích prvků kolem, měli jste si všimli, že rozhraní tvůrce poskytuje užitečné snap pomocné parametry, které jsou založeny na [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Tyto pokyny vám umožní vytvořit vysoce kvalitního aplikace, které budou mít známých vzhled a chování pro uživatele Mac.

Pokud oblast hledání **rozhraní hierarchie** část, Všimněte si, jak jsou uvedeny rozložení a hierarchii elementů, které tvoří naše uživatelské rozhraní:

![Výběr elementu v hierarchii rozhraní](xib-images/xcode15.png "výběr elementu v hierarchii rozhraní")

Tady můžete vybrat položky, které chcete upravit nebo přetáhněte v případě potřeby změňte pořadí prvky uživatelského rozhraní. Například pokud prvku uživatelského rozhraní se vztahuje jiný element, vám může přetáhněte jej do dolní části seznamu, chcete-li nejvyšší položku v okně.

Další informace o práci s Windows v aplikaci Xamarin.Mac, najdete v tématu naše [Windows](~/mac/user-interface/window.md) dokumentaci.


## <a name="exposing-ui-elements-to-c-code"></a>Vystavení prvky uživatelského rozhraní pro kód C#

Po dokončení rozložení vzhledu a chování uživatelského rozhraní v Tvůrci rozhraní budete potřebovat ke zveřejnění prvky uživatelského rozhraní tak, aby je přístupná z kódu jazyka C#. K tomu, které budete používat akce a výstupy.


### <a name="setting-a-custom-main-window-controller"></a>Nastavení vlastní hlavní okno řadiče

Abyste mohli vytvořit výstupy a akce, která zveřejňuje prvky uživatelského rozhraní pro kód C#, aplikace Xamarin.Mac muset používat řadič okno Vlastní.

Postupujte takto:

1. V Tvůrci rozhraní na Xcode otevřete Storyboard aplikace.
2. Vyberte `NSWindowController` v návrhovou plochu.
3. Přepnout **Identity Inspector** zobrazení a zadejte `WindowController` jako **název třídy**:

    [![Úprava názvu třídy](xib-images/windowcontroller01.png "úpravy názvu – třída")](xib-images/windowcontroller01-large.png#lightbox)
4. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci.
5. A **WindowController.cs** soubor bude přidán do projektu v **řešení Pad** v sadě Visual Studio pro Mac:

    ![Nový název třídy v sadě Visual Studio pro Mac](xib-images/windowcontroller02.png "nový název třídy v sadě Visual Studio pro Mac")
6. Znovu otevřete Storyboard v Tvůrci rozhraní pro Xcode.
7. **WindowController.h** soubor bude k dispozici pro použití:

    [![Odpovídající soubor .h v Xcode](xib-images/windowcontroller03.png "odpovídající soubor .h v Xcode")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Výstupy a akcí

Proto co jsou výstupy a akce? Na tradiční programování uživatelské rozhraní .NET, je ovládací prvek v uživatelském rozhraní automaticky přístup jako vlastnost při jejím přidání. Věcí pracují různě v systému Mac, jednoduše přidání ovládacího prvku zobrazení není usnadňují kódu. Vývojář musí explicitně vystavit element uživatelského rozhraní pro kód. Aby to udělat, Apple nám poskytuje dvě možnosti:

-  **Výstupy** – výstupy jsou podobná vlastnosti. Pokud propojit do ovládacího prvku do zásuvky je vystaven kódu prostřednictvím vlastnosti, tak můžete provést akce, jako je připojení obslužné rutiny událostí, volání metod ho, např.
-  **Akce** – akce jsou obdobou příkazu vzor v grafickém subsystému WPF. Například při provádění akce v ovládacím prvku, například klikněte na tlačítko, ovládacího prvku automaticky volání metody v kódu. Akce jsou výkonný a pohodlný, protože může připojit až mnoho ovládacích prvků na stejné akce.

V Xcode, výstupy a akce se přidají přímo v kódu pomocí *přetahování řízení*. Přesněji řečeno, to znamená, že pokud chcete vytvořit výstupu nebo akce, zvolíte které elementu ovládacího prvku, který chcete přidat výstupu nebo akce, podržte klávesu **řízení** na klávesnici tlačítko a přetáhněte ji tuto kontrolu přímo do kódu.

Pro vývojáře Xamarin.Mac to znamená, přetáhněte do souborů se zakázaným inzerováním jazyka Objective-C, které odpovídají do souboru C# kde chcete vytvořit výstupu nebo akce. Visual Studio pro Mac vytvořit soubor s názvem **MainWindow.h** jako součást shim projektu Xcode vygeneroval Tvůrce rozhraní:

[![Příklad souboru .h v Xcode](xib-images/xcode16.png "příklad souboru .h v Xcode")](xib-images/xcode16-large.png#lightbox)

Tento soubor se zakázaným inzerováním .h zrcadlí **MainWindow.designer.cs** , se automaticky přidá do projektu Xamarin.Mac při novou `NSWindow` je vytvořena. Tento soubor se použije k synchronizovat změny provedené při Tvůrce rozhraní a je, kde vytvoříme výstupy a akce, aby se zveřejňují prvky uživatelského rozhraní pro kód C#.


#### <a name="adding-an-outlet"></a>Přidání výstupu

S základní znalosti o jaké jsou výstupy a akcí, podíváme se na vytváření výstupu vystavit prvku uživatelského rozhraní pro kód C#.

Postupujte takto:

1. V Xcode na nejvíce vpravo nahoře dolním rohu obrazovky klikněte na tlačítko **dvojitý kroužek** tlačítko Otevřít **pomocníka Editor**:

    [![Výběr editoru pomocníka](xib-images/outlet01.png "výběr pomocníka editoru")](xib-images/outlet01-large.png#lightbox)
2. Xcode dojde k přepnutí do režimu zobrazení rozdělení s **rozhraní editoru** na jedné straně a **Editor kódu** na straně druhé.
3. Všimněte si, že automaticky vybral Xcode **MainWindowController.m** v soubor **Editor kódu**, což je nesprávný. Pokud jste si z našich zabývat výstupy a akce se výše, je potřeba mít **MainWindow.h** vybrané.
4. V horní části **Editor kódu** klikněte na **automatické propojení** a vyberte **MainWindow.h** souboru:

    [![Výběr souboru správné h](xib-images/outlet02.png "výběr souboru správné h")](xib-images/outlet02-large.png#lightbox)
5. Xcode by měl mít nyní vybrán správný soubor:

    [![Vybraný správný soubor](xib-images/outlet03.png "vybrán správný soubor")](xib-images/outlet03-large.png#lightbox)
6. **Poslední krok je velmi důležité!** Pokud nemáte vybrán správný soubor, nebude možné vytvořit výstupy a akce nebo se zveřejní nesprávný třídy v jazyce C#!
7. V **rozhraní editoru**, podržte klávesu **řízení** klíče na klávesnici a klepnutím přetažením popisek jsme vytvořili výše do editoru kódu právě níže `@interface MainWindow : NSWindow { }` kódu:

    [![Přetažení k vytvoření nové výstupu](xib-images/outlet04.png "přetažení k vytvoření nové výstupu")](xib-images/outlet04-large.png#lightbox)
8. Zobrazí se dialogové okno. Ponechte **připojení** nastavena na výstupu a zadejte `ClickedLabel` pro **název**:

    [![Nastavení vlastností výstupu](xib-images/outlet05.png "nastavení vlastností výstupu")](xib-images/outlet05-large.png#lightbox)
9. Klikněte **Connect** tlačítko pro vytvoření výstupu:

    ![Dokončené výstupu](xib-images/outlet06.png "dokončené výstupu")
10. Uložte změny do souboru.


#### <a name="adding-an-action"></a>Přidání akce

V dalším kroku Podíváme se na vytváření akci, kterou chcete vystavit interakce uživatele s element uživatelského rozhraní pro kód C#.

Postupujte takto:

1. Zkontrolujte, zda jsme jsou pořád ještě v **pomocníka Editor** a **MainWindow.h** souboru se zobrazí na **Editor kódu**.
2. V **rozhraní editoru**, podržte klávesu **řízení** klíče na klávesnici a přetáhněte klikněte na tlačítko jsme vytvořili výše do editoru kódu právě níže `@property (assign) IBOutlet NSTextField *ClickedLabel;` kódu:

    [![Přetažení vytvořit akce](xib-images/action01.png "přetažení vytvořit akce")](xib-images/action01-large.png#lightbox)
3. Změna **připojení** typ akce:

    [![Vyberte typ akce](xib-images/action02.png "vyberte typ akce")](xib-images/action02-large.png#lightbox)
4. Zadejte `ClickedButton` jako **název**:

    [![Konfigurace akce](xib-images/action03.png "konfigurace akce")](xib-images/action03-large.png#lightbox)
5. Klikněte **Connect** tlačítko vytvořte akce:

    ![Dokončené akce](xib-images/action04.png "dokončené akce")
6. Uložte změny do souboru.

Pomocí uživatelského rozhraní drátové nahoru a viditelné na kód C# přepněte zpět na Visual Studio pro Mac a nechat ji synchronizujte změny z Xcode a rozhraní tvůrce.


### <a name="writing-the-code"></a>Psaní kódu

Vytvoření uživatelského rozhraní a jeho prvky uživatelského rozhraní, které jsou zpřístupněny kód prostřednictvím výstupy a akce jste připravení psát kód, který Oživte vašeho programu. Například otevřete **MainWindow.cs** soubor pro úpravy poklepáním v **řešení Pad**:

[![Soubor MainWindow.cs](xib-images/code01.png "MainWindow.cs souboru")](xib-images/code01-large.png#lightbox)

A přidejte následující kód, který `MainWindow` třídy pro práci s Ukázka výstupu, který jste vytvořili výše:

```csharp
private int numberOfTimesClicked = 0;
...

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Všimněte si, že `NSLabel` přistupuje v jazyce C# pomocí přímého názvu, že jste přiřadili ho v Xcode při jeho výstupu jste vytvořili v Xcode, v takovém případě se nazývá `ClickedLabel`. Žádné metody nebo vlastnosti objektu zveřejněné můžete přistupovat stejným způsobem, jako by všechny běžné třídy jazyka C#.

> [!IMPORTANT]
> Budete muset použít `AwakeFromNib`, místo jiné metody, jako `Initialize`, protože `AwakeFromNib` nazývá _po_ má načíst a vytvoření instance uživatelského rozhraní ze souboru .xib operačního systému. Pokud jste se pokusili pro přístup k souboru .xib ovládacího prvku popisek plně načtení a vytvoření instance, které byste získali, `NullReferenceException` chyba protože ovládací prvek popisek by dosud vytvořena.

Dál přidejte následující třídu k `MainWindow` třídy:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Tento kód se připojí k akci, kterou jste vytvořili v Xcode a Tvůrce rozhraní a bude volána vždy, když uživatel klikne na tlačítko.

Některé prvky uživatelského rozhraní automaticky jste vytvořili v akce, například položky v panelu nabídek výchozí, jako **otevřete...**  položku nabídky (`openDocument:`). V **řešení Pad**, dvakrát klikněte na **AppDelegate.cs** soubor otevřete pro úpravy a přidejte následující kód níže `DidFinishLaunching` metoda:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Klíč řádku je `[Export ("openDocument:")]`, sdělí `NSMenu` , **AppDelegate** má metodu `void OpenDialog (NSObject sender)` který reaguje na `openDocument:` akce.

Další informace o práci s nabídkami, najdete v tématu naše [nabídky](~/mac/user-interface/menu.md) dokumentaci.


## <a name="synchronizing-changes-with-xcode"></a>Synchronizace změn s Xcode

Když přepnete zpět do Visual Studio pro Mac od Xcode, všechny změny, které jste provedli v Xcode automaticky synchronizovat s Xamarin.Mac projektu.

Pokud jste vybrali **MainWindow.designer.cs** v **řešení Pad** budete moci zobrazit, jak naše výstupu a akce mají byly připojeny v našem kódu C#:

[![Synchronizace změn s Xcode](xib-images/sync01.png "synchronizace změn s Xcode")](xib-images/sync01-large.png#lightbox)

Všimněte si jak dvě definice v **MainWindow.designer.cs** souboru:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Zarovnání pomocí definice v **MainWindow.h** souboru v Xcode:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Jak vidíte, naslouchá změny do souboru h Visual Studio pro Mac a potom automaticky synchronizuje tyto změny v příslušné **. designer.cs** soubor umístěte je do vaší aplikace. Může také zjistíte, že **MainWindow.designer.cs** je konkrétní třídu, takže nemá Visual Studio pro Mac k úpravě **MainWindow.cs** který by přepsala veškeré změny, které jsme provedli pro třídu.

Obvykle se nikdy musíte otevřít **MainWindow.designer.cs** sami, ho se netýkají pro vzdělávací účely.

> [!IMPORTANT]
> Ve většině případů se Visual Studio pro Mac automaticky najdete v části veškeré změny provedené v Xcode a synchronizovat je se svým projektem Xamarin.Mac. Ve vypnutém výskyt, který se synchronizace nedojde automaticky přepněte zpět na Xcode a je zpět do Visual Studio pro Mac znovu. To se obvykle ji synchronizační cyklus.


## <a name="adding-a-new-window-to-a-project"></a>Přidávání nové okno do projektu

Kromě zajištění dostatečného okně hlavního dokumentu aplikace Xamarin.Mac chtít zobrazit další typy systému windows pro uživatele, například předvolby nebo panelů Inspector. Při přidávání nové okno do projektu byste měli vždycky používat **kakao okno s řadiče** možnost, jak je proces načítání okna z .xib souboru jednodušší.

Pokud chcete přidat nové okno, postupujte takto:

1. V **řešení Pad**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** .
2. V dialogovém okně Nový soubor vyberte **Xamarin.Mac** > **kakao okno s řadiče**:

    ![Přidání řadič nové okno](xib-images/new01.png "přidání řadič nové okno")
3. Zadejte `PreferencesWindow` pro **název** a klikněte na **nový** tlačítko.
4. Dvakrát klikněte **PreferencesWindow.xib** soubor otevřete pro úpravy v Tvůrci rozhraní:

    [![Úpravy okna v Xcode](xib-images/new02.png "úpravy okna v Xcode")](xib-images/new02-large.png#lightbox)
5. Vaše rozhraní návrhu:

    [![Návrh rozložení windows](xib-images/new03.png "návrh rozložení windows")](xib-images/new03-large.png#lightbox)
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Přidejte následující kód, který **AppDelegate.cs** zobrazíte nové okno:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

`var preferences = new PreferencesWindowController ();` Řádku vytvoří novou instanci třídy okno kontroler, který načte okno ze souboru .xib a zvýšení kapacity ho. `preferences.Window.MakeKeyAndOrderFront (this);` Řádek zobrazí nové okno pro uživatele.

Pokud spustíte kódu a vyberte **předvolby...**  z **nabídku aplikace**, zobrazí se okno:

![Spuštění ukázkové aplikace](xib-images/new04.png "spuštění ukázkové aplikace")

Další informace o práci s Windows v aplikaci Xamarin.Mac, najdete v tématu naše [Windows](~/mac/user-interface/window.md) dokumentaci.


## <a name="adding-a-new-view-to-a-project"></a>Přidávání nové zobrazení do projektu

Existují případy, kdy je snazší rozdělit návrh vaší okna na několik udržitelnější .xib soubory. Například jako přepnutí na obsah hlavního okna při výběru položky panelu nástrojů v **okno Předvolby** nebo záměnu obsah v reakci na **zdrojového seznamu** výběr.

Při přidávání nové zobrazení do projektu byste měli vždycky používat **kakao zobrazení s řadiče** možnost, jak je proces načítání zobrazení z .xib souboru jednodušší.

Pokud chcete přidat nové zobrazení, postupujte takto:

1. V **řešení Pad**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** .
2. V dialogovém okně Nový soubor vyberte **Xamarin.Mac** > **kakao zobrazení s řadiče**:

    ![Přidání nové zobrazení](xib-images/view01.png "přidání nové zobrazení")
3. Zadejte `SubviewTable` pro **název** a klikněte na **nový** tlačítko.
4. Dvakrát klikněte **SubviewTable.xib** soubor otevřete pro úpravy v Tvůrci rozhraní a návrhu, uživatelské rozhraní:

    [![Navrhování nového zobrazení v Xcode](xib-images/view02.png "návrhu nové zobrazení v Xcode")](xib-images/view02-large.png#lightbox)
5. Propojit všechny požadované akce a výstupy.
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Dále upravit **SubviewTable.cs** a přidejte následující kód, který **AwakeFromNib** souboru k vyplnění nového zobrazení, když je načten:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));
    DataSource.Sort ("Title", true);

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);

    // Auto select the first row
    ProductTable.SelectRow (0, false);
}
```

Výčet přidejte do projektu sledovat zobrazení, které je v současné době zobrazení. Například **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Upravte soubor .xib okna, které se využívají zobrazení a jeho zobrazení. Přidat **vlastní zobrazení** který bude fungovat jako kontejner pro zobrazení po jeho načtení do paměti kód C# a zveřejněte ji do zásuvky volat `ViewContainer`:

[![Vytváření požadované výstupu](xib-images/view03.png "vytváření požadované výstupu")](xib-images/view03-large.png#lightbox)

Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Potom upravte soubor .cs okna, které bude možné zobrazení nového zobrazení (například **MainWindow.cs**) a přidejte následující kód:

```csharp
private SubviewType ViewType = SubviewType.None;
private NSViewController SubviewController = null;
private NSView Subview = null;
...

private void DisplaySubview(NSViewController controller, SubviewType type) {

    // Is this view already displayed?
    if (ViewType == type) return;

    // Is there a view already being displayed?
    if (Subview != null) {
        // Yes, remove it from the view
        Subview.RemoveFromSuperview ();

        // Release memory
        Subview = null;
        SubviewController = null;
    }

    // Save values
    ViewType = type;
    SubviewController = controller;
    Subview = controller.View;

    // Define frame and display
    Subview.Frame = new CGRect (0, 0, ViewContainer.Frame.Width, ViewContainer.Frame.Height);
    ViewContainer.AddSubview (Subview);
}
```

Když je potřeba zobrazit nové zobrazení načtený ze souboru .xib v kontejneru okna ( **vlastní zobrazení** přidané v předchozím kroku), tento kód obslužné rutiny odebírání jakékoli existující zobrazení a odkládací ho pro novou. Vypadá to zobrazíte ji už máte zobrazení zobrazí, pokud tak se odeberou z obrazovky. Další trvá zobrazení, který byl předán v (jak načíst z řadiče zobrazení) změní ho, aby se vešla do oblasti obsahu a přidává ji k obsahu pro zobrazení.

Chcete-li zobrazit nové zobrazení, použijte následující kód:

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Tím se vytvoří novou instanci třídy řadiče zobrazení pro nové zobrazení, který se má zobrazit, nastaví její typ (podle výčtu k projektu nepřidají) a používá `DisplaySubview` metoda přidat do třídy okna ve skutečnosti zobrazit. Příklad:

[![Spuštění ukázkové aplikace](xib-images/view04.png "spuštění ukázkové aplikace")](xib-images/view04-large.png#lightbox)

Další informace o práci s Windows v aplikaci Xamarin.Mac, najdete v tématu naše [Windows](~/mac/user-interface/window.md) a [v dialogových oknech](~/mac/user-interface/dialog.md) dokumentaci.


## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce se soubory .xib v aplikaci Xamarin.Mac trvá. Jsme viděli různé typy a používá .xib souborů, které chcete vytvořit uživatelské rozhraní aplikace, jak vytvořit a udržovat .xib soubory v Xcode na rozhraní tvůrce a postupu při práci se soubory .xib v kódu jazyka C#.


## <a name="related-links"></a>Související odkazy

- [MacImages (ukázka)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Nabídky](~/mac/user-interface/menu.md)
- [Dialogy](~/mac/user-interface/dialog.md)
- [Práce s obrázky](~/mac/app-fundamentals/image.md)
- [systému macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
