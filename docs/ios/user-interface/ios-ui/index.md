---
title: "Uživatelské rozhraní v iOS"
description: "Zahrnuje práce s iOS uživatelské rozhraní v aplikaci pro Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: f456b54180d50cfc4b6b98ed8f3d4118c8397b37
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="user-interface-in-ios"></a>Uživatelské rozhraní v iOS

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




