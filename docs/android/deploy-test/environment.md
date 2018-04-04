---
title: Xamarin.Android Environment
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: ebac7bfe826388de83fedc4be5f268773ca2526b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinandroid-environment"></a>Xamarin.Android Environment

## <a name="execution-environment"></a>Prostředí pro spuštění

*Prostředí pro spuštění* je sada proměnných prostředí a vlastnosti systému Android, které ovlivňují spuštění programu. Vlastnosti systému Android můžete nastavit `adb shell setprop` příkaz, zatímco proměnné prostředí můžete nastavit, že nastavení `debug.mono.env` vlastnost systému:

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Vlastnosti systému Android se nastavují pro všechny procesy na cílovém zařízení.

Od verze Xamarin.Android 4.6, vlastnosti systému a proměnných prostředí může nastavit nebo přepsán pro jednotlivé aplikace tak, že přidáte *prostředí soubor* do projektu. Soubor prostředí je soubor ve formátu Unix prostého textu se [ **akce sestavení** z `AndroidEnvironment` ](~/android/deploy-test/building-apps/build-process.md).
Soubor prostředí obsahuje řádky s formátem *klíč = hodnota*.
Komentáře jsou řádky, které začínat `#`. Prázdné řádky budou ignorovány.

Pokud *klíč* začíná velkým písmenem, pak *klíč* je považován za proměnné prostředí a **setenv**(3) se používá k nastavení proměnné prostředí pro zadaný *hodnotu* během spuštění procesu.

Pokud *klíč* začíná malým písmenem, pak *klíč* je považován za ve vlastnosti systému Android a *hodnotu* je *výchozí hodnota*: Vlastnosti systému Android, které řídí chování při spuštění Xamarin.Android jsou prohledávat první z úložiště vlastností systému Android, a pokud není zadána hodnota z hodnoty zadané v souboru prostředí použít. Toto nastavení slouží k povolení `adb shell setprop` který se má použít k přepsání hodnoty, které pocházejí z prostředí soubor k diagnostickým účelům.

## <a name="xamarinandroid-environment-variables"></a>Proměnné prostředí Xamarin.Android

Podporuje Xamarin.Android `XA_HTTP_CLIENT_HANDLER_TYPE` proměnné, které může být nastaven buď prostřednictvím `adb shell setprop debug.mono.env` nebo prostřednictvím `$(AndroidEnvironment)` akce sestavení.


### `XA_HTTP_CLIENT_HANDLER_TYPE`

Sestavení kvalifikovaný typu, který musí dědit z [HttpMessageHandler](https://docs.microsoft.com/dotnet/api/system.net.http.httpmessagehandler?view=xamarinandroid-7.1) a je vytvořený z [ `HttpClient()` výchozí konstruktor](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient.-ctor?view=xamarinandroid-7.1#System_Net_Http_HttpClient__ctor).

V Xamarin.Android 6.1, tato proměnná prostředí není nastavena ve výchozím nastavení, a [HttpClientHandler](https://docs.microsoft.com/dotnet/api/system.net.http.httpclienthandler?view=xamarinandroid-7.1) se použije.

Alternativně hodnota `Xamarin.Android.Net.AndroidClientHandler` může být určen používat [ `java.net.URLConnection` ](https://developer.xamarin.com/api/type/Java.Net.URLConnection/) pro přístup k síti, která *může* povolení použití protokolu TLS 1.2, pokud ji podporuje Android.

Přidat v Xamarin.Android 6.1.

## <a name="xamarinandroid-system-properties"></a>Xamarin.Android System Properties

Xamarin.Android podporuje následující vlastnosti systému, které může být nastaven buď prostřednictvím `adb shell setprop` nebo prostřednictvím `$(AndroidEnvironment)` akce sestavení.

* `debug.mono.debug`
* `debug.mono.env`
* `debug.mono.gc`
* `debug.mono.log`
* `debug.mono.max_grefc`
* `debug.mono.profile`
* `debug.mono.runtime_args`
* `debug.mono.trace`
* `debug.mono.wref`
* `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

Hodnota `debug.mono.debug` vlastnost systému je celé číslo. Pokud `1`, pak chování "jako v případě" proces nebyly spuštěny s `mono --debug`.
Obecně to zobrazí informace o souboru a řádku v trasování zásobníku, atd., bez nutnosti, že aplikace spustit z ladicí program.

### `debug.mono.env`

Obsahuje `|`-oddělený seznam proměnných prostředí.

### `debug.mono.gc`

Hodnota `debug.mono.debug` vlastnost systému je celé číslo.
Pokud `1`, pak by se mělo protokolovat informace GC.

Jde o ekvivalent na situaci, kdy `debug.mono.log` vlastnost systému obsahovat `gc`.

### `debug.mono.log`

Ovládací prvky, které další informace budou protokolovány Xamarin.Android `adb logcat`.
Jedná se o řetězec oddělených čárkami (`,`), který obsahuje jeden z následujících hodnot:

* `all`: Tiskové out *všechny* zprávy. To je málokdy vhodné, zahrnuje `lref` zprávy.
* `assembly`: Tiskové out `.apk` a sestavení analýza zprávy.
* `gc`: Vytiskněte zprávy týkající se globální Katalog.
* `gref`: Vytiskněte JNI globální odkaz zprávy.
* `lref`: Vytiskněte JNI místní odkaz zprávy.  
    *Poznámka:*: budou *skutečně* nevyžádané pošty `adb logcat`.  
    V Xamarin.Android 5.1, tím se také vytvoří `.__override__/lrefs.txt` souboru, který můžete získat *obrovská*.  
    Vyhněte se.
* `timing`: Vytiskněte některé informace o metodě časování. Tím se také vytvoří soubory `.__override__/methods.txt` a `.__override__/counters.txt`.


### `debug.mono.max_grefc`

Hodnota `debug.mono.max_grefc` vlastnost systému je celé číslo.
To je hodnota *přepsání* výchozí zjistil maximální počet GREF pro cílové zařízení.

*Je potřeba počítat s:* to lze použít pouze s `adb shell setprop
debug.mono.max_grefc` jako hodnota nebudete mít k dispozici v čase s **environment.txt** souboru.

### `debug.mono.profile`

`debug.mono.profile` Vlastnost systému umožní profileru.
Odpovídá a používá stejné hodnoty jako, `mono --profile` možnost. (Viz [ **mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) man stránka Další informace.)

### `debug.mono.runtime_args`

`debug.mono.runtime_args` Systému vlastnost obsahuje další možnosti, které by měl být analyzuje **mono**.

### `debug.mono.trace`

`debug.mono.trace` Vlastnost systému umožní trasování.
Odpovídá a používá stejné hodnoty jako, `mono --trace` možnost. (Viz [ **mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) man stránka Další informace.)

Obecně platí *nepoužívejte*. Použití trasování bude spamu `adb logcat` výstupu severaly zpomalit chování programu a změnit chování programu (až a včetně přidání další chybové stavy).

*Někdy*, ale umožňuje provést některé další šetření...

### `debug.mono.wref`

`debug.mono.wref` Vlastnost systému umožňuje přepsání výchozího zjistil mechanismus JNI slabé odkaz. Existují dvě podporované hodnoty:

* `jni`: Použijte JNI slabé odkazy, jak vytvořené `JNIEnv::NewWeakGlobalRef()` a zničení podle `JNIEnv::DeleteWeakGlobalREf()`.
* `java`: Použití JNI globální odkaz, na které odkazuje na `java.lang.WeakReference` instance.

`java` použití, ve výchozím nastavení, až až 7 rozhraní API a na rozhraní API-19 (Kit Kat) s obrázky povoleno. (API-8 přidat `jni` odkazy a obrázky *překročila* `jni` odkazy.)

Tato vlastnost systému je užitečná pro účely testování a určité formy šetření.
*Obecně*, neměli byste ji měnit.

### <a name="xahttpclienthandlertype"></a>XA\_HTTP\_KLIENTA\_OBSLUŽNÁ RUTINA\_TYPU

Poprvé Xamarin.Android 6.1, tato proměnná prostředí deklaruje výchozí `HttpMessageHandler` implementace, která budou používána `HttpClient`. Ve výchozím nastavení se tato proměnná není nastavená, a bude používat Xamarin.Android `HttpClientHandler`.

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> Základní zařízení s Androidem musí podporovat protokol TLS 1.2.
Android 5.0 a novější podpora protokolu TLS 1.2


## <a name="example"></a>Příklad

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.


## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```



## <a name="related-links"></a>Související odkazy

- [Transport Layer Security](~/cross-platform/app-fundamentals/transport-layer-security.md)
