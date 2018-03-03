---
title: "Datová vazba"
description: "Datová vazba je technika propojení vlastnosti dva objekty tak, aby se změny v jednu vlastnost automaticky projeví ve jiné vlastnosti. Datová vazba je nedílnou součástí architektury Model-View-ViewModel (modelem MVVM) aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: eefff0ea23135c09d86284c0fddba9e80f50004a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="data-binding"></a>Datová vazba

_Datová vazba je technika propojení vlastnosti dva objekty tak, aby se změny v jednu vlastnost automaticky projeví ve jiné vlastnosti. Datová vazba je nedílnou součástí architektury Model-View-ViewModel (modelem MVVM) aplikace._

## <a name="the-data-linking-problem"></a>Problém s propojení dat

Aplikaci Xamarin.Forms se skládá z jedné nebo více stránek, z nichž každý obvykle obsahuje více objektů uživatelského rozhraní nazývá *zobrazení*. Jedním z primárních úkolů programu je zachovat tato zobrazení synchronizovat a můžete sledovat různé hodnoty nebo možnosti, které představují. Často zobrazení představují hodnoty z podkladové zdroje dat a uživatel manipuluje tato zobrazení, chcete-li změnit tato data. Při změně zobrazení, musí se tato změna promítnout základní data a podobně, když se změní v základních datech, tato změna musí projevit v zobrazení.

Tuto úlohu úspěšně zpracovat, je nutno program upozornit na změny těchto zobrazení nebo základní data. Běžné řešení je k definování události, které signalizován dojde ke změně. Obslužné rutiny události možné nainstalovat, je upozornění na tyto změny. Reaguje tak data z jednoho objektu do jiné. Ale po mnoho zobrazení se musí existovat také mnoho obslužné rutiny událostí a získá podílejí velké množství kódu.

## <a name="the-data-binding-solution"></a>Řešení vazba dat

Datová vazba automatizuje úlohy a vykreslí obslužné rutiny událostí zbytečné. (Události jsou stále nutné, ale protože je používá infrastrukturu vazby dat). Datové vazby se dají implementovat v kódu nebo v jazyce XAML, ale jsou mnohem častější v jazyce XAML, kde mohou pomoci snížit velikost souboru kódu na pozadí. Nahrazením procedurální kód obslužné rutiny událostí pomocí deklarativní kódu nebo kód aplikace je jednodušší a vyjasněno.

Mezi tyto dva objekty účastnící se datová vazba je téměř vždy element, který je odvozen od `View` a součástí visual rozhraní stránky. Druhý objekt je buď:

- Jiné `View` odvozené, obvykle na stejné stránce.
- Objekt v souboru kódu.

V ukázce programy například v [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) ukázkové, vazby dat mezi dvěma `View` odvozené konfigurace se často zobrazují pro účely přehlednost a jednoduchost. Však můžete použít stejné zásady pro vazby dat mezi `View` a dalších objektů. Pokud aplikace vytvořená s využitím architektury Model-View-ViewModel (modelem MVVM), třídu pomocí zadaných dat se často nazývá ViewModel.

Datové vazby jsou podrobně následující řadu články:

## <a name="basic-bindingsbasic-bindingsmd"></a>[Základní vazby](basic-bindings.md)

Informace o rozdílu mezi datové vazby cíl a zdroj a zobrazí jednoduché datové vazby v kódu a XAML.

## <a name="binding-modebinding-modemd"></a>[Režim vazeb](binding-mode.md)

Zjistit, jak můžou režimu vazby řídit tok dat mezi dvěma objekty.

## <a name="string-formattingstring-formattingmd"></a>[Formátování řetězců](string-formatting.md)

Datová vazba použijte pro formátování a zobrazení objektů jako řetězce.

## <a name="binding-pathbinding-pathmd"></a>[Cesta vazby](binding-path.md)

Ponořit hlouběji do `Path` vlastnosti datové vazby k dílčí vlastnosti a členy kolekce.

## <a name="binding-value-convertersconvertersmd"></a>[Převaděče hodnot vazeb](converters.md)

Převodníky hodnot vazby použijte ke změně hodnot v rámci datové vazby.

## <a name="the-command-interfacecommandingmd"></a>[Rozhraní příkazového řádku](commanding.md)

Implementace `Command` vlastnost s datové vazby.



## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Kapitola vazby dat z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [Rozšíření značek XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
