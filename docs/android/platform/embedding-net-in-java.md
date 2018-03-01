---
title: "Vložení .NET v jazyce Java"
description: "Postup použití knihovny Xamarin .NET v založené na jazyce Java nativní Android projektu"
ms.topic: article
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 1a25f4bc39e39ce58a07ed399082bf13284c16e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="embedding-net-in-java"></a>Vložení .NET v jazyce Java

V některých případech můžete přidat do existujícího projektu Android nativní knihovny Xamarin .NET. Chcete-li to provést, můžete použít [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) nástroj, chcete-li vaše knihovna .NET do nativní knihovny, která lze začlenit do nativní aplikace Android založené na jazyce Java.

 
## <a name="requirements"></a>Požadavky

Použití Embeddinator-4000 s Javou v systému Android, budete potřebovat následující:

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) nebo novější musí být nainstalován.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.4.99](https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android/lastSuccessfulBuild/Azure/) nebo novější musí být nainstalován.

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
