---
title: "Požadavky na systém"
description: "Předpoklady pro použití Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 08/28/2017
ms.openlocfilehash: 4a53053ebef88bf831b7749fa82f3444ecc26723
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="system-requirements"></a>Požadavky na systém

_Předpoklady pro použití Xamarin_

Xamarin produkty závisí na platformě sady SDK od Applu a Google na cílové iOS nebo Android, takže naše požadavky na systém odpovídat skript jeho. Tato stránka popisuje kompatibilitu systému pro Xamarin platformy a doporučené vývojového prostředí a verze sady SDK.

- [Vývojové prostředí](#devenv)
- [systému macOS požadavky](#mac)
- [Požadavky pro Windows](#windows)

Přejděte [pokyny k instalaci](#install) Další informace o získávání softwaru a požadované sady SDK.

<a name="devenv" />

## <a name="development-environments"></a>Vývojové prostředí

Tato tabulka obsahuje platformy, na kterých mohou být vytvořeny vývoj pro různé kombinace nástroj & operační systém:

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> K vývoji pro iOS na počítačích s Windows musí být [počítači Mac, které jsou dostupné v síti](~/ios/get-started/installation/windows/connecting-to-mac/index.md), pro vzdálené kompilace a ladění. Tento postup funguje i pokud máte Visual Studio, které běží uvnitř virtuálního počítače Windows na počítači Mac.

<a name="mac" />

## <a name="macos-requirements"></a>systému macOS požadavky

Použití počítači Mac pro vývoj na platformě Xamarin vyžaduje následující verze softwaru/SDK. Zkontrolujte verzi operačního systému a postupujte podle pokynů [instalační program Xamarin](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode lze nainstalovat (a aktualizovat) na [developer.apple.com](https://developer.apple.com/xcode/download/) nebo prostřednictvím Mac App Storu.

### <a name="testing--debugging-on-macos"></a>Testování a ladění na systému macOS

Mobilní aplikace Xamarin mohou být nasazeny na fyzických zařízení prostřednictvím USB pro testování a ladění (Xamarin.Mac aplikace může být testována přímo na vývojovém počítači; Apple Watch aplikace jsou nasazeny nejprve spárované iPhone).

[!include[](~/cross-platform/includes/macos-testing.md)]


<a name="windows" />

## <a name="windows-requirements"></a>Požadavky pro Windows

Použití počítači se systémem Windows pro vývoj na platformě Xamarin vyžaduje následující verze softwaru/SDK.
Zkontrolujte verzi operačního systému (a potvrďte, že nepoužíváte *Express* verze sady Visual Studio – Pokud ano, zvažte aktualizaci na *komunity* edition).
Visual Studio 2015 a instalační programy 2017 zahrnují možnost automaticky nainstalovat Xamarin.

[!include[](~/cross-platform/includes/windows-requirements.md)]


> [!NOTE]
>
>* Xamarin pro Visual Studio podporuje všechny sady Visual Studio 2015 nebo 2017 (Community, Professional a Enterprise).
>
>* Vývoj Xamarin.Forms aplikací pro univerzální platformu Windows (UWP) vyžaduje Windows 10 pomocí sady Visual Studio 2015 nebo 2017.


### <a name="testing--debugging-on-windows"></a>Testování a ladění na systému Windows

Mobilní aplikace Xamarin můžete nasadit do fyzického zařízení prostřednictvím USB pro testování a ladění (iOS, které zařízení musí být připojen k počítači Mac, není v počítači je spuštění sady Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]


> [!NOTE]
>
>* [Stažení emulátoru Windows Phone 8.1](https://www.microsoft.com/en-us/download/details.aspx?id=43719).
>* Emulátor Windows Phone 10 je součástí Visual Studio 2015 UWP SDK.

<a name="install" />

## <a name="installation-instructions"></a>Pokyny k instalaci

Nejnovější verze Xamarin pro systému macOS si můžete stáhnout z [xamarin.com/download](http://xamarin.com/download). Pro systém Windows, postupujte [Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio) pokyny k instalaci.

Úplný seznam naše aktuální verze produktu je k dispozici na [aktuální verze stránce](http://developer.xamarin.com/releases/current/). Tato stránka také popisuje verze jednotlivých produktu (a odkazy na poznámky k verzi) pro naše beta a alfa kanály.

Konkrétní [instalace](~/cross-platform/get-started/installation/index.md) pokyny pro každou platformu, jsou k dispozici zde:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

K dispozici je také další informace o [Xamarin.Forms požadavky a podporované platformy](~/xamarin-forms/get-started/installation.md).


## <a name="related-links"></a>Související odkazy

- [Stáhnout Xamarin](https://xamarin.com/download/)
- [Aktuální verze](https://developer.xamarin.com/releases/current/)
