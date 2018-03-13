---
title: "Vytváření prostředků pro různých obrazovky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: fcd77d97d492baee441cfd428e58ea83525f927e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="creating-resources-for-varying-screens"></a>Vytváření prostředků pro různých obrazovky

Android samotné běží na mnoha různých zařízení, každý s širokou škálu řešení, velikosti obrazovky a hustoty obrazovky. Android provede škálování a změna velikosti, aby vaše aplikace fungovat v těchto zařízeních, ale to může mít za následek neoptimálním průběhem uživatelské prostředí. Například může se zobrazit rozmazaně bitové kopie, bitové kopie může zabírat příliš mnoho (nebo není dostatek) obrazovky prostor, který spustí pozici prvků uživatelského rozhraní v rozložení bude překrývat nebo je příliš daleko od sebe.


## <a name="concepts"></a>Koncepty

Několik podmínky a Koncepty jsou důležité si uvědomit, pro podporu více obrazovky.

- **Velikost obrazovky** &ndash; množství fyzického místa na disku pro zobrazení vaší aplikace

- **Obrazovky hustotu** &ndash; počet pixelů v jakékoli dané oblasti na obrazovce. Typické Měrná jednotka služby je bodů na palec (dpi).

- **Řešení** &ndash; celkový počet pixelů na obrazovce. Při vývoji aplikace, není důležité jako velikost obrazovky a s velkou hustotou řešení.

- **Nezávislé na hustotě pixelů (dp)** &ndash; jde virtuální jednotku, která umožňují nezávislé hustotu rozložení třeba navrhnout. Převést distribučního bodu na obrazovce pixelů se používá následující vzorec:

    px &equals; dp &times; dpi &divide; 160

- **Orientace** &ndash; orientaci na obrazovce se považuje za na šířku, když je širší než vyšší. Orientace na výšku spočívá v tom, když je větší než šířka obrazovky. Orientaci můžete změnit během životního cyklu aplikace jako uživatel otočí zařízení.

Všimněte si, že první tři tyto koncepty souvisejí mezi &ndash; zvýšení řešení bez zvýšení hustoty zvýší velikost obrazovky. Ale pokud jsou vyšší hustotu i řešení pak velikost obrazovky může zůstat beze změny. Tento vztah mezi velikost, hustotu a rozlišení obrazovky zkomplikovat obrazovky podpora velmi rychle.

Pomoc při řešení této složitost, upřednostňuje použití rozhraní Android *nezávislé na hustotě pixelů (dp)* pro rozložení obrazovky. Pomocí nezávislé pixelů hustotu prvky uživatelského rozhraní se zobrazí uživateli, aby mít stejnou velikost fyzické na obrazovkách s jinou densities –.


## <a name="supporting-various-screen-sizes-and-densities"></a>Podpora různých velikost obrazovky a densities –

Android zpracovává většinu práce k vykreslení rozložení správně pro každou konfiguraci obrazovky. Existují však některé akce, které můžete provést na Pomozte systému.

Ve většině případů zajistit hustotu nezávislost stačí použití nezávislé na hustotě pixelů místo skutečného pixelů rozložení.
Android bude škálovat drawables za běhu na správnou velikost.
Je však možné, že tento škálování způsobí, že bitmap zobrazí rozmazaně. Abyste tomu předešli, může být nutné zadat alternativní zdroje pro různé densities –. Při navrhování zařízení pro více řešení a densities – obrazovky bude prokázat jednodušší začínat vyšší řešení nebo hustotu Image a pak vertikálně snížit kapacitu. To zabrání žádné stírá ani narušení, který může být důsledkem změnu velikosti.


### <a name="declare-the-screen-size-the-application-supports"></a>Deklarovat velikosti obrazovky aplikace podporuje

Deklarování velikosti obrazovky zajistí, že pouze podporovaných zařízení můžou stahovat aplikace. To se provádí nastavením [podporuje obrazovky](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) element v **AndroidManifest.xml** souboru. Tento element slouží k určení, jaké velikost obrazovky jsou podporovány v aplikaci. Daný obrazovky se považuje za podporovány, pokud aplikace můžete správně ho je rozložení k vyplnění obrazovky. Pomocí tohoto prvku manifestu aplikace se nezobrazí v [ *Google Play* ](https://play.google.com/) pro zařízení, která nesplňují specifikace obrazovky. Ale stále bude aplikace spuštěna v zařízeních s nepodporovanou obrazovky, ale může se rozmazaně rozložení a pixelován.

Chcete-li to provést v Xamarin.Android, je nutné nejprve přidat **AndroidManifest.xml** souboru do projektu, pokud ještě neexistuje:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Manifestu systému Android.](resources-for-varying-screens-images/01-android-manifest-vs-sml.png)](resources-for-varying-screens-images/01-android-manifest-vs.png#lightbox)

**AndroidManifest.xml** je přidán do **vlastnosti** adresáře. Soubor je pak upravit zahrnout [podporuje obrazovky](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Přidání podporuje obrazovky](resources-for-varying-screens-images/02-adding-supports-screens-vs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Manifestu systému Android.](resources-for-varying-screens-images/01-android-manifest-xs-sml.png)](resources-for-varying-screens-images/01-android-manifest-xs.png#lightbox)

**AndroidManifest.xml** je přidán do **vlastnosti** adresáře. Soubor je pak upravit zahrnout [podporuje obrazovky](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Přidání podporuje obrazovky](resources-for-varying-screens-images/02-adding-supports-screens-xs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-xs.png#lightbox)

-----

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Zadejte alternativní rozložení pro jiné velikosti obrazovky

I když Android změní velikost podle velikosti obrazovky, nemusí to být v některých případech dostatečné. To může být žádoucí, zvětšete velikost některé prvky uživatelského rozhraní na větší obrazovku, nebo chcete-li změnit umístění prvky uživatelského rozhraní pro menší obrazovky.

Počínaje 13 úroveň rozhraní API (Android 3.2), velikost obrazovky jsou zastaralé považuje pomocí sw*N*kvalifikátor distribučního bodu. Tento nový kvalifikátor deklaruje, že je velikost místa na dané rozložení. Důrazně doporučujeme, že aplikace, které jsou určené pro spuštění na Android 3.2 nebo vyšší musí používat tyto novější kvalifikátory.

Například v případě potřeby minimální 700dp šířky obrazovky rozložení alternativní rozložení by se dostala do složky **rozložení sw700dp**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Složky rozložení pro šířku 700dp obrazovky](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Složky rozložení pro šířku 700dp obrazovky](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


Jako vodítko Zde jsou některé čísla pro různá zařízení:

- **Typické phone** &ndash; 320dp: typické telefon

- **Tablet 5" nebo"tweener"zařízení** &ndash; 480dp: například Samsung Poznámka

- **Tablet 7"** &ndash; 600dp: například Barnes &amp; Noble Nook

- **Tablet 10"** &ndash; 720dp: například Motorola Xoom

Pro aplikace, že cílové rozhraní API úrovně až 12 (Android 3.1), rozložení by měli přejít v adresářích, které používají kvalifikátory **malé**/**normální**/**velké**  / **xlarge** jako generalizace různé velikost obrazovky, které jsou k dispozici v většina zařízení. Například následující obrázek existují alternativní zdroje pro čtyři různých obrazovek velikosti:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternativní zdroje pro čtyři velikosti obrazovky](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Alternativní zdroje pro čtyři velikosti obrazovky](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Následuje porovnání jak starší obrazovky velikost kvalifikátory úroveň 13 pre-API porovnání nezávislé na hustotě pixelů:

- je 426dp x 320dp **malé**

- je 470dp x 320dp **normální**

- je 640dp x 480dp **velké**

- je 960dp x 720dp **xlarge**

Na obrazovce novější velikost kvalifikátory na úrovni rozhraní API 13 a do mají vyšší prioritu než starší kvalifikátory obrazovky úrovně rozhraní API 12 a nižší.
Pro aplikace, které bude span starý a nový úrovně rozhraní API může být potřeba vytvořit alternativní prostředky pomocí obě sady kvalifikátory, jak je znázorněno na následujícím snímku obrazovky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternativní prostředků pomocí obou kvalifikátory](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Alternativní prostředků pomocí obou kvalifikátory](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Zadejte jiný bitmap pro densities – různých obrazovek

I když Android bude škálovat rastrové obrázky v případě potřeby u zařízení, rastrové obrázky, sami nemusí elegantně škálovat nahoru nebo dolů: mohou být přibližné nebo rozmazaně. Poskytnutí rastrové obrázky, které jsou vhodné pro hustotě bude zmírnění tohoto problému.

Například následující obrázek je příkladem rozložení a vzhled problémy, které mohou nastat při hustotu zadejte prostředky nejsou zadány.

![Snímky obrazovky bez hustotu prostředků](resources-for-varying-screens-images/06-density-not-provided.png)

Výsledky porovnejte s rozložení, které je navrženo s velkou hustotou konkrétní prostředky:

![Snímky obrazovky s hustotu specifické prostředky](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Vytvořit různých hustotu prostředky s Android Asset Studio

Vytvoření těchto bitmap různé densities – může to být trochu zdlouhavé. Jako takový Google vytvořil online nástroj, můžete tak omezit některé nebylo nutné pracně zabývajících se vytváření těchto bitmap volat [ **Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/).

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

Tento web vám pomůže s vytváření bitové mapy, které cílí na čtyři běžné obrazovky densities – tím, že poskytuje jednu image. Android Asset Studio bude poté vytvořte rastrových obrázků s určitá vlastní nastavení a pak mohly stáhnout jako soubor zip.


## <a name="tips-for-multiple-screens"></a>Tipy pro více obrazovky

Android běží na nepřeberné počet zařízení, a může to vypadat čtenáře kombinace velikosti obrazovky a hustoty obrazovky. Následující tipy minimalizovat úsilí nezbytné pro podporu různých zařízení:

- **Pouze návrh a vývoj, pro které je potřeba** &ndash; existuje mnoho různých zařízení odhlašování existuje, ale některé existovat ve výjimečných formuláře faktorů, které mohou vyžadovat mnoho úsilí pro návrh a vývoj pro. [ **Velikost obrazovky a s velkou hustotou** ](http://developer.android.com/resources/dashboard/screens.html) řídicí panel je na stránce poskytované Google, který poskytuje data na rozpis matice hustotu velikost/obrazovky obrazovky. Toto rozdělení poskytuje přehled o tom, jak náročnost vývoje na podporu obrazovky.

- **Použití distribučních bodů místo pixelů** -pixelů stát problémových jako obrazovky hustotu změny. Proveďte závislé hodnoty pixelů. Vyhněte se pixelů považuje distribučního bodu (nezávislé na hustotě pixelů).

- **Vyhněte se** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **kdekoli možné** &ndash; ho je zastaralé v rozhraní API úroveň 3 (Android 1.5) a bude mít za následek křehká rozložení. Není vhodné používat. Místo toho zkuste použít flexibilnější rozložení pomůcky, jako je [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), nebo nový [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/).

- **Vyberte jeden rozložení orientaci jako výchozí** &ndash; například místo alternativní prostředky **rozložení krajině** a **rozložení port**, put prostředky pro na šířku v **rozložení**a prostředky na výšku do **rozložení port**.

- **Použít LayoutParams výška a šířka** – při definování prvky uživatelského rozhraní v souboru XML rozložení aplikace Android pomocí **wrap_content** a **fill_parent** hodnoty budou mít další úspěch Zajistěte správné podívejte se na různých zařízeních než použití pixelu nebo hustotu nezávislých jednotek. Tyto hodnoty dimenze způsobit Android k prostředkům rastrový obrázek škálování podle potřeby. Z tohoto důvodu stejné jednotky nezávislé na hustotu jsou nejlépe rezervovány pro při zadání pravého okraje a odsazení prvků uživatelského rozhraní.


## <a name="testing-multiple-screens"></a>Testování více obrazovek

Aplikace pro Android se musí otestovat proti všechny konfigurace, které budou podporované. V ideálním případě by měl být testována zařízení na skutečné zařízeními ale v řadě případů to není možné nebo praktické.
Použití emulátoru a virtuální zařízení se systémem Android instalace pro každou konfiguraci zařízení v tomto případě bude hodit.

Sadu Android SDK poskytuje možnosti pro velikost, hustotu a řešení mnoho zařízení, replikují se některé emulátoru vzhledy může použít k vytvoření AVD společnosti.
Mnoho dodavatelů hardwaru podobně zadejte vzhledy pro svá zařízení.

Další možností je používat službu třetí strany testování služby.
Tyto služby bude trvat APK, spusťte ho na mnoha různých zařízení a potom poskytnout zpětnou vazbu, jak šlo aplikace.
