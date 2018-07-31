---
title: 'Proč Moje aplikace pro iOS 9 selže s: System.Exception: Nepodařilo se zařadit Objective-C object?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 93666fcb39f0cd717c14eb07e6407801e9f0642e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350596"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Proč Moje aplikace pro iOS 9 selže s: System.Exception: Nepodařilo se zařadit Objective-C object?

Může se zobrazit chyba tohoto formuláře:

> System.Exception: Nepodařilo se zařadit Objective-C object... Nepovedlo se najít existující spravované instance pro tento objekt...

Změny rozhraní API v systému iOS 9 vyžadují použít konstruktor zpětného volání při volání nespravovaného kódu, jako rozhraní API základní očekává. Přidat konstruktor zpětné volání do třídy, použijte následující řádek: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Další kroky

Potřebujete další pomoc, kontaktujte nás, nebo pokud tento problém i po použití výše uvedené informace, najdete [jaké možnosti podpory jsou k dispozici pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace týkající se možností kontaktu, návrhy, jak se dá soubor s novou chybou, v případě potřeby. 
