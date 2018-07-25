---
title: Základy iOS designeru
description: Tato příručka představuje návrháře Xamarin pro iOS. Ukazuje, jak používat v iOS designeru vizuálně Rozvrhněte ovládací prvky, jak získat přístup k tyto ovládací prvky v kódu a jak upravit vlastnosti.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 6905eddbc4488b08f9c9e896efe5f980e0e03345
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242365"
---
# <a name="ios-designer-basics"></a>Základy iOS designeru

_Tato příručka představuje návrháře Xamarin pro iOS. Ukazuje, jak používat v iOS designeru vizuálně Rozvrhněte ovládací prvky, jak získat přístup k tyto ovládací prvky v kódu a jak upravit vlastnosti._

Návrhář Xamarin pro iOS je podobný Tvůrce rozhraní Xcode vizuální rozhraní návrháře a Android Designer. Mezi jeho řadu funkcí, patří bezproblémovou integraci s Visual Studio pro Mac a Visual Studio 2015 a 2017, přetáhněte myší, rozhraní pro vytvoření obslužné rutiny událostí a možnost vykreslovat vlastní ovládací prvky.

## <a name="requirements"></a>Požadavky

IOS Designer je k dispozici v sadě Visual Studio pro Mac a Visual Studio 2015 a 2017 na Windows. V sadě Visual Studio 2015 nebo 2017 v iOS designeru vyžaduje připojení k správně nakonfigurovanou hostiteli buildu Mac, ale nemusí být spuštěná Xcode.

Tento průvodce to předpokládá znalost obsah do [Začínáme provede](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Jak funguje v iOS designeru

Tato část popisuje, jak v iOS designeru usnadňuje vytváření uživatelského rozhraní a jejím propojením s kódem.

IOS Designer umožňuje vývojářům vizuálně navrhovat uživatelské rozhraní aplikace. Jak je uvedeno v [Úvod do scénářů](~/ios/user-interface/storyboards/index.md) průvodce, scénář popisuje obrazovky (kontrolery zobrazení), které tvoří aplikaci, umístí na těchto kontrolery zobrazení a celkový tok navigační aplikace prvky rozhraní (zobrazení) . 

Kontroler zobrazení má dvě části: vizuální reprezentace v iOS designeru a přidružené třídy C#:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Kontroler zobrazení v iOS designeru](introduction-images/1-storyboardwithviewcontroller-vsmac.png "kontroler zobrazení v iOS designeru")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Kód pro kontroler zobrazení](introduction-images/2-viewcontrollercode-vsmac.png "kód pro kontroler zobrazení")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kontroler zobrazení v iOS designeru](introduction-images/1-storyboardwithviewcontroller-vs.png "kontroler zobrazení v iOS designeru")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Kód pro kontroler zobrazení](introduction-images/2-viewcontrollercode-vs.png "kód pro kontroler zobrazení")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

Ve výchozím stavu kontroler zobrazení neposkytuje všechny funkce. musí být vyplněno pomocí ovládacích prvků. Tyto ovládací prvky jsou umístěny v kontroleru zobrazení zobrazení obdélníkovou oblast, které obsahuje veškerý obsah na obrazovce. Většina kontrolery zobrazení obsahovat běžné ovládací prvky, jako je například tlačítek, popisků a textová pole, jak je znázorněno na následujícím snímku obrazovky, který ukazuje kontroler zobrazení obsahující tlačítko: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Kontroler zobrazení obsahující tlačítko](introduction-images/3-viewcontrollerwithbutton-vsmac.png "kontroler zobrazení obsahující tlačítko")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kontroler zobrazení obsahující tlačítko](introduction-images/3-viewcontrollerwithbutton-vs.png "kontroler zobrazení obsahující tlačítko")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Některé ovládací prvky, jako je například popisky, který obsahuje statický text, můžete přidat kontroler zobrazení a Zbývá samostatně. Ale častěji, ovládací prvky musí být přizpůsobit prostřednictvím kódu programu. Například tlačítko přidané v předchozím kroku musí něco udělat klepnutí, takže obslužná rutina události musí být přidány v kódu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud chcete přístup k a manipulaci s tlačítko v kódu, musí mít jedinečný identifikátor. Zadejte jedinečný identifikátor tak, že vyberete tlačítko, otevřete **oblasti vlastnosti**a nastavení jeho **název** pole hodnota jako například "Odeslat":

[![Nastavení názvu tlačítka v oblasti vlastnosti](introduction-images/4-settingbuttonname-vsmac.png "nastavení název tlačítka v oblasti Vlastnosti")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud chcete přístup k a manipulaci s tlačítko v kódu, musí mít jedinečný identifikátor. Zadejte jedinečný identifikátor tak, že vyberete tlačítko, otevřete **okno vlastností**a nastavení jeho **název** pole hodnota jako například "Odeslat":

[![Nastavení názvu tlačítka v okně Vlastnosti](introduction-images/4-settingbuttonname-vs.png "nastavení název tlačítka v okně Vlastnosti")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Teď, když tlačítko má název, může být přístupné z kódu. Ale jak to funguje?

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V **oblasti řešení**, navigační k **ViewController.cs** a kliknutím na indikátor zpřístupnění zjistí, že kontroler zobrazení `ViewController` soubory rozsahy definice třídy, dvě, z nichž každý obsahuje [částečné třídy](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definice:

[![Dva soubory, které tvoří třídu ViewController: ViewController.cs a ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "dva soubory, které tvoří třídu ViewController: ViewController.cs a ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V **Průzkumníka řešení**, navigační k **ViewController.cs** a kliknutím na indikátor zpřístupnění zjistí, že kontroler zobrazení `ViewController` definice třídy obsahuje dva soubory, každý z které se nachází [částečné třídy](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definice:

[![Dva soubory, které tvoří třídu ViewController: ViewController.cs a ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "dva soubory, které tvoří třídu ViewController: ViewController.cs a ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** mělo být vyplněno pomocí vlastního kódu související s `ViewController` třídy. V tomto souboru `ViewController` třídy reagovat na různé iOS metody životního cyklu kontroleru zobrazení, přizpůsobení uživatelského rozhraní a reakce na uživatelský vstup, jako je klepnutí tlačítka.

- **ViewController.designer.cs** je vygenerovaný soubor vytvořený nástrojem v iOS designeru mapování rozhraní vizuálně vytvořený kód. Protože změny tohoto souboru budou přepsány, by se nemělo upravovat. Deklarace vlastností v tomto souboru umožňují kód v `ViewController` třídy přístup, tím **název**, ovládací prvky sady nahoru v iOS designeru. Otevírání **ViewController.designer.cs** odhalí následující kód:

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

`SubmitButton` Deklarace vlastnosti se připojí celý `ViewController` třídy – ne jenom **ViewController.designer.cs** souboru – tlačítko definované ve scénáři. Protože **ViewController.cs** definuje část `ViewController` třídu, má přístup k `SubmitButton`.

Na následujícím snímku obrazovky ukazuje, že technologie IntelliSense nyní rozpoznává `SubmitButton` odkazovat v **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Technologie IntelliSense uznání odkaz tlacitkoOdeslat](introduction-images/6-submitbuttonintellisense-vsmac.png "IntelliSense uznání tlacitkoOdeslat odkaz")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Technologie IntelliSense uznání odkaz tlacitkoOdeslat](introduction-images/6-submitbuttonintellisense-vs.png "IntelliSense uznání tlacitkoOdeslat odkaz")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

Tato část vám ukázal, jak vytvořit tlačítko v iOS designeru a přistupovat k toto tlačítko v kódu.

Zbývající část tohoto dokumentu obsahuje další přehled v iOS designeru.

## <a name="ios-designer-basics"></a>Základy iOS designeru

Tato část představuje částí v iOS designeru a poskytuje přehled používání jeho funkcí.

### <a name="launching-the-ios-designer"></a>Spouští se iOS designeru

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Projekty Xamarin.iOS vytvořené pomocí sady Visual Studio pro Mac zahrnout ve scénáři. Chcete-li zobrazit obsah ve scénáři, poklikejte na soubor .storyboard v **oblasti řešení**:

[![Ve scénáři otevřete v iOS designeru](introduction-images/7-storyboardopen-vsmac.png "scénáře otevřete v iOS designeru")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Většina Xamarin.iOS projekty vytvořené pomocí sady Visual Studio 2015 nebo 2017 zahrnout ve scénáři. Chcete-li zobrazit obsah ve scénáři, poklikejte na soubor .storyboard v **Průzkumníka řešení**:

[![Ve scénáři otevřete v iOS designeru](introduction-images/7-storyboardopen-vs.png "scénáře otevřete v iOS designeru")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS Designer funkce

IOS Designer má primární šesti částí:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Oddíly v iOS designeru](introduction-images/8-sixpartsofiosdesigner-vsmac.png "oddíly v iOS designeru")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Návrhové ploše** – iOS Designer primární pracovní prostor. Zobrazit v oblasti dokumentu, umožňuje vizuální vytváření uživatelských rozhraní.
2. **Panel nástrojů pro omezení** – umožňuje přepínání mezi snímek na pozici prvky v uživatelském rozhraní úpravy režim a režim úprav omezení, dvěma různými způsoby.
3. **Panel nástrojů** – uvádí kontrolery, objekty, ovládací prvky, zobrazení dat, nástroje pro rozpoznávání gest, windows a panely je možné přetáhnout na návrhovou plochu a přidat k uživatelské rozhraní.
4. **Panel vlastnosti** – zobrazuje vlastnosti pro vybraný ovládací prvek, včetně identit, vizuální styly, dostupnost, rozložení a chování.
5. **Osnova dokumentu** – zobrazuje stromu ovládacích prvků, které tvoří rozložení pro rozhraní, který právě upravujete. Kliknutím na položky ve stromové struktuře vybere v iOS designeru a jeho vlastností v **oblasti vlastnosti**. To je užitečné pro výběr konkrétní ovládací prvek nejhlouběji vnořená uživatelské rozhraní.
6. **Dolní panel nástrojů** – obsahuje možnosti pro změnu zobrazení souboru .storyboard nebo .xib, včetně zařízení, orientace a Lupa v iOS designeru.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Oddíly v iOS designeru](introduction-images/8-sixpartsofiosdesigner-vs.png "oddíly v iOS designeru")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Návrhové ploše** – iOS Designer primární pracovní prostor. Zobrazit v oblasti dokumentu, umožňuje vizuální vytváření uživatelských rozhraní.
2. **Panel nástrojů pro omezení** – umožňuje přepínání mezi snímek na pozici prvky v uživatelském rozhraní úpravy režim a režim úprav omezení, dvěma různými způsoby.
3. **Panel nástrojů** – uvádí kontrolery, objekty, ovládací prvky, zobrazení dat, nástroje pro rozpoznávání gest, windows a panely je možné přetáhnout na návrhovou plochu a přidat k uživatelské rozhraní.
4. **Okno vlastností** – zobrazuje vlastnosti pro vybraný ovládací prvek, včetně identit, vizuální styly, dostupnost, rozložení a chování.
5. **Osnova dokumentu** – zobrazuje stromu ovládacích prvků, které tvoří rozložení pro rozhraní, který právě upravujete. Kliknutím na položky ve stromové struktuře vybere v iOS designeru a jeho vlastností v **okno vlastností**. To je užitečné pro výběr konkrétní ovládací prvek nejhlouběji vnořená uživatelské rozhraní.
6. **Dolní panel nástrojů** – obsahuje možnosti pro změnu zobrazení souboru .storyboard nebo .xib, včetně zařízení, orientace a Lupa v iOS designeru.

-----

### <a name="design-workflow"></a>Návrh pracovního postupu

#### <a name="adding-a-control-to-the-interface"></a>Přidání ovládacího prvku rozhraní

Přidání ovládacího prvku do rozhraní, přetáhněte ho **nástrojů** a umístěte jej na návrhové ploše. Při přidávání nebo umístění ovládacího prvku, zvýrazněte vertikálním a horizontálním pokyny běžně používané rozložení pozice například svisle na střed, vodorovné a okrajů:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Na návrhové ploše, zvýrazněte pokyny pro běžně používané rozložení pozice](introduction-images/9-layoutguides-vsmac.png "na návrhové ploše, zvýrazněte pokyny pro běžně používané rozložení pozice")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Na návrhové ploše, zvýrazněte pokyny pro běžně používané rozložení pozice](introduction-images/9-layoutguides-vs.png "na návrhové ploše, zvýrazněte pokyny pro běžně používané rozložení pozice")

-----

Modrá čára s koncovými body v předchozím příkladu poskytuje vodítko Vodorovné zarovnání visual usnadňující umístění tlačítka.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>Příkazy místní nabídky

Místní nabídka je k dispozici na návrhové ploše i v **Osnova dokumentu**. Tato nabídka poskytuje příkazy pro vybraný ovládací prvek a jeho nadřazeným prvkem, což je užitečné při práci s zobrazení ve vnořené hierarchie:

[![Kontextové nabídky na návrhové ploše](introduction-images/10-contextmenudesignsurface-vsmac.png "kontextovou nabídku povrchu návrhu")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Panel nástrojů pro omezení

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![Omezení nástrojů](introduction-images/11-constraintstoolbar-vsmac.png "panel nástrojů pro omezení")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Omezení nástrojů](introduction-images/11-constraintstoolbar-vs.png "panel nástrojů pro omezení")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

Panel nástrojů pro omezení bylo aktualizováno a nyní se skládá ze dvou ovládacích prvků: rámce režimu úprav / omezení editaci přepínač režimu omezení aktualizace / snímky tlačítko Aktualizovat.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Rámec režimu úprav / přepnout režim úprav omezení

V předchozích verzích nástroje v iOS designeru kliknete na pohled již vybrané na návrhové ploše přepínat mezi rámce úpravy režim a režim úprav omezení. Nyní ovládacím prvku přepínací tlačítko na panelu nástrojů omezení Přepne mezi těmito režimy úprav.

- Režim úpravy snímku:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Snímek tlačítka režimu úpravy](introduction-images/12a-frameeditingmode-vsmac.png "rámec úpravy tlačítka režimu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Snímek tlačítka režimu úpravy](introduction-images/12a-frameeditingmode-vs.png "rámec úpravy tlačítka režimu")

-----

- Režim úprav omezení:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Omezení úprav tlačítka režimu](introduction-images/12b-constrainteditingmode-vsmac.png "tlačítko Režim úprav omezení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Omezení úprav tlačítka režimu](introduction-images/12b-constrainteditingmode-vs.png "tlačítko Režim úprav omezení")

-----

#### <a name="update-constraints--update-frames-button"></a>Aktualizovat omezení / snímky tlačítko Aktualizovat

Aktualizovat omezení / aktualizaci snímků nachází tlačítko vpravo od rámce režimu úprav / přepnout režim úprav omezení.

- V rámci režimu úprav kliknete na toto tlačítko upraví snímků všechny vybrané elementy tak, aby odpovídaly jejich omezení.
- V režimu úprav omezení kliknete na toto tlačítko upraví omezení všechny vybrané elementy tak, aby odpovídala jeho snímků.

### <a name="bottom-toolbar"></a>Dolní panel nástrojů

Dolní panel nástrojů poskytuje způsob, jak vyberte zařízení, orientace a Lupa slouží k zobrazení souboru storyboard nebo .xib v iOS designeru:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolní panel nástrojů používá pro výběr zařízení a orientace povrchu návrhu](introduction-images/13-bottomtoolbar-vsmac.png "dolního panelu nástrojů používá pro výběr zařízení a orientace povrchu návrhu")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolní panel nástrojů používá pro výběr zařízení a orientace povrchu návrhu](introduction-images/13-bottomtoolbar-vs.png "dolního panelu nástrojů používá pro výběr zařízení a orientace povrchu návrhu")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Zařízení a orientace

Po rozbalení dolního panelu nástrojů zobrazí všechna zařízení, orientace a úpravy pro aktuální dokument. Kliknutím na jejich změny zobrazení na návrhové ploše. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolní panel nástrojů rozbalit a zobrazit zařízení a orientacím používaným](introduction-images/14-bottomtoolbarexpanded-vsmac.png "dolního panelu nástrojů, rozšířili i na zařízeních a orientace")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolní panel nástrojů rozbalit a zobrazit zařízení a orientacím používaným](introduction-images/14-bottomtoolbarexpanded-vs.png "dolního panelu nástrojů, rozšířili i na zařízeních a orientace")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Všimněte si, že výběrem zařízení a orientaci mění pouze jak v iOS designeru zobrazí náhled návrhu. Bez ohledu na aktuální výběr nově přidané omezení se použijí na všech zařízeních a orientace, není-li **upravit vlastnosti** tlačítka se použil k určení jinak.

Když [velikost třídy](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) jsou [povolené](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), **upravit vlastnosti** v rozšířené dolního panelu nástrojů zobrazí tlačítko.  Kliknutím **upravit vlastnosti** tlačítko zobrazí možnosti pro vytvoření rozhraní variace podle velikosti třída představovaná typem vybrané zařízení a orientaci. Vezměte v úvahu následující příklady:

- Pokud **iPhone SE** / **na výšku**, je vybrán, popover poskytne možnosti, jak vytvořit rozhraní variantu pro kompaktní šířku třída velikostí regulární výška. 
- Pokud **iPad Pro 9.7"** / **na šířku** / **zobrazení na celé obrazovce** je vybrána, bude popover poskytovat možnosti, jak vytvořit rozhraní variantu pro pravidelné šířka, pravidelné výška velikosti třídy.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolní panel nástrojů se používají ke změně rozhraní podle třídy velikosti](introduction-images/15-edittraitsbutton-vsmac.png "dolní panel nástrojů se používají ke změně rozhraní podle třídy velikosti.")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolní panel nástrojů se používají ke změně rozhraní podle třídy velikosti](introduction-images/15-edittraitsbutton-vs.png "dolní panel nástrojů se používají ke změně rozhraní podle třídy velikosti.")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Ovládací prvky zvětšení

Návrhové ploše podporuje přibližování přes několik ovládacích prvků:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Ovládací prvky zvětšení v dolní panel nástrojů](introduction-images/16-zoomcontrols-vsmac.png "ovládací prvky zvětšení v dolního panelu nástrojů")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ovládací prvky zvětšení v dolní panel nástrojů](introduction-images/16-zoomcontrols-vs.png "ovládací prvky zvětšení v dolního panelu nástrojů")

-----

Ovládací prvky zahrnují následující:

1. Přizpůsobit zobrazení
2. Oddálit
3. Přiblížit
4. Skutečná velikost (velikost v pixelech 1:1)

Tyto ovládací prvky upraveno přiblížení na návrhové ploše. Neovlivňují uživatelského rozhraní aplikace za běhu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="properties-pad"></a>Panel Vlastnosti

Použití **oblasti vlastnosti** k úpravě identity, vizuální styly, dostupnost a chování ovládacího prvku. Následující snímek obrazovky ukazuje **oblasti vlastnosti** možnosti pro tlačítko:

[![Panel vlastnosti pro tlačítko](introduction-images/17-buttonpropertiespad-vsmac.png "oblasti vlastnosti pro tlačítko")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Části panelu Vlastnosti

**Oblasti vlastnosti** skládá ze tří částí:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Okno vlastností

Použití **okno vlastností** úpravy identity, vizuální styly, dostupnost a chování ovládacího prvku. Následující snímek obrazovky ukazuje **okno vlastností** možnosti pro tlačítko:

[![V okně Vlastnosti tlačítko](introduction-images/17-buttonpropertieswindow-vs.png "okně Vlastnosti pro tlačítko")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Části okna Vlastnosti

**Okno vlastností** skládá ze tří částí:

-----

1.  **Widget** – hlavní vlastnosti ovládacího prvku, jako je název třídy, vlastnosti stylu, atd. Vlastnosti pro správu obsahu ovládacího prvku jsou obvykle umístěny zde.
2.  **Rozložení** – vlastnosti, které udržovat přehled o umístění a velikost ovládacího prvku, včetně omezení a snímků, jsou zde uvedeny.
3.  **Události** – události a obslužné rutiny událostí jsou zde uvedeny. Vhodný pro zpracování událostí, jako je například dotykového ovládání, klepněte na, přetáhněte atd. Události mohou být zpracovávány také přímo v kódu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Úprava vlastností v panelu Vlastnosti

Kromě úpravy s náhledem na návrhové ploše, podporuje úpravy vlastností v iOS designeru **oblasti vlastnosti**. Dostupných vlastností mění v závislosti na vybraný ovládací prvek, jak je znázorněno v následujících snímcích obrazovky:

[![Vlastnosti tlačítka](introduction-images/18a-buttonpropertiespad-vsmac.png "vlastnosti tlačítka")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Zobrazit vlastnosti kontroleru](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "zobrazit vlastnosti kontroleru")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Úprava vlastností v okně Vlastnosti

Kromě úpravy s náhledem na návrhové ploše, podporuje úpravy vlastností v iOS designeru **okno vlastností**. Dostupných vlastností mění v závislosti na vybraný ovládací prvek, jak je znázorněno v následujících snímcích obrazovky:

[![Vlastnosti tlačítka](introduction-images/18a-buttonpropertieswindow-vs.png "vlastnosti tlačítka")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Zobrazit vlastnosti kontroleru](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "zobrazit vlastnosti kontroleru")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Oddíl Identity oblasti vlastnosti teď zobrazují **modulu** pole. Je potřeba vyplnit v této části pouze při interoperabilitě se službou Swift třídy. Můžete zadat název modulu pro Swift třídy, které jsou namespaced.

#### <a name="default-values"></a>Výchozí hodnoty

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mnoho vlastností v **oblasti vlastnosti** zobrazit žádnou hodnotu nebo výchozí hodnotu. Nicméně kód aplikace může upravovat tyto hodnoty. **Oblasti vlastnosti** nezobrazuje hodnoty nastavené v kódu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Mnoho vlastností v **okno vlastností** zobrazit žádnou hodnotu nebo výchozí hodnotu. Nicméně kód aplikace může upravovat tyto hodnoty. **Okno vlastností** nezobrazuje hodnoty nastavené v kódu.

-----

#### <a name="event-handlers"></a>Obslužné rutiny událostí

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Chcete-li určit obslužné rutiny vlastních událostí pro různé události, použijte **události** karty **oblasti vlastnosti**. Například v následujícím snímku obrazovky `HandleClick` obsluhovala tlačítka **Touch uvnitř** události:

[![Panel vlastnosti s obslužnou rutinu události pro tlačítko nastavit](introduction-images/19-buttonpropertiespadevents-vsmac.png "oblasti vlastnosti s obslužnou rutinu události pro tlačítko")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li určit obslužné rutiny vlastních událostí pro různé události, použijte **události** karty **okno vlastností**. Například v následujícím snímku obrazovky `HandleClick` obsluhovala tlačítka **Touch uvnitř** události:

[![Okno Vlastnosti s obslužnou rutinu události pro tlačítko nastavit](introduction-images/19-buttonpropertieswindowevents-vs.png "okně Vlastnosti s obslužnou rutinu události pro tlačítko")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

Jakmile byl zadán obslužnou rutinu události, musí metoda se stejným názvem přidána do odpovídající třídu kontroleru zobrazení. V opačném případě `unrecognized selector` dojde k výjimce při klepnutí na tlačítko:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nerozpoznaný selektor výjimka](introduction-images/20-unrecognizedselector-vsmac.png "výjimku nerozpoznaný selektor")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

Všimněte si, že po obslužné rutiny události v zadané **oblasti vlastnosti**, iOS Designer se okamžitě otevřít odpovídajícím kódovým souborem a nabídky pro vložení deklarace metody. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nerozpoznaný selektor výjimka](introduction-images/20-unrecognizedselector-vs.png "výjimku nerozpoznaný selektor")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Příklad, který používá vlastní obslužné rutiny, najdete [Hello, iOS – Příručka Začínáme](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Zobrazení osnovy

IOS Designer můžete také zobrazit hierarchii rozhraní ovládacích prvků jako ohraničením. Přehled je k dispozici tak, že vyberete **Osnova dokumentu** kartu, jak je znázorněno níže:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Osnova dokumentu](introduction-images/21-buttonoutlineview-vsmac.png "osnovu dokumentu")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Osnova dokumentu](introduction-images/21-buttonoutlineview-vs.png "osnovu dokumentu")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Vybraný ovládací prvek v zobrazení osnovy stále synchronizovaná s vybraný ovládací prvek na návrhové ploše.  Tato funkce je užitečná pro výběr položky z rozhraní hluboce vnořené hierarchie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Vrátit zpět do Xcode

Je možné používat iOS Návrhář a Tvůrce rozhraní Xcode Zaměnitelně. Otevřete soubor .xib nebo scénáře v Tvůrce rozhraní Xcode, klikněte pravým tlačítkem na soubor a vyberte **otevřít v programu > Tvůrce rozhraní Xcode**, jak je znázorněno v následujícím snímku obrazovky:

[![Otevření scénáře Tvůrce rozhraní Xcode](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "otevření scénáře Tvůrce rozhraní Xcode")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Po provedení úprav v Tvůrci rozhraní Xcode, uložte soubor a vrátí do sady Visual Studio pro Mac. Změny se synchronizují do projektu Xamarin.iOS.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>podpora .xib

IOS Designer podporuje vytváření, úpravám a správě soubory .xib. Tyto jsou soubory formátu XML tohoto respresent jeden vlastní zobrazení které lze přidat do hierarchie zobrazení aplikace. Soubor .xib obvykle představuje rozhraní pro jedno zobrazení nebo obrazovku v aplikaci, že ve scénáři představuje mnoha obrazovkách a přechody mezi nimi.

Existuje mnoho názory, o kterých řešení – soubory .xib, scénáři nebo kód – je nejvhodnější pro vytváření a správa uživatelského rozhraní. Ve skutečnosti neexistuje ideální řešení a je vždy vhodné vzhledem k tomu vždycky ten nejlepší nástroj pro aktuální úlohu. Ale nutné dodat, soubory .xib jsou obecně největší účinek, když používá k reprezentování vlastní potřeby na více místech v aplikaci, jako je například buňky zobrazení tabulky vlastní zobrazení. 

Další dokumentaci ke službě pomocí soubory .xib najdete v následujících návodů:

- [Pomocí šablony .xib zobrazení](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Vytváří se vlastní TableViewCell pomocí .xib](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Vytvoření obrazovky spuštění pomocí .xib](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

Další informace týkající se použití scénářů, najdete [Úvod do scénářů](~/ios/user-interface/storyboards/index.md).

Tato a další pokyny týkající se Návrhář iOS odkazovat na používání scénáře jako standardní postup pro vytváření uživatelských rozhraní, protože většina Xamarin.iOS nové šablony projektu zadejte ve scénáři ve výchozím nastavení.

## <a name="summary"></a>Souhrn

Tato příručka poskytuje úvod do iOS Designer, popisující její funkce a nástroje, které nabízí pro vytváření překrásných uživatelských rozhraní osnovy.

## <a name="related-links"></a>Související odkazy

- [Úvod do scénářů](~/ios/user-interface/storyboards/index.md)
- [iOS navrhovatelé návod ovládacích prvků](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS s více obrazovkami](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android Designer – přehled](~/android/user-interface/android-designer/index.md)
- [Částečné třídy a metody](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Všemi zúčastněnými stranami návrháři Xamarin pro iOS – rozvíjet 2014 (video)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Použití v iOS designeru vytvoření obrazovky spuštění (video)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
