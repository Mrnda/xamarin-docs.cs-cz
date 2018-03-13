---
title: "Jednotná scénářů"
description: "Jednotná scénářů povolit iOS developer vytvoření uživatelského rozhraní pomocí jednoho storyboard, nikoli několik scénářů, tak, aby pokrývalo zvětšující rozsahu velikost obrazovky zařízení. Tento článek je navržená tak získali podrobnější přehled do operaci jednotná scénáře v rámci Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 30a952bf0df4db34c749de3d6198877b7a9766b9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="unified-storyboards"></a>Jednotná scénářů

_Jednotná scénářů povolit iOS developer vytvoření uživatelského rozhraní pomocí jednoho storyboard, nikoli několik scénářů, tak, aby pokrývalo zvětšující rozsahu velikost obrazovky zařízení. Tento článek je navržená tak získali podrobnější přehled do operaci jednotná scénáře v rámci Xamarin.iOS._

iOS 8 obsahuje nový, jednodušší použití mechanismus pro vytvoření uživatelského rozhraní – jednotná scénáře. Pomocí jednoho storyboard nepokrývají všechny velikosti obrazovky jiný hardware, mohou být vytvořeny rychlé a dobře reagovaly zobrazení v "návrhu-jednou, použijte m" stylu.

Jako vývojář už potřebuje vytvořit samostatné a konkrétní scénáře pro zařízení iPhone a iPad, mají flexibilitu při navrhování aplikací s společné rozhraní a pak přizpůsobit tohoto rozhraní pro jinou velikost třídy. Tímto způsobem dokáže se přizpůsobit aplikaci síly každý faktor formuláře a každé uživatelské rozhraní lze ladit k dosažení optimálního výkonu.

<a name="size-classes" />

## <a name="size-classes"></a>Velikost třídy

Před iOS 8, vývojáři použít `UIInterfaceOrientation` a `UIInterfaceIdiom` k rozlišení mezi režimy na výšku a šířku a mezi zařízení iPhone a iPad. V iOS8, je určen orientaci a zařízení pomocí *velikost třídy*.

Zařízení jsou definovány třídy velikosti ve vodorovném i svislém osy a existují dva typy tříd velikost v iOS 8:

-  **Regulární** – to je pro velikost promítat obrazovku (jako je iPad) nebo miniaplikaci, která vyvolává dojem na největší velikost (například `UIScrollView`
-  **Compact** – to je pro menší zařízení (například zařízení typu iPhone). Tato velikost bere v úvahu orientaci zařízení.


Pokud se dvěma konceptů použijí společně, výsledkem je mřížky 2 x 2, který definuje různé možné velikosti, které lze použít v obou různé orientace, jak je vidět v následujícím diagramu:

 [![](unified-storyboards-images/sizeclassgrid.png "Mřížky 2 x 2, který definuje různé možné velikosti, které lze použít v regulárním a Compact orientaci")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Vývojář může vytvořit řadič zobrazení, která používá čtyři možnosti, které by způsobilo různých rozložení (jak je vidět v grafiky výše).

### <a name="ipad-size-classes"></a>iPad velikost třídy

IPad kvůli překročení velikosti, má **regulární** třídy velikost obou směrech.

 [![](unified-storyboards-images/image1.png "iPad velikost třídy")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone velikost třídy

Pro iPhone má jinou velikost třídy na základě orientace zařízení:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone velikost třídy")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  Když je zařízení v režimu na výšku, má na obrazovce **compact** třídy vodorovně a **regulární** svisle
-  Když je zařízení v režimu na šířku, třídy obrazovku se vrátit zpět z režimu na výšku.

### <a name="iphone-6-plus-size-classes"></a>Velikost třídy iPhone 6 Plus

Velikosti jsou stejné jako starší Iphony v orientaci na výšku, ale liší na šířku:

[![](unified-storyboards-images/iphone6sizeclasses.png "Velikost třídy iPhone 6 Plus")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Protože iPhone 6 Plus má dostatečně velké na obrazovku, bude moci mít běžné třídy velikost šířka v režimu na šířku.

### <a name="support-for-a-new-screen-scale"></a>Podpora pro nové obrazovky škálování

Pro iPhone 6 Plus používá nové zobrazení sítnice HD s měřítko na obrazovce 3.0 (třikrát původní iPhone rozlišení obrazovky). Zajistit optimální prostředí v těchto zařízeních, zahrnují nové kresby určené pro tohoto měřítka obrazovky. V prostředí Xcode 6 a vyšší asset katalogů můžete zahrnout i obrázky v 1 x, 2 x a 3 x velikosti; jednoduše přidat nové prostředky bitové kopie a iOS vybere správný prostředky při spuštění v zařízení iPhone 6 Plus.

Rozpozná bitovou kopii načítání chování v iOS také `@3x` příponu na soubory obrázků. Například, pokud vývojář obsahuje prostředek obrázku (na různá řešení) v sadě aplikace s následujícími názvy souborů: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, a `MonkeyIcon@3x.png`. Na zařízeních iPhone 6 a `MonkeyIcon@3x.png` bitové kopie budou automaticky použity, pokud vývojář načte bitovou kopii pomocí následujícího kódu:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Nebo pokud se přiřadit bitovou kopii k prvku uživatelského rozhraní pomocí iOS návrháře jako `MonkeyIcon.png`, `MonkeyIcon@3x.png` se použije, znovu automaticky na zařízení iPhone 6 Plus.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Dynamické spouštěcí obrazovky

Spusťte soubor obrazovky se zobrazí úvodní obrazovka při k poskytnutí zpětné vazby uživateli, které aplikace je ve skutečnosti spouštění je spuštění aplikace pro iOS. Před iOS 8, by měl zahrnovat několik Vývojář `Default.png` prostředků pro každé zařízení typu, orientaci a rozlišení obrazovky aplikace by se systémem na bitovou kopii.

Nové do systému iOS 8, můžete vytvořit vývojář jedinou atomic `.xib` souboru v Xcode, kterou použije automatické rozložení a velikost třídy pro vytvoření *dynamické spusťte obrazovky* , bude fungovat pro každé zařízení, řešení a orientace. To nejen snižuje množství práce potřebné vývojáře pro vytváření a údržbu všechny prostředky požadované image, ale snižuje velikost nainstalované sady aplikace.

## <a name="traits"></a>Vlastnosti

Vlastnosti jsou vlastnosti, které lze použít k určení, jak rozložení mění její změny prostředí. Skládají se z sadu vlastností ( `HorizontalSizeClass` a `VerticalSizeClass` na základě `UIUserInterfaceSizeClass`), a také stylu rozhraní ( `UIUserInterfaceIdiom`) a měřítko zobrazení.

Všechny výše uvedené stavy jsou zabalena do kontejneru, který Apple odkazuje jako na kolekci znak ( `UITraitCollection`), který obsahuje pouze vlastnosti, ale také jejich hodnot.

## <a name="trait-environment"></a>Znak prostředí

Znak prostředí jsou nové rozhraní v iOS 8 a jsou moct vrátit kolekci znak pro následující objekty:

-  Obrazovky ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  Zobrazení řadičů ( `UIViewController` ).
-  Zobrazení ( `UIView` ).
-  Prezentace řadiče ( `UIPresentationController` ).


Vývojář kolekce znak vrácené znak prostředí používá k určení, jak by měla být rozložená uživatelské rozhraní.

Všechny prostředí znak Zkontrolujte hierarchie, jak je vidět v následujícím diagramu:

 [![](unified-storyboards-images/viewhierarchy.png "Diagram hierarchie znak prostředí")](unified-storyboards-images/viewhierarchy.png#lightbox)

Kolekce znak každou z výše uvedených znak prostředí mají bude procházet, ve výchozím nastavení z nadřazené do podřízené prostředí.

Kromě získání aktuální znak kolekce, má znak prostředí `TraitCollectionDidChange` metoda, která můžete přepsat v podtřídách řadič zobrazení nebo zobrazení. Vývojář můžete použít tuto metodu můžete upravit všechny prvky uživatelského rozhraní, které závisí na vlastnosti, když se změnily tyto vlastnosti.

## <a name="typical-trait-collections"></a>Typické znak kolekce

Tato část obsahuje typické typy znak kolekce, které budou uživatele při práci s iOS 8.

Toto je typické znak kolekce, která vývojář může zobrazit v zařízení iPhone:

<table width="100%" border="1px">
<thead>
<tr>
    <td>Vlastnost</td>
    <td>Hodnota</td>
</tr>
</thead>
<tbody>
<tr>
    <td><code>HorizontalSizeClass</code></td>
    <td>Compact</td>
</tr>
<tr>
    <td><code>VerticalSizeClass</code></td>
    <td>Regulární</td>
</tr>
<tr>
    <td><code>UserInterfaceIdom</code></td>
    <td>Telefon</td>
</tr>
<tr>
    <td><code>DisplayScale</code></td>
    <td>2.0</td>
</tr>
</tbody>
</table>

Sada výše by představují plně kvalifikovaný znak kolekce, jako má hodnoty pro všechny vlastnosti jeho výšku.

Je také možné, že znak kolekce, která chybí některé z jeho hodnoty (které Apple označuje jako *nespecifikovaný*):

<table width="100%" border="1px">
<thead>
<tr>
    <td>Vlastnost</td>
    <td>Hodnota</td>
</tr>
</thead>
<tbody>
<tr>
    <td><code>HorizontalSizeClass</code></td>
    <td>Compact</td>
</tr>
<tr>
    <td><code>VerticalSizeClass</code></td>
    <td>{neurčené}</td>
</tr>
<tr>
    <td><code>UserInterfaceIdom</code></td>
    <td>{neurčené}</td>
</tr>
<tr>
    <td><code>DisplayScale</code></td>
    <td>{neurčené}</td>
</tr>
</tbody>
</table>

Obecně platí ale když vývojář požaduje prostředí znak jeho výšku kolekce, vrátí kolekci plně kvalifikovaný jak je vidět v předchozím příkladu.

Pokud prostředí znak (například zobrazení nebo View Controller) není v aktuálním zobrazení hierarchie, vývojář může vrátit zpět nezadané hodnoty pro jeden nebo více vlastností znak.

Vývojáři získají kolekci částečně kvalifikovaný znak, pokud používají jednu z metod vytváření poskytovaných společností Apple, jako například `UITraitCollection.FromHorizontalSizeClass`, chcete-li vytvořit novou kolekci.

Jednu operaci, kterou lze provést na více kolekcí znak je jejich porovnání mezi sebou, což zahrnuje dotazem jeden znak kolekci, pokud obsahuje jiný. Pojmy *omezení* je, že pro libovolný znak zadaný v druhé kolekci, musí hodnota odpovídat přesně s hodnotou v první kolekci.

K otestování dvě vlastnosti použijte `Contains` metodu `UITraitCollection` předávání v hodnotě znak, který má být testována.

Vývojář může ručně provést porovnání v kódu k určení jak rozložení zobrazení nebo zobrazení řadičů. Ale `UIKit` interně používá tato metoda některé ze svých funkcí, jako Proxy vzhled, třeba poskytnout.

## <a name="appearance-proxy"></a>Vzhled Proxy

Proxy server vzhled byla zavedena v dřívějších verzích systému iOS umožňují vývojářům přizpůsobit vlastnosti jejich zobrazení. Bylo rozšířeno na iOS 8 pro podporu znak kolekce.

Vzhled proxy teď o novou metodu, `AppearanceForTraitCollection`, která vrací nový vzhled Proxy pro danou kolekci znak, který byl předán v. Všechna přizpůsobení, která provádí vývojář na které vzhled Proxy se projeví na zobrazení, která odpovídají ke kolekci zadaný znak.

Obecně vývojář předá částečně znak kolekce k `AppearanceForTraitCollection` metody, jako je ten, který právě zadali vodorovné velikost třídy z komprimovat, tak, aby se může přizpůsobit všechna zobrazení v aplikaci, která je vodorovně compact.

## <a name="uiimage"></a>UIImage

Jiné třídy, která Apple přidala je znak kolekce `UIImage`. V minulosti museli zadat na vývojáře @1X a @2x verzi všechny rastrové grafické asset, který se má zahrnout do aplikace (například ikonu).

iOS 8 se rozšířila na umožňuje vývojářům obsahují více verzi bitové kopie v katalog bitové kopie na základě kolekce znak. Vývojář může obsahovat třeba menší obrázek pro práci s třídu Compact znak a velikosti a úplnou bitovou kopii pro jiné kolekce.

Při použití uvnitř jednu z imagí `UIImageView` třída, zobrazení Image se automaticky zobrazí správnou verzi bitovou kopii pro jeho výšku kolekce. Pokud prostředí znak změní (například uživatel přepínání zařízení z na výšku na šířku), nové velikost bitové kopie odpovídající nová kolekce znak a změňte jeho velikost odpovídat aktuální verzi bitové kopie se automaticky vybere zobrazení bitové kopie zobrazí.

## <a name="uiimageasset"></a>UIImageAsset

Apple přidala novou třídu do systému iOS 8 s názvem `UIImageAsset` umožnit vývojář ještě větší kontrolu nad výběr image.

Prostředek obrázku zabalí zálohování všech různé verze bitové kopie a umožňuje vývojáři požádat o konkrétní bitovou kopii, která odpovídá kolekci znak, který byl předán v. Bitové kopie můžete přidat nebo odebrat z prostředek bitové kopie na průběžně.

Další informace o bitové kopie prostředky, najdete v článku společnosti Apple [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) dokumentaci.

## <a name="combining-trait-collections"></a>Kombinování kolekcí znak

Další funkce, která vývojář můžete provádět na výšku kolekce má dva přidat společně se bude mít za následek kombinované kolekce, kde jsou hodnoty zadané v druhá nahrazen nezadané hodnoty z jedné kolekce. To se provádí pomocí `FromTraitsFromCollections` metodu `UITraitCollection` třídy.

Jak jsme uvedli výše, pokud žádné z jsou vlastnosti je Nespecifikovaná v jedné z kolekcí znak a je uveden v jiné, hodnota se nastaví na zadanou verzi. Ale pokud existuje více verzí dané hodnoty zadané, hodnotu z poslední znak kolekce bude hodnotu, která se používá.


## <a name="adaptive-view-controllers"></a>Adaptivní zobrazení řadičů

Tato část se bude zabývat podrobnosti o tom, jak iOS zobrazení a zobrazení řadičů přijaly koncepty velikost třídy a vlastnosti automaticky být více adaptivní v aplikacích pro vývojáře.

### <a name="split-view-controller"></a>Rozdělení řadiče zobrazení

Jeden z řadiče zobrazení třídy, které se změnil na maximum v iOS 8 je `UISplitViewController` třídy. V minulosti vývojář využije často řadič zobrazení rozdělení na iPad verzi aplikace a pak by mít zajistit úplně jinou verzi jejich zobrazení hierarchie pro iPhone verzi aplikace.

V iOS 8 `UISplitViewController` třída je k dispozici na obě platformy (iPad a iPhone), který umožňuje vývojáři k vytvoření jedné hierarchie View Controller, který bude fungovat pro iPhone a iPad.

Pokud je zařízení typu iPhone na šířku, nabídne řadiče zobrazení rozdělení jeho zobrazení-souběžného, stejně jako když se zobrazí v zařízení iPad.

### <a name="overriding-traits"></a>Přepsání vlastnosti

Znak prostředí přeneseny z kontejneru nadřazené a podřízené kontejnery, stejně jako na následujícím obrázku zobrazující řadič zobrazení rozdělení na Ipadu v orientaci na šířku:

 [![](unified-storyboards-images/cascadingclasses01.png "Řadič zobrazení rozdělení na Ipadu v orientaci na šířku")](unified-storyboards-images/cascadingclasses01.png#lightbox)

Vzhledem k tomu, že iPad má běžné třídy velikost v vodorovného a svislého orientaci, se zobrazí zobrazení rozdělení hlavní i podrobné zobrazení.

V zařízení iPhone, kde je třída velikost compact v obou směrech, rozdělení řadiče zobrazení pouze zobrazuje podrobné zobrazení, jak vidíte níže:

 [![](unified-storyboards-images/cascadingclasses02.png "Rozdělení řadiče zobrazení pouze zobrazuje podrobné zobrazení")](unified-storyboards-images/cascadingclasses02.png#lightbox)

V aplikaci, kde chce zobrazení hlavní i podrobné zobrazení v zařízení iPhone v orientaci na šířku vývojáře musí vývojář vložení nadřazený kontejner pro řadiče zobrazení rozdělení a přepsat kolekci znak. Jak je vidět na následujícím obrázku:

 [![](unified-storyboards-images/cascadingclasses03.png "Vývojář musí vložení nadřazený kontejner pro řadiče zobrazení rozdělení a přepsat kolekci znak")](unified-storyboards-images/cascadingclasses03.png#lightbox)

A `UIView` je nastaven jako nadřazená položka řadiče zobrazení rozdělení a `SetOverrideTraitCollection` metoda je volána v zobrazení předávání v nové kolekce znak a cílení řadiče zobrazení rozdělení. Nová kolekce znak přepsání `HorizontalSizeClass`, jeho nastavení na hodnotu `Regular`tak, aby řadiče zobrazení rozdělení zobrazí hlavní i podrobné zobrazení u zařízení typu iPhone v orientaci na šířku.

Všimněte si, že `VerticalSizeClass` byla nastavena na `unspecified`, která umožňuje novou kolekci znak pro přidání do kolekce znak na nadřazený, což vede `Compact VerticalSizeClass` pro podřízené rozdělení View Controller.

### <a name="trait-changes"></a>Znak změny

Tato část bude trvat vypadá a podrobně v jak kolekce znak přechod při změně prostředí znak. Pokud je například zařízení otáčet z výšky na šířku.

 [![](unified-storyboards-images/traittransitions01.png "Výšky na šířku změny znak – přehled")](unified-storyboards-images/traittransitions01.png#lightbox)

Nejprve iOS 8 nepodporuje některé instalace přípravy přechod proběhla. V dalším kroku systému animuje přechodový stav. Nakonec iOS 8 vyčistí up všechny dočasné stavy vyžaduje během přechodu.

iOS 8 obsahuje několik zpětná volání, které vývojář můžete použít k účasti na změnu znak, jak je vidět v následující tabulce:

<table width="100%" border="1px">
<thead>
<tr>
    <td>Phase</td>
    <td>Zpětné volání</td>
    <td>Popis</td>
</tr>
</thead>
<tbody>
<tr>
    <td>Instalace</td>
    <td>
        <ul>
        <li><code>WillTransitionToTraitCollection</code></li>
        <li><code>TraitCollectionDidChange</code></li>
        </ul>
    </td>
    <td>
        <ul>
        <li>Tato metoda je volána na začátku změnu znak předtím, než získá kolekci znak nastavena na novou hodnotu.</li>
        <li>Metoda volána při změně hodnoty kolekce znak, ale před provedením jakékoli animace.</li>
        </ul>
    </td>
</tr>
<tr>
    <td>Animace</td>
    <td>
        <ul>
        <li><code>WillTransitionToTraitCollection</code></li>
        </ul>
    </td>
    <td>
        <ul>
        <li>Koordinátor přechodu, který získá předaná této metodě má <code>AnimateAlongside</code> vlastnost, která umožňuje vývojáři přidat animace, které bude spuštěn společně s výchozí animace.</li>
        </ul>
    </td>
</tr>
<tr>
    <td>Čištění</td>
    <td>
        <ul>
        <li><code>WillTransitionToTraitCollection</code></li>
        </ul>
    </td>
    <td>
        <ul>
        <li>Poskytuje metodu pro vývojáře zahrnout vlastní kód čištění po provedení přechodu.</li>
        </ul>
    </td>
</tr>
</tbody>
</table>

`WillTransitionToTraitCollection` Metoda je skvělá pro animace řadiče zobrazení spolu s změny znak kolekce. `WillTransitionToTraitCollection` Metoda je dostupná pouze na zobrazení řadičů ( `UIViewController`) a ne na jiných znak prostředích, jako je `UIViews`.

`TraitCollectionDidChange` Se skvěle hodí pro práci s `UIView` třídy, kde chce aktualizace uživatelského rozhraní, jako jsou vlastnosti změna vývojář.

### <a name="collapsing-the-split-view-controllers"></a>Sbalení řadiče zobrazení rozdělení

Nyní Pojďme proveďte bližší pohled na co se stane, když řadič zobrazení rozdělení Hroutí ze dvou sloupce k zobrazení jeden sloupec. V rámci této změny existují dva procesy, které vyžaduje:

-  Ve výchozím nastavení řadiče zobrazení rozdělení použije řadiče primární zobrazení jako zobrazení potom, co dojde sbalit. Vývojář toto chování můžete přepsat přepsáním `GetPrimaryViewControllerForCollapsingSplitViewController` metodu `UISplitViewControllerDelegate` a poskytuje všechny řadiče zobrazení, které se mají zobrazit ve sbalených stavu.
-  Sekundární řadič zobrazení musí nesloučí do primárního řadiče zobrazení. Vývojář nebudete muset provádět žádnou akci pro tento krok; Rozdělení řadiče zobrazení zahrnuje automatické zpracování této fáze podle hardwarovém zařízení. Však může být některých zvláštních případech, kde vývojář chtít pracovat s tuto změnu. Volání `CollapseSecondViewController` metodu `UISplitViewControllerDelegate` umožňuje řadičem hlavního zobrazení, který se má zobrazit, když dojde k sbalit, místo zobrazení podrobností.


### <a name="expanding-the-split-view-controller"></a>Rozšiřování řadiče zobrazení rozdělení

Nyní Pojďme proveďte bližší pohled na co se stane, když řadič zobrazení rozdělení je rozšířena z sbalený. Existují ještě jednou dvou fázích, které vyžaduje:

-  Nejprve definujte nový primární řadič zobrazení. Ve výchozím nastavení řadiče zobrazení rozdělení automaticky použije primární řadič zobrazení z sbaleným zobrazením. Vývojář znovu, můžete přepsat toto chování pomocí `GetPrimaryViewControllerForExpandingSplitViewController` metodu `UISplitViewControllerDelegate` .
-  Jakmile je primární řadič zobrazení byla vybrána, je nutné znovu vytvořit sekundární řadič zobrazení. Znovu řadiče zobrazení rozdělení zahrnuje automatické zpracování této fáze podle hardwarovém zařízení. Vývojář toto chování můžete přepsat pomocí volání `SeparateSecondaryViewController` metodu `UISplitViewControllerDelegate` .


V zobrazení řadič rozdělení primární řadič zobrazení hraje součástí rozšiřování i sbalení zobrazení implementací `CollapseSecondViewController` a `SeparateSecondaryViewController` metody `UISplitViewControllerDelegate`. `UINavigationController` implementuje tyto metody Automatické nabízené a pop řadiče sekundární zobrazení.

### <a name="showing-view-controllers"></a>Zobrazuje řadiče zobrazení

Další změny, která má provedené Apple iOS 8 je způsobem, že vývojář ukazuje řadiče zobrazení. V minulosti, pokud aplikace měl řadič zobrazení typu list (jako je například tabulka View Controller) a vývojář vám ukázal jinou (například v reakci na klepnutím na uživatele na buňku), aplikace by dosáhnout zpátky pomocí řadiče hierarchie Navigace View Controller a volání `PushViewController` metoda proti zobrazit nové zobrazení.

Toto zobrazí přísnou párování mezi řadičem navigační a prostředí, ve kterém byl spuštěn v. V iOS 8 má odpojené Apple tím, že poskytuje dvě nové metody toto:

-  `ShowViewController` – Přizpůsobení zobrazení nového řadiče zobrazení na základě jeho prostředí. Například v `UINavigationController` jednoduše vynutí nové zobrazení do zásobníku. V zobrazení řadič rozdělení nového řadiče zobrazení zobrazí na levé straně jako nový primární řadič zobrazení. Pokud je k dispozici žádný řadič kontejner zobrazení, zobrazí se nové zobrazení jako modální řadiče zobrazení.
-  `ShowDetailViewController` – Funguje podobným způsobem k `ShowViewController`, ale se implementuje na řadič zobrazení rozdělení nahradili nového řadiče zobrazení, které jsou předávány v zobrazení podrobností. Pokud řadiče zobrazení rozdělení sbalena (registrovaného může být v zařízení typu iPhone aplikace), volání bude přesměrován na `ShowViewController` metoda a nové zobrazení se zobrazí jako primární řadič zobrazení. Znovu Pokud je k dispozici žádný řadič zobrazení kontejneru, nové zobrazení se zobrazí jako modální řadiče zobrazení.


Tyto metody fungovat spuštěním v listu řadiče zobrazení a provede zobrazení hierarchie, dokud najdou řadiče zobrazení správném kontejneru pro zpracování zobrazení nového zobrazení.

Vývojářům můžete implementovat `ShowViewController` a `ShowDetailViewController` ve vlastní vlastní řadiče zobrazení získat stejné funkce automatizované, `UINavigationController` a `UISplitViewController` poskytuje.

### <a name="how-it-works"></a>Jak to funguje

V této části jsme se podívejte se na tom, jak jsou tyto metody ve skutečnosti implementují v iOS 8. První Podíváme se na nové `GetTargetForAction` metoda:

 [![](unified-storyboards-images/gettargetforaction.png "Nová metoda GetTargetForAction")](unified-storyboards-images/gettargetforaction.png#lightbox)

Tato metoda provede řetězec hierarchie, dokud nebude nalezen řadiče zobrazení správné kontejneru. Příklad:

1.  Pokud `ShowViewController` metoda je volána, prvního řadiče zobrazení v řetězci, který implementuje tuto metodu je řadičem navigace, takže se používá jako součást nové zobrazení.
1.  Pokud `ShowDetailViewController` místo toho byla volána metoda, rozdělení řadiče zobrazení je první řadič zobrazení, implementace, použije se jako nadřazená položka.


`GetTargetForAction` Metoda funguje tak, že hledání řadiče zobrazení, který implementuje příslušné akce a potom s dotazem tohoto řadiče zobrazení, pokud chcete přijímat této akce. Vzhledem k tomu, že tato metoda je veřejný, vývojáři mohou vytvářet své vlastní vlastní metody, které fungují stejně jako předdefinovaných v `ShowViewController` a `ShowDetailViewController` metody.

## <a name="adaptive-presentation"></a>Adaptivní prezentace

V iOS 8, Apple udělal Popover prezentací ( `UIPopoverPresentationController`) adaptivní také. Takže řadič Popover prezentace zobrazení automaticky nabídne normální Popover zobrazení v běžné třídy velikost, ale zobrazí se celé obrazovky v třídě vodorovně compact velikost (například na zařízení typu iPhone).

Abychom vyhověli změny v rámci systému jednotná storyboard, ke správě vidění řadiče zobrazení vytvořila nový objekt řadiče – `UIPresentationController`. Tento řadič se vytvoří od okamžiku, kdy řadiče zobrazení se zobrazí, dokud se zavře. Protože je Správa třídu, ho lze považovat za super třídu přes řadič zobrazení jako reaguje na změny zařízení, které mají vliv řadiče zobrazení (například orientace), které jsou pak krmí zpět do řadiče zobrazení ovládací prvky prezentace řadiče.

Když vývojáři uvede řadiče zobrazení pomocí `PresentViewController` metoda, je předávány správu procesu `UIKit`. Zpracovává UIKit (mimo jiné) ve správné řadiči styl vytváří, s jedinou výjimkou je při řadič zobrazení je nastaven styl `UIModalPresentationCustom`. Aplikace může zajistit tady je vlastní PresentationController spíše než `UIKit` řadiče.

### <a name="custom-presentation-styles"></a>Vlastní prezentace styly

Styl vlastní prezentace vývojáři mají možnost použít vlastní řadiče prezentace. Tento vlastní řadič lze upravit vzhled a chování je příbuzných k zobrazení.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Práce s třídami velikost

Adaptivní projektu Xamarin fotografie, která je součástí tento článek poskytuje pracovní příklad použití třídy velikost a adaptivní řadiče zobrazení v aplikaci iOS 8 jednotné rozhraní.

Při svém uživatelském rozhraní aplikace úplně vytvoří z kódu, a pomocí návrháře IOS a vytváření Unified Storyboard, použije se stejné techniky. Později v tomto článku ukážeme, jak používat velikost třídy s scénáře se Unified a Návrhář v aplikaci Xamarin iOS.

Nyní Podívejme bližší pohled na jak projektu adaptivní fotografie je implementace několik funkcí velikost třídy v iOS 8 pro vytvoření adaptivní aplikace.

### <a name="adapting-to-trait-environment-changes"></a>Přizpůsobení na výšku prostředí změny

Při spuštění aplikace adaptivní fotografie v zařízení iPhone, když uživatel otočí zařízení z výšky na šířku, rozdělení řadiče zobrazení se zobrazí seznamu a podrobností zobrazení:

 [![](unified-storyboards-images/rotation.png "Rozdělení řadiče zobrazení se zobrazí oba hlavní a zobrazit podrobnosti, jak je vidět tady")](unified-storyboards-images/rotation.png#lightbox)

Toho lze dosáhnout přepsáním `UpdateConstraintsForTraitCollection` na základě hodnoty na základě metod řadiče zobrazení a úpravě omezení `VerticalSizeClass`. Příklad:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>Přidání animace přechodu

Když řadič zobrazení rozdělení v adaptivní Fotkám, které aplikace přejde z sbaleny do rozšířené, animace se přidají do výchozí animací přepsáním `WillTransitionToTraitCollection` metoda řadiče zobrazení. Příklad:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>Přepsání prostředí znak

Jak zobrazit výše, vynutí aplikace adaptivní fotografie hlavní zobrazení a View Controller rozdělení jak podrobnostmi o když je zařízení iPhone v zobrazení na šířku.

Bylo umožněno pomocí následujícího kódu v Kontroleru zobrazení:

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Rozbalení a sbalení řadiče zobrazení rozdělení

Další Podívejme se na tom, jak byl implementují rozbalení a sbalení chování rozdělení View Controller v Xamarin. V `AppDelegate`, když je vytvořen řadiče zobrazení rozdělení, jeho delegáta je přiřazen ke zpracování těchto změn:

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

`SeparateSecondaryViewController` Metoda testy, zda se zobrazuje fotografie, a provede akci založenou na tomto stavu. Pokud žádné fotografie se zobrazí sbalí sekundární řadič zobrazení tak, aby se zobrazí řadičem hlavního zobrazení.

`CollapseSecondViewController` Metoda se používá při rozšiřování řadiče zobrazení rozdělení zobrazíte, pokud žádné fotografie existovat v zásobníku, pokud tak sbalí zpět do tohoto zobrazení.

### <a name="moving-between-view-controllers"></a>Přesouvání mezi řadiče zobrazení

V dalším kroku Podívejme se na jak aplikace adaptivní fotografie přesune mezi řadiče zobrazení. V `AAPLConversationViewController` třídy, když uživatel vybere buňku z tabulky `ShowDetailViewController` metoda je volána zobrazit podrobnosti:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>Zobrazení ukazatele zpřístupnění

V aplikaci adaptivní fotografií jsou několika místech, kde jsou zpřístupnění indikátory skrytý nebo zobrazený na základě na změny v prostředí znak. To se zpracovává následujícím kódem:

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

Tyto jsou implementované pomocí `GetTargetViewControllerForAction` metoda podrobněji výše.

Když řadič zobrazení tabulky je zobrazení dat, používá metody implementované výše, jestli se bude dojít push, a zda se má zobrazit nebo skrýt indikátoru zpřístupnění odpovídajícím způsobem:

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>Nové `ShowDetailTargetDidChangeNotification` typu

Apple byl přidán nový typ oznámení pro práci s velikost třídy a prostředí znak z v rámci řadič zobrazení rozdělení `ShowDetailTargetDidChangeNotification`. Toto oznámení se odešlou při každé změně cíl zobrazení podrobností pro řadič zobrazení rozdělení, například pokud je řadič rozšíří nebo sbalí.

Adaptivní fotografie aplikace používá toto oznámení aktualizovat stav indikátoru zpřístupnění, když se změní řadiče zobrazení podrobností:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

Trvat bližší pohled na adaptivní fotografie aplikace zobrazíte všechny způsoby, kterými této třídy velikost, znak kolekce a adaptivní řadiče zobrazení lze snadno vytvořit aplikaci Unified v Xamarin.iOS.

## <a name="unified-storyboards"></a>Jednotná scénářů

Nové sjednocené scénářů iOS 8, umožňuje vývojářům vytvářet, sloučený storyboard soubor, který lze zobrazit na zařízeních iPhone a iPad cílení na více tříd velikost. Pomocí Unified scénářů vývojář zapíše menší konkrétní kód uživatelského rozhraní a má pouze jedno rozhraní návrhu pro vytváření a údržbu.

Hlavní výhody Unified scénářů jsou:

-  Použijte stejný soubor scénáře pro zařízení iPhone a iPad.
-  Nasaďte zpětné iOS 6 a iOS 7.
-  Zobrazte náhled rozložení pro různá zařízení, orientace a verze operačního systému z v rámci Xamarin iOS Designer.

Tato funkce je plně podporovaný v sadě Visual Studio pro Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Povolení třídy velikost

Ve výchozím nastavení, budou všechny nového projektu Xamarin.iOS nám velikost třídy. K používání tříd velikost a adaptivní Segues uvnitř storyboard ze starší projektu, nejprve převeďte na formát Xcode 6 Unified Storyboard z uvnitř iOS Designer.

Otevřete scénáře, který má být převeden na iOS Designer a kontrola **použití třídy velikost** zaškrtnutí políčka:

 [![](unified-storyboards-images/sizeclass01.png "Zaškrtněte políčko použití velikost třídy")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS Designer bude tak jasné, že vývojář chce převést formát scénáři používat velikost třídy:

 [![](unified-storyboards-images/sizeclass02.png "Použití třídy velikost výstrahy")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> **Poznámka:**: Automatické rozložení také musí být kontrolované velikost třídy fungovala správně.

### <a name="generic-device-types"></a>Typy Obecné zařízení

Jakmile scénáři byl převeden na použití třídy velikost, ji budou zobrazí znovu, že v návrhovou plochu a **zobrazení jako** zařízení bude Obecné:

 [![](unified-storyboards-images/sizeclass03.png "Zobrazit jako typ Obecné zařízení")](unified-storyboards-images/sizeclass03.png#lightbox)

Pokud je vybraný typ zařízení, bude nastavena čtverce 600 × 600 velikost všechny řadiče zobrazení. Tato čtvereček představuje velikost všech šířku a výšku. všechny. Když iOS Designer je v tomto režimu, veškeré úpravy platit pro všechny třídy velikost.

Vývojář má také možnost zobrazení na návrhovou plochu, jako je iPhone:

 [![](unified-storyboards-images/sizeclass04.png "Zobrazení na návrhovou plochu, jako je iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

Nebo zobrazení jako iPad:

 [![](unified-storyboards-images/sizeclass05.png "Zobrazení na návrhovou plochu jako iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Vyberte třídu, velikost

Tlačítko výběru třídy velikost je v levém horním rohu na návrhovou plochu (téměř zobrazení jako rozevíracího seznamu). To umožňuje vývojáři vyberte, které třídy velikost jsou aktuálně upravovaný:

 [![](unified-storyboards-images/sizeclass06.png "Vyberte třídu, velikost")](unified-storyboards-images/sizeclass06.png#lightbox)

Selektor uvede výběr třídu velikost jako mřížka 3 x 3. Každý kvadratických v mřížce představuje kombinaci třídu šířka a výška třída. Hranaté center vybere třídu Any Width/Any výšku velikosti (což je výchozí zobrazení pro scénář Unified). Pokud je vybraná tato hranaté, vývojář se úpravy výchozí rozložení, který zdědí všechny ostatní konfigurace.

Hranaté v levém horním rohu mřížky představuje Compact třídu šířka/Compact výšku velikosti:

 [![](unified-storyboards-images/sizeclass07.png "Třída Compact šířka/Compact výšku velikosti")](unified-storyboards-images/sizeclass07.png#lightbox)

Tento režim odpovídá zařízení typu iPhone v orientaci na šířku. Hranaté v pravém dolním rohu mřížky představuje regulární šířka/Regular výšku velikosti třídu, která představuje iPad:

 [![](unified-storyboards-images/sizeclass08.png "Třídy regulárních šířka/regulární výška velikost")](unified-storyboards-images/sizeclass08.png#lightbox)

Chcete-li upravit rozložení pro zařízení typu iPhone v orientaci na výšku, vyberte v levém dolním druhou mocninu. Reprezentuje třídu velikost výška Compact šířka/Regular:

 [![](unified-storyboards-images/sizeclass09.png "Třída Compact šířka/Regular výška velikost")](unified-storyboards-images/sizeclass09.png#lightbox)

Klikněte v hranaté ji vyberte a návrhovou plochu, která se změní velikost řadiče zobrazení tak, aby odpovídala nové výběr:

 [![](unified-storyboards-images/sizeclass10.png "Návrhovou plochu, která se změní velikost řadiče zobrazení tak, aby odpovídala nové výběr, jak je znázorněno")](unified-storyboards-images/sizeclass10.png#lightbox)

Najdete v části třídy velikost tohoto článku informace na velikost třídy a jejich vliv na rozložení pro Iphony a Ipady.

### <a name="adaptive-segue-types"></a>Adaptivní Segue typy

Pokud vývojář použila scénářů před, pak budou obeznámeni s existující typy segue **Push**, **modální** a **Popover**. Pokud velikost třídy jsou povolené na soubor Unified Storyboard, jsou dostupné následující adaptivní Segue typy (které odpovídají nového zobrazení řadiče rozhraní API popsané výše): **zobrazit** a **zobrazit detaily** .

> [!IMPORTANT]
> **Poznámka:**: když velikost třídy jsou povolené, jakékoli existující segues bude možné převést na nové typy.

Trvat příklad iOS 8 aplikace, která používá scénáře se Unified s řadičem rozdělení zobrazení, který má jednoduché herní navigační nabídky v zobrazení předlohy. Pokud uživatel klikne na tlačítko nabídky, řadiče zobrazení vybrané položky se mají v sekci podrobností řadiče zobrazení rozdělení při spuštění v zařízení iPad. V zařízení iPhone by měl být řadiče zobrazení položky vloženy do zásobníku navigace.

K dosažení tohoto efektu, v iOS Designer ovládacího prvku, klikněte na tlačítko a přetáhněte řádek View Controller, který se má zobrazit. Po uvolnění tlačítka myši, vyberte `Show Detail` z Segue místní typu:

 [![](unified-storyboards-images/segue01.png "Vyberte zobrazení podrobností Segue typ místní nabídky")](unified-storyboards-images/segue01.png#lightbox)

Vytvoří se nový segue mezi tlačítko a řadiče zobrazení. Nyní spusťte aplikaci v simulátoru iPhone a zobrazí se v hlavní nabídce:

 [![](unified-storyboards-images/segue02.png "Hlavní nabídky")](unified-storyboards-images/segue02.png#lightbox)

Klikněte na **vyberte herní** tlačítko a řadiče zobrazení položky se vloží do zásobníku navigační:

 [![](unified-storyboards-images/segue03.png "Položky řadiče zobrazení se vloží do zásobníku navigace, jak je znázorněno")](unified-storyboards-images/segue03.png#lightbox)

Zastavte iPhone simulátoru a spusťte aplikaci v simulátoru iPad. Přepnout na orientaci na šířku a hlavní nabídky se znovu zobrazí:

 [![](unified-storyboards-images/segue04.png "V hlavní nabídce Zobrazit")](unified-storyboards-images/segue04.png#lightbox)

Znovu, klikněte na **vyberte herní** tlačítko a řadiče zobrazení položky se zobrazí v části Podrobnosti řadiče zobrazení rozdělení:

 [![](unified-storyboards-images/segue05.png "Položky řadiče zobrazení uvedené v části Podrobnosti o řadiče zobrazení rozdělení")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>S výjimkou Element od třídy, velikost

Existují situace, kdy se nevyžaduje v určité třídy velikost daného elementu (například zobrazení, řízení nebo omezení). Vyloučit element od třídy, velikost, vyberte požadovanou položku k vyloučení z **návrhová plocha**. Přejděte do dolní části **vlastnost Explorer** a klikněte na tlačítko **ozubené kolečko** rozevírací nabídce. Vyberte kombinace **šířka** a **výška** chcete vyloučit položky od:

[![](unified-storyboards-images/exclude-a.png "Vyberte kombinace šířky a výšky")](unified-storyboards-images/exclude-a.png#lightbox)

Nový *vyloučení případ* se zařadí do element v dolní části **Explorer vlastnost**. Další, zrušte zaškrtnutí políčka **nainstalovaná** zaškrtávací políčko pro danou třídu velikost:

[![](unified-storyboards-images/exclude-b.png "Zrušte zaškrtnutí políčka nainstalovaná")](unified-storyboards-images/exclude-b.png#lightbox)

Šířka a výška, které byla položka vyloučené z přepnout na návrhovou plochu, byl odebrán z dané třídy velikost, ale ne celý návrh uživatelského rozhraní:

 [![](unified-storyboards-images/exclude02.png "Přepnout na návrhovou plochu na šířku a výšku položky vyloučil z")](unified-storyboards-images/exclude02.png#lightbox)

Přepnout zpět a třída velikost Any Width/Any Výška elementu je stále navázaný:

 [![](unified-storyboards-images/exclude03.png "Přepnout zpět k třídě Any Width/Any výšku velikosti")](unified-storyboards-images/exclude03.png#lightbox)

Když aplikace běží v simulátoru iPad, zobrazí se element:

 [![](unified-storyboards-images/exclude04.png "Element při spuštěné aplikaci v simulátoru iPad")](unified-storyboards-images/exclude04.png#lightbox)

A když se aplikace spustí na zařízení iPhone simulátoru, nebyl nalezen element:

 [![](unified-storyboards-images/exclude05.png "Element chybí při spuštěné aplikaci v simulátoru iPhone")](unified-storyboards-images/exclude05.png#lightbox)

K odebrání případu vyloučení z elementu, jednoduše vyberte požadovaný prvek v **návrhová plocha**, přejděte do dolní části **vlastnost Explorer** a klikněte na tlačítko  **-** tlačítko vedle případ odebrat.

Informace o implementaci Unified scénářů, podívejte se na `UnifiedStoryboard` ukázkové aplikace Xamarin iOS 8 připojené k tomuto dokumentu.

## <a name="dynamic-launch-screens"></a>Dynamické spouštěcí obrazovky

Spusťte soubor obrazovky se zobrazí úvodní obrazovka při k poskytnutí zpětné vazby uživateli, které aplikace je ve skutečnosti spouštění je spuštění aplikace pro iOS. Před iOS 8, by měl zahrnovat několik Vývojář `Default.png` prostředků pro každé zařízení typu, orientaci a rozlišení obrazovky aplikace by se systémem na bitovou kopii. Například `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`atd.

Řešení v nové iPhone 6 a iPhone 6 Plus zařízení (a nadcházející Apple Watch) se všemi existující iPhone a iPad zařízení reprezentuje velkou řadu různých velikostí, orientace a řešení z `Default.png` spuštění obrazovky image prostředky, které musí možné vytvořit a spravovat. Kromě toho tyto soubory mohou být značně velké a bude "nafouknutí" sady dodávky aplikace zvýšení množství dobu potřebnou pro stažení aplikace z iTunes App Storu (pravděpodobně udržuje ho nebudou moct doručena přes mobilní síť) a zvýšit velikost úložiště na zařízení koncového uživatele vyžaduje.

Nové do systému iOS 8, můžete vytvořit vývojář jedinou atomic `.xib` souboru v Xcode, kterou použije automatické rozložení a velikost třídy pro vytvoření *dynamické spusťte obrazovky* , bude fungovat pro každé zařízení, řešení a orientace. To nejen snižuje množství práce potřebné vývojáře pro vytváření a údržbu všechny prostředky požadované image, ale to významně snižuje velikost nainstalované sady aplikace.


Dynamické spusťte obrazovky mají následující omezení a důležité informace:

 - Používat jenom `UIKit` třídy.
 - Použít jeden kořenový zobrazení, které je `UIView` nebo `UIViewController` objektu.
 - Nevytvářejte všechna připojení k kódu aplikace (nepřidáte **akce** nebo **výstupy**).
 - Nepřidáte `UIWebView` objekty.
 - Nepoužívejte všechny vlastní třídy.
 - Nepoužívejte atributy modulu runtime.

Pomocí výše uvedených pokynů na paměti Podíváme se na přidání dynamické spusťte obrazovky do existujícího projektu Xamarin iOS 8.

Postupujte takto:

1. Otevřete **Visual Studio pro Mac** a zatížení **řešení** přidat dynamické spusťte obrazovky.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši `MainStoryboard.storyboard` soubor a vyberte **otevřít v** > **Xcode rozhraní tvůrce**:

    [![](unified-storyboards-images/dls01.png "Otevřít v aplikaci Tvůrce Xcode rozhraní")](unified-storyboards-images/dls01.png#lightbox)
3. V Xcode, vyberte **soubor** > **nový** > **souboru...** :

    [![](unified-storyboards-images/dls02.png "Vyberte soubor / New")](unified-storyboards-images/dls02.png#lightbox)
4. Vyberte **iOS** > **uživatelské rozhraní** > **spusťte obrazovky** a klikněte na tlačítko **Další** tlačítko:

    [![](unified-storyboards-images/dls03.png "Vyberte iOS / uživatelské rozhraní nebo spusťte obrazovky")](unified-storyboards-images/dls03.png#lightbox)
5. Název souboru `LaunchScreen.xib` a klikněte na **vytvořit** tlačítko:

    [![](unified-storyboards-images/dls04.png "Název souboru LaunchScreen.xib")](unified-storyboards-images/dls04.png#lightbox)
6. Návrh úvodní obrazovka upravte přidáním grafické elementy a umístit je pro dané zařízení, orientace a velikost obrazovky pomocí rozložení omezení:

    [![](unified-storyboards-images/dls05.png "Úpravy návrh úvodní obrazovka")](unified-storyboards-images/dls05.png#lightbox)
7. Uložit změny do `LaunchScreen.xib`.
8. Vyberte **cíl aplikací** a **Obecné** karty:

    [![](unified-storyboards-images/dls06.png "Vyberte cíl aplikací a karta Obecné")](unified-storyboards-images/dls06.png#lightbox)
9. Klikněte na tlačítko **zvolte Info.plist** tlačítko, vyberte `Info.plist` pro aplikace Xamarin a klikněte na tlačítko **zvolte** tlačítko:

    [![](unified-storyboards-images/dls07.png "Vyberte Info.plist pro aplikace Xamarin")](unified-storyboards-images/dls07.png#lightbox)
10. V **ikony aplikace a spuštění bitové kopie** část, otevřete **spusťte soubor obrazovky** rozevírací seznam a vyberte `LaunchScreen.xib` vytvořili výše:

    [![](unified-storyboards-images/dls08.png "Vyberte LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. Uložte změny do souboru a vrátit k sadě Visual Studio for Mac.
12. Počkejte, než pro sadu Visual Studio pro Mac na dokončení synchronizace změn pomocí Xcode.
13. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **prostředků** složky a vyberte **přidat** > **přidat soubory...** :

    [![](unified-storyboards-images/dls09.png "Vyberte možnost Přidat nebo přidání souborů...")](unified-storyboards-images/dls09.png#lightbox)
14. Vyberte `LaunchScreen.xib` vytvořili výše, klikněte na **otevřete** tlačítko:

    [![](unified-storyboards-images/dls10.png "Vyberte soubor LaunchScreen.xib")](unified-storyboards-images/dls10.png#lightbox)
15. Sestavení aplikace.

### <a name="testing-the-dynamic-launch-screen"></a>Testování dynamické úvodní obrazovka

V sadě Visual Studio pro Mac vyberte simulátoru sítnice iPhone 4 a spusťte aplikaci. Dynamické spusťte obrazovky se zobrazí ve správném formátu a orientaci:

[![](unified-storyboards-images/dls11.png "Dynamické spusťte obrazovce zobrazen ve svislém orientaci")](unified-storyboards-images/dls11.png#lightbox)

Zastavte aplikaci v sadě Visual Studio pro Mac a vyberte zařízení s iOS 8 iPad. Spusťte aplikaci a úvodní obrazovka bude správně naformátován pro toto zařízení a orientaci:

[![](unified-storyboards-images/dls12.png "Dynamické spusťte obrazovce zobrazen v vodorovné orientaci")](unified-storyboards-images/dls12.png#lightbox)

Vraťte se na Visual Studio pro Mac a zastavit spuštění aplikace.

### <a name="working-with-ios-7"></a>Práce s iOS 7

K zachování zpětné kompatibility s iOS 7, zahrnout pouze obvyklé `Default.png` bitové kopie prostředky jako normální v aplikaci iOS 8. iOS se vrátit k předchozí chování a použijte tyto soubory jako úvodní obrazovky při spuštění v zařízení s iOS 7.

Informace o implementaci dynamické spusťte obrazovky v Xamarin, podívejte se na [dynamické spusťte obrazovky](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) ukázkové aplikace iOS 8, které jsou připojené k tomuto dokumentu.

## <a name="summary"></a>Souhrn

Tento článek trvalo rychlý pohled na velikost třídy a jejich vliv na rozložení v zařízení iPhone a iPad. Je popsané, jak fungují s třídami velikost vytvořit jednotné rozhraní vlastnosti, znak prostředí a kolekce znak. Trvalo Stručný pohled na adaptivní řadiče zobrazení a jak pracují s třídami velikost uvnitř jednotné rozhraní. Ho prohlédli implementace velikost třídy a rozhraní Unified zcela z kódu jazyka C# do aplikace Xamarin iOS 8.

Nakonec v tomto článku, na něž se základy vytváření Unified scénářů s Xamarin iOS Designer, který bude fungovat na všech zařízeních s iOS a vytvoření jedinou dynamické spusťte obrazovky, který se zobrazí jako úvodní obrazovky na každé zařízení s iOS 8.

## <a name="related-links"></a>Související odkazy

- [Adaptivní fotografie (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [Ukázka StoryboardIntro](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [Dynamické spusťte obrazovky (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Úvod do iOSu 8](~/ios/platform/introduction-to-ios8.md)
- [Dynamické rozložení v iOS8 - momentální 2014 (video)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
