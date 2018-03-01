---
title: Xamarin.Android Errors Matrix
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c3aab44fcb0522b762aaa101de0aeba08016b641
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android Errors Matrix

## <a name="errors-reference"></a>Odkaz na chyby

Tento dokument obsahuje některé informace o různých kódy chyb z Xamarin.

<table>
    <thead>
        <tr>
            <td>Kategorie</td>
            <td>Popis</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> Chyby</td></tr>
        <tr><td>XA1xxx</td><td>Kopírování souboru nebo symbolických odkazů (projekt související) chyby</td></tr>
        <tr><td>XA2xxx</td><td>Chybami linkeru</td></tr>
        <tr><td>XA3xxx</td><td>AOT chyby</td></tr>
        <tr><td>XA4xxx</td><td>Chyby generování kódu</td></tr>
        <tr><td>XA5xxx</td><td>RSZ a nástrojů chyb</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> Interní nástroje pro chyby</td></tr>
        <tr><td>XA7xxx</td><td>Vyhrazené</td></tr>
        <tr><td>XA8xxx</td><td>Vyhrazené</td></tr>
        <tr><td>XA9xxx</td><td>Chyb při správě licencí</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>Kódy chyb

### <a name="xa0xxx-errors"></a>XA0xxx chyby

<table>
    <thead>
        <tr><td>Kód chyby</td><td>Popis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>Neočekávaná chyba – vyplňte sestavu chyb na <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>'-devname bylo zadáno bez jakékoli akce specifické pro zařízení</td></tr>
        <tr><td>XA0002</td><td>Nebylo možné rozložit proměnnou prostředí: {0}.</td></tr>
        <tr><td>XA0003</td><td>'{0} .exe, název aplikace je v konfliktu s SDK nebo produktu, název sestavení (.dll).</td></tr>
        <tr><td>XA0004</td><td>Nové refcounting logiku vyžaduje sgen příliš povolení.</td></tr>
        <tr><td>XA0005</td><td>Výstupní adresář: {0} neexistuje.</td></tr>
        <tr><td>XA0006</td><td>V {0}' neexistuje žádný devel platformy, použijte – platforma = PLAT k určení sady SDK</td></tr>
        <tr><td>XA0007</td><td>Sestavení kořenové: {0} neexistuje.</td></tr>
        <tr><td>XA0008</td><td>Měli byste jim poskytnout jenom jeden kořenový sestavení</td></tr>
        <tr><td>XA0009</td><td>Při načítání sestavení došlo k chybě: {0}</td></tr>
        <tr><td>XA0010</td><td>Nebylo možné rozložit argumenty příkazového řádku: {0}</td></tr>
        <tr><td>XA0011</td><td>{0} byl sestaven s novější runtime ({1}), než MonoTouch podporuje</td></tr>
        <tr><td>XA0012</td><td>Neúplná data je k dispozici k dokončení `{0}`.</td></tr>
        <tr><td>XA0013</td><td>Profilace podporu vyžaduje sgen příliš povolení</td></tr>
        <tr><td>XA0014</td><td>iOS {0} nepodporuje vytváření aplikací cílení ARMv6</td></tr>
        <tr><td>XA0020</td><td>Nepodařilo se určit cestu mandroid.</td></tr>
        <tr><td>XA0100</td><td>EmbeddedNativeLibrary: {0} je neplatný v projektu aplikace pro Android. Místo toho použijte AndroidNativeLibrary.</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>XA1xxx chyby

<table>
    <thead>
        <tr><td>Kód chyby</td><td>Popis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>Nelze nalézt aplikaci na zadaný adresář</td></tr>
        <tr><td>XA1002</td><td>Nepodařilo se vytvořit symbolických odkazů, soubory byly zkopírovány.</td></tr>
        <tr><td>XA1003</td><td>Nelze ukončit aplikace: {0}. Možná budete muset ručně ukončit aplikaci.</td></tr>
        <tr><td>XA1004</td><td>Nelze získat seznam nainstalovaných aplikací.</td></tr>
        <tr><td>XA1005</td><td>Nelze ukončit aplikaci {0}' na zařízení objekt {1}.: {2}. Možná budete muset ručně ukončit aplikaci.</td></tr>
        <tr><td>XA1006</td><td>Aplikace: {0} nelze nainstalovat na zařízení objekt {1}.: {2}.</td></tr>
        <tr><td>XA1007</td><td>Spuštění aplikace: {0} na objekt {1}' zařízení se nezdařilo: {2}. Aplikace můžete pořád spustit ručně klepnutím na něm.</td></tr>
        <tr><td>XA1008</td><td>Nepodařilo se spustit simulátoru: {0}</td></tr>
        <tr><td>XA1009</td><td>Sestavení: {0} nelze zkopírovat do objekt {1}.: {2}</td></tr>
        <tr><td>XA1010</td><td>Nebylo možné načíst sestavení '{0}': {1}</td></tr>
        <tr><td>XA1011</td><td>Nelze přidat chybí soubor prostředků: {0}.</td></tr>
        <tr><td>XA1101</td><td>Nelze spustit aplikaci</td></tr>
        <tr><td>XA1102</td><td>Se nepodařilo připojit k aplikaci (pro příkaz kill ji): {0}</td></tr>
        <tr><td>XA1103</td><td>Se nepodařilo odpojit</td></tr>
        <tr><td>XA1104</td><td>Nepodařilo se odeslat paket: {0}</td></tr>
        <tr><td>XA1105</td><td>Neočekávaná odezva typu</td></tr>
        <tr><td>XA1106</td><td>Nelze získat seznam aplikací na zařízení: Vypršel časový limit požadavku.</td></tr>
        <tr><td>XA1107</td><td>Spuštění aplikace se nezdařilo.</td></tr>
        <tr><td>XA1201</td><td>Nepodařilo se načíst simulátoru: {0}</td></tr>
        <tr><td>XA1301</td><td>Nativní knihovny `{0}` ({1}) byl ignorován, protože neodpovídá aktuální architecture(s) sestavení ({2})</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>XA2xxx chyby

<table>
    <thead>
        <tr><td>Kód chyby</td><td>Popis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>Nelze propojit sestavení</td></tr>
        <tr><td>XA2002</td><td>Nelze přeložit odkaz: {0}</td></tr>
        <tr><td>XA2003</td><td>Možnost {0}' bude ignorován, protože propojení je zakázán.</td></tr>
        <tr><td>XA2004</td><td>Nebyl nalezen soubor definice navíc linkeru: {0}.</td></tr>
        <tr><td>XA2005</td><td>Definice z {0}' nebylo možné analyzovat.</td></tr>
        <tr><td>XA2006</td><td>Odkaz na položka metadat {0}. (definovanou v objekt {1}.) z '{2}' nebylo možné přeložit.</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>XA3xxx chyby

Toto jsou AOT chyby.

<table>
    <thead>
        <tr><td>Kód chyby</td><td>Popis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>Může AOT sestavení {0}.</td></tr>
        <tr><td>XA3002</td><td>Omezení AOT: {0}' musí být nastavena metoda statické vzhledem k tomu, že je upravena pomocí [MonoPInvokeCallback].</td></tr>
        <tr><td>XA3003</td><td>Konfliktní – ladění a--llvm možnosti. Ladění soft je zakázána.</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>XA4xxx chyby

Toto jsou chyby generování kódu.

<table>
    <thead>
        <tr><td>Kód chyby</td><td>Popis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>Hlavní šablonu nebylo možné expansed k `{0}`.</td></tr>
        <tr><td>XA4101</td><td>Registrátora nemůže vytvořit podpis pro typ `{0}`.</td></tr>
        <tr><td>XA4102</td><td>Registrátora byl nalezen neplatný typ `{0}` v podpis pro metodu `{2}`. Místo nich se používá `{1}`.</td></tr>
        <tr><td>XA4103</td><td>Registrátora byl nalezen neplatný typ `{0}` v podpis pro metodu `{2}`: typ implementuje INativeObject, ale nemá konstruktor, který přebírá dva (IntPtr, bool) argumenty</td></tr>
        <tr><td>XA4104</td><td>Registrátora nelze zařazování návratovou hodnotu pro typ `{0}` v podpis pro metodu `{1}`.</td></tr>
        <tr><td>XA4105</td><td>Registrátora nelze zařazování s parametrem typu `{0}` v podpis pro metodu `{1}`.</td></tr>
        <tr><td>XA4106</td><td>Registrátora nelze zařazování návratovou hodnotu pro strukturu `{0}` v podpis pro metodu `{1}`.</td></tr>
        <tr><td>XA4107</td><td>Registrátora nelze zařazování s parametrem typu `{0}` v podpis pro metodu `{1}`.</td></tr>
        <tr><td>XA4108</td><td>Registrátora nelze získat typ ObjectiveC pro spravovaný typ `{0}`.</td></tr>
        <tr><td>XA4109</td><td>Kompilace kódu generovaného registrátora se nezdařila. Prosím soubor sestavy chyb v <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>Registrátora nelze zařazování výstupní parametr typu `{0}` v podpis pro metodu `{1}`.</td></tr>
        <tr><td>XA4111</td><td>Registrátora nemůže vytvořit podpis pro typ `{0}' in method `{1}'.</td></tr>
        <tr><td>XA4200</td><td>Na ACW můžete generovat jenom pro typy 'claas'.</td></tr>
        <tr><td>XA4201</td><td>Nelze určit název JNI typu {0}.</td></tr>
        <tr><td>XA4203</td><td>Musí být plně kvalifikovaný název zadaného typu.</td></tr>
        <tr><td>XA4204</td><td>Nelze vyřešit typ rozhraní: {0}. Nechybí odkaz na sestavení?</td></tr>
        <tr><td>XA4205</td><td>[ExportField] lze použít pouze v metody s 0 parametry.</td></tr>
        <tr><td>XA4206</td><td>[Export] nelze použít na obecného typu.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] nelze použít na obecného typu.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] nelze použít na metodu vrácení void.</td></tr>
        <tr><td>XA4209</td><td>Nepodařilo se vytvořit JavaTypeInfo pro třídu: {0} z důvodu {1}</td></tr>
        <tr><td>XA4210</td><td>Budete muset přidat odkaz na Mono.Android.Export.dll při použití ExportAttribute nebo ExportFieldAttribute.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion {0}' je menší než $(TargetFrameworkVersion) objekt {1}. Pomocí rozhraní API-\ {1\} ACW kompilace.</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>XA5xxx chyby

<table>
    <thead>
        <tr><td>Kód chyby</td><td>Popis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>Chybějící {0}' kompilátoru. Nainstalujte Android NDK.</td></tr>
        <tr><td>XA5102</td><td>Převod z sestavení do nativního kódu se nezdařil. Prosím soubor sestavy chyb v <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>Kompilace souboru: {0} se nezdařila. Prosím soubor sestavy chyb v <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>Nativní propojení se nezdařilo. Přečtěte si uživatel příznaky poskytované RSZ: {0}</td></tr>
        <tr><td>XA5202</td><td>Nativní propojení se nezdařilo. Zkontrolujte protokol sestavení.</td></tr>
        <tr><td>XA5303</td><td>Nativní propojení upozornění: {0}</td></tr>
        <tr><td>XA5300</td><td>Sady SDK pro Android nebylo nalezeno nebo není zcela nainstalovat.</td></tr>
        <tr><td>XA5301</td><td>Chybí nástroj 'pruhu'. Nainstalujte Xcode součásti, nástroje příkazového řádku.</td></tr>
        <tr><td>XA5302</td><td>Chybí nástroj 'dsymutil'. Nainstalujte Xcode součásti, nástroje příkazového řádku.</td></tr>
        <tr><td>XA5203</td><td>Nepodařilo se vygenerovat symboly ladění (dSYM adresář). Zkontrolujte protokol sestavení.</td></tr>
        <tr><td>XA5204</td><td>Nepodařilo se odstranit poslední binárního souboru. Zkontrolujte protokol sestavení.</td></tr>
        <tr><td>XA5205</td><td>Chybí nástroj 'aapt'. Nainstalujte balíček Android SDK – nástroje pro vytváření.</td></tr>
        <tr><td>XA5206</td><td>{0}. Android prostředků {1} adresář neexistuje.</td></tr>
        <tr><td>XA5207</td><td>{0}. Java knihovny souboru {1} neexistuje.</td></tr>
        <tr><td>XA5208</td><td>Stahování se nezdařilo. Stáhnout {0} a umístí jej do adresáře {1}.</td></tr>
        <tr><td>XA5209</td><td>Rozbalení se nezdařilo. {0} stáhněte a rozbalte ho do adresáře {1}.</td></tr>
        <tr><td>XA5210</td><td>{0}. Nativní knihovny {1} soubor neexistuje.</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>XA6xxx chyby

<table>
    <thead>
        <tr><td>Kód chyby</td><td>Popis</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>Spuštěná verze Cecil nepodporuje odstraňování sestavení</td></tr>
        <tr><td>XA6002</td><td>Nelze odstranit sestavení `{0}`.</td></tr>
        <tr><td>XA6003</td><td>UnauthorizedAccessException zpráv]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>XA9xxx chyby

Tato kódy chyb jsou chyby licencování a aktivace.

 <a name="xa9000" />


#### <a name="xa9000"></a>XA9000

 **Příčina:** platnost licence

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
        </tr>
    </tbody>
</table>

 <a name="XA9001" />


#### <a name="xa9001"></a>XA9001

 **Příčina:** platnost zkušební verze

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
            <td>UPOZORNĚNÍ</td>
        </tr>
    </tbody>
</table>

 <a name="XA9002" />


#### <a name="xa9002"></a>XA9002

 **Příčina:** AndroidJavaSource

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Příčina:** AndroidJavaLibrary

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Má-li vazba sestavení .jar vložených, pak tato zasekne během balíčku, není čase vytvoření buildu.**

 **Příčina:** AndroidNativeLibrary

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9003" />


#### <a name="xa9003"></a>XA9003

 **Cause:** System.Runtime.Serialization

 **Zaškrtnutí během:** balíčku

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** System.ServiceModel.Web

 **Zaškrtnutí během:** balíčku

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Příčina:** Mono.Data.Tds

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **To je odkazován objektem System.Data.dll, které je povoleno**

 **Příčina:** Mono.Android.Export

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9004" />


#### <a name="xa9004"></a>XA9004

 **Příčina:** – profilace

 **Zaškrtnutí během:** balíčku

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9005" />


#### <a name="xa9005"></a>XA9005

 **Příčina:** omezení velikosti (32 kb)

 **Zaškrtnutí během:** balíčku

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>OK</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>

 <a name="XA9006" />


#### <a name="xa9006"></a>XA9006

 **Příčina:** System.Data.SqlClient obor názvů

 **Zaškrtnutí během:** balíčku

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9008" />


#### <a name="xa9008"></a>XA9008

 **Příčina:** sestavení z příkazového řádku

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9009" />


#### <a name="xa9009"></a>XA9009

 **Příčina:** chybí sériové číslo

 **Zaškrtnutí během:** Untestable

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
        </tr>
    </tbody>
</table>

 <a name="XA9010" />


#### <a name="xa9010"></a>XA9010

 **Příčina:** neplatný ProductId

 **Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
        </tr>
    </tbody>
</table>

Ekvivalent XA9018

 <a name="XA9011" />


#### <a name="xa9011"></a>XA9011

 **Příčina:** se nepodařilo aktualizovat licenční soubor (do nového formátu souboru)

 **Zaškrtnutí během:** aktivace

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
        </tr>
    </tbody>
</table>

 <a name="XA9012" />


#### <a name="xa9012"></a>XA9012

 **Příčina:** žádné Internetu

 **Zaškrtnutí během:** aktivace

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
        </tr>
    </tbody>
</table>

 <a name="XA9013" />


#### <a name="xa9013"></a>XA9013

 **Příčina:** došlo k neznámé chybě

 **Zaškrtnutí během:** aktivace

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
        </tr>
    </tbody>
</table>

 <a name="XA9014" />


#### <a name="xa9014"></a>XA9014

 **Příčina:** Neplatný aktivační kód

 **Zaškrtnutí během:** aktivace

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
        </tr>
    </tbody>
</table>

 <a name="XA9017" />


#### <a name="xa9017"></a>XA9017

 **Příčina:** aktivační server nevrací platnou licenci

 **Zaškrtnutí během:** aktivace

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
            <td>CHYBA</td>
        </tr>
    </tbody>
</table>

<a name="XA9018" />

#### <a name="xa9018"></a>XA9018

**Příčina:** neplatná licence

**Zaškrtnutí během:** sestavení

<table>
    <thead>
        <tr>
            <th>Spuštění</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Firmy</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>CHYBA</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
