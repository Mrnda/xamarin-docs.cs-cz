---
title: Kde jsou uloženy součásti na můj počítač?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.openlocfilehash: a53045a6179a26b30d824976d11fd2769a84811e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919394"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Kde jsou uloženy součásti na můj počítač?

Vždy, když instalujete součást Xamarin do projektu aplikace, získá umístěny na dvou místech:

1. Ve složce součásti na kořenové úrovni složky řešení. Pokud odeberete součásti z všechny projekty v řešení, bude získat odebrána z této složky také.

2. Kopie se také ukládají v následujících umístěních:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Proto úplně odebrat součást ze systému, odstraňte jej z vašich projektů řešení a z výše uvedené složky mezipaměti.
