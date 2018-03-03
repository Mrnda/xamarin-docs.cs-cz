---
title: "Požadavky na platformě Xamarin.Forms"
description: "Vývoj platformy a požadavky na systém pro Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: eecaf6a5-567c-49b2-ac83-2a195596c5bf
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2017
ms.openlocfilehash: 2eaf4c6180b51a827d8182d87ee2db0fd1726c8d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-requirements"></a>Požadavky na platformě Xamarin.Forms

_Vývoj platformy a požadavky na systém pro Xamarin.Forms._

Odkazovat [instalace](~/cross-platform/get-started/installation/index.md) najdete v článku Přehled instalace a postupy, které se používají různé platformy.

## <a name="target-platforms"></a>Cílové platformy

Xamarin.Forms aplikace může být zapsán pro následující operační systémy:

-  iOS 8 nebo novější
-  Android 4.0.3 (API 15) nebo vyšší ([podrobnosti](#android))
-  Windows 10 univerzální platformu Windows ([podrobnosti](#windows10))
-  Windows 8.1 nebo Windows Phone 8.1 WinRT ([podrobnosti](#windows))
-  *Windows Phone 8 Silverlight (zastaralé)*

Předpokládá se, že vývojáři mají znalost [přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md) a [sdílených projektů](~/cross-platform/app-fundamentals/shared-projects.md).

<a name="android" />

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


<a name="windows10" />

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Pokud řešení je vytvořen v systému macOS, nebyly přidány projekty Windows 10 UWP. Pokyny o tom, jak přidat tyto projekty do existujícího řešení najdete v tématu [přidávání univerzální platformu Windows (UWP) aplikace](~/xamarin-forms/platform/windows/installation/universal.md).


<a name="windows" />

### <a name="windows-81--windows-phone-81-winrt"></a>Windows 8.1 / Windows Phone 8.1 WinRT

Windows 8.1 / Windows Phone 8.1 WinRT projekty nebyly přidány, pokud řešení je vytvořen v systému macOS. Pokyny o tom, jak přidat tyto projekty do existujícího řešení najdete v tématu [přidání aplikace na Windows Phone](~/xamarin-forms/platform/windows/installation/phone.md) a [přidání aplikace pro Windows](~/xamarin-forms/platform/windows/installation/tablet.md).


## <a name="development-system-requirements"></a>Vývoj pro požadavky na systém

Xamarin.Forms aplikace mohou být vytvořeny v systému macOS a Windows. Windows Server a Visual Studio jsou však vyžadováno k vytvoření verzí aplikace Windows.

## <a name="mac-system-requirements"></a>Požadavky na systém Mac

Visual Studio pro Mac můžete použít k vývoji aplikací Xamarin.Forms na OS X El Capitan (10.11) nebo novější. K vývoji aplikací pro iOS, doporučujeme mít alespoň iOS 10 SDK a 8 Xcode nainstalována.

> [!NOTE]
>  V systému macOS nemůže být vyvinutá aplikací pro Windows.

## <a name="windows-system-requirements"></a>Požadavky na systém Windows

Xamarin.Forms aplikace pro iOS a Android se dají vytvářet v žádné instalaci systému Windows, která podporuje vývoj na platformě Xamarin. To vyžaduje Visual Studio 2013 Update 2 nebo novější běžící na systému Windows 7 nebo vyšší. Síťově připojeného počítače Mac se vyžaduje pro vývoj pro iOS.

Existují další požadavky pro následující typy aplikací pro Windows:

### <a name="universal-windows-platform-uwp"></a>Univerzální platforma Windows (UPW)

Vývoj aplikací Xamarin.Forms pro UPW vyžaduje:

* Windows 10

* Visual Studio 2015 nebo novější

* [Nástroje pro vývojáře Universal Windows](https://dev.windows.com/downloads/windows-10-sdk)

Projekty UWP jsou součástí řešení Xamarin.Forms vytvořené v sadě Visual Studio 2015 a Visual Studio 2017.
Můžete také [přidat univerzální platformu Windows (UWP) aplikace](~/xamarin-forms/platform/windows/installation/universal.md) do existujícího řešení Xamarin.Forms.



### <a name="windows-81-and-windows-phone-81-winrt"></a>Windows 8.1 a Windows Phone 8.1 WinRT

Vývoj Xamarin.Forms aplikací pro Windows 8.1 a Windows Phone 8.1 WinRT vyžaduje:

* Windows 8.1

* Visual Studio 2013 Update 2 nebo novější
