---
title: "fastlane pro iOS – certifikátu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: b98375f8a526cd08f7d11f4ea6bb3498db87009c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="fastlane-for-ios--cert"></a>fastlane pro iOS – certifikátu.

> [!IMPORTANT]
> fastlane doporučuje použít [ `match` ](~/ios/deploy-test/provisioning/fastlane/match.md) nástroj pro vytváření a údržbu certifikáty. Použití `cert` přímo pouze, pokud chtějí mít plnou kontrolu a znáte dostatek o podepisování kódu. Pomocí této akce stáhnout nejnovější identity pro podpis kódu.

## <a name="overview"></a>Přehled

Obvyklým zřizování zařízení provádí každý člen týmu vývoj pomocí Xcode nebo na portálu pro vývojáře Apple. Obsahuje několik kroků:

- Žádosti o certifikát vývoj
- Přidání zařízení do portálu
- Vytvoření ID aplikace
- Vytvoření profilu pro zřizování
- Stahování profilů a certifikátů

Každý z těchto kroků obsahuje proměnné, které je třeba vzít v úvahu, které závisí na typu aplikace, které vyvíjíte. Další informace o kroky potřebné k nastavení zařízení pro vývoj buď ručně, nebo prostřednictvím Xcode lze nalézt na [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

Tento průvodce představuje fastlane nástroje jako alternativu k použití Xcode a vysvětluje následující:

- [Co je certifikát?](#whatiscert)
- [Pomocí certifikátu](#using)
- [Další možnosti](#options)

## <a name="installation"></a>Instalace

Informace o instalaci a aktualizaci fastlane, najdete v části Úvod do [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) průvodce.

<a name="whatiscert" />

## <a name="what-is-cert"></a>Co je certifikát?

certifikát poskytuje terminálu rozhraní, které vytvoří nové identity pro podpis kódu (často označované jako vývojář _certifikát_) pro vývoj a distribuci prostředí.

<a name="using" />

## <a name="using-cert"></a>Pomocí certifikátu

Použití nástroje certifikátu, zadejte do terminálu rozhraní příkazového řádku následující příkaz:

    fastlane cert

Ve výchozím nastavení tím se vytvoří certifikát distribuce. Chcete-li vytvořit vývojový certifikát, předat `--development` příznak:

    fastlane cert --development

certifikát zobrazí výzvu k zadání Apple ID a hesla, zadejte proto to teď:

[ ![](cert-images/fastlane-image1.png "certifikát zobrazí výzvu k zadání Apple ID a hesla")](cert-images/fastlane-image1.png)

> [!IMPORTANT]
> Prvním je zadané heslo je uložen do místního systému macOS řetězce klíčů. Alternativně proměnné prostředí lze použít k uložení uživatelského jména a hesla, nebo můžete použít `export fastlane_DONT_STORE_PASSWORD=1` Pokud nechcete, aby vaše heslo uložené v řetězci klíčů. Další informace o správě přihlašovacích údajů s fastlane najdete na fastlane [Průvodce přihlašovací údaje správce](https://github.com/fastlane/fastlane/blob/master/credentials_manager/README.md).

Apple ID můžete také předat jako argument pomocí následujícího příkazu:

    fastlane cert -u myemailadress@domain.com

Pokud vaše Apple ID je připojen k více týmů, se zobrazí tady. Vyberte číslo, které odpovídá týmu, který chcete použít:

[ ![](cert-images/fastlane-image2.png "Vyberte tým, který chcete použít")](cert-images/fastlane-image2.png)

ID týmu lze předat také pomocí příznak následující:

    fastlane cert -l 2TU993NY9J

fastlane bude zkontrolujte, zda některé z dostupných podepisování certifikátů je nainstalována na místním počítači, a pokud je se bude používat.

Pokud není žádná podpisový certifikát, bude certifikát:

- Vytvořit nový privátní klíč a podepisování požadavku
- Generování, stáhněte a nainstalujte certifikát
- Import certifikátu a privátního klíče do řetězci klíčů

Pokud bylo dosaženo maximální počet podpisový identit, které jsou povoleny pro váš účet, bude vyvolána výjimka. Pokud chcete vytvořit nový podpisový identitu, musíte ručně odvolat jednu z existujících certifikátů prostřednictvím středisku pro vývojáře a akci opakujte.

> [!NOTE]
> fastlane nemůže stáhnout existující podpisový identit z středisku pro vývojáře, pokud nejsou již v řetězci klíčů. Je to proto, že privátní klíč existuje vždy jen v počítači, nebo v exported(*.p12) verze certifikátu a nikdy v Centru pro vývojáře.

<a name="options" />

## <a name="additional-options"></a>Další možnosti

Při použití certifikátu poskytnout další podporu lze použít následující možnosti:

- Použití `-–help` příznak seznam všechny dostupné příkazy:

        fastlane cert --help

- Použití `-–verbose` příznak zvýšení podrobností výstupu

        fastlane cert --development --verbose


## <a name="related-links"></a>Související odkazy

- [fastlane – certifikátu.](https://github.com/fastlane/fastlane/blob/master/cert/README.md)
