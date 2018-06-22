---
title: Proč nefunguje návrháře Visual Studio XAML pro soubory Xamarin.Forms XAML?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 43088beba6c6a86330cac164856be98d88f07fe2
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/25/2018
ms.locfileid: "34546148"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Proč nefunguje návrháře Visual Studio XAML pro soubory Xamarin.Forms XAML?

Xamarin.Forms nepodporuje aktuálně vizuální nástroje pro soubory XAML. Z tohoto důvodu se při pokusu o otevření souboru XAML formulářů v buď Visual Studio *uživatelského rozhraní návrháře XAML* nebo *návrháře XAML uživatelského rozhraní pomocí kódování*, je vyvolána se následující chybová zpráva:

> "Soubor nejde otevřít pomocí editoru vybrané. Zvolte prosím jiný editor."

Toto omezení je popsaná v [přehled](~/xamarin-forms/xaml/xaml-basics/index.md#Overview) části [Xamarin.Forms XAML Základy](~/xamarin-forms/xaml/xaml-basics/index.md) průvodce:

> "Není zatím vizuálního návrháře pro generování XAML v aplikacích Xamarin.Forms, takže všechny XAML musí být ručně psané."

Však lze zobrazit náhled Xamarin.Forms XAML dokumentu výběrem **zobrazení > ostatní okna > Náhled Xamarin.Forms** možnost nabídky.
