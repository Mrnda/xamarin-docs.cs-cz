---
title: Lokalizace rozhraní uživatele aplikace
description: Tento dokument popisuje koncepty napříč platformami mezinárodní prostředí a lokalizace a prověří, jak budou mít vliv na návrh aplikace.
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 299fe28a896c2bf2e5c420b330b8e8bde7d9dc22
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781953"
---
# <a name="localization"></a>Lokalizace

Tento průvodce představuje koncepty za *internacionalizace* a *lokalizace* a odkazy na pokyny o tom, jak vytvořit pomocí tyto koncepty mobilní aplikace Xamarin.

Pokud chcete nechat přeskočit rovnou na technické informace o lokalizaci aplikace Xamarin, spusťte některý z těchto články s návody specifické pro platformu:

- [**Xamarin.Forms** ](~/xamarin-forms/app-fundamentals/localization/index.md) napříč platformami lokalizace pomocí souborů RESX.
- [**Xamarin.iOS** ](~/ios/app-fundamentals/localization/index.md) lokalizace nativní platformy.
- [**Xamarin.Android** ](~/android/app-fundamentals/localization.md) lokalizace nativní platformy.

## <a name="i18n-and-l10n"></a>i18n a L10n

*Internacionalizace* je proces, aby váš kód způsobilé zobrazení různé jazyky a přizpůsobení jeho zobrazení pro různá národní prostředí (například číslo a formátování data). Tím se také označuje jako *globalizace*.

*Lokalizace* je krok, který následuje – vytváření prostředků (například řetězce a bitové kopie) pro každý jazyk a sdružování je internationalize aplikaci.

Mezinárodní prostředí, které často bude zkráceno na i18n – sdružená 18 písmen mezi "i" a "n". Lokalizace bude zkráceno podobně jako na L10n – 10 písmen mezi "L" a "n".

## <a name="overview"></a>Přehled

Toto téma představuje koncepty přidružené mezinárodní prostředí a lokalizace a jak se vztahují na vývoj mobilních aplikací v hlavní.
Při navrhování a vytváření aplikace, věcí, které bude pravděpodobně dříve pevně zakódované, ale které musí parametry pro lokalizaci patří:

-   Rozložení obrazovky a text
-   Ikony, grafika a barvy,
-   Videí a zvukových souborů
-   Dynamické textu a formátování textu (například čísla, měny a data)
 - Změny rozložení pro jazyky zprava doleva (RTL), a
-   Řazení dat.

Bez ohledu na to, které mobilní platformy vaší aplikace cílí tyto tipy vám pomůže vytvořit aplikaci lokalizované vysoké kvality.


## <a name="design-considerations"></a>Aspekty návrhu

Architektury aplikace, aby bylo možné lokalizaci jeho obsah se nazývá internacionalizace. Internacionalizace správně není, že více než jenom povolení pro jiný jazyk řetězce načíst za běhu – dobře navržených aplikace by měl povolit pro všechny prostředky se musí změnit podle jazyka a národního prostředí (včetně obrázků, zvuky a videa) a můžete přizpůsobit. formátování a rozložení, aby se zvládl různých velikost řetězce.

Tato část popisuje některé aspekty návrhu a vzít v úvahu při sestavování mezinárodní aplikace.

### <a name="layouts-and-string-length"></a>Rozložení a délka řetězce

Čínština, japonština řetězec může být velmi krátké – někdy jeden nebo dva znaky mohou být dostatečně smysl pro popisek vstupní pole.

(Němčině řetězce třeba) může být velmi dlouhé; někdy stane velmi dlouhé v jiných jazycích – buď stal oříznutí nebo jiného neočekávaně také přeformátování vašeho rozložení poměrně krátké aplikace word v angličtině.

Porovnejte řetězec délky pro několik položek na domovské obrazovce iOS angličtinu, němčinu a japonština:

[![](localization-images/language-compare-sml.png "Délka řetězce japonské němčině vs")](localization-images/language-compare.png#lightbox)

Všimněte si, že **nastavení** v angličtině (8 znaků) vyžaduje 13 znaků pro překlad němčině, ale pouze 2 znaky. v japonštině.

Rozložení, kdy jsou zobrazit popisek a vstupní pole vedle sebe obtížně se pracovat s když délka popisku se může značně lišit. Často je jednodušší lokalizaci, protože je k dispozici pro popisek a vstup celou šířku obrazovky rozložení, kde se zobrazí popisek nad polem.

Obecně platí Pokud vytváříte pevné rozložení (zejména elementy vedle sebe) umožňují alespoň 50 % vyžadovat další šířka než anglické řetězce pro popisky a text. Každý problém nevyřeší, ale bude poskytovat vyrovnávací paměť, která bude fungovat v mnoha případech.

### <a name="input-validation"></a>Ověření vstupu

Mějte na paměti předpokladů při zápisu pravidel ověřování. Mohou vám platný tak, aby vyžadovala textové pole zadejte "vyžadovat" alespoň tři znaky v angličtině, protože jedno písmeno velmi zřídka nemá žádný význam. V Čínské a japonština ale jeden znak může být platné zadání a ověřování zprávu "alespoň 3 znaky je požadovaná" nemá smysl pro tyto jazyky.

Další zdánlivě jednoduché úlohy, jako ověření e-mailovou adresu nebo adresu URL webu stane složitější s znaky nejsou omezeny na podmnožinu ASCII.

Zápisu pravidel ověřování s internacionalizace pamatovat – buď zvolte nejméně omezující pravidla nebo zápis logiky, který bude fungovat různě pro jednotlivé jazyky.

### <a name="images-and-color"></a>Bitové kopie a barvy

Ne každý image musí změnit podle volby jazyka uživatele. Mnoho ikony nebo fotografie budou vhodný pro všechny uživatele, v jakém jazyce stahované, není podstatné.
Některé prostředky smysl lokalizaci Přestože, jako například:

 - Obrázky znázorňující osoby nebo konkrétní umístění – vaše aplikace může působí relevantnější uživatelům se zobrazí místní osoby nebo umístění.
 - Ikony – některé používá může být specifické pro jazykovou verzi a můžete můžete usnadnit vaše aplikace používána lokalizace obrazů tak, aby odrážela místní pochopení.
 - Barvy – některé jazykové verze pochopit barvy jinak – red může znamenat upozornění v jedné oblasti, ale hodně štěstí v jiném. Při navrhování vaší aplikace k určení, zda jste by měl být vytváření mechanismus k lokalizaci barvy, obraťte se na rodilí mluvčí.


### <a name="videos-and-sound"></a>Videa a zvuku

Videa a zvuku přítomen speciální problémy při lokalizaci aplikaci, protože i když je poměrně snadné ho získat řetězce přeložit, zaznamenávání více voiceover sleduje nebo videosoubory, může být nákladné a obtížná.

Více kopií videa a zvukových souborů může také výrazně zvýšit velikost aplikace (obzvláště pokud jsou lokalizace do velký počet jazyků nebo máte velký počet mediálních souborů). Můžete uvažovat o stahování pouze prostředky požadovaných jazykových po uživatel nainstaloval aplikaci, ale v důsledku může také dojít nízký uživatelské prostředí v pomalých sítích.

Jsou často několika způsoby, jak řešit problémy lokalizace – nejdůležitější je zvažovat je počáteční a ujistěte se, že vaše aplikace je navržena tak, abyste dbali z nich.


### <a name="dates-times-numbers-and-currency"></a>Data, časy, čísla a měny

Pokud používáte formátování funkce .NET, musíte zadat jazykovou verzi, aby oddělovače desetinných míst jsou správně analyzovat (a vyhnout se výjimky převod hlášeny). Například 1,99 i 1,99 jsou platné desetinné reprezentace v závislosti na vaší národního prostředí.

Pokud data pocházejí ze známého zdroje (ie. z vlastní kód nebo webové služby, který ovládáte) můžete používat pevné kódování identifikátor jazykové verze, která odpovídá formátování, jako je například InvariantCulture, který bude fungovat pro standardní anglické jazykové formátování.

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

Pokud uživatel aplikace není se vstupní data, analyzovat pomocí CultureInfo instance, která odráží jejich národního prostředí:

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

Najdete v článku [Analýza číselných řetězců](http://msdn.microsoft.com/library/xbtzcc4w(v=vs.110).aspx) a [řetězců analýza data a času](http://msdn.microsoft.com/library/2h3syy57(v=vs.110).aspx) MSDN články pro další informace.

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>Pravé zprava doleva (RTL)

Některé jazyky, např. arabština, hebrejština a urdština (například) jsou čtení zprava doleva.
Aplikace, které podporují tyto jazyky využít návrhy obrazovky, které přizpůsobit pro čtečky zprava doleva, například:

 - Text by měl být zarovnaný doprava.
 - Popisky by se zobrazit vedle vstupních polí.
 - Výchozí tlačítko umístění je obecně obrácený.
 - Hierarchická navigace k načtení a animace (a další navigační metaphors a animací), které využívají pro kontext by také obrátit směr.

IOS a Android podporují rozložení zprava doleva a vykreslování písma s integrovanou funkcí, které pomáhají provádět úpravy výše. Xamarin.Forms nepodporuje aktuálně automaticky vykreslování zprava doleva.

### <a name="sorting"></a>Řazení

Různé jazyky definovat pořadí řazení jejich abeced odlišně, i když používají stejné znakovou sadu.

Najdete v článku [podrobné porovnání řetězců](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison) v [osvědčené postupy pro používání řetězců v rozhraní .NET Framework](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx) příklad kde jazyk (CultureInfo) má vliv pořadí řazení.

Není pravděpodobné že integrovanou databází funkcí na mobilních platformách budou podporovat řazení pro specifický jazyk, řazení, může být nutné implementovat další kód v obchodní logiky.

### <a name="text-search"></a>Hledání textu

Ujistěte se, psaní a testování vaší vyhledávací algoritmus s více jazyky v paměti. Co je třeba zvážit patří:

 - Automatické dokončování – Pokud jste vytvořili funkce automatického dokončování Ujistěte se, že jeho zdroje návrhy relevantní pro jazyk daného uživatele.
 - Odpovídající dotaz na data – hledat dotazy zadané v konkrétní jazyk provést proti jenom obsah zapsaný v daném jazyce nebo proti veškerý obsah ve vaší aplikaci?
 - Rozklad – Pokud chcete vyhledat podobné slova, word kořeny a další optimalizace vyhledávání je integrovaný hledání jsou tyto optimalizace vytvořené pro všechny jazyky, které podporujete?
 - Řazení – Ujistěte se, výsledky jsou seřazeny správně (viz výše).


### <a name="data-from-external-sources"></a>Data z externích zdrojů

Mnoho aplikací stahování dat z externích zdrojů z Twitteru a informační kanály RSS, počasí, zprávy nebo uložených ceny. Toto zobrazení pro uživatele musíte zvážit možnost, že se zobrazí obrazovka důležité nebo nejde přečíst informace k nim.

Existuje několik strategií, které můžete použít k zkuste a ujistěte se, že vaše aplikace zobrazí data pro uživatele relevantní:

 - Různých zdrojů – vaše aplikace může stáhnout data z jiného zdroje v závislosti na jazyk nebo národní prostředí uživatele. Národní prostředí příspěvky, počasí a stock ceny může dávat větší smysl než něco stažený z Severní Ameriky informačního kanálu.
 - Lokalizovaná zobrazení – Pokud zobrazujete Twitter nebo fotografie kanálu, můžete by měl zobrazit metadata (například čas potřebný) v jazyce vlastní, i v případě, že samotný obsah zůstane v původním jazyce.
 - Překlad – je do vaší aplikace udělat strojový překlad příchozích dat sestavení možnost překlad. Může to být automatická nebo uvážení uživatele – nezapomeňte upozorní uživatele, pokud to probíhá, vzhledem k tomu, že počítač překlady se nikdy ideální!

To také mohou ovlivnit externí odkazy do zvukových stop nebo videa – při navrhování vaší aplikace nezapomeňte připravte se na sourcing přeložit obsah nebo zajistit, že uživatelé jsou adekvátní o tom informováni prostřednictvím uživatelského rozhraní při obsahu nebude uvedené v jejich jazyk.


### <a name="dont-over-translate"></a>Nemáte přepsání převede

Některé řetězce ve vaší aplikaci nemusí potřebovat překladu, nebo v každém zvláštní pozornost podle překladač. Příklady mohou zahrnovat:

 - Adresy URL – Pokud seznam obsahuje adresu URL, může nebo nemusí je třeba upravit podle jazyka. Například facebook.com nevyžaduje překlad ji automaticky zjistí jazyk v hlavní lokalitě. Ostatní lokality obsahem specifická pro národní prostředí, můžete chtít nabízet jinou adresu URL, například yahoo.com versus yahoo.fr nebo yahoo.it.
 - Telefonní čísla – především těch s různé kódy zemí nebo čísla pro volání, která řeči konkrétního jazyka.
 - Kontaktní údaje – adresy a další informace se můžou lišit podle jazyk nebo národní prostředí.
 - Ochranné známky & názvy produktů – některé řetězce nepotřebují překladu vzhledem k tomu, že jste vždy zapsány ve stejném jazyce.

Nakonec nezapomeňte zahrnout podrobné pokyny pro překladač, pokud některé řetězce vyžadují speciální zacházení.


### <a name="formatted-text"></a>Formátovaný text

Obvykle k problému s mobilní aplikace protože řetězců obecně nejsou formátovat. Ale pokud RTF (například tučné a kurzíva formátování) je vyžadována ve vaší aplikaci Ujistěte se, překladač vědět, jak jako vstup, formátování, které se, řetězce soubory uložit správně a je správně naformátován než se zobrazí uživatelům (ie. Nenechte omylem formátování kódy, sami se zobrazí uživateli).



## <a name="translation-tips"></a>Tipy pro překlad

Řetězce používaný aplikací k překladu se považuje za součást procesu lokalizace. Obvykle tato úloha bude zajištěný vnějším zdrojem službě překlad a provádí vícejazyčné zaměstnanci, která nemusí vědět, aplikace nebo vaší firmě.

Následující tipy vám pomůže vytvořit řetězce, které se snadněji přeložte přesně a proto zlepšení kvality lokalizované aplikací.



### <a name="localize-complete-strings-not-words"></a>Dokončení řetězce, není slova pro lokalizaci

Vývojáři někdy trvat přístup, kdy při pokusu o zadejte jednoho slova nebo větu "fragmenty" tak, aby se můžete znovu použít v celé aplikaci. Například pro text "máte 5 zprávy." mohou například určit následující řetězce pro překlad

**Chybný**:

```csharp
"You have"
"no"
"message"
"messages"
```

a pak se pokusíte vytvořit správný frázi na průběžně v kódu pomocí zřetězení řetězců:

**Chybný**:

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

**Toto se nedoporučuje** protože nebude nutně fungovat pro všechny jazyky a bude těžko překladač pochopit kontextu jednotlivých krátké segmentu. Také vede k opětovnému použití přeložených řetězců, které může způsobit problémy později Pokud se používají v různých kontextech (a potom aktualizovat).


### <a name="allow-for-parameter-re-ordering"></a>Povolit pro parametr znovu řazení.

Některé programovací jazyky vyžadovat další syntaxe pro určení pořadí parametrů v řetězci, ale .NET, již podporuje koncept číslem zástupné symboly,

**Dobrý**:

```csharp
"a {0} b {1} cde {3}"
```

možné přeložit následující (kde pozice a pořadí zástupných symbolů mění)

```csharp
"{2} {3} f g h {0}"
```

a tokeny bude objednáno jako překladač určena. Ujistěte se, že obsahovat vysvětlení, co každý zástupný text obsahuje při odesílání řetězec k překladači.


### <a name="use-multiple-strings-for-cardinality"></a>Použít pro mohutnost vícenásobných řetězců

Vyhněte se řetězce jako `"You have {0} message/s."` zajistit lepší prostředí použít konkrétní řetězce pro každý stav:

**Dobrý**:

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

Budete muset psát kód ve vaší aplikaci k vyhodnocení číslo se zobrazí a zvolte příslušným řetězcem. Některé platformy (včetně iOS a Android) mají integrované funkce automaticky vybrat nejlepší řetězec množném čísle podle preferencí pro aktuální jazyk nebo národní prostředí.


### <a name="allowing-for-gender"></a>Aby vám umožnil pohlaví

Jazyky založené na Latin někdy použijte jiná slova v závislosti na pohlaví subjektu. Pokud aplikace zná o pohlaví, měli byste povolit přeloženými řetězci tak, aby odrážela to.

Je také zřejmější tak i v angličtině, kde najdete řetězce pro konkrétní osoby nebo uživatele vaší aplikace. Například některé weby zobrazit zprávy jako `"Bob commented on his post"` proto musíte řetězce pro mužského, ženského a NEBINÁRNÍ nebo neznámé pohlaví:

**Dobrý**:

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>Nemusíte znovu použít řetězce

Nebo přesněji, nemusíte znovu použijte řetězce právě, protože jsou podobně jako v případě, že vlastní řetězec má jiný účel nebo význam.

Příklad: Představte si máte přepínač zapnout nebo vypnout ve vaší aplikaci a ovládací prvek přepínač musí text pro "na" a "vypnuto" lokalizovat. Můžete také zobrazit hodnotu tohoto nastavení jinde v aplikaci v textu popisku. Jiné řetězce by měl použít pro zobrazení přepínače a stav na přepínač (i když jsou do jednoho řetězce v jazyce výchozí) – například:

-   "Na" – zobrazí na přepínači sám sebe
-   "Vypnuto" – zobrazí na přepínači sám sebe
-   "Na" – zobrazené v popisku
-   "Vypnuto" – zobrazené v popisku

To poskytuje maximální flexibilitu překladače:

-   Z důvodů návrhu možná přepínač samotné používá malá "na" a "off" ale zobrazit popisek používá velká "Na" a "Off".
-   Některé jazyky může být nutné přepínač hodnota, která má být zkratka a nevejde se do uživatelského ovládacího prvku rozhraní, zatímco dokončení (přeložený) aplikace word se mohou objevit v popisku.
-   Pro některé jazyky vykreslování přepínač může být případně pro kulturního znalosti použít "I" a "O", ale stále můžete chtít štítek, který chcete číst "Na" nebo "Vypnuto".

### <a name="translation-services"></a>Překladatelské služby

#### <a name="machine-translation"></a>Strojový překlad

Chcete-li vytvořit funkce překladu do vaší aplikace, zvažte [rozhraní API služby Azure překladač Text](https://azure.microsoft.com/en-au/services/cognitive-services/translator-text-api/).

Pro účely testování může použít jeden z mnoha nástrojů překlad online pro některé lokalizované textu v aplikaci během vývoje:

- [Překladač Bing](https://www.bing.com/translator/)
- [Převede Google](http://translate.google.com/)

Existuje mnoho dalších k dispozici. Kvalitu strojový překlad obecně není považováno za dobré dostatečně k uvolnění aplikace bez nejprve se zkontrolovat a testovat professional překladatelé nebo rodilí mluvčí.

#### <a name="professional-translation"></a>Profesionální překlad

Existují také professional překladatelské služby, které bude trvat vaší řetězce a distribuujte je do svých vlastních překladatelé vám poskytnou dokončení překlady za úplatu.

Jedním z nejznámějších služby je [LionBridge](http://www.lionbridge.com/). Většina profesionální služby podporovat všechny běžné typy souborů, včetně řetězce, XML, RESX a POT a nákupních objednávek.


## <a name="summary"></a>Souhrn

Tento článek se zavedl některé koncepty měli seznámit s před kterém se nejdřív internacionalizuje vaší aplikace a pak lokalizace vašich prostředků a také zahrnutých jak změnit jazykové předvolby pro každou platformu.

Tyto koncepty je použít pro různé specifické pro platformu a napříč platformami internacionalizace technik, které je možné s funkcí Xamarin.

Pokračujte ve čtení technické podrobnosti pro platformy, které vás zajímají:

- [Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md) napříč platformami lokalizace pomocí souborů RESX.
- [Xamarin.iOS](~/ios/app-fundamentals/localization/index.md) lokalizace nativní platformy.
- [Xamarin.Android](~/android/app-fundamentals/localization.md) lokalizace nativní platformy.



## <a name="related-links"></a>Související odkazy

- [Přehled lokalizace společnosti Apple](https://developer.apple.com/internationalization/)
- [Kontrolní seznam pro Android lokalizace](http://developer.android.com/distribute/tools/localization-checklist.html)
- [Osvědčené postupy pro vývoj aplikací připravených (MSDN)](http://msdn.microsoft.com/library/w7x1y988%28v=vs.90%29.aspx)
