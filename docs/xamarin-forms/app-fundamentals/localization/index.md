---
title: Lokalizace Xamarin.Forms
description: Předdefinované rozhraní .NET framework lokalizace jde použít k sestavení napříč platformami vícejazyčné aplikace s Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 29624eeccc8542b3296774f6b77480b664bee478
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="localization"></a>Lokalizace

_Předdefinované rozhraní .NET framework lokalizace jde použít k sestavení napříč platformami vícejazyčné aplikace s Xamarin.Forms._

## <a name="string-and-image-localizationtextmd"></a>[Řetězec a lokalizace bitové kopie](text.md)

Předdefinované mechanismus pro lokalizace používá aplikace .NET [RESX – soubory](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) a třídy v `System.Resources` a `System.Globalization` obory názvů. Soubory RESX obsahující přeloženými řetězci, které jsou součástí sestavení Xamarin.Forms, společně s generované kompilátorem třídu, která poskytuje přístup k překlady s silného typu. Pak je možné načíst přeložený text v kódu.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Lokalizace zprava doleva](right-to-left.md)

Směr toku je směr, ve kterém jsou prvky uživatelského rozhraní na stránce skenovalo oko. Lokalizace zprava doleva přidává podporu pro směr toku zprava doleva na platformě Xamarin.Forms aplikace.
