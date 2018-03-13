---
title: "Dotykové ovládání"
description: "Dotykové obrazovky na množství dnešních zařízení umožňují uživatelům rychle a efektivně využívat zařízení přirozené a intuitivní způsobem. Tato interakce se neomezuje jenom na jednoduchý touch detekce – je možné pomocí gest také. Například gesto roztahováním přiblížení, je velmi běžné příklad tohoto – roztáhnout součástí obrazovky se dvěma prsty, které uživatel může zvětšit nebo zmenšit. Tato příručka prověří touch a gest v iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: b0e1cf8b1cb18982fe319fef7c524aeb70be4a9b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="touch"></a>Dotykové ovládání

_Dotykové obrazovky na množství dnešních zařízení umožňují uživatelům rychle a efektivně využívat zařízení přirozené a intuitivní způsobem. Tato interakce se neomezuje jenom na jednoduchý touch detekce – je možné pomocí gest také. Například gesto roztahováním přiblížení, je velmi běžné příklad tohoto – roztáhnout součástí obrazovky se dvěma prsty, které uživatel může zvětšit nebo zmenšit. Tato příručka prověří touch a gest v iOS._


Stejně jako jiné mobilní platformy iOS má několik způsobů, jak zpracovat dotykového ovládání. Může podporovat více touch – mnoha bodů obraťte se na obrazovce – a komplexní gesta. Tento průvodce uvádí některé koncepty, a také zvláštnosti implementace touch a gest v systému iOS.

zapouzdří data touch v iOS `UITouch` třídy, která je k dispozici aplikací pomocí řady `UIResponder` metody. Aplikace můžete přepsat tyto metody v měly podtřídy `UIView` a `UIViewController`, které dědí `UIResponder`.

Kromě zaznamenání dat dotykového ovládání, iOS poskytuje prostředky pro interpretace vzorce úpravy do gesta. Tyto nástroje pro rozpoznávání gesto zase umožňuje interpretovat příkazy specifické pro aplikace, jako je například otočení bitové kopie nebo zapnout stránky. iOS poskytuje bohaté kolekci třídy pro zpracování běžné gesta s minimální přidaného kódu.

Volba mezi úpravy a nástroje pro rozpoznávání gesto může být matoucí jeden. Tento průvodce doporučuje, aby obecně by měl dává gesto rozpoznávání. Nástroje pro rozpoznávání gesto jsou implementované jako diskrétní třídy, které poskytují větší oddělení otázky a lepší zapouzdření. Díky tomu přehledné sdílet logiku mezi různá zobrazení, minimalizovat dobu kód napsaný.

Existují však situace když potřebujete použít nízké úrovně touch zpracování a i sledovat více prsty, například vytvoření programu finger-paint.

## <a name="sections"></a>Oddíly

-  [Dotykové ovládání v iOSu](touch-in-ios.md)
-  [Návod: Použití Touch v iOS](ios-touch-walkthrough.md)
-  [Sledování více dotyků](touch-tracking.md)

Tato příručka slouží jako úvod do Touch v iOS. Další informace o použití 3D Touch a hmatová zpětné vazby v iOS, které byly zavedeny v iOS 9 a 10 v uvedeném pořadí naleznete konkrétní příručky níže:

* [3D Touch](~/ios/platform/3d-touch.md)
* [Poskytování hmatové zpětné vazby](~/ios/user-interface/ios-ui/haptic-feedback.md)



## <a name="related-links"></a>Související odkazy

- [iOS Touch Start (ukázka)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch konečné (ukázka)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint (ukázka)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
