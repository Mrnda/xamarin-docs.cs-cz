---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353266"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Snímač rychlosti](xref:Xamarin.Essentials.SensorSpeed)

- **Nejrychlejší** – získejte data ze snímačů nejrychleji (není zaručeno, že se vrátíte na vlákně UI).
- **Hra** – ohodnotit vhodný pro hry (není zaručeno, že se vrátíte na vlákně UI).
- **Normální** – výchozí míra vhodný pro změny orientace obrazovky.
- **Uživatelské rozhraní** – ohodnotit vhodný pro hlavní uživatelské rozhraní.

Pokud vaše obslužná rutina události není zaručeno běžet ve vlákně uživatelského rozhraní a Pokud obslužná rutina události musí pro přístup k prvkům uživatelského rozhraní, použijte [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) metoda pro spuštění tohoto kódu na vlákně UI.