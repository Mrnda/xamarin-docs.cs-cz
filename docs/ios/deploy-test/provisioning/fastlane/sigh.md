---
title: "sigh fastlane pro iOS –"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3d80a0ab5583231f95241fb8d4f6e339e44a84ca
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="fastlane-for-ios--sigh"></a>sigh fastlane pro iOS –

> [!IMPORTANT]
> fastlane doporučuje použít [ `match` ](~/ios/deploy-test/provisioning/fastlane/match.md) pro vytváření a údržbu zřizovacích profilů. Použijte sigh přímo, pouze v případě, že chtějí mít plnou kontrolu a znát dostatek o podepisování kódu.

## <a name="overview"></a>Přehled

Obvyklým zřizování zařízení provádí každý člen týmu vývoj pomocí Xcode nebo na portálu pro vývojáře Apple. Obsahuje několik kroků:

- Žádosti o certifikát vývoj
- Přidání zařízení do portálu
- Vytvoření ID aplikace
- Vytvoření profilu pro zřizování
- Při stahování profilů a certifikátů

Každý z těchto kroků obsahovat proměnné, které je třeba vzít v úvahu, které závisí na typu aplikace, které vyvíjíte. Další informace o kroky potřebné k nastavení zařízení pro vývoj buď ručně, nebo prostřednictvím Xcode lze nalézt na [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

Tento průvodce představuje fastlane nástroje jako alternativu k použití Xcode a vysvětluje následující:

- [Co je sigh?](#whatissigh)
- [Vytvoření ID aplikace](#appid)
- [Přidání nových zařízení](#newdevices)
- [Pomocí sigh](#using)
- [Další možnosti](#options)

> [!NOTE]
> Pokud je existující ID aplikace, která odpovídá ID sady prostředků aplikace, a pokud vaše zařízení existuje v portálu pro vývojáře, můžete ignorovat kroky na [vytvoření ID aplikace](#appid) a [přidání zařízení](#newdevices). V takovém případě přejděte přímo k [pomocí sigh](#using) začít pracovat.

## <a name="installation"></a>Instalace

Informace o instalaci fastlane, najdete v části Úvod do [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) průvodce.

<a name="whatissigh" />

## <a name="what-is-sigh"></a>Co je sigh

sigh poskytuje terminálu rozhraní, které slouží k vytvoření a obnovení zřizovacích profilů pro všechny konfigurace: vývoj, distribuce úložiště aplikací, Ad Hoc distribuce a distribuce Enterprise. Jeho přidání, která poskytuje přímý způsob, jak stáhnout a opravu profily zřizování.

<a name="appid" />

## <a name="creating-an-app-id"></a>Vytvoření ID aplikace

ID aplikace lze vytvořit pomocí následujícího příkazu:

    fastlane produce -u your@appleid.com -a com.company.appname --skip_itc

Kde `com.company.appname` je ID sady prostředků aplikace, který se nachází v souboru Info.plist aplikace Xamarin.iOS, jak je uvedeno dále:

[ ![](sigh-images/fastlane-image5.png "Do souboru Info.plist aplikace Xamarin.iOS")](sigh-images/fastlane-image5.png)

Jedinečné ID aplikace musí být řetězec styl zpětného DNS. Po vytvoření, zachovat poznamenejte si, jak je budete muset používat v případě, že pomocí sigh dál v této příručce.

Pokud aplikace potřebuje má být vytvořena na [iTunes připojit](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md), odeberte `--skip_itc` příznak z výše uvedeného příkazu.

<a name="newdevices" />

## <a name="adding-new-devices"></a>Přidání nových zařízení

Pokud chcete přidat jednoho zařízení na portál pro vývojáře z příkazového řádku, zadejte následující příkaz:

```bash
fastlane run register_device name:"Adam iPhone" udid:"abcdeg1234567"
```

Chcete-li přidat více než jedno použití zařízení `register_devices` příkaz:

```bash
    register_devices(
        devices: {
            "iPhone 6" => "1234567890123456789012345678901234567890",
            "iPad Air 2" => "abcdefghijklmnopqrstvuwxyzabcdefghijklmn"
         }
    )
```

<a name="using" />

## <a name="using-sigh"></a>Pomocí sigh

Chcete-li začít používat nástroj sigh, zadejte následující příkaz do terminálu:

```bash
fastlane sigh
```

Ve výchozím nastavení vytvoří [obchod distribuční](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) profil pro zřizování. Chcete-li nastavit zařízení pro vývoj, předat `--development` příznak:

```bash
fastlane sigh --development
```

Zadejte svoje Apple ID uživatelské jméno po zobrazení výzvy fastlane. Můžete také být vyzváni k zadání hesla, že pokud je to poprvé použili jste fastlane. Pokud ne, načte proměnné prostředí hesla z řetězci klíčů.

Pokud vaše Apple ID je připojen k více týmů se zobrazí tady. Vyberte číslo, které odpovídá týmu, který chcete použít:

[ ![](sigh-images/fastlane-image2.png "Vyberte tým, který chcete použít")](sigh-images/fastlane-image2.png)

ID týmu lze předat také rozhraní příkazového řádku následujícím způsobem:

```bash
fastlane sigh -l 2TU993NY9J
```

Zadejte [ID aplikace](#appid)) vaší aplikace. Mějte na paměti, mělo by to odpovídat identifikátor svazku v souboru Info.plist vaší aplikace.

Všechna zařízení připojená ke svému účtu se zařadí do vašeho profilu zřizování.

pak bude fastlane vytvořit, stáhněte a nainstalujte profil zřizování pro vás.

Pokud středisku pro vývojáře, můžete zobrazit nově vytvořený profil zřizování, jak je uvedeno dále:

[ ![](sigh-images/fastlane-image10.png "Zobrazení nově vytvořený profil zřizování")](sigh-images/fastlane-image10.png)

sigh uloží zřizovacích profilů v aktuální složce, ve výchozím nastavení. Chcete-li změnit adresář výstupu, upravte `output_path`, nebo postupujte takto:

```bash
fastlane sigh -o "~/Library/MobileDevice/Provisioning Profiles"
```

<a name="options" />

## <a name="sigh-additional-options"></a>Další možnosti sigh

Poskytnout další podporu při použití sigh lze použít následující možnosti:

- Chcete-li stáhnout všechny profily zřizování používat:

    ````bash
    fastlane sigh download_all
    ```

- To use a specific signing identity for your provisioning profile use:

    ```bash
    fastlane sigh -c "Amy cert"
    ```
    
    Where `Amy cert` is the Code Signing Identity name.


## Related Links

- [fastlane - sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme)
