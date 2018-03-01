---
title: "Kde jsou uloženy součásti na můj počítač?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 9549363d7aeb5ff7a5cfa79eb38443aaa80b7019
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Kde jsou uloženy součásti na můj počítač?

Vždy, když instalujete součást Xamarin do projektu aplikace, získá umístěny na dvou místech:

1. Ve složce součásti na kořenové úrovni složky řešení. Pokud odeberete součásti z všechny projekty v řešení, bude získat odebrána z této složky také.

2. Kopie se také ukládají v následujících umístěních:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Proto úplně odebrat součást ze systému, odstraňte jej z vašich projektů řešení a z výše uvedené složky mezipaměti.
