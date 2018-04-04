---
title: Kde jsou uloženy součásti na můj počítač?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f0dad6e6219d373eaa9f8410aea7d96c81eceb6b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Kde jsou uloženy součásti na můj počítač?

Vždy, když instalujete součást Xamarin do projektu aplikace, získá umístěny na dvou místech:

1. Ve složce součásti na kořenové úrovni složky řešení. Pokud odeberete součásti z všechny projekty v řešení, bude získat odebrána z této složky také.

2. Kopie se také ukládají v následujících umístěních:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Proto úplně odebrat součást ze systému, odstraňte jej z vašich projektů řešení a z výše uvedené složky mezipaměti.
