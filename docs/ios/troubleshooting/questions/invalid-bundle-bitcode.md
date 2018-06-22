---
title: 'Chyba při odesílání na App Store: "Neplatný sady - nesmí být součástí bitcode možnosti zjištění v odesílání"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: cacb9040ddc8582490c68bcfd24e80c4c4679eb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777701"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Chyba při odesílání na App Store: "Neplatný sady - nesmí být součástí bitcode možnosti zjištění v odesílání"

watchOS aplikace a aplikace tvOS _vyžadují_ bitcode, když se odešlou do obchodu s aplikacemi. Při vytváření a odesílání watchOS a tvOS aplikace pomocí Xcode 8.3 nebo starší, může dojít k následující chybě (prostřednictvím e-mailových oznámení) při pokusu o nahrání do obchodu s aplikacemi:

>Neplatná sada - aplikaci nelze zpracovat, protože možnosti nesmí být součástí bitcode zjištění v odesílání. Je pravděpodobné, že nejsou sestavení aplikace pomocí nástrojů v Xcode.

Řešení tohoto problému je k vytváření aplikací s Xcode 9 a nejnovější verzi Xamarin.iOS.
