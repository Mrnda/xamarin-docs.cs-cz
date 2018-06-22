---
title: Požadavky na platformě Xamarin.Forms
description: Vývoj platformy a požadavky na systém pro Xamarin.Forms.
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: 75e6d25f95a0a3f18c83fe73f67ad4a7797f0924
ms.sourcegitcommit: c024f29ff730ae20c15e99bfe0268a0e1c9d41e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/23/2018
ms.locfileid: "34470327"
---
# <a name="xamarinforms-requirements"></a>Požadavky na platformě Xamarin.Forms

_Vývoj platformy a požadavky na systém pro Xamarin.Forms._

Odkazovat [instalace](~/cross-platform/get-started/installation/index.md) najdete v článku Přehled instalace a postupy, které se používají různé platformy.

## <a name="target-platforms"></a>Cílové platformy

Xamarin.Forms aplikace může být zapsán pro následující operační systémy:

- iOS 8 nebo novější
- Android 4.0.3 (API 15) nebo vyšší ([podrobnosti](#android))
- Windows 10 univerzální platformu Windows ([podrobnosti](#windows10))

Předpokládá se, že vývojáři mají znalost [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) a [sdílených projektů](~/cross-platform/app-fundamentals/shared-projects.md).

### <a name="additional-platform-support"></a>Podpora dalších platformy

Stav těchto platforem je k dispozici na [Xamarin.Forms Githubu](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support):

- Samsung Tizen
- macOS
- GTK #
- WPF

### <a name="platforms-from-earlier-versions"></a>Platformy z dřívějších verzí

Při použití Xamarin.Forms 3.0 nejsou podporovány tyto platformy:

- *Windows 8.1 / Windows Phone 8.1 WinRT*
- *Windows Phone 8 Silverlight*

### <a name="android"></a>Android

Měli byste mít nejnovější platformu Android SDK – nástroje a rozhraní API systému Android nainstalována. Můžete aktualizovat na nejnovější verzi pomocí [Android SDK Manager](~/android/get-started/installation/android-sdk.md).

Kromě toho je verze cílové/kompilace pro Android projekty **musí** nastavit na *použijte nejnovější nainstalované platformy*. Ale může být minimální verze nastaven na rozhraní API 15, můžete dál zajišťovat podporu zařízení, která používají Android 4.0.3 a novějších. Tyto hodnoty jsou nastavené **možnosti projektu**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Možnosti projektu > aplikace > Vlastnosti aplikace**

![](installation-images/options-android-vs-sml.png "Možnosti Android sestavení v sadě Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Sestavení > Obecné**

![](installation-images/options-general-sml.png "Sestavení > Obecné")

**Sestavení > aplikace pro Android**

![](installation-images/options-android-sml.png "Sestavení > aplikace pro Android")

-----

## <a name="development-system-requirements"></a>Vývoj pro požadavky na systém

Xamarin.Forms aplikace mohou být vytvořeny v systému macOS a Windows. Windows Server a Visual Studio jsou však vyžadováno k vytvoření verzí aplikace Windows.

## <a name="mac-system-requirements"></a>Požadavky na systém Mac

Visual Studio pro Mac můžete použít k vývoji aplikací Xamarin.Forms na OS X El Capitan (10.11) nebo novější. K vývoji aplikací pro iOS, doporučujeme mít alespoň iOS 10 SDK a 8 Xcode nainstalována.

> [!NOTE]
>  V systému macOS nemůže být vyvinutá aplikací pro Windows.

## <a name="windows-system-requirements"></a>Požadavky na systém Windows

Xamarin.Forms aplikace pro iOS a Android se dají vytvářet v žádné instalaci systému Windows, která podporuje vývoj na platformě Xamarin. To vyžaduje Visual Studio 2017 nebo novější běžící na systému Windows 7 nebo vyšší. Síťově připojeného počítače Mac se vyžaduje pro vývoj pro iOS.

<a name="windows10" />

### <a name="universal-windows-platform-uwp"></a>Univerzální platforma Windows (UPW)

Vývoj aplikací Xamarin.Forms pro UPW vyžaduje:

- Windows 10 (doporučené aktualizace Creators patří.)

- Visual Studio 2017

- [Windows 10 SDK](https://dev.windows.com/downloads/windows-10-sdk)

Projekty UWP jsou součástí řešení Xamarin.Forms vytvořené v aplikaci Visual Studio 2017, ale není řešení vytvořená v sadě Visual Studio for Mac.
Můžete [přidat univerzální platformu Windows (UWP) aplikace](~/xamarin-forms/platform/windows/installation/index.md) do existujícího řešení Xamarin.Forms kdykoli.
