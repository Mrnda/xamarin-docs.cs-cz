---
title: "iOS základy návrháře"
description: "Tento průvodce představuje návrháře Xamarin pro iOS. Ukazuje, jak používat iOS Návrhář vizuálně Rozvrhněte ovládací prvky, jak získat přístup k tyto ovládací prvky v kódu a jak upravit vlastnosti."
ms.topic: article
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 3046d779239076098a8b2fb74fc87e2f211074e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-basics"></a>iOS základy návrháře

_Tento průvodce představuje návrháře Xamarin pro iOS. Ukazuje, jak používat iOS Návrhář vizuálně Rozvrhněte ovládací prvky, jak získat přístup k tyto ovládací prvky v kódu a jak upravit vlastnosti._

Návrhář Xamarin pro iOS je podobná Xcode na rozhraní tvůrce vizuální rozhraní návrháře a Android návrháře. Mezi její mnoho funkcí patří bezproblémovou integraci s Visual Studio pro Mac a Visual Studio 2015 a 2017, přetahování myší, rozhraní pro nastavení obslužné rutiny událostí a možnost k vykreslení vlastní ovládací prvky.

## <a name="requirements"></a>Požadavky

IOS Návrhář je k dispozici v sadě Visual Studio pro Mac a v sadě Visual Studio 2015 a 2017 v systému Windows. V sadě Visual Studio 2015 nebo 2017 iOS Návrhář vyžaduje připojení k správně nakonfigurované Mac sestavení hostitele, i když Xcode nemusí být spuštěna.

Tato příručka předpokládá znalost obsahu zahrnutého v [Začínáme provede](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Jak funguje iOS návrháře

Tato část popisuje, jak iOS Návrhář usnadňuje vytváření uživatelské rozhraní a připojení ke kódu.

IOS Designer umožňuje vývojářům vizuální návrh uživatelské rozhraní aplikace. Jak je uvedeno v [Úvod do scénářů](~/ios/user-interface/storyboards/index.md) průvodce, scénáře popisuje obrazovky (řadiče zobrazení) tvořící aplikaci, umístit na ty řadiče zobrazení a aplikace celkové navigační toku prvky rozhraní (zobrazení) . 

Řadič zobrazení má dvě části: vizuální reprezentace v iOS Designer a přidruženou třídu C#:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Řadiče zobrazení v Návrháři iOS](introduction-images/1-storyboardwithviewcontroller-vsmac.png "řadiče zobrazení v Návrháři iOS")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png)

[![Kód pro řadič zobrazení](introduction-images/2-viewcontrollercode-vsmac.png "kód pro řadič zobrazení")](introduction-images/2-viewcontrollercode-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Řadiče zobrazení v Návrháři iOS](introduction-images/1-storyboardwithviewcontroller-vs.png "řadiče zobrazení v Návrháři iOS")](introduction-images/1-storyboardwithviewcontroller-vs-large.png)

[![Kód pro řadič zobrazení](introduction-images/2-viewcontrollercode-vs.png "kód pro řadič zobrazení")](introduction-images/2-viewcontrollercode-vs-large.png)

-----

Ve svém výchozím stavu řadič zobrazení neposkytuje žádné funkce; musí být naplněn s ovládacími prvky. Tyto ovládací prvky jsou umístěny v zobrazení řadiče zobrazení obdélníkovou oblast, která obsahuje veškerý obsah na obrazovce. Většina řadičů zobrazení obsahovat běžné ovládací prvky, např. tlačítka, popisky a textová pole, jak je znázorněno na následujícím snímku obrazovky, který ukazuje řadič zobrazení obsahující tlačítka: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Řadič zobrazení obsahující tlačítko](introduction-images/3-viewcontrollerwithbutton-vsmac.png "řadič zobrazení obsahující tlačítka")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Řadič zobrazení obsahující tlačítko](introduction-images/3-viewcontrollerwithbutton-vs.png "řadič zobrazení obsahující tlačítka")](introduction-images/3-viewcontrollerwithbutton-vs-large.png)

-----

Některé ovládací prvky, jako je například popisky, který obsahuje statický text, můžete přidat do řadiče zobrazení a zbývajících samostatně. Ale častěji, ovládací prvky musí být přizpůsobit prostřednictvím kódu programu. Například tlačítko přidané v předchozím kroku by tomu něco po klepnutí obslužné rutiny události musí být přidán v kódu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby bylo možné přistupovat a manipulovat s tlačítko v kódu, musí mít jedinečný identifikátor. Zadejte jedinečný identifikátor výběrem tlačítko Otevřít **vlastnosti Pad**a nastavení jeho **název** pole na hodnotu, jako je například "Odeslat":

[![Tlačítkem na název v poli vlastnosti pro nastavení](introduction-images/4-settingbuttonname-vsmac.png "tlačítkem na název v poli vlastnosti pro nastavení")](introduction-images/4-settingbuttonname-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby bylo možné přistupovat a manipulovat s tlačítko v kódu, musí mít jedinečný identifikátor. Zadejte jedinečný identifikátor výběrem tlačítko Otevřít **vlastnosti – okno**a nastavení jeho **název** pole na hodnotu, jako je například "Odeslat":

[![Název na tlačítko nastavení v okně vlastností](introduction-images/4-settingbuttonname-vs.png "tlačítkem na název nastavení v okně vlastností")](introduction-images/4-settingbuttonname-vs-large.png)

-----

Nyní, když na tlačítko název, můžete získat přístup v kódu. Ale jak to funguje?

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V **řešení Pad**, navigační k **ViewController.cs** a kliknete na indikátor zpřístupnění zjistí, že řadiče zobrazení `ViewController` rozsahy definice třídy, dva soubory, z nichž každý obsahuje [třídu](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definice:

[![Dva soubory, které tvoří třídě ViewController: ViewController.cs a ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "dva soubory, které tvoří třídě ViewController: ViewController.cs a ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V **Průzkumníku řešení**, navigační k **ViewController.cs** a kliknete na indikátor zpřístupnění zjistí, že řadiče zobrazení `ViewController` zahrnuje dva soubory, každý z definice třídy obsahující [třídu](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) definice:

[![Dva soubory, které tvoří třídě ViewController: ViewController.cs a ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "dva soubory, které tvoří třídě ViewController: ViewController.cs a ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png)

-----

- **ViewController.cs** by měl být vyplněny vlastní kód týkající se `ViewController` třídy. V tomto souboru `ViewController` třída reagovat na různých iOS metody životního cyklu řadiče zobrazení, přizpůsobení uživatelského rozhraní a reagovat na uživatelský vstup, jako je klepne na tlačítko.

- **ViewController.designer.cs** je to generovaný soubor vytvořený iOS Designer pro mapování rozhraní vizuálně sestavený ke kódu. Protože změny tohoto souboru budou přepsány, by neměl být upraven. Vlastnost deklarace v tomto souboru umožňují kód v `ViewController` třídy na přístupu, pomocí **název**, ovládací prvky sady nahoru v iOS Designer. Otevírání **ViewController.designer.cs** zjistí následující kód:

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

`SubmitButton` Deklarace vlastnosti připojí celý `ViewController` třídy - není právě na **ViewController.designer.cs** soubor – tlačítko definované ve scénáři. Vzhledem k tomu **ViewController.cs** definuje součástí `ViewController` třídu, má přístup k `SubmitButton`.

Následující snímek obrazovky ukazuje, že technologie IntelliSense nyní rozpoznává `SubmitButton` odkaz v **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![IntelliSense rozpozná odkaz Odeslat](introduction-images/6-submitbuttonintellisense-vsmac.png "rozpozná odkaz Odeslat IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IntelliSense rozpozná odkaz Odeslat](introduction-images/6-submitbuttonintellisense-vs.png "rozpozná odkaz Odeslat IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png)

-----

V této části vám ukázal, jak vytvořit tlačítka v iOS Designer a přístup k této tlačítko v kódu.

Zbývající část tohoto dokumentu obsahuje další přehled IOS Designer.

## <a name="ios-designer-basics"></a>iOS základy návrháře

Tato část uvádí části iOS Designer a poskytuje prohlídka její funkce.

### <a name="launching-the-ios-designer"></a>Spouštění iOS návrháře

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.iOS projekty vytvořené pomocí sady Visual Studio pro Mac zahrnují scénáře. Pokud chcete zobrazit obsah scénáře, poklikejte na soubor .storyboard v **řešení Pad**:

[![Scénář otevřít v Návrháři iOS](introduction-images/7-storyboardopen-vsmac.png "scénáře otevřít v Návrháři iOS")](introduction-images/7-storyboardopen-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Většina Xamarin.iOS projekty vytvořené pomocí sady Visual Studio 2015 nebo 2017 zahrnují scénáře. Pokud chcete zobrazit obsah scénáře, poklikejte na soubor .storyboard v **Průzkumníku řešení**:

[![Scénář otevřít v Návrháři iOS](introduction-images/7-storyboardopen-vs.png "scénáře otevřít v Návrháři iOS")](introduction-images/7-storyboardopen-vs-large.png)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS Návrhář funkce

IOS Návrhář má šest primární částí:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Části IOS Návrhář](introduction-images/8-sixpartsofiosdesigner-vsmac.png "části IOS návrháře")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png)

1. **Návrh prostor** – primárním pracovním prostorem návrháře iOS. Zobrazí v oblasti dokumentu, umožňuje vizuální tvorbu uživatelského rozhraní.
2. **Panel nástrojů omezení** – umožňuje přepínání mezi rámce úpravy režim a režim úprav omezení, dvěma různými způsoby na pozici prvky v uživatelském rozhraní.
3. **Sada nástrojů** – uvádí řadiče, objekty, ovládací prvky, zobrazení dat, nástroje pro rozpoznávání gesto, windows a panely můžete přetáhnout na návrhovou plochu a přidat k uživatelské rozhraní.
4. **Vlastnosti Pad** – se zobrazují vlastnosti pro vybraný ovládací prvek, včetně identitu, vizuální styly, usnadnění, rozložení a chování.
5. **Osnova dokumentu** – zobrazuje stromu ovládacích prvků, které tvoří rozložení pro rozhraní Upravovaný. Kliknutím na položku ve stromové struktuře vybere v iOS Designer a zobrazuje jeho vlastnosti v **Pad vlastnosti**. To je užitečné, když vyberete určitý ovládací prvek v hluboko vnořené uživatelské rozhraní.
6. **Dolů panel nástrojů** – obsahuje možnosti pro změnu, jak iOS Designer zobrazí .storyboard nebo .xib souboru, včetně zařízení, orientaci a přiblížení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Části IOS Návrhář](introduction-images/8-sixpartsofiosdesigner-vs.png "části IOS návrháře")](introduction-images/8-sixpartsofiosdesigner-vs-large.png)

1. **Návrh prostor** – primárním pracovním prostorem návrháře iOS. Zobrazí v oblasti dokumentu, umožňuje vizuální tvorbu uživatelského rozhraní.
2. **Panel nástrojů omezení** – umožňuje přepínání mezi rámce úpravy režim a režim úprav omezení, dvěma různými způsoby na pozici prvky v uživatelském rozhraní.
3. **Sada nástrojů** – uvádí řadiče, objekty, ovládací prvky, zobrazení dat, nástroje pro rozpoznávání gesto, windows a panely můžete přetáhnout na návrhovou plochu a přidat k uživatelské rozhraní.
4. **Vlastnosti – okno** – se zobrazují vlastnosti pro vybraný ovládací prvek, včetně identitu, vizuální styly, usnadnění, rozložení a chování.
5. **Osnova dokumentu** – zobrazuje stromu ovládacích prvků, které tvoří rozložení pro rozhraní Upravovaný. Kliknutím na položku ve stromové struktuře vybere v iOS Designer a zobrazuje jeho vlastnosti v **vlastnosti – okno**. To je užitečné, když vyberete určitý ovládací prvek v hluboko vnořené uživatelské rozhraní.
6. **Dolů panel nástrojů** – obsahuje možnosti pro změnu, jak iOS Designer zobrazí .storyboard nebo .xib souboru, včetně zařízení, orientaci a přiblížení.

-----

### <a name="design-workflow"></a>Pracovní postup návrhu

#### <a name="adding-a-control-to-the-interface"></a>Přidání ovládacího prvku rozhraní

Přidání ovládacího prvku do rozhraní, přetáhněte jej z **sada nástrojů** na návrhovou plochu. Při přidávání nebo umístění ovládacího prvku, zvýrazněte svislého a vodorovného pokyny běžně používané rozložení pozic například svisle na střed, vodorovném centru a okrajů:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Na návrhovou plochu, zvýrazněte pokyny pro běžně používané rozložení pozic](introduction-images/9-layoutguides-vsmac.png "na návrhovou plochu, zvýrazněte pokyny pro běžně používané rozložení pozice")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Na návrhovou plochu, zvýrazněte pokyny pro běžně používané rozložení pozic](introduction-images/9-layoutguides-vs.png "na návrhovou plochu, zvýrazněte pokyny pro běžně používané rozložení pozice")

-----

Modré tečkovaná čára v předchozím příkladu poskytuje vodítko visual zarovnání vodorovném centru usnadní umístění tlačítka.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>Příkazy nabídky kontextu

Je k dispozici na návrhovou plochu a v Kontextová nabídka **Osnova dokumentu**. Tato nabídka poskytuje příkazy pro vybraný ovládací prvek a jeho nadřazeným prvkem, což je užitečné, pokud práce se zobrazeními v vnořené hierarchii:

[![V místní nabídce na návrhovou plochu](introduction-images/10-contextmenudesignsurface-vsmac.png "v místní nabídce na návrhovou plochu")](introduction-images/10-contextmenudesignsurface-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Omezení panelu nástrojů

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![Panel nástrojů omezení](introduction-images/11-constraintstoolbar-vsmac.png "omezení panelu nástrojů")](introduction-images/11-constraintstoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Panel nástrojů omezení](introduction-images/11-constraintstoolbar-vs.png "omezení panelu nástrojů")](introduction-images/11-constraintstoolbar-vs-large.png)

-----

Panel nástrojů omezení byl aktualizován a nyní se skládá ze dvou ovládacích prvků: rámečku režimu úprav / omezení úpravy přepnutí režimu a omezení aktualizace / rámce tlačítko Aktualizovat.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Rámce režimu úprav / omezení úpravy přepnutí režimu

V předchozích verzích systému iOS Designer kliknutím na zobrazení o již vybrána na návrhovou plochu přepínat stav mezi rámce úpravy režim a režim úprav omezení. Nyní ovládacího prvku přepnutí na panelu nástrojů omezení Přepne mezi tyto úpravy v režimu.

- Rámce režimu úprav:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Rámce úpravy tlačítka režimu](introduction-images/12a-frameeditingmode-vsmac.png "úpravy tlačítka režimu s rámečkem")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Rámce úpravy tlačítka režimu](introduction-images/12a-frameeditingmode-vs.png "úpravy tlačítka režimu s rámečkem")

-----

- Úpravy režim omezení:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Omezení úpravy tlačítka režimu](introduction-images/12b-constrainteditingmode-vsmac.png "tlačítko režimu úprav omezení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Omezení úpravy tlačítka režimu](introduction-images/12b-constrainteditingmode-vs.png "tlačítko režimu úprav omezení")

-----

#### <a name="update-constraints--update-frames-button"></a>Aktualizovat omezení / aktualizace tlačítko rámce

Omezení aktualizace / aktualizace rámců tlačítko nachází vpravo od rámečku režimu úprav / omezení úpravy přepnutí režimu.

- V rámci režimu úprav upraví se kliknutím na toto tlačítko snímky všechny vybrané elementy tak, aby odpovídaly jejich omezení.
- V režimu úprav omezení upraví se kliknutím na toto tlačítko omezení všechny vybrané elementy tak, aby odpovídaly jejich rámce.

### <a name="bottom-toolbar"></a>Dolním panelu nástrojů

Spodním panelu nástrojů poskytuje způsob, jak vyberte zařízení, orientaci a přiblížení či oddálení použít k zobrazení souboru storyboard nebo .xib v iOS Designer:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dolním panelu nástrojů použit k výběru zařízení a orientaci pro návrhovou plochu](introduction-images/13-bottomtoolbar-vsmac.png "dolním panelu nástrojů použit k výběru zařízení a orientaci pro návrhové plochy")](introduction-images/13-bottomtoolbar-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dolním panelu nástrojů použit k výběru zařízení a orientaci pro návrhovou plochu](introduction-images/13-bottomtoolbar-vs.png "dolním panelu nástrojů použit k výběru zařízení a orientaci pro návrhové plochy")](introduction-images/13-bottomtoolbar-vs-large.png)

-----

#### <a name="device-and-orientation"></a>Zařízení a orientace

Po rozbalení spodním panelu nástrojů zobrazí všechna zařízení, orientace nebo úpravy vztahující se na aktuálním dokumentu. Klepnutím na jejich změníte zobrazení zobrazí na návrhovou plochu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Spodním panelu nástrojů rozbalit a zobrazit zařízení a orientace](introduction-images/14-bottomtoolbarexpanded-vsmac.png "dolním panelu nástrojů rozbalit a zobrazit zařízení a orientace")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Spodním panelu nástrojů rozbalit a zobrazit zařízení a orientace](introduction-images/14-bottomtoolbarexpanded-vs.png "dolním panelu nástrojů rozbalit a zobrazit zařízení a orientace")](introduction-images/14-bottomtoolbarexpanded-vs-large.png)

-----

Upozorňujeme, že výběrem zařízení a orientaci změní jenom jak iOS Návrhář přináší náhled návrhu. Bez ohledu na aktuální výběr nově přidaná omezení se použijí ve všech zařízeních a orientace, pokud **upravit vlastnosti** tlačítko jsou využívány k určení jinak.

Když [velikost třídy](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) jsou [povoleno](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), **upravit vlastnosti** tlačítko se zobrazí v rozšířené spodním panelu nástrojů.  Kliknutím **upravit vlastnosti** tlačítko zobrazí možnosti pro vytvoření variace rozhraní založené na třídě velikost reprezentována na vybrané zařízení a orientaci. Vezměte v úvahu následující příklady:

- Pokud **iPhone SE** / **na výšku**, je vybrána, popover zajistí možnosti, jak vytvořit variace rozhraní k šířce compact regulární výšku velikosti třídy. 
- Pokud **iPad Pro 9.7"** / **na šířku** / **celé obrazovky** je vybrána, popover poskytuje možnosti pro vytvoření variace rozhraní pro regulární šířka, regulární výšku velikosti třídy.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Spodním panelu nástrojů se používají ke změně rozhraní třídou velikost](introduction-images/15-edittraitsbutton-vsmac.png "dolním panelu nástrojů používá k odlišení rozhraní velikost – třída")](introduction-images/15-edittraitsbutton-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Spodním panelu nástrojů se používají ke změně rozhraní třídou velikost](introduction-images/15-edittraitsbutton-vs.png "dolním panelu nástrojů používá k odlišení rozhraní velikost – třída")](introduction-images/15-edittraitsbutton-vs-large.png)

-----

#### <a name="zoom-controls"></a>Ovládací prvky přiblížení

Návrhovou plochu, která podporuje přiblížení a oddálení prostřednictvím několika ovládacích prvků:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![Ovládací prvky zvětšení v dolním panelu nástrojů](introduction-images/16-zoomcontrols-vsmac.png "ovládacích prvků zvětšení v dolním panelu nástrojů")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ovládací prvky zvětšení v dolním panelu nástrojů](introduction-images/16-zoomcontrols-vs.png "ovládacích prvků zvětšení v dolním panelu nástrojů")

-----

Ovládací prvky zahrnují následující:

1. Přiblížení podle
2. Oddálit
3. Přiblížit
4. Skutečná velikost (1:1 pixel velikost)

Tyto ovládací prvky upravit přiblížení na návrhovou plochu. Neovlivňují uživatelské rozhraní aplikace za běhu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="properties-pad"></a>Vlastnosti odsazení

Použití **vlastnosti Pad** úpravy identity, vizuální styly, usnadnění a chování ovládacího prvku. Následující snímek obrazovky ukazuje **vlastnosti Pad** možnosti pro tlačítko:

[![Odsazení vlastnosti pro tlačítko](introduction-images/17-buttonpropertiespad-vsmac.png "The Pad vlastnosti pro tlačítko")](introduction-images/17-buttonpropertiespad-vsmac-large.png)
#### <a name="properties-pad-sections"></a>Vlastnosti Pad části

**Vlastnosti Pad** skládá ze tří částí:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Okno vlastností

Použití **vlastnosti – okno** upravit identity, vizuální styly, usnadnění a chování ovládacího prvku. Následující snímek obrazovky ukazuje **vlastnosti – okno** možnosti pro tlačítko:

[![Okno vlastností pro tlačítko](introduction-images/17-buttonpropertieswindow-vs.png "okně Vlastnosti pro tlačítko")](introduction-images/17-buttonpropertieswindow-vs-large.png)

#### <a name="properties-window-sections"></a>Vlastnosti – okno oddíly

**Vlastnosti – okno** skládá ze tří částí:

-----

1.  **Pomůcka** – hlavní vlastnosti ovládacího prvku, jako je například název třídy, vlastnosti stylu, atd. Zde jsou obvykle umístěny vlastnosti pro správu obsahu ovládacího prvku.
2.  **Rozložení** – vlastnosti, které udržování přehledu o umístění a velikost ovládacího prvku, včetně omezení a rámce, jsou zde uvedeny.
3.  **Události** – zde zadané události a obslužné rutiny událostí. Tato možnost je užitečná pro zpracování událostí například dotykového ovládání, klepněte na, přetáhněte atd. Události mohou být zpracovány také přímo v kódu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Úprava vlastností v panelu pro vlastnosti

Kromě úpravy s náhledem na návrhovou plochu, podporuje iOS Návrhář Úprava vlastností v **Pad vlastnosti**. Změna dostupné vlastnosti založené na vybraný ovládací prvek, které jsou popsány v následujících snímcích obrazovky:

[![Tlačítko Vlastnosti](introduction-images/18a-buttonpropertiespad-vsmac.png "tlačítko Vlastnosti")](introduction-images/18a-buttonpropertiespad-vsmac-large.png)

[![Zobrazit vlastnosti řadiče](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "zobrazit vlastnosti řadiče")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Úprava vlastností v okně vlastností

Kromě úpravy s náhledem na návrhovou plochu, podporuje iOS Návrhář Úprava vlastností v **vlastnosti – okno**. Změna dostupné vlastnosti založené na vybraný ovládací prvek, které jsou popsány v následujících snímcích obrazovky:

[![Tlačítko Vlastnosti](introduction-images/18a-buttonpropertieswindow-vs.png "tlačítko Vlastnosti")](introduction-images/18a-buttonpropertieswindow-vs-large.png)

[![Zobrazit vlastnosti řadiče](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "zobrazit vlastnosti řadiče")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png)

-----

> [!IMPORTANT]
> Části Identity ukazuje nyní vlastnosti odsazení **modulu** pole. Je nutné vyplnit v této části, pouze při interoperabilitě se službou Swift třídy. Umožňuje vám zadat název modulu pro Swift třídy, které jsou namespaced.

#### <a name="default-values"></a>Výchozí hodnoty

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mnoho vlastností v **vlastnosti Pad** zobrazit žádnou hodnotu nebo výchozí hodnotu. Nicméně může kód aplikace upravovat tyto hodnoty. **Vlastnosti Pad** nezobrazuje hodnotami nastavenými v kódu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Mnoho vlastností v **vlastnosti – okno** zobrazit žádnou hodnotu nebo výchozí hodnotu. Nicméně může kód aplikace upravovat tyto hodnoty. **Vlastnosti – okno** nezobrazuje hodnotami nastavenými v kódu.

-----

#### <a name="event-handlers"></a>Obslužné rutiny událostí

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud chcete zadat vlastní obslužné rutiny pro různé události, použijte **události** kartě **Pad vlastnosti**. Například na tomto snímku obrazovky `HandleClick` metoda zpracovává na tlačítko **Touch uvnitř** událostí:

[![Panelu Vlastnosti pro obslužnou rutinu pro tlačítko nastavit](introduction-images/19-buttonpropertiespadevents-vsmac.png "The Pad vlastnosti obslužnou rutinu nastavit pro tlačítko")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud chcete zadat vlastní obslužné rutiny pro různé události, použijte **události** kartě **vlastnosti – okno**. Například na tomto snímku obrazovky `HandleClick` metoda zpracovává na tlačítko **Touch uvnitř** událostí:

[![Okně vlastností obslužnou rutinu pro tlačítko nastavit](introduction-images/19-buttonpropertieswindowevents-vs.png "okně vlastností obslužnou rutinu nastavit pro tlačítko")](introduction-images/19-buttonpropertieswindowevents-vs-large.png)

-----

Jakmile obslužné rutiny události byl zadán, musí být metoda se stejným názvem přidána do odpovídající třídy kontroleru zobrazení. Jinak `unrecognized selector` výjimka se stane, když je stisknuté tlačítko:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Výjimku nerozpoznané selektor](introduction-images/20-unrecognizedselector-vsmac.png "výjimku nerozpoznané selektor")](introduction-images/20-unrecognizedselector-vsmac-large.png)

Všimněte si, že po obslužné rutiny události byl v zadaný **vlastnosti Pad**, iOS Designer okamžitě otevřete odpovídající soubor kódu a nabízet vložit deklarace metody. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Výjimku nerozpoznané selektor](introduction-images/20-unrecognizedselector-vs.png "výjimku nerozpoznané selektor")](introduction-images/20-unrecognizedselector-vs-large.png)

-----

Příklad, který používá vlastní obslužné rutiny, najdete v části [Hello, iOS – Příručka Začínáme](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Zobrazení osnovy

IOS Designer lze také zobrazit hierarchii rozhraní ovládacích prvků jako přehled. Obrys je k dispozici výběrem **Osnova dokumentu** kartě, jak je uvedeno níže:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Osnova dokumentu](introduction-images/21-buttonoutlineview-vsmac.png "Osnova dokumentu")](introduction-images/21-buttonoutlineview-vsmac-large.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Osnova dokumentu](introduction-images/21-buttonoutlineview-vs.png "Osnova dokumentu")](introduction-images/21-buttonoutlineview-vs-large.png)

-----

Vybraný ovládací prvek v zobrazení osnovy zůstává synchronizována s vybraný ovládací prvek na návrhovou plochu.  Tato funkce je užitečná pro výběrem položky z hierarchie hluboko vložené rozhraní.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Vrátit se do Xcode

Je možné používat iOS Designer a Xcode rozhraní tvůrce zcela zaměnitelným významem. Otevřete storyboard nebo soubor .xib v Xcode rozhraní tvůrce, klikněte pravým tlačítkem na soubor a vyberte **otevřete s > Xcode rozhraní tvůrce**, které jsou popsány v následující snímek obrazovky:

[![Otevření scénáře v Xcode rozhraní tvůrce](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "otevírání scénáře v Tvůrci rozhraní Xcode")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png)

Po provedení úpravy v Xcode rozhraní tvůrce, uložte soubor a vrátit k sadě Visual Studio for Mac. Změny se budou synchronizovat do projektu Xamarin.iOS.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>podpora .xib

IOS Návrhář podporuje vytváření, úpravám a správě .xib soubory. Tyto jsou soubory formátu XML této hodnota představovat jeden, vlastní zobrazení, které lze přidat do aplikace zobrazení hierarchie. Soubor .xib obvykle představuje rozhraní pro jednoho zobrazení nebo obrazovky v aplikaci, zatímco scénáře představuje mnoho obrazovky a přechody mezi nimi.

Existuje mnoho názory, o kterých řešení – soubory .xib, scénářů nebo kód – funguje nejlépe pro vytváření a údržbu uživatelské rozhraní. Ve skutečnosti neexistuje ideální řešení a je vždy vhodné zvažování nejlepší nástroj pro úlohu po ruce. Ale nutné dodat, .xib soubory jsou obecně nejúčinnější, když se používá k reprezentování vlastní zobrazení potřeby na více místech v aplikaci, například buňce zobrazení vlastní tabulky. 

Další dokumentaci o použití .xib souborů naleznete v následujících recepty:

- [Pomocí šablony .xib zobrazení](https://developer.xamarin.com/recipes/ios/general/templates/using_the_ios_view_xib_template/)
- [Vytváření vlastní TableViewCell, použití .xib](https://developer.xamarin.com/recipes/ios/content_controls/tables/custom-tableviewcell/)
- [Vytvoření obrazovky spustit pomocí .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)

Další informace týkající se použití scénářů, najdete v části [Úvod do scénářů](~/ios/user-interface/storyboards/index.md).

Tato a další iOS související Návrhář příručky naleznete s použitím scénářů jako standardní přístup pro vytváření uživatelského rozhraní, protože ve výchozím nastavení většina Xamarin.iOS nové šablony projektu poskytují scénáře.

## <a name="summary"></a>Souhrn

Tato příručka poskytuje úvod do systému iOS Návrhář popisující její funkce a osnovy nástroje, které nabízí pro návrh Krásný uživatelského rozhraní.

## <a name="related-links"></a>Související odkazy

- [Úvod do scénářů](~/ios/user-interface/storyboards/index.md)
- [iOS navrhovatelé návod ovládací prvky](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS s více obrazovkami](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android návrháře přehled](~/android/user-interface/android-designer/index.md)
- [Částečné třídy a metody](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Začnete návrháře Xamarin pro iOS – momentální 2014 (video)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Použití návrháře iOS k vytvoření obrazovky spuštění (video)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
