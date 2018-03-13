---
title: "Certifikáty a identifikátory"
description: "Tento průvodce vás provede vytvořením potřebné certifikáty a identifikátory, které bude vyžadovat, aby publikování aplikace Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a1065fb91a23827c4876654470cda5022aa1d3b8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="certificates-and-identifiers"></a>Certifikáty a identifikátory

_Tento průvodce vás provede vytvořením potřebné certifikáty a identifikátory, které se požadují k publikování aplikace Xamarin.Mac._

## <a name="certificates-and-identifiers"></a>Certifikáty a identifikátory

Přejděte [Apple Developer Member Center](http://developer.apple.com) konfigurace Mac pro vývoj. Hlavní nabídky jsou uvedeny níže:

[![Apple Developer Member Center](certificates-identifiers-images/devcenter01.png "Apple Developer Member Center.")](certificates-identifiers-images/devcenter01-large.png#lightbox)

Klikněte na **certifikáty, identifikátory a profily** odkaz:

[![Výběr certifikáty, identifikátory a profily](certificates-identifiers-images/devcenter02.png "výběru certifikátů, identifikátory a profily")](certificates-identifiers-images/devcenter02-large.png#lightbox)

Klikněte na tlačítko na **odkaz certifikáty** v části **Mac aplikace** části:

[![Výběr odkaz certifikáty](certificates-identifiers-images/devcenter03.png "výběru certifikátů odkazu")](certificates-identifiers-images/devcenter03-large.png#lightbox)

Klikněte na **všechny** propojit a klikněte na  **+**  tlačítko:

[![Výběrem možnosti všechny a přidání nové položky](certificates-identifiers-images/certif01.png "výběrem možnosti všechny a přidání nové položky")](certificates-identifiers-images/certif01-large.png#lightbox)

Z sem stahování **zprostředkující certifikáty** (po celém světě vývojáře vztahů certifikační autority a vývojáře ID certifikační autority) v případě potřeby. Ale tady by měly být automaticky instalační program pro vývojáře pomocí Xcode.

Zbytek této části vás provede všechny čtyři oddíly pro dokončení instalace Mac vývojářský účet.

-   **ID aplikace pro Mac zaregistrovat** – vývojář muset postupujte podle těchto kroků pro každou aplikaci, budou zapisovat.
-   **Zaregistrovat systému macOS systémy** – používá se pouze požadované při přidávání počítače se otestovat s.
-   **Vytvářet certifikáty** – pouze při vytváření up jednou vyžaduje certifikáty a při obnovování je novější.
-   **Vytvořit profil zřizování** – Vývojář nutné provést následující kroky pro každou novou aplikaci zapisovat a při přidávání nových systémů.

Klikněte **přehled** odkaz v horní části levé části stránky se vraťte do této nabídky kdykoli.

### <a name="register-mac-app-id"></a>Zaregistrovat ID aplikace pro Mac

Vývojář musí zaregistrovat ID aplikace pro každou aplikaci zapsána. Pomocí následujících kroků vytvořte položku pro základní ukázkovou aplikaci názvem "MacWriter".

1. Zadejte **popis ID aplikace** a vyberte některou **App Services** vyžadující aplikace: 

    [![Zadání popisu a aplikaci služby](certificates-identifiers-images/devcenter04.png "zadat popis a aplikaci služby")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. Zadejte **ID sady** pro aplikaci a kliknutím **pokračovat** tlačítko: 

    [![Zadáním ID sady](certificates-identifiers-images/devcenter05.png "zadáte ID sady")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. Zkontrolujte zadané informace a klikněte na **odeslání** tlačítko: 

    [![Ověřování informací o](certificates-identifiers-images/devcenter06.png "ověřování informací o")](certificates-identifiers-images/devcenter06-large.png#lightbox)

Některé **App Services** by mohly vyžadovat další konfiguraci (například Icloudu). Pokud je to tento případ, vyberte nové ID aplikace právě vytvořili a klikněte **upravit** tlačítko:

[![Úpravy nové ID aplikace](certificates-identifiers-images/devcenter07.png "úpravy nové ID aplikace")](certificates-identifiers-images/devcenter07-large.png#lightbox)

Konfigurace serveru služby iCloud služeb, klikněte na **upravit** tlačítko:

[![Konfigurace služby serveru služby iCloud](certificates-identifiers-images/devcenter08.png "konfigurace služby serveru služby iCloud")](certificates-identifiers-images/devcenter08-large.png#lightbox)

Odsud můžete nakonfigurovat vývojář databáze, které budete používat:

[![Konfigurace databáze](certificates-identifiers-images/devcenter09.png "konfigurace databáze")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### <a name="register-macos-systems"></a>Registrace systému macOS systémy

Pokud chcete vytvořit profil pro zřizování pro testování, vývojář muset mít počítače Mac zaregistrované. Nesmí být delší než 100 počítačích pro testování svých aplikací pro Mac se můžete zaregistrovat.

V Centru pro vývojáře Mac vyberte **všechny** z **zařízení** části a klikněte na tlačítko  **+**  tlačítko:

[![Přidání nového počítače](certificates-identifiers-images/devcenter10.png "přidání nového počítače")](certificates-identifiers-images/devcenter10-large.png#lightbox)

Zadejte **název** a **UUID** počítače k přidání a klikněte na **pokračovat** tlačítko. Zkontrolujte informace a kliknutím na možnost **zaregistrovat** tlačítko:

[![Zadání informací o novém počítači](certificates-identifiers-images/devcenter11.png "zadávání informací o novém počítači")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### <a name="create-certificates"></a>Vytvoření certifikátů

V části certifikáty použijte k vytvoření několika různých typů certifikátů, které se použijí k podepisování aplikací systému Mac:

[![Vytvoření nového certifikátu](certificates-identifiers-images/certif01.png "vytváření nového certifikátu")](certificates-identifiers-images/certif01-large.png#lightbox)

Existují tři hlavní typy certifikátu:

-   **Certifikát počítače Mac vývoj** – volitelné pro vývoj aplikací Obecné, ale vyžaduje Pokud vývojář plánuje používat funkce, jako Icloudu nebo nabízená oznámení. Vývojář potřebovat certifikát vývoj předtím, než mohou vytvářet profily zřizování, který povolení jejich přístupu k nim.
-   **Mac App Storu** – vývojář potřebovat certifikát pro své aplikace a jiný certifikát pro instalační program.
-   **ID vývojáře** – certifikáty pro aplikace a instalační program, pokud se rozhodnete distribuovat mimo Mac App Storu.

V následujících částech se příklady vytváření každou z výše uvedených typů certifikátů.

#### <a name="mac-development-certificate"></a>Vývoj pro certifikát počítače Mac

Jak je uvedeno nahoře, certifikát vývoj aplikace Mac není povinné, pokud systému macOS funkce, jako jsou Icloudu nebo nabízená oznámení jsou používány.

Vytvořit nový certifikát vývoj takto:

1. Vyberte **Mac vývoj** a klepněte na tlačítko **pokračovat**: 

     [![Přidání certifikátu vývoj](certificates-identifiers-images/certif02.png "přidání certifikátu vývoj")](certificates-identifiers-images/certif02-large.png#lightbox)
2. Na další obrazovce vysvětluje, jak vytvořit soubor žádosti o nahrát podepsání certifikátu pomocí přístup do řetězce klíčů: 

    [![Obrazovka řetězce klíčů přístupu nahrávání](certificates-identifiers-images/certif03.png "obrazovka nahrávání přístupu řetězce klíčů")](certificates-identifiers-images/certif03-large.png#lightbox)
3. Zvolte smysluplný běžný název certifikátu, tak, aby se snadno rozpoznatelné později při posledním certifikát je vytvořen. Mějte na paměti, uložení souboru, takže se nachází v dalším kroku: 

     ![Export certifikátu](certificates-identifiers-images/image12.png "Export certifikátu")
4. Žádost o soubor certifikátu (rozšíření `.certSigningRequest`) budou uloženy lokálně na Mac. Mějte na paměti, kde je uložen (výchozí locationis ploše) ji budou muset být zvolena v dalším kroku: 

     [![Nahrání souboru certifikátu](certificates-identifiers-images/image13.png "nahrání souboru certifikátu")](certificates-identifiers-images/image13-large.png#lightbox)
5. Klikněte na tlačítko **Stáhnout** k získání certifikátu a klikněte dvakrát na jeho v instalaci **řetězce klíčů**: 

     [![Stahování vývojový certifikát](certificates-identifiers-images/image15.png "stahování vývojový certifikát")](certificates-identifiers-images/image15-large.png#lightbox)
6. Klikněte na tlačítko **Stáhnout** k získání certifikátu a klikněte dvakrát na jeho v instalaci **řetězce klíčů**. **Vývojáře certifikát nástroj** zobrazí certifikáty takto: 

     [![Nástroje Developer](certificates-identifiers-images/image16.png "nástroje Developer")](certificates-identifiers-images/image16-large.png#lightbox)
7. Zobrazí také v **řetězce klíčů** podobné výjimky: 

     ![Certifikátu v nástroji Keychain Access](certificates-identifiers-images/image17.png "certifikátu v přístup do řetězce klíčů")

Jak už jsme zmínili, že certifikátu vývojáře není vždy vyžaduje Pokud vývojář je implementace systému macOS funkcí, jako je iCloud a nabízených oznámení. Také je nutné vytvořit **profil zřizování vývoj**, který bude potřeba k testování aplikací z Mac App Storu.

#### <a name="mac-app-store-certificates"></a>Certifikáty Mac App Storu

K vydání aplikace na webu App Store **Mac App Storu** certifikát, který bude používat k podepsání aplikace a balíček Instalační služby systému Mac bude muset znovu vytvořit.

1. Vyberte **Mac App Storu** typ certifikátu a klikněte na **pokračovat** tlačítko: 

    [![Vytváření certifikát obchod](certificates-identifiers-images/certif04.png "vytváření certifikát obchodu s aplikacemi")](certificates-identifiers-images/certif04-large.png#lightbox)
2. Vyberte typ certifikát, který chcete vytvořit (každého typu pro uvolnění do App Storu bude potřeba): 

    [![Vyberte typ certifikátu](certificates-identifiers-images/certif05.png "vyberete typ certifikátu")](certificates-identifiers-images/certif05-large.png#lightbox)
3. Na další stránku vysvětluje, jak používat **přístup do řetězce klíčů** můžete vytvořit soubor žádosti o certifikát. Postupujte podle pokynů: 

     [![Generování řetězce klíčů žádost](certificates-identifiers-images/certif06.png "generování žádost o řetězce klíčů")](certificates-identifiers-images/certif06-large.png#lightbox)
4. Zvolte popisný **běžný název** – použijte například textu "App Store aplikace" v názvu: 

     ![Zadat popisný název](certificates-identifiers-images/image20.png "zadat popisný název")
5. Žádost o soubor certifikátu (rozšíření `.certSigningRequest`) budou uloženy lokálně na Mac. Mějte na paměti, kde je uložen (výchozí umístění je na ploše): 

     [![Ukládání certifikát](certificates-identifiers-images/image21.png "uložení certifikátu")](certificates-identifiers-images/image21-large.png#lightbox)
6. Klikněte na tlačítko **Stáhnout** k získání vašeho certifikátu a klikněte dvakrát na jeho v instalaci **řetězce klíčů**: 

      [![Stáhnout certifikát obchod](certificates-identifiers-images/image23.png "stáhnout certifikát obchodu s aplikacemi")](certificates-identifiers-images/image23-large.png#lightbox)
7. Klikněte na tlačítko **pokračovat** a přesně stejným postupem ke stažení této doby bude pro jiný certifikát, *instalační program*: 

     [![Výběr instalační program](certificates-identifiers-images/image24.png "výběr instalační program")](certificates-identifiers-images/image24-large.png#lightbox)
8. Zvolte popisný **běžný název** – příklad použijte text "Instalační program AppStore" v názvu: 

     ![Nastavení názvu certifikátu](certificates-identifiers-images/image25.png "nastavení názvu certifikátu")
9. Žádost o soubor certifikátu (rozšíření `.certSigningRequest`) budou uloženy lokálně na Mac. Mějte na paměti, kde je uložen (výchozí umístění je na ploše): 

     [![Nahrávání certifikátu](certificates-identifiers-images/image26.png "nahrávání certifikátu")](certificates-identifiers-images/image26-large.png#lightbox) 

     [![Stažení certifikátu distribučního](certificates-identifiers-images/image28.png "stáhnout certifikát distribuce")](certificates-identifiers-images/image28-large.png#lightbox)
10. Klikněte na tlačítko **Stáhnout** k získání certifikátu a klikněte dvakrát na jeho v instalaci **řetězce klíčů**. Nástroj certifikátu vývojáře zobrazí certifikáty takto: 

     [![Nástroje Developer](certificates-identifiers-images/image29.png "nástroje Developer")](certificates-identifiers-images/image29-large.png#lightbox)
11. Teď dvě nové certifikáty bude zobrazen **řetězce klíčů**: 

     [![Certifikátu v nástroji Keychain Access](certificates-identifiers-images/image30.png "certifikátu v přístup do řetězce klíčů")](certificates-identifiers-images/image30-large.png#lightbox)

#### <a name="developer-id-certificates"></a>Certifikáty ID vývojáře

Samoobslužné uvolnění Xamarin.Mac aplikace (ne vydání prostřednictvím Apple App Store), vývojář potřebovat certifikát ID vývojáře k podepsání aplikace pro verzi a instalaci.

Postupujte takto:

1. Z **certifikáty** část, začněte tím, že klikněte na tlačítko  **+**  tlačítko a pak vyberte **vývojáře ID** přepínače: 

    [![Přidání vývojáře ID](certificates-identifiers-images/certif07.png "přidávání ID vývojáře")](certificates-identifiers-images/certif07-large.png#lightbox)
2. Klikněte **pokračovat** tlačítko a vyberte typ ID vývojáře k vytvoření: 

    [![Výběr typu ID vývojáře](certificates-identifiers-images/certif08.png "výběr typu ID vývojáře")](certificates-identifiers-images/certif08-large.png#lightbox)
3. Dva se bude vyžadovat, jeden pro přihlášení vlastní aplikace a druhý k podepsání aplikace Instalační služby. Buďte opatrní při pojmenování žádosti o certifikát pro tyto klíče: pomocí popisných názvů, které zahrnují text `Application` a `Installer` , může se jednat o rozlišující později.
4. Na další obrazovce bude poskytuje podrobné pokyny o tom, jak vytvořit certifikát, klikněte na **pokračovat** tlačítko: 

    [![Postup vytvoření certifikátu](certificates-identifiers-images/certif09.png "postup vytvoření certifikátu")](certificates-identifiers-images/certif09-large.png#lightbox)
5. Zvolte popisný **běžný název** – použijte například text "Developer ID aplikace" v názvu: 

     ![Zadávání názvu certifikátu](certificates-identifiers-images/image33.png "zadávání názvu certifikátu")
6. Žádost o soubor certifikátu (rozšíření `.certSigningRequest`) se uloží místně na vašem Mac. Mějte na paměti, kde je uložen (výchozí umístění je na ploše): 

     [![Nahrávání certifikátu](certificates-identifiers-images/certif10.png "nahrávání certifikátu")](certificates-identifiers-images/certif10-large.png#lightbox) 

     [![Stažení ID vývojáře](certificates-identifiers-images/certif11.png "stahování ID vývojáře")](certificates-identifiers-images/certif11-large.png#lightbox)
7. Klikněte na tlačítko **Stáhnout** k získání certifikátu a klikněte dvakrát na jeho v instalaci **řetězce klíčů**.
8. Klikněte na tlačítko **pokračovat** a přesně stejným postupem ke stažení této doby bude pro jiný certifikát, *instalační program*.
9. Zvolte popisný **běžný název** – použijte například v názvu text "Developer ID instalační program": 

     ![Zadáte běžný název](certificates-identifiers-images/image38.png "zadáte běžný název")
10. Žádost o soubor certifikátu (rozšíření `.certSigningRequest`) budou uloženy lokálně na Mac. Mějte na paměti, kde je uložen (výchozí umístění je na ploše): 

     [![Nahrávání certifikátu](certificates-identifiers-images/certif10.png "nahrávání certifikátu")](certificates-identifiers-images/certif10-large.png#lightbox)
11. Certifikát je poté k dispozici ke stažení – klikněte na tlačítko **Stáhnout** před kliknutím na tlačítko **provádí**: 

     [![Stažení certifikátu](certificates-identifiers-images/certif11.png "stažení certifikátu")](certificates-identifiers-images/certif11-large.png#lightbox)
12. Klikněte na tlačítko **Stáhnout** k získání certifikátu a klikněte dvakrát na jeho v instalaci **řetězce klíčů**. **Vývojáře certifikát nástroj** zobrazí certifikáty takto: 

     [![Nástroje Developer](certificates-identifiers-images/certif12.png "nástroje Developer")](certificates-identifiers-images/certif12-large.png#lightbox)
13. Následující položky, které jsou viditelné v **řetězce klíčů**: 

     [![Certifikátu v nástroji Keychain access](certificates-identifiers-images/image43.png "certifikátu v přístup do řetězce klíčů")](certificates-identifiers-images/image43-large.png#lightbox)


## <a name="related-links"></a>Související odkazy

- [Instalace](/visualstudio/mac/installation/)
- [Hello, ukázka Mac](~/mac/get-started/hello-mac.md)
- [Distribuce aplikací na Mac App Storu](https://developer.apple.com/devcenter/mac/checklist/)
- [ID vývojáře a těchto pravidel](https://developer.apple.com/resources/developer-id/)
