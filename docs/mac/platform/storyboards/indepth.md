---
title: Práce s scénářů v Xamarin.Mac
description: Tento dokument popisuje, jak pracovat s scénářů v Xamarin.Mac, prozkoumání postup je načíst z kódu, životního cyklu řadiče zobrazení, řetězu respondér, segues okno řadiče, nástroje pro rozpoznávání gesto a další.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 72986ed4247c3b6f66f6f1813d74bf0a95d0de53
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792837"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Práce s scénářů v Xamarin.Mac

Scénář definuje všechny uživatelské rozhraní pro danou aplikaci rozdělit do funkční přehled jeho zobrazení řadičů. V Tvůrci rozhraní Xcode na každý z těchto řadičů žije v jeho vlastní scény.

[![](indepth-images/intro01.png "Scénáře v Tvůrci rozhraní na Xcode")](indepth-images/intro01.png#lightbox)

Scénář je souboru prostředků (s příponami z `.storyboard`), získá součástí sady prostředků aplikace Xamarin.Mac, když je zkompilovat a dodaný. Definovat výchozí scénáře pro vaši aplikaci, upravit jej na `Info.plist` soubor a vyberte **hlavní rozhraní** z rozevíracího pole: 

[![](indepth-images/sb01.png "Info.plist editor")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>Načítání z kódu

Mohou nastat situace, když potřebujete načíst konkrétní scénáře z kódu a vytvořit řadič zobrazení ručně. K provedení této akce můžete použít následující kód:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName` Načte Storyboard soubor se zadaným názvem, který byl součástí sady aplikace. `InstantiateControllerWithIdentifier` Vytvoří instanci řadiče zobrazení s danou identitu. Při navrhování uživatelského rozhraní nastavíte v Xcode na rozhraní tvůrce Identity:

[![](indepth-images/sb02.png "Nastavení ID scénáře")](indepth-images/sb02.png#lightbox)

Volitelně můžete `InstantiateInitialController` metodu pro zatížení řadiče zobrazení, který byl přiřazen počáteční řadiče v Tvůrci rozhraní:

[![](indepth-images/sb03.png "Nastavení počáteční řadiče")](indepth-images/sb03.png#lightbox)

Je označena kvalifikátorem **Storyboard vstupní bod** a na šipku otevřete skončila výše.

<a name="View-Controllers" />

## <a name="view-controllers"></a>Zobrazení řadičů

Zobrazení řadičů definovat vztahy mezi daného zobrazení informací v rámci aplikace Mac a datový model, který poskytuje tyto informace. Každý nejvyšší úrovně scény ve scénáři, představuje jeden řadič zobrazení v kódu aplikace Xamarin.Mac.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>Životní cyklus řadiče zobrazení

Několik nových metod jsou přidané do `NSViewController` třídy pro podporu scénářů v systému macOS. Co je nejdůležitější jsou že metody postupujte podle použití reagovat na životní cyklus zobrazení, se řídí daný řadiče zobrazení:

- `ViewDidLoad` – Tato metoda je volána, když je zobrazení načtena ze souboru scénáře.
- `ViewWillAppear` – Tato metoda je volána těsně před zobrazení se zobrazí na obrazovce.
- `ViewDidAppear` – Tato metoda je volána přímo po zobrazení je zobrazené na obrazovce.
- `ViewWillDisappear` – Tato metoda je volána těsně před zobrazení se odebere z obrazovky.
- `ViewDidDisappear` – Tato metoda je volána přímo po odebrání zobrazení na obrazovce.
- `UpdateViewConstraints` – Tato metoda je volána, když omezení, které definují zobrazení automaticky rozložení pozice a velikosti je potřeba aktualizovat.
- `ViewWillLayout` – Tato metoda je volána těsně před na obrazovce jsou nastíněny dílčích zobrazení tohoto zobrazení.
- `ViewDidLayout` – Tato metoda je volána přímo po dílčích zobrazení zobrazení zobrazení jsou nastíněny na obrazovce.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>Řetězu respondér

Kromě toho `NSViewControllers` jsou teď součástí okna _respondér řetězu_:

[![](indepth-images/vc01.png "Řetězu respondér")](indepth-images/vc01.png#lightbox)

A jako takový jsou drátové up přijmout a reakce na události, jako je vyjmutí, kopírování a vložení výběr položky nabídky. Toto automatické View Controller navázání dochází pouze na aplikace běžící v systému macOS Sierra (10.12) a větší.

<a name="Containment" />

### <a name="containment"></a>Členství ve skupině

V scénářů, můžete nyní implementovat řadiče zobrazení (například řadiče zobrazení rozdělení a karta View Controller) _omezení_, tak, aby mohly "obsahovat" ostatní dílčí řadiče zobrazení:

[![](indepth-images/vc02.png "Příklad zahrnutí řadiče zobrazení")](indepth-images/vc02.png#lightbox)

Podřízené zobrazení řadiče obsahují metody a vlastnosti, které chcete je propojují zpátky na jejich nadřazené řadič zobrazení a pracovat s zobrazení a odebrání zobrazení na obrazovce.

Mají všechny řadiče zobrazení kontejneru integrovaná v systému macOS konkrétní rozložení, který Apple navrhnout provedením Pokud vytvoření vlastní vlastní zobrazení řadičů kontejneru:

[![](indepth-images/vc03.png "Rozložení řadiče zobrazení")](indepth-images/vc03.png#lightbox)

Řadiče zobrazení kolekce obsahuje pole kolekce zobrazit položky, z nichž každý obsahuje jeden nebo více řadičů zobrazení, které obsahují vlastní zobrazení.

<a name="Segues" />

## <a name="segues"></a>Segues

Segues zadejte vztahy mezi všemi na pozadí, které definují rozhraní vaší aplikace. Pokud jste obeznámeni s práce v scénářů v iOS, víte, že Segues pro iOS obvykle definují přechodů mezi zobrazení celé obrazovky. Tím se liší od systému macOS, když Segues obvykle definují "[omezení](#Containment)", kde jeden scény je podřízená položka nadřazené scény.

V systému macOS zpravidla skupiny jejich zobrazení společně v rámci stejného časového období používání prvky uživatelského rozhraní, jako je například rozdělení zobrazení a karty většiny aplikací. Na rozdíl od iOS, kde zobrazení musí být přešla zapnout a vypnout obrazovku, zobrazení kvůli omezené fyzického místa.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>Prezentace Segues

S ohledem na systému macOS tendence směrem členství ve skupině, existují situace, kdy _Segues prezentace_ používají, jako je například modální okna, náhledů a Popovers. typy systému macOS poskytuje následující předdefinované segue:

- **Zobrazit** -zobrazí cílem Segue jako nemodální okno. Tento typ Segue použijte například k dispozici další instanci okna dokumentu. ve vaší aplikaci.
- **Modální** -uvede cíl Segue jako modální okna. Tento typ Segue použijte například k dispozici okno Předvolby pro vaši aplikaci.
- **List** -uvede cílem Segue jako list připojen do nadřazeného okna. Například použijte tento typ segue nabídne najít a nahradit listu.
- **Popover** -uvede cílem Segue jako popover okno. Například použijte tento typ Segue Pokud chcete nabízet možnosti při kliknutí na prvek uživatelského rozhraní uživatelem.
- **Vlastní** -uvede cíl Segue pomocí vlastního typu Segue definované vývojář. Najdete v článku [vytváření vlastní Segues](#Creating-Custom-Segues) části níže další podrobnosti.

Při použití Segues prezentace, můžete přepsat `PrepareForSegue` metoda nadřazené řadič zobrazení pro prezentaci k chybě při inicializaci a proměnné a poskytují řadiče zobrazení se zobrazí žádná data.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>Aktivuje Segues

Spouštěná Segues umožňují určit pojmenovaný Segues (prostřednictvím jejich **identifikátor** vlastnost v Tvůrci rozhraní) a potom kliknul aktivované událostmi, jako je například uživatel kliknutím na tlačítko nebo voláním `PerformSegue` metoda v kódu:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Segue ID je definován v Xcode na rozhraní tvůrce při jsou rozložení uživatelském rozhraní aplikace:

[![](indepth-images/sg02.png "Zadávání Segue název")](indepth-images/sg02.png#lightbox)

V Kontroleru zobrazení, která funguje jako zdroj Segue, by měly přepsat `PrepareForSegue` metoda a proveďte všechny inicializace požadované předtím, než se spustí Segue a zadaný kontroler zobrazení se zobrazí:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Volitelně můžete přepsat `ShouldPerfromSegue` metoda a kontrolu, jestli Segue skutečně proveden prostřednictvím kódu C#. Ručně vidění zobrazení řadičů, volání jejich `DismissController` metoda je odebrat ze zobrazení, když už nejsou potřeba.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Vytváření vlastní Segues

Můžou nastat situace, když vaše aplikace vyžaduje typ Segue není poskytovaný definované v systému macOS Segues sestavení v. Pokud je to tento případ, můžete vytvořit vlastní Segue, jemuž lze přiřadit v Xcode na rozhraní tvůrce při rozložení uživatelského rozhraní vaší aplikace.

Například pokud chcete vytvořit nový typ Segue, který nahradí aktuální řadiče zobrazení uvnitř okna (namísto otevírání cílové scény v novém okně), můžeme použít následující kód:

```csharp
using System;
using AppKit;
using Foundation;

namespace OnCardMac
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Swap the controllers
            source.View.Window.ContentViewController = destination;

            // Release memory
            source.RemoveFromParentViewController ();
        }
        #endregion

    }
        
}
```

Několik věcí, které si zde:

- Používáme `Register` atribut vystavit této třídy lze Objective-C nebo systému macOS.
- Jsme přepsání `Perform` metoda naše vlastní Segue ve skutečnosti akci.
- Jsme nahrazujete okna `ContentViewController` kontroler s cílem (cíl) Segue definováno.
- Nemůžeme se odebrání původní řadič zobrazení pro uvolnění paměti pomocí `RemoveFromParentViewController` metoda.

V Xcode na rozhraní tvůrce použít tento nový typ Segue, musíme nejprve kompilace aplikace potom přepnout na Xcode a přidat nové Segue mezi dvěma scény. Nastavte **styl** k **vlastní** a **Segue třída** k `ReplaceViewSegue` (název naše vlastní třídy Segue):

[![](indepth-images/sg01.png "Nastavení Segue – třída")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>Okno řadiče

Okno řadiče obsahovat a řídit s různými typy okno, které můžete vytvořit aplikaci systému macOS. Pro scénářů mají následující funkce:

1. Musí zadat řadič zobrazení obsahu. Bude jím stejného obsahu řadiče zobrazení, který má podřízená okna.
2. `Storyboard` Vlastnost bude obsahovat scénáři, která řadič okno byl načten z, jinak `null` Pokud není načtená ze scénáře.
3. Můžete volat `DismissController` metoda pro danou okno zavřít a jeho odebrání ze zobrazení.

Jako řadiče zobrazení okna řadiče implementovat `PerformSegue`, `PrepareForSegue` a `ShouldPerfromSegue` metody a může sloužit jako zdroj Segue operace.

Okno řadiče jsou zodpovědní za následující funkce systému macOS aplikace:

- Konkrétní Kontrola osamocených spravují.
- (Pokud je k dispozici) spravují záhlaví okna a panelu nástrojů.
- Spravují obsahu řadiče zobrazení k zobrazení obsahu okna.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>Nástroje pro rozpoznávání gesto

Nástroje pro rozpoznávání gesto pro systému macOS téměř identické se svými protějšky v iOS a umožňuje vývojářům snadno přidat gesta (například kliknutí na tlačítko myši) na elementy v uživatelském rozhraní vaší aplikace.

Ale kde gesta v iOS jsou určeny návrhu aplikace (například klepnutím na obrazovce s dvěma prsty), většina gesta v systému macOS je dáno hardwaru.

Pomocí nástroje pro rozpoznávání gesto můžete výrazně snížit množství kód potřebný k přidávání vlastních interakce k položce v uživatelském rozhraní. Jak můžete automaticky určit mezi dvojité a jednoho kliknutí, klikněte na tlačítko a přetáhněte události, atd.

Místo `MouseDown` událost v View Controller, měli byste použít pro rozpoznávání gesto se zpracovat událost vstupu uživatele při práci s scénářů.

Následující nástroje pro rozpoznávání gesto jsou k dispozici v systému macOS:

- `NSClickGestureRecognizer` -Zaregistrujte myši směrem dolů a událostí.
- `NSPanGestureRecognizer` -Registry tlačítko myši směrem dolů, přetáhněte a verzí události.
- `NSPressGestureRecognizer` -Registry tlačítko myši stisknuté pro dané množství času událostí.
- `NSMagnificationGestureRecognizer` -Zaregistruje událost zvětšení z trackpadu hardwaru.
- `NSRotationGestureRecognizer` -Zaregistruje událost otočení z trackpadu hardwaru.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Pomocí odkazů na scénáře

Odkaz Storyboard umožňuje využít scénáře návrh složitých a rozdělit na menší scénářů, které získat na něj odkazovat z původní, proto odebírání odebrání složitost a provádění výsledná jednotlivých scénářů snadnější k návrhu a Udržujte.

Kromě toho můžete zadat odkaz Storyboard _ukotvení_ na jiné scény v rámci stejné Storyboard nebo konkrétní scény na jiný.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Odkazování na externí scénáře

Pokud chcete přidat odkaz na externí Storyboard, postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **přidat** > **nový soubor...**   >  **Mac** > **Storyboard**. Zadejte **název** pro nové scénáře a klikněte na **nový** tlačítko: 

    [![](indepth-images/ref01.png "Přidání nové scénáře")](indepth-images/ref01.png#lightbox)
2. V **Průzkumníku**, dvakrát klikněte na nový název Storyboard otevřete pro úpravy v Tvůrci rozhraní pro Xcode.
2. Návrh rozložení scény nové scénáře, jako za normálních okolností byste a uložte změny: 

    [![](indepth-images/ref02.png "Navrhování rozhraní")](indepth-images/ref02.png#lightbox)
3. Umožňuje přepnout do scénáře, který budete přidávat odkaz na v Tvůrci rozhraní.
4. Přetáhněte **scénáře odkaz** z **objektu knihovny** na návrhovou plochu: 

    [![](indepth-images/ref03.png "Výběr Storyboard odkaz v knihovně")](indepth-images/ref03.png#lightbox)
5. V **atribut Inspector**, vyberte název **Storyboard** kterou jste vytvořili výše: 

    [![](indepth-images/ref04.png "Konfigurace odkazu")](indepth-images/ref04.png#lightbox)
6. Ovládací prvek, klikněte na Widget uživatelského rozhraní (např. tlačítka) na existující scény a vytvořit nové Segue k **Storyboard odkaz** kterou jste právě vytvořili.  V místní nabídce vyberte **zobrazit** k dokončení Segue: 

    [![](indepth-images/ref06.png "Nastavení typu Segue")](indepth-images/ref06.png#lightbox) 
8. Uložte změny do scénáře.
9. Vraťte se k Visual Studio pro Mac na synchronizaci změn.

Zobrazí se při spuštění aplikace a uživatel klikne na prvek uživatelského rozhraní, které jste vytvořili Segue z počáteční řadiče okno z externí Storyboard zadaný v odkaz na scénáře.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Odkazování na konkrétní scény v externí scénáře

Chcete-li přidat odkaz na konkrétní scény externí Storyboard (a ne počáteční řadič okno), postupujte takto:

1. V **Průzkumníku**, dvakrát klikněte na externí Storyboard otevřete pro úpravy v Tvůrci rozhraní pro Xcode.
2. Přidejte nové scény a návrh jeho rozložení běžným způsobem: 

    [![](indepth-images/ref07.png "Navrhování rozložení v Xcode")](indepth-images/ref07.png#lightbox)
3. V **Identity Inspector**, zadejte **Storyboard ID** pro řadič nové scény okno: 

    [![](indepth-images/ref08.png "Nastavení ID scénáře")](indepth-images/ref08.png#lightbox)
3. Otevřete scénáře, který budete přidávat odkaz na v Tvůrci rozhraní.
4. Přetáhněte **scénáře odkaz** z **objektu knihovny** na návrhovou plochu: 

    [![](indepth-images/ref03.png "Výběr Storyboard odkaz z knihovny")](indepth-images/ref03.png#lightbox)
5. V **Identity Inspector**, vyberte název **Storyboard** a **ID odkazu** (Storyboard ID) scény, kterou jste vytvořili výše: 

    [![](indepth-images/ref09.png "Nastavení ID odkazu")](indepth-images/ref09.png#lightbox)
6. Ovládací prvek, klikněte na Widget uživatelského rozhraní (např. tlačítka) na existující scény a vytvořit nové Segue k **Storyboard odkaz** kterou jste právě vytvořili. V místní nabídce vyberte **zobrazit** k dokončení Segue: 

    [![](indepth-images/ref06.png "Nastavení typu Segue")](indepth-images/ref06.png#lightbox) 
8. Uložte změny do scénáře.
9. Vraťte se k Visual Studio pro Mac na synchronizaci změn.

Pokud je aplikace spustit a uživatel klikne na prvek uživatelského rozhraní, kterou jste vytvořili Segue z scény s danou **Storyboard ID** z externí Storyboard zadaný v Storyboard odkaz se zobrazí.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Odkazování na konkrétní scény ve scénáři, stejné

Pokud chcete přidat odkaz na konkrétní scény stejné Storyboard, postupujte takto:

1. V **Průzkumníku**, dvakrát klikněte na scénáři otevřete pro úpravy.
2. Přidejte nové scény a návrh jeho rozložení běžným způsobem: 

    [![](indepth-images/ref11.png "Úpravy storyboard v Xcode")](indepth-images/ref11.png#lightbox)
3. V **Identity Inspector**, zadejte **Storyboard ID** pro řadič nové scény okno: 

    [![](indepth-images/ref12.png "Nastavení ID scénáře")](indepth-images/ref12.png#lightbox)
3. Přetáhněte **scénáře odkaz** z **sada nástrojů** na návrhovou plochu: 

    [![](indepth-images/ref03.png "Výběr Storyboard odkaz z knihovny")](indepth-images/ref03.png#lightbox)
5. V **atribut Inspector**, vyberte **ID odkazu** (Storyboard ID) scény, kterou jste vytvořili výše: 

    [![](indepth-images/ref13.png "Nastavení ID odkazu")](indepth-images/ref13.png#lightbox)
6. Ovládací prvek, klikněte na Widget uživatelského rozhraní (např. tlačítka) na existující scény a vytvořit nové Segue k **Storyboard odkaz** kterou jste právě vytvořili. V místní nabídce vyberte **zobrazit** k dokončení Segue: 

    [![](indepth-images/ref06.png "Výběr typu Segue")](indepth-images/ref06.png#lightbox) 
8. Uložte změny do scénáře.
9. Vraťte se k Visual Studio pro Mac na synchronizaci změn.

Pokud je aplikace spustit a uživatel klikne na prvek uživatelského rozhraní, kterou jste vytvořili Segue z scény s danou **Storyboard ID** ve stejném scénáři, zadaný v Storyboard odkaz se zobrazí.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Příklad komplexní scénáře

Práce s scénářů v aplikaci Xamarin.Mac komplexní například najdete [SourceWriter ukázkovou aplikaci](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter je editor jednoduché zdrojového kódu, který poskytuje podporu pro doplňování kódu a zvýraznění syntaxe jednoduché.

Kód SourceWriter má plně komentář, pokud je k dispozici odkazy být zadané a z klíčové technologie nebo metody na příslušné informace v dokumentaci k Xamarin.Mac příručky.

## <a name="related-links"></a>Související odkazy

- [MacStoryboard (ukázka)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s Windows](~/mac/user-interface/window.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do systému Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
