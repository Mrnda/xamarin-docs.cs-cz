---
title: Obecné nejčastější dotazy
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.openlocfilehash: b81f5c09ea06acf87449448f43b6c0474a6737b0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917470"
---
# <a name="general-frequently-asked-questions"></a>Obecné nejčastější dotazy

## <a name="portable-class-libraries"></a>Knihovny přenosných tříd
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Jak si můžu zobrazit, které knihovny se podporují v PCL?](pcl-support-libraries.md)
Tento průvodce popisuje prostředky a metody k určení toho, jestli existující knihovnu podporuje různé cílové platformy PCL nebo může být převeden na PCL profilu.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[Rozhraní PCL Reflection API](pcl-reflection.md)
Microsoft vyvinul nové rozhraní API reflexe pro použití v přenosné knihovny tříd. Pokud máte některé existující reflexe kód, který chcete přesunout do PCL, nemusí fungovat.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Případová studie PCL: Jak můžu řešit problémy související se System.Diagnostics.Tracing pro balíček NuGet Microsoft TPL Dataflow?](pcl-case-study.md)
Xamarin.iOS a Xamarin.Android neimplementují 100 % každých PCL profilu, který povolí jako odkazy. Projekty Xamarin pro praktické usnadnění práce v sadě Visual Studio pro Mac, Visual Studio a Správce balíčků NuGet povolí několik profilů, které mají pouze nedokončené implementace. Například Xamarin.iOS ani Xamarin.Android aktuálně obsahuje kompletní implementace typů v `System.Diagnostics.Tracing` PCL oboru názvů. Můžete obejít tím přepnutím projekt aplikace tak, aby odkazovaly přenositelností net45 + win8 + wp8 + wpa81 verzi toku dat TPL knihovny.

## <a name="nuget-packages--xamarin-components"></a>Balíčky NuGet & Xamarin součásti
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Jak můžu aktualizovat NuGet?](nuget-update.md)
Aktualizace NuGet, rozšíření a doplňky naleznete v části **aktualizace** ve **Správce balíčků NuGet**. Podrobné navigace na vyhledat aktualizace v sadě Visual Studio pro Mac & Visual Studio je v této příručce.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Jak můžu přejít na starší verzi balíčku NuGet?](nuget-package-downgrade.md)
Visual Studio pro Mac & Visual Studio k dispozici funkce pro výběr starších verzí balíčků a jejich instalace automaticky. Podobně jako u jak aktualizace balíčků.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Po aktualizaci balíčků NuGet dochází k chybě kvůli chybějícím balíčkům](nuget-packages-missing.md)
Potíže hlášené především na platformě Xamarin.Forms ukázkové aplikace řešení, ale potenciální potíže se může stát při jakékoli projekt, který používá balíčky NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Sjednocující komponenty Google Play Services a NuGet](gps-components-nuget.md)
Použít existuje několik Google Play Services součásti a balíčky NuGet, ale aby usnadnily pro vývojáře, jsme jste nyní unified naše součásti a NuGet balíčky do dvou. Téměř vždy třeba použít služby Google Play. Použití balíčku (Froyo) je pouze pokud aktivně cílíte Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Kde na mém počítači jsou komponenty uložené?](component-storage.md)
Vždy, když instalujete součást Xamarin do projektu aplikace, získá umístěny ve dvou umístěních uvedených v této příručce.


## <a name="troubleshooting"></a>Poradce při potížích
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Kde najdu informace o mojí verzi a protokoly?](version-logs.md)
Tento průvodce detailně kde najít většina diagnostické informace, které lze použít k řešení potíží Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Kdy a jak mám ohlásit chybu?](howto-file-bug.md)
Tato příručka obsahuje typy pro vyplnění sestavy chyb vysoce kvalitní tak, aby naše technici jsou možné určit příčinu (a všechny potenciální opravy) pro problém efektivněji.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Proč Xamarin nepodporuje Jenkins?](xamarin-jenkins.md)
Volaných je sada CI open-source; z důvodu tohoto počtu problémy, které jsou přímo způsobeny volaných *samotné* bude muset být vytvářené jako problémy, na které jste získali kód; například [hlavní úložišti volaných](https://github.com/jenkinsci/jenkins), nebo úložišti pro [ Jenkins.App](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Jaká nastavení projektu vyžaduje ladicí program?](debugger-settings.md)
Aby ladicí program na fungovat podle očekávání (přístupů zarážky, protokoly zobrazení ladění atd.) zobrazení informací instrumentace a ladění vývojáře obě povoleny. Tento průvodce detailně způsob vyhledání a aktivaci těchto nastavení.

