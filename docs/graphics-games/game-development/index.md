---
title: Úvod do vývoj her pro Xamarin
description: Povaha vývoj her pro se může značně lišit od vývoj jiné typy aplikací. Tento článek je úvodem do vývoj her s funkcí technologie, které lze použít s Xamarin.iOS a Xamarin.Android. Poskytuje nejvyšší úrovni diskuzi o tom, jak jsou vytvářeny hry a vzorkování technologií, které jsou k dispozici pro použití s Xamarin.iOS a Xamarin.Android.
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: b2df6d431004bbfa140b6cae1d069404af92c1df
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-game-development-with-xamarin"></a>Úvod do vývoj her pro Xamarin

_Povaha vývoj her pro se může značně lišit od vývoj jiné typy aplikací. Tento článek je úvodem do vývoj her s funkcí technologie, které lze použít s Xamarin.iOS a Xamarin.Android. Poskytuje nejvyšší úrovni diskuzi o tom, jak jsou vytvářeny hry a vzorkování technologií, které jsou k dispozici pro použití s Xamarin.iOS a Xamarin.Android._

Vývoj her může být velmi zajímavé hlavně zadaný jak snadno může být publikování práci na mobilních platformách. Tento článek popisuje koncepty a technologie související s vývoj her, který vám pomůže vytvořit hry, zda chcete vytvořit vysoce kvalitní AAA herní nebo jenom k programu pro zábavu je.

Tento článek obsahuje následující témata:

- **Herní oproti koncepty programování bez herní** – jsme budete prozkoumat některé koncepty, které jsou buď jedinečné pro vývoj her, nebo jsou sdíleny s jinými typy vývoj ale si zaslouží zvýraznění z důvodu jejich důležitosti.
- **Herní vývojový tým** – v této části vypadá na různých rolích v týmu herní vývojářů.
- **Vytváření herní nápad** – v této části vám pomůžou vytvořit nové herní nápad – prvním krokem při vytvoření nové hry.
- **Technologie vývoj her** – zde některé dostupné napříč platformami technologie, které může zvýšit produktivitu jako vývojář herní budete seznamu jsme.


# <a name="game-vs-non-game-programming-concepts"></a>Herní vs. Programování konceptů bez hra

Přesun do vývoj her pro programátory jsou často čelí nových konceptů a vývojové vzory. Tato část představuje podrobný pohled některé koncepce.


## <a name="the-game-loop"></a>Herní smyčky

Typické herní vyžaduje konstantní přesunu, nebo změňte neděje na obrazovce v reakci na interakci s uživatelem a automatické herní logiku. Toho je dosaženo pomocí co se obvykle označuje jako *herní smyčky*. Herní smyčka je nějaký typ opakování ve smyčce prohlášení (například dobu smyčku), který se spouští na velmi vysokou frekvenci, jako je například 30 a 60 *snímků za sekundu*.

Toto je diagram jednoduché smyčky herní:

![](images/image1.png "Toto je diagram jednoduché herní smyčky")

Technologie, které probereme níže se rychle abstraktní skutečné chvíli smyčky, ale i přes tato abstrakce koncept aktualizace každých rámce, bude k dispozici.

Výkon kódu můžete prioritu i nejjednodušším her. Příklad: funkci, která přebírá 10 milisekund k provedení může mít významný dopad na výkon hry –, zvlášť pokud je vyvolána více než jednou za snímek. Pokud vaše hra běží na 30 snímků za sekundu pak to znamená, že každý snímek musí být spuštěn v části 33 milisekundách. Naopak takové funkce i pravděpodobně patrné, pokud se provede jenom v reakci na kliknutí na tlačítko v aplikaci bez hra.

Mezi běžné typy logiky, která může být provádí každých rámce, patří:

- **Čtení vstupní** – hra potřebovat zjistit, pokud má uživatel zpracoval hru kontrolou vstupní hardwaru, jako je například dotykové obrazovky, klávesnice, myš nebo herní zařízení.
- **Přesun** – objekty, které přesunout z jednoho místa na jiný přesune obvykle velmi malé částka každý rámeček umožňuje iluzi plynulá práce pohybu.
- **Kolizí** – vyžadují mnoho hry časté testování jestli jsou různé objekty překrývající se nebo protínající se operátory. Jsme zaměříme kolizí do větší hloubky v další části v tomto článku. Přesouvání a kolizí může zpracovávat fyzický systém simulace.
- **Kontrola podmínky specifické** – stav hru lze řídit určité podmínky, například zda hráč dosáhl dostatek bodů nebo jestli má přiděleném čase doba skončí.
- **Chování AI** – každých rámce logiky, která slouží k řízení chování objektů, které nejsou řízené přehrávač, jako je například patrolling enemy nebo přesun soupeř ovladače kolem oválný.
- **Vykreslování** – Většina her zaktualizuje, co se zobrazí na obrazovce každý snímek. Tuto akci lze provést v reakci na změny, které mají dopad na hraní her (například znak procházení úroveň) nebo jednoduše zajistit visual Polština (například klesající sníh nebo animovaný ikon).

Mějte na paměti, řadu aktivit uvedených výše můžete změnit stav celé aplikace, zatímco mnoho bez herní aplikace mívají změny stavu v reakci na události vyvolaných.


## <a name="content-loading-and-unloading"></a>Obsahu načítání a uvolňování

V závislosti na technologii, kterou používáte v vývoj, může být potřeba ručně načítání a uvolňování (nebo uvolnění) obsah. Ručně načítání a uvolňování prostředků může být nutné několik důvodů:

 - Prostředky, může trvat dlouhou dobu načíst relativně k délce jeden snímek. Některé prostředky může trvat i načtení, sekund, které by vážně přerušit zkušenosti, pokud načíst mid hraní her. Pokud čas načítání je zvlášť zdlouhavé (například dvě nebo více než jedna sekunda) můžete zobrazit animovaný načítání obrazovky nebo průběhu panelu.
 - Prostředky může využívat velké množství paměti RAM, vyžadování active správu co je načtena nevejde se do co poskytované ve hře cílové platformy.
 - Hry může třeba zobrazit další prostředky, než lze zobrazit v paměti RAM. "Otevřít World" hry často patří velkých prostředích, které přehrávače lze hladce Procházet – se s žádné načítání obrazovky. V takovém případě musíte vytvořit vlastní systému streamování obsahu a správu využití paměti.

Zpracování v okamžiku načtení vyžadování kódu vlastní načítání může být nutné vlastních formátů souborů.


## <a name="math"></a>Matematické

Mnoho hry vyžadují pokročilejší Matematika než jiné herní aplikace. Úroveň matematické samozřejmě závisí na složitosti příslušnou hru. 3D hry obecně vyžadují další matematické než 2D. Naštěstí můžete začít používat vždy s jednoduché hry a zjistěte, jak můžete přejít. Vývoj her může být skvělým způsobem, jak další matematické!

Pokud jste obeznámeni s roviny kartézský – používající souřadnice X a Y umístění objektů – pak víte, dostatek začít pracovat s vývoj her. Na obrázku kartézský rovině s kladné Y ukazující směrem nahoru:

![](images/image2.png "Ukazuje to kartézský rovině s kladné Y přejdete nahoru")

> [!IMPORTANT]
> Některé moduly nebo rozhraní API pomocí souřadnicový systém, kde zvýšení hodnoty Y objektu přesune ho dolů, při jiných systémů používat souřadnicový systém, kde je kladné Y nahoru. Mějte to na paměti, pokud jste přesunutí mezi systémy.
Trigonometrické funkce (například sinus a kosinus) běžně se používají v 2D hry, které implementují třídu jakoukoli formu otočení.



Pokud plánujete vytváření 3D herní pak budete pravděpodobně muset se seznamte s koncepty z lineární Algebra (pro otáčení a přesun v 3D prostoru) a také některé Calculus (pro implementaci akcelerace).


## <a name="content-pipelines"></a>Kanály obsahu

Termín *obsahu kanálu* odkazuje na poslední formát při použití v hry pro proces, který soubor má získat z jeho formátu při vytvořené (jako je soubor bitové kopie .png). Koncová formát závisí, na který typ obsahu, se používá a které také technologie je používána pro obsah k dispozici.

Některé kanály obsahu může být velmi rychlé a vyžadují žádné úsilí. Například většina herní moduly a rozhraní API můžete načíst formát souboru .png v nezpracovaném formátu. Na druhé straně složitější formátů (například 3D modelů) může potřebovat ke zpracování na odlišném formátu než načítá a zpracování může trvat nějakou dobu v závislosti na velikost a složitost assetu.


# <a name="game-development-teams"></a>Herní vývojové týmy

Vývoj her zavádí nové role a produktů pro jednotlivce, tento proces. Většina vývojářů herní nejsou vyhovět širokou škálu dovednosti potřebné k uvolnění úplné hry, tak počet disciplíně neexistuje. Mějte na paměti, že to není úplný seznam oblastí vývoj – jen některé z běžnějších ty.

- **Programátory** – čtení v tomto článku bude spadat do této kategorie patří většina lidí. Role programátory v vývoj her je podobná role pro programátory v jiných herní aplikace. Odpovědnosti zahrnují zápis logiku pro řízení toku ve hře, vývoj systémy pro běžné úlohy v kontextu daného projektu, přidávání a zobrazování obsahu a – samozřejmě – oprava chyb.
- **2D umělcem** – 2D umělci jsou zodpovědní za vytváření *2D prostředky*. Mezi ně patří soubory bitových kopií pro grafické uživatelské rozhraní ve hře, částice, prostředí a znaky. Pokud hra, které vyvíjíte 3D, nemusí být zodpovědná za prostředí a znaky 2D umělci. Můžete najít volné obrázky pro vaše hra na [ http://opengameart.org/ ](http://opengameart.org/) .
- **3D umělci** – 3D umělci jsou zodpovědní za vytváření *3D prostředky*. Mezi ně patří 3D modely pro prostředí, znaků a props (nábytku, zařízení a jiných inanimate objektů). Některé týmy rozlišit mezi 3D umělci a tvůrci 3D animací v závislosti na velikosti týmu. Můžete najít volné 3D obrázky pro vaše hra na [ http://opengameart.org/ ](http://opengameart.org/) .
- **Her Návrhář** – herní Designer jsou zodpovědní za definování, jak se přehrávají hra. To může zahrnovat základní rozhodnutí, například nastavení hra, obecným cílem hry a jak přehrávač prochází hra. Herní Designer může být také účastnící se velmi podrobné rozhodnutí například mapování vstup na akce, definování koeficienty pro přesun nebo úroveň ups a návrhu úrovně rozložení. Mějte na paměti, termín *Návrhář* mohou odkazovat na herní designer nebo vizuálního návrháře v závislosti na kontextu.
- **Zvukových Návrhář** – zvukové Designer jsou zodpovědní za prostředky zvuk hra. Některé týmy mohou rozlišovat mezi jednotlivce zodpovědný za vytváření zvukové efekty a autoři hudby, zatímco menší týmy může mít za všechny zvuk jeden uživatel.


# <a name="creating-a-game-idea"></a>Vytváření herní nápad

Navrhování hru se může zobrazit být snadné provést – po všech Jediným požadavkem je, "Zkontrolujte něco zábavné." Bohužel se celá řada vývojářů najít sami se ztrátou okamžiku vytvoření představu, ze kterého chcete spustit vývoj.

Disciplíně herní návrhu není vysvětlené snadno a vyžaduje, aby postup pro zlepšení stejně jako na techniky nebo nemá programování, ale v této části můžete spustit na cestu.

Nové vývojáři měli začněte v malém rozsahu. Může být obtížné odolat riziko remake velkých, moderní video hře, ale menší hry může být lepší prostředí učení a rychlejší průběh zajišťuje pro více díky následujícím prostředí.

Mnoho hry, jak pro učení, jakož i komerční hry, začněte jako zlepšování nebo změnu existující hra. Jeden způsob generování nápady je podívejte se na jiné hry pro inspiraci. Můžete například zvážit hru, která chcete osobní a pokuste se identifikovat jaké charakteristiky hraní her zajistěte zábavné. To může být zkoumání ovládání ve hře mechanismy nebo pokročíte prostřednictvím scénáře. Nezapomeňte si vzít v úvahu "retro" hry také při hledání nových nápadů.

Jiné technika pro generování nových nápadů je vzít v úvahu konkrétní genre, například stavebnice hry, hry strategie nebo platformers. Pro vývojáře genre může poskytují dobrý výchozí bod.

Nové provedení existující hry je také výukových prostředí, i když to může omezit obchodní realizace dokončení produktu. Proces vytvoření hry, i na takovou, která je přesný klonování, poskytuje cenné výukových prostředí.


# <a name="game-development-technology"></a>Vývoj her pro technologie

Vývojáře, kteří používají Xamarin.Android a Xamarin.iOS mít řadu různých technologií, které jsou k dispozici je pomoct vývoj her. V této části se popisují některé nejčastěji používané řešení pro různé platformy.


## <a name="cocossharp"></a>CocosSharp

CocosSharp je open source, a platformy verze Kokosové 2D herní stroje. Modul poskytuje přístup k Android, iOS, Mac OS X, Windows Desktop, Windows RT a Windows Phone.

CocosSharp se zaměřuje na jednoduchý programátory rozhraní API pro vývoj her pro 2D. Růst v herní na mobilních zařízeních pomohlo reignite v době Oblíbené 2D vývoj her pro provedení CocosSharp vhodným technologie pro koníčku a komerční projekty agentem. Je k dispozici jako soubory zdrojového kódu nebo .dll, (které můžete získat prostřednictvím balíčku NuGet) ale nenabízí o vizuální editor; interakce s modulem CocosSharp proto vyžaduje znalosti programování.

Chcete-li začít pracovat s CocosSharp, podívejte se na naše [CocosSharp příručky](~/graphics-games/cocossharp/index.md).

Hry dané Ninjas je vytvořena s CocosSharp a může být to dobrý výchozí bod Pokud hledáte již spuštěnou herní pro různé platformy:

![](images/image3.png "Hry dané Ninjas byl vytvořen s CocosSharp")

Můžete ho stáhnout a získat další informace [AngryNinjas Github stránce](https://github.com/xamarin/AngryNinjas).


## <a name="monogame"></a>MonoGame

MonoGame je open source, křížové platformy verzi rozhraní API XNA společnosti Microsoft. MonoGame můžete použít k nastavení hry pro iOS, Android, Mac OS X, Linux, Windows, Windows RT a Windows Phone.

Na rozdíl od CocosSharp MonoGame je technicky není herní modul, ale spíše herní vývoj rozhraní API. To znamená, že práci s MonoGame vyžaduje přímo Správa herní objekty, ručně kreslení objekty a implementaci běžných objektů, jako je například kamery a *scény grafy* (nadřazenou hierarchii podřízené mezi herní objekty). Chcete pochopit rozdíl, zvažte, že CocosSharp je postavená na MonoGame. MonoGame umožňuje zobecnit některé specifické pro platformu technologie, jako je například grafiky, vykreslování a audia, zatímco CocosSharp obsahuje kód pro uspořádání a implementace herní logiku.

MonoGame nenabízí standardní vizuální vývojové prostředí, tak práci s MonoGame vyžaduje znalosti programování.

Významné hry pomocí MonoGame příklady:

FEZ:

![](images/image7.png "FEZ")

Bastionu:

![](images/image8.jpg "Bastionu")

Pokud chcete začít pracovat s MonoGame, přejděte na našem [MonoGame příručky](~/graphics-games/monogame/index.md).


## <a name="urhosharp"></a>UrhoSharp

UrhoSharp je napříč platformami vysoké úrovně 3D a 2D modul, který slouží k vytvoření animovaný 2D a 3D scény pro vaše aplikace pomocí geometrie, materiály, indikátory a kamery.

![](images/urhosharp.gif "UrhoSharp je napříč platformami vysoké úrovně 3D a 2D modul, který slouží k vytvoření animovaný 3D a 2D scény")

Podívejte se [UrhoSharp příručky](~/graphics-games/urhosharp/index.md) začít pracovat.

## <a name="additional-technology"></a>Další technologie

Technologie uvedených výše je jenom ukázka technologií, které jsou k dispozici. Další významné technologie patří:

- **Pohyblivý symbol Kit** – Xamarin poskytuje podporu pro framework pohyblivý symbol Kit herní společnosti Apple, která umožňuje přístup ke všem funkci nativní rozhraní API. Vzhledem k tomu, že pohyblivý symbol Kit je technologie, které jsou vytvořené Apple, poskytuje těsná integrace se zbytkem ekosystému iOS. Samozřejmě pohyblivý symbol Kit není napříč platformami proto jej nelze použít v systému Android. Další informace o použití Kit pohyblivý symbol najdete v tomto blogu:  [http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Scény Kit** – Xamarin taky poskytuje podporu pro framework scény Kit společnosti Apple, který zjednodušuje implementace 3D grafický do aplikací pro iOS. Scény Kit je také technologie poskytovaných společností Apple, takže má integrace i požadavky specifické pro platformu uvedených výše pro pohyblivý symbol Kit. Další informace o scény Kit najdete v tomto blogu: [http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK –** OpenTK (který zastupuje otevřete nástroj Kit) poskytuje nízké úrovně OpenGL přístup pro iOS, Apple a Mac hardwaru. Další informace o OpenTK najdete na hlavní stránce:  [http://www.opentk.com/](http://www.opentk.com/)


# <a name="summary"></a>Souhrn

Tento článek popisuje hlavní koncepty vývoj her a poskytuje informace o tom, jak začít vytvářet první hru. Jakmile dokončíte Tento článek, další kroky jsou vyberte technologie a začít pracovat prostřednictvím našich série kurzů propojený v příslušné části výše.

## <a name="related-links"></a>Související odkazy

- [CocosSharp příručky](~/graphics-games/cocossharp/index.md)
- [MonoGame příručky](~/graphics-games/monogame/index.md)
- [UrhoSharp příručky](~/graphics-games/urhosharp/index.md)
