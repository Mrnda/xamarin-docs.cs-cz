---
title: Styly
description: "Přizpůsobení vzhledu pomocí styly"
ms.topic: article
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 934948579e5f3fb19c7afe49f4e86a1ef255b77f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="styles"></a>Styly

## <a name="introductionintroductionmd"></a>[Úvod](introduction.md)

Xamarin.Forms aplikace často obsahují více ovládacích prvků, které mají stejné vzhled. Může být nastavení vzhled jednotlivých ovládacích prvků opakovaných a chyba náchylné k chybám. Místo toho styly lze vytvořit které přizpůsobení vzhledu ovládacího prvku pomocí seskupení a nastavení vlastnosti, které jsou k dispozici na typ ovládacího prvku.

## <a name="explicit-stylesexplicitmd"></a>[Explicitní styly](explicit.md)

*Explicitní* styl je ten, který je selektivně použít u ovládacích prvků nastavením jejich [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnosti.

## <a name="implicit-stylesimplicitmd"></a>[Implicitní styly](implicit.md)

*Implicitní* styl je ten, který se používá ve všech ovládacích prvků na stejné [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), bez nutnosti každý ovládací prvek tak, aby odkazovaly styl.

## <a name="global-stylesapplicationmd"></a>[Globální styly](application.md)

Styly může být k dispozici globálně jejich přidáním do aplikace [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). To pomáhá zamezit duplicitních stylů stránky a ovládací prvky.

## <a name="style-inheritanceinheritancemd"></a>[Styl dědičnosti](inheritance.md)

Styly může dědit vlastnosti z jiných styly snížit duplikování a povolit opakované použití.

## <a name="dynamic-stylesdynamicmd"></a>[Dynamické styly](dynamic.md)

Styly nezadávejte reagovat na změny vlastností a zůstanou nezměněny po dobu trvání aplikace. Ale aplikace reagovat na změny styl dynamicky za běhu pomocí dynamické prostředky.

## <a name="device-stylesdevicemd"></a>[Styly zařízení](device.md)

Xamarin.Forms obsahuje šest *dynamické* styly, označované jako *zařízení* styly v [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) třídy. Všech šest styly mohou být použity k [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) pouze instance.