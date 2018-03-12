---
title: "Proč Moje aplikace pro iOS 9 dojde k chybě: System.Exception: Nepodařilo se zařazování objekt jazyka Objective-C?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: baba2526eefa1b69d47da47b73ea0bd417ecdc57
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Proč Moje aplikace pro iOS 9 dojde k chybě: System.Exception: Nepodařilo se zařazování objekt jazyka Objective-C?

Zobrazí může chybu tohoto formuláře:

> System.Exception: Nepodařilo se zařazování objekt jazyka Objective-C... Nelze najít existující spravované instance pro tento objekt...

Rozhraní API změny v iOS 9 vyžaduje, aby se konstruktor zpětného volání používané při volání nespravovaného kódu jako rozhraní API základní očekává, že. Pomocí následujícího řádku přidejte konstruktor zpětného volání pro třídu: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Další kroky

O další pomoc, kontaktujte nás, nebo pokud tento problém zůstane i po použití výše uvedené informace najdete v tématu [jaké možnosti podpory jsou dostupná pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace o možnostech kontaktní, návrhy, a také jak soubor nové chyby v případě potřeby. 