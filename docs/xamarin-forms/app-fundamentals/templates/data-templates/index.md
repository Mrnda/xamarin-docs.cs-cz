---
title: Šablony dat Xamarin.Forms
description: Šablonu DataTemplate slouží k určení vzhledu data na podporovaných ovládacích prvků a obvykle sváže data mají být zobrazeny.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994968"
---
# <a name="xamarinforms-data-templates"></a>Šablony dat Xamarin.Forms

_Šablonu DataTemplate slouží k určení vzhledu data na podporovaných ovládacích prvků a obvykle sváže data mají být zobrazeny._

## <a name="introductionintroductionmd"></a>[Úvod](introduction.md)

Xamarin.Forms datové šablony umožňují definovat prezentaci dat pro podporované ovládací prvky. Tento článek obsahuje úvod do šablon dat, posouzení důvod, proč jsou nezbytné.

## <a name="creating-a-datatemplatecreatingmd"></a>[Vytvořit šablonu DataTemplate](creating.md)

Datové šablony lze vytvořit jako vložené, v [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), nebo z vlastního typu nebo odpovídající typ buňky Xamarin.Forms. Vložená šablona by měla použít, pokud není nutné opakovaně používat šablony jinde. Alternativně můžete použít opakovaně datové šablony tak, že definujete ho jako vlastní typ, nebo jako ovládací prvek úrovni úrovni stránky nebo aplikace na úrovni prostředků.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Vytváření DataTemplateSelector](selector.md)

A [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) můžete použít k výběru [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) za běhu na základě hodnoty vlastnosti vázané na data. To umožňuje více `DataTemplate` instance, které chcete použít pro stejný typ objektu, pro přizpůsobení vzhledu konkrétní objekty. Tento článek ukazuje, jak vytvářet a využívat `DataTemplateSelector`.


## <a name="related-links"></a>Související odkazy

- [Datové šablony (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
