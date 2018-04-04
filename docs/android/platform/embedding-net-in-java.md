---
title: Vložení .NET v jazyce Java
description: Postup použití knihovny Xamarin .NET v založené na jazyce Java nativní Android projektu
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2018
ms.openlocfilehash: 0ca855975db4b20fe80e98e2e818b8fde27f558c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="embedding-net-in-java"></a>Vložení .NET v jazyce Java

V některých případech můžete přidat do existujícího projektu Android nativní knihovny Xamarin .NET. Chcete-li to provést, můžete použít [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) nástroj, chcete-li vaše knihovna .NET do nativní knihovny, která lze začlenit do nativní aplikace Android založené na jazyce Java.

 
## <a name="requirements"></a>Požadavky

Použití Embeddinator-4000 s Javou v systému Android, budete potřebovat následující:

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) nebo novější musí být nainstalován.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) nebo novější musí být nainstalován.

-   **Sady pro vývojáře Java** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější musí být nainstalován.

-   **Mono** &ndash; [Mono 5.0](http://www.mono-project.com/download/) nebo novější musí být nainstalován.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio můžete upravit a kompilaci kódu C#.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio pro Mac můžete upravit a kompilaci kódu C#.

-----

 
## <a name="using-the-embeddinator-4000"></a>Pomocí Embeddinator-4000

Používat knihovny .NET v projektu nativní Android, použijte následující kroky:

1.  Vytvoření projektu C# knihovna pro Android.

2.  Nainstalujte Embeddinator 4000 prostřednictvím balíčku NuGet.

3.  Spusťte Embeddinator na sestavení knihovna pro Android.

4.  Použijte vygenerovaný soubor AAR v jazyce Java projekt v Android Studio.

Tyto kroky jsou podrobně popsány v [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/getting-started-java-android.html) dokumentaci.
