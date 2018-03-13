---
title: "Interaktivní sešity"
description: "Pomocí sešity můžete vytvořit za provozu dokumenty s kódem jazyka C# pro experimentování, výuky za školení nebo zkoumat."
ms.topic: article
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: c111d2f873270eab78eee92edc3d884d1e92fdd8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="interactive-workbooks"></a>Interaktivní sešity

_Pomocí sešity můžete vytvořit za provozu dokumenty s kódem jazyka C# pro experimentování, výuky za školení nebo zkoumat._

Sešity můžete použít jako samostatná aplikace, oddělený od vaší IDE.

Chcete-li začít vytvářet nový sešit, spusťte aplikaci sešity. Pokud jste nenainstalovali to už, navštivte [instalace](~/tools/workbooks/install.md#install) stránky. Zobrazí se výzva k vytvoření sešitu v vaši platformu podle vlastní volby, který se automaticky připojí k aplikaci agenta umožňuje vizualizovat vašeho dokumentu v reálném čase.

Pokud sešity aplikace je již spuštěna, můžete vytvořit nový dokument procházením **soubor > Nový**.

Sešity můžete uložit a později znovu otevřít v aplikaci. Pak je můžete sdílet s ostatními k předvedení nápady, prozkoumejte nových rozhraní API nebo naučit nových konceptů.

## <a name="code-editing"></a>Úpravy kódu

Okně pro úpravu kódu poskytuje doplňování kódu, barevné zvýrazňování syntaxe, vložené diagnostiky za provozu a podpora příkaz víceřádkový.

[ ![](workbook-images/inspector-0.6.0-repl-small.png "Okně pro úpravu kódu poskytuje doplňování kódu, barevné zvýrazňování syntaxe, vložené diagnostiky za provozu a podpora Víceřádkový – příkaz")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Sešity Xamarin jsou uloženy ve `.workbook` souboru, který je soubor CommonMark se některá metadata v horní části (najdete v části [typy souborů sešitů](#Workbooks_Files_Types) další podrobnosti o tom, jak lze uložit sešity).

### <a name="nuget-package-support"></a>Balíček NuGet podpory

Mnoho oblíbených balíčky NuGet jsou podporované přímo v sešitech Xamarin. Balíčky můžete vyhledat procházením **soubor > Přidat balíček**. Přidání balíčku se automaticky otevře `#r` příkazy odkazování na sestavení balíčku, což umožňuje je používal hned.

Při ukládání sešitu s odkazy na balíček, tyto odkazy jsou také uložena. Pokud sdílíte sešit s jinou osobou, stáhnou automaticky odkazované balíčky.

Existují známé omezení týkající se podpory balíček NuGet v sešitech:

  * Nativní knihovny jsou podporována pouze v iOS a to pouze při propojení s spravované knihovně.
  * Balíčky, které závisí na `.targets` soubory nebo skripty prostředí PowerShell se pravděpodobně nepodaří fungovat podle očekávání.
  * Pokud chcete odebrat ani změnit závislost balíčku, upravte manifest do sešitu v textovém editoru. Správa správný balíček je na cestě.

### <a name="xamarinforms-support"></a>Xamarin.Forms Support

Pokud balíček Xamarin.Forms NuGet odkazujete v sešitu, změní se sešit aplikace jeho hlavní zobrazení být založené na platformě Xamarin.Forms. Budete mít přístup prostřednictvím `Xamarin.Forms.Application.Current.MainPage`.

Na kartě Inspector zobrazení má také speciální podpora znázorňující hierarchii zobrazení Xamarin.Forms vám pomohou pochopit vaše rozložení.

## <a name="rich-text-editing"></a>Úpravy formátovaného textu

Můžete upravit text kolem kódu pomocí editoru zahrnuty, jak je uvedeno dále:

![](workbook-images/inspector-0.6.2-editing.gif "Upravit text kolem kódu pomocí předdefinovaných editoru")

### <a name="markdown-authoring"></a>Vytváření markdownu

Sešit autoři mohou někdy jednodušší přímo upravit CommonMark "zdroj" sešitu s jejich oblíbeném editoru.

Mějte na paměti, pokud upravit a uložit sešit v rámci klienta sešity, CommonMark text může naformátována.

Upozorňujeme, že kvůli rozšíření CommonMark používáme povolit YAML metadata v sešitu soubory, `---` je vyhrazený pro tento účel. Pokud chcete vytvořit [tematické přerušení](http://spec.commonmark.org/0.27/#thematic-break) text, měli byste použít `***` nebo `___` místo. Takové zalomení je nutno v sešity 1.2 a dříve z důvodu chyby při uložení.

### <a name="improvements-in-workbooks-13"></a>Vylepšení v sešitech 1.3

Také jsme jste rozšířené syntaxe Markdownu bloku uvozovky mírně ke zlepšení prezentace. Přidáním emoji jako první znak v Bloková citace můžete ovlivnit barvu pozadí nabídky:

- `> [!NOTE]
>se vykreslí jako poznámka s modrým pozadím
- `> [!IMPORTANT]
>se vykreslí jako upozornění s žlutý pozadí
- `> [!WARNING]
>se vykreslí jako problém s červenou pozadí

Můžete také propojit hlavičky v dokumentu sešitu. Kotvy u každého záhlaví se vygeneruje s ID ukotvení se textu záhlaví je zpracovat takto:

1. Záhlaví je použita nižší.
1. Budou odebrány všechny znaky s výjimkou alfanumerické znaky a spojovníky.
1. Všechny prostory jsou nahrazeny pomlčky.

To znamená, že hlavičku jako "Důležité záhlaví" získá id `important-header` a může být propojený vložením odkaz na `#important-header` v sešitu.

## <a name="document-structure"></a>Struktura dokumentu

### <a name="cell"></a>Buňky

Samostatná jednotka obsahu, představující spustitelný kód nebo markdownu. Buňky kódu se skládá z až čtyři dílčí součásti:

- Editor
  - Vyrovnávací paměti
- Diagnostika kompilátoru
- Výstup konzoly
- Výsledky spuštění

### <a name="editor"></a>Editor

Komponenta interaktivní textu buňky. U kódu buněk je to editoru kód s zvýraznění syntaxe, atd. Pro markdownu buněk to je editor obsahu formátovaného textu s závislé na kontextu formátování a vytváření panelu nástrojů.

### <a name="buffer"></a>Vyrovnávací paměti
Skutečné textového obsahu editoru.

### <a name="compiler-diagnostics"></a>Diagnostika kompilátoru

Všechny diagnostiky byly při kompilaci kódu, vykreslen pouze v případě, že se požaduje explicitní spuštění. Zobrazí okamžitě níže editoru buňky.

### <a name="console-output"></a>Výstup konzoly

Žádný výstup zapsán standardní na více systémů nebo standardní chyba během provádění buňky. Standardní na více systémů bude vykreslen v černý text a standardní chyba bude vykreslen červeně.

### <a name="execution-results"></a>Výsledky spuštění

Po úspěšné kompilace, bude vykreslen bohatý a interaktivní potenciálně reprezentace výsledky pro buňku, zadaný v důsledku toho je ve skutečnosti produkovaný provádění. Výjimky jsou považovány za výsledky v tomto kontextu, vzhledem k tomu, že se vytváří v důsledku ve skutečnosti spuštění kompilace.

## <a name="workbooks-files-types"></a>Typy souborů sešitů

### <a name="plain-files"></a>Nešifrovaná soubory

Ve výchozím nastavení uloží sešitu jako prostý text `.workbook` souboru, který obsahuje text ve formátu CommonMark.

### <a name="packages"></a>Balíčky

Balíček sešitu je adresář, který je s názvem se `.workbook` rozšíření.
Na vyhledávací na Mac a v dialogové okno otevřít sešity Xamarin a nejnovější soubory nabídky tento adresář bude rozpoznán jako by šlo soubor.

Musí obsahovat adresář `index.workbook` souboru, který je sešit skutečným prostý text, který bude načten v sešitech Xamarin. Adresář může také obsahovat prostředků vyžaduje `index.workbook`, včetně obrázků nebo jiné soubory.

Pokud jako prostý text `.workbook` soubor, který odkazuje na prostředky z jeho stejný adresář je otevřen v sešitech 0.99.3 nebo novější, při jeho uložení, se převede do `.workbook` balíčku. To platí na Mac a Windows.

> [!NOTE]
> **Poznámka:** uživatele Windows se otevře `package.workbook\index.workbook` souboru přímo, ale balíček budou chovat jinak stejně jako na macu.

### <a name="archives"></a>Archivy

Sešit balíčky, probíhá adresáře, může být obtížné snadno distribuovat přes internet. Řešení je archivy sešitu. Sešit archivu je balíček zip komprimované sešitu, s názvem `.workbook` rozšíření.

Počínaje sešity 1.1, při ukládání balíčku sešitu, dialogové okno Uložit nabízí možnost ukládání jako archiv místo. Sešity 1.0 měl žádnou integrovanou možnost vytváření nebo ukládání archivy.

Ve verzi 1.0 sešity, když byl po otevření sešitu archivu, transparentně byl převeden do sešitu balíčku a soubor zip bylo ztraceno. V sešity 1.1 zůstane tento soubor zip. Když uživatel uloží archivu, se nahradí nový soubor zip.

Sešit archivu můžete vytvořit ručně kliknutím pravým tlačítkem myši na balíček sešit a výběrem **komprimovat** v systému Mac, nebo **Odeslat > komprimované složky (ZIP)** v systému Windows. Přejmenujte soubor zip do mají `.workbook` příponu souboru. To funguje pouze s balíčky sešitu, soubory není prostý sešitu.

## <a name="related-links"></a>Související odkazy

- [Vítá vás sešity](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
