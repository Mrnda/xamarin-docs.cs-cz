---
ms.topic: include
ms.openlocfilehash: 5c11c94956f8d56c66c50a9a480177c5b77c2643
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947420"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Snímač rychlosti](xref:Xamarin.Essentials.SensorSpeed)

- **Nejrychlejší** – získejte data ze snímačů nejrychleji (není zaručeno, že se vrátíte na vlákně UI).
- **Hra** – ohodnotit vhodný pro hry (není zaručeno, že se vrátíte na vlákně UI).
- **Normální** – výchozí míra vhodný pro změny orientace obrazovky.
- **Uživatelské rozhraní** – ohodnotit vhodný pro hlavní uživatelské rozhraní.

Pokud vaše obslužná rutina události není zaručeno běžet ve vlákně uživatelského rozhraní a Pokud obslužná rutina události musí pro přístup k prvkům uživatelského rozhraní, použijte [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) metoda pro spuštění tohoto kódu na vlákně UI.