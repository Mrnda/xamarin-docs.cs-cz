---
title: Účinky Xamarin.Forms
description: Účinky Povolit nativní ovládací prvky na jednotlivých platformách k přizpůsobení bez nutnosti uchýlit se k implementaci vlastní zobrazovací jednotky.
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994455"
---
# <a name="xamarinforms-effects"></a>Účinky Xamarin.Forms

_Xamarin.Forms uživatelského rozhraní jsou vykreslovány pomocí nativní ovládací prvky cílové platformy, povolení aplikace Xamarin.Forms pro zachování odpovídající vzhled a chování pro každou platformu. Účinky Povolit nativní ovládací prvky na jednotlivých platformách k přizpůsobení bez nutnosti uchýlit se k implementaci vlastní zobrazovací jednotky._

## <a name="introduction-to-effectsintroductionmd"></a>[Úvod do efekty](introduction.md)

Účinky Povolit nativní ovládací prvky na jednotlivých platformách přizpůsobit a jsou obvykle používány pro používání stylů pro malé změny. Tento článek obsahuje úvod do efekty, popisuje hranici mezi efekty a vlastní renderery a popisuje `PlatformEffect` třídy.

## <a name="creating-an-effectcreatingmd"></a>[Vytvoření efektu](creating.md)

Účinky zjednodušit vlastní nastavení ovládacího prvku. Tento článek popisuje způsob vytvoření efektu změní barvu pozadí [ `Entry` ](xref:Xamarin.Forms.Entry) řídit, kdy ovládací prvek získá fokus.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Předávání parametrů efektu](passing-parameters/index.md)

Vytvoření efekt, který je nakonfigurovaný prostřednictvím parametrů umožňuje efekt znovu použít. Tyto články ukazují použití vlastností pro předání parametrů do efekt a změna parametrů běhu.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Vyvolávání událostí z efektu](touch-tracking.md)

Účinky může vyvolat události. Tento článek ukazuje, jak vytvořit událost, která implementuje sledování nízké úrovně vícebodový dotyk prstu a signály pro dotykové ovládání stisknutí, přesunutí a verze aplikace.
