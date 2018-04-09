---
title: Zařízení s více jádry & Xamarin.Android
description: Android můžete spustit na několik architektury jiného počítače. Tento dokument popisuje různé architektury procesoru, které může být použit pro aplikace pro Xamarin.Android. Tento dokument taky vysvětluje, jak Android aplikace pro podporu různé architektury procesoru spojených. Bude potřeba zavést aplikace binární rozhraní (ABI) a bude poskytnuta pokyny týkající se které bis používat v aplikaci Xamarin.Android.
ms.prod: xamarin
ms.assetid: D812883C-A14A-E74B-0F72-E50071E96328
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0288ba6aa8a3c9eb89208161f60ba831723444c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="multi-core-devices--xamarinandroid"></a>Zařízení s více jádry & Xamarin.Android

_Android můžete spustit na několik architektury jiného počítače. Tento dokument popisuje různé architektury procesoru, které může být použit pro aplikace pro Xamarin.Android. Tento dokument taky vysvětluje, jak Android aplikace pro podporu různé architektury procesoru spojených. Bude potřeba zavést aplikace binární rozhraní (ABI) a bude poskytnuta pokyny týkající se které bis používat v aplikaci Xamarin.Android._


## <a name="overview"></a>Přehled

Android umožňují vytvářet "fat binárních souborů," jedné `.apk` soubor, který obsahuje zkompilovaný kód, který bude podporovat více různé architektury procesoru. Toho dosahuje tím, že přidružíte každého jednotlivého zkompilovaný kód s *binární rozhraní aplikace*. ABI je použít k řízení, které zkompilovaný kód spustí na dané hardwarové zařízení. Například pro spuštění na x86 Android aplikace zařízení, je nutné zahrnout x86 ABI podporují při kompilaci aplikace.

Konkrétně, bude každý aplikace pro Android podporovat alespoň jeden *binární rozhraní vložených aplikace* (EABI). EABI jsou specifické pro embedded softwarové programy. Typické EABI popíše věci, jako:

-   Sada instrukcí procesoru.

-   Endianitou paměti ukládá a načte za běhu.

-   Binární formát soubory objektů a program knihovny, a také typu obsahu je povoleno nebo podporováno v těchto souborů a knihoven.

-   Různé konvence sloužící k předávání dat mezi kódu aplikace a systému (například: registry nebo zásobníku používání při jsou volány funkce, omezení zarovnání atd.)

-   Zarovnání a velikost omezení pro typy výčtu, struktury, pole a pole.

-   Seznam symbolů funkce dostupné pro počítač kódu v době běhu, obvykle z velmi konkrétní sadu vybrané knihovny.



### <a name="armeabi-and-thread-safety"></a>armeabi a bezpečný přístup z více vláken

Binární rozhraní aplikace se bude zabývat rozpis naleznete níže, ale je důležité nezapomenout, že `armeabi` je runtime používané Xamarin.Android *není zaručeno bezpečné používání vláken*. Pokud aplikace, která má `armeabi` podporu nasazení na `armeabi-v7a` zařízení, dojde k mnoha neobvyklé a unexplainable výjimky.

Z důvodu chyby v Android 4.0.0, 4.0.1, 4.0.2 a 4.0.3, bude možné nativní knihovny převzata z `armeabi` adresář, i když je `armeabi-v7a` adresář existuje a zařízení je `armeabi-v7a` zařízení.

> [!NOTE]
> Xamarin.Android zajistí, že `.so` jsou přidány do APK ve správném pořadí. Tato chyba by neměl být problém pro uživatele Xamarin.Android.


### <a name="abi-descriptions"></a>Popisy ABI

Každý ABI nepodporuje Android je identifikována jedinečný název.



#### <a name="armeabi"></a>armeabi

Toto je název EABI pro založené na ARM procesorů, které podporují alespoň sada instrukcí ARMv5TE. Android následuje ABI little endian ARM GNU/Linux. Tato ABI nepodporuje s hardwarovým řízením výpočty s plovoucí desetinnou čárkou. Všechny operace FP provádí softwaru pomocných funkcí, které pocházejí z kompilátoru `libgcc.a` statické knihovny. SMP zařízení nepodporuje `armeabi`.

**Poznámka:**: pro Xamarin.Android `armeabi` kód není zaručeno bezpečné používání vláken a neměl by se používat na více procesorů `armeabi-v7a`zařízení (viz dále). Pomocí `aremabi` kódu na jedním jádrem `armeabi-v7a` zařízení je bezpečné.



#### <a name="armeabi-v7a"></a>armeabi-v7a

Toto je jiná sada instrukcí procesory založené na ARM, který rozšiřuje `armeabi` EABI popsané výše. `armeabi-v7a` EABI obsahuje podporu pro hardware operace s plovoucí desetinnou čárkou a více zařízení procesoru (SMP). Aplikace, která používá `armeabi-v7a` EABI můžete očekávat výrazně zrychlit přes aplikaci, která používá `armeabi`.

**Poznámka:** `armeabi-v7a` zkompilovaný kód se nespustí na ARMv5 zařízení.



#### <a name="arm64-v8a"></a>arm64-v8a

Je to sada instrukcí 64-bit, která je založená na architektuře procesoru ARMv8. Tato architektura se používá v *Nexus 9*.
Xamarin.Android 5.1 poskytuje experimentální podporu pro tato architektura (Další informace najdete v tématu [povolenými experimentálními funkcemi](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features)).



#### <a name="x86"></a>x86

Toto je název ABI pro procesorů, které podporují pokyn nastavit běžně pojmenované *x86* nebo *IA-32*. Tato ABI odpovídá pokyny pro sada instrukcí Pentium Pro, včetně sady instrukcí MMX, SSE, SSE2 a SSE3. Neobsahuje žádné další volitelné IA-32 instrukce sada rozšíření, jako:

-  MOVBE instrukcí.
-  Další rozšíření SSE3 (SSSE3).
-  všechny variant SSE4.


**Poznámka:** Google TV, i když je spuštěna na x86, není podporována pro Android NDK nebo



#### <a name="x8664"></a>x86_64

Toto je název ABI pro procesory, které podporují x86 64-bit sada instrukcí (také označuje jako *x64* nebo *AMD64*). Xamarin.Android 5.1 poskytuje experimentální podporu pro tato architektura (Další informace najdete v tématu [povolenými experimentálními funkcemi](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features)).


#### <a name="mips"></a>MIPS

Toto je název ABI pro na základě MIPS procesorů, které podporují alespoň `MIPS32r1` sada instrukcí. Ani MIPS 16 ani `micromips` podporují Android.

**Poznámka:** MIPS zařízení aktuálně nepodporuje Xamarin.Android, ale bude v budoucí verzi.



#### <a name="apk-file-format"></a>Soubor formátu APK

Balíček aplikace Android je formát souboru, který obsahuje všechny kód, prostředků, prostředků a certifikáty potřebné pro aplikace pro Android. Je `.zip` , ale používá soubor `.apk` příponou názvu souboru. Po rozbalení obsahu `.apk` vytvořené Xamarin.Android si můžete prohlédnout ve na následující snímek obrazovky:

[![Obsah .apk](multicore-devices-images/00.png)](multicore-devices-images/00.png#lightbox)

Rychlé popis obsahu `.apk` souboru:

-   **AndroidManifest.xml** &ndash; jde `AndroidManifest.xml` soubor v binárním formátu XML.

-   **Classes.Dex** &ndash; obsahuje kód aplikace, kompilovat do `dex` formát souboru, který je používán Android runtime virtuálního počítače.

-   **Resources.arsc** &ndash; tento soubor obsahuje všechny předkompilovaných prostředky pro aplikaci.

-   **lib** &ndash; tento adresář obsahuje zkompilovaný kód pro každý ABI. Bude obsahovat jeden podsložky pro každou ABI, která byla popsána v předchozí části. Na snímku obrazovky výše `.apk` dotyčném obsahuje nativní knihovny pro obě `armeabi-v7a` a `x86` .

-   **META-INF** &ndash; tento adresář (pokud existuje) se používá k ukládání podpisový informace, balíček a rozšíření konfigurační data.

-   **res** &ndash; tento adresář obsahuje prostředky, které nebyly zkompilovat do `resources.arsc` .

> [!NOTE]
> Soubor `libmonodroid.so` je nativní knihovny požadovaných všech aplikací Xamarin.Android.



#### <a name="android-device-abi-support"></a>Podpora zařízení se systémem Android ABI

Každé zařízení se systémem Android v až dvě bis podporuje provádění nativního kódu:

-   **"Primární" ABI** &ndash; odpovídá kód počítače použité pro bitovou kopii systému.

-   **"Sekundární" ABI** &ndash; Toto je volitelné ABI, který taky podporuje bitovou kopii systému.


Například typický zařízení ARMv5TE pouze mít primární ABI z `armeabi`, zatímco zařízení s ARMv7 by zadejte primární ABI z `armeabi-v7a` a sekundární ABI z `armeabi`. Typické x86 zařízení by určit pouze primární ABI z `x86`.


### <a name="android-native-library-installation"></a>Knihovna pro Android nativní instalace

Během instalace balíčku, nativní knihovny v rámci `.apk` jsou extrahovány do adresáře nativní knihovny aplikace, obvykle `/data/data/<package-name>/lib`a jsou následně označovány jako `$APP/lib`.

Chování instalace nativní knihovny pro Android se výrazně liší mezi verzemi systému Android.



#### <a name="installing-native-libraries-pre-android-40"></a>Instalaci nativní knihovny: Před Android 4.0

Android starší než 4.0 Zmrzlinová Sandwichovy pouze extrahuje nativní knihovny z *jednotné ABI* v rámci `.apk`. Aplikace pro Android z této ročníku se nejprve pokusí extrahujte všechny nativní knihovny pro primární ABI, a pokud neexistuje žádný takový knihovny, Android potom extrahujte všechny nativní knihovny pro sekundární ABI. Ne "slučování" se provádí.

Představte si třeba situaci, kde je aplikace nainstalována v `armeabi-v7a` zařízení. `.apk,` Který podporuje obě `armeabi` a `armeabi-v7a`, má následující ABI `lib` adresářů a souborů v ní:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Po instalaci bude obsahovat adresář nativní knihovny:

```shell
$APP/lib/libtwo.so # from the armeabi-v7a directory in the apk
```

Jinými slovy, ne `libone.so` je nainstalovaná. To může způsobit problémy, jako `libone.so` není k dispozici pro aplikaci načíst za běhu. Toto chování při neočekávaná, je zaznamená jako chyby a přeřazených jako "[funguje tak, jak má](http://code.google.com/p/android/issues/detail?id=9089)."

V důsledku toho, pokud je cílem Android verze starší než 4.0, je potřeba zadat *všechny* nativní knihovny pro *každý* ABI, která bude podporovat aplikace, který je `.apk` by měl obsahovat :

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libone.so
lib/armeabi-v7a/libtwo.so
```


#### <a name="installing-native-libraries-android-40-ndash-android-403"></a>Instalaci nativní knihovny: Android 4.0 &ndash; Android 4.0.3

Android 4.0 Zmrzlinová Sandwichovy změní logice extrakce. Ho bude výčet všechny nativní knihovny najdete v tématu Pokud basename souboru již byly extrahovány, a pokud jsou splněny obě následující podmínky, pak knihovně budou extrahována:

-   Nebyla již byly extrahovány ho.

-   Nativní knihovny ABI odpovídá ABI primární nebo sekundární cílové složky.


Splnění těchto podmínek umožňuje "slučování" chování; To znamená pokud bychom měli `.apk` s tímto obsahem:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Po instalaci, pak bude obsahovat adresář nativní knihovny:

```shell
$APP/lib/libone.so
$APP/lib/libtwo.so
```

Bohužel se toto chování je pořadí závislé, jak je popsáno v následujícím dokumentu - [problém 24321: Galaxy Nexus 4.0.2 používá armeabi nativního kódu při armeabi a armeabi v7a je součástí apk](http://code.google.com/p/android/issues/detail?id=25321).

Nativní knihovny jsou zpracovány "v pořadí" (jak je uvedeno ve, například rozbalte) a *nejprve odpovídat* je rozbalena. Vzhledem k tomu, `.apk` obsahuje `armeabi` a `armeabi-v7a` verze `libtwo.so`a `armeabi` je uvedena první, je `armeabi` verze, který se extrahuje, *není* `armeabi-v7a` verze:

```shell
$APP/lib/libone.so # armeabi
$APP/lib/libtwo.so # armeabi, NOT armeabi-v7a!
```

Kromě toho i pokud obě `armeabi` a `armeabi-v7a` bis nejsou zadány (jak je popsáno níže v části *deklarace podporované bis*), Xamarin.Android vytvoří následující element v.
`csproj`:

```xml
<AndroidSupportedAbis>armeabi,armeabi-v7a</AndroidSupportedAbis>
```

V důsledku toho `armeabi` `libmonodroid.so` bude nalezen nejprve v rámci `.apk`a `armeabi` `libmonodroid.so` bude ten, který se extrahuje, i když `armeabi-v7a` `libmonodroid.so` přítomen a optimalizované pro cíl. Můžete také výsledkem skrytého běhové chyby, jako `armeabi` není SMP bezpečné.


##### <a name="installing-native-libraries-android-404-and-later"></a>Instalaci nativní knihovny: Android 4.0.4 a novější

Android 4.0.4 změny logice extrakce: bude výčet všech nativních knihoven, čtení souboru basename, následně rozbalte primární verze ABI (pokud existuje), nebo sekundární ABI (pokud existuje). To umožňuje chování "slučování"; To znamená pokud bychom měli `.apk` s tímto obsahem:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Po instalaci, pak bude obsahovat adresář nativní knihovny:

```shell
$APP/lib/libone.so # from armeabi
$APP/lib/libtwo.so # from armeabi-v7a
```


### <a name="xamarinandroid-and-abis"></a>Xamarin.Android a bis 

Xamarin.Android podporuje následující architektury:

-  `armeabi`
-  `armeabi-v7a`
-  `x86`

Xamarin.Android poskytuje experimentální podporu pro následující architektury:

-  `arm64-v8a`
-  `x86_64`


Všimněte si, že jsou moduly runtime 64-bit *není* nutná k provozování vaší aplikace na 64bitových zařízeních. Další informace o povolenými experimentálními funkcemi v Xamarin.Android 5.1 najdete v tématu [povolenými experimentálními funkcemi](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features).

Xamarin.Android aktuálně neposkytuje podporu pro `mips`.



### <a name="declaring-supported-abis"></a>Deklarování podporované na ABI

Ve výchozím nastavení, použije výchozí Xamarin.Android `armeabi-v7a` pro **verze** sestavení a `armeabi-v7a` a `x86` pro **ladění** sestavení. Podpora pro různé bis lze nastavit pomocí možnosti projektu pro projekt Xamarin.Android. V sadě Visual Studio, to je možné nastavit **Android možnosti** stránky projektu **vlastnosti**v části **Upřesnit** kartě, jak je znázorněno na následujícím snímku obrazovky:

![Android možnosti rozšířené vlastnosti](multicore-devices-images/vs-abi-selections.png)


V sadě Visual Studio pro Mac, může být vybraný podporované architektury na **Android sestavení** stránka **možnosti projektu**v části **Upřesnit** kartě, jak je znázorněno v následujícím snímek obrazovky:

[![Podporované bis Android sestavení](multicore-devices-images/xs-abi-selections-sml.png)](multicore-devices-images/xs-abi-selections.png#lightbox)

Když může být nutné deklarovat další podporu ABI jako např. kdy existují některých situacích:

-   Nasazení aplikace do `x86` zařízení.

-   Nasazení aplikace do `armeabi-v7a` zařízení k zajištění zabezpečení vlákna.



## <a name="summary"></a>Souhrn

Tento dokument popsané různé architektury procesoru, může se systémem Android aplikace. Zavedly se binární rozhraní aplikace a jak se používají v Android pro podporu různorodých architektury procesoru.
Pak se diskutovat o tom, jak určit ABI podporu v aplikaci Xamarin.Android a zvýrazní problémy, které vznikají při použití aplikace Xamarin.Android na `armeabi-v7a` zařízení, které jsou určené jenom pro `armeabi`.


## <a name="related-links"></a>Související odkazy

- [Architektura MIPS](http://www.mips.com/products/product-materials/processor/mips-architecture)
- [ABI pro architekturu ARM (PDF)](http://infocenter.arm.com/help/topic/com.arm.doc.ihi0036b/IHI0036B_bsabi.pdf)
- [Android NDK](http://developer.android.com/tools/sdk/ndk/index.html)
- [Vystavení 9089:Nexus jeden – nenačte všechny nativní knihovny z armeabi, pokud existuje nejméně jedna knihovna v armeabi v7a](http://code.google.com/p/android/issues/detail?id=9089)
- [Problém 24321: Galaxy Nexus 4.0.2 používá armeabi nativního kódu při armeabi a armeabi v7a je součástí apk](http://code.google.com/p/android/issues/detail?id=25321)
