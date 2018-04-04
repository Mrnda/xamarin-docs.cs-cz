---
title: Jaká verze Xamarin.Android přidat podporu typu Lupa?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 065c68a373f67bb352b59dc88ef89daec8b51ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Jaká verze Xamarin.Android přidat podporu typu Lupa?

**Poznámka:** Tento průvodce byl původně zapsán Android L ve verzi Preview.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) přidána podpora Android L Preview.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) přidána podpora Android typu Lupa.

Xamarin podporuje pouze aktivně aktuální stabilní verzi nástroje Xamarin. Níže uvedené informace jsou poskytovány "jako-je" pro starší verze nástroje. Nejnovější informace o verzích Xamarin, zkontrolujte [zde](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>"Chybějící android.jar pro úroveň rozhraní API 21" ve verzi Preview Android L

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Může zobrazí následující chybová zpráva (nebo podobnou):

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Tato zpráva znamená, že není nainstalována platformy Android SDK pro rozhraní API úroveň 21. Buď ji nainstalovat v Android SDK Manager (Nástroje > Otevřít na Android SDK Manager...), nebo změňte projekt Xamarin.Android cílovou verzi API, která je nainstalovaná.

Existuje několik řešení tohoto problému:

1. Změňte projekt tak, aby jeho cílem 19 rozhraní API nebo nižší.

2. Přejmenujte složku android 21 z android 21 pro android L. (V nejlépe by měl použít pouze jako dočasné opravu a vůbec nemusejí velmi dobře fungovat.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Dočasně ponížit zpět na Android API úrovně 21 "L" preview [1]:

    1.  Odstranit **LOCALAPPDATA %\\Android\\android-sdk\\platformy\\android 21** 
    2.  Extrahovat [1] do **C:\\uživatelé\\<username>\\data aplikací\\místní\\Android\\android-sdk\\platformy** k vytvoření **android-L** složky.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Může zobrazí následující chybová zpráva (nebo podobnou):

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

To znamená, že není nainstalována platformy Android SDK pro rozhraní API úroveň 21. Buď ji nainstalovat v Android SDK Manager (Nástroje > SDK správce...), nebo změňte projekt Xamarin.Android cílovou verzi API, která je nainstalovaná.

Existuje několik řešení tohoto problému:

1. Změňte projekt tak, aby jeho cílem 19 rozhraní API nebo nižší.

2. Přejmenujte složku android 21 z android 21 pro android L. (V nejlépe by měl použít pouze jako dočasné opravu a vůbec nemusejí velmi dobře fungovat.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Dočasně ponížit zpět na Android API úrovně 21 "L" preview [1]:

    1.  Delete **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  Extrahovat [1] do **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** vytvořit **android-L** složky.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
