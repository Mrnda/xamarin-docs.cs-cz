---
title: Rozhraní .NET vložení v systému Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: topgenorth
ms.author: toopge
ms.date: 06/15/2018
ms.openlocfilehash: e90d1e6258d4cfd9c918c566c9e18c358ee7668a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067169"
---
# <a name="net-embedding-on-android"></a>Rozhraní .NET vložení v systému Android

V některých případech můžete přidat do existujícího projektu Android nativní knihovny Xamarin .NET. Chcete-li to provést, můžete použít [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/) nástroj, chcete-li vaše knihovna .NET do nativní knihovny, která lze začlenit do nativní aplikace Android založené na jazyce Java.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="xamarinandroid-requirements"></a>Požadavky na Xamarin.Android

Pro Xamarin.Android pro práci s .NET vložení budete potřebovat následující:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) nebo novější musí být nainstalován.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) nebo novější musí být nainstalován.

-   **Sady pro vývojáře Java** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější musí být nainstalován.


## <a name="using-embeddinator-4000"></a>Pomocí Embeddinator 4000

Používat knihovny .NET v projektu nativní Android, použijte následující postup:

1.  Vytvoření projektu C# knihovna pro Android.

2.  Nainstalujte [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Vyhledejte **Embeddinator 4000.exe** a přidejte ho do vaší **CESTU**. Příklad:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  Spusťte Embeddinator 4000 na sestavení knihovny. Příklad:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Použijte vygenerovaný soubor AAR v jazyce Java projekt v Android Studio.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="xamarinandroid-requirements"></a>Požadavky na Xamarin.Android

Pro Xamarin.Android pro práci s .NET vložení budete potřebovat následující:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) nebo novější musí být nainstalován.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) nebo novější musí být nainstalován.

-   **Sady pro vývojáře Java** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější musí být nainstalován.

-   **Monofonní** &ndash; [Mono 5.0](http://www.mono-project.com/download/) nebo novější musí být nainstalovaný (mono je nainstalován pomocí sady Visual Studio pro Mac).


## <a name="using-embeddinator-4000"></a>Pomocí Embeddinator 4000

Používat knihovny .NET v projektu nativní Android, použijte následující postup:

1.  Vytvoření projektu C# knihovna pro Android.

2.  Nainstalujte [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Vyhledejte **Embeddinator 4000.exe** a přidejte **mono** pro vaši cestu. Příklad:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  Spusťte Embeddinator 4000 na sestavení knihovny. Příklad:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Použijte vygenerovaný soubor AAR v jazyce Java projekt v Android Studio.

-----

Možnosti příkazového řádku a využití jsou popsané v [Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) dokumentaci.


## <a name="callbacks"></a>Zpětná volání

Další informace o [volání mezi C# a Java](callbacks.md).

## <a name="samples"></a>Ukázky kódu

* [Počasí ukázkové aplikace](https://github.com/jamesmontemagno/embeddinator-weather)
