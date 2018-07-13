---
title: Použití stylů XAML u aplikací Xamarin.Forms
description: Tato příručka vysvětluje, jak přizpůsobit vzhled aplikace Xamarin.Forms pomocí stylů XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 5145572b30302e58c36250fff40e8b637fcd221f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995072"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Použití stylů XAML u aplikací Xamarin.Forms

## <a name="introductionintroductionmd"></a>[Úvod](introduction.md)

Aplikace Xamarin.Forms často obsahují více ovládacích prvků, které mají stejné vzhled. Nastavení vzhledu jednotlivých ovládacích prvků může být opakované a náchylné k chybám. Místo toho styly lze vytvořit, které přizpůsobení vzhledu ovládacího prvku tak, že seskupování a nastavení vlastností dostupných pro typ ovládacího prvku.

## <a name="explicit-stylesexplicitmd"></a>[Explicitní styly](explicit.md)

*Explicitní* style je ten, který je selektivně použít u ovládacích prvků tak, že nastavíte jejich [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnosti.

## <a name="implicit-stylesimplicitmd"></a>[Implicitní styly](implicit.md)

*Implicitní* style je ten, který používá všechny ovládací prvky stejného [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), aniž by každý ovládací prvek tak, aby odkazovaly styl.

## <a name="global-stylesapplicationmd"></a>[Globální styly](application.md)

Styly může být k dispozici globálně jejich přidáním do vaší aplikace [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). To pomáhá vyhnout duplikaci styly napříč stránek a ovládacích prvků.

## <a name="style-inheritanceinheritancemd"></a>[Dědičnost stylů](inheritance.md)

Styly může dědit z jiné styly snížit duplikace a umožňují opakované použití.

## <a name="dynamic-stylesdynamicmd"></a>[Dynamické styly](dynamic.md)

Styly není reagovat na změny vlastností a zůstanou nezměněny po dobu trvání aplikace. Ale aplikace reagovat na změny stylu dynamicky za běhu pomocí dynamické prostředky.

## <a name="device-stylesdevicemd"></a>[Styly zařízení](device.md)

Xamarin.Forms obsahuje šest *dynamické* styly, označované jako *zařízení* styly, v [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) třídy. Všech šest styly lze použít u [ `Label` ](xref:Xamarin.Forms.Label) pouze instance.
