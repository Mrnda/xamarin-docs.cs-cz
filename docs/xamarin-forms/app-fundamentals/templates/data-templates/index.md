---
title: "Šablony dat"
description: "Šablonu DataTemplate slouží k určení vzhledu dat na podporované ovládací prvky a obvykle se váže k dat, který se má zobrazit."
ms.topic: article
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 99a00c98471ae85af2a8cba2e1e52444370a9332
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="data-templates"></a>Šablony dat

_Šablonu DataTemplate slouží k určení vzhledu dat na podporované ovládací prvky a obvykle se váže k dat, který se má zobrazit._

## <a name="introductionintroductionmd"></a>[Úvod](introduction.md)

Šablony dat Xamarin.Forms poskytují možnost definovat prezentaci dat na podporované ovládací prvky. Tento článek obsahuje úvod do šablony dat, prozkoumání, proč jsou zapotřebí.

## <a name="creating-a-datatemplatecreatingmd"></a>[Vytváření šablonu DataTemplate](creating.md)

Data šablony lze vytvořit vložený, v [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), nebo z vlastního typu nebo příslušného typu buňky Xamarin.Forms. Šablonu vložené by měl použijí, pokud není nutné znovu použít šablonu dat jinam. Šablonu dat můžete alternativně znovu použít tak, že definujete jej jako vlastní typ nebo jako prostředek řízení úrovni úrovně stránky nebo na úrovni aplikace.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Vytváření DataTemplateSelector](selector.md)

A [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) je možné vybrat [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) za běhu na základě hodnoty vlastnosti vázané na data. To umožňuje více `DataTemplate` instance, který bude použit na stejný typ objektu, chcete-li přizpůsobit vzhled konkrétní objekty. Tento článek ukazuje, jak vytvářet a využívat `DataTemplateSelector`.


## <a name="related-links"></a>Související odkazy

- [Šablony dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
