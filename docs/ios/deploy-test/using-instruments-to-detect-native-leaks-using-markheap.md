---
title: Profilace – aplikace Xamarin.iOS s nástroji
description: Jak používat nástroje na Xamarin.iOS aplikace na zařízení nebo v simulátoru.
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a5c44531a8f05529b169390f25b2946133411098
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Profilace – aplikace Xamarin.iOS s nástroji

_Jak používat nástroje na Xamarin.iOS aplikace na zařízení nebo v simulátoru._

Xcode **Instruments** je nástroj, který je možné do profilu aplikace Xamarin.iOS na zařízení nebo v simulátoru. Mono používá jeho těsně za běhu modelu kompilace kódu a nástroje není tento druh dat interpretovat dobře, tak může být složité pro práci s výstupem na základě simulátoru aplikace, které používají nástroje.
Kvůli tomuto problému se zaměří Tato příručka o tom, jak používat aplikaci vývojáře k interpretaci Instruments výstupu v tomto dokumentu.

## <a name="requirements"></a>Požadavky

Nástrojů Xcode lze spustit pouze v počítačích Mac.

## <a name="opening-the-instruments-app"></a>Otevření nástrojů aplikace

Vyberte zařízení a spusťte aplikaci nástroje:

1.  Otevřete projekt Xamarin.iOS v sadě Visual Studio for Mac.
2.  Vyberte **ladění | iPhone** konfigurace.
3.  Připojení zařízení s iOS k počítači.
4.  V **spustit** nabídce vyberte možnost **odeslat do zařízení** . Aplikace se teď vytvořené a nahrané do zařízení.
5.  V **nástroje** nabídce vyberte možnost **spuštění nástroje**.


Instruments bude nyní otevřít a zobrazit následující dialogové okno:

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Výběr šablony profilace")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

Kliknutím vyberte **přidělení** šablony. Další šablony jsou platné, ale tento článek popisuje pouze **přidělení** šablonu profilu.

Potom vyberte zařízení a aplikací pomocí nabídky v horní části okna:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Vyberte zařízení a aplikací")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

Zařízení s iOS by měl vybraný v nabídce v horní části okna a aplikace se profilovaným, měla by být vybrána vedle sebe (**MemoryDemo** na snímku obrazovky výše).

Pokud zařízení není uvedené v části v nabídce, podívejte se **konzoly** v sadě Visual Studio pro Mac pro chybové zprávy, které mohou být zobrazeny, když chcete aplikaci nasadit do zařízení. Ujistěte se také, zařízení zřízená pro vývoj prostřednictvím Xcode médií.

Klikněte na tlačítko **zvolte** by se měla objevit tlačítko a na další obrazovce:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Rozhraní profilace")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

Klikněte na tlačítko záznam (červené kolečko v levé horní části) ke spuštění profilování.

Následující snímek obrazovky ukazuje příklad profilace pomocí **Instruments**:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Příklad profilace pomocí nástroje")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>Souhrn

Tento průvodce vám ukázal, jak spustit nástrojů Xcode k monitorování aplikace pro iOS z v sadě Visual Studio for Mac. Pokračujte [Instruments návod](~/ios/deploy-test/walkthrough-apples-instrument.md) příklad toho, jak diagnostikovat chybu paměti pomocí nástroje.

## <a name="related-links"></a>Související odkazy

- [Návod nástroje](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Uvolňování paměti Xamarin.iOS](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
