---
title: "Obecné nejčastější dotazy"
ms.topic: article
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 0c01bfb08d525fd336b29f405304efaeaf749f35
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="general-frequently-asked-questions"></a>Obecné nejčastější dotazy

## <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate
### <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarinvisualstudio-2017-rcmd"></a>[Můžete použít Visual Studio 2017 Release Candidate s Xamarinem?](visualstudio-2017-rc.md)
Popis aktuální důsledky použití Xamarin s Visual Studio 2017 Release Candidate (RC) stejně jako informace o tom, jak nainstalovat Xamarin ve Visual Studio2017 RC.

## <a name="portable-class-libraries"></a>Knihovny přenosných tříd
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Jak lze zobrazit v PCL jsou podporované jaké knihovny?](pcl-support-libraries.md)
Tento průvodce popisuje prostředky a metody k určení toho, jestli existující knihovnu podporuje různé cílové platformy PCL nebo může být převeden na PCL profilu.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[Reflexe PCL rozhraní API](pcl-reflection.md)
Microsoft vyvinul nové rozhraní API reflexe pro použití v přenosné knihovny tříd. Pokud máte některé existující reflexe kód, který chcete přesunout do PCL, nemusí fungovat.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Případová studie PCL: jak můžete vyřešit problémy související s System.Diagnostics.Tracing pro balíček NuGet toku dat TPL Microsoft?](pcl-case-study.md)
Xamarin.iOS a Xamarin.Android neimplementují 100 % každých PCL profilu, který povolí jako odkazy. Projekty Xamarin pro praktické usnadnění práce v sadě Visual Studio pro Mac, Visual Studio a Správce balíčků NuGet povolí několik profilů, které mají pouze nedokončené implementace. Například Xamarin.iOS ani Xamarin.Android aktuálně obsahuje kompletní implementace typů v `System.Diagnostics.Tracing` PCL oboru názvů. Můžete obejít tím přepnutím projekt aplikace tak, aby odkazovaly přenositelností net45 + win8 + wp8 + wpa81 verzi toku dat TPL knihovny.

## <a name="nuget-packages--xamarin-components"></a>Balíčky NuGet & Xamarin součásti
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Jak můžete aktualizovat NuGet?](nuget-update.md)
Aktualizace NuGet, rozšíření a doplňky naleznete v části **aktualizace** ve **Správce balíčků NuGet**. Podrobné navigace na vyhledat aktualizace v sadě Visual Studio pro Mac & Visual Studio je v této příručce.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Jak se downgradovat balíček NuGet?](nuget-package-downgrade.md)
Visual Studio pro Mac & Visual Studio k dispozici funkce pro výběr starších verzí balíčků a jejich instalace automaticky. Podobně jako u jak aktualizace balíčků.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Chybějící balíčky chyba po aktualizaci balíčků Nuget](nuget-packages-missing.md)
Potíže hlášené především na platformě Xamarin.Forms ukázkové aplikace řešení, ale potenciální potíže se může stát při jakékoli projekt, který používá balíčky NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Sjednotit Google Play Services součásti a NuGet](gps-components-nuget.md)
Použít existuje několik Google Play Services součásti a balíčky NuGet, ale aby usnadnily pro vývojáře, jsme jste nyní unified naše součásti a NuGet balíčky do dvou. Téměř vždy třeba použít služby Google Play. Použití balíčku (Froyo) je pouze pokud aktivně cílíte Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Kde jsou uloženy součásti na můj počítač?](component-storage.md)
Vždy, když instalujete součást Xamarin do projektu aplikace, získá umístěny ve dvou umístěních uvedených v této příručce.


## <a name="troubleshooting"></a>Poradce při potížích
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Kde najdu Moje informace o verzi a protokoly?](version-logs.md)
Tento průvodce detailně kde najít většina diagnostické informace, které lze použít k řešení potíží Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Kdy a jak by měla I souboru sestavy chyb?](howto-file-bug.md)
Tato příručka obsahuje typy pro vyplnění sestavy chyb vysoce kvalitní tak, aby naše technici jsou možné určit příčinu (a všechny potenciální opravy) pro problém efektivněji.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Proč není volaných Xamarin podporuje?](xamarin-jenkins.md)
Volaných je sada CI open-source; z důvodu tohoto počtu problémy, které jsou přímo způsobeny volaných *samotné* bude muset být vytvářené jako problémy, na které jste získali kód; například [hlavní úložišti volaných](https://github.com/jenkinsci/jenkins), nebo úložišti pro [ Jenkins.App](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Nastavení projektu, které jsou požadovány pro ladicí program?](debugger-settings.md)
Aby ladicí program na fungovat podle očekávání (přístupů zarážky, protokoly zobrazení ladění atd.) zobrazení informací instrumentace a ladění vývojáře obě povoleny. Tento průvodce detailně způsob vyhledání a aktivaci těchto nastavení.
