---
title: Publikování nezávisle
ms.prod: xamarin
ms.assetid: 6FB4DEF2-01AD-C5FE-0950-CE1BF088A9C6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: f7ba0620a4639ff62e2d75d7cf8f02fcc01faac5
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/04/2018
ms.locfileid: "33113470"
---
# <a name="publishing-independently"></a>Publikování nezávisle

Je možné k publikování aplikace bez použití žádné existující tržišť Android. V této části se popisují tyto jiné metody publikování a licencování úrovně Xamarin.Android.


## <a name="xamarin-licensing"></a>Licencování Xamarin

Pro vývoj, nasazení a distribuci aplikací Xamarin.Android k dispozici jsou čtyři licencí:

-   **Visual Studio Community** &ndash; pro studenty, malé týmy a OSS vývojáře, kteří používají Windows.

-   **Visual Studio Professional** &ndash; pro jednotlivé vývojáře nebo malé týmy (jenom Windows). Tuto licenci nabízí standardní nebo cloudové předplatné, přístup k obsahu další univerzity Xamarin a žádná omezení použití.

-   **Visual Studio Enterprise** &ndash; pro týmy jakékoli velikosti (jenom Windows). Tuto licenci obsahuje funkce enterprise, standard nebo cloudové předplatné.

Přejděte [visualstudio.com](https://www.visualstudio.com/xamarin/) stahování Community Edition nebo Další informace o zakoupení edice Professional a Enterprise.


## <a name="allow-installation-from-unknown-sources"></a>Povolit instalace z neznámých zdrojů

Ve výchozím nastavení Android zabrání uživatelům stahování a instalaci aplikací z umístění než Google Play. Instalace z jiných marketplace zdrojů, uživatel musí povolit *neznámé zdroje* nastavení na zařízení před pokusem o instalaci aplikace. Toto nastavení lze najít v části **Nastavení > zabezpečení**, jak je znázorněno v následujícím diagramu:

[![Obrazovka nastavení zabezpečení](publishing-independently-images/settings.png)](publishing-independently-images/settings.png#lightbox)


> [!IMPORTANT]
> Někteří poskytovatelé sítě by mohly bránit instalaci aplikací z neznámých zdrojů, bez ohledu na toto nastavení.



## <a name="publishing-by-e-mail"></a>Publikování pomocí e-mailu

Verze APK se připojuje k e-mailu je rychlý a snadný způsob, jak distribuovat aplikace pro uživatele. Když uživatel otevře e-mail na zařízení se systémem Android zapnuté, Android rozpozná přílohu APK a zobrazit **nainstalovat** tlačítko jak je znázorněno na následujícím obrázku:

[![Tlačítko pro přílohy instalovat](publishing-independently-images/publishing-via-email.png)](publishing-independently-images/publishing-via-email.png#lightbox)

Přestože distribuční prostřednictvím e-mailu je jednoduché, poskytuje několik ochranná opatření proti pirátství nebo neoprávněná distribuce. Nejlepší je vyhrazený pro situacích, kdy jsou několik příjemců aplikace a jsou důvěryhodné nechcete distribuce aplikace.


## <a name="publishing-by-web"></a>Publikování pomocí webové

Je možné aplikaci distribuovat webový server. To lze provést odeslání aplikací na webový server a pak zadají odkaz ke stažení pro uživatele. Pokud zařízení se systémem Android zapnuté přejde k odkazu a potom stáhne aplikace, aplikace bude nainstalována automaticky po dokončení stahování.


## <a name="manually-installing-an-apk"></a>Ruční instalaci APK

Ruční instalace je třetí možnost pro instalaci aplikace. Za účelem ruční instalace aplikace:

1.   **Distribuovat kopii APK uživateli** &ndash; například tato kopie mohou být distribuovány na disku CD nebo USB flash disku.
1.   **(Uživatel) nainstaluje aplikaci na zařízení s Androidem** &ndash; pomocí příkazového řádku *Android ladění most* (**adb**) nástroj. **ADB** je univerzální nástroj příkazového řádku, která umožňuje komunikaci se instance emulátoru nebo zařízení se systémem Android zapnuté. Sada Android SDK zahrnuje **adb**; se nachází v adresáři  **<sdk>/platform-tools /**.

Zařízení s Androidem musí být připojen pomocí kabelu USB k počítači.
Počítače se systémem Windows může také vyžadovat další ovladače USB od dodavatele phone rozpoznala **adb**. Pokyny k instalaci pro tyto další ovladače USB je nad rámec tohoto dokumentu.

Před vydáním žádné **adb** příkazy, je dobré vědět, které instance emulátoru nebo zařízení připojená, pokud existuje. Je možné zobrazit seznam co je připojen pomocí `devices` příkaz, jak je předvedeno v následující fragment kódu:

```shell
$ adb devices
List of devices attached
        0149B2EC03012005device
```

Po potvrzení připojená zařízení může být aplikace nainstalována vydáním `install` s **adb**:

```shell
$ adb install <path-to-apk>
```

Následující fragment kódu ukazuje příklad instalaci aplikace na připojené zařízení:

```shell
$ adb install helloworld.apk
3772 KB/s (3013594 bytes in 0.780s)
        pkg: /data/local/tmp/helloworld.apk
Success
```

Pokud je aplikace již nainstalována, `adb install` nebude možné nainstalovat APK a bude sestava selhání, jak je znázorněno v následujícím příkladu:

```shell
$ adb install helloworld.apk
4037 KB/s (3013594 bytes in 0.728s)
        pkg: /data/local/tmp/helloworld.apk
Failure [INSTALL_FAILED_ALREADY_EXISTS]
```

Bude potřeba k odinstalaci aplikace ze zařízení. Nejprve vydávat `adb uninstall` příkaz:

```shell
adb uninstall <package_name>
```

Následující fragment kódu je příklad odinstalace aplikace:

```shell
$ adb uninstall mono.samples.helloworld
Success
```
