---
title: "Uživatelské rozhraní v iOS"
description: "Zahrnuje práce s iOS uživatelské rozhraní v aplikaci pro Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2B3E45FA-C30F-D708-0E8F-3EE02BD1A867
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 831ddfff7e05c391472b280095564f90528369ff
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface-in-ios"></a>Uživatelské rozhraní v iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[Vzhled rozhraní API](introduction-to-the-appearance-api.md)

iOS umožňuje mnoho visual atributy ovládacích prvků uživatelského rozhraní jako motivu pomocí rozhraní API UIAppearance.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Vytváření objektů uživatelského rozhraní](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple skupin souvisejících částí funkce do "rozhraní", které rovnat Xamarin.iOS obory názvů. `UIKit` je obor názvů, který obsahuje všechny uživatelské rozhraní ovládací prvky pro iOS.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Možnosti rozložení](~/ios/user-interface/ios-ui/layout-options.md)

Existují dva různé mechanismy pro řízení rozložení, když je zobrazení po změně velikosti nebo otáčet: Automatická změna velikosti a automatické rozložení.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Poskytnutí zpětné vazby hmatová](~/ios/user-interface/ios-ui/haptic-feedback.md)

Tento článek se zabývá nové typy hmatová zpětnou vazbu k dispozici v iOS 10 a jejich implementaci v Xamarin.iOS.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Práce z vlákna uživatelského rozhraní](~/ios/user-interface/ios-ui/ui-thread.md)

Váš kód pouze měli přístup z více vláken změny ovládací prvky rozhraní z hlavní uživatele (nebo uživatelského rozhraní). Všechny aktualizace uživatelského rozhraní, ke kterým došlo v jiném podprocesu (například zpětného volání nebo na pozadí vlákno) nemusí získat vykresluje na obrazovku, nebo může způsobit i havárie.




