---
title: Kde jsou komponenty uložené na mém počítači?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350716"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Kde jsou komponenty uložené na mém počítači?

Pokaždé, když se do projektu aplikace nainstalujete komponenty Xamarin, získá umístěny na dvou místech:

1. Ve složce součásti na kořenové úrovni složku řešení. Pokud součást odebrat ze všech projektů v řešení, odebere získat z této složky také.

2. V následujících umístěních je také uložena kopie:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Tak zcela odstranit komponenty z vašeho systému, odstraňte ho z projektů nebo řešení a složky mezipaměti výše.
