---
title: Debuggable atribut
ms.topic: article
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: fe516a9780b8b1cdc478a49fe3b6963097649a80
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2018
---
# <a name="debuggable-attribute"></a>Debuggable atribut



Chcete-li ladění možné, podporuje Android jazyce Java ladění přenosu protokolu (JDWP). Toto je technologie, která umožňuje nástroje, jako je ADB ke komunikaci s JVM. Při JDWP je důležité při vývoji, by mělo být zakázáno před aplikace je publikován.

JDWP může být hodnota `android:debuggable` atribut v aplikaci pro Android. Xamarin.Android nabízí tyto způsoby nastavte tento atribut:

1.  Vytvoření `AndroidManifext.xml` souborů a nastavení `android:debuggable` atribut existuje.
2.  Včetně `ApplicationAttribute` v `.CS` souboru takto: `[assembly: Application(Debuggable=false)]` .


Pokud `AndroidManifest.xml` a `ApplicationAttribute` jsou k dispozici, obsah `AndroidManifest.xml` mají přednost před co je zadána `ApplicationAttribute`.

Pokud `AndroidManifest.xml` ani `ApplicationAttribute` nachází, je výchozí hodnota `android:debuggable` atribut závisí na tom, jestli jsou generovány symboly ladění. Pokud jsou k dispozici symboly ladění, pak nastaví Xamarin.Android `android:debuggable` atribut `true`.

Všimněte si, že hodnota `android:debuggable` atribut nemusí záviset na konfiguraci sestavení. Je možné pro verzi sestavení tak, aby měl `android:debuggable` nastaven atribut na hodnotu true.


## <a name="related-links"></a>Související odkazy

- [Debuggable aplikace na trhu Android](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
