---
title: Instalace Xamarin.iOS v systému Windows
description: Tento článek popisuje, jak nastavit počítače s Windows a Mac sestavení hostitele pro vývoj na platformě Xamarin.iOS.
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: e6f50a48481be3ca5c64332f5a182e44715740c0
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/23/2018
---
# <a name="installing-xamarinios-on-windows"></a>Instalace Xamarin.iOS v systému Windows

_Tento článek popisuje, jak nastavit počítače s Windows a Mac sestavení hostitele pro vývoj na platformě Xamarin.iOS._

## <a name="overview"></a>Přehled

K vytvoření aplikace Xamarin.iOS s Visual Studio 2017 v systému Windows, budete potřebovat:
 
-  Počítače s Windows s Visual Studio 2017 nainstalována. To může být fyzický nebo virtuální počítač.
    - [Požadavky na systém Windows](~/cross-platform/get-started/requirements.md#windows-requirements)
    
-  Síťově přístupné Mac nastavit pomocí nástroje pro sestavení a Xamarin.iOS společnosti Apple. Visual Studio 2017 přistupuje k tomuto počítači přes síťové připojení při použití nástroje pro sestavení společnosti Apple, které jsou potřebné pro kompilaci nativní aplikace pro iOS aplikace. 
    - [Požadavky na systém Mac](~/cross-platform/get-started/requirements.md#macos-requirements)

## <a name="setup"></a>Instalace

Pro jejich nastavení pro vývoj na platformě Xamarin.iOS v Visual Studio 2017, postupujte takto:

1. Nastavení systému Windows (instalace Visual Studio 2017)

    Xamarin.iOS pracuje s edice Visual Studio 2017 Community, Professional a Enterprise, v samostatné nebo virtuální počítač.
    
    - [Nainstalovat Visual Studio 2017](~/cross-platform/get-started/installation/windows.md).

2. Nastavení Mac (instalaci Xcode a Visual Studio pro Mac)

    Pro vytváření, ladění a podepsání aplikace iOS pro distribuci, Visual Studio 2017 musí mít přístup k síti na Mac sestavení hostitele nakonfigurovaný s obou Apple developer tools (Xcode) a Xamarin.iOS.

    - [Stáhněte a nainstalujte Xcode z Mac App Storu](https://itunes.apple.com/us/app/xcode/id497799835?mt=12). 
    - [Instalaci sady Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/installation), který se nainstaluje také Xamarin.iOS.

    > [!NOTE] 
    > Pokud si přejete není pro instalaci sady Visual Studio pro Mac, počínaje [Visual Studio 2017 verze 15,6 operací](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 mohou automaticky konfigurovat hostitele sestavení Mac k softwaru, které jsou potřebné k vytvoření aplikace Xamarin.iOS . Další informace najdete v tématu [Mac automatické zřizování](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning).

3. Pár na Mac (připojit k počítači Mac Visual Studio 2017)

    Pro Visual Studio 2017 při použití nástroje pro sestavení iOS na Mac dva počítače se musí připojit přes síť.

    - [Číst dvojici Mac průvodci](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="summary"></a>Souhrn

Tento článek popisuje postup nastavení počítače s Windows a jeho přidružené sestavení hostitele Mac pro vývoj na platformě Xamarin.iOS.

## <a name="next-steps"></a>Další kroky

- [Úvod k Xamarin.iOSu pro Visual Studio](introduction-to-xamarin-ios-for-visual-studio.md)
- [Konfigurace Visual Studio 2017](config-options.md)
- [Zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md)
