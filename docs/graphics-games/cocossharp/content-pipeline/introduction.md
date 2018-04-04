---
title: Úvod do obsahu kanálů
description: Obsah kanálů jsou aplikací nebo součástí aplikace, který slouží k převedení souborů do formátu, který lze načíst herní projekty. Kanál MonoGame obsah je implementace konkrétní obsahu kanálu pro převod souborů pro CocosSharp a MonoGame projekty.
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2c3619fac771bd7962f6940a24d7c1ff81173d75
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-content-pipelines"></a>Úvod do obsahu kanálů

_Obsah kanálů jsou aplikací nebo součástí aplikace, který slouží k převedení souborů do formátu, který lze načíst herní projekty. Kanál MonoGame obsah je implementace konkrétní obsahu kanálu pro převod souborů pro CocosSharp a MonoGame projekty._

Tento článek obsahuje rámcové informace o obsahu kanály, především zaměřené na *MonoGame obsahu kanálu*, což je použít s CocosSharp a MonoGame implementace obsahu kanálu.


## <a name="what-is-a-content-pipeline"></a>Co je obsahu kanálu?

Termín *obsahu kanálu* je obecný termín pro proces převodu soubor z jednoho formátu do druhého. *Vstupní* obsahu kanálu je obvykle soubor výstupem nástroje pro vytváření, jako jsou například soubory bitové kopie z aplikace Photoshop. Vytvoří obsahu kanálu *výstup* soubor ve formátu, který můžete načíst přímo pomocí herní projektu. Výstupní soubory obvykle jsou optimalizované pro rychlé načítání a snížit velikost disku.

Obsah může být kanály příkazového řádku spustitelné soubory, vyhrazené aplikace založené na grafickém uživatelském rozhraní nebo moduly plug-in vložených v jiné aplikaci, třeba v sadě Visual Studio. Kanály obsahu obvykle běží před provedením příslušnou hru. Pokud obsahu kanál je přidružený některé aplikace jako v aplikaci Visual Studio, může spustit během kompilace. Pokud obsahu kanálu je samostatná aplikace, pak ho může být při výslovně vás vyzval k tomu uživatelem. Aplikace nebo logiku zodpovědná za převod konkrétní vstupní soubor (například **.png**) pro přidružené výstupní soubor se označuje jako *Tvůrce*. 

Jsme můžete vizualizovat cestu, která soubor přebírá z vývojového do načítá za běhu následujícím způsobem:

![](introduction-images/image1.png "V tomto diagramu jsou vizualizována cestu, která přebírá soubor z vývojového do načítá za běhu")

## <a name="why-use-a-content-pipeline"></a>Proč používat obsahu kanálu?

Kanály obsahu zavést krok navíc mezi vytváření aplikace a hry, které můžete zvýšit dobu kompilace a přidání složitosti do procesu vývoje. Bez ohledu na těchto aspektů obsahu kanálů představit vývoj her pro řadu výhod:


### <a name="converting-to-a-format-understood-by-the-game"></a>Převod do formátu rozumí hry

CocosSharp a MonoGame poskytují metody pro načítání různých typů obsahu; ale obsah musí být ve správném formátu před načítá. Většina typů obsahu vyžadují nějaký typ převodu před načítá. Například zvuk efekty při **.wav** převést do formátu **.xnb** soubor má být načten za běhu, protože CocosSharp a MonoGame nepodporují načítání **.wav** Formát souboru.


### <a name="converting-to-a-format-native-to-the-hardware"></a>Převod do formátu nativní na hardware.

Jiný hardware mohou považovat obsah jinak za běhu. Například můžete CocosSharp hry načíst soubory obrázků při vytváření `CCSprite` instance. Stejný kód se dá použít k načtení souborů na iOS a Android, ukládá každou platformu jinak načíst soubor. V důsledku toho obsahu kanálu MonoGame formáty texture **.xnb** soubory odlišně v závislosti na cílové platformy.


### <a name="reducing-size-on-disk"></a>Zmenšení velikosti na disku 

Obsah kanály lze použít k odebrání informací, což je užitečné v době autora, ale není nutné za běhu. Původní soubor (vstupu) může ukládat všechny informace, které pomáhají udržovat existující obsah tvůrci obsahu, ale výstupní soubor může být stripped-down ponechat malý celkové herní soubor. Tento faktor je užitečné zejména pro mobilní hry, které jsou staženy spíše než rozdělit na instalačním médiu.


### <a name="reducing-load-time"></a>Snižuje čas načtení

Hry může vyžadovat úpravy obsahu zlepšujících výkon modulu runtime, ke zlepšení vizuály nebo přidávání nových funkcí. Například mnoho 3D hry vypočítat osvětlení jednou a potom použít výsledek tohoto výpočtu při vykreslování komplexní scény. Od provádění tyto výpočty při načítání obsahu může být výtažkovými výpočet můžete místo toho provést, pokud je integrovaná hra. Výsledný výpočty můžou být součástí obsahu, povolení obsahu mnohem rychleji než v opačném případě by bylo možné ho načíst. 


## <a name="xnb-file-extension"></a>Přípona souboru xnb

**.Xnb** přípona souboru je rozšíření pro všechny soubory, které jsou výstupem Monogame obsahu kanálu. To odpovídá rozšíření souborů výstupem Microsoft XNA obsahu kanálu.

**.Xnb** bez ohledu na původní typ souboru se používá rozšíření. Jinými slovy, soubory obrázků (**.png**), zvukových souborů (**.wav**), a jakékoli vlastní typy souborů budou všechny výstupu jako **.xnb** soubory, když uplyne obsahu kanálem. Vzhledem k tomu, že rozšíření nelze použít k rozlišení různých souborových formátů pak CocosSharp a MonoGame metody, které načíst **.xnb** soubory Nečekejte rozšíření při načítání souboru.

Soubory .xnb CocosSharp a MonoGame lze vytvořit pomocí nástroje Monogame kanál, kterému se věnujeme [v tomto návodu](~/graphics-games/cocossharp/content-pipeline/walkthrough.md).


## <a name="summary"></a>Souhrn

Tento článek poskytuje přehled a výhod obsahu kanálů obecně platí, a také Úvod do kanálu MonoGame obsah.

## <a name="related-links"></a>Související odkazy

- [Dokumentace MonoGame kanálu](http://www.monogame.net/documentation/?page=Pipeline)
