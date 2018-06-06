---
title: fastlane iOS – odpovídat
description: Tento dokument popisuje fastlane na shodu příkaz, který se používá k vytváření a údržbu kód podepisování certifikátů, profily pro vývoj pro iOS zřizování.
ms.prod: xamarin
ms.assetid: C4A2A67E-0643-4CED-B1A9-79D65054F3CA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 668538d4c9048175fb95f9d010bb5e95c800fea8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785447"
---
# <a name="fastlane-for-ios---match"></a>fastlane iOS – odpovídat

Obvyklým zřizování zařízení má byla provádí každý člen týmu vývoj pomocí Xcode nebo na portálu pro vývojáře Apple. Obsahuje několik kroků:

- Žádosti o certifikát vývoj
- Přidání zařízení do portálu
- Vytvoření ID aplikace
- Vytvoření profilu pro zřizování
- Stahování profilů a certifikátů

Další informace o kroky potřebné k nastavení zařízení pro vývoj buď ručně, nebo prostřednictvím Xcode najdete v [zařízení Povisioning](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

Tento průvodce představuje způsob použití nástroje fastlane jako alternativu k použití Xcode.

## <a name="installation"></a>Instalace

Informace o instalaci fastlane, najdete v části Úvod do [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) průvodce.

<a name="whatismatch" />

## <a name="what-is-match"></a>Co je shoda?

porovnání má na starosti vytvoření a udržování kódu podpisové certifikáty a zřizování profily a umožňuje iOS vývojového týmu sdílet jeden podpis identity napříč všechny vývojáře kódu.

Při nasazení aplikace v App Storu, provádění testování verze beta nebo instalaci aplikace do zařízení, má každý člen týmu pro vývoj vlastních podpisové identity. To může mít za následek konfliktní identit a profily a nemusíte vytvářet ručně, otáčení a spravovat profily a ID aplikace.

Místo toho shodu vytvoří a udržuje všech certifikátů a profilů pro vás a ukládá je do úložiště privátní git. Vývojáři všechny týmu pro přístup a použít tyto přihlašovací údaje. Pak to poskytuje další zabezpečení pro certifikáty: nejen v úložišti git privátní, budou také jsou také šifrován [přístupové heslo](#passphrase). Ukládání artefaktů v úložišti pro podpis kódu umožňuje team agentů a správcům, a aktualizovat otočit certifikáty podle potřeby, což znamená, že je méně času stráveného distribuci nové certifikáty pro každý vývojář.

> [!IMPORTANT]
> Shoda v současné době nepodporuje profily interní Enterprise.

<a name="initializing" />

## <a name="initializing-your-project-with-match"></a>Inicializace vašeho projektu s shodou

Pokud jste správce týmu, vytvoření úložiště privátní git, buď prostřednictvím webu github.com nebo bitbucket.com Ujistěte se, že přidáte, že všichni členové jako přispěvatele k úložišti týmu.

Pomocí terminálu, změňte adresář na adresář projektu a spusťte:

    fastlane match init

Po zobrazení výzvy zadejte adresu URL úložiště git:

 [![](match-images/fastlane-image7.png "Zadejte adresu URL úložiště git")](match-images/fastlane-image7.png#lightbox)

Adresu URL můžete najít a zkopírovat kliknutím **klonovat nebo stáhnout** tlačítko na webu github.com, jak je uvedeno dále:

[![](match-images/fastlane-image6.png "Adresu URL v části tlačítko klonovat nebo stáhnout na webu github.com")](match-images/fastlane-image6.png#lightbox)

Inicializace projektu vytvoří matchfile – textový soubor, který lze upravovat předat proměnné prostředí nástroj pro porovnání. Dole je zobrazená příklad matchfile:

[![](match-images/fastlane-image8.png "Příklad matchfile")](match-images/fastlane-image8.png#lightbox)

<a name="running" />

## <a name="running-match"></a>Spuštění shody

> [!NOTE]
> doporučuje fastlane, které dřív, než spustíte odpovídají poprvé, měli byste zvážit vymazání stávající profilů a certifikátů pomocí [odpovídat nuke příkaz](#using).

V závislosti na tom, jaké prostředí vyžadovat, že k vytvoření nového certifikátu a profil pro zřizování a uložte ji do vaší nové úložiště git můžete použít některou z následujících příkazů:

    fastlane match appstore

    fastlane match adhoc

    fastlane match development

Kromě vytváření nových certifikátů a profilů, pomocí kteréhokoli z těchto příkazů přidá (nebo aktualizovat, pokud ještě splněné) do vašeho úložiště git následující položky:

- Certifikáty složky
- Profily složky
- Soubor readme s základní pokyny
- Shoda verze

[![](match-images/fastlane-image9.png "Struktura projektu v úložišti git")](match-images/fastlane-image9.png#lightbox)

Profily zřizování jsou nainstalovány ve `~/Library/MobileDevice/Provisioning Profiles`. Certifikáty a soukromé klíče jsou nainstalovány přímo v řetězci klíčů.

<a name="using" />

### <a name="using-the-nuke-command"></a>Pomocí `nuke` příkaz

Pokud máte untidy certifikátů můžete použít `nuke` k odvolání certifikátů a profilů pro každé prostředí použijte následující příkazy:

    fastlane match nuke

Chcete-li odebrat všechny certifikáty a zřizovacích profilů pro jakékoli konkrétní prostředí:

    fastlane match nuke development

 or

    fastlane match nuke distribution

fastlane potvrdí soubory, které se odeberou než nic odstraněna.

<a name="passphrase" />

### <a name="passphrase"></a>Přístupové heslo

Při spuštění `match` poprvé, zobrazí se výzva k nastavit heslo pro úložiště git. Nezapomeňte si pamatujete heslo, protože ho budete potřebovat při spuštění shodu na jiný počítač. To je další vrstvu zabezpečení: každý ze souborů se budou šifrovat pomocí OpenSSL. Každý následující spuštění `match` na nový počítač zobrazí výzvu pro toto přístupové heslo – po začátku zadání se heslo bude přidán do místní řetězce klíčů.

Chcete-li nastavit heslo pro dešifrování profilech pomocí proměnné prostředí, použijte `MATCH_PASSWORD`.

<a name="options" />

## <a name="additional-options"></a>Další možnosti

Poskytnout další podporu při použití shodu lze použít následující možnosti:

- Použití `-–help` příznak seznam všechny dostupné příkazy:

        fastlane match cert --help

- Použití `-–verbose` příznak zvýšení podrobností výstupu:

        fastlane match --development --verbose

- Použití `--force_for_new_devices` příznak vynutit zajišťování profily obnovit, pokud došlo ke změně počet zařízení na portál pro vývojáře "

        fastlane match development --force_for_new_devices

## <a name="related-links"></a>Související odkazy

- [fastlane - odpovídat](https://github.com/fastlane/fastlane/blob/master/match/README.md)
