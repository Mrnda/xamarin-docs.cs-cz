---
title: Profilace aplikací pro Android
description: Tato příručka vysvětluje, jak používat profileru nástroje pro zjištění výkonu a využití paměti aplikace pro Android.
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C823FEE-A6F6-4C31-9EB6-E51407A2FD8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/03/2018
ms.openlocfilehash: e62ac290423db1c18e7e50d55b2b3550f99d1533
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34549276"
---
# <a name="profiling-android-apps"></a>Profilace aplikací pro Android

Před nasazením aplikace na obchod s aplikacemi, je důležité poznat a opravit všechny kritické body, problémy využití příliš mnoho paměti nebo neefektivní používání síťových prostředků. Dva profileru nástroje jsou k dispozici pro tento účel:

-  Xamarin Profiler 
-  Android profileru v Android studiu

Tento průvodce uvádí profileru Xamarin a poskytuje podrobné informace o Začínáme s používáním Android profileru.

 
## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin profileru je samostatná aplikace, které jsou integrovány v sadě Visual Studio a Visual Studio pro Mac pro profilace aplikace Xamarin z prostředí IDE. Další informace o používání profileru Xamarin najdete v tématu [Xamarin profileru](~/tools/profiler/index.md).

> [!NOTE]
> Musí být [Visual Studio Enterprise](https://www.visualstudio.com/vs/compare/) odběratele k odemknutí funkci Xamarin profileru v buď Visual Studio Enterprise v systému Windows nebo Visual Studio for Mac.
 
## <a name="android-studio-profiler"></a>Android Studio profileru

Android Studio 3.0 a novějších zahrnuje nástroj na Android profileru. Android profileru můžete použít k měření výkonu Xamarin Android aplikace vytvořené s nástroji Visual Studio &ndash; bez nutnosti licenci Visual Studio Enterprise. Na rozdíl od profileru Xamarin, ale Android profileru není integrovaná pomocí sady Visual Studio a slouží pouze k profilu balíček aplikace pro Android (APK), který byl předem vytvořené a importovat Android profileru.

### <a name="launching-a-xamarin-android-app-in-android-profiler"></a>Spuštění aplikace Xamarin Android v Android profileru

Následující kroky popisují, jak spuštění aplikace Xamarin Android v nástroji Android Studio Android profileru. V příkladu snímky obrazovky níže, Xamarin Forms [XamagonXuzzle](https://developer.xamarin.com/samples/mobile/LivePlayer/XamagonXuzzleLP/) aplikace je vytvořená a profilovaným pomocí Android profileru:

1.  Možnosti sestavení v projektu pro Android, zakažte **použití sdílených Runtime**. Tím se zajistí, že balíček aplikace pro Android (APK) je vytvořena bez závislosti na sdílený modul runtime Mono vývoj čas.

    ![Zákaz použití sdílený modul Runtime](profiling-images/vswin/01-turn-off-shared-runtime.png)

2.  Vytváření aplikace pro **ladění** a nasadíte ho do fyzického zařízení nebo emulátor. To způsobí, že přihlášeni **ladění** verzi APK má být sestaven.
    Pro **XamagonXuzzle** příkladu je s názvem výsledné APK **com.companyname.XamagonXuzzle Signed.apk**.

3.  Otevřete složku projekt a přejděte do **bin/Debug**. V této složce, vyhledejte **Signed.apk** verze aplikace a zkopírujte jej do pohodlně přístupném místě (například stolní počítače). Na následujícím snímku obrazovky APK **com.companyname.XamagonXuzzle Signed.apk** je umístěn a zkopírují se na ploše:

    [![Umístění ladění podepsaný soubor APK](profiling-images/vswin/02-locating-the-debug-apk-sml.png)](profiling-images/vswin/02-locating-the-debug-apk.png#lightbox)

4.  Spustit Android Studio a vyberte **profil nebo ladění APK**:

    ![Úvodní obrazovka Android Studio od profileru](profiling-images/vswin/03-android-studio.png)

5.  V **vyberte soubor APK** dialogové okno, přejděte na APK, který vytvořené a zkopírovali dříve. Vyberte APK a klikněte na **OK**: 
    
    ![Výběr v dialogovém okně vyberte soubor APK APK](profiling-images/vswin/04-select-apk-dialog.png)

6.  Android Studio načte APK a dissassembles **classes.dex**:

    ![Nastavení APK](profiling-images/vswin/05-setting-up-the-apk.png)

7.  Po načtení APK Android Studio zobrazí následující obrazovka projektu pro APK. Klikněte pravým tlačítkem na název aplikace v zobrazení stromu vlevo a vyberte **otevřete nastavení modulu**:

    [![Umístění položky nabídky Otevřít nastavení modulu](profiling-images/vswin/06-open-module-settings-sml.png)](profiling-images/vswin/06-open-module-settings.png#lightbox)

8.  Přejděte na **nastavení projektu > moduly**, vyberte **-podepsané** uzel aplikace a pak klikněte na  **&lt;ne SDK&gt;**:

    [![Navigace na nastavení SDK](profiling-images/vswin/07-project-settings-modules-sml.png)](profiling-images/vswin/07-project-settings-modules.png#lightbox)

9.  V **modulu SDK** rozevírací nabídky vyberte úroveň Android SDK, který byl použit k vytvoření aplikace (v tomto příkladu úroveň rozhraní API 26 byl použit k vytvoření **XamagonXuzzle**):

    [![Nastavení úrovně SDK projektu](profiling-images/vswin/08-project-sdk-level-sml.png)](profiling-images/vswin/08-project-sdk-level.png#lightbox)

    Klikněte na tlačítko **použít** a **OK** uložit toto nastavení.

10. Spusťte profileru z ikonu panelu nástrojů:

    [![Umístění ikony panelu nástrojů profileru](profiling-images/vswin/09-launch-profiler-sml.png)](profiling-images/vswin/09-launch-profiler.png#lightbox)

11. Vyberte cíl nasazení pro spuštění nebo profilace aplikace a klikněte na **OK**. Cíl nasazení může být fyzické zařízení nebo virtuální zařízení se systémem v emulátoru. V tomto příkladu se používá Nexus 5 X zařízení:

    ![Vyberte cíl nasazení](profiling-images/vswin/10-select-deployment-target.png)

12. Po spuštění profileru, bude trvat několik sekund, než ho se připojit k nasazení zařízení a proces aplikace. Když se instaluje APK, budou hlásit Android profileru **žádné připojená zařízení** a **žádné debuggable procesy**.

    [![Nainstaluje APK profileru](profiling-images/vswin/11-no-connected-devices-sml.png)](profiling-images/vswin/11-no-connected-devices.png#lightbox)

13. Po několika sekundách Android profileru dokončí APK instalace a spuštění APK, vytváření sestav, název zařízení a název profilovaný proces aplikace (v tomto příkladu **LGE Nexus 5 X** a  **com.companyname.XamagonXuzzle**, v uvedeném pořadí):

    [![Okno profileru po spuštění](profiling-images/vswin/12-profiler-starts-sml.png)](profiling-images/vswin/12-profiler-starts.png#lightbox)

14. Poté, co jsou označený debuggable proces a zařízení, Android profileru začne profilace aplikace:

    [![Zobrazí profileru spuštěné aplikace](profiling-images/vswin/13-profiler-running-sml.png)](profiling-images/vswin/13-profiler-running.png#lightbox)

15. Když klepnete **náhodně PŘESKUPIT** na tlačítko **XamagonXuzzle** (což způsobí, že se posunutí a náhodně přeskupit dlaždice), zobrazí se využití procesoru zvýšit během intervalu náhodného přeskupování aplikace:

    [![Využití procesoru, když je náhodně PŘESKUPIT tlačítko stisknuté](profiling-images/vswin/14-tap-randomize-sml.png)](profiling-images/vswin/14-tap-randomize.png#lightbox)


### <a name="using-the-android-profiler"></a>Pomocí Android profileru

Podrobné informace o použití Android profileru je součástí [dokumentaci Android Studio](https://developer.android.com/studio/profile/android-profiler.html).
V následujících tématech bude určen vývojářům Xamarin Android:

-   [Procesor profileru](https://developer.android.com/studio/profile/cpu-profiler.html) &ndash; vysvětluje, jak zkontrolovat aplikace využití procesoru a vlákno aktivitu v reálném čase.

-   [Paměti profileru](https://developer.android.com/studio/profile/memory-profiler.html) &ndash; zobrazí v reálném čase graf využití paměti aplikaci a obsahuje tlačítko pro přidělení paměti záznamů pro analýzu.

-   [Sítě profileru](https://developer.android.com/studio/profile/network-profiler.html) &ndash; zobrazí v reálném čase síťové aktivity dat odesílaných i přijímaných v: aplikace.
