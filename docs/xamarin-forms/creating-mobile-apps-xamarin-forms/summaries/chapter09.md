---
title: "Souhrn kapitoly 9. Volání rozhraní API podle platformy"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 637096d3ebb7fb90321f7f459e0ca9e51572d935
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Souhrn kapitoly 9. Volání rozhraní API podle platformy

Někdy je nutné spouštět nějaký kód, který se liší podle platformy. Tato kapitola prozkoumá technik.

## <a name="preprocessing-in-the-shared-asset-project"></a>Předběžné zpracování do sdílené položky projektu

Xamarin.Forms sdílený prostředek projektu může spustit jinou kód pro každou platformu pomocí jazyka C# direktivy preprocesoru `#if`, `#elif`, a `endif`. Tento postup je znázorněn v [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Trojitá snímek proměnné formátu odstavce](images/ch09fg01-small.png "Model zařízení a operační systém")](images/ch09fg01-large.png "Model zařízení a operační systém")

Kód výsledné však může být ugly a obtížné číst.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Paralelní třídy v projektu sdíleného prostředku

Více strukturovaný přístup k provádění specifických pro platformy kódu v systému SAP ukazují [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) ukázka. Všechny projekty platformy obsahuje stejně jako s názvem třídy a metody, ale implementována pro konkrétní platformu. SAP pak jednoduše vytvoří instanci třídy a volá metodu.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService a knihovny přenosných tříd

Knihovnu nemůže získat přístup obvykle třídy v projektu aplikace. Toto omezení zdá se, že aby se zabránilo technika ukazuje **PlatInfoSap2** nelze použít ve PCL. Však obsahuje Xamarin.Forms třídy s názvem [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) k veřejné třídy v projektu aplikace z PCL používající reflexe .NET.

Musí definovat PCL `interface` se členy, je nutné použít u různých platforem. Potom všechny platformy obsahují implementace tohoto rozhraní. Musí být označeny třídu, která implementuje rozhraní [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) na úrovni sestavení.

PCL pak použije obecná [ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) metodu `DependencyService` k získání instance třídy platformu, která implementuje rozhraní.

Tento postup je znázorněn v [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) ukázka.

## <a name="platform-specific-sound-generation"></a>Zvukové generování specifických pro platformy

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) ukázka přidá signálů k **MonkeyTap** program díky přístupu k zařízení pro zvuk generování u různých platforem.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 9 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Ukázky kapitoly 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Závislosti služby](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
