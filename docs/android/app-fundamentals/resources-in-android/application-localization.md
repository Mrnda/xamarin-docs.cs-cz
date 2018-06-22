---
title: Lokalizace aplikací a řetězcové prostředky
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: cfb127500f919b61788087465700dfed213d5eb2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765851"
---
# <a name="application-localization-and-string-resources"></a>Lokalizace aplikací a řetězcové prostředky

Lokalizace aplikací spočívá v poskytování alternativní prostředků cílit na konkrétní oblast nebo národní prostředí. Například můžete zadat řetězce lokalizovaných jazyků pro jednotlivé země, nebo můžete změnit barvy nebo rozložení tak, aby odpovídaly konkrétní jazykové verze. Android se načíst a používat prostředky, které jsou vhodné pro národní prostředí zařízení v době běhu bez uložení změn do zdrojového kódu.

Například následující obrázek ukazuje je stejná aplikace spuštěna v tři národní prostředí jiné zařízení, ale text zobrazovaný v každé tlačítko je specifická pro národní prostředí nastavené na každé zařízení:

[![Příklady tři různá národní prostředí](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

V tomto příkladu obsah soubor rozložení **Main.axml** vypadá takto:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

V příkladu nahoře řetězec pro tlačítko byl načten z prostředků tím, že poskytuje ID prostředku pro řetězec:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Řetězce prostředků pro tři jazyky](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Řetězce prostředků pro tři jazyky](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Lokalizace aplikací pro Android

Pro čtení [Úvod k lokalizaci](~/cross-platform/app-fundamentals/localization.md) pro tipy a Rady k lokalizace mobilní aplikace.

[Lokalizace aplikací pro Android](~/android/app-fundamentals/localization.md) Průvodce obsahuje konkrétnější příklady o tom, jak přeložit řetězce a lokalizaci obrázky pomocí Xamarin.Android.



## <a name="related-links"></a>Související odkazy

- [Lokalizace aplikací pro Android](~/android/app-fundamentals/localization.md)
- [Lokalizace a platformy – přehled](~/cross-platform/app-fundamentals/localization.md)
