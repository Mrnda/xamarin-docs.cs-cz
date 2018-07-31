---
title: Obecné – nejčastější dotazy
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 06aa6569301d1bfdbf9f6fd1e7397a38a9beb6f6
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350817"
---
# <a name="general-frequently-asked-questions"></a>Obecné – nejčastější dotazy

## <a name="portable-class-libraries"></a>Přenosné knihovny tříd

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Jak si můžu zobrazit, které knihovny se podporují v PCL?](pcl-support-libraries.md)
Tento průvodce popisuje prostředky a metody pro zjištění, zda vaše stávající knihovny podporuje různé cílové platformy PCL nebo může být převeden na profilem PCL.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[Rozhraní PCL Reflection API](pcl-reflection.md)
Společnost Microsoft vyvinula nové rozhraní API reflexe pro použití v přenosné knihovny tříd. Pokud máte existující reflexe kódu, který chcete přesunout do PCL, nemusí fungovat.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Případová studie PCL: Jak můžu řešit problémy související se System.Diagnostics.Tracing pro balíček NuGet Microsoft TPL Dataflow?](pcl-case-study.md)
Xamarin.iOS a Xamarin.Android neimplementují 100 % každý profilem PCL umožňující jako odkazy. Pro usnadnění praktické práce v sadě Visual Studio pro Mac, Visual Studio a Správce balíčků NuGet projekty Xamarin umožňuje použít několik profilů, které mají pouze implementací neúplná. Například Xamarin.iOS ani Xamarin.Android aktuálně obsahuje úplnou implementaci typů v `System.Diagnostics.Tracing` PCL oboru názvů. Můžete alternativně vyřešit pomocí přepínání projektu aplikace odkaz portable-net45 + win8 + wp8 + wpa81 verzi Knihovna TPL datového toku.

## <a name="nuget-packages--xamarin-components"></a>Balíčky NuGet a komponenty Xamarin
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Jak můžu aktualizovat NuGet?](nuget-update.md)
NuGet aktualizací, rozšíření a doplňky najdete v části **aktualizace** kartu **Správce balíčků NuGet**. Podrobné navigaci na vyhledat aktualizace v sadě Visual Studio pro Mac a Visual Studio je v této příručce.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Jak můžu přejít na starší verzi balíčku NuGet?](nuget-package-downgrade.md)
Visual Studio pro Mac a Visual Studio k dispozici funkce pro starší verze balíčků výběru a instalace je automaticky. Podobně jako způsob aktualizace balíčků.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Po aktualizaci balíčků NuGet dochází k chybě kvůli chybějícím balíčkům](nuget-packages-missing.md)
Tento problém především byla oznámena v řešení Xamarin.Forms ukázkové aplikace, ale potenciál k tomuto problému může dojít pro žádný projekt, který používá balíčky NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Sjednocující komponenty Google Play Services a NuGet](gps-components-nuget.md)
Použít existuje několik Google Play Services součásti a balíčky NuGet, ale aby to bylo jednodušší pro vývojáře, jsme jste nyní unified naše komponenty a NuGet na dva balíčky. V téměř všech případech by měla sloužit služby Google Play. Jediným důvodem pro použití balíčku (Froyo) je, pokud aktivně cílíte Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Kde na mém počítači jsou komponenty uložené?](component-storage.md)
Pokaždé, když se do projektu aplikace nainstalujete komponenty Xamarin, získá umístěny ve dvou umístěních uvedených v této příručce.


## <a name="troubleshooting"></a>Poradce při potížích
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Kde najdu informace o mojí verzi a protokoly?](version-logs.md)
Tato příručka podrobně popisuje, kde najít většina diagnostických informací, které slouží k řešení potíží s problémy s Xamarinem.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Kdy a jak mám ohlásit chybu?](howto-file-bug.md)
Tato příručka obsahuje tipy pro vyplnění sestavy chyb vysoce kvalitní, tak, aby naši technici budou moct určit příčinu (a všechny možné opravy) problému efektivněji.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Proč Xamarin nepodporuje Jenkins?](xamarin-jenkins.md)
Jenkins je open source sada CI; z důvodu tolik problémů, které jsou přímo způsobeny Jenkinse *samotného* bude muset být zaznamenávané jako problémy, proti které jste získali kód; například [hlavním úložišti Jenkins](https://github.com/jenkinsci/jenkins), nebo úložiště pro [ Jenkins.App](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Jaká nastavení projektu vyžaduje ladicí program?](debugger-settings.md)
Aby ladicí program fungovat podle očekávání (průchodů zarážky, zobrazit protokoly ladění atd.) zobrazené informace pro vývojáře pro instrumentaci a ladění musí být povoleny. Tato příručka podrobně popisuje, jak najít a aktivaci tato nastavení.

