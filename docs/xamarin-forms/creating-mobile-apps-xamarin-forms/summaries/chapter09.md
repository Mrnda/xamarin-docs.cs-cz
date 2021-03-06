---
title: Souhrn kapitoly 9. Volání rozhraní API pro konkrétní platformu
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 9. Volání rozhraní API pro konkrétní platformu'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: e7b2eea22758155db7d79fa26f3376e16cf16a45
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39157013"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Souhrn kapitoly 9. Volání rozhraní API pro konkrétní platformu

> [!NOTE] 
> Poznámky na této stránce označit oblasti, kde se Xamarin.Forms se rozcházela z materiály uvedené v seznamu.

Někdy je potřeba spouštět nějaký kód, který se liší podle platformy. Tato kapitola popisuje techniky.

## <a name="preprocessing-in-the-shared-asset-project"></a>Předběžné zpracování do sdílené položky projektu

Xamarin.Forms sdílený projekt prostředků můžete provést různý kód pro každou platformu s využitím C# direktivy preprocesoru `#if`, `#elif`, a `endif`. To je patrné [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Trojitá snímek proměnné ve formátu odstavec](images/ch09fg01-small.png "Model zařízení a operační systém")](images/ch09fg01-large.png#lightbox "Model zařízení a operační systém")

Výsledného kódu však lze bez zbytečných prvků a obtížně čitelné.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Paralelní třídy v projektu sdíleného prostředku

Více strukturovaný přístup při provádění kódu specifické pro platformu v systému SAP jsou popsané v článku [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) vzorku. Všechny projekty platformy obsahuje identicky pojmenovanou třídy a metody, ale implementována pro konkrétní platformu. SAP pak jednoduše vytvoří instanci třídy a volá metodu.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService a přenosné knihovny tříd

> [!NOTE] 
> Knihovny přenosných tříd byly nahrazeny knihovny .NET Standard. Ukázkový kód z knihy se převedlo na použití knihovny .NET standard.

Knihovnu nelze přistoupit, obvykle tříd v projektech aplikací. Toto omezení zdá se, že aby se zabránilo techniku ukazuje **PlatInfoSap2** z používán v knihovně. Ale Xamarin.Forms obsahuje třídu s názvem [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) reflexe .NET, která používá pro přístup k veřejné třídy v projektu aplikace z knihovny.

Musíte definovat knihovny `interface` s členy, je třeba použít u různých platforem. Pak každou z platforem obsahuje implementaci rozhraní. Třída, která implementuje rozhraní musí být označena [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) na úrovni sestavení.

Pak používá obecné knihovna [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) metoda `DependencyService` k získání instance třídy platforma, která implementuje rozhraní.

To je patrné [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) vzorku.

## <a name="platform-specific-sound-generation"></a>Generování zvukového specifické pro platformu

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) ukázka přidá signálů k **MonkeyTap** program díky přístupu do zařízení pro generování zvuk u různých platforem.

## <a name="related-links"></a>Související odkazy

- [Kapitola 9 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Ukázky kapitoly 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Závislost služby](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
