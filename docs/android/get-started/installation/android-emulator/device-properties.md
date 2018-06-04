---
title: Úprava vlastností virtuálního zařízení se systémem Android
description: Tento článek vysvětluje, jak upravit vlastnosti profilu virtuálního zařízení se systémem Android pomocí Správce zařízení Android.
ms.prod: xamarin
ms.assetid: 3E33C136-8042-4184-A40C-3200D8CD99CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 75ac85c67825e5db1b663d00f10eee6d093bfc1f
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733916"
---
# <a name="editing-android-virtual-device-properties"></a>Úprava vlastností virtuálního zařízení se systémem Android

_Tento článek vysvětluje, jak upravit vlastnosti profilu virtuálního zařízení se systémem Android pomocí Správce zařízení Android._


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Správce zařízení Android** podporuje úpravy vlastností profilu, jednotlivé virtuální zařízení s Androidem. **Nové zařízení** a **zařízení upravit** obrazovky seznam vlastností virtuálního zařízení z prvního sloupce s odpovídajícími hodnotami každé vlastnosti v druhém sloupci (jak je vidět v tomto příkladu): 

[![Příklad obrazovky nové zařízení](device-properties-images/win/01-new-device-editor-sml.png)](device-properties-images/win/01-new-device-editor.png#lightbox)

Když vyberete vlastnost, podrobný popis této vlastnosti se zobrazí na pravé straně. Můžete upravit *vlastnosti profilu hardwaru* a *AVD vlastnosti*. Vlastnosti profilu hardwaru (například `hw.ramSize` a `hw.accelerometer`) popisují fyzické charakteristiky emulované zařízení. Tyto vlastnosti obsahovat velikost obrazovky, množství dostupné paměti RAM, zda je k dispozici zrychlení. Vlastnosti AVD zadejte operaci AVD při spuštění. Můžete například nakonfigurovat AVD vlastností k určení, jak AVD používá vývojovém počítači grafické karty pro vykreslování.

Vlastnosti můžete změnit pomocí následujících pokynů:

-   Chcete-li změnit vlastnost typu boolean, klikněte na tlačítko zaškrtnutí vpravo od vlastnost typu boolean:

    ![Změna vlastnost typu boolean](device-properties-images/win/02-boolean-value.png)

-   Chcete-li změnit *výčtu* (výčtové) vlastnosti, klikněte na šipku dolů vpravo vlastnosti a vyberte novou hodnotu.

    ![Změna vlastnosti výčtu](device-properties-images/win/04-enum-value.png)

-   Chcete-li změnit vlastnost řetězec nebo celé číslo, klikněte dvakrát na aktuální nastavení řetězec nebo celé číslo ve sloupci Hodnota a zadejte novou hodnotu.

    ![Změna ve vlastnosti integer.](device-properties-images/win/03-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Správce zařízení Android** podporuje úpravy vlastností profilu, jednotlivé virtuální zařízení s Androidem. **Nové zařízení** a **zařízení upravit** obrazovky seznam vlastností virtuálního zařízení z prvního sloupce s odpovídajícími hodnotami každé vlastnosti v druhém sloupci (jak je vidět v tomto příkladu): 

[![Příklad obrazovky nové zařízení](device-properties-images/mac/01-new-device-editor-sml.png)](device-properties-images/mac/01-new-device-editor.png#lightbox)

Když vyberete vlastnost, podrobný popis této vlastnosti se zobrazí na pravé straně. Můžete upravit *vlastnosti profilu hardwaru* a *AVD vlastnosti*. Vlastnosti profilu hardwaru (například `hw.ramSize` a `hw.accelerometer`) popisují fyzické charakteristiky emulované zařízení. Tyto vlastnosti obsahovat velikost obrazovky, množství dostupné paměti RAM, zda je k dispozici zrychlení. Vlastnosti AVD zadejte operaci AVD při spuštění. Můžete například nakonfigurovat AVD vlastností k určení, jak AVD používá vývojovém počítači grafické karty pro vykreslování.

Vlastnosti můžete změnit pomocí následujících pokynů:

-   Chcete-li změnit vlastnost typu boolean, klikněte na tlačítko zaškrtnutí vpravo od vlastnost typu boolean:

    ![Změna vlastnost typu boolean](device-properties-images/mac/02-boolean-value.png)

-   Chcete-li změnit *výčtu* (výčtové) vlastnosti, klikněte na rozevírací nabídku napravo od vlastnosti a vyberte novou hodnotu.

    ![Změna vlastnosti výčtu](device-properties-images/mac/04-enum-value.png)

-   Chcete-li změnit vlastnost řetězec nebo celé číslo, klikněte dvakrát na aktuální nastavení řetězec nebo celé číslo ve sloupci Hodnota a zadejte novou hodnotu.

    ![Změna ve vlastnosti integer.](device-properties-images/mac/03-integer-value.png)

-----

Poskytuje podrobné vysvětlení vlastností uvedených v následující tabulce **nové zařízení** a **zařízení Editor** obrazovky:

[!include[](~/android/includes/emulator-properties.md)]

Další informace o těchto vlastnostech najdete v tématu [vlastnosti profilu hardwaru](https://developer.android.com/studio/run/managing-avds.html#hpproperties).

