---
title: "Práce s ikony a obrázků"
description: "Tento článek se zabývá navrhování a práce s ikony a bitové kopie v rámci Xamarin.tvOS aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d1052695bb7337a18d1a2f1f7015e9079f86f6f5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-icons-and-images"></a>Práce s ikony a obrázků

_Tento článek se zabývá navrhování a práce s ikony a bitové kopie v rámci Xamarin.tvOS aplikace._

Vytváření poutavé ikony a obrazů jsou důležitou součástí vývoj dokonalé uživatelské prostředí pro aplikace pro Apple TV. Tato příručka popisuje kroky potřebné k vytvoření a zahrnutí nezbytné grafické prostředky pro vaše aplikace Xamarin.tvOS:

- [Spuštění bitové kopie](#Launch-Image) -spouštěcí image se zobrazí při prvním spuštění aplikace a je nahrazena první obrazovce aplikace po dokončení spuštění.
- [Na základě bitové kopie](#Layered-Images) – specifické pro Apple TV, společnosti Apple nové práci na základě bitové kopie s paralaxy účinek vytvořit 3D efekt pro vybrané položky. Existuje několik způsobů, jak [vytvořit na základě bitové kopie](#Creating-Layered-Images).
- [Ikona aplikace](#App-Icons) -ikony jsou požadovány pro obrazovky nejen televizního Apple Domů, ale do obchodu App Store. Musí být zadán jako obrázek na vrstvu.
- [TOP police Image](#Top-Shelf-Image) – Pokud je vaše aplikace je umístěn na horní řádek domovské obrazovce, je nutné bitovou kopii horní police s přehledem funkcí vaší aplikace. Volitelně můžete zadat [dynamický obsah police horní](#Dynamic-Top-Shelf-Content) Zvýrazněte obsah ve vaší aplikaci.
- [Her Center image](#Game-Center-Images) -když vaše aplikace je hry a používá herní centrum, několik dalších Image bude vyžadovat.
- [Nastavení obrázků projektu Xamarin.tvOS](#Setting-Xamarin.tvOS-Project-Images) -popisuje kroky potřebné ke spuštění bitové kopie a ikona aplikace pro vaši aplikaci Xamarin.tvOS.

> [!IMPORTANT]
> **Poznámka:** všechny bitové kopie na Apple TV jsou v řešení 1 x (`@1x`) a měli byste _pouze_ pomocí bitových kopií této velikosti. Včetně větší, vyšší rozlišení grafiky nejen časově stáhnout a použít více paměti a velikost úložiště, ale musí být dynamicky změní měřítko obrázku za běhu a bude mít negativní vliv kreslení výkon.

<a name="Launch-Image" />

## <a name="launch-image"></a>Spuštění bitové kopie

Spusťte bitová kopie je první věcí, které se zobrazí při prvním spuštění aplikace Xamarin.tvOS na Apple TV, a jako takový každé tvOS aplikace musíte zadat bitovou kopii spustit. 

Spuštění bitové kopie se zobrazí rychle a vyvolává dojem, že aplikace je rychlý a reaguje. Apple TV nahradí bitovou kopii spustit na první obrazovce aplikace krátce došlo po.

Spouštěcí bitové kopie nejsou příležitost, reklamy nebo uměleckého výraz, existují pouze k vyvolají dojem, že vaše aplikace spustí rychle a je připravený k použití.

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>Spuštění bitové kopie</b></td>
</tr>
<tr>
    <td><b>Velikost</b></td>
    <td>1920px x 1080px

    Non-layered `.png` files only</td>
</tr>
</table>

Apple umožňuje následující návrhy pro návrh obrázek spuštění vaší aplikace:

- **Téměř identickou na první obrazovce** -vaše obrazovky spuštění by měl být co nejblíže vaší aplikace první obrazovce co možná. Včetně různých grafiky nebo element může vést obtěžování "flash", když se zobrazí na první obrazovce.
- **Vyhněte se použití Text** -spuštění bitové kopie jsou statické a jako takový, nebude možné lokalizovat před zobrazením.
- **Downplay spuštění** -protože Apple TV uživatelé často přepínat aplikace, by neměl upoutat pozornost do procesu spuštění aplikace.
- **Žádné reklamy nebo Branding** – vaše Image spusťte by se neměla používat jako obrazovce o nebo zahrnout všechny branding, pokud je statický součástí první obrazovce vaší aplikace. Výhradně nesmějí reklamy.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>Nastavení spouštěcí bitové kopie

Pokud chcete nastavit pro bitovou kopii spusťte projekt tvOS, postupujte podle následujících kroků:

1. V **Průzkumníku řešení**, dvakrát klikněte na `Assets.xcassets` otevřete pro úpravy: 

    [![](icons-images-images/asset01.png "Soubor Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. V **Asset Editor**, klikněte na `LaunchImages` asset: 

    [![](icons-images-images/asset02.png "LaunchImages asset")](icons-images-images/asset02.png#lightbox)
3. Klikněte na **1 x Apple TV** položku a vyberte bitovou kopii spustit nebo volitelně přetáhněte novou bitovou kopii ze systému souborů: 

    [![](icons-images-images/asset03.png "Vybrat bitovou spouštěcí kopii")](icons-images-images/asset03.png#lightbox)
4. Uložte provedené změny.

<a name="Layered-Images" />

## <a name="layered-images"></a>Vrstvený bitové kopie

Nový Apple TV, pracují na základě bitové kopie s paralaxy účinek účelem 3D efektu, která pomáhá chránit uživatele na pohovce duševně připojený k obsahu na obrazovce v místnosti.

Vrstvený Image obsahují z dva (2) do pěti (5) samostatných vrstev, které společně tvoří úplnou bitovou kopii. S výjimkou vrstva pozadí každou vrstvu používá jeho pořadí Z-order společně s průhlednost k vytvoření dojem hloubky. Když uživatel pracuje s bitovou kopii vrstvu, jsou vyšší seřazené Z vrstvy škálovat a překryté k vytvoření této vliv.

[![](icons-images-images/layered01.png "Diagram vrstev seřazené Z bitové kopie")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> **Poznámka:** Layered Image jsou požadovány pro ikony vaší aplikace a jsou volitelné pro ostatní [může získat fokus položky](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (například obrázek police Top). Apple navrhuje pomocí na základě bitové kopie pro žádný obrázek, který můžete získat fokus ve vaší aplikaci.




Apple umožňuje následující návrhy pro návrh obrázků na základě:

- **Ujistěte se, neprůhledné vrstva pozadí** -vrstvě pozadí (vrstva 1) **musí** být neprůhledné nebo budete dojde k chybě při pokusu použít bitovou kopii na základě na Apple TV. Všechny ostatní vrstvy může obsahovat několik úrovní průhlednosti k vylepšení 3D účinek.
- **Izolovat popředí, střední a pozadí elementy** -umístit viditelného elementy (například herní znaků) v popředí, použijte prostřední pro sekundární elementy nebo stínů. Nakonec zahrnovat neutrální pozadí zajistit úsek pro vyšší vrstvy.
- **Zachovat Text v popředí** – Pokud chcete, aby vaše text, který má být zakrytý vyšší úrovně, obecně by mělo být na nejvyšší vrstvě.
- **Použít jednoduchý vrstvení** -The vliv paralaxy byla navržená tak, aby se jemně tak zachovat vrstev s minimálním aby jarring, reálné účinky.
- **Zahrnout zónu bezpečné** -protože během paralaxy vliv můžete oříznuty horní vrstvy, které potřebujete k vytvoření ohraničení bezpečné zóny do každé vrstvě. Pokud se zobrazí obsah příliš zavřít edge vrstvy, se stane oříznout vypnout. Horní vrstvy, budou mít další škálování a oříznutí než nižších vrstvách. Najdete v článku [nastavení velikosti vrstvy obrazu](#Sizing-Image-Layers) části níže.
- **Náhled nejčastěji** – na základě bitové kopie musí často zajistit, že požadovaný efekt 3D dochází, a že neobsahuje žádný obsah v jednotlivých vrstvách je se vyhnete náhled zobrazit. Je vhodné prohlédnout na základě bitové kopie na skutečné Apple TV hardwaru a ujistěte se, že vykreslují podle očekávání.

Pokud je to možné, byste měli vždycky používat integrované `UIKit` ovládacích prvků pro zobrazení obrázků vrstvu, jak bude automaticky získají účinek paralaxy když se do fokus.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>Nastavení velikosti obrázku vrstev

Je důležité si nezapomeňte zahrnout _bezpečné zóny_ ohraničení do každou vrstvu, která bude tvoří vrstvu Image. Protože jednotlivých vrstvách můžete škálovat a oříznout během paralaxy účinek, můžete obsah vrstvy oříznout vypnout, pokud je příliš zavřete okraj vrstvy:

[![](icons-images-images/layered02.png "ohraničení 35 pixelů")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>Vytváření na základě bitové kopie

tvOS funguje s vrstvami obrázky v následujících formátech:

- **Soubory CAR** – jedná se o proprietární formát katalog Asset vytvořit společností Apple. Auto soubory nelze vytvořit přímo, jsou vytvořeny v době kompilace z jakékoli LSR souborů a součástí vaší sady prostředků aplikace.
- **Obrázky LSR** – to je formát proprietární bitové kopie vytvořené Apple. Použití [paralaxy modul plug-in aplikace Photoshop Adobe exportu](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) nebo [paralaxy Náhled](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) k vytvoření a zobrazení náhledu na základě bitové kopie ve formátu LSR.
- **Assets.xcassets** – z dva (2) do pěti (5) standardní `.png` formátovaný Image obsažený v katalog Asset, který se zkompiluje do AUTA nebo LSR formátu na základě bitové kopie v době kompilace.
- **Soubory LCR** – to je formát souboru proprietární vytvořené Apple. LCR soubory jsou určeny k použití jako další obsah stáhnout z jednoho z vašich serverů obsahu. LCR soubor by měl být nikdy součástí vaší sady prostředků aplikace. Soubory LCR se generují z LSR nebo Photoshop soubory pomocí `layerutil` příkazového řádku nástroje dodávaná s Xcode.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>Náhled paralaxy dokumentu

Apple vytvořili [paralaxy Náhled](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) preview a vytvořený na základě bitové kopie potřebné pro ikon aplikace a může získat fokus nepovinných položek. Náhledu zobrazí všechny vrstvy, která tvoří dokončené bitové kopie na základě:

[![](icons-images-images/layered03.png "Náhled paralaxy dokumentu")](icons-images-images/layered03.png#lightbox)

Při zobrazení náhledu bitovou kopii vrstvu, můžete pomocí myši otočit bitovou kopii a náhled paralaxy účinek. Použití  **+**  (plus) a  **-**  (tlačítka přidávat a odebírat vrstvy minus).

Když vytváříte novou bitovou kopii vrstvu, může být exportován ve formátu LSR a součástí sady vaší aplikace.

Další informace o vytváření a zobrazování náhledu na základě bitové kopie, najdete v tématu společnosti Apple [vytváření kresby paralaxy](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) části [aplikace Průvodce programováním pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/).

<a name="App-Icons" />

## <a name="app-icons"></a>Ikony aplikace

Aplikace Xamarin.tvOS bude vyžadovat nejen ikony aplikace pro Apple TV Domů obrazovky, ale také ikonu pro obchod s aplikacemi. Ikona aplikace je první změňte zajistit skvělé dojem na potenciální uživatelů a měli komunikovat účel vaší aplikace na první pohled.

[![](icons-images-images/icon01.png "Ikona aplikace")](icons-images-images/icon01.png#lightbox)

Každé aplikace musíte zadat malá a velká verzi jeho ikona aplikace. Malé ikony se použije na obrazovce Apple TV Domů, když je aplikace nainstalovaná. Velké verze je používána obchodu s aplikacemi. Velké ikony aplikace by měl napodobovat vzhledu a chování verze malé ikony.

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>Malé ikony</b></td>
    <td colspan="2"><b>Velkých ikon.</b></td>
</tr>
<tr>
    <td><b>Skutečná velikost</b></td>
    <td>400px x 240px</td>
    <td><b>Velikost</b></td>
    <td>1280px x 768px</td>
</tr>
<tr>
    <td><b>Velikost bezpečné zóny</b></td>
    <td>370px x 222px</td>
    <td></td>
    <td></td>
</tr>
<tr>
    <td><b>Nezaostřená velikost</b></td>
    <td>300px x 180px</td>
    <td></td>
    <td></td>
</tr>
<tr>
    <td><b>Cílených velikost</b></td>
    <td>370px x 222px</td>
    <td></td>
    <td></td>
</tr>
</table>

> [!IMPORTANT]
> **Poznámka:** ikony aplikace musí být uvedeny jako **na základě bitové kopie**. Najdete v tématu [na základě bitové kopie](#Layered-Images) části výše další podrobnosti.




Apple poskytuje následující návrhy pro vytvoření ikony aplikace:

- **Nabízí bod fokus jedním** – návrh vaší ikona s bodem jeden fokus umístit přímo v centru ikonu.
- **Použít jednoduchý pozadí** – zjednodušení ikonu pozadí tak, aby se horní vrstvy zvýraznění. Zvažte použití jednoduchého barva nebo jemně přechod.
- **Omezit velikost textu** – vzhledem k tomu, že název aplikace se zobrazí pod ikonou, když je vybraný uživatelem, by měla pouze zahrnete textu je nezbytné pro návrh ikonu.
- **Nepoužívejte snímky obrazovky** – snímky obrazovky jsou příliš složité pro ikonu a Nepovolit uživatelům zobrazit účel vaší aplikace na první pohled.
- **Udržování ikony hranaté** – tvOS automaticky použije masku trochu zaokrouhlí rozích ikony. Neobsahují tento zaokrouhlení sami.
- **Pečlivě si vrstvy oddělení** – Text by měl být v horní většina vrstvy, sekundární položky ve středu a neutrální pozadí, která umožňuje vaší horní vrstev a Vylepšete.
- **Použít přechody a pečlivě stínů** – přechody a stínů může kolidovat s paralaxy účinek, je třeba používat opatrně. Jednoduché horní shora dolů, přechodu styly light tmavý nejlépe fungovat. Stínů obvykle nejlépe jako sharp, s ostrými odstíny fungovat.
- **Lišit průhlednost vrstvy** – použití různých úrovní průhlednosti na vyšší úrovně ikony aplikace zvýšit 3D účinek. Vrstva pozadí musí být neprůhledné jinak bude výsledkem chyba.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>Nastavení ikony aplikace

Pokud chcete nastavit ikony aplikace vyžaduje pro svůj projekt tvOS, postupujte podle následujících kroků:

1. V **Průzkumníku řešení**, dvakrát klikněte na `Assets.xcassets` otevřete pro úpravy: 

    [![](icons-images-images/asset01.png "Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. V **Asset Editor**, rozbalte `App Icon & Top Shelf Image` asset: 

    [![](icons-images-images/asset04.png "Rozbalte asset horní police Image")](icons-images-images/asset04.png#lightbox)
3. Potom rozbalte položku `App Icon - Small` asset: 

    [![](icons-images-images/asset05.png "Rozbalte ikona aplikace – malé asset")](icons-images-images/asset05.png#lightbox)
4. Potom rozbalte `Back` asset a klikněte na `Contents` položku: 

    [![](icons-images-images/asset06.png "Potom rozbalte asset zpět")](icons-images-images/asset06.png#lightbox)
5. Klikněte na **1 x Apple TV položka** a vyberte soubor obrazu.
6. Zopakujte výše uvedené kroky pro `Front` a `Middle` prostředky.
7. Opakujte stejný postup můžete definovat `App Icon - Large` asset.
4. Uložte provedené změny.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>Horní police Image

Pokud uživatel má umístit aplikace Xamarin.tvOS na začátek řádku na obrazovce Apple TV Domů, velký obrázek police horní zobrazí, pokud vaše aplikace je vybraná uživatelem. Tuto bitovou kopii měli zvýrazněte funkcí aplikace nebo zadejte přímé odkazy na jeho obsah.

[![](icons-images-images/topshelf01.png "Horní obrázek police – příklad")](icons-images-images/topshelf01.png#lightbox)

Bitovou kopii police horní lze zadat buď jako jednu statickou `.png` nebo `.lsr` souboru (v tématu [vytváření bitových kopií vrstvený](#Creating-Layered-Images)) nebo ji dynamicky vytvoří za běhu jako jeden řádek může získat fokus položek (najdete v části [ Dynamické nejvyšší police obsah](#Dynamic-Top-Shelf-Content) níže).

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>Horní police Image</b></td>
</tr>
<tr>
    <td><b>Velikost</b></td>
    <td>1920px x 720px

    Static `.png` or layered `.lsr` file</td>
</tr>
</table>

Apple poskytuje následující návrhy pro vytváření obrázků police horní:

- **Použití statické obrazů bohaté** – Pokud vaše aplikace neposkytuje dynamický obsah, bude jeho horní police Image bez může získat fokus. Použijte tuto bitovou kopii s přehledem funkcí aplikace nebo vaší značkou.
- **Odkaz na obsah aplikace** – dynamické rozložení police horní zadejte rychlý odkaz na obsah, který vyhledá uživatele nejdůležitější ve vaší aplikaci. Pomocí této oblasti poskytovat rychlý odkaz a spusťte aplikaci okamžitě přejít do daný obsah.
- **Prezentují nejnovější obsah** – bohaté horní police obsahu můžete kreslení uživatele do vaší aplikace a aby byly pro další použití. Použijte jako oblast pro prezentaci nejvyšší hodnocení nebo nejnovější obsah.
- **Přizpůsobené obsah** – oblíbených aplikací v horní části řádku domovské obrazovce nebo jejich nejčastěji používané místní uživatelé. Pomocí police horní můžete zobrazit obsah, který by se použily nejvíc zajímat.
- **Služby Active Directory není povoleno** – reklamy výhradně nesmějí se zobrazí v horní části police. Může zobrazit nejnovější nabízené ke koupi obsah, ale mají být zobrazeny žádné informace o cenách.

### <a name="setting-the-top-shelf-image"></a>Nastavení nejvyšší police bitové kopie

Pokud chcete nastavit horní obrázek police požadované pro svůj projekt tvOS, postupujte podle následujících kroků:

1. V **Průzkumníku řešení**, dvakrát klikněte na `Assets.xcassets` otevřete pro úpravy: 

    [![](icons-images-images/asset01.png "Soubor Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. V **Asset Editor**, rozbalte `App Icon & Top Shelf Image` asset: 

    [![](icons-images-images/asset04.png "Rozbalte asset horní police Image")](icons-images-images/asset04.png#lightbox)
3. Klikněte na `Top Shelf Image` asset: 

    [![](icons-images-images/asset07.png "Zdroj obrázku police horní")](icons-images-images/asset07.png#lightbox)
5. Klikněte na **1 x Apple TV položka** a vyberte soubor obrazu.
6. Uložte provedené změny.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>Dynamické nejvyšší police obsahu

Místo zobrazení statický obrázek police Top, může obsahovat police horní dynamické řádek [může získat fokus položky](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) nebo dynamické sadu posouvání hlaviček. Obě tyto dynamické styl umožňují Zvýrazněte obsah poskytl aplikace nebo přechod do jeho nejpoužívanější funkce.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>Rozdělenou řádek obsahu

Tento typ dynamického horní police obsahu představuje jeden řádek posouvání, může získat fokus položky volitelně rozdělena na oddíly. Ho se obvykle používá k nové, zvýrazněte oblíbených nebo nedávno zobrazit obsah aplikace.

Obsah je zobrazen jako jeden a vodorovného posouvání seznamu obsahu s popiskem pod aktuální část vybraného obsahu (který má právě fokus). Pokud uživatel vybere určitou část obsahu, spustí se vaše aplikace a musí být odebrány přímo do tohoto obsahu.

Následující velikosti obsahu se bude vyžadovat:

<table width="100%" border="1px">
<tr>
    <td><b>&nbsp;</b></td>
    <td><b>Plakát (2:3)</b></td>
    <td><b>Hranaté (1:1)</b></td>
    <td><b>HDTV (16:9)</b></td>
</tr>
<tr>
    <td><b>Skutečná velikost</b></td>
    <td>404px x 608px</td>
    <td>608px x 608px</td>
    <td>908px x 512px</td>
</tr>
<tr>
    <td><b>Velikost bezpečné zóny</b></td>
    <td>380px x 570px</td>
    <td>570px x 570px</td>
    <td>852px x 479px</td>
</tr>
<tr>
    <td><b>Nezaostřená velikost</b></td>
    <td>333px x 500px</td>
    <td>500px x 500px</td>
    <td>782px x 440px</td>
</tr>
<tr>
    <td><b>Cílených velikost</b></td>
    <td>380px x 570px</td>
    <td>570px x 570px</td>
    <td>852px x 479px</td>
</tr>
</table>

Apple poskytuje následující návrhy pro rozdělenou obsahu řádek:

- **Dokončení řádek** – by měl poskytovat dostatek obsahu k celou šířku obrazovky.
- **Změna měřítka obrázků smíšený** – The rozdělenou obsahu řádek byl navržený tak, aby udržení směs velikosti obrázků (ze seznamu výše uvedeného). Pokud ale použijete zároveň velikosti obrázků, mějte na paměti, že další škálování se použije k normalizaci zobrazení obsahu.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>Posouvání Inset hlaviček

Volitelně může být aplikace Xamarin.tvOS jeho obsah v horní části police jako automaticky posouvání a opakování ve smyčce kolekci hlaviček, které téměř celou obrazovku. Tento styl se obvykle používá k prezentují bohatou a nový obsah jako nový televizní pořady.

Kromě automatické posouvání, může uživatel převzít kontrolu nad do hlaviček a posuňte se v obou směrech pomocí vzdáleného Siri. Provedení malá, cyklické gesto na vzdáleném Siri, když banner se nezvýrazní aktivují účinek paralaxy pro tento informační zpráva.

<table width="100%" border="1px">
<tr>
    <td colspan="2"><b>Obrázek hlavičky (navíc široký)</b></td>
</tr>
<tr>
    <td><b>Skutečná velikost</b></td>
    <td>1940px x 624px</td>
</tr>
<tr>
    <td><b>Velikost bezpečné zóny</b></td>
    <td>1740px x 620px</td>
</tr>
<tr>
    <td><b>Nezaostřená velikost</b></td>
    <td>1740px x 560px</td>
</tr>
<tr>
    <td><b>Cílených velikost</b></td>
    <td>1740px x 620px</td>
</tr>
</table>

Posouvání Inset hlaviček můžete buď zadat jako statického `.png` nebo vrstvený `.lsr` souboru.

Apple poskytuje následující návrhy pro Inset hlaviček posouvání:

- **Velikost obsahu** -minimálně hlaviček tří (3) by měl poskytovat pro posouvání působí přirozené. By měla obsahovat maximálně 8 (osm) hlaviček, případně ho zajistěte navigační pevný pro koncového uživatele.
- **Obsah textu** – Pokud vaše banner vyžaduje textu v by měl být součástí bitovou kopii informační zpráva. Pokud používáte vrstvený bitové kopie, vaše text by měl být v nejvyšší vrstvě.

Najdete v tématu společnosti Apple [TVServices Framework – referenční informace](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) pro další informace o přidání příponu nejvyšší police do vaší aplikace pro poskytování dynamické horní police obsahu.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Obrázky herní Centrum

Pokud je vaše aplikace Xamarin.tvOS hry a jste zahrnuli herním centru podpory, se bude vyžadovat několik další prostředky bitové kopie:

- **Ikony dosažení** -neprůhledného bitové kopie je vyžadována pro každý úspěchu, který budou automaticky oříznuty do kruh. Herní bonusy jsou bez může získat fokus položky.
- **Řídicí panel kresby** -volitelné bitové kopie může být zadaný, který se zobrazí v horní části řídicího panelu vaší aplikace v herním centru. Tyto Image jsou bez může získat fokus.
- **Kresby žebříček** -je nutné zadat mezi jedna (1) do tří (3) poměr stran 16:9 bitových kopií pro každý žebříček, který podporuje aplikace. To může být buď statické `.png` nebo vrstvený `.lsr` soubory. Kresby žebříček je může získat fokus.

<table width="100%" border="1px">
<tr>
    <td><b>&nbsp;</b></td>
    <td><b>Dosažení ikony</b></td>
    <td><b>Řídicí panel kresby</b></td>
    <td><b>Žebříček kresby</b></td>
</tr>
<tr>
    <td><b>Zobrazené velikosti</b></td>
    <td>200px x 200px</td>
    <td>923px x 150px</td>
    <td>není k dispozici</td>
</tr>
<tr>
    <td><b>Skutečná velikost</b></td>
    <td>320px x 320px</td>
    <td>není k dispozici</td>
    <td>659px x 371px</td>
</tr>
<tr>
    <td><b>Velikost bezpečné zóny</b></td>
    <td>není k dispozici</td>
    <td>není k dispozici</td>
    <td>618px x 348px</td>
</tr>
<tr>
    <td><b>Nezaostřená velikost</b></td>
    <td>není k dispozici</td>
    <td>není k dispozici</td>
    <td>548px x 309px</td>
</tr>
<tr>
    <td><b>Cílených velikost</b></td>
    <td>není k dispozici</td>
    <td>není k dispozici</td>
    <td>618px x 348px</td>
</tr>
</table>

Další informace o práci s herním centru, najdete v tématu společnosti Apple [Průvodce programováním v herním centru](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html).

<a name="Working-with-Images" />

## <a name="working-with-images"></a>Práce s obrázky

Vzhledem k tomu, že tvOS 9 je podmnožinou iOS 9, se stejné techniky umožňuje zahrnout a zobrazení obrázků v aplikaci Xamarin.iOS, fungovat taky Xamarin.tvOS aplikace. Najdete v tématu naše [zobrazení bitovou kopii](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Další informace naleznete v dokumentaci.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Nastavení projektu Xamarin.tvOS obrázků

Jak jsme uvedli výše, vyžadují všechny aplikace tvOS [spuštění bitové kopie](#Launch-Image), a [ikonu aplikace](#App-Icons). Tato část obsahuje výběr spuštění bitové kopie a ikona aplikace projektu aplikace Xamarin.tvOS po jejich nastavení v katalog Asset.

Postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte `Info.plist` otevřete pro úpravy: 

    [![](icons-images-images/info01.png "Soubor Info.plist")](icons-images-images/info01.png#lightbox)
2. V **Info.Plist Editor**, vyberte katalog prostředků (nakonfigurované výše v [nastavení ikony aplikace](#Setting-the-App-Icons) část) pro **ikony aplikace**: 

    [![](icons-images-images/info02.png "Info.Plist Editor")](icons-images-images/info02.png#lightbox)
3. Potom vyberte katalog prostředků (nakonfigurované výše v [nastavení bitovou kopii spusťte](#Setting-the-Launch-Image) část) pro **spuštění bitové kopie**.
4. Uložte provedené změny.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnuté všechny typy obrázků a použít v aplikaci Xamarin.tvOS velikosti. Nejprve zahrnutých spuštění bitové kopie, na základě bitové kopie, ikony aplikace, horní police Image a Image herní centrum. Potom zahrnutých práce s obrázky v aplikaci Xamarin.tvOS.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
