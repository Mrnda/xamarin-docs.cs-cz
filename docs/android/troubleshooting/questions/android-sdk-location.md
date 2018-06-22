---
title: Kde můžete nastavit Moje umístění sady SDK pro Android?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 11/16/2017
ms.openlocfilehash: e7af6635aeec72a9a0e0c01eeb6eb0a77d2a1d7b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764005"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Kde můžete nastavit Moje umístění sady SDK pro Android?

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio, přejděte na **nástroje > Možnosti > Xamarin > Nastavení Androidu** k zobrazení a nastavte umístění, sady SDK pro Android:

[![Karta umístění příklad v předvolbách](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Výchozí umístění pro každou z cest je následujícím způsobem:

- Java Development Kit umístění: 

    **C:\\soubory programu\\Java\\jdk1.8.0_131**

- Umístění sady SDK pro Android: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK umístění: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

Všimněte si, že číslo verze na NDK se může lišit. Například místo z **android ndk-r13b**, může to být starší verze, jako **android ndk-r10e**.

Pokud chcete nastavit umístění sady SDK pro Android, zadejte úplnou cestu do adresáře sady SDK pro Android **Android SDK umístění** pole. Přejděte do umístění sady SDK pro Android v Průzkumníku souborů, zkopírujte cestu z panelu Adresa a vložte tuto cestu do **Android SDK umístění** pole.
Například, pokud je vaše umístění sady SDK pro Android na **C:\\uživatelé\\uživatelské jméno\\data aplikací\\místní\\Android\\Sdk**, odstraňte původní cestu v  **Android SDK umístění** pole, vložte v této cestě a klikněte na tlačítko **OK**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac, přejděte na **Předvolby > Projekty > SDK umístění > Android**. V **Android** klikněte na tlačítko **umístění** k zobrazení a nastavte umístění SDK karty:

[![Karta umístění příklad v předvolbách](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Výchozí umístění pro každou z cest je následujícím způsobem:

- Umístění sady SDK pro Android: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK umístění: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Umístění Java SDK (JDK): 

    **/usr**

Všimněte si, že číslo verze na NDK se může lišit. Například místo z **android ndk-r14b**, může to být starší verze, jako **android ndk-r10e**.

Pokud chcete nastavit umístění sady SDK pro Android, zadejte úplnou cestu do adresáře sady SDK pro Android **Android SDK umístění** pole. Můžete vybrat složku sady SDK pro Android v nástroji hledání, stiskněte klávesu **CTRL +&#8984;+ I** zobrazíte informace o složce klikněte a přetáhněte cestu napravo od **kde:**, zkopírujte a vložte jej do **sady SDK pro Android Umístění** pole **umístění** kartě. Například, pokud je vaše umístění sady SDK pro Android na **~/Library/Developer/Android/Sdk**, odstraňte původní cestu v **Android SDK umístění** pole, vložte v této cestě a klikněte na tlačítko **OK**.

-----
