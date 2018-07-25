---
title: GDB
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 886cc1de87bd8225bd0389d2e7b84b546ffb39d7
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241494"
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Přehled

Xamarin.Android 4.10 zavedené částečně se podporuje pro použití `gdb` pomocí `_Gdb` cíl nástroje MSBuild. 

> [!NOTE]
> `gdb` podpora vyžaduje, aby byla instalace sady Android NDK.

Existují tři způsoby, jak používat `gdb`:

1.  [Sestavení se rychlé nasazení je povolené ladění](#Debug_Builds_with_Fast_Deployment) .
1.  [Ladění sestavení se rychlé nasazení zakázané](#Debug_Builds_without_Fast_Deployment) .
1.  [Sestavení pro vydání](#Release_Builds) .


Když něco selže, podrobnosti najdete [Poradce při potížích s](#Troubleshooting) oddílu.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>Rychlé nasazení sestavení pro ladění

Při vytváření a nasazování ladění sestavení s rychlé nasazení je povolené, `gdb` lze připojit pomocí `_Gdb` cíl nástroje MSBuild.

Nejdřív nainstalujte na aplikaci. To lze provést prostřednictvím integrovaného vývojového prostředí nebo prostřednictvím příkazového řádku:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

Za druhé, spusťte `_Gdb` cíl. Na konci spuštění `gdb` vytiskne příkazového řádku:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

`_Gdb` Cíl se spustí libovolného Spouštěč aktivity deklarované v rámci vaší `AndroidManifest.xml` souboru. Chcete-li spuštění aktivity explicitně zadat, použijte `RunActivity` vlastnosti Msbuildu. Spuštění služeb a jiných objektů, které s Androidem se v tuto chvíli nepodporuje.

`_Gdb` Vytvoří cíl `gdb-symbols` adresáře a zkopírujte obsah vaší cílové `/system/lib` a `$APPDIR/lib` adresáře existuje.


> [!NOTE]
> Obsah `gdb-symbols` adresáře jsou vázané na nasazené na Android cíl a nebude automaticky nahradit by měl při změně cíle. (Vezměte v úvahu to chybu.) Pokud změníte s Androidem cílových zařízení, musíte ručně odstranit tento adresář.

Nakonec zkopírujte vygenerovaný `gdb` příkazů a spustit v prostředí:

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

Sestavení pro ladění *s* rychlé nasazení fungovat tak, že zkopírujete sady Android NDK `gdbserver` programu do nasazení rychle `.__override__` adresáře. Když je zakázané rychlé nasazení, tento adresář neexistuje.

Existují dvě alternativní řešení:

-   Nastavte `debug.mono.log` vlastnost systému tak, aby `.__override__` adresář bude vytvořen.
-   Zahrnout `gdbserver` v rámci vaší `.apk`.

### <a name="setting-the-debugmonolog-system-property"></a>Nastavení `debug.mono.log` vlastnost systému

Chcete-li nastavit `debug.mono.log` vlastnosti systému, použijte `adb` příkaz:

```bash
$ adb shell setprop debug.mono.log gc
```

Po nastavení vlastnosti systému, spustit `_Gdb` cíl a vytištěného `gdb` příkazů, jako ladění sestavení s konfigurací rychlé nasazení:

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


### <a name="including-gdbserver-in-your-app"></a>Včetně `gdbserver` ve vaší aplikaci

Zahrnout `gdbserver` ve vaší aplikaci:

1. Najít `gdbserver` v rámci Android NDK (by měla být v **$ANDROID\_NDK\_cesta/předem připravených/android-arm/gdbserver/gdbserver**) a zkopírujte do adresáře vašeho projektu.

2. Přejmenovat `gdbserver` k **libs/armeabi-v7a/libgdbserver.so**.

3. Přidat **libs/armeabi-v7a/libgdbserver.so** do svého projektu pomocí **akce sestavení** z `AndroidNativeLibrary`.

4. Znovu sestavte a znovu nainstalujte aplikaci.

Jakmile aplikace byl přeinstalován, spusťte `_Gdb` cíl a vytištěného `gdb` příkazů, jako ladění sestavení s konfigurací rychlé nasazení:

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
3.  K dispozici přístup `gdbserver`.

`INTERNET` Oprávnění je povolena ve výchozím nastavení ladění aplikace. Pokud již není k dispozici v aplikaci, můžete přidat je buď tak, že upravíte **Properties/AndroidManifest.xml** nebo úpravou [vlastnosti projektu](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest).

Ladění aplikací můžete povolit buď nastavením [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) vlastnosti vlastního atributu `true`, nebo tak, že upravíte **Properties/AndroidManifest.xml** a nastavení `//application/@android:debuggable` atribut `true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

K dispozici přístup `gdbserver` může být poskytnuta podle [ladění sestavení bez rychlé nasazení](#Debug_Builds_without_Fast_Deployment) oddílu.

Jeden zkrabacení: `_Gdb` cíl nástroje MSBuild se ukončit všechny dříve spuštěné instance aplikace. Tato funkce nebude pracovat na cílech Android předem v4.0.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Poradce při potížích

### <a name="monopmip-doesnt-work"></a>`mono_pmip` nefunguje

`mono_pmip` – Funkce (užitečné pro [získání spravovaných rámců](http://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)) se exportuje z `libmonosgen-2.0.so`, který `_Gdb` target není aktuálně stáhněte dolů. (Tato chyba bude opravena v budoucí verzi.)

Chcete-li povolit volání funkcí, které jsou umístěné v `libmonosgen-2.0.so`, zkopírujte z cílové zařízení do `gdb-symbols` adresáře:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Potom restartujte relaci ladění.

### <a name="bus-error-10-when-running-the-gdb-command"></a>Service Bus Chyba: 10 při spuštění `gdb` příkaz

Když `gdb` příkazu se `"Bus error: 10"`, restartujte zařízení s Androidem.

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

To je obvykle znak, který obsah `gdb-symbols` adresáře nejsou synchronizované s Androidem cíl. (Došlo ke změně cíle s Androidem?)

Odstraňte prosím `gdb-symbols` adresář a akci opakujte.
