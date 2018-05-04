---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Běžné úlohy porovnání
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 84b3edc1a8cbc9ad642d94b457321786fae5fe70
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="common-tasks-comparison"></a>Běžné úlohy porovnání

| Úloha | WPF | Xamarin.Forms |
|--- |--- |--- |
|Zobrazí zpráva na obrazovce s tlačítka|`MessageBox`|`Page.DisplayAlert`|
|Vytvoření časovač|`DispatcherTimer` – Třída|`Device.StartTimer` statickou metodu|
|Získat výchozí velikost písma|`SystemFonts` Statická třída|`Device.GetNamedSize` statickou metodu|
|Otevřete URI nebo adresa URL|`Process.Start`|`Device.OpenUri`|
|Zobrazte seznam akce (seznam tlačítka)|není k dispozici|`Page.DisplayActionSheet`|
