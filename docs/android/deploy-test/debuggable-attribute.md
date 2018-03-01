---
title: Debuggable atribut
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>Debuggable atribut

<a name="Overview" />


Chcete-li ladění možné, podporuje Android jazyce Java ladění přenosu protokolu (JDWP). Toto je technologie, která umožňuje nástroje, jako je ADB ke komunikaci s JVM. Při JDWP je důležité při vývoji, by mělo být zakázáno před aplikace je publikován.

JDWP může být hodnota `android:debuggable` atribut v aplikaci pro Android. Xamarin.Android nabízí tyto způsoby nastavte tento atribut:

1.  Vytvoření `AndroidManifext.xml` souborů a nastavení `android:debuggable` atribut existuje.
1.  Včetně `ApplicationAttribute` v `.CS` souboru takto: `[assembly: Application(Debuggable=false)]` .


Pokud `AndroidManifest.xml` a `ApplicationAttribute` jsou k dispozici, obsah `AndroidManifest.xml` mají přednost před co je zadána `ApplicationAttribute`.

Pokud ani `AndroidManifest.xml` a `ApplicationAttribute`, pak výchozí hodnota `android:debuggable` atribut závisí na tom, jestli jsou generovány symboly ladění. Pokud jsou k dispozici symboly ladění, pak nastaví Xamarin.Android `android:debuggable` atribut `true`.

Všimněte si, že hodnota `android:debuggable` atribut nemusí záviset na konfiguraci sestavení. Je možné pro verzi sestavení tak, aby měl `android:debuggable` nastaven atribut na hodnotu true.


## <a name="related-links"></a>Související odkazy

- [Debuggable aplikace na trhu Android](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
