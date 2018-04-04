---
title: Ruční vytvoření balíčků NuGet pro Xamarin
description: Tato stránka obsahuje některé tipy k sestavení balíčky NuGet, které cílí na platformě Xamarin.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: ce8003c862205ac80dfea2095009562ae7ebb23f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Ruční vytvoření balíčků NuGet pro Xamarin

_Tato stránka obsahuje některé tipy k sestavení balíčky NuGet, které cílí na platformě Xamarin._

> [!NOTE]
> Xamarin Studio 6.2 (a Visual Studio pro Mac) zahrnuje schopnost _automaticky_ generování balíčků NuGet z PCL, .NET Standard nebo sdílených projektů. Odkazovat [Multiplatform knihovny pro sdílení kódu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) průvodce další podrobnosti.

## <a name="nuget-package-xamarin-profiles"></a>Balíček NuGet Xamarin profily

Web NuGet [podpora více verzí rozhraní .NET Framework a profily](https://docs.nuget.org/create/enforced-package-conventions) popisuje, jak pro podporu různých architektury Microsoft a profilů, ale nezahrnuje názvy cílový framework používané Xamarin.

Hlavní Xamarin cílové rozhraní používá dnes jsou:

* **MonoAndroid** -Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [unifikované API](~/cross-platform/macios/unified/index.md) (podporuje 64bitová verze)
* **Xamarin.Mac** -Xamarin.Mac na mobilní profil, který je ekvivalentní prostor pro Xamarin.iOS a rozhraní API Xamarin.Android.

Je také cíle pro starší iOS [klasické rozhraní API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -klasické rozhraní API pro iOS

A **příponou .nuspec** souborů, které všechny tyto cílem by vypadat podobně jako:

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

Výše ignoruje všechny knihovny přenosných tříd.

Většina **příponou .nuspec** soubory zadejte číslo verze rozhraní target Framework, ale je nepovinný, pokud vaše sestavení funguje se všemi verzemi této cílové rozhraní. Pokud vaše target byla **lib\MonoAndroid** by to znamenat, funguje s jakoukoli verzi nástroje Xamarin.Android.

Můžete zadat verzi sady čísel bez desetinné čárky nebo můžete zadat pomocí desetinných míst. Bez desetinné čárky NuGet právě vezme všechna čísla a zapněte ji do verze vložením '.' mezi každou číslici.

V výše uvedené "MonoAndroid10" znamená "Android 1.0". To znamená právě projektu [cílové rozhraní](~/android/app-fundamentals/android-api-levels.md) musí být MonoAndroid verze 1.0 nebo vyšší. Verze je uveden v `<TargetFrameworkVersion>` element v souboru projektu.

O vysvětlení:

- **MonoAndroid403** odpovídá Android 4.0.3 a novějších (ie API úrovně 15)
- **Xamarin.iOS10** odpovídá Xamarin.iOS 1.0 a novější
- **Xamarin.iOS1.0** také odpovídající Xamarin.iOS 1.0 a novější


## <a name="pcl-nugets-with-platform-dependencies"></a>PCL NuGets závislosti platformy

PCL profily mají omezenou jaké rozhraní .NET framework získají přístup k rozhraní API a určitě nemůžou získat přístup kódu pro konkrétní platformu. Tyto odkazy 3. stran popisují různé přístupy k vytváření balíčků NuGet, které používají PCL a nativního rozhraní API k zajištění kompatibility pro Xamarin a jiné platformy:

- [Postup pro vás provedl pracovní knihovny přenosných tříd](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Efektu PCL návnada a přepínače](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Vytvoření NuGet PCL, který funguje s Xamarin.iOS](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

Tato externí [seznam profilů PCL s jejich název cílové NuGet](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) je také užitečné referenční.

## <a name="examples"></a>Příklady

Některé příklady open source, které mohou odkazovat na:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) – zápis vaší aplikace pomocí System.Net.Http, ale vyřadit této knihovně a přejde se výrazně rychlejší (zobrazení [zdroj](https://github.com/paulcbetts/ModernHttpClient)).
- [**Splat** ](https://www.nuget.org/packages/Splat/) – knihovnu aby věcí a platformy, která má být (zobrazení [zdroj](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -knihovnu křížové platformy pro vykreslování vektorová grafika na rozhraní .NET (zobrazení [zdroj](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).


## <a name="related-links"></a>Související odkazy

- [Nugetizer 3000 automatizovat vytváření Nuget](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [Aktualizace NuGets pro iOS 64-bit](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [Včetně NuGet ve vašem projektu](/visualstudio/mac/nuget-walkthrough/index.md)
