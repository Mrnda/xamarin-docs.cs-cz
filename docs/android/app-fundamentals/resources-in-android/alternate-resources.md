---
title: "Alternativní prostředky"
ms.topic: article
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 7ebbf2a9215c8472ae2f286728cb2f819e8331cb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="alternate-resources"></a>Alternativní prostředky

Alternativní prostředky jsou tyto prostředky, které cílí na určité zařízení nebo spuštění konfiguraci, jako je aktuální jazyk, velikost konkrétní obrazovku nebo hustotě pixelů. Pokud Android se může shodovat na prostředek, který je specifičtější pro určité zařízení nebo konfiguraci než výchozí prostředek, bude tento prostředek použít místo. Pokud nenajde alternativní prostředek, který odpovídá aktuální konfiguraci, bude možné načíst výchozí prostředky. Jak Android rozhodne, jaké prostředky budou používat aplikace se budeme podrobněji níže v části umístění prostředku

Alternativní prostředky jsou uspořádána jako podadresář ve složce prostředky podle typ prostředku, stejně jako výchozí prostředky. Název podadresáře alternativní prostředek je ve formátu: _ResourceType_-_kvalifikátor_

*Kvalifikátor* je název, který identifikuje konfigurace konkrétní zařízení.
Název, každý z nich oddělené pomlčkou může být více než jeden kvalifikátor. Například následující snímek obrazovky ukazuje jednoduchý projektu, který má alternativní zdroje pro různé konfigurace, třeba národního prostředí, hustotě, obrazovky velikost a orientaci:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Alternativní prostředky](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Alternativní prostředky](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

Následující pravidla platí při přidávání kvalifikátory pro typ prostředku:

1. Může existovat více než jeden kvalifikátor s každou kvalifikátor oddělené pomlčkou.

2. Kvalifikátory může být zadán pouze jednou.

3. Kvalifikátory musí být v pořadí, ve kterém se objeví v následující tabulce.

Možné kvalifikátory jsou uvedené dole pro referenci:

- **MCC a MNC** &ndash; [mobilní směrové číslo země](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) a volitelně [kódu mobilní sítě](http://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). SIM karta poskytne MCC, zatímco zařízení je připojené k síti se poskytují MNC. Přestože je možné cíl národní prostředí pomocí kód země mobilní, doporučujeme možných přístupů je použít jazyk kvalifikátor níže uvedené. Chcete-li například cílové prostředky k Německo, kvalifikátor by `mcc262`. Pro cílové prostředky T-Mobile v USA kvalifikátor je `mcc310-mnc026`.
  Úplný seznam kódy mobilních země a kódy mobilní sítě najdete v části <http://mcclist.com/>.

- **Jazyk** &ndash; dvou písmen [kód ISO 639-1 jazyka](http://en.wikipedia.org/wiki/ISO_639-1) a volitelně následované dvěma písmeno [kód oblasti ISO-3166-alpha-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Pokud jsou k dispozici obě kvalifikátory, pak jsou odděleny `-r`. Například pro cílový hovořícího anglicky francouzština národní prostředí poté kvalifikátor z `fr` se používá. Chcete cílit French-Canadian národní prostředí, `fr-rCA` se použije. Úplný seznam kódů a kódy oblastí, najdete v části [kódy pro reprezentaci názvy jazyků](http://www.loc.gov/standards/iso639-2/php/English_list.php) a [názvy země a elementy kódu](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **Nejmenší šířka** &ndash; Určuje nejmenší šířku obrazovky aplikace je určen pro spuštění ve. Podrobněji v [vytváření prostředků pro různých obrazovky](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  K dispozici na úrovni rozhraní API 13 (Android 3.2) a vyšší. Například kvalifikátor `sw320dp` slouží do cílových zařízení, jehož výška a šířka je minimálně 320dp.

- **Dostupná šířka** &ndash; minimální šířka obrazovky ve formátu w*N*distribučního bodu, kde *N* je šířka v hustotu nezávislé pixelů.
  Tato hodnota může změnit, protože uživatel otočí zařízení. Podrobněji v [vytváření prostředků pro různých obrazovky](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  K dispozici v úrovni rozhraní API 13 (Android 3.2) a vyšší. Příklad: w720dp kvalifikátor se používá k cílové zařízení, která mají šířku minimálně 720dp.

- **K dispozici výška** &ndash; minimální výška obrazovky v h formátu*N*distribučního bodu, kde *N* je výška v distribučního bodu. Tato hodnota může změnit, protože uživatel otočí zařízení. Podrobněji v [vytváření prostředků pro různých obrazovky](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  K dispozici v úrovni rozhraní API 13 (Android 3.2) a vyšší. Například h720dp kvalifikátor je používají k zaměření zařízení, která mají výška minimálně 720dp

- **Velikost obrazovky** &ndash; Tento kvalifikátor je generalizace velikosti obrazovky, který tyto prostředky jsou pro. Je popsaná v podrobněji [vytváření prostředků pro různých obrazovky](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Možné hodnoty jsou `small`, `normal`, `large`, a `xlarge`. Přidání rozhraní API úrovni 9 (Android 2.3 a Androidu 2.3.1/Android 2.3.2)

- **Obrazovky aspekt** &ndash; to je založené na poměr stran, není orientace obrazovky. Dlouhé obrazovky je širší. Přidat na úrovni rozhraní API 4 (Android 1.6). Možné hodnoty jsou dlouho a notlong.

- **Orientace obrazovky** &ndash; obrazovky orientaci na výšku nebo na šířku. To lze změnit během životního cyklu aplikace.
  Možné hodnoty jsou `port` a `land`.

- **Ukotvení režimu** &ndash; pro zařízení v automobilu ukotvení nebo ukotvení HelpDesk. Přidat na úrovni rozhraní API 8 (Android 2.2.x). Možné hodnoty jsou `car` a `desk`.

- **Režim noc** &ndash; zda je aplikace spuštěna v noci nebo dne. To může změnit v průběhu životního cyklu aplikace a měl by poskytnout vývojářům možnost používat tmavšího verze rozhraní v noci. Přidat na úrovni rozhraní API 8 (Android 2.2.x). Možné hodnoty jsou `night` a `notnight`.

- **Obrazovky hustotě pixelů (dpi)** &ndash; počet pixelů v dané oblasti na obrazovce fyzické. Obvykle vyjádřený jako bodů na palec (dpi). Možné hodnoty jsou:

    - `ldpi` &ndash; Nízkou hustotu obrazovky.

    - `mdpi` &ndash; Střední hustotu obrazovky

    - `hdpi` &ndash; S vysokou hustotou obrazovky

    - `xhdpi` &ndash; Velmi vysokou hustotou obrazovky

    - `nodpi` &ndash; Prostředky, které nechcete škálovat

    - `tvdpi` &ndash; Počínaje úroveň rozhraní API 13 (Android 3.2) pro obrazovky mezi mdpi a hdpi.

- **Typ dotykovou obrazovku** &ndash; Určuje typ dotykovou obrazovku zařízení může mít. Možné hodnoty jsou `notouch` (žádné dotykovou obrazovku), `stylus` (odporových dotykovou obrazovku vhodný pro pera), a `finger` (dotykovou obrazovku).

- **Klávesové dostupnosti** &ndash; Určuje, jaký druh klávesnice je k dispozici. To může změnit v průběhu životního cyklu aplikace &ndash; například když uživatel otevře klávesnice hardwaru. Možné hodnoty jsou:

    - `keysexposed` &ndash; Zařízení má k dispozici klávesnici. Pokud není žádná klávesnice softwaru povolené, pak to se používá pouze při otevření klávesnice hardwaru.

    - `keyshidden` &ndash; Zařízení nemá klávesnice hardwaru, ale je skrytá a žádné klávesnice softwaru je povoleno.

    - `keyssoft` &ndash; zařízení má klávesnici softwaru povolené.

- **Primární metoda zadávání textu** &ndash; použijte k určení toho, jaké typy hardwaru klíče jsou k dispozici pro vstup. Možné hodnoty jsou:

    - `nokeys` &ndash; Neexistují žádné klíče hardwaru pro vstup.

    - `qwerty` &ndash; Není k dispozici qwerty klávesnice.

    - `12key` &ndash; Je 12 klíč hardwaru klávesnice


- **Navigace klíč dostupnosti** &ndash; pro až 5-stejným způsobem jako nebo řídicí navigace (směrové pad) k dispozici. Toto můžete změnit po dobu platnosti vaší aplikace. Možné hodnoty jsou:

    - `navexposed` &ndash; navigační klíče jsou k dispozici pro uživatele

    - `navhidden` &ndash; Navigační klávesy nejsou k dispozici.

-  **Primární metoda navigační Non-Touch** &ndash; druh navigace k dispozici v zařízení. Možné hodnoty jsou:

    - `nonav` &ndash; k dispozici pouze navigační zařízení je dotykovou obrazovku

    - `dpad` &ndash; je k dispozici pro navigaci d pad (na směru klávesnici)

    - `trackball` &ndash; zařízení má trackball navigace

    - `wheel` &ndash; neobvyklé scénáře, kde je jeden nebo více směrové kola k dispozici

-  **Verze platformy (úroveň rozhraní API)** &ndash; úroveň rozhraní API podporuje zařízení ve formátu v*N*, kde *N* je úroveň rozhraní API, která je cílem. Například verze 11 se zaměří na úroveň rozhraní API 11 (Android 3.0) zařízení.


Další informace o prostředku v tématu kvalifikátory [poskytuje prostředky](http://developer.android.com/guide/topics/resources/providing-resources.html) na webu pro vývojáře v systému Android.


## <a name="how-android-determines-what-resources-to-use"></a>Jak Android Určuje, jaké prostředky pro použití

Je velmi možné a pravděpodobné, že aplikace platformy Android bude obsahovat mnoho prostředků. Je důležité pochopit, jak bude Android vyberte prostředky pro aplikaci při spuštění v zařízení.

Android určuje základní prostředky podle iterování přes test následujících pravidel:

- **Odstranění odporuje kvalifikátory** &ndash; například, pokud zařízení orientaci na výšku, pak všechny adresáře prostředků na šířku budou odmítnuty.

- **Ignorovat kvalifikátory nepodporuje** &ndash; ne všechny kvalifikátory jsou k dispozici pro všechny úrovně rozhraní API. Pokud adresář prostředků obsahuje kvalifikátor, která není podporována zařízením, bude tento adresář prostředků ignorována.

- **Určit další nejvyšší prioritou kvalifikátor** &ndash; odkazující na výše uvedené tabulce vyberte další nejvyšší prioritou kvalifikátor (shora dolů).

- **Zachovat všechny adresáře prostředků pro kvalifikátor** &ndash; Pokud neexistují žádné adresářů prostředků, které se shodují kvalifikátor na výše uvedené tabulce vyberte další nejvyšší prioritou kvalifikátor (shora dolů).

Tato pravidla jsou také znázorněné v následujícím diagramu:

[![Vývojový diagram prostředky](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

Když systém hledá hustotu specifických prostředků a nemůžete je najít, pokusí se najít další hustotu konkrétní prostředky a škálování je. Android nemusí nezbytně použít výchozí prostředky.
Například může Android při vyhledávání pro prostředek s nízkou hustotou a není k dispozici, vyberte v podobě výkonných verzi prostředku nad výchozí hodnotu nebo střední hustotu prostředky. Dělá to, protože v podobě výkonných prostředků je možné rozšířit faktorem 0,5, což bude mít za následek méně viditelnost problémy než škálování dolů střední hustotu prostředků, která by vyžadovala faktor 0,75.

Jako příklad vezměte v úvahu aplikace, která má následující adresáře drawable prostředků:

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

A teď je aplikace spuštěna v zařízení s následující konfiguraci:

- **Národní prostředí** &ndash; en-GB
- **Orientace** &ndash; portu
- **Obrazovky hustotu** &ndash; hdpi
- **Typ dotykovou obrazovku** &ndash; notouch
- **Primární vstupní metoda** &ndash; 12key

Se vyloučí francouzské prostředky, jako jsou v konfliktu se národního prostředí `en-GB`, nám s ponechat:

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

V dalším kroku se vybere první kvalifikátor z výše uvedené tabulce kvalifikátory: MCC a MNC. Neexistují žádné adresáře prostředků, které obsahují tento kvalifikátor proto kód MCC/MNC je ignorován.

Další kvalifikátor je zaškrtnuto, což je jazyk. Neexistují prostředky, které odpovídají kód jazyka. Všechny adresáře prostředků, které neodpovídají kód jazyka `en` byly zamítnuty, takže seznamu prostředků je nyní:

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

Další kvalifikátor, která je k dispozici je pro orientace obrazovky, tak všechny adresáře prostředků, které neodpovídají orientace obrazovky z `port` se vyloučí:

    drawable-en-port
    drawable-en-port-ldpi

Dále je kvalifikátor pro hustotě, `ldpi`, výsledkem vyloučení jednoho další adresář prostředků:

    drawable-en-port-ldpi

V důsledku tohoto procesu Android používat drawable prostředků v adresáři prostředků `drawable-en-port-ldpi` pro zařízení.

> [!NOTE]
> Kvalifikátory velikost obrazovce zadejte jedinou výjimkou tohoto procesu výběru. Je možné pro Android vyberte prostředky, které jsou určené pro obrazovky menší než poskytuje jaké aktuální zařízení. Například velké obrazovce zařízení mohou používat prostředky poskytovat normální velikosti obrazovky. Ale zpětného to není pravda: do stejného zařízení promítat obrazovku nebude používat zdroje uvedené obrazovce xlarge. Pokud Android nelze najít sadu prostředků, které odpovídá dané obrazovku o velikosti, dojde k chybě aplikace.
