---
title: Animace
description: Xamarin.Forms zahrnuje vlastní animace infrastrukturu, která je jednoduchá pro vytvoření jednoduché animací při zároveň dostatečně flexibilní na to, k vytvoření komplexní animace.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 7cff122e7ecc24f5ad93bd863ee422981871f857
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="animation"></a>Animace

_Xamarin.Forms zahrnuje vlastní animace infrastrukturu, která je jednoduchá pro vytvoření jednoduché animací při zároveň dostatečně flexibilní na to, k vytvoření komplexní animace._

Xamarin.Forms třídy animace cílí různé vlastnosti vizuální prvky, typické animace progresivně změna vlastnost z jednu hodnotu do jiné prostřednictvím v časovém intervalu. Všimněte si, že žádné rozhraní jazyka XAML pro Xamarin.Forms třídy animace. Však může být animace zapouzdřené v [chování](~/xamarin-forms/app-fundamentals/behaviors/index.md) a potom na něj odkazovat z XAML.

## <a name="simple-animationssimplemd"></a>[Jednoduché animace](simple.md)

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Třída poskytuje rozšiřující metody, které lze použít k vytvoření jednoduché animace, které otočit, škálování, převede a vykreslit [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) instance. Tento článek ukazuje vytvoření a zrušení animace pomocí `ViewExtensions` třídy.

## <a name="easing-functionseasingmd"></a>[Funkce uvolnění](easing.md)

Zahrnuje Xamarin.Forms [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) třídu, která umožňuje zadat přenos funkci, která určuje, jak zrychlit animací nebo zpomalit jejich používáte. Tento článek ukazuje, jak používat předdefinované funkce usnadnění a postup vytvoření vlastní funkce usnadnění.

## <a name="custom-animationscustommd"></a>[Vlastní animace](custom.md)

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Třída je stavebním blokem všechny animace Xamarin.Forms, pomocí metod rozšíření v [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) třídy vytvoří jednu nebo více `Animation` objekty. Tento článek ukazuje, jak používat `Animation` třída k vytvoření a zrušit animací, synchronizaci více animací a vytvořit vlastní animace, které použije animaci vlastnosti, které nejsou animované existující metodami animace.

