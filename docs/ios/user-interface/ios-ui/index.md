---
title: Uživatelská rozhraní v iOS
description: Tento dokument obsahuje odkazy na příručky, které popisují, jak sestavit uživatelské rozhraní aplikace Xamarin.iOS. Propojené postupy zahrnují rozhraní API vzhled vytváření objektů uživatelského rozhraní, možnosti rozložení a další.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: a51d3f57106a282ed72b45dedf356739244e247f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790364"
---
# <a name="user-interfaces-in-ios"></a>Uživatelská rozhraní v iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Rozhraní API pro vzhled](introduction-to-the-appearance-api.md)

iOS umožňuje mnoho visual atributy ovládacích prvků uživatelského rozhraní jako motivu pomocí rozhraní API UIAppearance.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Vytváření objektů uživatelského rozhraní](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple skupin souvisejících částí funkce do "rozhraní", které rovnat Xamarin.iOS obory názvů. `UIKit` je obor názvů, který obsahuje všechny uživatelské rozhraní ovládací prvky pro iOS.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Možnosti rozložení](~/ios/user-interface/ios-ui/layout-options.md)

Existují dva různé mechanismy pro řízení rozložení, když je zobrazení po změně velikosti nebo otáčet: Automatická změna velikosti a automatické rozložení.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Poskytování hmatové zpětné vazby](~/ios/user-interface/ios-ui/haptic-feedback.md)

Tento článek se zabývá nové typy hmatová zpětnou vazbu k dispozici v iOS 10 a jejich implementaci v Xamarin.iOS.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Práce s vláknem uživatelského rozhraní](~/ios/user-interface/ios-ui/ui-thread.md)

Váš kód pouze měli přístup z více vláken změny ovládací prvky rozhraní z hlavní uživatele (nebo uživatelského rozhraní). Všechny aktualizace uživatelského rozhraní, ke kterým došlo v jiném podprocesu (například zpětného volání nebo na pozadí vlákno) nemusí získat vykresluje na obrazovku, nebo může způsobit i havárie.




