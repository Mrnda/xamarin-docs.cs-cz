---
title: 'Xamarin.Essentials: řešení potíží'
description: Tento dokument popisuje postupy řešení problémů při vývoji s knihovnou Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 8cb18ab029d2fd161c60fda7e130f319b8f0c3ab
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947371"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: řešení potíží

![Předběžné verze NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Chyba: Pro Xamarin.Android.Support.Compat se zjistil konflikt verzí

Při aktualizaci balíčků NuGet, může dojít k následující chybě (nebo přidání nového balíčku) s projektem Xamarin.Forms, která používá Xamarin.Essentials:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Problém je neodpovídající závislosti pro dva balíčky Nuget. To můžete vyřešit tak, že ručně přidáte konkrétní verzi závislosti (v tomto případě **Xamarin.Android.Support.Compat**), který může podporovat.

K tomuto účelu balíčku NuGet, který je zdrojem konflikt ručně přidat a používat **verze** seznamu vybrat konkrétní verzi. Verze 27.0.2.1 Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet nyní bude tuto chybu vyřešit.

Odkazovat na [tento příspěvek na blogu](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) pro další informace a video o tom, jak problém vyřešit.

Pokud narazíte na jakékoli problémy nebo Najít chybu prosím zprávu o [úložiště Xamarin.Essentials GitHub](http://github.com/xamarin/Essentials).
