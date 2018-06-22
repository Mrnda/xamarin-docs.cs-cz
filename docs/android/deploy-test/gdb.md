---
title: GDB
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 1292db3534570dace90639958a3d5be9f6466716
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765243"
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Přehled

Xamarin.Android 4.10 zavedená částečná podpora pro použití `gdb` pomocí `_Gdb` MSBuild cíl. 

> [!NOTE]
> `gdb` podpora vyžaduje nainstalované Android NDK.

Existují tři způsoby, jak používat `gdb`:

1.  [Ladicí sestavení s rychlého nasazení povolené](#Debug_Builds_with_Fast_Deployment) .
1.  [Ladicí sestavení s rychlého nasazení zakázáno](#Debug_Builds_without_Fast_Deployment) .
1.  [Sestavení pro vydání](#Release_Builds) .


Když dojde k potížím, najdete v tématu [Poradce při potížích s](#Troubleshooting) části.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>Laděná sestavení s rychlé nasazení

Při vytváření a nasazování ladění sestavení s rychlého nasazení povolené, `gdb` lze připojit pomocí `_Gdb` MSBuild cíl.

Nejdřív nainstalujte na aplikaci. To lze provést pomocí rozhraní IDE, nebo prostřednictvím příkazového řádku:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

Za druhé, spusťte `_Gdb` cíl. Na konci spuštění `gdb` budou vytištěny příkazového řádku:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

`_Gdb` Cíl se spustí libovolný Spouštěče aktivity deklarované v rámci vaší `AndroidManifest.xml` souboru. Chcete-li explicitně zadat, která aktivita se má spustit, použijte `RunActivity` vlastnosti MSBuild. V tuto chvíli nepodporuje spouštění služeb a jiných objektů, které Android.

`_Gdb` Cíl vytvoří `gdb-symbols` adresář a zkopírujte obsah vaše cíle `/system/lib` a `$APPDIR/lib` adresáře existuje.


> [!NOTE]
> Obsah `gdb-symbols` , jsou svázané s Android cíl jste nasadili do adresáře a nebude automaticky nahradit by se měl můžete změnit cíl. (Zvažte to chyby.) Pokud změníte Android cílových zařízení, musíte ručně odstranit tento adresář.

Nakonec zkopírujte vygenerovaného `gdb` příkazů a spustit ve vašem prostředí:

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment" />

## <a name="debug-builds-without-fast-deployment"></a>Laděná sestavení bez rychlé nasazení

Laděná sestavení *s* rychlého nasazení fungovat tak, že zkopírujete Android NDK `gdbserver` program do nasazení rychlé `.__override__` adresáře. Při rychlé nasazení je zakázaná, tento adresář neexistuje.

Existují dva alternativní řešení:

-   Nastavte `debug.mono.log` vlastnost systému tak, aby `.__override__` adresář bude vytvořen.
-   Zahrnout `gdbserver` v rámci vaší `.apk`.

### <a name="setting-the-debugmonolog-system-property"></a>Nastavení `debug.mono.log` vlastnost systému

Chcete-li nastavit `debug.mono.log` vlastnosti systému, použijte `adb` příkaz:

```bash
$ adb shell setprop debug.mono.log gc
```

Jakmile byla nastavena vlastnost systému, spustit `_Gdb` cíle a tištěných `gdb` příkaz jako s ladění sestavení s konfigurací rychlého nasazení:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```


### <a name="including-gdbserver-in-your-app"></a>Včetně `gdbserver` v aplikaci

Zahrnout `gdbserver` v rámci vaší aplikace:

1. Najít `gdbserver` v rámci Android NDK (musí být v **$ANDROID\_NDK\_cesta/předem/android-arm/gdbserver/gdbserver**) a zkopírujte jej do adresáře projektu.

2. Přejmenování `gdbserver` k **libs/armeabi-v7a/libgdbserver.so**.

3. Přidat **libs/armeabi-v7a/libgdbserver.so** do projektu s **akce sestavení** z `AndroidNativeLibrary`.

4. Znovu sestavte a znovu nainstalujte aplikaci.

Jakmile aplikaci byl přeinstalován, provést `_Gdb` cíle a tištěných `gdb` příkaz jako s ladění sestavení s konfigurací rychlého nasazení:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds" />

## <a name="release-builds"></a>Sestavení pro vydání

`gdb` podpora vyžaduje tři věci:

1.  `INTERNET` Oprávnění.
2.  Ladění aplikace povolena.
3.  Přístupné `gdbserver`.

`INTERNET` Oprávnění je povoleno ve výchozím nastavení v aplikacích pro ladění. Pokud již není součástí vaší aplikace, může ho přidáte buď úpravou **Properties/AndroidManifest.xml** nebo úpravou [vlastnosti projektu](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/).

Aplikace může být povoleno ladění. buď v nastavení [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) vlastnost vlastní atribut `true`, nebo úpravou **Properties/AndroidManifest.xml** a nastavení `//application/@android:debuggable` atribut `true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

Přístupné `gdbserver` mohou být poskytovány následující [ladění sestavení bez rychlého nasazení](#Debug_Builds_without_Fast_Deployment) části.

Jeden zkrabacení: `_Gdb` MSBuild cíl bude ukončit všechny dříve spuštěné instance aplikace. Tato funkce nebude pracovat na předem Android v4.0 cíle.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Poradce při potížích

### <a name="monopmip-doesnt-work"></a>`mono_pmip` nefunguje

`mono_pmip` – Funkce (užitečné pro [získání rámce zásobníku spravované](http://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)) exportují z `libmonosgen-2.0.so`, což `_Gdb` target není aktuálně stáhněte dolů. (Tento problém bude vyřešený v příští verzi.)

Chcete-li povolit volání funkcí, které jsou umístěné v `libmonosgen-2.0.so`, zkopírujte jej z cílového zařízení do `gdb-symbols` directory:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Potom restartujte relaci ladění.

### <a name="bus-error-10-when-running-the-gdb-command"></a>Sběrnici Chyba: 10, při spuštění `gdb` příkaz

Když `gdb` příkaz chyby se s `"Bus error: 10"`, restartovat zařízení s Androidem.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>Žádné trasování zásobníku po připojení

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

Toto je obvykle znaménkem, obsah `gdb-symbols` directory nejsou synchronizované s Androidem cílových. (Došlo ke změně Android cílových?)

Odstraňte prosím `gdb-symbols` adresář a zkuste to znovu.
