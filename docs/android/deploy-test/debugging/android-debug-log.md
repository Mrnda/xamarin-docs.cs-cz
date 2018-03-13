---
title: "Protokol pro Android ladění"
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26ac42e4b7acbe19dee746130fc335fdf18ffc46
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="android-debug-log"></a>Protokol pro Android ladění

## <a name="android-debug-log-overview"></a>Přehled protokolu Android ladění

Jeden velmi běžné efektu vývojáři použít k ladění aplikací je pro volání do `Console.WriteLine`. Však na mobilní platformu jako Android neexistuje žádné konzoly. Zařízení se systémem Android poskytuje protokolu, který můžete použít při zápisu aplikace. To se někdy označuje jako "logcat' z důvodu příkaz, který zadáte, se jej načíst.

## <a name="accessing-from-visual-studio"></a>Přístup k ze sady Visual Studio

V sadě Visual Studio 2015 a vyšší Android a iOS jsou unified dotyková zařízení protokolu.

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 & 2017

Nové okno zařízení protokolu nástroje pro sadu Visual Studio zobrazí v protokolech pro zařízení s Androidem a iOS. Dá se spuštěním některý z následujících příkazů: 

-   **Zobrazení > jiných Windows > protokolu zařízení**
-   **Nástroje > Android > protokolu zařízení**
-   **Android nástrojů > protokolu zařízení**

Jakmile se zobrazí okno nástroje fyzického zařízení je možné vybrat z pole se seznamem zařízení. Pokud je vybraná zařízení, automaticky spustí přidání položky protokolu ze spuštěné aplikaci v tabulce. Přepínání mezi zařízení zastavit a spustit protokolování zařízení. V pořadí pro zařízení, než se objeví v komponenty combobox musí být načteny projekt pro Android. Pokud zařízení nezobrazí v komponentě combobox, zkontrolujte nejprve, pokud je k dispozici v spustit ladění rozevíracího seznamu. 

Toto okno Nástroj obsahuje: tabulku položky protokolu, pole se seznamem pro výběr zařízení, způsob, jak zrušit položky protokolu, vyhledávacího pole a přehrát či zastavit nebo pozastavit tlačítky. 



## <a name="accessing-from-the-command-line"></a>Přístup k z příkazového řádku

Další možnost, chcete-li zobrazit protokol ladění je prostřednictvím příkazového řádku. Otevřete okno konzoly a přejděte do složky nástrojů platformy Android SDK (jako je **C:\android-sdk-windows\platform-tools**). 

Pokud jenom jedno zařízení je připojen, lze zobrazit v protokolu s:

```shell
$ adb logcat
```

Pokud je připojeno více než jedno zařízení, musí identifikovat zařízení. Například `adb -d logcat` obsahuje protokol jenom fyzické zařízení připojené, při `adb -e logcat` obsahuje protokol pouze spuštěným emulátorem. 

Další příkazy najdete právě spuštěním **adb**.



## <a name="writing-to-the-debug-log"></a>Zápis do protokolu ladění

Může být zprávy zapisovány do protokolu ladění pomocí metody na [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) třídy: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

Tímto se vytvoří:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```


## <a name="interesting-messages"></a>Zajímavé zprávy

Při čtení protokolu a zejména v případě, že poskytuje fragmenty protokolu pro ostatní (jako poskytuje úplné soubor protokolu je přehnaně (1) a (2) nejde přečíst), *nejdůležitější* řádek začínat je tvaru:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Konkrétně řádek odpovídající regulárnímu výrazu:

```shell
^I.*ActivityManager.*Starting: Intent
```

a který obsahuje název balíčku aplikace. Toto je řádek, který odpovídá spuštění aktivity, a *většina* (ale ne všechny) z následujících zpráv by se měly týkat aplikace. 

Konkrétně každých zpráva obsahuje identifikátor procesu (pid) procesu generování zprávy. Ve výše `ActivityManager` zprávy, proces `12944` vygeneroval zprávu. Pokud chcete zjistit, který proces je proces laděné aplikace, vyhledejte mono. MonoRuntimeProvider zpráva: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Tato zpráva pochází z proces, který byl spuštěn, a všechny následující zprávy, které obsahují stejné pid pocházet ze stejného procesu. 
