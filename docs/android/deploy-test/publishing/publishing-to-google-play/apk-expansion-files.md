---
title: Soubory rozšíření APK
ms.prod: xamarin
ms.assetid: DB7E38E8-3C4E-5191-27EA-22DE63044FE2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f2ca86fb27b5bd4b6cb7ba855fbd0a2abfa1381d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="apk-expansion-files"></a>Soubory rozšíření APK

Některé aplikace (některé hry, například) vyžadují další prostředky a prostředky, než lze zadat do aplikace pro Android maximální limit velikosti způsobené Google Play. Tento limit, závisí na verzi systému Android, které je cílem vašeho APK:

-  100MB pro APKs, které cílí Android 4.0 nebo vyšší (API úrovně 14 nebo vyšší).
-  50MB pro APKs, které cílí na Android 3.2 nebo nižší (API úrovně 13 nebo vyšší).

K překonání tohoto omezení, bude hostitelem a distribuovat dvěma Google Play *soubory rozšíření* přejdete spolu APK umožní aplikaci nepřímo překročí toto omezení. 

Na většinu zařízení při instalaci aplikace soubory rozšíření se stáhne společně s APK a bude uložena do umístění sdíleného úložiště (karta SD nebo USB připojit oddíl) na zařízení. Na několik starší zařízení nemusí soubory rozšíření nainstalujte pomocí APK automaticky. V těchto situacích je nutné pro aplikace obsahovat kód, který se stáhnout soubory rozšíření, když uživatel poprvé spustí aplikace.

Rozšíření soubory jsou považovány za *neprůhledného binární objekty BLOB (obb)* a může být až do velikosti 2 GB. Android neprovádí žádné speciální zpracování na tyto soubory, po stažení &ndash; souborů může být v libovolném formátu, který je vhodný pro aplikaci. Doporučeným přístupem k rozšíření soubory koncepčně, vypadá takto:

-   **Hlavní rozšíření** &ndash; tento soubor je soubor primární rozšíření prostředky a prostředky, které nebudou vyhovovat APK limit velikosti. Soubor hlavní rozšíření by měl obsahovat primární prostředky, které aplikace potřebuje a zřídka by měly být aktualizovány.
-   **Oprava rozšíření** &ndash; slouží pro malých aktualizací souborů hlavní rozšíření. Tento soubor můžete aktualizovat. Je zodpovědností aplikace provést všechny potřebné opravy nebo aktualizace z tohoto souboru.


Rozšíření soubory musí být odeslán ve stejnou dobu jako APK je načtený.
Google play neumožňuje soubor rozšíření k odeslání do existující APK nebo pro existující APKs aktualizovat. Pokud je nutné aktualizovat soubor rozšíření, pak se musí se nahrát nový APK `versionCode` aktualizovat.


## <a name="expansion-file-storage"></a>Rozšíření úložiště

Pokud se stahují soubory do zařízení, se uloží v  **_úložiště sdíleného_v/Android/obb/_název balíčku_**:

-   **_úložiště sdíleného_**  &ndash; Toto je adresář zadaný `Android.OS.Environment.ExternalStorageDirectory` .
-   **_Název balíčku_**  &ndash; Toto je název balíčku aplikace Java stylu.


Po stažení, soubory rozšíření by neměl být přesunout, změnit, přejmenovat nebo odstranili z jejich umístění na zařízení. Uděláte to tak způsobí, že soubory rozšíření znovu stáhnout a původní soubory se odstraní. Kromě toho adresář souboru rozšíření by měl obsahovat pouze soubory pack rozšíření.

Soubory rozšíření nabízí žádné zabezpečení nebo ochranu kolem obsah &ndash; další aplikace nebo uživatelé mohou přistupovat k souborům uložené ve sdíleném úložišti.

Pokud je nutné rozbalte soubor rozšíření, by měly být rozbalené soubory uložené v samostatném adresáři, například ten, který v `Android.OS.Environment.ExternalStorageDirectory`.

Alternativu k extrahování souborů ze souboru rozšíření je čtení prostředků nebo prostředky přímo ze souboru rozšíření. Soubor rozšíření není nic jiného než soubor zip, který lze použít s odpovídajícími `ContentProvider`. [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) obsahuje sestavení, [System.IO.Compression.Zip](https://github.com/mattleibow/Android.Play.ExpansionLibrary/tree/master/System.IO.Compression.Zip), což zahrnuje `ContentProvider` který umožňuje přístup k některé soubory médií přímé souboru. Pokud jsou se soubory médií zabalené do souboru zip, může volání přehrávání média přímo použít bez nutnosti rozbalte soubor zip soubory v souboru zip. Přidá do souboru zip by neměl komprimovány mediálních souborů. 


### <a name="filename-format"></a>Název souboru formátu

Pokud jsou staženy soubory rozšíření, Google Play použijte následující schéma k název rozšíření:

    [main|patch].<expansion-version>.<package-name>.obb

Jsou tři součásti toto schéma:

-   `main` nebo `patch` &ndash; to určuje, jestli je hlavní nebo opravný soubor rozšíření. Může být jen jedna pro každou z.
-   `<expansion-version>` &ndash; Toto je celé číslo, které odpovídá `versionCode` z APK soubor byl první přidružené.
-   `<package-name>` &ndash; Toto je název balíčku aplikace Java stylu.


Například pokud je verze APK 21 a názvu balíčku se `mono.samples.helloworld`, bude mít název souboru hlavní rozšíření **main.21.mono.samples.helloworld**.


## <a name="download-process"></a>Proces stahování

Při instalaci aplikace z obchodu Google Play soubory rozšíření by měl stáhli a uložili společně s APK. V některých situacích tomu nemusí dojít nebo rozšíření soubory budou odstraněny. Zpracování tohoto stavu, je potřeba zkontrolovat, zda rozšíření soubory existují a pak stáhnout, v případě potřeby aplikace. Následující vývojový diagram zobrazuje doporučenou pracovního postupu tohoto procesu:

[![Vývojový diagram rozšíření APK](apk-expansion-files-images/apkexpansion.png)](apk-expansion-files-images/apkexpansion.png#lightbox)

Při spuštění aplikace, měli zkontrolovat Pokud chcete zobrazit, pokud příslušná rozšíření soubory existují v aktuální zařízení. Pokud ne, pak aplikace musíte provést žádost na webu Google Play [licencování aplikace](http://developer.android.com/google/play/licensing/index.html). Tato kontrola se provádí pomocí *licence ověření knihovny (úroveň)*a musí být provedeny pro volné a licencované aplikace. ÚROVEŇ se primárně používají placené aplikace vynutit licenční omezení. Google však má rozšířené úroveň, tak, aby se dá použít s také rozšíření knihovny. Volné aplikací mít k provedení kontroly úroveň, ale můžete ignorovat licenční omezení. Žádost o úroveň zodpovídá za poskytování následující informace o rozšíření soubory, které aplikace vyžaduje: 

-   **Velikost souboru** &ndash; velikosti souboru rozšíření soubory se používají v rámci kontroly, které určuje, zda již byly staženy soubory správné rozšíření.
-   **Názvy souborů** &ndash; Toto je název souboru (na aktuální zařízení) které musí být uloženy sady rozšíření.
-   **Adresa URL pro stahování** &ndash; adresa URL, který se má použít ke stažení sady rozšíření. Toto je jedinečný pro každé stahování a krátce po je poskytována vypršení platnosti.


Poté, co byla provedena kontrola úroveň, aplikace by měla stáhnout soubory rozšíření, vezměte v úvahu následující body v rámci stahování:

-  Zařízení nemusí mít dostatek místa k uložení souborů rozšíření.
-  Pokud Wi-Fi není k dispozici, uživatel by měl mít povolený pozastavení nebo zrušit ke stažení, aby se zabránilo nechtěných dat poplatky.
-  Soubory rozšíření se stáhnou na pozadí k zabránění blokování interakce uživatele.
-  Při stahování probíhá na pozadí, má být zobrazena indikátor průběhu.
-  Chyb vzniklých při stahování jsou řádně zpracovávaný a obnovitelný.



## <a name="architectural-overview"></a>Přehled architektury

Když v hlavní aktivitě spustí, zkontroluje zobrazíte, když se stáhnou soubory rozšíření. Pokud jsou staženy soubory, se musí být kontrolované platnosti.

Pokud nebyly staženy soubory rozšíření nebo pokud jsou soubory jsou neplatné, je nutné stáhnout nové soubory rozšíření. Ohraničené služby je vytvořen jako součást aplikace. Při spuštění v hlavní aktivitě aplikace používá službu ohraničené provést kontrolu proti služby Google licencování a zjistěte, názvy souborů rozšíření a adresu URL soubory ke stažení. Službu ohraničené si následně stáhne soubory do vlákna na pozadí.

K usnadnění náročnost integrovat soubory rozšíření do aplikace, Google vytvořit několik knihovny v jazyce Java. Knihovny dotyčném jsou:

-   **Stažení programu knihovna** &ndash; jde knihovnu, která snižuje náročnost integrovat rozšíření soubory v aplikaci. Knihovny se stáhnout soubory rozšíření ve službě pozadí, zobrazovat uživatelská oznámení, zpracování problémy se síťovým připojením, obnovení souborů ke stažení, atd.
-   **Licence ověření knihovny (úroveň)** &ndash; knihovnu pro vytváření a zpracování volání do služby licencování aplikace. Ho lze také provést licencování zkontroluje, jestli aplikace je autorizovaný pro použití v zařízení.
-   **Knihovna Zip APK rozšíření (volitelné)** &ndash; Pokud jsou soubory rozšíření v souboru zip, tuto knihovnu bude fungovat jako poskytovateli obsahu a umožňuje aplikaci číst prostředky a prostředky přímo ze souboru zip bez nutnosti rozšířit souboru zip soubor.


Tyto knihovny přenesené C# a jsou dostupné v rámci Apache 2.0 licence. Rychle integrovat rozšíření soubory do existující aplikace, můžete tyto knihovny přidat do existující aplikace Xamarin.Android. Kód je k dispozici na [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) na Githubu.
