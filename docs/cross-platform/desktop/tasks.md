---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Běžné úlohy porovnání
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4d45ae61c5d7a7093d51c4440d11dd883c157a4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="common-tasks-comparison"></a>Běžné úlohy porovnání

| Úloha | WPF | Xamarin.Forms |
|--- |--- |--- |
|Zobrazí zpráva na obrazovce s tlačítka|`MessageBox`|`Page.DisplayAlert`|
|Vytvoření časovač|`DispatcherTimer` – Třída|`Device.StartTimer` statickou metodu|
|Získat výchozí velikost písma|`SystemFonts` Statická třída|`Device.GetNamedSize` statickou metodu|
|Otevřete URI nebo adresa URL|`Process.Start`|`Device.OpenUri`|
|Zobrazte seznam akce (seznam tlačítka)|není k dispozici|`Page.DisplayActionSheet`|
