---
title: Sandboxing Xamarin.Mac aplikace
description: "Tento článek se zabývá sandboxing Xamarin.Mac aplikaci pro verzi na webu App Store. Pokrývá všechny prvky, které patří do sandboxing, např. kontejneru adresáře, oprávnění, určit uživatele oprávnění, oprávnění oddělení a vynucení jádra."
ms.topic: article
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9e64f1962e35372a6058f4b515efa5a61c1c9e45
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="sandboxing-a-xamarinmac-app"></a>Sandboxing Xamarin.Mac aplikace

_Tento článek se zabývá sandboxing Xamarin.Mac aplikaci pro verzi na webu App Store. Pokrývá všechny prvky, které patří do sandboxing, např. kontejneru adresáře, oprávnění, určit uživatele oprávnění, oprávnění oddělení a vynucení jádra._

## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte stejné možnosti izolovaného prostoru aplikace jako při práci s Objective-C nebo Swiftu.

[![Příkladem spuštěné aplikaci](sandboxing-images/intro01.png "příklad spuštěné aplikaci")](sandboxing-images/intro01-large.png)

V tomto článku vám nabídneme základní informace o práci s sandboxing v Xamarin.Mac aplikace a všechny prvky, které patří do sandboxing: kontejneru adresáře, oprávnění, určit uživatele oprávnění, oprávnění oddělení a vynucení jádra. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` atributy Umožňuje propojit až tříd jazyka C# a uživatelského rozhraní jazyka Objective-C objekty elementy.

## <a name="about-the-app-sandbox"></a>O izolovaný prostor aplikace

Izolovaný prostor aplikace poskytuje silnou ochranu proti škody, které může být způsobeno škodlivé aplikace spuštěná na Macu omezením přístupu, který má aplikace systémové prostředky.

Bez v izolovaném prostoru aplikace má úplná oprávnění uživatele, který je spuštěna aplikace a můžete získat přístup nebo dělat všechno, co uživatel může. Pokud aplikace obsahuje zabezpečení díry (nebo všechny rozhraní, které používá), můžete se hacker potenciálně zneužít tato slabé stránky a používat aplikaci převzít kontrolu nad Mac, který je spuštěn na.

Omezením přístupu k prostředkům na základě jednotlivých aplikací v izolovaném prostoru aplikaci poskytuje ochranu proti krádeži, poškození nebo zlými úmysly ze strany aplikace běžící na počítači uživatele.

Izolovaný prostor aplikace je technologie řízení přístupu integrovaná v systému macOS (vynucené na úrovni jádra), která poskytuje strategie dva účely:

1. Izolovaný prostor aplikace umožňuje vývojáři popisují _jak_ aplikace bude komunikovat s operačním systémem a tímto způsobem, jsou udělena pouze přístupová práva, které jsou nutné k provedení úlohy a další.
2. Izolovaný prostor aplikace umožňuje uživatelům bezproblémově udělit další přístup k systému prostřednictvím otevřít a uložit dialogových oken, přetažení operace a další běžné uživatelské interakce.

### <a name="preparing-to-implement-the-app-sandbox"></a>Příprava k implementaci izolovaný prostor aplikace

Elementy izolovaného prostoru aplikace, která bude mít podrobněji v článku jsou následující:

- Kontejner adresáře
- Oprávnění
- Určit uživatele oprávnění
- Oddělení oprávnění
- Vynucení jádra

Jakmile porozumíte tyto podrobnosti, budete moct vytvořit plán pro přijetí izolovaného prostoru aplikace v aplikaci Xamarin.Mac.

Nejprve musíte určit, zda je vaše aplikace vhodným kandidátem na sandboxing (Většina aplikací jsou). Dále musíte vyřešit všechny nekompatibility rozhraní API a určit, jaký prvek izolovaného prostoru aplikace budete potřebovat. Nakonec podívejte se do aplikace pomocí oprávnění oddělení můžete maximalizovat úroveň obrany aplikace.

Některé umístění systému souborů, které vaše aplikace používá při přijímání izolovaného prostoru aplikace, se budou lišit. Adresář kontejneru, který se použije pro soubory podpory aplikace, databáze, mezipaměti a další soubory, které nejsou dokumenty uživatele zvlášť, bude mít vaše aplikace. Systému macOS i Xcode poskytuje podporu pro migraci tyto typy souborů, z jejich starší verze umístění do kontejneru.

## <a name="sandboxing-quick-start"></a>Sandboxing rychlý start

V této části vytvoříme jednoduchou aplikaci Xamarin.Mac, která používá webové zobrazení (který vyžaduje připojení k síti, která je omezená pod sandboxing, pokud není výslovně vyžádané) jako příklad Začínáme s izolovaného prostoru aplikace.

Ověříme, že aplikace je ve skutečnosti v izolovaném prostoru a zjistěte, jak odstraňovat potíže a řešit běžné chyby izolovaného prostoru aplikace.

### <a name="creating-the-xamarinmac-project"></a>Vytvoření projektu Xamarin.Mac

Umožňuje provést následující příkaz pro vytvoření projektu naše ukázka:

1. Spuštění sady Visual Studio pro Mac a kliknutím **nové řešení...** Odkaz.
2. Z **nový projekt** dialogové okno, vyberte **Mac** > **aplikace** > **kakao aplikace**: 

    [![Vytvoření nové aplikace kakao](sandboxing-images/sample01.png "vytvořit novou aplikaci, kakao")](sandboxing-images/sample01-large.png)
3. Klikněte **Další** tlačítko, zadejte `MacSandbox` pro název projektu a klikněte na tlačítko **vytvořit** tlačítko: 

    [![Zadat název aplikace](sandboxing-images/sample02.png "zadáte název aplikace")](sandboxing-images/sample02-large.png)
4. V **řešení Pad**, dvakrát klikněte **Main.storyboard** soubor otevřete pro úpravy v Xcode: 

    [![Úpravy hlavní storyboard](sandboxing-images/sample03.png "úpravy hlavní storyboard")](sandboxing-images/sample03-large.png)
5. Přetáhněte **webové zobrazení** do okna, velikost vyplnil celou oblast obsahu, a nastavte ji na zvýšit nebo snížit s okno: 

    [![Přidání webové zobrazení](sandboxing-images/sample04.png "přidání webové zobrazení")](sandboxing-images/sample04-large.png)
6. Vytvoření výstupu pro webové zobrazení s názvem `webView`: 

    [![Vytvoření nové výstupu](sandboxing-images/sample05.png "vytváření nové výstupu")](sandboxing-images/sample05-large.png)
7. Vrátit k sadě Visual Studio pro Mac a dvakrát klikněte na soubor **ViewController.cs** v soubor **řešení Pad** otevřete pro úpravy.
8. Přidejte následující příkaz using: `using WebKit;`
9. Ujistěte se, `ViewDidLoad` metoda vypadá takto: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Uložte provedené změny.

Aplikaci spustit a ujistěte se, že je na webu Apple nezobrazí v okně:

[![Zobrazuje příklad aplikace spuštění](sandboxing-images/sample06.png "zobrazující spuštění příklad aplikace")](sandboxing-images/sample06-large.png)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>Podepisování a zřizování pro aplikace

Můžeme provést aktivaci izolovaného prostoru aplikace, nejprve musíme zřídit a podepsání aplikace Xamarin.Mac.

Umožní, postupujte takto:

1. Přihlaste se k portálu pro vývojáře Apple: 

    [![Protokolování do portálu pro vývojáře Apple](sandboxing-images/sign01.png "přihlášením se do portálu pro vývojáře Apple")](sandboxing-images/sign01-large.png)
2. Vyberte **certifikáty, identifikátory a profily**: 

    [![Výběr certifikáty, identifikátory a profily](sandboxing-images/sign02.png "výběru certifikátů, identifikátory a profily")](sandboxing-images/sign02-large.png)
3. V části **Mac aplikace**, vyberte **identifikátory**: 

    [![Výběr identifikátory](sandboxing-images/sign03.png "výběr identifikátory")](sandboxing-images/sign03-large.png)
4. Vytvoření nového ID pro aplikaci: 

    [![Vytvoření nového ID aplikace](sandboxing-images/sign04.png "vytváření nové ID aplikace")](sandboxing-images/sign04-large.png)
5. V části **profily zřizování**, vyberte **vývoj**: 

    [![Výběr vývoj](sandboxing-images/sign05.png "výběr vývoj")](sandboxing-images/sign05-large.png)
6. Vytvoření nového profilu a vyberte **vývoj aplikací pro Mac**: 

    [![Vytvoření nového profilu](sandboxing-images/sign06.png "vytvoření nového profilu")](sandboxing-images/sign06-large.png)
7. Vyberte ID aplikace jsme vytvořili výše: 

    [![Výběr ID aplikace](sandboxing-images/sign07.png "výběr ID aplikace")](sandboxing-images/sign07-large.png)
8. Vyberte vývojáři pro tento profil: 

    [![Přidání vývojáři](sandboxing-images/sign08.png "přidání vývojáře")](sandboxing-images/sign08-large.png)
9. Vyberte počítače, pro tento profil: 

    [![Výběr povolené počítačů](sandboxing-images/sign09.png "výběr povolené počítačů")](sandboxing-images/sign09-large.png)
10. Zadejte název profilu: 

    [![Profil pojmenujete](sandboxing-images/sign10.png "pojmenujete profil")](sandboxing-images/sign10-large.png)
11. Klikněte **provádí** tlačítko.

> [!IMPORTANT]
> V některých případech může být nutné stáhnout nový profil zřizování přímo z portálu pro vývojáře společnosti Apple a poklikejte na něj nainstalovat. Může se také muset zastavte a restartujte Visual Studio pro Mac, než bude moci nový profil.

V dalším kroku potřebujeme načíst nové ID aplikace a profilu na vývojovém počítači. Pojďme postupujte takto:

1. Spusťte Xcode a vyberte položku **Předvolby** z **Xcode** nabídky: 

    ![Úprava účtů v Xcode](sandboxing-images/sign11.png "úpravy účty v Xcode")
2. Klikněte na **zobrazit podrobnosti...**  tlačítko: 

    ![Klepnutím na tlačítko Zobrazit podrobnosti](sandboxing-images/sign12.png "klepnutím na tlačítko Zobrazit podrobnosti")
3. Klikněte na **aktualizovat** tlačítko (v levém dolním).
4. Klikněte na **provádí** tlačítko.

Dále je potřeba v našem Xamarin.Mac projektu vyberte nové ID aplikace a profil zřizování. Pojďme postupujte takto:

1. V **řešení Pad**, dvakrát klikněte **Info.plist** soubor otevřete pro úpravy.
2. Ujistěte se, že **identifikátor svazku** odpovídá naše ID aplikace, které jsme vytvořili výše (Příklad: `com.appracatappra.MacSandbox`): 

    [![Úpravy identifikátor balíku](sandboxing-images/sign13.png "úpravy identifikátor balíku")](sandboxing-images/sign13-large.png)
3. V dalším kroku klikněte dvakrát na **Entitlements.plist** souboru a ujistěte se, naše **Icloudu úložiště dvojic klíč-hodnota** a **Icloudu kontejnery** všechny odpovídat naše ID aplikace, které jsme vytvořili výše (Příklad: `com.appracatappra.MacSandbox`): 

    [![Úpravy souboru Entitlements.plist](sandboxing-images/sign17.png "úprav souboru Entitlements.plist")](sandboxing-images/sign17-large.png)
3. Uložte provedené změny.
4. V **řešení Pad**, poklikejte na soubor projektu otevřete její možnosti pro úpravy:  

    ![Editign na řešení možnosti](sandboxing-images/sign14.png "Editign možnosti řešení")
5. Vyberte **podepisování Mac**, zkontrolujte **přihlásit k sadě aplikací** a **přihlášení je balíček Instalační služby**. V části **profil pro zřizování**, vyberte příslušnou možnost, jsme vytvořili výše: 

    ![Nastavení profilu pro zřizování](sandboxing-images/sign15.png "nastavení profilu pro zřizování")
6. Klikněte **provádí** tlačítko.

> [!IMPORTANT]
> **Poznámka:** bude pravděpodobně nutné ukončit a restartovat Visual Studio pro Mac, aby ho rozpoznat nové ID aplikace a profil zřizování, který jste nainstalovali Xcode.

#### <a name="troubleshooting-provisioning-issues"></a>Řešení potíží s problémy se zřizováním

V tomto okamžiku by měl pokusit spusťte aplikaci a ujistěte se, že všechno, co je podepsaná a správně zřízený. Pokud aplikace stále běží jako předtím, vše, co je správná. V případě selhání může získat dialogové okno stejný, jako je následující:

[![Příklad problém dialog zřizování](sandboxing-images/sign16.png "příklad problém dialog zřizování")](sandboxing-images/sign16-large.png)

Tady jsou nejběžnější příčiny zřizování a podepisování problémy:

- ID sady aplikace neodpovídá ID aplikace vybraný profil.
- ID vývojáře neodpovídá ID vývojáře vybraný profil.
- Identifikátor UUID testuje na Mac není zaregistrován v rámci vybraného profilu.

V případě chyby opravte problém na portál pro vývojáře Apple, aktualizujte profily v Xcode a provést čisté sestavení v sadě Visual Studio for Mac.

### <a name="enable-the-app-sandbox"></a>Povolit aplikaci izolovaného prostoru

Povolíte izolovaného prostoru aplikace, vyberte zaškrtávací políčko v možnosti projekty. Postupujte takto:

1. V **řešení Pad**, dvakrát klikněte **Entitlements.plist** soubor otevřete pro úpravy.
2. Zkontrolujte i **povolte oprávnění** a **povolit aplikaci Sandboxing**: 

    [![Úpravy oprávnění a povolení sandboxing](sandboxing-images/sign17.png "úpravy oprávnění a povolení sandboxing")](sandboxing-images/sign17-large.png)
3. Uložte provedené změny.

V tomto okamžiku jste povolili izolovaného prostoru aplikace, ale nezadali přístup k požadované síti pro webové zobrazení. Pokud nyní aplikaci spustíte, měli byste obdržet prázdné okno:

[![Zobrazuje webový přístup blokován](sandboxing-images/sample08.png "zobrazující webový přístup blokován")](sandboxing-images/sample08-large.png)

### <a name="verifying-that-the-app-is-sandboxed"></a>Ověření, že je aplikace v izolovaném prostoru

Kromě zajištění dostatečného prostředků blokování chování existují tři hlavní způsoby, jak zjistit, pokud byl úspěšně v izolovaném prostoru Xamarin.Mac aplikace:

1. V hledání, zkontrolujte obsah `~/Library/Containers/` složky – pokud je aplikace v izolovaném prostoru, bude složka s názvem, jako je identifikátor vaší aplikace sady (Příklad: `com.appracatappra.MacSandbox`): 

    [![Otevření aplikace sady](sandboxing-images/sample09.png "otevření aplikace sady")](sandboxing-images/sample09-large.png)
2. Systém se zobrazí aplikace jako v izolovaném prostoru v monitoru aktivity:
    - Spustit monitorování aktivity (v části `/Applications/Utilities`). 
    - Zvolte **zobrazení** > **sloupce** a ujistěte se, že **izolovaného prostoru** je zaškrtnuta možnost položku nabídky.
    - Ujistěte se, že sloupci izolovaného prostoru čte `Yes` pro aplikaci: 

    [![Kontrola aplikace v monitoru aktivity](sandboxing-images/sample10.png "Kontrola aplikace v monitoru aktivity")](sandboxing-images/sample10-large.png)
3. Zkontrolujte, že je binární aplikace v izolovaném prostoru:
    - Spusťte aplikaci terminálu.
    - Přejděte do aplikace `bin` adresáře.
    - Tento příkaz: `codesign -dvvv --entitlements :- executable_path` (kde `executable_path` je cesta k aplikaci): 

    [![Kontrola aplikace na příkazovém řádku](sandboxing-images/sample11.png "Kontrola aplikace na příkazovém řádku")](sandboxing-images/sample11-large.png)

### <a name="debugging-a-sandboxed-app"></a>Ladění aplikace pro práci s řešeními v izolovaném prostoru

Ladicí program se připojí k Xamarin.Mac aplikací prostřednictvím protokolu TCP, což znamená, že ve výchozím nastavení při povolení sandboxing, nelze se připojit k aplikaci, takže pokud se pokusíte spustit aplikaci bez oprávnění povoleno, dojde k chybě *"nelze se připojit k ladicí program"*. 

[![Požadované možnosti nastavení](sandboxing-images/debug01.png "požadované možnosti nastavení")](sandboxing-images/debug01-large.png)

**Povolit odchozí síťové připojení (klienta)** oprávnění, která je potřeba v ladicím programu, povolíte tento jeden, bude mít ladění normálně. Vzhledem k tomu, že ladíte nelze bez ní, jste aktualizovali jsme `CompileEntitlements` cíle pro `msbuild` pro každou aplikaci, která je v izolovaném prostoru pro ladění sestavení pouze automaticky přidat tato oprávnění pro oprávnění. Verze sestavení by měli používat oprávnění zadaný v souboru oprávnění ponechat beze změny.

### <a name="resolving-an-app-sandbox-violation"></a>Řešení porušení izolovaný prostor aplikace

Pokud v izolovaném prostoru Xamarin.Mac pokusu o přístup k prostředku, který má aplikace nesmí být explicitně povolené, dojde k narušení izolovaného prostoru aplikace. Například naše webové zobrazení se již nebude moci zobrazit na webu Apple.

Nejběžnější zdroj aplikace izolovaného prostoru porušení nastane, když nastavení nárocích zadaný v sadě Visual Studio pro Mac neodpovídá požadavkům aplikace. Znovu zpět na našem příkladu chybějící síťové připojení oprávnění, která uchovává webového zobrazení z pracovní.

#### <a name="discovering-app-sandbox-violations"></a>Zjišťování porušení izolovaný prostor aplikace

Pokud máte podezření na narušení izolovaného prostoru aplikace dochází v aplikaci Xamarin.Mac, je nejrychlejší způsob, jak vyhledat potíže s použitím **konzoly** aplikace.

Postupujte takto:

1. Daná aplikace zkompilování a spuštění ze sady Visual Studio for Mac.
2. Otevřete **konzoly** aplikace (z `/Applications/Utilties/`).
3. Vyberte **všechny zprávy** na bočním panelu a zadejte `sandbox` do vyhledávání: 

    [![Příkladem sandboxing problém v konzole](sandboxing-images/resolve01.png "příklad sandboxing problém v konzole")](sandboxing-images/resolve01-large.png)

Pro náš příklad aplikaci výše, uvidíte, že jádra/blokuje `network-outbound` provoz z důvodu aplikace izolovaného prostoru, protože jsme nebyly požadovaná práva.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Oprava aplikace izolovaného prostoru porušení spojenými s oprávněními

Teď, když jsme viděli, jak najít aplikaci Sandboxing narušení, podíváme se, jak lze vyřešit úpravou oprávnění naší aplikace.

Postupujte takto:

1. V **řešení Pad**, dvakrát klikněte **Entitlements.plist** soubor otevřete pro úpravy.
2. V části **oprávnění** část, zkontrolujte **povolit odchozí síťové připojení (klienta)** políčko: 

    [![Úpravy oprávnění](sandboxing-images/sign17.png "úpravy oprávnění")](sandboxing-images/sign17-large.png)
3. Uložte změny do aplikace.

Pokud jsme pak provést výše pro náš příklad aplikaci sestavit a spustit ji, webový obsah nyní se zobrazí podle očekávání.

## <a name="the-app-sandbox-in-depth"></a>Podrobný izolovaný prostor aplikace

Mechanismy řízení přístupu poskytované izolovaného prostoru aplikace jsou několik a snadno pochopitelné. Způsobem, že každá aplikace bude používat aplikace izolovaného prostoru je však jedinečný a na základě požadavků aplikace.

Vzhledem k ochraně aplikace Xamarin.Mac zneužití škodlivý kód ukrást vaše usilovně, musí existovat pouze jedna chyba buď do aplikace (nebo jeden z knihovny nebo rozhraní využívá) převzít kontrolu nad aplikace interakce s systém.

Izolovaný prostor aplikace byly navrženy pro zabránit převzetí (nebo omezit, které může způsobit škodu) tím, že se k zadání aplikace určený interakce s v systému. Systém bude jenom udělit přístup k prostředku, který vaše aplikace vyžaduje, aby získat jeho provádět úlohy a žádné další.

Při návrhu izolovaného prostoru aplikace, navrhujete nejhoršího. Pokud škodlivý kód stane aplikace dojde k ohrožení, je omezený přístup jenom soubory a prostředky v izolovaném prostoru aplikace.

### <a name="entitlements-and-system-resource-access"></a>Oprávnění a přístupu k prostředkům systému

Jak jsme viděli výše, Xamarin.Mac aplikace, která nebyla v izolovaném prostoru jsou udělena úplná práva a přístup uživatele, který je spuštění aplikace. Pokud dojde k ohrožení, škodlivý kód, aplikace se nechráněných může fungovat jako agenta pro nepřátelských chování, s široké rozsahu potenciální pro provádění škodu.

Když zapnete izolovaného prostoru aplikace, odeberte všechny kromě minimální sadu oprávnění, které pak znovu povolit na základě jen potřeba pomocí aplikace Xamarin.Mac oprávnění. 

Upravit prostředky izolovaného prostoru aplikace aplikace tak, že upravíte jeho **Entitlements.plist** soubor a kontrola nebo výběr práva potřebná z rozevírací seznamy editory:

[![Úpravy oprávnění](sandboxing-images/sign17.png "úpravy oprávnění")](sandboxing-images/sign17-large.png)

### <a name="container-directories-and-file-system-access"></a>Kontejner adresářů a přístupu k systému souborů

Když aplikace Xamarin.Mac přijme izolovaného prostoru aplikace, kterému má přístup do následujícího umístění:

- **Adresář kontejneru aplikace** -při prvním spuštění operačního systému vytvoří speciální _adresář kontejneru_ tam, kde všechny jeho prostředky přejít, že pouze můžete získat přístup. Aplikace bude mít přístup čtení/zápisu do tohoto adresáře.
- **Aplikace skupiny kontejneru adresáře** – aplikace je možné udělit přístup k jedné nebo několika _skupiny kontejnery_ , jsou sdíleny mezi aplikací ve stejné skupině.
- **Soubory zadán uživatel** -aplikace automaticky získá přístup k souborům, které jsou explicitně otevřít nebo je přetáhnout do aplikace uživatelem.
- **Související položky** -s odpovídající oprávnění, vaše aplikace může mít přístup do souboru se stejným názvem, ale jiné rozšíření. Například dokument, který byl uložen jako i `.txt` souboru a `.pdf`.
- **Dočasné adresáře, nástroje příkazového řádku adresářů a konkrétní čtení world umístění** -aplikace má různým stupněm přístup k souborům v jiných umístěních dobře definovaný jako zadaný v systému.

#### <a name="the-app-container-directory"></a>Adresář kontejneru aplikace

Adresář kontejneru aplikace Xamarin.Mac aplikace má následující vlastnosti:

- Je v skrytá umístění, do domovské složky uživatele (obvykle `~Library/Containers`) a je přístupný pomocí `NSHomeDirectory` – funkce (viz níže) v rámci vaší aplikace. Protože je v domovský adresář, každý uživatel dostane vlastní kontejner pro aplikaci.
- Aplikace má neomezený přístup pro čtení a zápis k adresáři kontejneru a všechny jeho dílčí adresářů a souborů v něm.
- Většina cesta hledání systému macOS na rozhraní API jsou relativní vzhledem k aplikaci kontejneru. Například kontejner bude mít vlastní **knihovny** (přístup prostřednictvím `NSLibraryDirectory`), **podporu aplikace** a **Předvolby** podadresářů.
- systému macOS vytváří a vynucuje připojení mezi a aplikace a jejímu kontejneru prostřednictvím podepisování kódu. Dokonce je jiná aplikace pokusí zfalšovat aplikace pomocí jeho **identifikátor svazku**, nebude mít přístup k kontejneru kvůli podepisování kódu.
- Kontejner není uživatelem generovaný souborů. Místo toho je pro soubory, které vaše aplikace používá například databáze, mezipaměti nebo jiné konkrétní typy dat.
- Pro _materiálů uložených_ typů aplikace (třeba společnosti Apple fotografií), obsah uživatele přejde do kontejneru.

> [!IMPORTANT]
> **Poznámka:** bohužel Xamarin.Mac nemá 100 % pokrytí rozhraní API ještě (na rozdíl od Xamarin.iOS), výsledkem `NSHomeDirectory` v aktuální verzi Xamarin.Mac nebyl mapován rozhraní API.

Jako dočasné řešení můžete použít následující kód:

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>Adresář kontejneru skupiny aplikací

Od verze systému Mac macOS 10.7.5 (a vyšší), můžete použít aplikaci `com.apple.security.application-groups` oprávnění pro přístup k sdílené kontejner, který je společný pro všechny aplikace ve skupině. Můžete použít tento sdílený kontejner pro jiné uživatele přístupných obsah, jako jsou databáze nebo jiné typy souborů podpory aplikace (například mezipaměti).

Kontejnery skupiny se automaticky přidají do kontejneru každou aplikaci izolovaného prostoru (pokud jsou součástí skupiny) a jsou uložené v `~/Library/Group Containers/<application-group-id>`. ID skupiny _musí_ začínají vaše ID Team vývoj a dobou, například:

```plist
<team-id>.com.company.<group-name>
```

Další informace najdete v tématu společnosti Apple [přidání aplikace do skupiny aplikací](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) v [nárocích Key Reference](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Powerbox a souboru systému přístup mimo aplikaci kontejneru

V izolovaném prostoru Xamarin.Mac aplikace mají přístup k umístění souborů systému mimo jeho kontejneru následujícími způsoby:

- Na konkrétní směr uživatele (prostřednictvím otevřít a uložit v dialogových oknech nebo jiné metody, jako je například přetažení).
- Pomocí oprávnění pro konkrétní soubor systému umístění (například `/bin` nebo `/usr/lib`).
* Pokud je umístění systému souborů v určité adresáře, které jsou world čitelný (například sdílení).

_Powerbox_ je systému macOS technologie zabezpečení, která komunikuje s uživatelem a rozbalte v izolovaném prostoru Xamarin.Mac aplikace soubor přístupová práva. Powerbox má žádné rozhraní API, ale transparentně aktivována, pokud aplikace zavolá `NSOpenPanel` nebo `NSSavePanel`. Je povolen Powerbox přístup prostřednictvím oprávnění, která jste nastavili pro vaši aplikaci Xamarin.Mac.

Když v izolovaném prostoru aplikace zobrazí otevřenou nebo uložit dialogové okno, okno je předkládán Powerbox (a ne AppKit) a proto má přístup k libovolný soubor nebo adresář, který má uživatel přístup k.

Pokud uživatel vybere soubor nebo adresář otevřený nebo uložit dialogové okno (nebo nastavuje tažením buď na ikonu aplikace), přidá Powerbox přidruženou cestu do izolovaného prostoru aplikace.

Kromě toho systém automaticky povolí následující aplikace v izolovaném prostoru:

- Připojení k systému vstupní metodu.
- Volání služeb vybraných uživatelem z **služby** nabídky (pouze pro služby označena jako _bezpečné pro přístup z aplikace izolovaného prostoru_ poskytovatelem služeb).
- Otevřené soubory zvolte uživatel z **otevřít poslední** nabídky.
- Mezi další aplikace, použijte kopírování a vkládání.
- V následujících umístěních čtení world číst soubory:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Číst a zapisovat soubory v adresářích vytvořené `NSTemporaryDirectory`.

Jako výchozí, soubory otevřít nebo uložit v izolovaném prostoru aplikací Xamarin.Mac zůstat přístupný dokud aplikace ukončí (Pokud je soubor byl stále otevřen, když je aplikace ukončena). Otevřené soubory se automaticky obnoví do izolovaného prostoru aplikace pomocí funkce Obnovení systému macOS při příštím spuštění aplikace.

Pokud chcete zadat trvalost k souborům mimo kontejner Xamarin.Mac aplikace, použijte obor zabezpečení záložky (viz níže).

#### <a name="related-items"></a>Související položky

Izolovaný prostor aplikace umožňuje aplikaci přístup k související položky, které mají stejný název souboru, ale různá rozšíření. Tato funkce má dvě části: a) seznam rozšíření související s v dané aplikaci `Info.plst` souboru kódu b) říct izolovaný prostor, jaké aplikace budou by se tyto soubory.

Existují dva scénáře, kde to dává smysl:

1. Aplikace musí být možné uložit jinou verzi souboru (s příponou nové). Například export `.txt` do souboru `.pdf` souboru. Tuto situaci můžete vyřešit, je nutné použít `NSFileCoordinator` přistupovat k souboru. Budete volat `WillMove(fromURL, toURL)` metoda nejdřív přesuňte soubor na nové rozšíření a pak zavolají `ItemMoved(fromURL, toURL)`.
2. Aplikace je potřeba otevřít hlavní soubor s jedno rozšíření a několik podpůrných souborů s různými příponami. Například film a soubor Subtitle. Použijte `NSFilePresenter` k získání přístupu k souboru sekundární. Zadejte hlavní soubor `PrimaryPresentedItemURL` vlastnost a sekundární soubor `PresentedItemURL` vlastnost. Po otevření souboru hlavní volání `AddFilePresenter` metodu `NSFileCoordinator` třídy pro registraci soubor sekundární.

V obou případech aplikace na **Info.plist** souboru musí deklarovat typů dokumentů, které můžete otevřít aplikaci. Pro soubor jakéhokoli typu, přidejte `NSIsRelatedItemType` (s hodnotou `YES`) k jeho záznam v `CFBundleDocumentTypes` pole.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Otevřít a uložit chování dialogové okno s aplikacemi v izolovaném prostoru

Následující omezení se umístí na `NSOpenPanel` a `NSSavePanel` při jejich volání z aplikace v izolovaném prostoru Xamarin.Mac:

- Nelze vyvolat prostřednictvím kódu programu **OK** tlačítko.
- Nelze prostřednictvím kódu programu změnit výběr uživatele v `NSOpenSavePanelDelegate`.

Kromě toho jsou následující úpravy dědičnosti na místě:

- **Aplikace není v izolovaném prostoru** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Záložky obor zabezpečení a přístupu k prostředkům trvalé

Jak jsme uvedli výše, můžete k aplikaci v izolovaném prostoru Xamarin.Mac přístup k souboru či prostředku mimo jeho kontejneru prostřednictvím přímé uživatelské interakce (jak poskytované PowerBox). Ale přístup k těmto prostředkům není zachována automaticky přes spustí aplikace nebo systému restartování.

Pomocí _Security-Scoped záložky_, aplikace v izolovaném prostoru Xamarin.Mac můžete zachovat záměru uživatele a spravovat přístup k dané prostředky po aplikaci restartovat.

#### <a name="security-scoped-bookmark-types"></a>Typy záložku obor zabezpečení

Při práci s Security-Scoped záložky a trvalé přístup k prostředkům, existují dva případy použití sistine:

- **Záložku App-Scoped poskytuje trvalý přístup k zadané uživatelem souboru nebo složce.** 

    Například pokud aplikace v izolovaném prostoru Xamarin.Mac umožňuje používat k otevření dokumentu externí pro úpravy (pomocí `NSOpenPanel`), aplikace, můžete vytvořit záložku App-Scoped tak, aby měl přístup ke stejnému souboru znovu.
- **Záložku Document-Scoped poskytuje trvalé přístup k konkrétní dokumentu pro dílčí soubor.** 

Například video úpravy aplikaci, která vytvoří soubor projektu, který má přístup k jednotlivé bitové kopie, videosoubory a zvukové soubory, které budou později zkombinované do jednoho film.

Když uživatel naimportuje soubor prostředků do projektu (prostřednictvím `NSOpenPanel`), aplikace vytvoří záložku Document-Scoped pro položku, která je uložená v projektu, aby soubor má vždy přístup k aplikaci.

Záložku Document-Scoped lze vyřešit jakékoli aplikace, která můžete otevřít záložku data a samotného dokumentu. To podporuje přenositelnost, které uživateli umožňují odesílat soubory projektu pro jiného uživatele, které mají všechny záložky fungovat také pro ně.

> [!IMPORTANT]
> **Poznámka:** Document-Scoped Bookman můžete _pouze_ přejděte do jednoho souboru a nikoliv složka a tento soubor nemůže být v umístění používá systém (například `/private` nebo `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Použití záložek obor zabezpečení

Pomocí jednoho z typů Security-Scoped záložku, vyžaduje, abyste proveďte následující kroky:

1. **Nastavit příslušná oprávnění v Xamarin.Mac aplikaci, která potřebuje používat záložky Security-Scoped** -For App-Scoped záložky nastavit `com.apple.security.files.bookmarks.app-scope` nárocích klíče `true`. Pro záložky Document-Scoped nastavit `com.apple.security.files.bookmarks.document-scope` nárocích klíče `true`.
2. **Vytvoření záložky Security-Scoped** – můžete to udělat pro libovolný soubor nebo složku, která uživatel zadal přístup k (prostřednictvím `NSOpenPanel` třeba), že aplikace bude potřebovat trvalé přístup k. Použití `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` metodu `NSUrl` třídy za účelem vytvoření záložky.
3. **Vyřešte záložku Security-Scoped** – když aplikace potřebuje pro přístup k prostředku znovu (třeba po restartování) je nutné přeložit záložku na adresu URL obor zabezpečení. Použití `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` metodu `NSUrl` třída přeložit záložky.
4. **Explicitně upozornění systému, kterou chcete přístup k souboru z adresy URL Security-Scoped** – tento krok je třeba provést okamžitě po získání adresy URL Security-Scoped výše, nebo pokud chcete později po nutnosti znovu získat přístup k prostředku uvolní, když váš přístup k němu. Volání `StartAccessingSecurityScopedResource ()` metodu `NSUrl` třída zahájíte přístup k adrese URL Security-Scoped.
5. **Explicitně upozornění systému, který jste hotovi přístupu k souboru z adresy URL Security-Scoped** -co nejdříve by měl informovat systém, když aplikace už potřebuje přístup k souboru (například pokud uživatel nezavře ji). Volání `StopAccessingSecurityScopedResource ()` metodu `NSUrl` třída zastavit, přístup k adrese URL Security-Scoped.

Po můžete opustit přístup k prostředku, budete muset vraťte se ke kroku 4 znovu a znovu vytvořit tak přístup. Pokud je restartován Xamarin.Mac aplikace, musíte vraťte se ke kroku 3 a znovu vyřešit záložky.

> [!IMPORTANT]
> **Poznámka:** chyby k uvolnění přístup k prostředkům Security-Scoped URL způsobí, že aplikace Xamarin.Mac pronikly jádra prostředky. V důsledku toho aplikace už nebude moct přidávat umístění systému souborů k jejímu kontejneru, dokud se nerestartuje.

### <a name="the-app-sandbox-and-code-signing"></a>Izolovaný prostor aplikace a podepisování kódu

Po povolení aplikace izolovaného prostoru a povolit konkrétním požadavkům pro vaši aplikaci Xamarin.Mac (prostřednictvím oprávnění), musí kódu přihlašovací projektu pro sandboxing vstoupily v platnost. Je třeba provést, protože oprávnění požadované pro aplikaci Sandboxing je spojena s podpisem aplikaci pro podpis kódu. 

systému macOS vynucuje propojení mezi aplikace kontejner a jeho podpis kódu, tímto způsobem, žádná jiná aplikace přístup tohoto kontejneru, i v případě, že ho je falšování identity aplikace ID sady prostředků. Tento mechanismus funguje takto:

1. Systém vytvoří kontejner aplikace, nastaví _seznam řízení přístupu_ (ACL) v tomto kontejneru. Položka řízení počáteční přístupu v seznamu obsahuje aplikace _určen požadavek_ (DR), který popisuje, jak budoucích verzí aplikace může být rozeznána (Pokud byl upgradován).
2. Pokaždé, když aplikace se stejným ID sady spouští, systém kontroluje, že podpisu kódu aplikace odpovídá určené požadavky uvedené v jedné z položek v seznamu ACL kontejneru. Pokud systém nenajde odpovídající, zabrání aplikaci spouštění.

Kódu přihlašování funguje takto:

1. Před vytvořením projektu Xamarin.Mac, získáte vývojový certifikát, distribuce certifikát a certifikát ID vývojáře z portálu pro vývojáře Apple.
2. Když Mac App Storu distribuuje Xamarin.Mac aplikace, je podepsaný podpisem kódu Apple.

Při testování a ladění, budete používat verzi aplikace Xamarin.Mac, zda jste přihlášeni (který se použije k vytvoření kontejneru aplikace). Později Pokud chcete testovat nebo verzi nainstalujte z Apple App Storu, ji budou podepsané podpisem společnosti Apple a nebude možné spustit (protože nemá stejným podpisem kódu jako původní kontejneru aplikace). V takovém případě se zobrazí hlášení havárií podobný následujícímu:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Chcete-li odstranit tento problém, budete muset upravit položce seznamu ACL tak, aby odkazoval na Apple podepsaný verzi aplikace.

Další informace o vytváření a stahuje potřebné pro Sandboxing profily zřizování, najdete v tématu [podepisování a zřizování aplikace](#Signing_and_Provisioning_the_App) část výše.

#### <a name="adjusting-the-acl-entry"></a>Úprava položce seznamu ACL

Pokud chcete povolit Apple podepsaný verzi Xamarin.Mac aplikaci spustit, postupujte takto:

1. Otevřete aplikaci Terminálové (v `/Applications/Utilities`).
2. Otevřete okno vyhledávací Apple podepsaný verzi Xamarin.Mac aplikace.
3. Typ `asctl container acl add -file ` v okně terminálu.
4. Ikona aplikace Xamarin.Mac z okna vyhledávací přetažení ho na okno terminálu.
5. Úplná cesta k souboru bude přidán k příkazu v terminálu.
6. Stiskněte klávesu **Enter** provedení příkazu.

Seznam ACL kontejneru nyní obsahuje požadavky určené kódu pro obě verze Xamarin.Mac aplikace a systému macOS se teď povolit buď verze ke spuštění.

#### <a name="display-a-list-of-acl-code-requirements"></a>Zobrazení seznamu ACL kód požadavky

Pomocí následujícího postupu můžete zobrazit seznam požadavků kódu v seznamu řízení přístupu kontejneru:

1. Otevřete aplikaci Terminálové (v `/Applications/Utilities`).
2. Typ `asctl container acl list -bundle <container-name>`.
3. Stiskněte klávesu **Enter** provedení příkazu.

`<container-name>` Je obvykle identifikátor balíku Xamarin.Mac aplikace.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Návrh aplikace Xamarin.Mac pro izolovaný prostor aplikace

Je běžné pracovní postup, který by měl být sledována při navrhování aplikace Xamarin.Mac pro izolovaný prostor aplikace. Ale nutné dodat, jaké jsou specifikace implementace sandboxing v aplikaci se chystáte je jedinečný pro danou aplikaci funkce.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Šest kroků pro přijetí izolovaný prostor aplikace

Návrh aplikace Xamarin.Mac pro izolovaný prostor aplikace obvykle se skládá z následujících kroků:

1. Určí, zda aplikace vhodné pro sandboxing.
2. Navrhování strategie vývoje a rozdělení.
3. Vyřešte případné nekompatibility rozhraní API.
4. Požadovaná oprávnění aplikace izolovaného prostoru se vztahují na Xamarin.Mac projektu.
5. Přidejte oprávnění oddělení pomocí XPC.
6. Implementujte strategie migrace.

> [!IMPORTANT]
> **Poznámka:** musíte nejen izolovaného prostoru hlavní spustitelný soubor aplikace sady, ale také každých zahrnuté pomocné rutiny aplikace nebo nástroj v této sady. To je potřeba pro všechny aplikace distribuované z Mac App Storu a pokud je to možné, by mělo být provedeno pro jakoukoli jinou formu distribuce aplikací.

Seznam všech spustitelné binární soubory aplikace Xamarin.Mac sady zadejte následující příkaz v terminálu:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Kde `[Your-App-Bundle]` je název a cesta k sadě aplikace.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Určit, zda je vhodný pro sandboxing Xamarin.Mac aplikace

Většina Xamarin.Mac aplikace jsou plně kompatibilní se izolovaného prostoru aplikace a proto vhodný pro sandboxing. Pokud aplikace vyžaduje chování, které neumožňuje izolovaného prostoru aplikace, měli byste zvážit alternativní způsob.

Pokud vaše aplikace vyžaduje jednu z následujících chování, není kompatibilní s aplikací izolovaného prostoru:

- **Autorizace služby** -s aplikace Sandbox, nemůže pracovat s funkcí popsaných v [referenční dokumentace jazyka C autorizace služby](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **Usnadnění rozhraní API** -nelze vytvořit izolovaný prostor usnadnění aplikace, jako jsou třeba čtečky obrazovky nebo aplikace, které řídí jiné aplikace.
- **Odesílat události Apple na libovolné aplikace** -aplikace vyžaduje odesílání událostí Apple neznámé, libovolné aplikace, nemůže být v izolovaném prostoru. Známé seznam názvem aplikace aplikace může být stále v izolovaném prostoru a oprávnění bude nutné zahrnout seznamu názvem aplikace.
- **Odeslat uživatelské informace slovníky v distribuované oznámení na jiné úlohy** -s aplikace Sandbox, nemůže obsahovat `userInfo` slovníku při publikování na `NSDistributedNotificationCenter` objekt pro zasílání zpráv Další úlohy.
- **Načtení rozšíření jádra** -načítání rozšíření jádra zakazuje izolovaného prostoru aplikace.
- **Simulaci uživatele vstupní otevřít a uložit dialogů** – prostřednictvím kódu programu manipulace s otevřít nebo uložit dialogová okna pro simulaci nebo alter uživatelský vstup zakazuje izolovaného prostoru aplikace.
- **Přístup k nebo nastavení předvoleb na ostatní aplikace** -manipulace s nastavení jiné aplikace zakazuje izolovaného prostoru aplikace.
- **Konfigurace nastavení sítě** -izolovaného prostoru aplikace zakazuje manipulace s nastavení sítě.
- **Ostatní aplikace se ukončuje** -Sandbox aplikace zakazuje použití `NSRunningApplication` ukončit jiné aplikace.

### <a name="resolving-api-incompatibilities"></a>Řešení nekompatibility rozhraní API

Při návrhu aplikace Xamarin.Mac pro izolovaný prostor aplikace se můžete setkat nekompatibilitu s použití některých systému macOS rozhraní API.

Zde jsou několik běžných problémů a věcí, které můžete provést k vyřešení je:

- **Otevření, uložení a sledování dokumentů** – Pokud spravujete dokumentů pomocí jakékoli technologie jiné než `NSDocument`, by měl přejdete k němu z důvodu integrované podpory pro izolovaný prostor aplikace. `NSDocument` automaticky spolupracuje s PowerBox a poskytuje podporu pro zachování dokumenty v rámci vaší izolovaného prostoru, pokud se uživatel přesune v hledání.
- **Zachovat přístup k souboru systémové prostředky** – Pokud Xamarin.Mac aplikace závisí na trvalý přístup k prostředkům mimo jeho kontejneru Udržovat přístup pomocí záložky obor zabezpečení.
- **Vytvoření položky přihlášení pro aplikaci** -s aplikace Sandbox, nelze vytvořit přihlášení pomocí položky `LSSharedFileList` ani můžete upravit stav spuštění služeb pomocí `LSRegisterURL`. Použití `SMLoginItemSetEnabled` fungovat, jak je popsáno v jablka [přidávání položek přihlášení pomocí služby Management Framework](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) dokumentaci.
- **Přístup k uživatelským datům** – Pokud používáte POSIX funkce, jako `getpwuid` získat domovského adresáře uživatele z adresářových služeb, zvažte použití kakao nebo základní Foundation symboly, jako `NSHomeDirectory`.
- **Přístup k předvolby další aplikace** – protože izolovaného prostoru aplikace přesměruje cesta vyhledání rozhraní API do kontejneru aplikace Úprava předvolby proběhne v rámci tohoto kontejneru a přístup k jiné předvoleb aplikace v není povolen. 
- **Použití vložených Video HTML5 v zobrazení webové** – Pokud Xamarin.Mac aplikace používá WebKit přehrávání vložená videa HTML5, je nutné také propojit aplikaci pro rozhraní AV Foundation. Izolovaný prostor aplikace zabrání CoreMedia play tyto videa, jinak hodnota.

### <a name="applying-required-app-sandbox-entitlements"></a>Použití oprávnění požadované aplikace izolovaného prostoru

Budete muset upravit oprávnění pro každou aplikaci Xamarin.Mac, který chcete spustit v izolovaném prostoru aplikaci a zkontrolujte **povolit Sandboxing aplikace** zaškrtávací políčko.

Podle toho, funkce aplikace, můžete povolit další oprávnění k přístupu k funkcím operačního systému nebo prostředky. Aplikace Sandboxing funguje nejlépe, když minimalizujete oprávnění, které požadavek na úplné minimálním předpokladem pro spuštění aplikace právě náhodně oprávnění tak povolit.

Chcete-li zjistit, které oprávnění Xamarin.Mac aplikace vyžaduje, postupujte takto:

1. Povolit aplikaci izolovaný prostor a spuštění aplikace Xamarin.Mac.
2. Spusťte pomocí funkcí aplikace.
3. Otevřete aplikaci konzoly (k dispozici v `/Applications/Utilities`) a vyhledejte `sandboxd` porušení v **všechny zprávy** protokolu.
4. Pro každou `sandboxd` , vyřešte problém, buď pomocí kontejneru aplikace místo jiné umístění systému souborů nebo použít oprávnění aplikace izolovaného prostoru pro povolení přístupu k funkcím s omezeným přístupem operačního systému.
5. Znovu spustit a otestovat všechny funkce aplikace Xamarin.Mac znovu.
6. Dokud všechny opakování `sandboxd` porušení bylo vyřešeno.

### <a name="add-privilege-separation-using-xpc"></a>Přidání oprávnění oddělení pomocí XPC

Při vývoji aplikace Xamarin.Mac pro izolovaný prostor aplikaci, podívejte se na chování aplikace v podmínkách oprávnění a přístupu a potom vezměte v úvahu oddělení s vysokým rizikem operací do své vlastní XPC služby.

Další informace najdete v tématu společnosti Apple [vytváření služeb XPC](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) a [průvodci programováním služby a démoni](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Implementace strategie migrace

Pokud jsou vydání nové, v izolovaném prostoru verzi Xamarin.Mac aplikace, který nebyl dříve v izolovaném prostoru, budete muset aktuální uživatelům zajistit bezproblémovou upgradu. 

 Informace o tom, jak implementovat manifestu migrace kontejneru najdete společnosti Apple [migrace aplikace na izolovaném prostoru](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) dokumentaci.

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout sandboxing trvá Xamarin.Mac aplikace. Nejdřív jsme vytvořili jednoduše Xamarin.Mac aplikaci zobrazit základní informace o aplikaci izolovaného prostoru. V dalším kroku jsme vám ukázal, jak vyřešit porušení izolovaného prostoru. Potom vzali jsme podrobný podívejte se na aplikace izolovaný prostor a nakonec jsme prohlédl návrhu aplikace Xamarin.Mac pro izolovaný prostor aplikace.



## <a name="related-links"></a>Související odkazy

- [Publikování do obchodu App Store](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [O izolovaný prostor aplikace](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Běžné problémy Sandboxing aplikace](https://developer.apple.com/library/content/qa/qa1773/_index.html)
