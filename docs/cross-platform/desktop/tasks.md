---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Běžné úlohy porovnání
description: Tento dokument porovná postupy k provádění různých běžných úloh na grafický subsystém WPF a Xamarin.Forms. Vypadá to na tlačítka, časovače, velikosti písem, otevřete identifikátoru URI a zobrazení listu akce.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780473"
---
# <a name="common-tasks-comparison"></a>Běžné úlohy porovnání

| Úloha | WPF | Xamarin.Forms |
|--- |--- |--- |
|Zobrazí zpráva na obrazovce s tlačítka|`MessageBox`|`Page.DisplayAlert`|
|Vytvoření časovač|`DispatcherTimer` – Třída|`Device.StartTimer` statickou metodu|
|Získat výchozí velikost písma|`SystemFonts` Statická třída|`Device.GetNamedSize` statickou metodu|
|Otevřete URI nebo adresa URL|`Process.Start`|`Device.OpenUri`|
|Zobrazte seznam akce (seznam tlačítka)|není k dispozici|`Page.DisplayActionSheet`|
