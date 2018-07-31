---
title: Klávesové zkratky editoru sešity Xamarin
description: Tento dokument popisuje klávesové zkratky, které jsou k dispozici pro použití v editoru Xamarin Workbooks. Zejména dohlíží na různé způsoby, jak použít klávesu Return.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: c2b4a8c1bcb8f7b88ab2ae1e2906b1c9c702b76a
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351675"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Klávesové zkratky editoru sešity Xamarin

## <a name="the-return-key-and-its-nuances"></a>Vrátí klíč a jeho odlišnosti

Následující tabulka popisuje různé klávesové zkratky pro provádění kódu a vytváření markdownu. Přesměrovali jsme velmi pečlivě zvolit rozumné a konzistentní klávesové zkratky, které jsou známé a plynulé.

|Klávesová zkratka|Buňky kódu|Markdown buňky|
|--- |--- |--- |
|<kbd>Vrátí</kbd>|<p>Pokud blikající kurzor na konec vyrovnávací paměti buňky a buňku může být úspěšně analyzován, se spustí a výsledky se zobrazí pod vyrovnávací paměti a novou buňku kódu bude vložen a zaměřuje buňky po prováděnou buňky.</p><p>Pokud analýza není úspěšné, vloží nový řádek do vyrovnávací paměti. Diagnostika kompilátoru nebude vytvořeno, pokud analýzy se nepovedlo úspěšně dokončit.</p>|<p><kbd>Vrátí</kbd> vykazují různé chování v závislosti na kontextu Markdownu za blikajícím kurzorem.</p><ul><li>Pokud kurzor se v bloku kódu Markdown, je vložena literálu nový řádek.</li><li>Pokud blikající kurzor je v seznamu bloku Markdownu, vytvořte novou položku seznamu nebo rozdělit aktuální položky seznamu.</li><li>Pokud blikající kurzor je v jiném typu blok Markdownu, vytvořte nový blok odstavce nebo rozdělení aktuálního bloku.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Windows</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Vždy se pokusí analyzovat a spouštět obsah buňky. Je-li sestavení úspěšné, výsledky (včetně zpracování výjimek) se zobrazí pod vyrovnávací paměť, a pokud neexistují žádné další buňky, nové vytvoření, který se zaměřuje.</p><p>Pokud nejsou žádné chyby kompilace, diagnostiky se zobrazí a vyrovnávací paměť zůstane cílené s nezměněná pozice blikajícího kurzoru.</p>|Vloží a pokrývá novou buňku kódu po aktuální buňky markdownu.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Windows</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Vloží a pokrývá novou buňku markdownu za aktuální buňku.|Stejné chování jako <kbd>vrátit</kbd>|
|<kbd>Shift‑Return</kbd>|Vždy vloží nový řádek, bez ohledu na pozici blikajícího kurzoru nebo obsahu.|Vloží konec řádku pevný v rámci aktuálního bloku Markdownu.|
