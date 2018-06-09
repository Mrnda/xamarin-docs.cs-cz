---
title: Lokalizace Xamarin.Forms
description: Předdefinované rozhraní .NET framework lokalizace jde použít k sestavení napříč platformami vícejazyčné aplikace s Xamarin.Forms. Je možné lokalizovat textu a obrázků a aplikace podporují směr toku zprava doleva.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 78731924324a1ddd34c0d197070699e2998c1513
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240590"
---
# <a name="xamarinforms-localization"></a>Lokalizace Xamarin.Forms

_Předdefinované rozhraní .NET framework lokalizace jde použít k sestavení napříč platformami vícejazyčné aplikace s Xamarin.Forms._

## <a name="string-and-image-localizationtextmd"></a>[Lokalizace řetězců a obrázků](text.md)

Předdefinované mechanismus pro lokalizace používá aplikace .NET [RESX – soubory](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) a třídy v `System.Resources` a `System.Globalization` obory názvů. Soubory RESX obsahující přeloženými řetězci, které jsou součástí sestavení Xamarin.Forms, společně s generované kompilátorem třídu, která poskytuje přístup k překlady s silného typu. Pak je možné načíst přeložený text v kódu.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Lokalizace zprava doleva](right-to-left.md)

Směr toku je směr, ve kterém jsou prvky uživatelského rozhraní na stránce skenovalo oko. Lokalizace zprava doleva přidává podporu pro směr toku zprava doleva na platformě Xamarin.Forms aplikace.
