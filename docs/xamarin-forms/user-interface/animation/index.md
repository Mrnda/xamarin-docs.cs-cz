---
title: Animace v Xamarin.Forms
description: Xamarin.Forms obsahuje svou vlastní animace infrastrukturu, která je pro vytvoření jednoduché animace, a také byla dostatečně všestranné k vytvoření složitých animace přitom jednoduché.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998289"
---
# <a name="animation-in-xamarinforms"></a>Animace v Xamarin.Forms

_Xamarin.Forms obsahuje svou vlastní animace infrastrukturu, která je pro vytvoření jednoduché animace, a také byla dostatečně všestranné k vytvoření složitých animace přitom jednoduché._

Třídy animace Xamarin.Forms určená různé vlastnosti objektu vizuální prvky typické animace postupně změnou vlastnosti z jednu hodnotu na jiný po určitou dobu. Všimněte si, že neexistuje žádná rozhraní XAML pro třídy animace Xamarin.Forms. Nicméně, animace, lze zapouzdřit [chování](~/xamarin-forms/app-fundamentals/behaviors/index.md) a pak na něj odkazovat z XAML.

## <a name="simple-animationssimplemd"></a>[Jednoduché animace](simple.md)

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Třída poskytuje metody rozšíření, které lze použít k vytvoření jednoduché animace, které otočit, škálování, překlad a fade [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) instancí. Tento článek popisuje vytvoření a rušení animací `ViewExtensions` třídy.

## <a name="easing-functionseasingmd"></a>[Funkce uvolnění](easing.md)

Zahrnuje Xamarin.Forms [ `Easing` ](xref:Xamarin.Forms.Easing) třídu, která vám umožní určit přenos funkci, která určuje, jak urychlit animace nebo zpomalit běží. Tento článek ukazuje, jak využívat předem definovaných funkcí usnadnění a jak vytvořit vlastní funkcí usnadnění.

## <a name="custom-animationscustommd"></a>[Vlastní animace](custom.md)

[ `Animation` ](xref:Xamarin.Forms.Animation) Třída je stavebním blokem všech animací Xamarin.Forms, pomocí metody rozšíření v [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) třídy vytvoří jednu nebo více `Animation` objekty. Tento článek popisuje způsob použití `Animation` třída pro vytvoření a zrušit animace, synchronizaci více animací a vytvoření vlastních animací, které animovat vlastnosti, které nejsou animované existující metody animace.
