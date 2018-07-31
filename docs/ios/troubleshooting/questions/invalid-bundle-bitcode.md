---
title: 'Chyba při odesílání do App Store: "Neplatná sada – možnosti nesmí být vložen do bitcode se zjistily"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 393c1ed81c68d21b610781dfe09de97969e031d1
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350921"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Chyba při odesílání do App Store: "Neplatná sada – možnosti nesmí být vložen do bitcode se zjistily"

watchOS a tvOS aplikací _vyžadují_ bitcode odeslání do App Store. Při vytváření a odesílání watchOS a tvOS aplikace s využitím Xcode 8.3 nebo starší, může dojít k následující chybě (prostřednictvím e-mailové oznámení) při pokusu o nahrání do App Store:

>Neplatná sada – aplikace nelze zpracovat, protože se zjistily možnosti, které mají být vloženy bitcode není povoleno. Je pravděpodobné, že se vytvoření aplikace se sadou nástrojů k dispozici v Xcode.

Řešení tohoto problému je k vytváření aplikací s Xcode 9 a nejnovější verzí Xamarin.iOS.
