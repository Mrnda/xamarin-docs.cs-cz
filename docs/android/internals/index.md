---
title: Rozšířené koncepty a interní informace o
description: Základní architektura za Xamarin.Android a jeho návrhu rozhraní API.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/21/2018
ms.openlocfilehash: 79e61db4c27a2d29b4ee0a9d39f2d25ea5d93303
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/23/2018
ms.locfileid: "34458351"
---
# <a name="advanced-concepts-and-internals"></a>Rozšířené koncepty a interní informace o

_Tento oddíl obsahuje témata, které popisují architekturu, rozhraní API návrhu a omezení Xamarin.Android. Kromě toho obsahuje témata, která popisují jeho implementace kolekce paměti a sestavení, která jsou k dispozici v Xamarin.Android. Protože je Xamarin.Android [open-source](https://github.com/xamarin/xamarin-android), je také možné zjistit vnitřního chodu Xamarin.Android tak, že prověří jeho zdrojový kód._


##  <a name="architectureandroidinternalsarchitecturemd"></a>[Architektura](~/android/internals/architecture.md)

Tento článek vysvětluje základní architektury za aplikace pro Xamarin.Android. Se vysvětluje, jak se aplikace Xamarin.Android spustit v prostředí Mono provádění společně s Androidem runtime virtuálního počítače a vysvětluje tyto klíčové koncepty jako Android obálky s možností a spravované obálky s možností. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[Návrh rozhraní API](~/android/internals/api-design.md)

Kromě základních základní knihovny tříd, které jsou součástí Mono Xamarin.Android se dodává s vazby pro různé rozhraní Android API umožňuje vývojářům vytvářet nativní aplikace pro Android s Mono.

Základem Xamarin.Android existuje je modul spolupráce této world mostů jazyka C# s Java world a přístup k rozhraním API Java z jazyka C# nebo jinými jazyky rozhraní .NET poskytuje vývojářům.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Sestavení](~/cross-platform/internals/available-assemblies.md)

Xamarin.Android se dodává s několika sestavení. Stejně jako Silverlight je rozšířené podmnožině klientů sestavení .NET, Xamarin.Android je také rozšířené podmnožinu několik Silverlight a plochy sestavení .NET. 

