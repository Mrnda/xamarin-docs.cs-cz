---
title: Práce s oprávnění
description: Oprávnění jsou možnosti speciální aplikace a k aplikacím, které jsou správně nakonfigurovány pro použití je udělena oprávnění zabezpečení.
ms.prod: xamarin
ms.assetid: 8A3961A2-02AB-4228-A41D-06CB4108D9D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 6ced541ca9df6fcae1643dc14c2e19807e972822
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-entitlements"></a>Práce s oprávnění

_Oprávnění jsou možnosti speciální aplikace a k aplikacím, které jsou správně nakonfigurovány pro použití je udělena oprávnění zabezpečení._

V iOS, aplikace běžet v _izolovaného prostoru_, která poskytuje sada pravidel, které omezují přístup mezi aplikace a některé systémové prostředky nebo uživatelská data. _Oprávnění_ slouží k vyžádání, že systém rozbalte izolovaného prostoru poskytnout aplikaci další funkce.

K rozšíření možností vaší aplikace, je třeba zadat nárok v souboru Entitlements.plist vaší aplikace. Jenom některé funkce se dá rozšířit a tyto podmínky jsou uvedeny v [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce a popisuje [pod](#keyreference). Oprávnění jsou předány na systém jako dvojice klíč/hodnota a obecně je zapotřebí na možnost pouze jedna. Konkrétní klíče a hodnoty jsou popsané [nárocích Key Reference](#keyreference) dále v této příručce.
Visual Studio pro Mac a Visual Studio poskytují zrušte rozhraní pro přidání do aplikace Xamarin.iOS pomocí editoru Entitlements.plist oprávnění.
Tento průvodce představuje editoru Entitlements.plist a způsobu jeho použití. Je také odkaz všechna oprávnění, které mohou být přidány do projektu iOS pro každou funkci.

## <a name="entitlements-and-provisioning"></a>Oprávnění a zřizování


Soubor Entitlements.plist slouží k určení oprávnění a se používá k podepisování sady aplikace.

Některé další zřizování je však vyžaduje pro zajištění toho, že je aplikace správně podepsaný kód. Použít profil zřizování musí obsahovat ID aplikace, který má povolené požadované možnosti. Informace o tom, jak to udělat, najdete [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce.

> [!IMPORTANT]
> Soubor Entitlements.plist pomůže vyplňte správné vlastnosti pro aplikace pomocí možnosti, ale nemůže generovat profil pro zřizování, protože nejsou propojená k účtu vývojáře Apple. Musíte se stále ke generování profil pro zřizování pomocí portálu pro vývojáře k nasazení a distribuce aplikace.

## <a name="set-entitlements-in-a-xamarinios-project"></a>Nastavit oprávnění v projektu Xamarin.iOS

Kromě vyberte a konfiguraci služby požadované aplikace při definování ID aplikace, oprávnění musí být rovněž konfigurována ve projektu Xamarin.iOS úpravou **Info.plist** a  **Entitlements.plist** soubory.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ke konfiguraci oprávnění v sadě Visual Studio pro Mac, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte **Info.plist** soubor otevřete pro úpravy.
2. V **iOS cíl aplikací** , zadejte název aplikace a zadejte **identifikátor svazku** který byl vytvořen při definování ID aplikace:

  ![](entitlements-images/servicexs01.png "Zadejte identifikátor svazku")

3. Uložit změny **Info.plist** souboru.
4. V **Průzkumníku řešení**, dvakrát klikněte **Entitlements.plist** soubor otevřete pro úpravy:

    ![](entitlements-images/servicexs02.png "Úpravy oprávnění")

5. Vyberte a nakonfigurujte jakékoli oprávnění požadované pro aplikace pro Xamarin.iOS tak, že budou odpovídat instalační program, který byl definován při vytvoření ID aplikace.
6. Uložit změny **Entitlements.plist** souboru.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ke konfiguraci oprávnění v sadě Visual Studio, postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **Info.plist**, vyberte **otevřít v programu...** a **Editor seznamu vlastností** soubor otevřete pro úpravy.
2. V **iOS cíl aplikací** , zadejte název aplikace a zadejte **identifikátor svazku** který byl vytvořen při definování ID aplikace:

  ![](entitlements-images/servicevs01.png "Nastavení identifikátor balíku")

3. Uložit změny **Info.plist** souboru.
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Entitlements.plist** soubor, vyberte **otevřít v programu...** a **Editor seznamu vlastností** otevřete pro úpravy:

    ![](entitlements-images/servicevs02.png "Úpravy oprávnění")

    Alternativně poklikáním na **Entitlements.plist** soubor se otevře editor XML zdroj, který vám umožní nastavit hodnotu vlastnosti a klíč nárocích podle popisu v [oprávnění odkaz](#keyreference)části níže.

5. Vyberte a nakonfigurujte jakékoli oprávnění požadované pro aplikace pro Xamarin.iOS tak, že budou odpovídat instalační program, který byl definován při vytvoření ID aplikace.
6. Uložit změny **Entitlements.plist** souboru.


-----

<a name="add-new" />

## <a name="adding-a-new-entitlementsplist-file"></a>Přidání nového souboru Entitlements.plist

Oprávnění jsou přidány do aplikace přes Entitlements.plist soubor. Tento soubor je ve výchozím nastavení součástí Xamarin.iOS projekty, ale může to být chybí starší projekty.

Přidání souboru Entitlements.plist do vaší Xamarin.iOS provést následující kroky:

1.  Klikněte pravým tlačítkem na soubor projektu a přejděte do **Přidat > Nový soubor...** :

    ![Přidat soubory kontextové nabídky](entitlements-images/image1.png)
2.  V dialogovém okně Nový soubor vyberte **iOS > seznam vlastností** a pojmenujte ji oprávnění:

    ![Dialogové okno Nový soubor](entitlements-images/image2.png)

<a name="keyreference" />

## <a name="entitlement-key-reference"></a>Odkaz na nárocích klíče

Klíče nárocích jde přidat prostřednictvím panelu zdroj Entitlements.plist editoru. Požadované klíče se obvykle přidá při použití editoru Entitlements.plist ale jsou níže uvedených pro referenci.

### <a name="wallet"></a>Peněženka

*   **Popis**: dříve označované jako Passbook, Peněženka je aplikace, která uchovává a spravuje předává. Tyto předává může být platební karty, karty úložiště, nástupu předává nebo lístků.

    - **Identifikátor typu pass**
        * **Keys**: com.apple.developer.pass-type-identifiers
        * **Řetězec**: `$(TeamIdentifierPrefix)*`

* **Poznámky k**:
    - Tato akce povolí aplikaci, kterou chcete povolit všechny typy předat. Omezení aplikace a povolit pouze podmnožinu typy průchodu týmů, nastavte hodnotu řetězce: `$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

    Kde pass.$(CFBundleIdentifier) je předat ID, který byl vytvořen [výše](~/ios/platform/passkit.md)

<a name="icloud" />

### <a name="icloud"></a>iCloud

*   **Popis**: Icloudu poskytuje uživatelům iOS pohodlný a jednoduchý způsob ukládání obsahu a sdílet mezi zařízeními. Existují čtyři způsoby vývojáři mohou poskytovat úložiště pro své uživatele pomocí serveru služby iCloud: úložiště klíč-hodnota, UIDocument úložiště, CoreData a pomocí CloudKit přímo k poskytování úložiště pro jednotlivé soubory a adresáře. Další informace o těchto naleznete úvod k příručce serveru služby iCloud.

    - **Dokumenty na serveru služby iCloud & CloudKit**
        - **Klíče**: identifikátorů com.apple.developer.ubiquity kontejneru
        - **Řetězec**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`
    - **Icloudu KeyValue úložiště**
        - **Key**: com.apple.developer.ubiquity-kvstore-identifier
        - **Řetězec**: `$(TeamIdentifierPrefix)$(CFBundleIdentifier)`

* **Poznámky k**:
    - `$(TeamIdentifierPrefix)` Řetězec můžete nalézt protokolování developer.apple.com a najdete **Member Center > váš účet > Souhrn účtu vývojáře** k získání ID týmu (nebo jednotlivé ID pro vývojáře v jednom). Bude řetězec 10 znaků (například A93A5CM278).
    - `$(CFBundleIdentifier)` Řetězec začíná `iCloud` a při Icloudu kontejneru je vytvářet podle kroků v nastavený [práce s možností](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) průvodce.
    - $Expand`(TeamIdentifierPrefix)` a `$(CFBundleIdentifier)` zástupné symboly lze použít a bude nahrazena pro správné hodnoty v čase vytvoření buildu.

### <a name="app-groups"></a>Skupiny aplikací

- **Popis**: skupinu aplikace umožňuje různé aplikace (nebo aplikace a její rozšíření) pro přístup k umístění úložiště sdílený soubor.

    - **Klíč**: com.apple.security.application-groups
    - **Řetězec**: group.$(CFBundleIdentifier)

<a name="apple-pay" />

### <a name="apple-pay"></a>Platím Apple

- **Popis**: Apple platím umožňuje uživatelům platit pro fyzické zboží prostřednictvím zařízení iOS.
    - **Key**: com.apple.developer.in-app-payments
    - **Řetězec**: merchant.your.mechantid

### <a name="push-notifications"></a>Nabízená oznámení

- **Klíč**: přístupových bodů prostředí
- **Řetězec**: `production` nebo `development`

### <a name="siri"></a>Siri

- **Popis**: SiriKit umožňuje aplikaci pro iOS k poskytování služeb, které jsou k dispozici Siri a mapy aplikace na zařízení s iOS pomocí rozšíření aplikace a nové tříd Intent a uživatelského rozhraní záměry architektury. Další informace najdete v části Úvod do SiriKit průvodce.
    - **Key**: com.apple.developer.siri

### <a name="personal-vpn"></a>Osobní VPN

- **Key**: com.apple.developer.networking.vpn.api
- **Řetězec**: Povolit síť vpn

### <a name="keychain-sharing"></a>Sdílení řetězce klíčů

- **Popis**: sdílení řetězce klíčů umožňuje vývojáři aplikace sdílet hesla, které jsou uložené v řetězci klíčů zařízení s jinými aplikacemi vyvinutými stejné týmem. Přístup se dá omezit předáním identifikátor skupiny přístup řetězce klíčů v řetězci.
    - **Klíč**: keychain-access-groups
    - **String**: $(AppIdentifierPrefix) $(CFBundleIdentifier)

### <a name="inter-app-audio"></a>Zvuk mezi aplikacemi

- **Popis**: mimo aplikaci zvuk umožňuje vývojářům datového proudu zvuk mezi aplikacemi.
    - **Klíč**: mimo zvuk aplikace
    - **Logická hodnota**: Ano

### <a name="associated-domains"></a>Associated Domains

- **Popis**: přidružené domén, které by měly být zpracovány jako univerzální odkazy, které mají být předány s toto oprávnění. Univerzální odkazy se dá implementovat umožňuje přímé propojení mezi aplikací a webu. Měli byste jim poskytnout položku pro každou doménu, která vaše aplikace podporuje a každý záznam by měl začínat obráceným `applinks:`
    - **Key**: com.apple.developer.associated-domains
    - **Řetězec**: webcredentials:example.com

### <a name="data-protection"></a>Ochrana dat

- **Popis**: Povolení ochrany dat využívá integrovaný šifrovací hardware k ukládání citlivých dat, které se používají ve vaší aplikaci v zašifrovaném formátu. Ve výchozím nastavení je k dokončení ochrany (soubory jsou přístupné jenom když je zařízení odemknout) nastavit úroveň ochrany.
    - **Key**: com.apple.developer.default-data-protection
    - **String**: NSFileProtectionComplete

### <a name="homekit"></a>HomeKit

- **Popis**: The HomeKit framework poskytuje platformu pro nastavení, konfiguraci a správě podporovaná zařízení domácí automatizace – všechny ze zařízení s iOS. Další informace o použití HomeKit naleznete úvod do HomeKit průvodce.
    - **Key**: com.apple.developer.homekit
    - **Logická hodnota**: Ano

### <a name="healthkit"></a>HealthKit

- **Popis**: HealthKit je zavedená v iOS 8, která poskytuje centralizovanou, koordinované a zabezpečení dat úložiště pro informace týkající se stavu rozhraní. Další informace o použití HealthKit naleznete úvod do HealthKit průvodce.
    - **Key**: com.apple.developer.healthkit
    - **Logická hodnota**: Ano

### <a name="wireless-accessory-configuration"></a>Konfigurace bezdrátového příslušenství

- **Popis**: použití konfigurace bezdrátového příslušenství umožníte své aplikaci konfigurovat příslušenství MFI s připojením Wi-Fi
    - **Key**: com.apple.external-accessory.wireless-configuration
    - **Logická hodnota**: Ano

## <a name="summary"></a>Souhrn

Tato příručka se zavedl oprávnění a jejich použití v sadě Visual Studio pro Mac a v sadě Visual Studio. Je k dispozici také odkaz dvojic klíč/hodnota pro každou funkci.