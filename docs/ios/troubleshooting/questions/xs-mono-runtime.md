---
title: Nastavení proměnných prostředí Mono Runtime pro iOS projekty v Xamarin studiu
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 5f4f3a2de012d35ddca9c1fa830d599d9d5acb17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Nastavení proměnných prostředí Mono Runtime pro iOS projekty v Xamarin studiu

Pokud potřebujete nastavit všechny runtime proměnných prostředí pro Mono, může být nastavena v **možnosti projektu > Spustit > Obecné** stránky.

Poznámka: Proměnné prostředí uvolňování paměti pro SGen (MONO\_GC\_parametry) sady tímto způsobem bude použit, pouze při spuštění z Xamarin Studio. Spuštění aplikace ze zařízení,-li nastavení pro Sgen budou ignorovány. 

Trvale nastavit proměnnou prostředí pro aplikaci, musíte přidat jako argument další mtouch (pro všechny příslušné konfigurace):

```csharp
   --setenv=NAME=VALUE
```

Informace o proměnných prostředí, které lze nastavit, najdete na stránku Mono man: [ http://docs.go-mono.com/?link=man%3amono(1) ](http://docs.go-mono.com/?link=man%3amono(1)) najdete v části s názvem: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Nastavení proměnných prostředí pro projekt")
