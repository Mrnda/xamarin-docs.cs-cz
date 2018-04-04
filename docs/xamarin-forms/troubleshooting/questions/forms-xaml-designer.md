---
title: Proč nefunguje návrháře Visual Studio XAML pro soubory Xamarin.Forms XAML?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 1f82f16429ca23a4ba6806f775310dd90126096e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Proč nefunguje návrháře Visual Studio XAML pro soubory Xamarin.Forms XAML?

Xamarin.Forms nepodporuje aktuálně vizuální nástroje pro soubory XAML. Z tohoto důvodu se při pokusu o otevření souboru XAML formulářů v buď Visual Studio *uživatelského rozhraní návrháře XAML* nebo *návrháře XAML uživatelského rozhraní pomocí kódování*, je vyvolána se následující chybová zpráva:

> "Soubor nejde otevřít pomocí editoru vybrané. Zvolte prosím jiný editor."

Toto omezení je popsaná v [přehled](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) části [Xamarin.Forms XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md) průvodce:

> "Není zatím vizuálního návrháře pro generování XAML v aplikacích Xamarin.Forms, takže všechny XAML musí být ručně psané."
