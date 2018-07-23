---
title: Profilace aplikací Xamarin.iOS pomocí Instruments
description: Tento dokument popisuje způsob použití společnosti Apple Instruments aplikace do profilu aplikace pro Xamarin.iOS nainstalovaný na zařízení nebo simulátor.
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9b6168eba91a87af88891b9e07e3dd395301cc48
ms.sourcegitcommit: 021027b78cb2f8061b03a7c6ae59367ded32d587
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2018
ms.locfileid: "39182205"
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Profilace aplikací Xamarin.iOS pomocí Instruments

Xcode **nástrojů** je nástroj, který je možné do profilu aplikace Xamarin.iOS na zařízení nebo simulátoru. Mono používá jeho Just-in-Time model pro kompilaci kódu a nástroje není tento druh dat interpretovat dobře, tak může být obtížné pro práci s výstup ze simulátoru aplikacím, které používají nástroje.
Kvůli tomuto problému tato příručka se soustředí na tom, jak používat aplikace pro vývojáře k interpretaci výstupu nástroje v tomto dokumentu.

## <a name="requirements"></a>Požadavky

Instrumentů Xcode se spustí jenom v počítačích Mac.

## <a name="opening-the-instruments-app"></a>Otevřete aplikaci nástroje

Vyberte zařízení a spuštění aplikace nástrojů:

1. Otevřete projekt Xamarin.iOS v sadě Visual Studio pro Mac.
2. Vyberte **ladění | iPhone** konfigurace.
3. Připojte zařízení s Iosem k počítači.
4. V **spustit** nabídce vyberte možnost **odeslat do zařízení** . Aplikace se teď vytvořené a do tohoto zařízení nahrát.
5. V **nástroje** nabídce vyberte možnost **spuštění nástrojů**.


Nástrojů se nyní otevřít a zobrazit následující dialogové okno:

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Zvolit šablonu, profilace")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

Kliknutím vyberte **přidělení** šablony. Další šablony jsou platné, ale tento článek popisuje pouze to, **přidělení** šablonu profilu.

Potom vyberte zařízení a aplikací v nabídce v horní části okna:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Vyberte zařízení a aplikací")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

V nabídce v horní části okna by měla vybrané zařízení s Iosem a aplikací určených k profilaci, by měla být zaškrtnutá vedle něj (**MemoryDemo** ve výše uvedeném snímku obrazovky).

Pokud zařízení není uvedený v seznamu v nabídce, zkontrolujte, **konzoly** v sadě Visual Studio for Mac pro chybové zprávy, které se může zobrazit při nasazení aplikace do zařízení. Také se ujistěte, že zařízení byla zřízena pro vývoj až po médií Xcode.

Klikněte na tlačítko **zvolit** by se měla zobrazit tlačítko a na další obrazovce:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Rozhraní profilování")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

Klikněte na tlačítko záznam (červený kruh vlevo nahoře) spuštění profilování.

Následující snímek obrazovky ukazuje příklad profilace pomocí **nástrojů**:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Příklad použití nástrojů pro profilaci")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>Souhrn

Tento průvodce vám ukázal, jak spustit nástroje Xcode pro monitorování aplikací pro iOS ze sady Visual Studio pro Mac. Pokračujte [nástrojů návod](~/ios/deploy-test/walkthrough-apples-instrument.md) příklad toho, jak pro diagnostiku problému paměti pomocí nástroje.

## <a name="related-links"></a>Související odkazy

- [Názorný postup nástroje](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Uvolňování paměti Xamarin.iOS (příspěvek na blogu)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
