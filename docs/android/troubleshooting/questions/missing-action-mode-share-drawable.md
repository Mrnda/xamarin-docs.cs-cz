---
title: "Android.Support.v7.AppCompat - nalezen žádný prostředek, který odpovídá zadaným názvem: Line 'android: actionModeShareDrawable."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 07655587642c3e1aa94d035e76f6f6758340546d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - nalezen žádný prostředek, který odpovídá zadaným názvem: Line 'android: actionModeShareDrawable.

1. Zajistěte, aby že si stáhnout nejnovější funkce a také Android 5.0 (rozhraní API 21) SDK prostřednictvím Android SDK Manager.

2. Ujistěte se, že jsou kompilace aplikace s compileSdkVersion nastaven na hodnotu 21. Volitelně můžete nastavit targetSdkVersion na 21 také.

3. Pokud budete potřebovat předchozí verze, jako je například rozhraní API 19, stáhněte si prosím odpovídající verze nalezeným na stránce Nuget:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Poznámka:*: Pokud jste ručně nainstalovat prostřednictvím konzoly Správce balíčků, ujistěte se taky nainstalovat stejnou verzi Xamarin.Android.Support.v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Referenční dokumentace přetečení zásobníku: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Viz také

- [Které balíčky Android SDK si mám nainstalovat?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

