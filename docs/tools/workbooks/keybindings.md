---
title: "Xamarin sešity Editor klávesové zkratky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: cbbf070a9c94221d98dedcd1df884a897ff7df3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin sešity Editor klávesové zkratky

## <a name="the-return-key-and-its-nuances"></a>Klíč vrátit a jeho drobné odlišnosti

Následující tabulka popisuje různé vazeb klíče pro provádění kódu a vytváření markdownu. Přesměrovali jsme pozor zvolit rozumný a konzistentní vazeb klíče, které jsou známé a plynulá práce.

<table>
  <tr>
    <th>Vazbu na klíč</th>
    <th>Buňky kódu</th>
    <th>Markdown Cell</th>
  </tr>
  <tr>
    <td><kbd>Vrátí</kbd></td>
    <td>
      <p>Pokud znak je na konci buňky vyrovnávací paměti a buňky možné úspěšně analyzovat, bude spuštěn a výsledky se zobrazí pod vyrovnávací paměti a nové buňky kódu se vloží a zaměřuje buňky po spuštění buňky.</p>
      
      <p>Pokud analýza není úspěšné., nový řádek se vloží do vyrovnávací paměti. Kompilátoru diagnostiky nebude možné vytvořit, pokud analýza není úspěšné.</p>
    </td>
    <td>
      <p>
        <kbd>Vrátí</kbd> vykazuje různé chování v závislosti na kontextu Markdownu na pomocí kurzoru.
      </p>
      <ul>
        <li>
Pokud pomocí kurzoru v bloku kódu Markdownu, vloží se literálu nový řádek.
        </li>
        <li>
Pokud pomocí kurzoru v Markdownu seznamu bloku, vytvořit novou položku seznamu, nebo rozdělit aktuální položky seznamu.
        </li>
        <li>
Pokud pomocí kurzoru v žádný jiný druh Markdownu bloku, vytvořit nový blok odstavce nebo rozdělení aktuálnímu bloku.
        </li>
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Return</kbd></dd>
      </dl>
    </td>
    <td>
      <p>Vždy se pokusí pro analýzu a zpracování obsah buňky. Pokud kompilace úspěšné, výsledky (včetně zpracování výjimek) se zobrazí pod vyrovnávací paměť, a pokud neexistují žádné další buněk, budou novou vytvořen a zaměřuje.</p>
      
      <p>Pokud nejsou žádné chyby kompilace, zobrazí diagnostiky a vyrovnávací paměť zůstanou cílených s pomocí kurzoru pozici beze změny.</p>
    </td>
    <td>
Vloží a pokrývá nové buňky kódu po aktuální buňky markdownu.
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Shift‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Shift‑Return</kbd></dd>
      </dl>
    </td>
    <td>
Vloží a pokrývá nové buňky markdownu po aktuální buňky.
    </td>
    <td>
Stejné chování jako <kbd>vrátit</kbd>
    </td>
  </tr>
  <tr>
    <td><kbd>Shift‑Return</kbd></td>
    <td>
Vždy vloží nový řádek, bez ohledu na umístění pomocí kurzoru nebo obsahu.
    </td>
    <td>
Vloží zalomení řádku pevný v aktuální Markdownu bloku.
    </td>
  </tr>
</table>
