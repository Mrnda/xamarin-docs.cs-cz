---
title: 'Xamarin.Essentials: řešení potíží'
description: Tento dokument popisuje postupy řešení potíží vzniklých při vývoji s knihovnou Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 3dba315aec2475cb334110ba7555f773f4165aa1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066731"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: řešení potíží

![Předběžné verze NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Chyba: Pro Xamarin.Android.Support.Compat zjištěn konflikt verzí

K následující chybě může dojít při aktualizaci balíčků NuGet (nebo přidání nového balíčku) s Xamarin.Forms projekt, který používá Xamarin.Essentials:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.7.0.17-preview -> Xamarin.Android.Support.CustomTabs 27.0.2 -> Xamarin.Android.Support.Compat (= 27.0.2) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Problém je neodpovídající závislosti pro dvě NuGets. To lze vyřešit tak, že ručně přidáte konkrétní verze závislosti (v tomto případě **Xamarin.Android.Support.Compat**), může podporovat.

K tomuto účelu NuGet, který je zdrojem konfliktu ručně přidat a používat **verze** seznamu můžete vybrat konkrétní verzi. Verze 27.0.2 Xamarin.Android.Support.Compat NuGet aktuálně bude tuto chybu vyřešit.

Odkazovat na [tomto příspěvku na blogu](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) Další informace a video o tom, jak problém vyřešit.

Pokud narazíte na všechny problémy nebo najít chyby prosím oznamte na [úložiště Xamarin.Essentials GitHub](http://github.com/xamarin/Essentials).
