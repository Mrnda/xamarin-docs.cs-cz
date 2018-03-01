---
title: "Proč nefunguje návrháře Visual Studio XAML pro soubory Xamarin.Forms XAML?"
ms.topic: article
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fae0792b6db940bb8b4aa4772eb0b5bb42b78da1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Proč nefunguje návrháře Visual Studio XAML pro soubory Xamarin.Forms XAML?

Xamarin.Forms nepodporuje aktuálně vizuální nástroje pro soubory XAML. Z tohoto důvodu se při pokusu o otevření souboru XAML formulářů v buď Visual Studio *uživatelského rozhraní návrháře XAML* nebo *návrháře XAML uživatelského rozhraní pomocí kódování*, je vyvolána se následující chybová zpráva:

> "Soubor nejde otevřít pomocí editoru vybrané. Zvolte prosím jiný editor."

Toto omezení je popsaná v [přehled](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) části [Xamarin.Forms XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md) průvodce:

> "Není zatím vizuálního návrháře pro generování XAML v aplikacích Xamarin.Forms, takže všechny XAML musí být ručně psané."
