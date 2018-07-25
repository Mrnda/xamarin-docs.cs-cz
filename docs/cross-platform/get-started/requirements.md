---
title: Požadavky na systém
description: Tento dokument uvádí požadavky na systém pro sestavování aplikací s využitím kódu Xamarin na počítače se systémy Mac a Windows. Taky obsahuje odkazy na pokyny k instalaci.
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: conceptdev
ms.author: crdun
ms.date: 07/24/2018
ms.openlocfilehash: 6d16f01965b6b3bcba35cf14d4000f53a4400653
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241975"
---
# <a name="system-requirements"></a>Požadavky na systém

Produkty Xamarin využívají platformu SDK od společnosti Apple a Google do cílového systému iOS nebo Android, takže naše požadavky na systém odpovídat jejich. Tato stránka popisuje kompatibilitu systému pro platformu Xamarin a doporučené vývojové prostředí a verze sady SDK.

- [Vývojová prostředí](#devenv)
- [požadavky na macOS](#mac)
- [Požadavky na Windows](#windows)

Přejděte [pokyny k instalaci](#install) pro další informace o získání softwaru a požadované sady SDK.

<a name="devenv" />

## <a name="development-environments"></a>Vývojová prostředí

Tato tabulka uvádí platformy, na kterých se dají vytvářet pomocí kombinace jiný vývojářský nástroj & operační systém:

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> K vývoji pro iOS na počítačích Windows musí být [počítači Mac, které jsou dostupné v síti](~/ios/get-started/installation/windows/connecting-to-mac/index.md), pro vzdálené sestavování a ladění. Tento postup funguje i pokud používáte sadu Visual Studio běží uvnitř virtuálního počítače s Windows na počítači Mac.

<a name="mac" />

## <a name="macos-requirements"></a>požadavky na macOS

Použití počítače Mac pro vývoj na platformě Xamarin vyžaduje následující software/SDK verze. Zkontrolujte verzi operačního systému a postupujte podle pokynů [instalační program Xamarin](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode se můžou nainstalovat (a aktualizovat) na [developer.apple.com](https://developer.apple.com/xcode/download/) nebo prostřednictvím Mac App Store.

### <a name="testing--debugging-on-macos"></a>Testování a ladění v systému macOS

Mobilní aplikace Xamarin je nasadit do fyzického zařízení přes USB pro testování a ladění (Xamarin.Mac aplikace můžete otestovat přímo v počítači vývoje. Apple Watch aplikace nasazená nejprve na spárovaném zařízení iPhone).

[!include[](~/cross-platform/includes/macos-testing.md)]

<a name="windows" />

## <a name="windows-requirements"></a>Požadavky na Windows

Použití Windows počítače pro vývoj na platformě Xamarin vyžaduje následující verze softwaru/SDK.
Zkontrolujte verzi operačního systému (a potvrďte, že nepoužíváte *Express* verzi sady Visual Studio – v takovém případě zvažte aktualizaci na *komunity* edition).
Instalační program sady Visual Studio 2017 zahrnuje možnost automaticky nainstalujte si Xamarin (**vývoj mobilních aplikací pomocí .NET**).

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
>
>- Xamarin pro Visual Studio podporuje všechny sady Visual Studio 2017 (Community, Professional a Enterprise).
>
>- K vývoji aplikací Xamarin.Forms pro univerzální platformu Windows (UPW) vyžaduje Windows 10 pomocí sady Visual Studio 2017.

### <a name="testing--debugging-on-windows"></a>Testování a ladění na Windows

Mobilní aplikace Xamarin je nasadit do fyzického zařízení přes USB pro testování a ladění (iOS, které zařízení musí být připojené k počítači Mac není počítači se systémem Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]

<a name="install" />

## <a name="installation-instructions"></a>Pokyny k instalaci

Nejnovější verzi Xamarinu pro macOS můžete stáhnout z [xamarin.com/download](http://xamarin.com/download). Pro Windows, postupujte [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) pokyny k instalaci.

Úplný seznam všech našich aktuální verze produktu je k dispozici na [aktuální stránky vydaných verzí](http://developer.xamarin.com/releases/current/). Tato stránka také popisuje verze jednotlivých produktů (a odkazy na poznámky k verzi) pro naši beta a alfa kanály.

Konkrétní [instalace](~/cross-platform/get-started/installation/index.md) pokyny pro jednotlivé platformy, jsou k dispozici zde:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

K dispozici je také další informace o [Xamarin.Forms požadavky a podporované platformy](~/xamarin-forms/get-started/installation.md).

## <a name="related-links"></a>Související odkazy

- [Stažení Xamarinu](https://visualstudio.microsoft.com/xamarin/)
- [Aktuální verze](https://developer.xamarin.com/releases/current/)
