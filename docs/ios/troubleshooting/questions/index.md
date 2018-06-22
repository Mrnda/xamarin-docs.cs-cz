---
title: Nejčastější dotazy
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: f8264c48b3b4679d45d5603b5637df0016cd3d17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30780232"
---
# <a name="frequently-asked-questions"></a>Nejčastější dotazy

## <a name="general-questions"></a>Obecné otázky

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Můžu použít virtuální počítač Mac se Xamarinem?](mac-vm.md)
Ano, ale jenom na hardwaru Mac.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Jak se dá přejít na starší verzi produktu Xcode?](downgrade-xcode.md)
Tato příručka obsahuje odkazy na přístup k předchozí verze Xcode a také na nejnovější verzi.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[Kde můžu nastavit umístění sad SDK pro iOS?](ios-sdk.md)
Pro většinu uživatelů jsou nastaveny na hodnotu správné umístění automaticky. Tento průvodce uvádí výchozí umístění sady SDK a jak ji změnit podle potřeby.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[Jak lze znovu možnosti pro vývojáře po aktualizaci iOS?](update-developer-options.md)
IOS chyb může způsobit, že možnosti pro vývojáře k zmizet po aktualizaci verze iOS, to byl zaznamenán přepnutím na verzi iOS 8.x. Tato příručka popisuje, jak můžete opětovně povolena možnosti.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[V iOS 8 nefunguje poloha uživatele](ios8-user-location.md)
Tato příručka vysvětluje, jak upravit info.plist pro povolení umístění uživatele v iOS 8.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[Kde najdu soubor .dSYM, který symbolizuje protokoly chybových ukončení iOSu?](symbolicate-ios-crash.md)
Tato příručka popisuje základní kroky pro symbolicating protokoly havárií iOS k diagnostice dojde k chybě. Taky obsahuje odkazy na další zdroje pro pokročilejší symbolication techniky & informace o interpretaci protokolů havárií iOS.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Jak nastavím proměnné prostředí Mono Runtime pro projekty iOS v nástroji Xamarin Studio?](xs-mono-runtime.md)
Pokud potřebujete nastavit všechny runtime proměnných prostředí pro Mono, může být nastavena v **možnosti projektu > Spustit > Obecné** stránky.

## <a name="publishing-questions"></a>Publikování otázky

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[Chyba při odesílání na App Store: "Neplatný sady - nesmí být součástí bitcode možnosti zjištění v odesílání"](invalid-bundle-bitcode.md)

Odesílá se aplikace, _vyžadují_ bitcode, jako je například watchOS a tvOS aplikacemi, je nutné provést pomocí Xcode 9.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[Můžete změnit výstupní cestu k souboru soubor IPA?](ipa-output-path.md)
Od verze 7 cyklus Xamarin můžete použít vlastní cíle MSBuild dosáhnout.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[Jak můžete zkopírovat soubor IPA výstupní soubory do složky, vyřaďte TFS?](ipa-tfs.md)
Ano, tato příručka popisuje, jak.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Můžete přidat soubory do nebo odebrat soubory ze souboru soubor IPA po sestavení v sadě Visual Studio?](modify-ipa.md)
Ano, je možné, ale bude obvykle vyžadují, aby znovu podepsat `.app` sady po provedení změny. Všimněte si, že změnou `.ipa` souboru není nutné v běžném provozu. Tento článek poskytuje výhradně pro informační účely.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Je možné vytvořit archivu .xcarchive ze sady Visual Studio?](create-xcarchive.md)
Od verze Xamarin 4, je možné vytvořit `.xcarchive` ze systému Windows nastavením `ArchiveOnBuild` vlastnost `true`.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[Proč moje odesílání aplikace neproběhne úspěšně kvůli této chybě: Disallowed paths ( "iTunesMetadata.plist" ) found at ...](itunesmetadata-disallowed-paths.md)
Tato chyba je výsledkem změny v procesu ověřování společnosti Apple App Store. Tento kód chyby je _není_ související s konkrétní verzi Xamarin jste nainstalovali, takže přechod na starší verzi bude _není_ pomoci. Tato příručka odkazy na další informace o tom, jak vyřešit problém.


## <a name="diagnosing-specific-error-messages"></a>Diagnostikování specifické chybové zprávy

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[Chyba iOS Designeru s RegisterServicePort](error-registerserviceport.md)
Chyby s `RegisterServicePort` a chybové zprávy podobné jako výše jsou běžně problém s spywaru nebo malwaru v počítači. Tato příručka údaje potvrzení diagnostiky a informace o odebrání spywaru nebo malwaru.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[Proč moje sestavení iOS neproběhne úspěšně kvůli této chybě: no valid iPhone code signing keys found in keychain](no-codesigning-keys.md)
Tato chybová zpráva nastane, když v projektu hledá platné přihlašovací údaje podepisování kódu, ale nemůže nalézt je. Podepisování kódu se vyžaduje pro účely testování a nasazení na zařízení s iOS fyzické; stejně jako aplikace pro Ad-hoc a ukládání sestavení.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Proč moje aplikace pro iOS 9 selže kvůli této chybě: System.Exception: Failed to marshal the Objective-C object](exception-marshal-obj-c.md)
Rozhraní API změny v iOS 9 vyžaduje, aby se konstruktor zpětného volání používané při volání nespravovaného kódu jako rozhraní API základní očekává, že.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[Chyba za běhu: Sestavení mscorlib.dll se nenašlo, nebo se nedalo načíst.](error-mscorlib-not-found.md)
K tomuto problému dochází při *Skrytá* `.monotouch-32` a `.monotouch-64` složky chybí `.xcarchive` pro podepisování nebo soubor IPA je vytvoření, spuštění chyba za běhu.

## <a name="deprecated"></a>Zastaralé

> [!IMPORTANT]
> Následující články nevztahují na problémy, které byly vyřešeny v posledních verzích Xamarin. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[Soubor IPA má 0 bajtů.](ipa-zero-bytes.md)
Nebyly některé známé problémy v předchozích verzích nástroje Xamarin, který by mohl způsobit soubor IPA v systému Windows, chcete-li být 0 bajtů.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[Chyba IBTool: Operaci nešlo dokončit.](error-ibtool.md)
Apple [pevné](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) to `ibtool` je nejjednodušší oprava chyb v Xcode 6.1.1, takže upgrade na Xcode 6.1.1 nebo vyšší.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[Chyba MT1009: Sestavení nešlo zkopírovat.](error-mt1009.md)
To ovlivní uživatelům používajícím Xamarin.iOS 7.2.6. Tento problém je z důvodu oprávnění k souboru nutnosti vyšší oprávnění, když je nainstalován Xamarin.iOS pomocí jiného uživatelského účtu poté hlavní účet pro vývojáře.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe vrátilo...](exception-amddevicenotificationsubscribe.md)
Tato zpráva se může zobrazit v dialogové okno chyby při prvním spuštění sady Visual Studio pro Mac, nebo v `mtbserver.log` souboru. Všimněte si, že se jedná o problém s neobvyklé. Pokud Visual Studio je potíže s připojením k hostiteli sestavení Mac, jsou jiné chyby, které budou s větší pravděpodobností, než se objeví v `mtbserver.log` souboru.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
Tato chyba se může zobrazit v `Mac Server Log` v sadě Visual Studio.
