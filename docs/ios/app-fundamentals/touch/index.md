---
title: Zpracování Touch v aplikace Xamarin.iOS
description: Tento dokument obsahuje odkazy na příručky, které popisují, jak pracovat s dotykového ovládání, více touch, gesta a 3D Touch v aplikaci pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: eb8dce8b13345c13a6f95ae7784bd135e7d1f1f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784159"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Zpracování Touch v aplikace Xamarin.iOS

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
