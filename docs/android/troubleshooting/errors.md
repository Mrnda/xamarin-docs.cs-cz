---
title: Xamarin.Android Errors Matrix
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: e9916d8e264c202a914e6fd70e664beea276e93d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774145"
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Odkaz na chyby

Tento dokument obsahuje některé informace o různých kódy chyb z Xamarin.

|Kategorie|Popis|
|--- |--- |
|XA0xxx|mandroid chyby|
|XA1xxx|Kopírování souboru nebo symbolických odkazů (projekt související) chyby|
|XA2xxx|Chybami linkeru|
|XA3xxx|AOT chyby|
|XA4xxx|Chyby generování kódu|
|XA5xxx|RSZ a nástrojů chyb|
|XA6xxx|Interní nástroje pro chyby mandroid|
|XA7xxx|Vyhrazené|
|XA8xxx|Vyhrazené|
|XA9xxx|Chyb při správě licencí|


## <a name="error-codes"></a>Kódy chyb

### <a name="xa0xxx-errors"></a>XA0xxx chyby

|Kód chyby|Popis|
|--- |--- |
|XA0000|Neočekávaná chyba – vyplňte [Sestava chyb](http://bugzilla.xamarin.com).|
|XA0001|'-devname bylo zadáno bez jakékoli akce konkrétní zařízení.|
|XA0002|Nebylo možné rozložit proměnnou prostředí: {0}.|
|XA0003|'{0} .exe, název aplikace je v konfliktu s SDK nebo produktu, název sestavení (.dll).|
|XA0004|Nové refcounting logiku vyžaduje sgen příliš povolení.|
|XA0005|Výstupní adresář: {0} neexistuje.|
|XA0006|V {0}' neexistuje žádný devel platformy, použijte – platforma = PLAT k určení sady SDK|
|XA0007|Sestavení kořenové: {0} neexistuje.|
|XA0008|Měl by poskytnout jenom jeden kořenový sestavení.|
|XA0009|Při načítání sestavení došlo k chybě: {0}.|
|XA0010|Nebylo možné rozložit argumenty příkazového řádku: {0}.|
|XA0011|{0} byl sestaven s novější runtime ({1}), než MonoTouch podporuje.|
|XA0012|K dokončení {0}' je k dispozici neúplná data.|
|XA0013|SGen – povolení příliš profilace podporu vyžaduje.|
|XA0014|iOS {0} nepodporuje vytváření aplikací cílení ARMv6.|
|XA0020|Nepodařilo se určit cestu mandroid.|
|XA0100|EmbeddedNativeLibrary: {0} je neplatný v projektu aplikace pro Android. Místo toho použijte AndroidNativeLibrary.|

### <a name="xa1xxx-errors"></a>XA1xxx chyby

|Kód chyby|Popis|
|--- |--- |
|XA1001|Nelze najít aplikace v určeném adresáři.|
|XA1002|Nepodařilo se vytvořit symbolických odkazů, soubory byly zkopírovány.|
|XA1003|Nelze ukončit aplikace: {0}. Možná budete muset ručně ukončit aplikaci.|
|XA1004|Nelze získat seznam nainstalovaných aplikací.|
|XA1005|Nelze ukončit aplikaci {0}' na zařízení objekt {1}.: {2}. Možná budete muset ručně ukončit aplikaci.|
|XA1006|Aplikace: {0} nelze nainstalovat na zařízení objekt {1}.: {2}.|
|XA1007|Spuštění aplikace: {0} na objekt {1}' zařízení se nezdařilo: {2}. Aplikace můžete pořád spustit ručně klepnutím na něm.|
|XA1008|Nepodařilo se spustit simulátoru: {0}.|
|XA1009|Sestavení: {0} nelze zkopírovat do objekt {1}.: {2}.|
|XA1010|Nebylo možné načíst sestavení '{0}': {1}.|
|XA1011|Nelze přidat chybí soubor prostředků: {0}.|
|XA1101|Nelze spustit aplikaci.|
|XA1102|Se nepodařilo připojit k aplikaci (pro příkaz kill ji): {0}.|
|XA1103|Nelze odpojit.|
|XA1104|Nepodařilo se odeslat paket: {0}.|
|XA1105|Typ neočekávanou odpověď.|
|XA1106|Nelze získat seznam aplikací na zařízení: Vypršel časový limit požadavku.|
|XA1107|Aplikaci se nepodařilo spustit.|
|XA1201|Nepodařilo se načíst simulátoru: {0}.|
|XA1301|Nativní knihovny {0}. ({1}) byl ignorován, protože neodpovídá aktuální architecture(s) sestavení ({2}).|

### <a name="xa2xxx-errors"></a>XA2xxx chyby

|Kód chyby|Popis|
|--- |--- |
|XA2001|Sestavení nelze propojit.|
|XA2002|Nelze přeložit odkaz: {0}.|
|XA2003|Možnost {0}' bude ignorován, protože propojení je zakázán.|
|XA2004|Nebyl nalezen soubor definice navíc linkeru: {0}.|
|XA2005|Definice z {0}' nebylo možné analyzovat.|
|XA2006|Odkaz na položka metadat {0}. (definovanou v objekt {1}.) z '{2}' nebylo možné přeložit.|

### <a name="xa3xxx-errors"></a>XA3xxx chyby

Toto jsou AOT chyby.

|Kód chyby|Popis|
|--- |--- |
|XA3001|Může AOT sestavení: {0}.|
|XA3002|Omezení AOT: {0}' musí být nastavena metoda statické vzhledem k tomu, že je upravena pomocí [MonoPInvokeCallback].|
|XA3003|Konfliktní – ladění a--llvm možnosti. Ladění soft je zakázána.|


### <a name="xa4xxx-errors"></a>XA4xxx chyby

Toto jsou chyby generování kódu.

|Kód chyby|Popis|
|--- |--- |
|XA4001|Hlavní šablonu nebylo možné expansed k: {0}.|
|XA4101|Registrátora nemůže vytvořit podpis pro typ: {0}.|
|XA4102|Registrátora nalezen neplatný typ {0}' v podpisu pro metodu '{2}'. Objekt {1}' použijte místo toho.|
|XA4103|Registrátora nalezen neplatný typ {0}' v podpis pro metodu '{2}': typ implementuje INativeObject, ale nemá konstruktor, který přebírá dva (IntPtr, bool) argumenty.|
|XA4104|Registrátora nelze zařazování návratovou hodnotu pro typ {0}' v podpis pro metodu objekt {1}.|
|XA4105|Parametr typu {0}' v podpis pro metodu objekt {1}' nelze zařazování registrátora.|
|XA4106|Registrátora nelze zařazování návratovou hodnotu pro strukturu {0}' v podpis pro metodu objekt {1}.|
|XA4107|Parametr typu {0}' v podpis pro metodu objekt {1}' nelze zařazování registrátora.|
|XA4108|Registrátora nelze získat typ ObjectiveC pro spravovaný typ: {0}.|
|XA4109|Kompilace kódu generovaného registrátora se nezdařila. Prosím soubor [Sestava chyb](http://bugzilla.xamarin.com).|
|XA4110|Registrátora nelze v podpis pro metodu objekt {1}' zařazování výstupní parametr typu: {0}.|
|XA4111|Registrátora nemůže vytvořit podpis pro typ {0}' v metodě objekt {1}.|
|XA4200|Na ACW můžete generovat jenom pro typy 'claas'.|
|XA4201|Nelze určit název JNI typu {0}.|
|XA4203|Musí být plně kvalifikovaný název zadaného typu.|
|XA4204|Nelze vyřešit typ rozhraní: {0}. Nechybí odkaz na sestavení?|
|XA4205|[ExportField] lze použít pouze v metody s 0 parametry.|
|XA4206|[Export] nelze použít na obecného typu.|
|XA4207|[ExportField] nelze použít na obecného typu.|
|XA4208|[Java.Interop.ExportFieldAttribute] nelze použít na metodu vrácení void.|
|XA4209|Nepodařilo se vytvořit JavaTypeInfo pro třídu: {0} z důvodu {1}.|
|XA4210|Budete muset přidat odkaz na Mono.Android.Export.dll při použití ExportAttribute nebo ExportFieldAttribute.|
|XA4211|AndroidManifest.xml //uses-sdk/@android:targetSdkVersion {0}' je menší než $(TargetFrameworkVersion) objekt {1}. Pomocí rozhraní API-\ {1\} ACW kompilace.|


### <a name="xa5xxx-errors"></a>XA5xxx chyby

|Kód chyby|Popis|
|--- |--- |
|XA5101|Chybějící {0}' kompilátoru. Nainstalujte Android NDK.|
|XA5102|Převod z sestavení do nativního kódu se nezdařil. Prosím soubor [Sestava chyb](http://bugzilla.xamarin.com).|
|XA5103|Kompilace souboru: {0} se nezdařila. Prosím soubor [Sestava chyb](http://bugzilla.xamarin.com).|
|XA5201|Nativní propojení se nezdařilo. Přečtěte si uživatel příznaky poskytované RSZ: {0}|
|XA5202|Nativní propojení se nezdařilo. Zkontrolujte protokol sestavení.|
|XA5303|Nativní propojení upozornění: {0}.|
|XA5300|Sady SDK pro Android nebylo nalezeno nebo není zcela nainstalovat.|
|XA5301|Chybí nástroj 'pruhu'. Nainstalujte Xcode součásti, nástroje příkazového řádku'.|
|XA5302|Chybí nástroj 'dsymutil'. Nainstalujte Xcode součásti, nástroje příkazového řádku'.|
|XA5203|Nepodařilo se vygenerovat symboly ladění (dSYM adresář). Zkontrolujte protokol sestavení.|
|XA5204|Nepodařilo se odstranit poslední binárního souboru. Zkontrolujte protokol sestavení.|
|XA5205|Chybí nástroj 'aapt'. Nainstalujte balíček Android SDK – nástroje pro vytváření.|
|XA5206|{0}. Android prostředků {1} adresář neexistuje.|
|XA5207|{0}. Java knihovny souboru {1} neexistuje.|
|XA5208|Stahování se nezdařilo. Stáhnout {0} a umístí jej do adresáře {1}.|
|XA5209|Rozbalení se nezdařilo. {0} stáhněte a rozbalte ho do adresáře {1}.|
|XA5210|{0}. Nativní knihovny {1} soubor neexistuje.|

### <a name="xa6xxx-errors"></a>XA6xxx chyby

|Kód chyby|Popis|
|--- |--- |
|XA6001|Spuštěná verze Cecil nepodporuje odstraňování sestavení.|
|XA6002|Nelze odstranit sestavení: {0}.|
|XA6003|UnauthorizedAccessException zpráva.|

### <a name="xa9xxx-errors"></a>XA9xxx chyby

Tyto kódy chyb jsou chyby licencování a aktivace.


#### <a name="xa9000"></a>XA9000

 **Příčina:** platnost licence

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|UPOZORNĚNÍ|UPOZORNĚNÍ|UPOZORNĚNÍ|UPOZORNĚNÍ|UPOZORNĚNÍ|


#### <a name="xa9001"></a>XA9001

 **Příčina:** platnost zkušební verze

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|UPOZORNĚNÍ|UPOZORNĚNÍ|UPOZORNĚNÍ|UPOZORNĚNÍ|UPOZORNĚNÍ|



#### <a name="xa9002"></a>XA9002

 **Příčina:** AndroidJavaSource

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|OK|OK|OK|OK|

 **Příčina:** AndroidJavaLibrary

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|OK|OK|OK|OK|

 **Má-li vazba sestavení .jar vložených, pak tato zasekne během balíčku, není čase vytvoření buildu.**

 **Příčina:** AndroidNativeLibrary

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|OK|OK|OK|OK|


#### <a name="xa9003"></a>XA9003

 **Cause:** System.Runtime.Serialization

 **Zaškrtnutí během:** balíčku

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|OK|OK|OK|

 **Cause:** System.ServiceModel.Web

 **Zaškrtnutí během:** balíčku

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|OK|OK|OK|

 **Příčina:** Mono.Data.Tds

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|OK|OK|OK|

 **To je odkazován objektem System.Data.dll, které je povoleno**

 **Příčina:** Mono.Android.Export

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|OK|OK|OK|OK|



#### <a name="xa9004"></a>XA9004

 **Příčina:** – profilace

 **Zaškrtnutí během:** balíčku

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|OK|OK|OK|



#### <a name="xa9005"></a>XA9005

 **Příčina:** omezení velikosti (32 kb).

 **Zaškrtnutí během:** balíčku

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|OK|-|-|-|



#### <a name="xa9006"></a>XA9006

 **Cause:** System.Data.SqlClient namespace.

 **Zaškrtnutí během:** balíčku

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|OK|OK|OK|


#### <a name="xa9008"></a>XA9008

 **Příčina:** sestavení z příkazového řádku.

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|OK|OK|OK|


#### <a name="xa9009"></a>XA9009

 **Příčina:** chybí sériové číslo.

 **Zaškrtnutí během:** Untestable

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|CHYBA|CHYBA|CHYBA|


#### <a name="xa9010"></a>XA9010

 **Příčina:** neplatný ProductId.

 **Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|CHYBA|CHYBA|CHYBA|

Ekvivalent XA9018.



#### <a name="xa9011"></a>XA9011

 **Příčina:** se nepodařilo aktualizovat licenční soubor (do nového formátu souboru).

 **Zaškrtnutí během:** aktivace

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|CHYBA|CHYBA|CHYBA|

#### <a name="xa9012"></a>XA9012

 **Příčina:** žádné Internetu

 **Zaškrtnutí během:** aktivace

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|CHYBA|CHYBA|CHYBA|


#### <a name="xa9013"></a>XA9013

 **Příčina:** došlo k neznámé chybě

 **Zaškrtnutí během:** aktivace

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|CHYBA|CHYBA|CHYBA|


#### <a name="xa9014"></a>XA9014

 **Příčina:** Neplatný aktivační kód

 **Zaškrtnutí během:** aktivace

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|CHYBA|CHYBA|CHYBA|


#### <a name="xa9017"></a>XA9017

 **Příčina:** aktivační server nevrací platnou licenci.

 **Zaškrtnutí během:** aktivace

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|CHYBA|CHYBA|CHYBA|CHYBA|CHYBA|


#### <a name="xa9018"></a>XA9018

**Příčina:** neplatná licence

**Zaškrtnutí během:** sestavení

|Spuštění|Indie|Business(Trial)|Firmy|Enterprise|
|--- |--- |--- |--- |--- |
|-|-|CHYBA|-|-|

