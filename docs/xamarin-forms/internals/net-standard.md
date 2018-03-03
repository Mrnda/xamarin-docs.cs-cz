---
title: "Standardní 2.0 podpora v rozhraní .NET v Xamarin.Forms"
description: "Tento článek vysvětluje, jak převést Xamarin.Forms aplikace na používání standardní .NET 2.0."
ms.topic: article
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 7923ccdf85ffcbbc239df9e9df751f561615baa1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Standardní 2.0 podpora v rozhraní .NET v Xamarin.Forms

_Tento článek vysvětluje, jak převést Xamarin.Forms aplikace na používání standardní .NET 2.0._

.NET standard je specifikace rozhraní API .NET, která by měla být k dispozici na všech implementace rozhraní .NET. Ho usnadňuje sdílet kód mezi aplikací klasické pracovní plochy, mobilní aplikace a hry a cloudových služeb, tak, že převedou identické rozhraní API na různých platformách. Informace o platformách podporovaných aplikací .NET Standard najdete v tématu [podpora v rozhraní .NET implementace](/dotnet/standard/net-standard#net-implementation-support/).

.NET standard knihovny jsou nahrazení pro přenosných třída knihovny PCL (). Však knihovnu, která je cílena .NET Standard je stále PCL a se označuje jako PCL se .NET Standard na základě. Některé PCL profily jsou namapované na .NET Standard verze a profilů, které mají mapování, budou moci odkazovat na sebe navzájem typy dvě knihovny. Další informace najdete v tématu [PCL kompatibility](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4 umožňuje aplikacím Xamarin.Forms cílové rozhraní .NET 2.0 standardní nahrazením PCL knihovnu standardní rozhraní .NET 2.0. Toho lze dosáhnout následujícím způsobem:

- Ujistěte se, [.NET Core 2.0](https://www.microsoft.com/net/download/core) je nainstalovaná.
- Aktualizujte řešení Xamarin.Forms, aby používalo Xamarin.Forms 2.4 nebo vyšší.
- Přidejte knihovnu .NET Standard do řešení, které cílí standardní rozhraní .NET 2.0.
- Odstraňte třídu, která je přidána ke knihovně .NET Standard.
- Přidejte balíček NuGet 2.4 (nebo vyšší) Xamarin.Forms ke knihovně .NET Standard.
- V rámci platformy projektu přidejte odkaz na knihovnu .NET Standard a odeberte odkaz na projekt PCL, který obsahuje logiku Xamarin.Forms uživatelského rozhraní.
- Zkopírujte soubory z projektu PCL ke knihovně .NET Standard.
- Odeberte PCL projekt, který obsahuje logiku Xamarin.Forms uživatelského rozhraní.


## <a name="related-links"></a>Související odkazy

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Možnosti sdílení kódu](~/cross-platform/app-fundamentals/code-sharing.md)
