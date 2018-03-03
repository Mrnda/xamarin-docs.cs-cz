---
title: "Účinek"
description: "Uživatelská rozhraní Xamarin.Forms jsou vykreslovány pomocí nativní ovládací prvky typu cílovou platformu, povolíte Xamarin.Forms aplikací zachovat odpovídající vzhledu a chování pro každou platformu. Účinky povolí nativní ovládací prvky na jednotlivých platformách k přizpůsobení bez nutnosti uchýlit implementaci vlastní zobrazovací jednotky."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: dd8b98982052e15744bf67ece6a25cd940a9875a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="effects"></a>Účinek

_Uživatelská rozhraní Xamarin.Forms jsou vykreslovány pomocí nativní ovládací prvky typu cílovou platformu, povolíte Xamarin.Forms aplikací zachovat odpovídající vzhledu a chování pro každou platformu. Účinky povolí nativní ovládací prvky na jednotlivých platformách k přizpůsobení bez nutnosti uchýlit implementaci vlastní zobrazovací jednotky._

## <a name="introduction-to-effectsintroductionmd"></a>[Úvod do efekty](introduction.md)

Účinky Povolit nativní ovládací prvky na každou platformu, která lze přizpůsobit a jsou obvykle používány pro malé stylů změny. Tento článek obsahuje úvod do důsledky, popisuje hranice mezi efekty a vlastní nástroji pro vykreslování a popisuje `PlatformEffect` třídy.

## <a name="creating-an-effectcreatingmd"></a>[Vytváření efekt](creating.md)

Účinky zjednodušit přizpůsobení ovládacího prvku. Tento článek ukazuje, jak vytvořit efekt změní barvu pozadí [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řídit, kdy ovládací prvek získá fokus.

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[Předávání parametrů pro efekt](passing-parameters/index.md)

Vytváření efektu, který je nakonfigurovaný prostřednictvím parametrů umožňuje účinek znovu použije. Tyto články demonstruje použití vlastnosti předat parametry do vliv a změna parametrů běhu.

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[Vyvolání událostí z efekt](touch-tracking.md)

Účinky vyvolat události. Tento článek ukazuje, jak vytvořit událost, která implementuje sledování nízké úrovně prstem více touch a dá signál aplikaci pro stisknutí dotykového ovládání, přesun a verze.