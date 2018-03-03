---
title: "Vytváření Android služeb"
description: "Tato příručka popisuje Xamarin.Android služby, které jsou Android součásti, které umožňují práci, kterou jde provést bez rozhraní aktivního uživatele. Služby se velmi často používají pro úlohy, které se provádí na pozadí, jako je časově náročná výpočtů a stahování souborů, přehrávání hudby a tak dále. Ho vysvětluje různé scénáře, které služby jsou vhodné pro a ukazuje, jak pro jejich implementaci, k provedení úlohy na pozadí dlouho běžící i pro zajištění rozhraní vzdáleného volání procedur."
ms.topic: article
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 77a3db2b36a0fb423ac2481fbde80c1a732cac4b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="creating-android-services"></a>Vytváření Android služeb

_Tato příručka popisuje Xamarin.Android služby, které jsou Android součásti, které umožňují práci, kterou jde provést bez rozhraní aktivního uživatele. Služby se velmi často používají pro úlohy, které se provádí na pozadí, jako je časově náročná výpočtů a stahování souborů, přehrávání hudby a tak dále. Ho vysvětluje různé scénáře, které služby jsou vhodné pro a ukazuje, jak pro jejich implementaci, k provedení úlohy na pozadí dlouho běžící i pro zajištění rozhraní vzdáleného volání procedur._

## <a name="android-services-overview"></a>Přehled služby Android

Mobilní aplikace nejsou jako aplikace klasické pracovní plochy. Stolní počítače mají velkým množství prostředků, jako jsou obrazovky nemovitosti, paměti, úložiště a připojené napájení, mobilní zařízení nepodporují. Těchto omezení vynutit mobilních aplikací se bude chovat jinak. Například malou obrazovku na mobilních zařízeních obvykle znamená, že jenom jedna aplikace (tj. aktivita) je zobrazen v čase. Ostatní aktivity jsou na pozadí a vložena do pozastaveném stavu, kde nelze provést veškerou práci. Ale stejně, protože aplikace platformy Android se na pozadí neznamená, že není možné aplikace a pokračovat v práci. 

Aplikace pro Android se skládají nejméně jedné následující čtyři primární součásti: _aktivity_, _vysílání příjemci_, _poskytovatelů obsahu_a _Služby_. Aktivity jsou kamenem mnoho kvalitních aplikací pro Android, protože poskytují uživatelského rozhraní, která umožňuje uživatelům interakci s aplikací. Při rozhodování o provedení souběžné nebo práce pozadí, aktivity jsou však není vždy nejlepší volbou.
 
Primární mechanismus pro práce na pozadí v Android je _služby_. Android služby je komponenta, která slouží k nějakou práci bez uživatelského rozhraní. Služba může stáhnout soubor, přehrání Hudba nebo použít filtr na bitovou kopii. Služby lze také meziprocesová komunikace (_IPC_) mezi aplikací pro Android. Můžete třeba použít jednu aplikaci pro Android hudební přehrávač služba, která je z jiné aplikace nebo aplikace mohou být vystaveny data (například kontaktní informace osoby) do jiných aplikací prostřednictvím služby. 

Služby a jejich schopnost provádět práce na pozadí, jsou klíčové pro poskytování hladký a plynulá práce uživatelského rozhraní. Všechny aplikace pro Android mají _hlavního vlákna_ (také označované jako _vlákna uživatelského rozhraní_) na které se spouštějí aktivity. Aby zařízení reakce, Android musí být schopen aktualizovat uživatelské rozhraní ve výši 60 snímků za sekundu. Pokud aplikace pro Android provádí spoustu práce na hlavní vlákno, pak Android bude vyřadit rámce, což způsobí, že uživatelské rozhraní zobrazí trhané (někdy označují jako _janky_). To znamená, že by se měla dokončit práce ve vláknu uživatelského rozhraní v časový interval mezi dvěma snímky, přibližně 16 milisekund (1 sekundu každých 60 rámce). 

Jak tuto situaci řešit, může vývojář v aktivitě pomocí vláken, provádět některé práci, kterou by blokovat uživatelského rozhraní. To však může způsobit problémy. Je velmi možné, že bude Android destroy a znovu vytvořit více instancí aktivity. Android však se automaticky nezničí vláken, které může mít za následek nevracení paměti. Typickým příkladem tohoto objektu je při [zařízení otočen](~/android/app-fundamentals/handling-rotation.md) &ndash; Android se pokusí destroy instance aktivity a pak znovu vytvořte novou:

![Když otočí zařízení, zničením instance 1 a je vytvořena instance 2](images/image-01.png)

To je potenciální nevrácená paměť systému &ndash; vlákno vytvořené první instance aktivity bude stále spuštěna. Pokud vlákno odkazuje na první instance aktivity, bude to Android zabránit paměti shromažďování objektu. Ale druhou instanci aktivity stále vytváření (který pak může vytvořit nové vlákno). Otáčení zařízení několikrát za sebou rychlé mohou vyčerpat všechny paměti RAM a vynutit Android ukončen, bude celá aplikace získat paměť.

Jako existuje pravidlo Pokud by měl pracovní provést outlive aktivity, pak služby by se vytvořit pro tuto práci. Ale pokud práce platí jenom v kontextu aktivity, pak vytvořit vlákno k provedení práce může být vhodnější. Například vytváření miniaturu fotografie, která byla právě přidána do aplikace Galerie fotografií pravděpodobně provedeno ve službě. Vlákno však může být vhodnější pro přehrání některé Hudba, který by měl buďte vyslyšeni pouze při aktivitu v popředí.

Práce pozadí můžete rozdělit na dvě obecné klasifikace:

1. **Dlouhé spouštění úlohy** &ndash; jde práci, kterou právě probíhá, dokud explicitně byla zastavena. Příklad _dlouho běžící úlohy_ je aplikace, která datové proudy Hudba nebo který musíte sledovat data shromážděná z senzoru. Tyto úlohy musí spustit i v případě, že aplikace nemá žádné viditelné uživatelské rozhraní.
2. **Pravidelné úlohy** &ndash; (někdy označovány jako _úlohy_) pravidelné úlohy je ten, který je poměrně krátké doby trvání (několik sekund) a spouští se podle plánu (tj. jednou za den, týden nebo možná právě jednou v Další 60 sekund). Příkladem je stažení souboru z Internetu nebo generování miniaturu obrázku.

Existují čtyři různé typy Android služeb:

* **Vázaný služby** &ndash; A _vázaný služby_ je služba, která má některé další součást (obvykle aktivitu) na něj navázaná. Vázané služba poskytuje rozhraní, které umožňuje vázané součásti a služba dojít ke vzájemné interakci. Jakmile neexistují žádné další klienty svázaná se službou, Android vypne službu.

* **`IntentService`** &ndash;  _`IntentService`_  Je podtřídou specializované `Service` třídu, která zjednodušuje vytváření služby a využití. `IntentService` Je určená ke zpracování jednotlivých autonomního volání. Na rozdíl od služby, která dokáže zpracovat souběžně více volání, `IntentService` se víc podobá _pracovní fronty procesoru_ &ndash; pracovní zařazen do fronty a `IntentService` zpracuje každou úlohu, jeden současně na jednom pracovní vlákno. Obvykle`IntentService` není vázán na aktivitu nebo Fragment. 

* **Spuštění služby** &ndash; A _spustila_ je služba, která byla spuštěna podle jiných Android součásti (například aktivitu) a běží průběžné na pozadí, dokud něco explicitně informuje zastavení služby. Na rozdíl od vázané služby spuštěná služba nemá žádné klienty přímo vázána. Z tohoto důvodu je důležité při návrhu spuštěné služby tak, aby mohl být řádně restartován podle potřeby.

* **Hybridní služba** &ndash; A _hybridní služby_ je služba, která má vlastnosti _spustila_ a _vázaný služby_. Hybridní služba může být spuštěno při součást váže k němu nebo může být spuštění některých událostí. Komponenta klienta může nebo nemusí být svázaná se službou hybridní. Hybridní služba poběží dál dlouho, dokud je explicitně sdělili zastavit nebo nejsou žádné další klienty vazba.

Jaký typ služby, která používá je závislé na požadavky na aplikace. Jako existuje pravidlo `IntentService` nebo vázané služby postačí pro většinu úloh, které musíte provést aplikace platformy Android, takže předvoleb má být poskytnut do jednoho z těchto dvou typů služeb. `IntentService` Je vhodná pro "jednorázové" úlohy, jako je například stahování souboru, zatímco vázané služby bude vhodné, pokud je potřeba časté interakce s aktivity nebo Fragment. 

Zatímco většina služby spuštěné na pozadí, je speciální podkategorie říká _popředí služby_. Toto je služba, která je vyšší prioritu (ve srovnání s normální service) pro některé práci pro uživatele (například přehrávání hudby). 

Je také možné spustit službu vlastní proces na stejném zařízení, to se někdy označuje jako _vzdálené služby_ nebo jako _mimo proces služby_. To vyžadovat další úsilí, které chcete vytvořit, ale může být užitečná pro když aplikace potřebuje funkce sdílení s jinými aplikacemi a, v některých případech vylepšit uživatelské prostředí aplikace. 

Každý z těchto služeb má svou vlastní vlastnosti a jednání a proto se budeme podrobněji v příručkách pro své vlastní.
