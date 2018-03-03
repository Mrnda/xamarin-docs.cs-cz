---
title: "Poradce při potížích"
description: "Tento článek obsahuje vědět problémy se můžete setkat při práci s tvOS podpory pro Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 278b9e782073a26dc04bac9418613ea4c09db445
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Poradce při potížích

_Tento článek obsahuje vědět problémy se můžete setkat při práci s tvOS podpory pro Xamarin._

<a name="Known-Issues" />

## <a name="known-issues"></a>Známé problémy

Aktuální verze nástroje podpory pro Xamarin tvOS má následující známé problémy:

- **Monofonní Framework** – Mono 4.3 Cryptography.ProtectedData nepodaří dešifrovat data z Mono 4.2. V důsledku toho balíčky NuGet se nezdaří obnovení došlo k chybě `Data unprotection failed` při konfiguraci chráněného zdroje NuGet.
    - **Alternativní řešení** – v sadě Visual Studio pro Mac, budete muset přidat zpět všechny balíček NuGet zdroje tohoto ověřování hesla před znovu pokusem o obnovení balíčků.
- **Visual Studio pro Mac s F # Add-in** – Chyba při vytváření šablonu F # Android v systému Windows. To by měl i nadále fungovat správně na macu.
- **Xamarin.Mac** – spuštění Xamarin.Mac jednotná šablony projektu s cílem Framework nastavena na `Unsupported`, automaticky otevřeném okně `Could not connect to the debugger` , může se zobrazit.
    - **Alternativní řešení potenciální** – starší verzi k dispozici v našem stabilní kanál Mono framework verze.
- **Xamarin Visual Studio & Xamarin.iOS** – při nasazení aplikací WatchKit v sadě Visual studio, chyba `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` , může se zobrazit.

Využijte sestavy žádné chyby, můžete k [Bugzilla](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

## <a name="troubleshooting"></a>Poradce při potížích

V následujících částech jsou uvedené některé známé problémy, ke kterým dochází při používání tvOS 9 s Xamarin.tvOS a řešení těchto problémů:

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>Neplatný spustitelný soubor - spustitelný soubor neobsahuje bitcode

Při pokusu o odeslání Xamarin.tvOS aplikace do App Storu Apple TV, může získat chybovou zprávu ve formátu _"Neplatný spustitelný soubor - spustitelný soubor neobsahuje bitcode"_.

Chcete-li tento problém vyřešit, postupujte takto:

1. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na soubor projektu Xamarin.tvOS v **Průzkumníku řešení** a vyberte **možnosti**.
2. Vyberte **tvOS sestavení** a ujistěte se, že jste na **verze** konfigurace: 

    [ ![](troubleshooting-images/ts01.png "Vyberte tvOS možnosti sestavení")](troubleshooting-images/ts01.png)
3. Přidat `--bitcode=asmonly` k **mtouch další argumenty** pole a klikněte na tlačítko **OK** tlačítko.
4. Opětovné sestavení aplikace v rámci **verze** konfigurace.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>Jestli vaše aplikace obsahuje Bitcode tvOS

Pokud chcete ověřit, že buildu Xamarin.tvOS aplikace obsahuje Bitcode, otevřete aplikaci terminálu a zadejte následující příkaz:

```csharp
otool -l /path/to/your/tv.app/tv
```

Ve výstupu vyhledejte následující:

```csharp
Section
  sectname __bundle
   segname __LLVM
      addr 0x0000000100001000
      size 0x000000000000124f
    offset 4096
     align 2^0 (1)
    reloff 0
    nreloc 0
     flags 0x00000000
 reserved1 0
 reserved2 0
```

`addr` a `size` se bude lišit, ale další pole by měly být shodné.

Budete muset Ujistěte se, že jakékoli třetí strany statické (`.a`) knihovny používáte byly vytvořeny tvOS knihovny (není knihovny iOS) a že se také obsahuje informace o bitcode.

Pro aplikace nebo knihovny, které zahrnují platný bitcode `size` bude větší než 1. Existují některé situace, kde můžete knihovnu mít bitcode značky, ještě neobsahuje platný bitcode. Příklad:

**Neplatný Bitcode**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**Platný Bitcode** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

Všimněte si rozdílu v `size` mezi dvě knihovny v uvedených příkladu běží nad. Knihovny musí vygenerovat z archivu sestavení Xcode pomocí povolit bitcode (nastavení Xcode `ENABLE_BITCODE`) jako řešení tohoto problému velikost.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Aplikace, které obsahovat pouze řez arm64 musí také mít "arm64" v seznamu UIRequiredDeviceCapabilities v Info.plist

Při odesílání aplikace pro Apple TV App Store publikace, může být ve tvaru dojde k chybě:

_"Aplikace, které obsahovat pouze řez arm64 musí také mít"arm64"v seznamu UIRequiredDeviceCapabilities v Info.plist"_

Pokud k tomu dojde, upravit vaše `Info.plist` soubor a zkontrolujte, zda má následující klíče:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

Znovu zkompiluje aplikaci pro verzi a znovu odešlete do iTunes připojit.

### <a name="task-mtouch-execution----failed"></a>Spuštění úkolu "MTouch"--se nezdařilo

Pokud používáte knihovnu 3. stran (například MonoGame) a vaše verze kompilace se nezdařila s dlouho řadu chybové zprávy, které končí na `Task "MTouch" execution -- FAILED`, zkuste přidat `-gcc_flags="-framework OpenAL"` k vaší **touch další argumenty**:

[ ![](troubleshooting-images/mtouch01.png "MTouch provedení úlohy")](troubleshooting-images/mtouch01.png)

Musí rovněž zahrnovat `--bitcode=asmonly` v **touch další argumenty**, možnosti linkeru nastavili **odkaz všechny** a proveďte čistou kompilace.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS-90471 error. Chybí velkých ikon.

Pokud se zobrazí zpráva ve tvaru "ITMS 90471 došlo k chybě. Velké ikony chybí"při pokusu o odeslání Xamarin.tvOS aplikace pro Apple TV App Store pro verzi, zkontrolujte následující:

1. Ujistěte se, že jste zahrnuli velkých ikon. prostředky v vaše `Assets.car` soubor, který jste vytvořili pomocí [ikony aplikace](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) dokumentaci.
2. Ujistěte se, že jste zahrnuli `Assets.car` souboru z [práce s ikony a bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md) dokumentaci v vaší sady konečné aplikace.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>Neplatný sady – aplikaci, která podporuje herní zařízení musí také podporovat vzdálené Apple TV

or 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>Neplatný sady – Apple TV aplikací pomocí rozhraní herní musí obsahovat klíč GCSupportedGameControllers v Info.plist aplikace

Herní zařízení slouží k vylepšení hraní her a poskytnout představu o záchranných ve hře. Můžete také používají pro řízení standardní rozhraní Apple TV, uživatel nemá přepínat mezi vzdálených a kontroleru.

Pokud jste odeslali Xamarin.tvOS aplikace s podporou herní řadič k obchodu s aplikacemi Apple TV a chybová zpráva se zobrazuje ve tvaru:


_Zjistili jsme jeden nebo více problémů s vaší poslední doručení pro "název aplikace". Vaše doručení bylo úspěšné, ale chcete opravit následující problémy ve vašem další doručení:_

_Neplatný sady – aplikaci, která podporuje herní zařízení musí podporovat i vzdálené Apple TV._

or 

_Neplatný sady – Apple TV aplikací pomocí rozhraní herní musíte zahrnout klíč GCSupportedGameControllers Info.plist aplikace._

Řešení, je přidat podporu pro vzdálené Siri (`GCMicroGamepad`) do vaší aplikace `Info.plist` souboru. Profil Kontroleru herní Micro byl přidán nástrojem Apple pro vzdálené Siri. Například patří následující klíče:

```xml
<key>GCSupportedGameControllers</key>  
  <array>  
    <dict>  
      <key>ProfileName</key>  
      <string>ExtendedGamepad</string>  
    </dict>  
    <dict>  
      <key>ProfileName</key>  
      <string>MicroGamepad</string>  
    </dict>  
  </array>  
<key>GCSupportsControllerUserInteraction</key>  
<true/>
```

> [!IMPORTANT]
> **Poznámka:** Bluetooth herní zařízení jsou volitelné nákupu, které by mohly způsobit koncovým uživatelům, aplikace nemůže vynutit uživateli zakoupit. Pokud vaše aplikace podporuje herní zařízení se musí taky podporovat vzdálené Siri tak, aby hra funkční všichni uživatelé Apple TV.

Další informace najdete v tématu naše [práce s herní zařízení](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) části našich [Siri vzdálené a Bluetooth řadiče](~/ios/tvos/platform/remote-bluetooth.md) dokumentaci.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Nekompatibilní cílové rozhraní:. NetPortable, verze = v4.5, profil = Profile78

Při pokusu o přidání přenosných třída knihovny PCL () do projektu Xamarin.tvOS může získat zprávu tvoří:

_Nekompatibilní cílové rozhraní:. NetPortable, verze = v4.5, profil = Profile78_

Chcete-li tento problém vyřešit, přidejte soubor XML s názvem ` Xamarin.TVOS.xml` s následujícím obsahem:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

Do následujícího umístění:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

Všimněte si, že číslo profil v cestě musí shodovat s počtem profilu PCL.

Pomocí tohoto souboru na místě nyní byste měli mít úspěšně PCL soubor přidat do projektu Xamarin.tvOS.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
