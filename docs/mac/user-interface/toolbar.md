---
title: "Panely nástrojů"
description: "Tento článek popisuje práci s panely nástrojů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržba panely nástrojů v Xcode a rozhraní tvůrce, je vystavení kódu a práci s nimi prostřednictvím kódu programu."
ms.topic: article
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 92b90f8d4655ea89b67e81f3235b6fd9b6d92833
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="toolbars"></a>Panely nástrojů

_Tento článek popisuje práci s panely nástrojů v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržba panely nástrojů v Xcode a rozhraní tvůrce, je vystavení kódu a práci s nimi prostřednictvím kódu programu._

Vývojáři Xamarin.Mac práce pomocí sady Visual Studio pro Mac mají přístup ke stejným ovládacích prvků uživatelského rozhraní, které jsou k dispozici pro vývojáře v systému macOS práce s Xcode, včetně ovládací prvek panelu nástrojů. Protože Xamarin.Mac integruje přímo s Xcode, je možné použít Tvůrce Xcode je rozhraní pro vytváření a údržbu položky panelu nástrojů. Tyto položky panelu nástrojů můžete také vytvořit v jazyku C#.

Panely nástrojů v systému macOS jsou přidány do horní části okna a poskytují snadný přístup k příkazům související se jeho funkce. Panely nástrojů můžete skryté, zobrazí nebo přizpůsobit uživatelé aplikace, a můžou poskytnout položky panelu nástrojů různými způsoby.

Tento článek popisuje základní informace o práci s panely nástrojů a položky panelu nástrojů v aplikaci Xamarin.Mac. 

Než budete pokračovat, pročtěte [Hello, Mac](~/mac/get-started/hello-mac.md) článku – konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) části – Jak zahrnuje klíčové koncepty a techniky, které se použijí v této příručce.

Taky podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu. Vysvětluje `Register` a `Export` atributy používané pro připojení k třídy jazyka Objective-C třídy jazyka C#.

## <a name="introduction-to-toolbars"></a>Úvod do panely nástrojů

Žádné okno v systému macOS aplikace může zahrnovat panelu nástrojů:

![Okno s příklad s panelem nástrojů](toolbar-images/info01.png "okno s příklad s panelu nástrojů")

Panely nástrojů poskytují snadný způsob pro uživatele vaší aplikace rychle se dostat k důležité nebo běžně používané funkce. Například úpravy dokumentu aplikace může poskytovat položek panelu nástrojů pro nastavení barvy, změna písma nebo tisk aktuálním dokumentu.

Panely nástrojů můžete zobrazovat položky třemi způsoby:

1. **Ikona a Text.** 

     ![Panel nástrojů s ikony a text](toolbar-images/info02.png "panel nástrojů s ikony a text.")

2. **Ikona pouze** 

     ![Panelu nástrojů jen ikonu](toolbar-images/info03.png "jen ikonu panelu nástrojů")

3. **Pouze text** 

     ![Panel nástrojů textovém](toolbar-images/info04.png "textovém panelu nástrojů")

Přepínání mezi těmito režimy tak, že kliknete pravým tlačítkem na panelu nástrojů a v kontextové nabídce vyberete režim zobrazení:

![Kontextové nabídky pro panel nástrojů](toolbar-images/info05.png "kontextové nabídky pro panel nástrojů")

Použijte stejné nabídce zobrazen panel nástrojů v menší velikosti:

![Panel nástrojů s ikony. Malé ikony](toolbar-images/info06.png "panel nástrojů s ikony. Malé ikony")

Také umožňuje přizpůsobení panelu nástrojů v nabídce:

![Dialogové okno použít pro přizpůsobení panelu nástrojů](toolbar-images/info07.png "dialogu použít pro přizpůsobení panelu nástrojů")

Při nastavování panelu nástrojů v Tvůrci rozhraní na Xcode, vývojář může poskytnout další panelu nástrojů položky, které nejsou součástí výchozí konfiguraci. Uživatelé aplikace potom můžete přizpůsobit panel nástrojů, přidávání a odebírání těchto předdefinovaných položek podle potřeby. Samozřejmě panelu nástrojů můžete obnovit výchozí konfiguraci.

Panelu nástrojů se automaticky připojí k **zobrazení** nabídce, která umožňuje uživatelům skrýt ho, je zobrazit a upravit ho:

![Položky panelu nástrojů v nabídce zobrazení](toolbar-images/info08.png "položky panelu nástrojů v nabídce zobrazení")

Najdete v článku [integrovanou funkci nabídky](~/mac/user-interface/menu.md) dokumentace pro další podrobnosti.

Kromě toho pokud panelu nástrojů je správně nakonfigurován v rozhraní tvůrce, aplikace se automaticky zachová přizpůsobení panelu nástrojů napříč více spustí aplikace.

Další části této příručky popisují, jak vytvořit a udržovat panely nástrojů s Xcode na rozhraní tvůrce a jak pracovat s nimi v kódu.

## <a name="setting-a-custom-main-window-controller"></a>Nastavení vlastní hlavní okno řadiče

Prvky uživatelského rozhraní kódu C# pomocí výstupy a akce, aplikace Xamarin.Mac vystavit musí použít řadič vlastní okno:

1. V Tvůrci rozhraní na Xcode otevřete storyboard aplikace.
2. Vyberte řadič okno na návrhovou plochu.
3. Přepnout **Identity Inspector** a zadejte "WindowController" jako **název třídy**: 

    [![Nastavení názvu vlastní třídu zařízení okno](toolbar-images/windowcontroller01.png "název vlastní třídu pro kontroler okno nastavení")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci.
5. A **WindowController.cs** soubor bude přidán do projektu v **řešení Pad** v sadě Visual Studio pro Mac: 

    ![V panelu řešení pro výběr WindowController.cs](toolbar-images/windowcontroller02.png "WindowController.cs, vyberte v poli pro řešení")

6. Znovu otevřete storyboard v Tvůrci rozhraní pro Xcode.
7. **WindowController.h** soubor bude k dispozici pro použití: 

    [![Soubor WindowController.h](toolbar-images/windowcontroller03.png "WindowController.h souboru")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Vytvoření a údržbu panely nástrojů v Xcode

Panely nástrojů se vytvářejí a spravují s Xcode na rozhraní tvůrce. Na panelu nástrojů do aplikace přidat, upravit primární storyboard aplikace (v tomto případě **Main.storyboard**) poklepáním v **řešení Pad**:

![Otevření Main.storyboard v panelu řešení pro](toolbar-images/edit01.png "otevření Main.storyboard v panelu pro řešení")

V **knihovny Inspector**, zadejte "nástroj" v **vyhledávacího pole** aby bylo snazší zobrazíte všechny položky k dispozici nástrojů:

![Knihovna Inspector, vyfiltrovány a zobrazí se položky panelu nástrojů](toolbar-images/edit02.png "The Inspector knihovny vyfiltrovány a zobrazí se položky panelu nástrojů")

Přetáhněte na okno v panelu nástrojů **rozhraní editoru**. Pomocí panelu nástrojů nakonfigurovat pomocí nastavení vlastnosti v své chování **atributy Inspector**:

![Nástroj Inspector atributy pro panel nástrojů](toolbar-images/edit04.png "The Inspector atributy pro panel nástrojů")

K dispozici jsou následující vlastnosti:

1. **Zobrazení** -Určuje, zda panelu nástrojů zobrazí ikony, text nebo obojí
2. **Při spuštění viditelné** -li vybráno, zobrazí ve výchozím nastavení se panelu nástrojů.
3. **Přizpůsobitelné** – Pokud vyberete, uživatelé mohou upravit a přizpůsobení panelu nástrojů.
4. **Oddělovač** – Pokud vyberete, dynamické vodorovném řádku odděluje od obsah okna panelu nástrojů.
5. **Velikost** -nastaví velikost panelu nástrojů
6. **Automatické ukládání** – Pokud vyberete, aplikace se zachová změny konfigurace nástrojů uživatele napříč aplikace spustí.

Vyberte **automatické ukládání** možnost a nechat všechny ostatní vlastnosti v obnoveno výchozí nastavení. 

Po otevření panelu nástrojů v **rozhraní hierarchie**, otevřete dialogové okno Vlastní nastavení tak, že vyberete položku panelu nástrojů:

![Přizpůsobení panelu nástrojů](toolbar-images/edit05.png "přizpůsobení panelu nástrojů")

Pomocí tohoto dialogového okna pro nastavení vlastností pro položky, které již jsou součástí nástrojů návrhu výchozí panel nástrojů pro aplikaci, a zajistit další panelu nástrojů položky pro uživatele po přizpůsobení panelu nástrojů vyberte. Chcete-li přidat položky do panelu nástrojů, přetáhněte je z **knihovny Inspector**:

![Knihovna Inspector](toolbar-images/edit06.png "Inspector knihovny")

Následující položky panelu nástrojů můžete přidat:

- **Obrázek položka panelu nástrojů** -položku panelu nástrojů s vlastní image jako ikona.
- **Flexibilní položky panelu nástrojů místo** -flexibilní prostoru se používá k odůvodnění, dalších nástrojů položky. Například jeden nebo více položek panelu nástrojů následuje položka panelu nástrojů flexibilní místa a jiná položka panelu nástrojů by připnout poslední položky na pravé straně panelu nástrojů.
- **Místo položky panelu nástrojů** -pevné mezery mezi položky na panelu nástrojů
- **Položka panelu nástrojů oddělovače** -viditelné oddělovač mezi dva nebo více položek panelu nástrojů pro seskupení
- **Přizpůsobení panelu nástrojů položky** – umožňuje uživatelům umožnit přizpůsobení panelu nástrojů
- **Tisk položka panelu nástrojů** – umožňuje uživatelům tisknout otevřít dokument
- **Zobrazit položku panelu nástrojů barvy** -zobrazí volby barev standardní systému: 

     ![Výběr barvy systému](toolbar-images/edit07.png "systémový výběr barvy")

- **Zobrazit položku panelu nástrojů písma** -zobrazí dialogové okno Písmo standardní systému: 

     ![Výběr písma](toolbar-images/edit08.png "modulu pro výběr písem")

> [!IMPORTANT]
> Jak uvidíte později, mnoho standardní kakao ovládacích prvků uživatelského rozhraní například vyhledávacích polí, segmentovaným ovládací prvky a vodorovné posuvníky lze také přidat na panel nástrojů.

### <a name="adding-an-item-to-a-toolbar"></a>Přidání položky do panelu nástrojů

Postup přidání položky na panel nástrojů, vyberte v panelu nástrojů **rozhraní hierarchie** a klikněte na jednu z jeho položky způsobuje zobrazit dialogové okno Vlastní nastavení. V dalším kroku přetáhněte novou položku z **knihovny Inspector** k **povolené položky panelu nástrojů** oblasti:

![Povolené položky panelu nástrojů v dialogovém okně přizpůsobení panelu nástrojů](toolbar-images/add01.png "The položky panelu nástrojů povolen v dialogovém okně přizpůsobení panelu nástrojů")

Pokud chcete mít jistotu, že nová položka je součástí výchozí panel nástrojů, přetáhněte ji do **položky panelu nástrojů výchozí** oblasti: 

![Změna pořadí položek panelu nástrojů tak, že přetáhnete](toolbar-images/add02.png "tak, že přetáhnete změny pořadí položek panelu nástrojů")

Chcete-li změnit pořadí položek panelu nástrojů výchozí, přetáhněte je doleva nebo doprava.

Pak pomocí **atributy Inspector** k nastavení výchozích vlastností pro položku:

![Přizpůsobení položka panelu nástrojů pomocí Inspector atributy](toolbar-images/add03.png "přizpůsobení pomocí Inspector atributy položka panelu nástrojů")

K dispozici jsou následující vlastnosti:

- **Název obrázku** -obrázek, který má použít jako ikona pro položku
- **Popisek** -Text k zobrazení pro položku na panelu nástrojů
- **Popisek palety** -Text k zobrazení pro položku v **povolené položky panelu nástrojů** oblasti
- **Značka** -volitelné jedinečný identifikátor, který pomáhá identifikovat položku v kódu.
- **Identifikátor** -definuje typ položky panelu nástrojů. Vlastní hodnotu lze zvolit položku panelu nástrojů v kódu.
- **Volitelný** – Pokud je zaškrtnuto, položka bude fungovat stejně jako tlačítko Zapnout nebo vypnout.

> [!IMPORTANT]
> Přidat položku, kterou chcete **povolené položky panelu nástrojů** oblasti, ale není výchozí panel nástrojů k poskytování možnosti přizpůsobení pro uživatele. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Přidání dalších ovládacích prvků uživatelského rozhraní na panelu nástrojů

Několik prvky uživatelského rozhraní kakao, jako pole hledání a segmentovaným ovládací prvky lze také přidat na panel nástrojů.

Chcete-li to vyzkoušet, otevřete panelu nástrojů v **rozhraní hierarchie** a vyberte položku panelu nástrojů tím otevřete dialogové okno Vlastní nastavení. Přetáhněte **pole hledání** z **knihovny Inspector** k **povolené položky panelu nástrojů** oblasti:

![V dialogovém okně přizpůsobení panelu nástrojů](toolbar-images/add05.png "pomocí dialogu přizpůsobení panelu nástrojů")

Z tohoto umístění pomocí rozhraní tvůrce konfigurace pole hledání a umístěte ji do kódu pomocí akce nebo výstupu.

## <a name="built-in-toolbar-item-support"></a>Podpora položky integrovaného panelu nástrojů

Několik prvky uživatelského rozhraní kakao interakci s položkami standardním panelu nástrojů ve výchozím nastavení. Například, přetáhněte **textového zobrazení** na okno aplikace a umístit je tak, aby vyplnil oblast obsahu:

[![Přidání zobrazení textu do aplikace](toolbar-images/edit09.png "přidání textového zobrazení aplikace")](toolbar-images/edit09-large.png#lightbox)

Uložte dokument, vraťte se do sady Visual Studio pro Mac, aby synchronizovat s Xcode, spusťte aplikaci, zadejte text, vyberte ho a klikněte **barvy** položka panelu nástrojů. Všimněte si, že textového zobrazení automaticky spolupracuje s volby barev:

![Funkce integrované nástrojů pomocí výběru zobrazení a barvu textu](toolbar-images/edit10.png "integrovaný panel nástrojů funkce pomocí výběru zobrazení a barvu textu")

## <a name="using-images-with-toolbar-items"></a>Bitové kopie pomocí položky panelu nástrojů

Pomocí **položka panelu nástrojů Image**, všechny bitové mapy image přidat do **prostředky** složky (a zadané sestavení akce **sady prostředků**) lze zobrazit na panelu nástrojů jako ikona:

1. V sadě Visual Studio pro Mac v **řešení Pad**, klikněte pravým tlačítkem myši **prostředky** složky a vyberte **přidat** > **přidat soubory** .
2. Z **přidat soubory** dialogové okno pole, přejděte na požadovanou bitové kopie, vyberte je a klikněte **otevřete** tlačítko: 

    [![Výběr Image přidat](toolbar-images/edit11.png "výběru Image, přidat")](toolbar-images/edit11-large.png#lightbox)

3. Vyberte **kopie**, zkontrolujte **použít stejnou akci pro všechny vybrané soubory**a klikněte na tlačítko **OK**:

    ![Výběr akce kopie pro přidání bitové kopie](toolbar-images/edit12.png "výběrem akce kopie pro přidání bitové kopie")

4. V **řešení Pad**, dvakrát klikněte na **MainWindow.xib** otevřít v Xcode.

5. Vyberte v panelu nástrojů **rozhraní hierarchie** a klikněte na jednu z jeho položky tím otevřete dialogové okno Vlastní nastavení.

6. Přetáhněte **položka panelu nástrojů bitové kopie** z **knihovny Inspector** na panelu nástrojů **povolené položky panelu nástrojů** oblasti: 

    ![Přidat položku panelu nástrojů bitové kopie do oblasti povolené položky panelu nástrojů](toolbar-images/edit14.png "položka panelu nástrojů Image přidáno do oblasti povolené položky panelu nástrojů")

7. V **atributy Inspector**, vyberte bitovou kopii, která byla právě přidána v sadě Visual Studio pro Mac: 

    ![Nastavení vlastní image pro položku panelu nástrojů](toolbar-images/edit15.png "nastavení vlastní image pro položku panelu nástrojů")

8. Nastavte **popisek** "Koše" a **Popisek palety** "Vymazat dokumentu": 

    ![Nastavení panelu nástrojů položky popisek a palety popisek](toolbar-images/edit16.png "nastavení panelu nástrojů položky popisek a Popisek palety")

9. Přetáhněte **položka panelu nástrojů oddělovače** z **knihovny Inspector** na panelu nástrojů **povolené položky panelu nástrojů** oblasti: 

    [![Položka panelu nástrojů oddělovače přidáno do oblasti povolené položky panelu nástrojů](toolbar-images/edit17.png "položka panelu nástrojů A oddělovače přidáno do oblasti povolené položky panelu nástrojů")](toolbar-images/edit17-large.png#lightbox)

10. Přetáhněte položka oddělovač, položka "Koš", která má **výchozí položky panelu nástrojů** oblasti a sadu pořadí panelu nástrojů položky z zleva doprava (barvy, písma, oddělovač, Koš, flexibilní místa, tisk) takto: 

    ![Položky panelu nástrojů výchozí](toolbar-images/edit18.png "výchozí položky panelu nástrojů")

11. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Spusťte aplikaci ověřte, zda je ve výchozím nastavení zobrazí nový panel nástrojů:

![Panel nástrojů s vlastní výchozí položky](toolbar-images/edit19.png "s vlastní výchozí položky panelu nástrojů")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Vystavení položky panelu nástrojů, výstupy a akcí

Pro přístup k panelu nástrojů nebo Položka panelu nástrojů v kódu, musí být připojené výstupu nebo akce:

1. V **řešení Pad**, dvakrát klikněte na **Main.storyboard** otevřít v Xcode.
2. Ujistěte se, že vlastní třídu "WindowController" přiřazený k řadiči hlavní okno v **Identity Inspector**:

    [![Pomocí Identity Inspector nastavení vlastní třídy pro okno řadič](toolbar-images/edit20a.png "pomocí Identity Inspector nastavit vlastní třídu pro kontroler okno")](toolbar-images/edit20a-large.png#lightbox)

3. Potom vyberte položku panelu nástrojů v **rozhraní hierarchie**: 

    ![Když vyberete položku panelu nástrojů v hierarchii rozhraní](toolbar-images/edit20.png "vyberete položku panelu nástrojů v hierarchii rozhraní")  

4. Otevřete **pomocníka zobrazení**, vyberte **WindowController.h** soubor a přetáhněte ovládací prvek z položky panelu nástrojů na **WindowController.h** souboru.
5. Nastavte **připojení** typ, který má **akce**, zadejte "trashDocument" **název**a klikněte na tlačítko **připojit** tlačítko: 

    [![Konfigurace akce pro položku panelu nástrojů](toolbar-images/edit23.png "konfigurace akce pro položku panelu nástrojů")](toolbar-images/edit23-large.png#lightbox)

6. Vystavení **textového zobrazení** jako výstupu názvem "documentEditor" v **ViewController.h** souboru: 

    [![Konfigurace výstupu pro zobrazení textu](toolbar-images/edit24.png "konfigurace výstupu pro zobrazení textu")](toolbar-images/edit24-large.png#lightbox)

7. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

V sadě Visual Studio pro Mac, upravit **ViewController.cs** souboru a přidejte následující kód:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Potom upravte **WindowController.cs** souboru a přidejte následující kód k dolnímu okraji `WindowController` třídy:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Při spuštění aplikace, **Koš** položka panelu nástrojů bude aktivní:

![Panel nástrojů s položku Koš active](toolbar-images/edit25.png "panel nástrojů s položku Koš active")

Všimněte si, že **Koš** položka panelu nástrojů lze nyní odstranit text.

## <a name="disabling-toolbar-items"></a>Deaktivace položek panelu nástrojů

Zakázat položku na panelu nástrojů, vytvořte vlastní `NSToolbarItem` třídy a přepsat `Validate` metoda. V Tvůrci rozhraní přiřadíte vlastní typ na položku, kterou chcete povolit nebo zakázat.

Chcete-li vytvořit vlastní `NSToolbarItem` třídy, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **nový soubor...** . Vyberte **Obecné** > **prázdné třídy**, zadejte "ActivatableItem" **název**a klikněte na tlačítko **nový** tlačítko: 

![Přidání prázdné třídy v sadě Visual Studio pro Mac](toolbar-images/custom01.png "přidání prázdné třídy v sadě Visual Studio pro Mac")

Potom upravte **ActivatableItem.cs** souboru tímto:

```csharp
using System;

using Foundation;
using AppKit;

namespace MacToolbar
{
    [Register("ActivatableItem")]
    public class ActivatableItem : NSToolbarItem
    {
        public bool Active { get; set;} = true;

        public ActivatableItem ()
        {
        }

        public ActivatableItem (IntPtr handle) : base (handle)
        {
        }

        public ActivatableItem (NSObjectFlag  t) : base (t)
        {
        }

        public ActivatableItem (string title) : base (title)
        {
        }

        public override void Validate ()
        {
            base.Validate ();
            Enabled = Active;
        }
    }
}
```

Klikněte dvakrát na **Main.storyboard** otevřít v Xcode. Vyberte **Koš** položka panelu nástrojů vytvořili výše a změňte její třída "ActivatableItem" v **Identity Inspector**:

![Nastavení vlastní třídu pro položku panelu nástrojů](toolbar-images/custom02.png "nastavení vlastní třídu pro položku panelu nástrojů")

Vytvoření výstupu názvem `trashItem` pro **Koš** položka panelu nástrojů. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode. Nakonec otevřete **MainWindow.cs** a aktualizovat `AwakeFromNib` metoda tímto:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Spusťte aplikaci a Všimněte si, že **Koš** položka je nyní zakázána v panelu nástrojů:

![Panel nástrojů s položku neaktivní Koš](toolbar-images/custom03.png "s neaktivní Koš položku panelu nástrojů")

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce s panely nástrojů a položek panelu nástrojů v aplikaci Xamarin.Mac trvá. Je popsané, jak vytvořit a udržovat panely nástrojů v Tvůrci Xcode je rozhraní, jak některé ovládací prvky uživatelského rozhraní automaticky pracovat položky panelu nástrojů, jak pracovat s panely nástrojů v kódu jazyka C# a jak povolit a zakázat položky panelu nástrojů.


## <a name="related-links"></a>Související odkazy

- [MacToolbar (ukázka)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Lidské rozhraní pokyny pro panely nástrojů](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Úvod do panely nástrojů](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
