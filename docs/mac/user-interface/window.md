---
title: Windows
description: Tento článek se zabývá práci s windows a panely v Xamarin.Mac aplikace. Popisuje vytváření windows a panely v Xcode a rozhraní tvůrce, jejich načtení ze scénářů a .xib souborů a práce s nimi prostřednictvím kódu programu.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f45bc69b74d98c7b9130f2caeaee91b184c38d87
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="windows"></a>Windows

_Tento článek se zabývá práci s windows a panely v Xamarin.Mac aplikace. Popisuje vytváření windows a panely v Xcode a rozhraní tvůrce, jejich načtení ze scénářů a .xib souborů a práce s nimi prostřednictvím kódu programu._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup do stejné Windows a panely, které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a udržovat Windows a panelů (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Podle jeho účel, Xamarin.Mac aplikace může být jeden nebo více Windows na obrazovce pro správu a koordinovat informace zobrazí a funguje s. Hlavní funkce okna jsou:

1. K zajištění oblast, ve kterém může být zobrazení a ovládací prvky umístěny a spravované.
2. Přijmout a reakce na události v reakci na interakce uživatele s klávesnici a myš.

Windows může být použité v nemodální stavu (třeba textový editor, který může mít více dokumentů současně) nebo modální (například dialogu Export, které musí být před pokračováním aplikace zavře).

Panely jsou zvláštní druh okno (podtřídou třídy základní `NSWindow` – třída), která obvykle obsluhují pomocné funkce v aplikaci, například nástroj windows jako textový formát kontroly a systému volby barev.

[![](window-images/intro01.png "Úpravy okna v Xcode")](window-images/intro01.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s Windows a panelů v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Úvod do systému Windows

Jak jsme uvedli výše, okno poskytuje oblast, ve kterém může být zobrazení a ovládací prvky umístěny a spravované a reaguje na události podle interakce uživatele (buď prostřednictvím klávesnici nebo myš).

Podle společnosti Apple existují pět hlavních typů Windows v macOS aplikace:

- **Okno dokumentu** -okna dokumentu obsahuje data uživatele na základě souborů, jako jsou tabulky nebo textový dokument.
- **Okna aplikace na** -okno s aplikací je hlavní okno aplikace, která není na základě dokumentu (např. aplikace kalendáře na Macu).
- **Panel** – panelu jako plovoucí nad jiné windows a poskytuje nástroje, nebo otevřete ovládací prvky, které uživatelé mohou pracovat s dokumenty jsou. V některých případech může být průhledná panelu (třeba při práci s velké grafiky).
- **Dialogové okno** – dialogové okno se zobrazí v reakci na akci uživatele a obvykle poskytuje uživatelům způsoby, jak můžete dokončit akci. Zobrazí se dialogové okno vyžaduje odpověď od uživatele, než je možné uzavřít. (Viz [práce s dialogová okna](~/mac/user-interface/dialog.md))
- **Výstrahy** – výstraha je zvláštní druh dialog, který se zobrazí, když dojde k vážný problém (například chybu) nebo jako upozornění (například příprava k odstranění souboru). Vzhledem k tomu, že výstraha je dialogové okno, taky vyžaduje odpověď uživatele předtím, než je možné uzavřít. (Viz [práce s výstrahami](~/mac/user-interface/alert.md))

Další informace najdete v tématu [o Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Hlavní klíč a neaktivní Windows

Windows v aplikaci Xamarin.Mac můžete vzhled a chování různě v závislosti na tom, jak uživatel aktuálně interakci s nimi. Je volána přední dokumentu nebo okna aplikace, který je aktuálně zaměření pozornosti uživatele _hlavní okno_. Ve většině případů toto okno bude také _klíč okno_ (okno, které aktuálně přijímá vstup uživatele). Ale není vždy o tento případ, například výběr barvy a může být otevřený a být klíč okně, které uživatel komunikuje s na změnu stavu položky v okně dokumentu (který by byl stále hlavní okno).

Hlavní a klíč Windows (pokud jsou samostatné) jsou vždy aktivní, _neaktivní Windows_ jsou otevřená okna, které nejsou v popředí. Například textový editor aplikace může mít více než jeden dokument otevřít najednou pouze hlavní okno by byl aktivní, všechny ostatní by být neaktivní. 

Další informace najdete v tématu [o Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Pojmenování Windows

Okno může zobrazit záhlaví okna a když se zobrazí název, je obvykle název aplikace, název dokumentu pracuje nebo funkce okna (například Inspector). Některé aplikace nezobrazí záhlaví, protože jsou rozpoznatelném podle nebyl zřejmý a nefungují s dokumenty.

Apple navrhnout podle následujících pokynů:

- Použijte název vaší aplikace pro nadpis okna Hlavní, není dokument. 
- Název nového okna dokumentu `untitled`. Pro první nový dokument, nemusíte připojit číslo k názvu (například `untitled 1`). Pokud uživatel vytvoří další nový dokument před uložením a titulkových první, že okno volání `untitled 2`, `untitled 3`atd.

Další informace najdete v tématu [pojmenování Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Windows přes celou obrazovku

V systému macOS, okně aplikace můžete přejít celé obrazovky skrytí vše, včetně panelu nabídek aplikací (který může být odhalena přesunutím kurzoru do horní části obrazovky) k poskytování rušivě je volné interakci s jeho obsahu.

Apple navrhuje podle následujících pokynů:

- Určí, zda má smysl pro okno přejdete celé obrazovky. Aplikace, které poskytují stručný interakce (například kalkulačky) by neměla poskytovat režim celé obrazovky.
- Pokud úloha přes celou obrazovku vyžaduje ji zobrazte panelu nástrojů. Obvykle je v režimu celé obrazovky skrytý na panelu nástrojů.
- Celé obrazovce měli všichni uživatelé funkce potřebovat k dokončení úlohy.
- Pokud je to možné vyhněte vyhledávací interakce při uživatel je v celé obrazovce.
- Využijte výhod místo vyšší obrazovky bez posunem fokus od hlavní úloh.

Další informace najdete v tématu [Windows přes celou obrazovku](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Panelů

Panelu je pomocného okna, který obsahuje ovládací prvky a možnosti, které mají vliv na aktivní dokument nebo výběr (například systém volby barev):

[![](window-images/panel01.png "Panel barvy")](window-images/panel01.png#lightbox)

Panely může být buď _konkrétní aplikaci_ nebo _systémové_. Konkrétní aplikaci panelů float v horní části okna dokumentu aplikace a zmizí, když je aplikace na pozadí. Systémové panelů (například **písem** panely), float nad všechna otevřená okna bez ohledu na to aplikace. 

Apple navrhuje podle následujících pokynů:

- Obecně platí, použijte standardní panel, průhledné panely musí být použit pouze zřídka a pro graficky náročné úlohy.
- Zvažte použití panelu udělit uživatelům snadný přístup k důležité ovládací prvky nebo informace, které přímo ovlivňuje jejich úkolů.
- Skrýt a zobrazit panely podle potřeby.
- Panely by měla vždycky obsahovat záhlaví.
- Panely by neměla zahrnovat minimalizaci aktivní tlačítko.

#### <a name="inspectors"></a>inspektoři

Většina moderních aplikací systému macOS prezentovat pomocného ovládacích prvků a možnosti, které mají vliv na aktivní dokument nebo výběr jako _inspektoři_ které jsou součástí hlavní okno (jako **stránky** aplikace, viz následující obrázek), místo použití panelu Windows:

[![](window-images/panel02.png "Kontrolor příklad")](window-images/panel02.png#lightbox)

Další informace najdete v tématu [panelů](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) a na našem [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) ukázková aplikace pro úplnou implementaci daného **Inspector rozhraní** v Xamarin.Mac aplikaci.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Vytváření a údržbu systému Windows v Xcode

Když vytvoříte novou aplikaci Xamarin.Mac kakao, zobrazí okno Standardní prázdné, ve výchozím nastavení. Toto systému windows je definována v `.storyboard` automaticky zahrnutý v projektu. Chcete-li upravit návrh vašeho systému windows v **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souboru:

[![](window-images/edit01.png "Výběr hlavní storyboard")](window-images/edit01.png#lightbox)

Otevře se okno návrhu v Xcode na rozhraní Tvůrce:

[![](window-images/edit02.png "Úpravy uživatelského rozhraní v Xcode")](window-images/edit02.png#lightbox)

V **atribut Inspector**, nejsou k dispozici několik vlastnosti, které můžete definovat a ovládat okně aplikace:

- **Název** -Toto je text, který se zobrazí v záhlaví okna.
- **Automatické ukládání** -Toto je _klíč_ který se použije k ID okna, když se automaticky uloží pozice jeho a nastavení.
- **Záhlaví** -okno zobrazovat záhlaví.
- **Unified nadpis a nástrojů** – Pokud je toto okno obsahuje panel nástrojů, musí být součástí záhlaví.
- **Úplné velikosti zobrazení obsahu** -umožňuje oblast obsahu okna pod záhlaví.
- **Stínové** -nemá okno stínu.
- **Texturou** -texturou windows můžete použít efekty (např. sytější) a přesunout tak, že přetáhnete kdekoli na jejich textu.
- **Zavřete** -nemá okna tlačítko Zavřít.
- **Minimalizovat** -nemá okně tlačítko Minimalizovat.
- **Změnit velikost** -nemá okno změny velikosti ovládacího prvku.
- **Tlačítka panelu nástrojů** -nemá okno Zobrazit/skrýt tlačítka panelu nástrojů.
- **Obnovitelné** -je pozice a nastavení automaticky uložený a obnovený okna.
- **Viditelné na spuštění** -okna automaticky zobrazí při `.xib` načíst soubor.
- **Skrýt na deaktivovat** -je okno skrytá, jakmile se zobrazí na pozadí.
- **Verze, když uzavřený** -okno vymazat z paměti, když je uzavřený.
- **Popisy tlačítek vždy zobrazení** -jsou trvale zobrazí popisky tlačítek.
- **Přepočítá zobrazení smyčky** -je pořadí zobrazení Přepočítat před vykreslením okna.
- **Mezery**, **Exposé** a **Cycling** -všechny definovat chování okna v těchto prostředích systému macOS. 
- **Celá obrazovka** -Určuje, pokud v tomto okně můžete zadat režim celé obrazovky. 
- **Animace** – ovládací prvky typu k dispozici pro okno animace.
- **Vzhled** -řídí vzhled okna. Teď je pouze jeden vzhled, akvamarínová.

Najdete v článku společnosti Apple [Úvod do Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) a [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) dokumentace pro další podrobnosti.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Nastavení výchozí velikost a umístění

Chcete-li nastavit počáteční umístění vaší okna a řídit její velikost, přepněte do **velikost Inspector**:

[![](window-images/edit07.png "Výchozí velikost a umístění")](window-images/edit07.png#lightbox)

Zde můžete nastavit počáteční velikost okna, poskytněte minimální a maximální velikost, nastavit počáteční umístění na obrazovce a řídit ohraničení kolem okna.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Nastavení vlastní hlavní okno řadiče

Abyste mohli vytvořit výstupy a akce, která zveřejňuje prvky uživatelského rozhraní pro kód C#, aplikace Xamarin.Mac muset používat řadič okno Vlastní.

Postupujte takto:

1. V Tvůrci rozhraní na Xcode otevřete Storyboard aplikace.
2. Vyberte `NSWindowController` v návrhovou plochu.
3. Přepnout **Identity Inspector** zobrazení a zadejte `WindowController` jako **název třídy**: 

    [![](window-images/windowcontroller01.png "Nastavení názvu – třída")](window-images/windowcontroller01.png#lightbox)
4. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci.
5. A `WindowController.cs` soubor bude přidán do projektu v **Průzkumníku řešení** v sadě Visual Studio pro Mac: 

    [![](window-images/windowcontroller02.png "Výběr řadiče systému windows")](window-images/windowcontroller02.png#lightbox)
6. Znovu otevřete Storyboard v Tvůrci rozhraní pro Xcode.
7. `WindowController.h` Soubor bude k dispozici pro použití: 

    [![](window-images/windowcontroller03.png "Úpravy souboru WindowController.h")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Přidání prvky uživatelského rozhraní

Definovat obsah časového období, přetáhněte ovládací prvky z **knihovny Inspector** na **rozhraní editoru**. Najdete v tématu naše [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) dokumentace pro další informace o vytvoření a povolení ovládacích prvků pomocí rozhraní tvůrce.

Například můžeme přetáhněte panelu nástrojů **knihovny Inspector** do okna v **rozhraní editoru**:

[![](window-images/edit03.png "Výběr panelu nástrojů v knihovně")](window-images/edit03.png#lightbox)

Dalším kroku přetáhněte **textového zobrazení** a velikost, aby vyplnil celou oblast v panelu nástrojů:

[![](window-images/edit04.png "Přidání zobrazení textu")](window-images/edit04.png#lightbox)

Vzhledem k tomu, že chceme, aby **textového zobrazení** zmenšit a růst jako změny velikosti okna, umožňuje přepnout do **Editor omezení** a přidejte následující omezení:

[![](window-images/edit05.png "Úpravy omezení")](window-images/edit05.png#lightbox)

Kliknutím pro **Red I světla** v horní části editoru a kliknutím na **přidat omezení 4**, jsme informace o tom textového zobrazení pro danou X, Y přilepit souřadnice a zvětšení nebo zmenšení vodorovně a svisle jako se změnila velikost okna.

Nakonec umožňuje vystavit **textového zobrazení** k programování s využitím **výstupu** (a zkontrolujte, zda vyberte `ViewController.h` souborů):

[![](window-images/edit06.png "Konfigurace výstupu")](window-images/edit06.png#lightbox)

Uložte změny a přepněte zpět na Visual Studio pro Mac k synchronizaci s Xcode.

Další informace o práci s **výstupy** a **akce**, najdete v tématu naše [výstupu a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) dokumentaci.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Standardní okno pracovního postupu

Pro jakékoli okno, které vytvoříte a pracovat v aplikaci Xamarin.Mac proces je v podstatě stejný jako co jsme právě dokončili výše:

1. Pro nové windows, které nejsou výchozím nastavení automaticky přidat do projektu přidejte do projektu novou definici okna. To se bude zabývat rozpis naleznete níže.
2. Dvakrát klikněte `Main.storyboard` soubor otevřete okno návrh pro úpravy v Xcode na rozhraní tvůrce.
3. Přetáhněte nové okno do návrhu uživatelské rozhraní a propojte okna do hlavní okno pomocí _Segues_ (Další informace najdete v článku [Segues](~/mac/platform/storyboards/indepth.md#Segues) části našich [práce s scénářů](~/mac/platform/storyboards/indepth.md) dokumentaci).
3. Nastavte všechny vlastnosti požadované okno **atribut Inspector** a **velikost Inspector**.
4. Přetáhněte v ovládacích prvcích potřebné k sestavení vaše rozhraní a je v konfiguraci **atribut Inspector**.
5. Použití **velikost Inspector** zpracovat změny velikosti pro prvky vaši uživatelského rozhraní.
6. Vystavení okna prvky uživatelského rozhraní pro kód C# prostřednictvím **výstupy** a **akce**.
7. Uložte změny a přepněte zpět na Visual Studio pro Mac k synchronizaci s Xcode.

Teď, když máme základní okna vytvořeného, podíváme procesů, které jsou typické Xamarin.Mac, aplikace nebude při práci s windows. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Zobrazení okna výchozí

Ve výchozím nastavení, bude nová aplikace Xamarin.Mac automaticky zobrazení okna definovaný v `MainWindow.xib` souborů při spuštění:

[![](window-images/display01.png "Okno s příklad systémem")](window-images/display01.png#lightbox)

Vzhledem k tomu, že jsme změnit návrh toto okno výše, teď obsahuje výchozí panelu nástrojů a **textového zobrazení** ovládacího prvku. Následující části v `Info.plist` soubor je odpovědná za zobrazování toto okno:

[![](window-images/display00.png "Úpravy Info.plist")](window-images/display00.png#lightbox)

**Hlavní rozhraní** rozevírací slouží k výběru scénáře, který se použije jako hlavní aplikace uživatelského rozhraní (v tomto případě `Main.storyboard`).

Řadič zobrazení je automaticky přidá do projektu řídit tento hlavní Windows, který se zobrazí (spolu s jeho primární zobrazení). Je definována v `ViewController.cs` souboru a připojeny k **vlastník souboru** v Tvůrci rozhraní v části **Identity Inspector**:

[![](window-images/display02.png "Nastavení vlastník souboru")](window-images/display02.png#lightbox)

Pro naše okno rádi bychom znali mohla mít název z `untitled` při prvním otevření můžeme přepsání `ViewWillAppear` metoda v `ViewController.cs` , aby vypadala jako následující:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Jsme se nastaví hodnota okna `Title` vlastnost `ViewWillAppear` metoda místo `ViewDidLoad` metoda proto, že při zobrazení může být načtena do paměti, kdy není ještě vytvořen plně. Pokud jsme se pokusili získat přístup `Title` vlastnost `ViewDidLoad` metoda by se nám získat `null` výjimky, protože okno nebyl byla vytvořená a drátové až vlastnost ještě.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Zavřením okna prostřednictvím kódu programu

Může dojít k situaci, které chcete prostřednictvím kódu programu zavřete okno v aplikaci Xamarin.Mac, kromě nutnosti uživatele, klikněte na tlačítko okno **zavřete** tlačítko nebo pomocí položky nabídky. systému macOS nabízí dva různé způsoby, jak zavřít `NSWindow` prostřednictvím kódu programu: `PerformClose` a `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Volání `PerformClose` metodu `NSWindow` simuluje uživatel kliknutím na tlačítko okno **Zavřít** tlačítko na okamžik zvýraznění tlačítko a zavřením okna.

Pokud aplikace implementuje `NSWindow`na `WillClose` událostí, bude vyvolána, před zavření okna. Pokud vrátí událost `false`, pak nebude zavření okna. Pokud okno nemá **Zavřít** tlačítko nebo nelze zavřít, z jakéhokoli důvodu operačního systému bude generovat výstrahy zvuk.

Příklad:

```csharp
MyWindow.PerformClose(this);
```

Pokoušel zavřete `MyWindow` `NSWindow` instance. Pokud bylo úspěšné, se zavřou okna, jinak bude mít vygenerované výstrahy zvuk a bude zůstanou otevřené.

<a name="Close" />

### <a name="close"></a>Zavřít

Volání `Close` metodu `NSWindow` není simuluje uživatel kliknutím na tlačítko okno **Zavřít** tlačítko pomocí na okamžik zvýraznění tlačítko, jednoduše zavře okno.

Okno nemusí být viditelné bude uzavřen a `NSWindowWillCloseNotification` oznámení budou odeslány do výchozí centrum oznámení pro okno dochází k uzavření.

`Close` Metoda se liší dvěma způsoby důležité z `PerformClose` metoda:

1. Se nebude pokoušet o zvýšení `WillClose` událostí.
2. Není simulaci, uživatel kliknutím **Zavřít** tlačítko pomocí na okamžik zvýraznění tlačítko.

Příklad:

```csharp
MyWindow.Close();
```

By zavřete `MyWindow` `NSWindow` instance.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Změny systému Windows

V systému macOS, Apple poskytl způsob, jak informovat uživatele, který obsah okno (`NSWindow`) byl upraven uživatelem a je nutné uložit. Pokud okno obsahuje změněný obsah, malé černý bod se zobrazí v jeho **Zavřít** pomůcky:

[![](window-images/close01.png "Okno s upravené značky")](window-images/close01.png#lightbox)

Pokud se uživatel pokusí zavřete okno nebo ukončení aplikace Mac, protože existují neuložené změny okně obsah, měli reprezentovat [dialogové okno](~/mac/user-interface/dialog.md) nebo [modální list](~/mac/user-interface/dialog.md) a uživateli umožňují uložit jejich změny první:

[![](window-images/close02.png "A uložte stylů se zobrazí při zavření okna")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Označení okno jako upravené

Označit okno jako s upravit obsah, použijte následující kód:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

A jakmile tato změna byla uložena, zrušte příznak upravené pomocí:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Ukládají se změny před jeho zavřením okna

Pokud chcete sledovat pro uživatele zavřením okna a povolil jejich uložit upravený obsah předem, budete muset vytvořit podtřídou třídy `NSWindowDelegate` a přepsat její `WindowShouldClose` metoda. Příklad:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on resu;t
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

Pro připojení k vaší okno instanci tohoto delegáta použijte následující kód:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Ukládají se změny před jeho zavřením aplikace

Nakonec Xamarin.Mac aplikace by měla zkontrolovat Pokud některý z jeho systému Windows obsahovat změněný obsah a umožnit uživatelům před ukončením uložit změny. Chcete-li to provést, upravte vaše `AppDelegate.cs` souboru, přepsat `ApplicationShouldTerminate` metoda vypadá takto:

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>Práce s více oken

Většina aplikací systému Mac dokumentů na základě můžete upravit více dokumentů ve stejnou dobu. Textový editor může mít například více textových souborů otevřete pro úpravy ve stejnou dobu. Ve výchozím nastavení, má naší nové aplikace Xamarin.Mac **soubor** nabídku **nový** položku automaticky přes drátové sítě až `newDocument:` **akce**.

Přidáme k aktivaci této nové položky a umožnit uživatelům otevřít více kopií naše hlavní okno pro úpravu více dokumentů najednou.

Umožňuje upravit naše `AppDelegate.cs` souboru a přidejte následující počítaný vlastnost:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Použijeme to sledovat počet neuložené souborů, aby bylo možné přidělit zpětnou vazbu pro uživatele (podle pokynů společnosti Apple jak je popsáno výše).

Dál přidejte následující metodu:

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Tento kód vytvoří novou verzi kontroleru okno, načte nové okno, díky hlavní a klíč okna a jeho nastaví název. Nyní když jsme spuštění aplikace a vyberte **nový** z **souboru** nabídky nové okno editor otevřít, který se zobrazí:

[![](window-images/display04.png "Byla přidána nové okno bez názvu")](window-images/display04.png#lightbox)

Pokud jsme otevřít **Windows** nabídky, můžete zobrazit aplikace je automaticky sledování a zpracování naše otevřete windows:

[![](window-images/display05.png "V nabídce Windows")](window-images/display05.png#lightbox)

Další informace o práci s nabídkami v aplikaci Xamarin.Mac, najdete v tématu naše [práce s nabídkami](~/mac/user-interface/menu.md) dokumentaci.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Získávání aktuálně aktivní okno

V aplikaci Xamarin.Mac, která můžete otevřít více oken (dokumenty) jsou časy, kdy budete muset získat aktuální, nejhornější okna (okna klíče). Následující kód vrátí okna klíče:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Je možné volat v libovolném třída nebo metoda, která potřebuje přístup k okno aktuální, klíče. Pokud je aktuálně otevřený žádný časový interval, bude vracet `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Přístup k všechny aplikace systému Windows

Je možné časy potřebujete-li získat přístup ke všem systému windows, že má vaše aplikace Xamarin.Mac aktuálně otevřené. Například pokud soubor, který chce uživatel otevřete, projděte si je již otevřen v existujícímu okně.

`NSApplication.SharedApplication` Udržuje `Windows` vlastnost, která obsahuje pole všechna otevřená okna ve vaší aplikaci. Můžete iterovat přes toto pole pro přístup k veškerému windows aktuální aplikace. Příklad:

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

V příkladu kódu jsme jsou přetypování každý vrácený okna Vlastní `ViewController` třídy v naší aplikaci a hodnotu vlastní testování `Path` vlastnosti pro cestu k souboru, který se chce uživatel otevřít. Pokud soubor je již spuštěna, jsme před přináší toto okno.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Úprava velikosti okna v kódu

Existují situace, když aplikace potřebuje ke změně velikosti okna v kódu. Změna velikosti a pozice časového období, můžete upravit tak, aby ho na `Frame` vlastnost. Při úpravě velikost okna, obvykle musíte také upravit jeho počátek, aby okna ve stejném umístění, z důvodu systému macOS na souřadnicový systém.

Na rozdíl od iOS, kde v levém horním rohu představuje (0,0) systému macOS používá mathematic souřadnicový systém, kde levém dolním rohu obrazovky představuje (0,0). V iOS souřadnice zvýšit, jak přesunout dolů směrem doprava. V systému macOS souřadnice zvýšit v hodnotě směrem nahoru napravo. 

Následující příklad kódu změní velikost okna:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Když upravíte windows velikost a umístění v kódu, budete muset Ujistěte se, že respektují minimální a maximální velikosti, které jste nastavili v Tvůrci rozhraní. To nebude automaticky dodržení a bude moct provádět okna větší nebo menší než tato omezení.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Monitorování změn velikost okna

Je možné časy potřebujete-li sledovat změny v velikost okna uvnitř Xamarin.Mac aplikace. Chcete-li například ho překreslit obsah tak, aby odpovídal nové velikosti.

Pokud chcete monitorovat změny velikosti, nejdříve se ujistěte, zda jste přiřadili vlastní třídu pro okno řadič v Tvůrci rozhraní pro Xcode. Například `MasterWindowController` v následujícím:

[![](window-images/resize01.png "Nástroj Inspector Identity")](window-images/resize01.png#lightbox)

Potom upravte vlastní třídy Kontroleru okno a monitorování `DidResize` událostí v okně Kontroleru k upozornění na změny velikosti za provozu. Příklad:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

Volitelně můžete `DidEndLiveResize` událostí pouze oznámení po dokončení uživatele změna velikost okna. Příklad:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>Nastavení intervalu pro správu a je název a reprezentována souboru

Při práci s windows, které představují dokumenty, `NSWindow` má `DocumentEdited` vlastnost, že pokud nastavena na `true` zobrazí malý bod v uživateli přidělit jako ukazatel toho, že soubor byl upraven a před zavřením uložit na tlačítko Zavřít.

Umožňuje upravit naše `ViewController.cs` souboru a proveďte následující změny:

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

Můžeme také monitorování `WillClose` událostí na okno a kontrolu stavu `DocumentEdited` vlastnost. Pokud je `true` musíme uživateli přidělit možnost uložit změny do souboru. Pokud jsme spuštění vaší aplikace a zadání nějakého textu, zobrazí se tečky:

[![](window-images/file01.png "Změněné okna")](window-images/file01.png#lightbox)

Pokud jsme Zkuste zavřít okno, se nám získat výstrahu:

[![](window-images/file02.png "Zobrazení uložení dialogové okno")](window-images/file02.png#lightbox)

Pokud jsme se načítají ze souboru dokumentu jsme můžete nastavit v záhlaví okna na soubor název pomocí `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` – metoda (oznámeno, že `path` je řetězec představující soubor otevíráte). Kromě toho jsme nastavit adresu URL souboru pomocí `window.RepresentedUrl = url;` metoda.

Pokud adresu URL odkazuje na typ souboru známého operačního systému, zobrazí se ikona jeho v záhlaví. Pokud uživatel kliknutí pravým tlačítkem myši na ikonu, zobrazí se cesta k souboru.

Umožňuje upravit `AppDelegate.cs` souboru a přidejte následující metodu:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

Nyní když jsme spuštění vaší aplikace, vyberte **otevřete...**  z **soubor** nabídce vyberte možnost textový soubor ze **otevřete** dialogové okno pole a otevřete jej:

[![](window-images/file03.png "Otevřené dialogové okno")](window-images/file03.png#lightbox)

Zobrazí se souboru a název bude nastavena s ikonou souboru:

[![](window-images/file04.png "Načíst obsah souboru")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Přidávání nové okno do projektu

Kromě zajištění dostatečného okně hlavního dokumentu aplikace Xamarin.Mac chtít zobrazit další typy systému windows pro uživatele, například předvolby nebo panelů Inspector.

Pokud chcete přidat nové okno, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy v Xcode na rozhraní tvůrce.
2. Přetáhněte novou **okno řadiče** z **knihovny** na **návrhová plocha**:

    [![](window-images/new01.png "Výběr nového okna řadiče v knihovně")](window-images/new01.png#lightbox)
3. V **Identity Inspector**, zadejte `PreferencesWindow` pro **Storyboard ID**: 

    [![](window-images/new02.png "Nastavení ID scénáře")](window-images/new02.png#lightbox)
5. Vaše rozhraní návrhu: 

    [![](window-images/new03.png "Návrh uživatelského rozhraní")](window-images/new03.png#lightbox)
6. Otevřete nabídku aplikace (`MacWindows`), vyberte **předvolby...** , Řízení klikněte a přetáhněte do nového okna: 

    [![](window-images/new05.png "Vytváření segue")](window-images/new05.png#lightbox)
7. Vyberte **zobrazit** z místní nabídky.
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Pokud jsme spustit kód a vybrat **předvolby...**  z **nabídku aplikace**, zobrazí se okno:

[![](window-images/new04.png "Ukázka předvolby nabídky")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Práce s panelů

Jak jsme uvedli na začátku tohoto článku, panelu jako plovoucí nad jiné windows a poskytuje nástroje nebo ovládací prvky, které uživatelé mohou pracovat s dokumenty jsou otevřené. 

Stejně jako jakýkoli jiný typ okno, které vytvoříte a pracovat v aplikaci Xamarin.Mac proces je v podstatě stejný:

1. Přidejte novou definici okna do projektu.
2. Dvakrát klikněte `.xib` soubor otevřete okno návrh pro úpravy v Xcode na rozhraní tvůrce.
2. Nastavte všechny vlastnosti požadované okno **atribut Inspector** a **velikost Inspector**.
4. Přetáhněte v ovládacích prvcích potřebné k sestavení vaše rozhraní a je v konfiguraci **atribut Inspector**.
5. Použití **velikost Inspector** zpracovat změny velikosti pro prvky vaši uživatelského rozhraní.
6. Vystavení okna prvky uživatelského rozhraní pro kód C# prostřednictvím **výstupy** a **akce**.
7. Uložte změny a přepněte zpět na Visual Studio pro Mac k synchronizaci s Xcode.

V **atribut Inspector**, máte následující možnosti, které jsou specifické pro panelů:

[![](window-images/panel03.png "Atribut Inspector")](window-images/panel03.png#lightbox)

- **Styl** – umožňují upravit styl panelu z: regulární Panel (vypadá jako standardní okno), nástroj Panel (má menší záhlaví), HUD Panel (jsou průhledné a záhlaví je součástí na pozadí).
- **Bez aktivace** -Určuje, v okně klíče se změní na panelu.
- **Zdokumentujte modální** – Pokud dokument modální, panelu pouze plovoucí nad aplikace windows, jinak obtéká nad všemi.


Pokud chcete přidat nový Panel, postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** .
2. V dialogovém okně Nový soubor vyberte **Xamarin.Mac** > **kakao okno s řadiče**:

    [![](window-images/panels00.png "Přidávání nového řadiče okna")](window-images/panels00.png#lightbox)
3. Zadejte `DocumentPanel` pro **název** a klikněte na **nový** tlačítko.
4. Dvakrát klikněte `DocumentPanel.xib` soubor otevřete pro úpravy v Tvůrci rozhraní: 

    [![](window-images/new02.png "Úpravy panel")](window-images/new02.png#lightbox)
5. Odstranit existující okna a přetáhněte ji z panelu **knihovny Inspector** v **rozhraní editoru**: 

    [![](window-images/panels01.png "Odstranění existující okna")](window-images/panels01.png#lightbox)
6. Propojte panelu až **vlastník souboru*-**okno*- **výstupu**: 

    [![](window-images/panels02.png "Přetahování k přenosu do panelu")](window-images/panels02.png#lightbox)
7. Přepnout **Identity Inspector** a nastavte třídy panelu na `DocumentPanel`: 

    [![](window-images/panels03.png "Nastavení panelu – třída")](window-images/panels03.png#lightbox)
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.
7. Upravit `DocumentPanel.cs` soubor a změňte definici třídy na následující: 

    `public partial class DocumentPanel : NSPanel`
8. Uložte změny do souboru.

Upravit `AppDelegate.cs` souboru a nastavit `DidFinishLaunching` metoda vypadá takto:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Pokud jsme spuštění aplikace, zobrazí se na panelu:

[![](window-images/panels04.png "V panelu ve spuštěné aplikaci")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Panel Windows jsou zastaralé společností Apple a měl by být nahrazen **Inspector rozhraní**. Úplný příklad vytvoření **Inspector** v aplikaci Xamarin.Mac, najdete v tématu naše [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) ukázkovou aplikaci.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek trvá podrobný pohled na práci s Windows a panelů v aplikaci Xamarin.Mac. Jsme viděli různé typy a používá Windows a panelů, jak vytvořit a udržovat Windows a panely v Xcode na rozhraní tvůrce a postupy pro práci s Windows a panely v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacWindows (ukázka)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (ukázka)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s nabídkami](~/mac/user-interface/menu.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do systému Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
