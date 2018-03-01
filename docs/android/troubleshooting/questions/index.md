---
title: "Nejčastější dotazy"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d6d1a5f659317267859164181216177857a67fd2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="frequently-asked-questions"></a>Nejčastější dotazy

## <a name="installation--setup"></a>Nastavení a instalace

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Instalace balíčků které sady SDK pro Android](install-android-sdk-packages.md)

Instalace sady Android SDK neobsahuje automaticky všechny minimální požadované balíčky pro vývoj. Při jednotlivých vývojáře musí lišit, tato příručka popisuje balíčky, které budou obecně potřebné pro vývoj pomocí Xamarin.Android.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Kde můžete nastavit Moje umístění sady SDK pro Android?](android-sdk-location.md)

Tato příručka popisuje nastavení výchozí sady SDK pro Android, která by měla fungovat pro většinu nastavení; a jak změnit toto výchozí nastavení v sadě Visual Studio pro Mac nebo Visual Studio, v případě potřeby.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Jak lze aktualizovat verze Java Development Kit (JDK)?](update-jdk.md)

Tento článek ukazuje, jak aktualizovat verzi jazyka Java Development Kit (JDK) v systému Windows a macu.

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Můžete použít Java Development Kit (JDK) verze 9?](jdk9-errors.md)

Xamarin.Android vyžaduje JDK 8, místo JDK 9. V tomto článku jsou uvedené některé běžné chybové zprávy, které se může zobrazit, pokud je nainstalovaná JDK 9, společně s pokyny pro kontrolu verze JDK.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Jak lze ručně nainstalovat Android, podporují knihovny potřebné balíčky Xamarin.Android.Support?](install-android-support-library.md)

Tato příručka obsahuje příklady kroků pro instalaci `Xamarin.Android.Support.v4` knihovna podpory v systému Windows & Mac.

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[Instalace služby Google Play v emulátoru](install-gps.md)

Tato příručka odkazy na informace o instalaci služby Google Play v Android emulátoru sady Visual Studio.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Jaké ovladače USB jsou nutné k ladění Android v systému Windows?](android-drivers-debug-windows.md)

Chcete-li ladit na zařízení s Androidem při vývoji v systému Windows. musíte nainstalovat kompatibilní ovladač USB. Android SDK Manager zahrnuje "USB ovladač Google" ve výchozím nastavení, která přidává podporu pro Nexus zařízení.
Jiná zařízení vyžadují ovladače USB konkrétně publikováno výrobce zařízení. Tato příručka obsahuje informace o hledání tyto ovladače a jako alternativní metody testování.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Je možné se připojit k Android emulátorů spuštěn na Macu z virtuálního počítače Windows?](connect-android-emulator-mac-windows.md)

Tento průvodce obsahuje metody, pokud používáte emulátor Google Android.

## <a name="general-questions"></a>Obecné otázky

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Jak mohu automatizovat projektu Android testovací NUnit?](automate-android-nunit-test.md)

Tento průvodce popisuje kroky pro nastavení testovacího projektu Android NUnit _není_ Xamarin.UITest projektu. Příručky Xamarin.UITest můžete najít [zde](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Povolení technologie Intellisense v souborech Android .axml](enable-axml-intellisense.md)

Tato příručka popisuje postup aktivace služby Visual Studio Intellisense pro Android .axml soubory.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Proč můj sestavení pro vydání Android se připojit k Internetu?](android-internet.md)

Nejčastější příčinou tohoto problému je, že **INTERNET** oprávnění je automaticky součástí sestavení ladicí verze, ale musíte ručně nastavit pro sestavení pro vydání. Tato příručka popisuje, jak povolit oprávnění u sestavení pro vydání.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Podpora chytřejší Xamarin Android v4 nebo v13 balíčků NuGet](android-support-v4v13-libraries.md)

`Support-v4` a `Support-v13` nelze použít společně ve stejné aplikaci, to znamená, že se vzájemně vylučují. Důvodem je, že `Support-v13` ve skutečnosti obsahuje všechny typy a provádění `Support-v4`. Pokud zkuste a odkazovat oba ve stejném projektu, se setkají duplicitní typ chyby.


## <a name="deprecated"></a>Zastaralé

> [!NOTE]
> **Poznámka:** následující články, na které se týkají problémů, které byly vyřešeny v posledních verzích Xamarin. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Jaká verze Xamarin.Android přidat podporu typu Lupa?](xa-lollipop.md)

Tento průvodce byl původně zapsán Android L ve verzi Preview. Xamarin.Android 4.17 přidaná podpora pro Android Preview L & Xamarin.Android 4.20 přidána podpora pro Android typu Lupa.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - nalezen žádný prostředek, který odpovídá zadaným názvem: Line 'android: actionModeShareDrawable.](missing-action-mode-share-drawable.md)

Této chybě může dojít ve starších verzích Xamarin, pokud chybí některé požadované pacakages sady SDK pro Android.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Úprava parametrů Java paměti pro Android návrháře](android-designer-java-memory.md)

Výchozí parametry paměti, které se používají při spouštění `java` zpracování pro Android návrháře zřejmě není kompatibilní s některé konfigurace systému. Od verze Xamarin Studio 5.7.2.7 a Xamarin pro Visual Studio 3.9.344 tato nastavení můžete přizpůsobit na jednotlivých projektů.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Moje Android Resource.designer.cs soubor nebude aktualizovat.](resource-designer-wont-update.md)

Chyby v Xamarin.Studio 5.1 dříve poškozené soubory .csproj odstraněním částečně nebo zcela kód xml v souboru .csproj. To by způsobilo důležité části Android sestavení systému (například aktualizaci Android Resource.designer.cs) k selhání. Od verze 5.1.4 stabilní verzi na 15. července, tato chyba byla opravena; ale v mnoha případech je třeba opravit ručně, jak je popsáno v této příručce souboru projektu.



