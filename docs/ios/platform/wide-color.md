---
title: Široké barev v Xamarin.iOS
description: Tento dokument popisuje široké barvy a jeho použití v aplikaci Xamarin.iOS nebo Xamarin.Mac. Také poskytuje podrobný přehled mnoho důležité koncepty související barva například barevné prostory, kanálů a základní barvy.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 173919e0d5feda6ab7d34895cc834c5f36d737a8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788778"
---
# <a name="wide-color-in-xamarinios"></a>Široké barev v Xamarin.iOS

_Tento článek se zabývá široké barvy a jeho použití v aplikaci Xamarin.iOS nebo Xamarin.Mac._

iOS 10 a systému macOS Sierra vylepšuje podporu pro rozšířené rozsah pixelů a prostory celou rozsah, barvy v celém systému, včetně architektury, jako je například základní grafické prvky, základní Image, operačního systému a AVFoundation. Podpora pro zařízení s wide barev zobrazí se další opatřeny náběhem / tím, že poskytuje toto chování v rámci celého grafiky zásobníku.

## <a name="about-wide-color"></a>O široké barev

Jak jsme uvedli výše, vylepšuje podporu pro rozšířené rozsah pixelů a prostory celou rozsah, barvy v rámci systému, včetně architektury, jako je například základní grafické prvky, základní Image, operačního systému a AVFoundation iOS 10 a systému macOS Sierra. Podpora pro zařízení s wide barev zobrazí se další opatřeny náběhem / tím, že poskytuje toto chování v rámci celého grafiky zásobníku.

V 90's Apple vytvořili ColorSync pro zpracování barva zpracování na Mac. Budou také pomohl najít mezinárodní Color Consortium (ICC) k vytvoření a podporovat sadu standardy pro zpracování barev v počítači s hardwarem. Společnosti Apple pracují s ICC byl součástí ColorSync a byl součástí základní OS X (nazývané teď systému macOS).

Apple byl také prvkem technologie zobrazení se hardwaru, jako jsou zobrazit sítnice, nové zobrazení P3 a zobrazit P3 barevný prostor (vydané v iMac v 2015) a TrueTone zobrazí v specialisté iPad a iPhone 7 iPhone 7 Plus.

Široké barev v systému macOS Sierra a iOS 10 Apple mění způsob, kterým systému macOS a iOS zpracovávají barvu, která plně využít výhod těchto nové technologie zobrazení.

## <a name="core-color-concepts"></a>Základní koncepty barev

Následující základní koncepty barva musí být uvedeny před přepnutím hlubší pohled na barvu v systému macOS a iOS:

### <a name="color-space"></a>Barevný prostor

Barevný prostor je prostředí, ve kterém můžete reprezentované barvy a porovnání. Může být jeden až čtyři dimenzí místa, které je definované intenzitou jeho součástí barev. 

[![](wide-color-images/color00.png "Barevný prostor")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>Kanály barev

Barva součásti můžete také označovány jako kanály barev. Některé známé reprezentace bude prostory RGB, šedá prostory, prostory CMYK nebo nezávislé prostory zařízení. 

[![](wide-color-images/color02.png "Barva součásti lze také odkazovat na jako kanály barev")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>Základní barva barvy

Základní barva barvy zadejte souřadnicový systém, který se používá k porovnání a výpočetní barvy. Základní barva barvy obvykle spadají nejvíce intenzivního verzi daného barvu, která může být generována v rámci kanálu barev.

[![](wide-color-images/color01.png "Zadejte základní barva barvy souřadnicový systém, který se používá k porovnání a výpočetní barvy")](wide-color-images/color01.png#lightbox)

V případě RGB barevný prostor reprezentované výše, jsou základní barvy barva where `1.0` jsou ukotvené souřadnice (například `[1.0, 0.0, 0.0]` pro red).

### <a name="color-gamut"></a>Barevný rozsah

Barevný rozsah se rozumí všechny barev, které může být definováno jako kombinaci jednotlivých barev kanálů v rámci udělení barevný prostor.

[![](wide-color-images/color03.png "Příklad barvu barevného rozsahu")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>Co je široké barva

Před pokrývajících tématu široké barvy, se i diskuzi o aktuální oborový standard pro barvu, standardní barevný prostor RGB (sRGB). Je nejčastěji používané barevný prostor v oblasti výpočetních dnes a je výchozí barevný prostor pro iOS a systému macOS.

Barevný prostor sRGB má následující vlastnosti:

- Je založena na standardu ITU-R BT.709.
- Má přibližnou gama 2.2.
- Reprezentuje typické osvětlení (D65).

Vzhledem k tomu, že je sRGB tak často že používané v odvětví, vývojář provádět některé předpoklady, barvu uvedené věrně reprezentované na libovolném zařízení, které se zobrazí na. To však vždy nemusí být tento případ. Kromě toho jsou různých barev, které se nehodí do sRGB barevný prostor a pro ně, není možné vyjádřit v ní.

Například mnoho textilu jsou navrženy s vlákny pomocí mnoha barvy a barvy na návratem mimo sRGB. Také mnoho produkty, které uživatel zaznamená ve své každodenní životnosti vytvořené mají jasné, ostré barvy, které spadají mimo sRGB barevný prostor. Některé z nejvíc poutavé příklady barev, které nelze vyjádřit v sRGB věci ve své podstatě jako sunsets, podzimním nechá, exotické květy a tropické vod.

Uživatelé, kteří mají byla zaznamenávání digitální bitové kopie v NEZPRACOVANÉM formátu může mít bitové kopie na jejich zařízení, která obsahují všechny tato barva data, i když to není možné vyjádřit správně v sRGB barevný prostor a pro ně nelze zobrazit správně na obrazovce.

### <a name="the-display-p3-color-space"></a>Zobrazení P3 barevný prostor

V 2015 Apple vydala nové produkty (iMac a iPad Pro 9.7"), které poskytují nové zobrazení P3. barevný prostor pro zpracování problémy vytvořené sRGB barevný prostor.

[![](wide-color-images/color04.png "Nové zobrazení P3 barevný prostor")](wide-color-images/color04.png#lightbox)

Zobrazení P3 barevný prostor má následující vlastnosti:

- Podporuje širokou barevný prostor pro moderní hardwarových platforem.
- Založené na standardu SMPTE DCI-P3. DCI P3 byl navržený pro digitální projektory ale Apple pro podporu monitorování, byla změněna.
- Má přibližnou gama 2.2.
- Reprezentuje typické osvětlení (D65).

Podle společnosti Apple jsou uživatelé přesunutí své pracovní postupy pro jejich mobilní platformy. Řešení problémů prezentace barvu, předložený sRGB v odborníky v oblasti mobilních zařízení (iPad specialisté), vyžaduje více než jen včetně široké barevné zobrazení. Jedno z řešení se k upgradu kalibrace objekt pro vytváření, takže kalibrován konkrétní zařízení na zajištění objekt pro vytváření, ze zařízení na zařízení, barevné zobrazení byla přesná a konzistentní.

Jiné řešení, je zahrnutí správy úplné a systémové barev, který Apple má součástí iOS 10 a systému macOS Sierra. 

### <a name="the-extended-range-srgb-color-space"></a>Rozsah Extended sRGB barevný prostor

Nové správy barvu celého systému iOS 10 je účet pro všechny stávající aplikace iOS, které jsou vytvořené a upřesnění pro sRGB. Byla navržena k zajištění, aby ho nebylo vliv buď barva reprezentace nebo aplikace výkon tyto existující aplikace.

Pro zpracování této situace je k dispozici Apple Extended rozsah sRGB barevný prostor v iOS 10 (a také macOS Sierra).

Rozsah Extended sRGB barevný prostor má následující vlastnosti:

- Obsahuje všechny stejné sRGB základní barvy.
- Má přibližnou gama 2.2.
- Reprezentuje typické osvětlení (D65).
- Podporuje záporné hodnoty a hodnoty větší než jedna (1).

Při povolení pro hodnoty menší než nula a větší než 1, sRGB Extended rozsah, barevný prostor umožňuje nejenom pro existující aplikace k dispozici barev v sRGB bez přístupů výkonu nebo narušení, ale umožňuje barevný prostor k reprezentaci všech Barva uvnitř viditelných spektrum. Všechny tyto dosahuje přitom stejné bodů ukotvení jako sRGB barevný prostor.

### <a name="extended-range-srgb-in-action"></a>Rozšířené sRGB rozsah v akci

Pokud chcete zobrazit, jak fungují hodnoty mimo nula a jeden v rozsahu Extended sRGB barevný prostor, proveďte na následující příklad nejvíce nasycených červené k dispozici v zobrazení P3 barevný prostor:

[![](wide-color-images/color05.png "Jak fungují hodnoty mimo nula a jeden v rozsahu Extended sRGB barevný prostor")](wide-color-images/color05.png#lightbox)

V zobrazení P3, bude tato barva reprezentováno `[1.0, 0.0, 0.0]` a v rozsahu Extended sRGB by bylo `[1.358, -0.074, -0.012]`. Protože sRGB hodnoty jsou úplné obsažené v rámci P3 zobrazení a zobrazení P3 hodnoty leží "mimo" sRGB rozsahy.

Pro fyzický hardware, který umožňuje pixelů hodnoty přechod od velmi pozitivní k extrémně záporné hodnoty ho můžete zobrazit všechny dostupné ve viditelném spektru barvy a tyto hodnoty mohou být v rozsahu Extended sRGB barevný prostor reprezentována.

### <a name="device-pixel-formats"></a>Formáty pixelů zařízení 

SRGB barevný prostor byla z velké části standardizovat na formátu 8bitové pixelů, protože 8 bitů na barvu kanál je ve většině případů dostatečná k popisu barev v sRGB. Toto není ideální, ale dostatečně funkční a nabízí dobrý kompromis mezi využití paměti a procesoru pro zobrazení obrázků.

Protože P3 zobrazení může představovat souřadnice barev mimo barevný prostor sRGB, vyžaduje 16 bitů na barvu kanál, aby správně reprezentoval barvy s rozšířenou rozsah sRGB barevný prostor.

## <a name="system-wide-wide-color-support"></a>Podpora systémové široké barev

Pro zajištění plné podpory široké barvy a široký rozsah uvnitř iOS 10 a systému macOS Sierra, Apple má rozšířené tyto architektury všech výhod sRGB Extended rozsah barevný prostor a P3 zobrazení:

- UIKit (pro jenom iOS)
- SceneKit
- Základní grafiky
- ImageIO
- Základní Image
- WebKit
- SpriteKit
- Základní animace
- AppKit (pro pouze v systému macOS)

Kromě toho sítnice zobrazení je vylepšená podpora pro rozšířené rozsah sRGB barevný prostor a P3 zobrazení zobrazí.

Široké barvu je podporována a mohou být používány následující typy obsahu aplikací:

- Statický obrázek prostředky součástí sady prostředků aplikace.
- Dokument a síť na základě prostředky obrázků.
- Pokročilé média, například Live fotografie nebo obrázky v NEZPRACOVANÉM formátu.
- 3D grafický shaderu texture bitové kopie.

## <a name="solving-the-color-problem"></a>Řešení problému s barvou

Obsah se zobrazuje v aplikaci mohou pocházet z široké škály barev bohaté zdrojů. Kromě toho lze na širokou škálu zařízení, každou s vlastní řadu funkcí, barva zobrazení zobrazit tento obsah.

Aplikace pro iOS 10 přemosťuje rozdíl mezi tyto dva problémy pomocí nového předdefinované, systémové barva Správa systému. Tento systém zajistí, že bitovou kopii, vypadá stejně na jakékoli zařízení s iOS, bez ohledu na to, které barevný prostor byl v kódování bitovou kopii.

Správa barev začíná každé bitové kopie s přidružené barevný prostor (nebo profil barev). Tyto informace se používají v _barvu odpovídající proces_ kde budou barev v zdrojové bitové kopie odpovídat barev v výstupní zařízení. Vzhledem k tomu, že každý pixel na obrázku musí být shodná barvu, může být časově náročná a způsobit přetížení při procesoru v zařízení.

Vzhledem k povaze _barvu odpovídající proces_, může být tento převod potenciálně "míru ztrát" Pokud výstupní zařízení má menší barevného rozsahu, než se zdrojovou bitovou kopií.

Naštěstí výpočtů, které patří do _barvu odpovídající procesu_ lze snadno hardwaru urychlit (buď na procesoru nebo GPU) a Apple zajišťuje ji automaticky funguje tak, že vytváření podpory do základní systémů, jako jsou křemenný 2D ColorSync a základní animace. Pro správně s příznakem obsah bez kódování mohli využít těchto funkcí.

Správa barev byl podporován na jednotlivých platformách následujícím způsobem:

- **systému macOS** -systému macOS byl barva spravované od zahájení.
- **iOS** -iOS obsahoval podporované Automatická barva správy od sady iOS 9.3 (nebo novější).

### <a name="designing-for-wide-gamut"></a>Navrhování pro široký rozsah

Společnost Apple má následující návrhy pro návrh a pomocí široké barvu barevného široké rozsahu obsahu bitové kopie v aplikacích pro iOS a systému macOS:

- Použít široký rozsah obsahu pouze v zkontrolujte smysl v aplikaci, budou automaticky nepoužívejte everywhere.
- Používejte pouze obsah široký rozsah ostré barvy, kde může zvýšit činnost koncového uživatele.
- Není nutné změnit veškerý obsah na P3 pro existující aplikace.

Sada nástrojů společnosti Apple umožňuje podporu pro obsah image široký rozsah postupné souhlas, tak podpora široké barev v aplikaci není vše nebo nic situace.

### <a name="upgrading-existing-content-to-wide-color"></a>Upgrade stávajícího obsahu na široké barev

Společnost Apple má následující návrhy pro upgrade stávajícího obsahu bitové kopie na široké barev:

- Právě "nepřiřazovat" profil P3 k obsahu v bitové kopii úpravy aplikace. Díky tomu bude jednoduše přemapovat barvu existující obsah do nového barevný prostor s neočekávaným výsledkům, jako je například roztažení barvy a nevejde se do nového prostoru a změnit tak bitovou kopii.
- Obsah image bude nutné "převést" na profil P3 zobrazení pomocí aplikace pro úpravu obrázků.

### <a name="file-formats-and-color-profiles"></a>Formáty souborů a profilů barev

Společnost Apple má následující návrhy pro formáty souborů a profilů barev použít v aplikace široké Barva obrázku obsah:

- Použijte profil "Zobrazení P3" Barva RGB pracovní prostory.
- Použijte 16bitové za režim kanálu barvy.
- Použít iMac opožděno 2015 (nebo novější) přesně náhled obsahu bitové kopie.
- Obrázek prostředky exportujte jako soubory PNG 16 bitů s vloženým "Zobrazení P3" ICC profilem.

> [!IMPORTANT]
> Pomocí **uložit pro Web** nebo **exportovat prostředky** funkce nalézt v nejoblíbenější image úpravy softwaru _nikoli_ pracovní pro celou barev obrázků vzhledem k tomu, že tyto funkce nebyly aktualizovat o podporu specifikace formátu požadovaný soubor ještě.

### <a name="supporting-wide-color-with-asset-catalogs"></a>Podpora široké barvy s Asset katalogů

V systému macOS Sierra a iOS 10 se rozšířila Apple Asset katalogů umožňuje zahrnout a kategorizace statický obrázek obsah aplikace sady pro podporu široké barev.

Pomocí Asset katalogů poskytovat následující výhody pro aplikaci:

- Poskytují nejlepší volbou nasazení pro prostředky statický obrázek.
- Podporují opravy automatické barev.
- Podporují optimalizace formátu automatické pixelů.
- Podporují aplikace segmentování a zúžení aplikace, které zajišťuje, že pouze obsah, který je relevantní get doručen do zařízení koncového uživatele.

Apple udělal následující vylepšení Asset katalogů pro podporu široké barev:

- Podporují 16bitové (podle barev kanál) zdroje obsahu.
- Pomocí zobrazení rozsah podporují vytváření katalogu urychlen obsah. Obsah lze označit pro sRGB nebo P3 zobrazení rozsahů.

Vývojář má tři možnosti pro podporu široké barva obsah ve svých aplikacích:

1. **Nedělat nic** – od široké barevného obsahu musí být použit pouze v situacích, kde aplikace potřebuje k zobrazení ostré barvy (kde se bude rozšířené možnosti pro uživatele), obsah i mimo tento požadavek by měl být ponecháno-je. Bude pokračovat k vykreslení správně ve všech situacích hardwaru.
2. **Upgrade stávajícího obsahu na zobrazení P3** – to vyžaduje, aby vývojáři nahradit existující obsah image v jejich katalog Asset nový upgradovaný soubor ve formátu P3 a označit ji jako takový v editoru Asset. V čase vytvoření buildu se budou generovat bitovou kopii sRGB odvozené z těchto prostředků.
3. **Zadejte optimalizované Asset obsahu** – v takovém případě bude poskytovat vývojáře 8bitové sRGB a vize zobrazení P3 16bitové každé bitové kopie v katalogu Asset (s příznakem správně v editoru asset).

### <a name="asset-catalog-deployment"></a>Nasazení katalog Asset

Následující se stane, když vývojáři odešle aplikace k obchodu s aplikacemi s Asset katalogů, které zahrnují široké Barva obsahu bitové kopie:

- Při nasazení aplikace pro koncového uživatele řezů aplikace bude zajištěno, že pouze příslušné obsahu variant doručuje do zařízení uživatele.
- Na zařízení, které nepodporují široké barvu je bez nákladů datové části pro včetně široké barva obsah, jako je nikdy odeslaná do zařízení.
- `NSImage` v systému macOS Sierra (a novější) automaticky vybere nejlepší reprezentace obsahu pro zobrazení na hardware.
- Zobrazit obsah se aktualizuje automaticky, když nebo hardwarové zařízení zobrazit změny vlastností.

### <a name="asset-catalog-storage"></a>Asset Catalog úložiště

Úložiště katalog Asset má následující vlastnosti a důsledky pro aplikaci:

- V čase vytvoření buildu Apple pokusí optimalizaci úložiště bitové kopie obsahu prostřednictvím převody vysoké kvality obrázku.
- 16 bitů se používají na barvu kanál pro celou barva obsah.
- Komprese obsahu závislé bitové kopie slouží k nižší dodávky velikosti obsahu. Byly přidány nové "míru ztrát" komprimace optimalizovat velikosti obsahu.

## <a name="colors-in-user-interfaces"></a>Barvy v uživatelská rozhraní

Při práci s barvy v uživatelském rozhraní, většina pixelů na obrazovce jsou v plnou barvou. Kromě toho většina těchto pixelů nemáte pocházet z bitové kopie prostředky, ale jsou vykreslovány přímo pomocí aplikace (nebo OS jménem aplikace).

Široký rozsah, barvy může nést výzvy, několik při práci na úrovni uživatelského rozhraní:

- **Komunikaci barvy** – při posuzování barev návrháři a vývojáři obvykle je _předpokládá, že_ sRGB podílejí barevný prostor. Takže barvu, která může být předávají jako `rgb(128, 45, 56)` nebo `#FF0456`. V široký rozsah návrh tyto reprezentace neposkytují dostatek informací, aby přesně vyjadřovat zadaná barva, pracovní barevný prostor také musí být zahrnut. Apple navrhuje pomocí `P3(128, 45, 56)` a `P3#FF0456` místo. 
- **Výběr barvy** – většina úpravy oblíbených bitové kopie a návrh softwaru trpí stejná omezení jako barevný prostor sRGB při použití výběru jejich předdefinované barvy. Návrháře měli ujistit, že se "Zobrazení P3" barevný prostor v dialogovém okně Výběr barvy při práci s návrhy široké barev.
- **Kódování barvy** – obě `NSColor` (macOS) a `UIColor` (iOS & tvOS) mají nové usnadňující metody pro generování barev P3 přímo a obě se rozšířily a podporovat barvy v rozsah Extended sRGB barevný prostor také.
- **Ukládání barvy** -byste měli věnovat při ukládání široký rozsah barvy v dokumentu aplikace. Kde 8 bitů na kanál barva fungovala bez problémů pro barevný prostor sRGB, použije se pro široké barvy 16 bitů na barvu kanál. Vývojář musí také sledovat instancí předpokládané barevný prostor (protože všechno, co se tradičně sRGB pouze).

## <a name="colors-on-the-web"></a>Barvy na webu

Potřeba dát pozor při práci s wide barev v webové stránky a na zařízení, které podporují celou barevné zobrazení. Pokud všechny bitové kopie obsahu, který byl součástí webu má byla správně označené, iOS a systému macOS bude automaticky barvu shody a je správně zobrazit.

Dotazy na média je také možné vyřešit prostředky mezi P3 a sRGB podporující zařízení:

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple má také WebKit návrh, který vám umožní šablon stylů CSS zadat v jiné prostory barva kromě předpokládané sRGB místa.

## <a name="rendering-off-screen-images-in-app"></a>Vykreslování obrázků mimo obrazovku v aplikaci

Na základě typu aplikace vytváří, může povolit uživateli zahrnují obsahu bitové kopie, budou mít shromážděné z Internetu nebo vytvořte obsahu bitové kopie přímo v aplikaci (například vector, například kreslení aplikace).

V obou případech aplikace může vykreslit požadované dokumentů v široké barvu pomocí rozšířené funkce přidané do iOS i systému macOS mimo obrazovku.

### <a name="drawing-wide-color-in-ios"></a>Kreslení široké barev v iOS

Před hovoříte o jak ke správnému nakreslení bitovou kopii široké barev v iOS 10, podívejte se na následující běžné kreslení kód iOS:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...
    
    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
}
```

Dochází k potížím s standardní kód, který bude potřeba řešit _před_ může sloužit k vykreslení bitovou kopii široké barev. `UIGraphics.BeginImageContext (size)` Metoda používaná k spouštění iOS kreslení obrázku má následující omezení:

- Kontexty bitové kopie ji nelze vytvořit s větší než 8 bitů na barvu kanálu.
- Ho nemůže představují barev v rozsahu Extended sRGB barevný prostor.
- Nemá schopnost poskytovat rozhraní k vytvoření kontextů v barevný prostor jiný sRGB kvůli rutiny nízké úrovně C volané na pozadí.

Ke zpracování těchto omezení a kreslení bitovou kopii široké barev v iOS 10, použijte následující kód:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

Nové `UIGraphicsImageRenderer` třída vytvoří nový kontext bitové kopie, který umožňuje zpracovávat sRGB Extended rozsah barevný prostor a má následující funkce:

- Je plně barva spravované ve výchozím nastavení.
- Podporuje rozšířené rozsah sRGB barevný prostor ve výchozím nastavení.
- Inteligentně rozhodne, pokud vykreslovat v sRGB nebo rozsah Extended sRGB barevný prostor na možnosti zařízení s iOS, která aplikace běží na základě.
- Plně a automaticky spravuje kontext bitové kopie (`CGContext`) životnost, takže vývojáři nemá starat o volání začínat a končit příkazy kontextu.
- Je kompatibilní s `UIGraphics.GetCurrentContext()` metoda.

`CreateImage` Metodu `UIGraphicsImageRenderer` třída je volat za účelem vytvoření bitové kopie široké barva a předán dokončení obslužná rutina s kontextem bitovou kopii k vykreslení do. Všechny výkresu se provádí v rámci této obslužné rutiny dokončení následujícím způsobem:

- `UIColor.FromDisplayP3` Metoda vytvoří novou syté červenou barvu v široký rozsah zobrazení P3 barevný prostor a slouží k vykreslení poloviny rámeček. 
- Druhá polovina rámeček se vykresluje v normálním sRGB plně zcela využita červenou barvu pro porovnání.

### <a name="drawing-wide-color-in-macos"></a>Kreslení v systému macOS široké barev

`NSImage` Třída rozšířila v systému macOS Sierra pro podporu kreslení široké barev obrázků. Příklad:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);
    
    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();
    
    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();
    
    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>Na obrazovce vykreslování obrázků v aplikaci

Vykreslování obrázků široké barvu na obrazovce, probíhá proces podobná kreslení obrázku mimo obrazovku široké barvu pro systému macOS a iOS uvedené výše.

### <a name="rendering-on-screen-in-ios"></a>Na obrazovce vykreslování v iOS

Pokud aplikace potřebuje k vykreslení bitovou kopii v široké barvu na obrazovce v iOS, přepsat `Draw` metodu `UIView` dotyčném jako obvykle. Příklad:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
            ...
        }
    }
}
``` 

IOS 10 stejně jako s `UIGraphicsImageRenderer` třídy uvedené výše, inteligentně rozhodne, pokud vykreslovat v sRGB nebo rozsah Extended sRGB barevný prostor na základě schopností zařízení iOS, které aplikace běží, když `Draw` metoda je volána. Kromě toho `UIImageView` byla správa od sady iOS 9.3 také barev.

Pokud aplikace potřebuje vědět, jak probíhá na vykreslování `UIView` nebo `UIViewController`, můžete zkontrolovat nové `DisplayGamut` vlastnost `UITraitCollection` třídy. Tato hodnota bude `UIDisplayGamut` výčtu následující:

- P3
- SRGB
- Tento parametr

Pokud aplikace chce ovládací prvek, který barevný prostor slouží k vykreslení bitovou kopii, můžete použít novou `ContentsFormat` vlastnost `CALayer` k určení požadované barevný prostor. Tato hodnota může být `CAContentsFormat` výčtu následující:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>Na obrazovce vykreslování v systému macOS

Pokud aplikace potřebuje k vykreslení bitovou kopii v široké barvu na obrazovce v systému macOS, přepsat `DrawRect` metodu `NSView` dotyčném jako obvykle. Příklad:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

Znovu ji inteligentně rozhodne Pokud vykreslovat v sRGB nebo rozsah Extended sRGB barevný prostor podle možnosti hardwaru Mac, který aplikace běží, když `DrawRect` metoda je volána.

Pokud aplikace chce ovládací prvek, který barevný prostor slouží k vykreslení bitovou kopii, můžete použít novou `DepthLimit` vlastnost `NSWindow` třídu k určení požadované barevný prostor. Tato hodnota může být `NSWindowDepth` výčtu následující:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých široké barvy a způsoby, jakými může implementovat a použít uvnitř Xamarin.iOS nebo Xamarin.Mac aplikace.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
