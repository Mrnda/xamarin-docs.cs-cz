---
title: Principy SiriKit koncepty
description: Tento článek popisuje klíčové koncepty, které budou potřebné pro práci s SiriKit v aplikaci pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 56325345204cd2017d688375d9d51c5c83f15e26
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="understanding-sirikit-concepts"></a>Principy SiriKit koncepty

_Tento článek popisuje klíčové koncepty, které budou potřebné pro práci s SiriKit v aplikaci pro Xamarin.iOS._


Nové iOS 10 SiriKit povoluje aplikace Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele na zařízení s iOS pomocí Siri a aplikace mapy. Tato funkce je uvedený v jedné nebo více rozšíření aplikace pomocí nového **záměry** a **uživatelského rozhraní záměry** architektury.

Umožňuje aplikaci pro iOS k poskytování služeb, které jsou dostupné pro uživatele pomocí Siri a mapy aplikace na zařízení s iOS pomocí rozšíření aplikace a nové SiriKit **záměry** a **záměry uživatelského rozhraní** architektury.

Siri pracuje s koncept **domény**, skupiny víte akce pro související úlohy. Každý interakce, který má aplikace s Siri musí spadat do jednoho z jeho domény známé služby následujícím způsobem:

- Zvuk nebo video volání.
- Rezervace pravé.
- Správa cvičení nebo.
- Zasílání zpráv.
- Hledání fotografie.
- Odesílání nebo přijímání platby.

Když uživatel odešle žádost siri jeden rozšíření aplikace služby, odešle SiriKit rozšíření **záměr** objekt, který popisuje žádost uživatele spolu s podpůrné daty. Rozšíření aplikace vygeneruje odpovídající **odpovědi** objekt pro v dané **záměr**, s podrobnostmi o tom, jak můžete rozšíření žádost zpracovat.

## <a name="the-intents-and-intents-ui-extensions"></a>Záměry a rozšíření tříd Intent uživatelského rozhraní

Siri a mapy aplikace komunikovat s aplikace služby prostřednictvím dva různé typy rozšíření aplikace:

- **Rozšíření tříd Intent** -poskytuje Siri a mapy s aplikací obsah a provádí úlohy potřebný ke splnění všech podporovaných tříd Intent.
- **Rozšíření tříd Intent uživatelského rozhraní** -poskytuje vlastní uživatelské rozhraní, které se zobrazí pro obsah aplikace v rámci Siri nebo mapy.

Aplikace musí poskytnout podporu SiriKit rozšíření tříd Intent a zodpovídá za poskytování informací, které Siri a mapy můžete k dispozici pro uživatele a pro zpracování, tříd Intent.

Vytváření rozšíření tříd Intent uživatelského rozhraní je volitelná, protože Siri obvykle zpracovává všechny interakce uživatele a má standardní, integrované uživatelské rozhraní pro prezentování informací v každé z podporovaných oblastí. Tím, že poskytuje rozšíření tříd Intent uživatelského rozhraní, můžete použít aplikaci **uživatelského rozhraní záměr** framework nabídne bohatou a vlastní uživatelské rozhraní s obchod, kde je branding aplikace a další informace.

## <a name="siri-and-the-maps-app-role"></a>Siri a roli aplikace mapy

Požadavky mluvené uživatele jsou jazyk zpracovat a sémanticky analyzovat Siri, která převede tyto požadavky, do níž lze provést akci záměry, které může zpracovat rozšíření záměr.

Mapy rozšíření záměr aplikace používá k zobrazení informací v rozhraní mapy v odpovědi na akce uživatele. Například požaduje nedaleko restaurace nebo získávání zkontroluje restaurace aplikace.

Siri a mapy spravovat všechny interakce uživatele a zobrazit výsledky pomocí rozhraní standardní systému. Role rozšíření aplikace je primárně zajistit data, která získá zobrazí. Aplikace může volitelně zadejte rozšíření tříd Intent uživatelského rozhraní a vlastní uživatelské rozhraní k vylepšení výchozí rozhraní systému k dispozici.

## <a name="interacting-with-siri-via-sirikit"></a>Interakci s Siri prostřednictvím SiriKit

Tato část nabídne přehled o tom, jak SiriKit umožňuje uživatelům interakci s využitím Siri. Z důvodu v tomto příkladu budeme používat falešných MonkeyChat aplikace:

[![](understanding-sirikit-images/monkeychat01.png "Ikona MonkeyChat")](understanding-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat udržuje vlastní kontaktní adresáře uživatele přátel, každý přidružený název obrazovky (např. Bobo třeba) a umožňuje uživatelům poslat textové konverzace každý friend podle názvu jejich obrazovky.

Existuje mnoho způsobů, může tento uživatel spustit interakci s aplikací, protože jiné osoby může být stejné odeslán požadavek v mnoha různých formulářích.

Například pokud uživatel chtěli odeslat zprávu o jejich friend Bobo, mohou mít následující konverzace s Siri:

_Uživatel: Hey Siri, odeslat zprávu MonkeyChat._<br />
_Siri: pro kterého?_<br />
_Uživatel: Bobo._<br />
_Siri: Co chcete tedy na Bobo?_<br />
_Uživatel: Pošlete prosím další banánů._<br />

Jinou osobu může být stejné odeslán požadavek s jinou konverzace:

_Uživatel: Odeslání zprávy do Bobo na MonkeyChat._<br />
_Siri: Co chcete tedy na Bobo?_<br />
_Uživatel: Pošlete prosím další banánů._<br />

A jiný uživatel může nastavit i kratší žádost:

_Uživatel: MonkeyChat Bobo pošlete prosím další banánů._<br />
_Siri: Ok odesílání zprávy prosím odesílejte další banánů Bobo na Monkeychat._<br />

Nebo můžete vytvořit stejném požadavku v jiném jazyce:

_Uživatel: MonkeyChat Bobo s'il vous plaît envoyer plus de bananes._<br />
_Siri: Oui, envoi zpráva s'il vous plaît envoyer plus de bananes ve Bobo sur Monkeychat._<br />

Ještě jiný uživatel může být velmi podrobné v jejich konverzace:

_Uživatel: Hey Siri, můžete prosím nemáte mi upřednostnit a spusťte aplikaci MonkeyChat odesílat text se zprávou, pošlete prosím další banánů._<br />
_Siri: pro kterého?_<br />
_Uživatel: Moje nejlepší pal Bobo._<br />

Kromě toho existuje mnoho způsobů, které může Siri odpověď na žádost, některé závislosti na tom, jak byl požadavek:

- **Podržte tlačítko Domů** -Siri bude poskytovat další visual odpovědi s omezenou ústní zpětnou vazbu.
- **Podle "Blogu Hey Siri"** -Siri bude více ústní a zadejte méně visual odpovědi.

Siri je také přizpůsobená pro usnadnění vyhovovaly uživatele a bude komunikovat a reagovat na základě těchto potřeb.

Bez ohledu na to, jak požadavku nebo jak Siri odpoví na žádost Siri zpracovává konverzace s uživatelem a aplikaci (přes jeho rozšíření) poskytuje funkce.

Když uživatel odešle žádost ústní siri, jsou tyto kroky, které bude postupovat podle Siri:

[![](understanding-sirikit-images/monkeychat02.png "Kroky, které bude postupovat podle Siri")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. Nejprve Siri trvá zvuk uživatele **řeči** a převede jej na text.
2. V dalším kroku text bude převeden do **záměr**, strukturovaná reprezentace žádost uživatele.
3. Podle úmyslu, bude trvat Siri **akce** provést žádost uživatele.
4. Nakonec Siri nabídne **odpovědí** (visual i ústní) na základě akce prováděné uživateli.

Existují tři hlavní způsoby, které aplikace lze použít při uživatele konverzace s Siri:

[![](understanding-sirikit-images/monkeychat03.png "Tři hlavní způsoby, že aplikaci lze použít při uživatelé konverzace s Siri")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Slovník** – to je, jak aplikaci informuje Siri slova, je nutné vědět, abyste mohli pracovat s ním.
2. **Aplikace logiky** – jedná se o akce a odpovědi, které bude trvat aplikace na základě dané tříd Intent.
3. **Uživatelské rozhraní** -Toto je volitelné, vlastní uživatelské rozhraní, které aplikace můžete udělit jeho odpovědi v.

### <a name="example"></a>Příklad

Výše uvedené informace, zkontrolujte, jak by následující konverzace komunikovat s MonkeyChat aplikace:

_Uživatel: Hey Siri, send a zprávy Bobo na MonkeyChat._<br />
_Siri: Co chcete tedy na Bobo?_<br />
_Uživatel: Pošlete prosím další banánů._<br />

První role, která přebírá aplikace konverzace je pomoct Siri pochopit řeči uživatele:

[![](understanding-sirikit-images/monkeychat04.png "Pomáhá pochopit řeči uživatelé Siri")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri nemá název "Bobo" ve své databázi, ale aplikace nepodporuje a s Siri přes jeho termínů sdílí tyto informace. Aplikace také pomáhá rozpoznat, že je Bobo příjemce, protože není zadán k Siri jako Siri *kontaktujte*.

Siri ví, že více je nutný k odeslání zprávy než právě příjemce, tak rychle zkontroluje s příponou aplikace zda zprávu vyžaduje obsah. Vzhledem k tomu, že nemá MonkeyChat, Siri bude reagovat na uživatele s na otázku: *"Co chcete tedy na Bobo?"*

V uvedeném příkladu uživatel reagoval, *"Pošlete prosím další banánů"*, který bude Siri sady do strukturovaných **záměr**:

[![](understanding-sirikit-images/monkeychat05.png "Siri bude do strukturovaných záměr sady odpověď uživatele")](understanding-sirikit-images/monkeychat05.png#lightbox)

Strukturované záměr bude obsahovat tyto informace:

- **Doména:** zprávy
- **Záměr:** sendMessage
- **Příjemce:** Bobo
- **Obsah:** pošlete prosím další banánů

Každá doména má jako sadu vědět *akce* , můžete provést v nich a na základě domény a akci, nula na mnoho parametrů může být součástí záměr odesílají do aplikace.

Záměr se pak posílají do rozšíření aplikace pro zpracování. Výsledkem je zpracování záměr, bude generovat aplikace **IntentResponse** které budou seskupeny se záměrem a obsahovat parametry, které popisují, co aplikace aplikace se záměrem.

Každý IntentResponse budou taky zahrnovat **kód odpovědi** který sděluje Siri, pokud aplikace bylo možné dokončit žádost, nebo ne. Některé domény mají velmi konkrétní chybové odpovědi kódy, které lze také odeslat.

Nakonec bude obsahovat IntentResponse `NSUserActivity` (třeba používaných pro podporu ruční Off). `NSUserActivity` Se použije ke spuštění aplikace, pokud odpověď vyžaduje, aby nechte Siri prostředí a zadejte aplikaci dokončit, protože se. 

Siri automaticky sestavit odpovídající `NSUserActivity` spusťte aplikaci a vyzvednutí tam, kde uživatel přestali v prostředí Siri. Ale aplikace může poskytovat vlastní `NSUserActivity` s přizpůsobit informace, pokud se vyžaduje.

Po zpracování záměr a vrátí odpověď Siri aplikace má ji potom zobrazí výsledky uživateli (ústně i vizuálně):

[![](understanding-sirikit-images/monkeychat06.png "Výsledky se uživateli zobrazí ústně i vizuálně")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri má několik předdefinovaných odezvy uživatelského rozhraní pro každé z domén, které jsou k dispozici pro aplikaci. Ale vzhledem k tomu, že MonkeyChat poskytl volitelné rozšíření záměr uživatelského rozhraní, použije se na výsledky konverzace pro uživatele ve výše uvedeném příkladu.

## <a name="the-intent-lifecycle"></a>Záměrné životního cyklu

Existují tři hlavní úkoly, které rozšíření aplikace bude potřeba udělat při plánování práce s záměry:

[![](understanding-sirikit-images/monkeychat07.png "Záměrné životního cyklu")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. Aplikace musí **vyřešit** každý parametr na události. Aplikace v důsledku toho bude volat vyřešte několikrát (jednou za každý parametr) a někdy víckrát na stejný parametr dokud aplikaci a uživatele dohodnou na co je požadováno.
2. Aplikace musí **potvrdit** , můžete zpracovat požadovaný záměr a řekněte Siri o očekávaný výsledek.
3. Nakonec musí aplikace **zpracování** záměr a postupujte podle kroků k dosažení požadovaný výsledek.

### <a name="the-resolve-stage"></a>Vyřešte fáze

Fáze vyřešte pomáhá pochopit hodnoty, které uživatel poskytl a zajišťuje, že to, co uživatel ve skutečnosti čeho co se stane, když aplikace zpracovává záměr Siri. 

Tato fáze taky poskytuje možnost pro aplikaci k ovlivnění Siri na chování při konverzace s uživatelem. K tomuto účelu bude poskytovat aplikaci **odpovědi řešení**. Existuje několik předdefinovaných odpovědi na různé typy dat, která funguje s technologií Siri.

Nejběžnější řešení odpověď z aplikace bude **úspěch**, znamená aplikace neodpovídala určitou část dat z parametru (například uživatelské jméno obrazovky) část informace ví o.

Můžou nastat situace, když aplikace potřebuje k potvrzení, že daný požadavek odpovídá správné část informace, které bude vědět o. V těchto případech bude odesílat **ConfirmationRequired** odpověď na zeptejte Ano nebo ne pro uživatele, jako *"Odeslání zprávy do Bobo dobře?"*

Může být ostatních případech, kde bude aplikace vyžadovat, aby uživatel vybrat ze seznamu krátké možnosti. V takovém případě bude poskytovat aplikaci **rozlišení více tras** odpověď se seznamem dvou až deset možnosti pro uživatele můžete například vybrat ze: 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri zpracuje uživatele provedení výběru, ústně nebo interakcí s uživatelským rozhraním Siri a výsledek bude odeslán zpět do aplikace.

V ostatních případech nemusí být dostatek informací pro aplikaci, chcete-li vyřešit parametr nebo může být příliš mnoho odpovídajících položek přeložit pomocí rozlišení více tras (třeba 80 uživatele s Bobo v jejich název). V této případech bude odesílat aplikace **NeedsMoreDetails** odpovědi a Siri vyzve uživatele, které mají být konkrétnější.

Pokud uživatel nezadal hodnotu, která je pro něj potřeba proces záměr, kterou může odesílat **NeedsValue** odpovědi tak, aby měl Siri požádat uživatele o hodnotě.

Pokud aplikace nepodporuje hodnotu, která uživatel udělil pro specifického parametru, kterou může odesílat **UnsupportedWithReason** odpovědí k zadání důvodu, proč nebyla podporovaná hodnota. Siri pak vyzve uživatele, pro úplně novou hodnotu a daný je z důvodu je požadovaná.

Nakonec použijte **NotRequired** odpovědi Siri říct, že aplikace nevyžaduje hodnotu pro daný parametr. Pokud uživatel poskytuje jednu i přesto, budou se ignorovat jednoduše Siri.

### <a name="the-confirm-stage"></a>Fázi potvrzení

Fázi potvrzení má dva účely:

- Říct Siri očekávaný výsledek zpracování záměrem tak, aby Siri můžete informace pro uživatele co se děje provést.
- Poskytuje kontrolu možnost všechny požadované stavy aplikace možná muset udělat požadavek předložený uživateli, jako má dostatek peníze v bance k provedení požadované platby.

Aplikace bude poskytovat **záměr odpovědi** z kroku potvrdit, který by měl být naplněný jako velkého množství informací aplikace má k dispozici, takže Siri může komunikovat se efektivně s uživatelem.

Podle typu domény a akce, Siri může vyzvat uživatele k potvrzení, například před odesláním platba nebo rezervace pravé.

### <a name="the-handle-stage"></a>Popisovač fáze

Fáze zpracování je nejdůležitější část pracovat se záměrem, protože je bod, kde aplikace splní požadavek uživatele provedením úlohy, kterou byla požádána udělat.

Stejně jako ve fázi potvrzení, aplikace musí zajistit co nejvíce informací o výsledek, tak to Siri můžete souvisí uživatele. Někdy tyto informace se zobrazí vizuálně nebo jindy Siri bude jednoduše řeči zpět na uživatele. 

Můžou nastat situace když aplikace může vyžadovat čas navíc ke zpracování daného požadavku, jako je například volání zpoždění sítě nebo za provozu uživatel potřebuje ke splnění tohoto požadavku (např. dokončení a přesouvání pořadí nebo řídí automobilu na umístění uživatele). Když Siri je čekání na odpověď zpět z aplikace, zobrazí čeká se na uživatelské rozhraní k upozorněním, že aplikace je zpracování požadavku uživatele.

V ideálním případě aplikace by měl poskytovat odpověď Siri během několika sekund dvě až tři nejvíce. Pokud aplikace zná, že dané odpovědi bude trvat delší dobu procesu, musí poslat **InProgress** kód odpovědi na Siri. Siri bude potom informovat uživatele, že aplikace je zpracování požadavku na pozadí a bude pokračovat, i když opustí prostředí Siri.

## <a name="adding-sirikit-to-the-app"></a>Přidání SiriKit do aplikace

S SiriKit v iOS 10 Apple vytvořil dvě nové rozšíření body:

- **Rozšíření tříd Intent** – Siri poskytuje obsah aplikace a provádí úlohy potřebný ke splnění všech podporovaných tříd Intent.
- **Rozšíření tříd Intent uživatelského rozhraní** -poskytuje vlastní uživatelské rozhraní, které se zobrazí pro obsah aplikace v rámci Siri. 

Je také rozhraní API k poskytování slova a slovní spojení Siri, které pomáhají při rozpoznávání ve tvaru:

- **Slovník aplikace** -slova a slovní spojení, které jsou společné pro každého uživatele aplikace.
- **Uživatel termínů** -slova a slovní spojení, které jsou jedinečné pro danou aplikaci uživatele.

## <a name="the-intents-extension"></a>Rozšíření tříd Intent

Rozšíření tříd Intent je zodpovědná za zpracování hlavní interakce mezi aplikací a Siri následujícím způsobem:

[![](understanding-sirikit-images/intents01.png "Rozšíření tříd Intent")](understanding-sirikit-images/intents01.png#lightbox)

Rozšíření záměr může podporovat jeden nebo více tříd Intent, je maximálně vývojáře k rozhodování o tom, jak si přejí implementovat SiriKit v aplikaci. Vývojář může také přidat samostatné záměr rozšíření pro každý záměr museli zpracovávat.  Ale nutné dodat, Apple požadavky, že vývojář omezit počet záměr rozšíření tak, aby Siri nemá otevřete proti aplikace, které vyžadují další paměť a čas pro zpracování více procesů.

Vývojář také měli vědět, že rozšíření záměr bude spuštěna na pozadí, zatímco Siri je aktivní. To umožní Siri vykonávat aktivně konverzace s uživatelem při stále komunikaci s příponou zpracovat informace o požadavku.

## <a name="privacy-and-security-considerations"></a>Aspekty zabezpečení a ochrany osobních údajů

Apple přijalo skvělé opatření zajistit, aby uživatel soukromé informace o zabezpečení při práci s Siri a jako takový existuje několik interakce, které vyžadují uživatele k přihlášení na zařízení s iOS. Například při požadavku pravé nebo vytváření platba.

Kromě toho jsou konkrétní chování, které aplikace může chtít omezit na uživatele se protokolují do zařízení. V takových situacích může požádat o aplikaci **omezení při uzamčení** chování. To se provádí prostřednictvím nastavení `Info.plist` souboru.

Rozhraní místní ověřování není k dispozici pro rozšíření záměr, takže aplikace můžete požádat uživatele, pro další ověřovací informace i v případě, že zařízení jsou již odemknuta.

Nakonec dotykový identifikátor je k dispozici pro rozšíření záměr tak, aby aplikace můžete dokončit transakci pomocí dotykový identifikátor a integrované list dotykový identifikátor se zobrazí nad rozhraní Siri.

Kromě toho chce zajistit, že uživatelé zná, když se odesílají informace do 3. stran aplikace a jako takový uživatel Apple **musí** vyslovte konkrétní název aplikace (jak je uvedeno v aplikace sady zobrazovaný název) při vytváření požadavku.

Apple určený Siri provádět přirozené, plynulá práce konverzace s uživatelem a z tohoto důvodu, název sady aplikace mohou být používány mnoho řeči částí, bez ohledu na její souvislosti přirozeně v požadavku uživatele.

Jedním z běžných činností, které budou uživatelé dělat je "verbify" název aplikace, jinými slovy, trvá název aplikace a jeho použití jako operaci v požadavku. Například *"MonkeyChat Bobo těch, které byly skvělé banánů."*

## <a name="the-intents-ui-extension"></a>Rozšíření tříd Intent uživatelského rozhraní

Rozšíření uživatelského rozhraní záměry uvede možnost převést aplikace uživatelského rozhraní a značka do prostředí Siri a aby uživatele působí připojený k aplikaci. S touto příponou aplikace můžete zahrnout značky, jakož i visual a dalších informací do zápis.

[![](understanding-sirikit-images/intents02.png "Příklad výstupu rozšíření tříd Intent uživatelského rozhraní")](understanding-sirikit-images/intents02.png#lightbox)

Vždy vrátí uživatelského rozhraní rozšíření tříd Intent `UIViewController` a aplikace můžete přidat nic ho líbí uvnitř řadiče zobrazení jako je například zobrazující Další informace, které překročí počáteční odpovědi. Rozhraní záměry můžete také aktualizovat uživatele stavem dlouhotrvající události, jako je například jak dlouho bude trvat pravé, sdílení car k dosažení jejich umístění.

Rozšíření tříd Intent uživatelského rozhraní bude vždy zobrazí společně s další obsah Siri jako ikona aplikace a název v horní části uživatelského rozhraní nebo, podle úmyslu, tlačítka (například odeslání nebo zrušit), může se zobrazit v dolní části.

Existuje několik instancí, kde aplikace může nahradit informace, které Siri je zobrazení pro uživatele ve výchozím nastavení, jako je zasílání zpráv nebo mapy, kde aplikace může nahradit výchozí prostředí s jednou přizpůsobit aplikaci.

> [!IMPORTANT]
> Když je možné přidat interaktivní prvky, jako `UIButtons` nebo `UITextFields` do uživatelského rozhraní rozšíření záměr `UIViewController`, jsou výhradně zakázané jako rozhraní záměr v neinteraktivním a uživatel nebude moci komunikovat s nimi.

Je zcela volitelné pro aplikaci zadejte rozšíření záměr uživatelského rozhraní od Siri obsahuje výchozí sadu uživatelského rozhraní pro každý typ záměrné. Kromě toho rozhraní záměry uživatelského rozhraní jsou dostupné jenom pro některé, že se považují za Apple záměry mohou být užitečné pro uživatele.

## <a name="adding-sirikit-vocabulary"></a>Přidání SiriKit termínů

Poslední díl implementace SiriKit spočívá v rámci aplikace poskytnutím požadované termínů. Mnoho aplikací mít jedinečný způsobů popisující informace pro uživatele a jedinečné způsoby, uživatel bude poskytovat informace k aplikaci.

Z tohoto důvodu vyžaduje Siri aplikace pomoci porozumět slova a slovní spojení, které jsou jedinečné pro aplikaci. Některé z těchto frází budou součástí aplikace tak, aby každý uživatel, bude vědět a pochopit. Ještě jiné bude jedinečný pro daného uživatele aplikace.

### <a name="app-specific-vocabulary"></a>Slovník konkrétní aplikace

Slovník konkrétní aplikace definuje konkrétní slova a slovní spojení, které bude známé pro všechny uživatele aplikace, jako jsou typy vozidel nebo názvy cvičení. Protože se jedná o část aplikace, jsou definovány v `AppIntentVocabulary.plist` souboru jako součást sady hlavní aplikace. Kromě toho se lokalizované tyto slova a slovní spojení.

Existuje několik částí do slovníkový `AppIntentVocabulary.plist` souboru:

- **Příklad aplikace používá** -tyto poskytují sadu běžné případy použití pro požadavky, které může uživatel aplikace. Příklad: *"Start cvičení s MonkeyFit."*
- **Parametry** -tyto poskytují sadu typy bez standardní parametrů, které jsou specifické pro aplikaci. Například cvičení názvy MonkeyFit aplikace. Tyto obsahovat:
    - **Fráze** – umožňuje aplikaci zadejte jedinečný podmínky pro aplikaci. Například: "Bananarific" typ cvičení MonkeyFit aplikace. 
    - **Výslovnosti** -poskytuje pomocné parametry výslovnosti k Siri jako jednoduchý Výslovnost pro danou frázi. Například "ba nana ri zjistit".
    - **Příklad** -poskytuje příklad použití dané frázi v aplikaci. Například *"Spustit Bananarific v MonkeyFit"*.

Další informace najdete v tématu společnosti Apple [referenční informace k aplikacím termínů soubor formátu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

### <a name="user-specific-vocabulary"></a>Slovník pro konkrétní uživatele

Slovník konkrétní uživatel bude poskytovat slova nebo fráze, které jsou jedinečné pro jednotlivé uživatele aplikace. Toto bude poskytnuta v době běhu z hlavní aplikace (ne rozšíření aplikace) jako seřazený sadu podmínek seřazené nejvýznamnějších Priorita využití pro uživatele s podmínkami nejdůležitější na začátek seznamu.

Podívejte se na příklad aplikace MonkeyChat uvedené výše. MonkeyChat udržuje seznam všech kontaktů uživatele, které bude odesílání Siri prostřednictvím termínů konkrétní uživatele. Také udržuje seznam 10 nejnovější kontakty, které má messaged uživatel a má sadu kontakty oblíbená položka pro každého uživatele. V tomto příkladu Oblíbené kontakty musí být na začátku konkrétní termínů naše uživatele, za nímž následuje poslední kontakty pak zbytek kontakty uživatele.

Slovník pro konkrétní uživatele podporuje následující typy informací:

- Obraťte se na názvy.
- Cvičení názvy.
- Názvy alba fotografií.
- Klíčová slova fotografii.

Pokud aplikace závisí na iOS adresáře, aplikace nebude mít provádět žádnou akci, protože tyto informace je již k dispozici pro Siri. Aplikace stačí zadat kontaktní názvy, pokud má svůj vlastní jedinečný databáze kontaktů.

Při navrhování slovníku, zadejte pouze nezbytné hodnoty, které uživatelé vědět a zajímají. Vyhněte se poskytuje informace, jako jsou telefonní čísla nebo e-mailové adresy.

Aplikace také musí aktualizovat Siri rychle, pokud slovník konkrétní uživatel změní. Uživatelé jsou zvykli na vyžádání informace z Siri pro rychlé byl přidán do jejich zařízení iOS. Například pokud uživatel přidá nový kontakt v aplikaci, odesílat tyto informace k Siri Jakmile uživatel uloží ho.

Je důležité, aplikace _musí_ odstranit informace z termínů Siri neprodleně vzhledem k tomu, že uživatel může stát narušit, pokud je odstraněn část informace, ale Siri byla stále rozpozná ho hodin nebo dnů později.

> [!IMPORTANT]
> Aplikace musí odebrat všechny konkrétní termínů uživatele z Siri Pokud se uživatel rozhodne resetovat aplikace, nebo pokud se odhlásit.

## <a name="sirikit-permissions"></a>SiriKit oprávnění

Poslední díl SiriKit je zaměřená na oprávnění. Podobně jako ostatní funkce iOS (například fotografie, fotoaparát nebo kontakty), uživatelé mají udělit výslovná oprávnění pro aplikaci, aby komunikoval s Siri.

Tato aplikace je schopný poskytnout řetězec definující, které informace bude poskytovat Siri a poskytnout důvod, proč má uživatel udělit přístup. 

Apple naznačuje, že aplikace musí požádat o oprávnění z uživatelům použít první Siri, když uživatel otevře aplikaci po upgradu na iOS 10. Toto je tak, aby uživatelé vědět o integraci Siri a můžete předběžně schválené využití předtím, než se provede první žádosti.

## <a name="sirikit-and-maps"></a>SiriKit a mapy

Využívá rozhraní větší záměry přidány do systému iOS 10 SiriKit je nedílnou součástí iOS. Rozhraní framework záměry byl navržen sdílet s dalšími částmi systému běžné a sdílené akce a tříd Intent.

Rozhraní framework záměry překročí právě Siri integrace a poskytuje další funkce, například kontakty integrace, kde aplikace může stát výchozí telefonickou nebo zasílání zpráv aplikace pro konkrétní kontakty. Záměry také poskytují těsná integrace s CallKit uživatelům poskytnout nejlepší VOIP prostředky.

Mapy aplikace v iOS 10 přidal funkce, jako je pravé sdílení, kde se uživatel může sešit pravé přímo v rámci mapy uživatelského rozhraní. SiriKit poskytuje společný bod rozšíření s mapy, takže pravé sdílení (a dalších) záměry lze sdílet mezi Siri a mapy. 

To znamená, že pokud aplikace přijal SiriKit rozšíření, se zobrazí také v integrace mapy zdarma. 

## <a name="designing-a-great-siri-experience"></a>Navrhování optimálního uživatelského prostředí Siri

Navrhování vysoký výkon uživatele při integraci aplikace do Siri se liší od návrhu skvělá aplikace uživatelské rozhraní. Na rozdíl od normální situacích, kde je uživatel interakci s aplikace přímo na obrazovce při používání Siri existuje jsou tolikrát, kolikrát v případě žádné vizuální rozhraní je viditelná vůbec. Například pokud uživatel bylo zahájeno konverzace s *"Blogu Hey Siri"*.

### <a name="how-siri-helps-the-developer"></a>Jak Siri pomáhá vývojářům

Při navrhování aplikace interakce s Siri, aplikace bude vytváření *konverzačního rozhraní*, což znamená, kontext je odvozený od konverzace, který má Siri s uživatelským jménem aplikace.

Chybí vizuální informace uživatel musí udržovat přehled o informace se zobrazí v jejich head. Z toho důvodu Siri uvádí úplné minimální informace potřebné k dosažení úlohu, uživatel je chtějí dosáhnout.

Rozhraní konverzačního je ve tvaru otázky a odpovědi od uživatele a Siri během konverzace. Proto je důležité si myslíte o tom, jak Siri kladení otázek a odpovědí při navrhování toto rozhraní.

Proveďte následující příklad uživatele vytvoření zprávy, Siri může odpovídat na otázku *"Připravení odeslat?"* . Uživatel může reagovat, jako v mnoha různými způsoby *"Odeslat"*, *tlačítko Storno"* nebo i něco zcela nezávislé na tuto otázku. Bez ohledu na to, jak konverzace hraje Siri bude zpracování pro aplikaci a pouze poslat příslušné informace jakmile je k dispozici.

Existuje několik různých způsobů, uživatel může spustit konverzaci s Siri:

- Podle vyzvednutí zařízení, kliknutím na tlačítko Domů. V této situaci Siri nabídne další visual rozhraní a méně ústní odpovědi.
- Vyslovením *"Blogu Hey Siri"* a spuštění rukou volné konverzace. V takovém případě bude Siri méně visual a více ústní.
- Pomocí funkce usnadnění například bluetooth povolit sluchu pomůcky, kde bude přizpůsobit uživatelského rozhraní pro uživatele se zvláštními potřebami.
- Pomocí Car Play, kde uživatel musí zachovat jejich pozornost zaměřuje na řídí udržováním vyrušení na minimum.

### <a name="how-the-developer-helps-siri"></a>Jak vývojář pomáhá Siri

Při integraci aplikace s Siri, musí vývojář testovací Tato integrace často a ujistěte se, že se tím, že požádá pro stejnou část informace nebo úkolu jako v mnoha různými způsoby podle možnosti přijímání mnoho různých požadavků.

Vzhledem k tomu, že žádné dva uživatelé Považujte agentem, je velmi důležité, že vývojář získá jako v mnoha různých beta testery nejdříve pomohou optimalizovat integrace Siri. Může požádat uživatele o informace nebo zkontrolujte požadavky způsoby vývojář nikdy z a tento doladění může pomoci zajistit, že nejširší skupiny uživatelů mají optimálního uživatelského prostředí pomocí svou aplikaci Siri.

Testování v různých situacích a prostředí. Zahájit konverzace s Siri ve všech způsoby možné zajistit, že tyto konverzace zůstat kapaliny a přirozeně. Testování v umístění, kde uživatel by více než pravděpodobně používat aplikace, jako je ve frekventované sebou.

Ujistěte se, že aplikace poskytuje všechny informace, že musí správně představují požadavku a výsledku uživateli Siri. To platí hlavně při používání Siri v situaci, rukou volné.

### <a name="siri-design-guidelines"></a>Pokyny pro návrh Siri

Vždy, zda je Siri s konverzace s uživatelským jménem aplikace. Vývojář chce jistí, že tuto konverzaci zůstává jako plynulá práce a přirozené nejblíže.

Stejně jako s všechny důležité konverzace, musí vývojář zkontrolujte následující:

- Když je aplikace připravená pro konverzace.
- Aby aplikace naslouchá přesně to, co uživatel se pokouší provést.
- Aby aplikace vás odpovídající ptát v příslušnou dobu.
- Aby aplikace odpoví na žádost s informacemi, které je uživatel vyhledávání.

#### <a name="preparing-for-the-conversation"></a>Příprava konverzace

První věc zapamatujte si je, že uživatelé aplikace nejsou má být úplně stejně jako vývojář. Může pocházejí z různých pozadí řeči různé jazyky nebo se zvláštními potřebami při práci s aplikací.

Navíc vzhledem k tomu, že vývojář navržen a sestaven aplikace, mají hluboké, dokonalou znalosti, jak aplikace a jeho vnitřního chodu a funkce, které nebudou mít běžný uživatel. Vývojář tak může požádat požadavek siri jinak než běžné uživatele.

Z tohoto důvodu je důležité mít mnoho různých uživatelů nejdříve interakci s aplikaci pomocí Siri. Uživatelé mohou provádět požadavky aplikace prostřednictvím Siri, které vývojář se nikdy chápat, nebo v způsoby, které vývojář nebylo zvážit.

#### <a name="ensure-the-app-is-a-good-listener"></a>Ujistěte se, že aplikace je dobré naslouchací proces

Vývojář musí zajistit, aby aplikace je dobré naslouchací proces a dochází specifika konverzace, které splňují očekávání uživatele. Ale je také možné, že nemusí mít poskytnou všechny informace, že aplikace vyžaduje k dosažení požadovanou úlohu.

Existuje několik způsobů, že aplikace může zpracovat tuto situaci:

- **Vyberte dobrý výchozí hodnoty chybí** – například pravé, aplikace pro sdílení může výchozí k aktuálnímu umístění uživatele, pokud uživatel nezadal, kde chtěli být zachyceny z.
- **Zkontrolujte odhad kvalifikovaně** -pomocí konkrétní informace, které aplikace se budou shromažďovat na uživatele, aplikace pravděpodobně moci značku a kvalifikovaně odhad na chybějící informace, jako je například vyplnění chybějící mobilní číslo od uživatele kontaktní informace. Ale potřeba dát pozor předejdete chybný výskyt nečekaných událostí, jako je výběr nejnákladnější možnost atd.
- **Výzva pro více informací** -aplikace může mít Siri požádat uživatele o chybí hodnota. Klíč tady je však uchovávání konverzace jednoduché a do bodu. Uživatelé začnou rychle frustrovaní, pokud mají k odpovědi na několik otázek zajistit jeho žádost.
- **Zpracování výmysly řádně** – uživatel může zadat hodnotu, která nebyla očekávána aplikace nebo který nemůže zpracovávat v daném kontextu. Ujistěte se, že aplikace má vztah této situaci uživateli způsobem, který umožňuje vymazat a snadno je opravit.

Když se aplikace s jednu hodnotu, která je nejistá, je upřednostňovaný způsob zpracování to tak, aby měl Siri požádat uživatele o potvrzení. Například *"Měli jste na mysli Bobo dobře?"* , který se může odpovědět na Jednoduchá odpověď Ano nebo ne.

Když je situace, kdy může být několik možných voleb správná pro jednu hodnotu, je rozlišení více tras metodu upřednostňované zpracování. V takovém případě můžete Siri vyzvat uživatele s můžete vybírat až deset způsobů. Příklad:

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Pokud stále v otázku, mají Siri vyzvat uživatele k zadání odpovědí úplně nový, další specifické pro danou hodnotou.

#### <a name="request-final-confirmation"></a>Konečné potvrzení požadavku

Předtím, než aplikace ve skutečnosti provádí úlohy ke splnění požadavku uživatele, Siri zkontroluje s příponou aplikace k zajištění, že všechno, co je na místě. Například uživatel má dostatek peníze v jejich účtu k provedení požadované platby?

Kromě toho aplikace musí zajistit, že poskytuje všechny možné informace k Siri, můžete ho k dispozici pro uživatele a potvrďte, že úloha chystáte provést, splňuje jejich očekávání.

Jakmile uživatel potvrdí žádosti a aplikace byla provedena ho, je nutné aplikaci znovu zajistit, aby ho má všechny výsledky zpět do Siri, můžete spojit, je pro uživatele.

#### <a name="responding-to-the-request"></a>Odpověď na žádost

Siri má několik předdefinovaných uživatelského rozhraní pro jednotlivé domény a akce, které bude vědět o. Ale kde je to vhodné, aplikace může poskytovat vlastního rozšíření záměr uživatelského rozhraní pro obohacení činnost koncového uživatele prezentací branding aplikace a uživatelského rozhraní nebo Další informace, než bylo přítomné v žádosti.

Ale nutné dodat, omezení by měl použít při navrhování vlastních rozhraní pro Siri. Obvykle se uživatel je chtějí získat konkrétní úlohu provést co nejrychleji a nechce být přetíženy s víc informací.

Musí také dát pozor na zajistěte, aby vlastní uživatelské rozhraní vypadá a odpoví správně ve všech zařízení s iOS různých a orientace, že uživatel může mít nebo používat zařízení.

Podle potřeby použijte rozhraní API SiriKit skrýt všechny redundantní údaje již existuje ve výchozím nastavení uživatelského rozhraní Siri. Ještě Ujistěte se, že aplikace je stále zajišťuje informace k Siri, ho můžete zobrazit ústně ve rukou volné situacích.

Můžou nastat situace, kdy Siri spustí aplikaci pro splnění požadavku uživatele, například prezentace fotografie, které požadoval uživatel. V těchto situacích nemáte neohlášeného uživatele. Zobrazí informace o očekávané bez nutnosti zprostředkující kroky nebo další součinnosti. Nikdy zobrazit informace nebo provést úlohu, kterou uživatel není očekáván.

### <a name="polishing-the-design"></a>Leštění návrhu

Existuje několik kroků, které Apple navrhnout polština návrh konverzačního rozhraní. Nejprve je díky jasným a přesným metrikám termínů a použití případu příklady Siri.

Jedním ze způsobů, že uživatel zjistí aplikace je pomocí inicializace konverzace s Siri a požádá, *"Co můžete dělat?"* Siri se zobrazí několik různých věcí, můžete provést, včetně aplikace pro vývojáře a nejdůležitější příklad použijte případů, které má zadaný prostřednictvím jeho `plist` souboru.

Jak napsat případy použití dobrým příkladem:

- Ujistěte se, že příklady název aplikace.
- Ponechat v příkladu krátký a k bodu.
- Zadejte pro každou z tříd Intent, které aplikace podporuje více příkladů.
- Určení priority tříd Intent a příklady v nich podle nejběžnější případy použití pro aplikaci.
- Ujistěte se, že aplikace obsahuje lokalizované příklady.
- Zajistěte, aby každý příklad zadané funguje podle očekávání v rámci aplikace.
- Zabraňte Siri v příkladech, takže nezadávejte text jako *"Blogu Hey Siri..."*
- Například vyhnout všechny nepotřebné pleasantries *"prosím"* nebo *"Děkujeme"*.

Proveďte příslušnou dobu umožní zkoumat a experimentovat na jak aplikace může utvářejí konverzace, který má Siri s uživatelským jménem klienta. Ujistěte se, aby komunikoval s typické uživateli v celém procesu, jako jejich interakce s a očekávání aplikace může časem změnit.

Vždycky mějte na paměti k testování aplikace v různých situacích a všechny různé metody pro vyvolání konverzace s Siri. Test v reálném světě umístění uživatele by mohla využívat aplikaci a mimo office a podpory.

Zajistit, aby konverzace s Siri (jménem aplikace) se kapaliny, přirozený a "cítit právě". 

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých klíčové koncepty nutný SiriKit a zobrazuje, že mohou komunikovat s aplikace Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele na zařízení s iOS pomocí Siri a aplikace mapy.




## <a name="related-links"></a>Související odkazy

- [Ukázka ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Průvodce programováním SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Záměry Framework – referenční informace](https://developer.apple.com/reference/intents)
- [Záměry uživatelského rozhraní Framework – referenční informace](https://developer.apple.com/reference/intentsui)
