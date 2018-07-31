---
title: Xamarin pro vývojáře v Objective-C
description: Tento dokument obsahuje popis Xamarin.iOS pro vývojáře v Objective-C. To obsahuje odkazy na pokyny, které popisují, jak přechod do jazyka C# v jazyce Objective-C, jak vytvořit vazbu knihovnou Objective-C pro použití v jazyce C# a jak vytvářet multiplatformní mobilní aplikace.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 574bd4c0b6bed639230dbfb6209eb78216e8e47b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351005"
---
# <a name="xamarin-for-objective-c-developers"></a>Xamarin pro vývojáře v Objective-C

Xamarin nabízí cestu pro vývojáře cílené iOS přesunout jejich bez uživatelského rozhraní kódu pro více platforem C# tak, aby ho můžete použít kdekoli C# je k dispozici, včetně zařízení s Androidem pomocí Xamarin.Android a různých verzí Windows. Ale to, že používáte C# s Xamarinem neznamená, že nemůžou využívat stávající dovednosti a kód Objective-C. Ve skutečnosti znalost jazyka Objective-C vám umožňuje lepší developer Xamarin.iOS protože Xamarin poskytuje nativní aplikace pro iOS a OS X platformy rozhraní API, které znáte a máte rádi, jako jsou UIKit, Core animace, základní funkce a základní grafické prvky pár. Ve stejnou dobu získáte sílu jazyka C#, včetně funkcí, jako je LINQ a obecné typy, jakož i bohaté .NET základní knihovny tříd pro použití v nativních aplikacích.

Kromě toho Xamarin umožňuje využít stávající Objective-C prostředkům prostřednictvím technologie znát pod některým z vazby. Stačí vytvořit statické knihovny v jazyce Objective-C a zveřejníte ho na C# prostřednictvím vazbu, jak je znázorněno v následujícím diagramu:

 [![](images/01-bindings.png "Statické knihovny v Objective-C, které jsou zveřejňovány prostřednictvím vazby do jazyka C#")](images/01-bindings.png#lightbox)

To nemusí být omezený na kódu bez uživatelského rozhraní. Vazby můžete zveřejnit uživatelského rozhraní kódu také vyvinuté v jazyce Objective-C.

## <a name="transitioning-from-objective-c"></a>Přechod z Objective-C

Najdete tu množství informace na našem webu dokumentace, abychom vám tranistion xamarinu, ukazuje, jak integrovat toho, co už znáte kód jazyka C#. Mezi nejdůležitější funkce, které vám pomůžou začít patří:

-   [Úvod do jazyka C# pro vývojáře v Objective-C](primer.md) – krátký základy pro vývojáře v Objective-C uvažujete o přesunu do Xamarinu a jazyka C#. 
-   [Návod: Vytvoření vazby knihovnou Objective-C](~/ios/platform/binding-objective-c/walkthrough.md) – podrobný postup pro opakované použití existující kód Objective-C aplikace pro Xamarin.iOS. 


## <a name="binding-objective-c"></a>Vazeb Objective-C

Jakmile znát jak C# si vede k Objective-C a již dříve pracovali prostřednictvím vazby návod výše, bude v pořádku pro přechod na platformu Xamarin. Následujícím způsobem si podrobnější informace o technologiích vazby Xamarin.iOS, včetně komplexní vazby odkazu je k dispozici v [vazeb Objective-C](~/ios/platform/binding-objective-c/index.md) oddílu.

## <a name="cross-platform-development"></a>Vývoj pro různé platformy

Nakonec po přesunutí pro Xamarin.iOS, budete chtít podívejte se různé platformy pokyny, pomocí kterých máme, včetně případové studie máme devleoped spolu s osvědčené postupy pro vytváření napříč platformami, opakovaně použitelný kód obsažený v aplikacíodkaz[ Sestavování aplikací platformu pro různé části](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
