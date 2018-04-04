---
title: Xamarin pro vývojáře jazyka Objective-C
description: Pokud jste vývojář jazyka Objective-C, se i na moct využívat znalosti a existující kód jazyka Objective-C na platformě Xamarin při využívat kód znovu použít výhody jazyka C#. Tato část slouží jako vstupní bod do Xamarin.iOS a odkazuje na širokou řadu informace o použití existujícího kódu jazyka Objective-C z jazyka C#.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e29762fb258f7d796878c85bfe6f7aaa93207c5e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-for-objective-c-developers"></a>Xamarin pro vývojáře jazyka Objective-C

_Pokud jste vývojář jazyka Objective-C, se i na moct využívat znalosti a existující kód jazyka Objective-C na platformě Xamarin při využívat kód znovu použít výhody jazyka C#. Tato část slouží jako vstupní bod do Xamarin.iOS a odkazuje na širokou řadu informace o použití existujícího kódu jazyka Objective-C z jazyka C#._

Nabízí Xamarin cestu pro vývojáře cílené iOS přesunout jejich bez uživatelského rozhraní chcete-li bez ohledu na platformu C# kód, aby ho bylo možné použít kdekoli C# je k dispozici, včetně zařízení s Androidem přes Xamarin.Android a různých typů systému Windows. Ale stejně, protože používáte C# s Xamarinem neznamená, že nelze využít existujících dovedností a kód jazyka Objective-C. Ve skutečnosti znalost jazyka Objective-C zadává jste vývojář lepší Xamarin.iOS protože Xamarin zpřístupní všechny nativní platformu iOS a OS X rozhraní API, které znáte a kterými rádi pracují, například UIKit, animace jádra, základní Foundation a základní grafické prvky a další. Ve stejnou dobu získáte sílu jazyka C#, včetně funkcí, jako jsou LINQ a obecné typy a také bohaté rozhraní .NET základní knihovny tříd pro použití v nativních aplikací.

Kromě toho Xamarin umožňuje využít existující Objective-C prostředky pomocí technologie vědět jako vazby. Jednoduše vytvořte statické knihovny v Objective-C a zveřejnění jazyka C# prostřednictvím vazbu, jak je znázorněno v následujícím diagramu:

 [![](images/01-bindings.png "Statické knihovny v Objective-C zveřejňovány prostřednictvím vazbu na C#")](images/01-bindings.png#lightbox)

Toto nemusí být omezeny na kód bez uživatelského rozhraní. Vazby můžou zpřístupnit vyvinuté v Objective-C také uživatelského rozhraní kódu.

## <a name="transitioning-from-objective-c"></a>Přechod z jazyka Objective-C

Najdete tu nadbytku informace na našem webu Dokumentace ke snadné tranistion pro Xamarin, znázorňující integrovat co znáte kód C#. Některé mezi nejdůležitější funkce, které vám pomůžou začít patří:

-   [Úvod do jazyka C# pro vývojáře jazyka Objective-C](primer.md) – krátkodobých Úvod do jazyka Objective-C vývojářům vyhledávání přesunout do Xamarin a jazyka C#. 
-   [Návod: Vytvoření vazby knihovna jazyka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md) – podrobný návod pro opětovné použití existujícího kódu jazyka Objective-C aplikace pro Xamarin.iOS. 


## <a name="binding-objective-c"></a>Vazba Objective-C

Po pochopit o tom, jak C# porovnává jazyka Objective-C a již dříve pracovali prostřednictvím výše vazby návodu budete v pořádku pro přechod na platformě Xamarin. Jako následnou, podrobnější informace o Xamarin.iOS vazby technologie, včetně komplexní vazby odkaz je k dispozici v [vazby Objective-C](~/ios/platform/binding-objective-c/index.md) části.

## <a name="cross-platform-development"></a>Vývoj pro různé platformy

Nakonec po přesunutí pro Xamarin.iOS, budete chtít najdete pokyny a platformy, které máme, včetně případové studie referenční aplikací máme devleoped, společně s osvědčené postupy pro vytváření a platformy, opakovaně použitelný kód obsažené v [ Sestavování aplikací platformy křížové části](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
