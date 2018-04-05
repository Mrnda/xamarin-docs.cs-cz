---
title: Protokol pro Android ladění
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/04/2018
ms.openlocfilehash: e0e22fe35dc5042a7b3c895a250803e936611629
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/05/2018
---
# <a name="android-debug-log"></a>Protokol pro Android ladění

Jeden velmi běžné efektu vývojáři použít k ladění aplikací je pro volání do `Console.WriteLine`. Však na mobilní platformu jako Android neexistuje žádné konzoly. Zařízení se systémem Android poskytuje protokolu, který můžete použít při zápisu aplikace. To se někdy označuje jako _logcat_ z důvodu příkaz, který zadáte, se jej načíst. Použití **protokol ladění** nástroj k zobrazení dat protokolu.

## <a name="android-debug-log-overview"></a>Přehled protokolu Android ladění

**Protokol ladění** nástroj poskytuje způsob, jak zobrazit výstup protokolu při ladění aplikace pomocí sady Visual Studio. Protokol ladění podporuje následující zařízení:

-   Fyzické Android telefony, tablety a wearables.
-   Zařízení s Androidem virtuální systémem emulátor Google Android. 

> [!NOTE]
> **Protokol ladění** nástroj není kompatibilní s Xamarin Player za provozu.

**Protokol ladění** nezobrazí zprávy protokolu, které jsou generovány, když aplikace běží samostatné na zařízení (tj. když je odpojený ze sady Visual Studio).


## <a name="accessing-the-debug-log-from-visual-studio"></a>Přístup k protokolu ladění ze sady Visual Studio

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li otevřít **zařízení protokolu** nástroje, klikněte na tlačítko **zařízení protokolu (logcat)** ikonu na panelu nástrojů:

[![Umístění zařízení protokolu nástroje na panelu nástrojů](android-debug-log-images/vswin-01-logcat-sml.png)](android-debug-log-images/vswin-01-logcat.png#lightbox)

Alternativně spusťte **zařízení protokolu** nástroj z jednoho z následujících možností nabídky:

-   **Zobrazení > jiných Windows > protokolu zařízení**
-   **Nástroje > Android > protokolu zařízení**

Následující snímek obrazovky ukazuje různé součásti **ladění nástroj** okno:

[![Části okna nástroje ladění](android-debug-log-images/vswin-03-features-sml.png)](android-debug-log-images/vswin-03-features.png#lightbox)

-   **Selektor zařízení** &ndash; vybere které fyzického zařízení nebo spuštěného emulátoru k monitorování.

-   **Záznamy protokolu** &ndash; tabulku protokolu zpráv z logcat.

-   **Zrušte zaškrtnutí položky protokolu** &ndash; vymaže všechny aktuální položky protokolu z tabulky.

-   **Přehrát či pozastavit** &ndash; přepíná mezi aktualizace nebo pozastavení zobrazení nové položky protokolu.

-   **Zastavit** &ndash; zastaví zobrazení nové položky protokolu.

-   **Pole pro vyhledávání** &ndash; Enter hledání řetězců do tohoto pole můžete filtrovat podmnožinu záznamy protokolu.


Když **protokol ladění** se zobrazí okno nástroje, pomocí zařízení rozevírací nabídky vyberte zařízení s Androidem k monitorování:

[![Umístění zařízení selektor](android-debug-log-images/vswin-02-devices-combo-sml.png)](android-debug-log-images/vswin-02-devices-combo.png#lightbox)

Po výběru zařízení se **zařízení protokolu** nástroj automaticky přidá položky protokolu z spuštěné aplikaci &ndash; tyto položky protokolu jsou uvedené v v tabulce položky protokolu. Přepínání mezi zařízeními zastaví a spustí protokolování zařízení. Všimněte si, že projekt Android musí být načteny před veškerá zařízení, která se zobrazí v modulu pro výběr zařízení. Pokud zařízení nezobrazí v modulu pro výběr zařízení, ověřte, zda je k dispozici v sadě Visual Studio zařízení rozevírací nabídce vedle položky **spustit** tlačítko.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Chcete-li otevřít **zařízení protokolu**, klikněte na tlačítko **zobrazení > dotyková zařízení > zařízení protokolu**:

[![Umístění protokolu zařízení položky nabídky](android-debug-log-images/vsmac-01-logcat-sml.png)](android-debug-log-images/vsmac-01-logcat.png#lightbox)

Následující snímek obrazovky ukazuje různé součásti **ladění nástroj** okno:

[![Funkce okna Nástroj pro ladění](android-debug-log-images/vsmac-03-features-sml.png)](android-debug-log-images/vsmac-03-features.png#lightbox)

-   **Selektor zařízení** &ndash; vybere které fyzického zařízení nebo spuštěného emulátoru k monitorování.

-   **Záznamy protokolu** &ndash; tabulku protokolu zpráv z logcat.

-   **Zrušte zaškrtnutí položky protokolu** &ndash; vymaže všechny aktuální položky protokolu z tabulky.

-   **Pole pro vyhledávání** &ndash; Enter hledání řetězců do tohoto pole můžete filtrovat podmnožinu záznamy protokolu.

-   **Zobrazit zprávy** &ndash; Přepíná zobrazení informační zprávy.

-   **Zobrazit upozornění** &ndash; Přepíná zobrazení zprávy upozornění (zprávy upozornění se zobrazují v žlutý).

-   **Zobrazit chyby** &ndash; Přepíná zobrazení chybové zprávy (zprávy upozornění jsou zobrazené červeně).

-   **Znovu připojit** &ndash; znovu připojí k zařízení a aktualizuje zobrazení položka protokolu.

-   **Přidání značka** &ndash; vloží zprávu značky (například `--- Marker N ---`) po nejnovější položky protokolu, kde _N_ je čítač, který se spouští z 1 a zvýší o 1 při přidávání nové značky.

Když se zobrazí okno nástroje ladění protokolu, použijte rozevírací nabídce zařízení vybrat zařízení s Androidem k monitorování:

[![Umístění zařízení selektor](android-debug-log-images/vsmac-02-devices-combo-sml.png)](android-debug-log-images/vsmac-02-devices-combo.png#lightbox)

Po výběru zařízení se **zařízení protokolu** nástroj automaticky přidá položky protokolu z spuštěné aplikaci &ndash; tyto položky protokolu jsou uvedené v v tabulce položky protokolu. Přepínání mezi zařízeními zastaví a spustí protokolování zařízení. Všimněte si, že projekt Android musí být načteny před veškerá zařízení, která se zobrazí v modulu pro výběr zařízení. Pokud zařízení nezobrazí v modulu pro výběr zařízení, ověřte, zda je k dispozici v sadě Visual Studio zařízení rozevírací nabídce vedle položky **spustit** tlačítko.

-----


## <a name="accessing-from-the-command-line"></a>Přístup k z příkazového řádku

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Další možností je chcete zobrazit protokol ladění prostřednictvím příkazového řádku. Otevřete okno příkazového řádku a přejděte do složky nástrojů platformy Android SDK (obvykle se nachází v této složky sady SDK nástroje platformy **C:\\Program Files (x86)\\Android\\android-sdk\\ Nástroje platformy**).

Pokud jenom jedno zařízení (fyzického zařízení nebo emulátoru) je připojen, lze zobrazit v protokolu tak, že zadáte následující příkaz:

```shell
$ adb logcat
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Další možností je chcete zobrazit protokol ladění prostřednictvím příkazového řádku. Otevřete okno terminálu a přejděte do složky nástrojů platformy Android SDK (obvykle se nachází v této složky sady SDK nástroje platformy **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/platform-tools**).

Pokud jenom jedno zařízení (fyzického zařízení nebo emulátoru) je připojen, lze zobrazit v protokolu tak, že zadáte následující příkaz:

```shell
$ ./adb logcat
```

-----


Pokud je připojeno více než jedno zařízení, musí být explicitně identifikovány zařízení. Například **-d logcat adb** zobrazí protokol jenom fyzické zařízení připojené, při **adb -e logcat** obsahuje protokol pouze spuštěným emulátorem.

Další příkazy najdete tak, že zadáte **adb** a čtení zpráv nápovědy.


## <a name="writing-to-the-debug-log"></a>Zápis do protokolu ladění

Může být zprávy zapisovány do **protokol ladění** pomocí metody [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) třídy.
Příklad: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

To vytváří výstup podobný následujícímu:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

Je také možné použít `Console.WriteLine` k zápisu **protokol ladění** &ndash; tyto zprávy se zobrazují v logcat ve formátu mírně odlišný výstup (Tento postup je zvlášť užitečné při ladění aplikace Xamarin.Forms na Android):

```csharp
System.Console.WriteLine ("DEBUG - Button Clicked!");
```

To vytváří výstup podobný následujícímu v logcat:

```
Info (19543) / mono-stdout: DEBUG - Button Clicked!
```

## <a name="interesting-messages"></a>Zajímavé zprávy

Při čtení protokolu (a hlavně při poskytování protokolu fragmenty pro ostatní), perusing soubor protokolu v celé jeho šíři je často příliš náročná.
Aby bylo snazší procházet zprávy protokolu, spusťte hledání položka protokolu, která vypadá přibližně takto:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Konkrétně vyhledejte řádek, který vyhovuje regulární výraz, který také obsahuje název balíčku aplikace:

```shell
^I.*ActivityManager.*Starting: Intent
```

Toto je řádek, který odpovídá spuštění aktivity, a *většina* (ale ne všechny) z následujících zpráv by se měly týkat aplikace.

Všimněte si, že každá zpráva obsahuje identifikátor procesu (pid) procesu generování zprávy. Ve výše `ActivityManager` zprávy, proces `12944` vygeneroval zprávu. Pokud chcete zjistit, který proces je proces laděné aplikace, vyhledejte **mono. MonoRuntimeProvider** zpráva: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Tato zpráva pochází z proces, který byl spuštěn. Všechny následné zprávy, které obsahují tento pid pocházet ze stejného procesu.
