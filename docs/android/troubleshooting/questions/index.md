---
title: Nejčastější dotazy
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: bbb1e92596fe69246a608eb0758e789f5b99e954
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935461"
---
# <a name="frequently-asked-questions"></a>Nejčastější dotazy

## <a name="installation--setup"></a>Nastavení a instalace

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Které balíčky Android SDK si mám nainstalovat?](install-android-sdk-packages.md)

Instalace sady Android SDK neobsahuje automaticky všechny minimální požadované balíčky pro vývoj. Při jednotlivých vývojáře musí lišit, tato příručka popisuje balíčky, které budou obecně potřebné pro vývoj pomocí Xamarin.Android.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Kde můžu nastavit umístění sad Android SDK?](android-sdk-location.md)

Tato příručka popisuje nastavení výchozí sady SDK pro Android, která by měla fungovat pro většinu nastavení; a jak změnit toto výchozí nastavení v sadě Visual Studio pro Mac nebo Visual Studio, v případě potřeby.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Jak aktualizovat verzi sady Java Development Kit (JDK)?](update-jdk.md)

Tento článek ukazuje, jak aktualizovat verzi jazyka Java Development Kit (JDK) v systému Windows a macu.

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Můžu používat sadu Java Development Kit (JDK) ve verzi 9?](jdk9-errors.md)

Xamarin.Android vyžaduje JDK 8, místo JDK 9. V tomto článku jsou uvedené některé běžné chybové zprávy, které se může zobrazit, pokud je nainstalovaná JDK 9, společně s pokyny pro kontrolu verze JDK.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Jak ručně nainstalovat podpůrné knihovny pro Android vyžadované balíčky Xamarin.Android.Support?](install-android-support-library.md)

Tato příručka obsahuje příklady kroků pro instalaci `Xamarin.Android.Support.v4` knihovna podpory v systému Windows & Mac.

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[Jak nainstalovat Služby Google Play do emulátoru?](install-gps.md)

Tato příručka odkazy na informace o instalaci služby Google Play v Android emulátoru sady Visual Studio.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Jaké ovladače USB potřebuji k ladění Androidu na Windows?](android-drivers-debug-windows.md)

Chcete-li ladit na zařízení s Androidem při vývoji v systému Windows. musíte nainstalovat kompatibilní ovladač USB. Android SDK Manager zahrnuje "USB ovladač Google" ve výchozím nastavení, která přidává podporu pro Nexus zařízení.
Jiná zařízení vyžadují ovladače USB konkrétně publikováno výrobce zařízení. Tato příručka obsahuje informace o hledání tyto ovladače a jako alternativní metody testování.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Je možné se připojit k emulátorům Androidu běžícím na Macu z virtuálního počítače s Windows?](connect-android-emulator-mac-windows.md)

Tento průvodce popisuje metody při použití emulátoru systému Android.

## <a name="general-questions"></a>Obecné otázky

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Jak automatizovat testovací projekt Android NUnit?](automate-android-nunit-test.md)

Tento průvodce popisuje kroky pro nastavení testovacího projektu Android NUnit _není_ Xamarin.UITest projektu. Příručky Xamarin.UITest můžete najít [zde](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Jak v souborech Androidu .axml povolit IntelliSense?](enable-axml-intellisense.md)

Tato příručka popisuje postup aktivace služby Visual Studio Intellisense pro Android .axml soubory.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Proč se moje sestavení Androidu pro vydání nemůže připojit k internetu?](android-internet.md)

Nejčastější příčinou tohoto problému je, že **INTERNET** oprávnění je automaticky součástí sestavení ladicí verze, ale musíte ručně nastavit pro sestavení pro vydání. Tato příručka popisuje, jak povolit oprávnění u sestavení pro vydání.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Inteligentnější balíčky NuGet pro podporu Xamarin Androidu v4 nebo v13](android-support-v4v13-libraries.md)

`Support-v4` a `Support-v13` nelze použít společně ve stejné aplikaci, to znamená, že se vzájemně vylučují. Důvodem je, že `Support-v13` ve skutečnosti obsahuje všechny typy a provádění `Support-v4`. Pokud zkuste a odkazovat oba ve stejném projektu, se setkají duplicitní typ chyby.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Jak lze vyřešit chyby PathTooLongException?](path-too-long-exception.md)

Tento článek vysvětluje, jak vyřešit **PathTooLongException** chyby, ke kterému může dojít při sestavování projektu Xamarin.Android.



## <a name="deprecated"></a>Zastaralé

> [!NOTE]
> Následující články nevztahují na problémy, které byly vyřešeny v posledních verzích Xamarin. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Která verze Xamarin.Androidu přidala podporu verze Lollipop?](xa-lollipop.md)

Tento průvodce byl původně zapsán Android L ve verzi Preview. Xamarin.Android 4.17 přidaná podpora pro Android Preview L & Xamarin.Android 4.20 přidána podpora pro Android typu Lupa.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat – Nenašel se žádný prostředek, který by odpovídal danému názvu: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Této chybě může dojít ve starších verzích Xamarin, pokud chybí některé požadované pacakages sady SDK pro Android.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Úprava parametrů paměti Java pro návrháře pro Android](android-designer-java-memory.md)

Výchozí parametry paměti, které se používají při spouštění `java` zpracování pro Android návrháře zřejmě není kompatibilní s některé konfigurace systému. Od verze Xamarin Studio 5.7.2.7 a Xamarin pro Visual Studio 3.9.344 tato nastavení můžete přizpůsobit na jednotlivých projektů.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Můj soubor Androidu Resource.designer.cs nejde aktualizovat](resource-designer-wont-update.md)

Chyby v Xamarin.Studio 5.1 dříve poškozené soubory .csproj odstraněním částečně nebo zcela kód xml v souboru .csproj. To by způsobilo důležité části Android sestavení systému (například aktualizaci Android Resource.designer.cs) k selhání. Od verze 5.1.4 stabilní verzi na 15. července, tato chyba byla opravena; ale v mnoha případech je třeba opravit ručně, jak je popsáno v této příručce souboru projektu.



