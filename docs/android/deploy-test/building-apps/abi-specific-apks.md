---
title: Sestavování specifických ABI APKs
description: Tento dokument popisuje jak sestavit APK, který bude cílit na jednom ABI pomocí Xamarin.Android.
ms.prod: xamarin
ms.assetid: D21B195B-4530-4EB2-8704-5C4349A2CDD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 89a78c8dd1243dcfea9d14bd9758c5403d1d21ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="building-abi-specific-apks"></a>Sestavování specifických ABI APKs

_Tento dokument popisuje jak sestavit APK, který bude cílit na jednom ABI pomocí Xamarin.Android._



## <a name="overview"></a>Přehled

V některých situacích může být výhodné pro aplikaci tak, aby měl více APKs – každý APK je podepsaná pomocí stejné úložiště klíčů a má stejný název balíčku, ale je zkompilovaném pro určité zařízení nebo konfigurace pro Android. Toto není doporučený postup – je mnohem jednodušší tak, aby měl jeden APK, který může podporovat více zařízení a konfigurace. Existují některé situace, kdy vytváření více APKs může být užitečná, například:

-  **Snížení velikosti APK** -Google Play ukládá omezení velikosti 100 MB na APK soubory. Vytváření zařízení konkrétní APK může snížit velikost APK jako potřebujete zadat podmnožinu prostředky a prostředky pro aplikaci.

-  **Podporují různé architektury procesoru** – Pokud je vaše aplikace má sdílené knihovny pro konkrétní procesoru, můžete distribuovat pouze sdílené knihovny pro tuto procesoru.


Více APKs může zkomplikovat distribuce - potíže, které řeší pomocí Google Play. Google Play zajistí, že správné APK doručuje do zařízení podle verze kódu aplikace a další metadata obsažené s **AndroidManifest.XML**. Konkrétní podrobnosti a omezení na tom, jak Google Play podporuje více APKs pro aplikaci, naleznete [Google dokumentaci o podpoře několika APK](http://developer.android.com/google/play/publishing/multiple-apks.html).

Tato příručka vyřeší postup vytvoření skriptu více APKs pro aplikace pro Xamarin.Android, každý APK cílení na konkrétní ABI. Bude se vztahovat v následujících tématech:

1.  Vytvořit jedinečný *kód verze* pro APK.
1.  Vytvořit dočasný verzi **AndroidManifest.XML** který se použije pro tuto APK.
1.  Sestavení aplikace pomocí **AndroidManifest.XML** z předchozího kroku.
1.  Příprava APK pro verzi podepisování a zip zarovnání ho.


Na konci tohoto průvodce je návod, který popisuje, jak tyto kroky, pomocí skriptu [převislým](http://martinfowler.com/articles/rake.html).



### <a name="creating-the-version-code-for-the-apk"></a>Vytváření kódu verze pro APK

Google doporučuje konkrétní algoritmus pro verzi kód, který používá verzi code sedm číslice (podrobnosti najdete v části *používá schéma verze kódu* v [dokument podpory více APK](http://developer.android.com/google/play/publishing/multiple-apks.html)).
Rozbalením této verze schématu kódu na osm číslic, je možné obsahovala nějaké informace ABI do verze kód, který zajistí, že Google Play provede distribuci správné APK do zařízení. Následující seznam popisuje to osm formátu číslic, která verze kódu (indexované zleva doprava):

-   **Index 0** (červeně v následujícím diagramu) &ndash; celé pro ABI:
    -   1 &ndash; `armeabi`
    -   2 &ndash; `armeabi-v7a`
    -   6 &ndash; `x86`

-   **Index 1 – 2** (oranžová v následujícím diagramu) &ndash; minimální úroveň API aplikací podporováno.

-   **Index 3-4** (modré v následujícím diagramu) &ndash; velikosti obrazovky podporované:
    -   1 &ndash; malé
    -   2 &ndash; normální
    -   3 &ndash; velké
    -   4 &ndash; xlarge

-   **Index 5 – 7** (zeleně v následujícím diagramu) &ndash; jedinečné číslo verze kódu. 
    Toto nastavení sám vývojář. Ho měli zvýšit pro jednotlivé veřejné verze aplikace.

Následující diagram znázorňuje pozici každý kód popsané v seznamu výše:

[![Diagram formát kódu osm číslice verze programového pomocí barev](abi-specific-apks-images/image00.png)](abi-specific-apks-images/image00.png#lightbox)


Google Play zajistí, že je správný APK doručit do zařízení na základě `versionCode` a APK konfigurace. APK nejvyšší kódem bude doručen do zařízení. Například aplikace může mít tři APKs s následujícími kódy verze:

-  11413456-ABI je `armeabi` ; které se budou zaměřovat API úroveň 14; malá na velké obrazovky; s číslem verze 456.
-  21423456-ABI je `armeabi-v7a` ; které se budou zaměřovat API úroveň 14; normální &amp; velké obrazovky; s číslem verze 456.
-  61423456-ABI je `x86` ; které se budou zaměřovat API úroveň 14; normální &amp; velké obrazovky; s číslem verze 456.

Chcete-li pokračovat v tomto příkladu, představte si, že chyby byla opravena, která byla specifické pro `armeabi-v7a`. Verze aplikace zvýší na 457 a nové APK je integrované `android:versionCode` nastavena na 21423457. VersionCodes pro `armeabi` a `x86` verze by zůstávají stejné.

Nyní Představte si, že x86 verze obdrží některé aktualizace nebo chyb opravy, které cílí na novější rozhraní API (API úrovně 19), proto je tato verze 500 aplikace. Nové `versionCode` změní na 61923500 při armeabi/armeabi-v7a zůstanou beze změny. V tomto okamžiku by byl kódy verzí:

-  11413456-ABI je `armeabi` ; které se budou zaměřovat API úroveň 14; malá na velké obrazovky; s názvem 456 a verze.
-  21423457-ABI je `armeabi-v7a` ; které se budou zaměřovat API úroveň 14; normální &amp; velké obrazovky; s názvem verze 457.
-  61923500-ABI je `x86` ; které se budou zaměřovat API úrovně 19; normální &amp; velké obrazovky; s názvem verze 500.


Zachování tyto kódy verze ručně, může být významné zátěž vývojář. Proces výpočtu správného `android:versionCode` a pak vytváření APK dobré automatizovat.
Příklad toho, jak to udělat, se budeme v tomto návodu na konci tohoto dokumentu.


### <a name="create-a-temporary-androidmanifestxml"></a>Vytvořit dočasný AndroidManifest.XML

I když je to nezbytně nutné, vytváření je dočasná **AndroidManifest.XML** pro každý ABI může pomoct zabránit problémům, které mohou nastat u úniku informací z jedné APK na druhý. Například je velmi důležitý, `android:versionCode` atribut je jedinečný pro každý APK.

Jak to provést, závisí na skriptování systému související se situací, ale obvykle zahrnuje načtení kopie Android manifest použít během vývoje, úprava ho a potom pomocí upravit manifestu během procesu vytváření.



### <a name="compiling-the-apk"></a>Kompilování APK

Vytváření APK za ABI se nejlépe provádí pomocí `xbuild` nebo `msbuild` jak je znázorněno v následující ukázkový příkaz:

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:Package /p:AndroidSupportedAbis=<TARGET_ABI> /p:IntermediateOutputPath=obj.<TARGET_ABI>/ /p:AndroidManifest=<PATH_TO_ANDROIDMANIFEST.XML> /p:OutputPath=bin.<TARGET_ABI> /p:Configuration=Release <CSPROJ FILE>
```

Následující seznam popisuje jednotlivé parametry příkazového řádku:

-   `/t:Package` &ndash; Vytvoří Android APK, která je podepsaná pomocí úložiště klíčů ladění

-   `/p:AndroidSupportedAbis=<TARGET_ABI>` &ndash; Tato ABI k cíli. Jeden z musí `armeabi`, `armeabi-v7a`, nebo `x86`

-   `/p:IntermediateOutputPath=obj.<TARGET_ABI>/` &ndash; Toto je adresář, který bude obsahovat zprostředkující soubory, které jsou vytvořené jako součást sestavení. Pokud potřeby Xamarin.Android vytvoří adresář s názvem po ABI, jako například `obj.armeabi-v7a`. Doporučujeme použít jednu složku pro každý ABI to zabrání problémy, které výsledek se soubory "úniku" z jednoho sestavení do jiného. Všimněte si, že je tato hodnota byla ukončena s oddělovač adresářů ( `/` v případě OS X).

-   `/p:AndroidManifest` &ndash; Tato vlastnost určuje cestu ke **AndroidManifest.XML** soubor, který se použije během sestavení.

-   `/p:OutputPath=bin.<TARGET_ABI>` &ndash; Toto je adresář, který bude obsahovat konečné APK. Xamarin.Android vytvoří adresář s názvem po ABI, například `bin.armeabi-v7a`.

-   `/p:Configuration=Release` &ndash; Vytvořit nové verze sestavení APK. Ladicí sestavení může nelze nahrát na web Google Play.

-   `<CS_PROJ FILE>` &ndash; To je cesta k `.csproj` soubor projektu Xamarin.Android.



### <a name="sign-and-zipalign-the-apk"></a>Přihlášení a Zipalign APK

Je nutné se přihlásit APK předtím, než mohou být distribuovány prostřednictvím webu Google Play. To lze provést pomocí `jarsigner` aplikace, která je součástí sady pro vývojáře Java. Následující postup použijte příkaz demonstrats řádku `jarsigner` na příkazovém řádku:

```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <PATH/TO/KEYSTORE> -storepass <PASSWORD> -signedjar <PATH/FOR/SIGNED_JAR> <PATH/FOR/JAR/TO/SIGN> <NAME_OF_KEY_IN_KEYSTORE>
```

Všechny aplikace Xamarin.Android musí být zip zarovnaný ještě před jejich spuštěním na zařízení. To je formát příkazového řádku používat:

```shell
zipalign -f -v 4 <SIGNED_APK_TO_ZIPALIGN> <PATH/TO/ZIP_ALIGNED.APK>
```


## <a name="automating-apk-creation-with-rake"></a>Automatizace vytváření APK s převislým

Ukázkový projekt [OneABIPerAPK](https://github.com/xamarin/monodroid-samples/tree/master/OneABIPerAPK) je jednoduchý projekt pro Android, která ukazují, jak k výpočtu ABI konkrétní verzi a sestavení tři samostatné APK je pro každou z následujících ABI:

-  armeabi
-  armeabi-v7a
-  x86


[Rakefile](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb) v ukázkovém projektu provede všechny kroky popsané v předchozích částech:

1. [Vytvoření android: versionCode](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L30) pro APK.

1. [Zápis android: versionCode](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L55) na vlastní **AndroidManifest.XML** pro tento APK.

1. [Kompilace sestavení pro vydání](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L63) projektu Xamarin.Android, který bude cílit jednotlivě ABI a pomocí **AndroidManifest.XML** který byl vytvořen v předchozím kroku.

1. [Přihlaste APK ](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L66) s úložiště klíčů produkční.

1. [Zipalign](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L67) APK.


Chcete-li vytvořit všechny APKs pro aplikaci, spusťte `build` převislým úloh z příkazového řádku:

```shell
$ rake build
==> Building an APK for ABI armeabi with ./Properties/AndroidManifest.xml.armeabi, android:versionCode = 10814120.
==> Building an APK for ABI x86 with ./Properties/AndroidManifest.xml.x86, android:versionCode = 60814120.
==> Building an APK for ABI armeabi-v7a with ./Properties/AndroidManifest.xml.armeabi-v7a, android:versionCode = 20814120.
```

Po dokončení úlohy převislým, bude tři `bin` složky se souborem `xamarin.helloworld.apk`. Další snímek obrazovky ukazuje každou z těchto složek s jejich obsah:

[![Umístění složky specifické pro platformu obsahující xamarin.helloworld.apk](abi-specific-apks-images/image01.png)](abi-specific-apks-images/image01.png#lightbox)


> [!NOTE]
> Proces sestavení uvedených v této příručce mohou být prováděny v jednom z mnoha různých sestavení systémy. I když nemáme předem napsané příklad, je také třeba umožnit s [prostředí Powershell](http://technet.microsoft.com/en-ca/scriptcenter/powershell.aspx) / [psake](https://github.com/psake/psake) nebo [zfalšovat](http://fsharp.github.io/FAKE/).


## <a name="summary"></a>Souhrn

Tato příručka poskytuje některé návrhy s vytvoření Android APK, které cílí zadejte ABI. Popsány i jeden možné schéma pro vytváření `android:versionCodes` architekturu procesoru, která APK je určený pro který bude identifikovat. Průvodce zahrnuty ukázkový projekt, který má jeho sestavení skripty pomocí převislým.



## <a name="related-links"></a>Související odkazy

- [OneABIPerAPK (ukázka)](https://developer.xamarin.com/samples/OneABIPerAPK/)
- [Publikování aplikace](~/android/deploy-test/publishing/index.md)
- [Podpora více APK na web Google Play](http://developer.android.com/google/play/publishing/multiple-apks.html)
