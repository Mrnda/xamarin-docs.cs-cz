---
title: Funkce Sandwichovy Zmrzlinová
description: Tento článek popisuje některé z nových funkcí, které jsou k dispozici vývojářům aplikací pomocí rozhraní API systému Android 4 - Zmrzlinová Sandwichovy. Ho zahrnuje několik novou technologií, uživatelské rozhraní a pak prozkoumá celou řadu nových funkcích, které nabízí Android 4 pro sdílení dat mezi aplikacemi a mezi zařízeními.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 3c5412629f6dcd31fb0a82634ef530569eef459a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764928"
---
# <a name="ice-cream-sandwich-features"></a>Funkce Sandwichovy Zmrzlinová

_Tento článek popisuje některé z nových funkcí, které jsou k dispozici vývojářům aplikací pomocí rozhraní API systému Android 4 - Zmrzlinová Sandwichovy. Ho zahrnuje několik novou technologií, uživatelské rozhraní a pak prozkoumá celou řadu nových funkcích, které nabízí Android 4 pro sdílení dat mezi aplikacemi a mezi zařízeními._

## <a name="overview"></a>Přehled

Operační systém Android verze 4.0 (14 úroveň rozhraní API) představuje hlavní přebudování Android operačního systému a obsahuje některé důležité změny a upgrady, včetně:

-   **Aktualizované uživatelské rozhraní** – několik nových funkcí uživatelského rozhraní poskytují vývojářům více power a flexibilitu při vytváření aplikace uživatelské rozhraní. Tyto nové funkce patří: `GridLayout` , `PopupMenu` , `Switch` pomůcky, a `TextureView` . 
-   **Lepší hardwarovou akceleraci** – 2D vykreslování nyní proběhne na grafický procesor pro všechny Android ovládací prvky. Kromě toho hardwarové akcelerace, ve výchozím nastavení zapnutý, v všechny aplikace vyvinuté pro Android 4.0. 
-   **Nová rozhraní API Data** – je nové přístup k datům, který nebyl dříve oficiálně přístupný, jako je například kalendářní data a uživatelský profil vlastníka zařízení. 
-   **Aplikace pro sdílení dat** – sdílení dat mezi aplikacemi a zařízení je teď jednodušší než kdy prostřednictvím technologie, jako `ShareActionProvider` , který umožňuje snadné vytváření sdílení akce z zobrazí panel Akce a *Android světla* pro *komunikace NFC (Near Field)* , který umožňuje rychle sdílet data mezi zařízeními v těsné blízkosti navzájem. 


V tomto článku vytvoříme prozkoumat tyto funkce a jiné změny, které byly provedeny na rozhraní API systému Android 4.0 a vysvětlíme, jak používat jednotlivé funkce s Xamarin.Android.

## <a name="user-interface-features"></a>Funkce uživatelského rozhraní

Celou řadu nové uživatelské rozhraní technologie jsou k dispozici s Androidem 4, včetně:

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – podporuje 2D mřížky rozložení ovládacích prvků. 
-   **[Přepínač pomůcky](~/android/user-interface/controls/switch.md)**  – umožňuje při přepínání mezi ON nebo OFF. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – umožňuje video a OpenGL obsahu v rámci zobrazení. 
-   **[Navigační panel](~/android/user-interface/controls/navigation-bar.md)**  – obsahuje virtuální tlačítka Zpět, domácích a víceúlohových. 


Kromě toho byly vylepšeny další prvky uživatelského rozhraní, například `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, který je nyní snazší s ním pracovat a karty, které mají zajímavější vzhled.

## <a name="sharing-features"></a>Sdílení funkce

Android 4 obsahuje několik nové technologie, které dejte nám sdílet data mezi zařízeními a ve všech aplikacích. Také poskytuje přístup na různé typy dat, které nebyly dříve dostupné, jako je například informací z kalendáře a vlastník zařízení uživatelský profil. V této části, které se podíváme řadu funkcí, které nabízí Android 4 tuto adresu těchto oblastech, včetně:

-  **[Android světla](~/android/platform/android-beam.md)**  – dat umožňuje sdílení prostřednictvím NFC.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – vytvoří zprostředkovatele, který umožňuje vývojářům zadejte sdílení akce z panelu akcí. 
-   **[Profil uživatele](~/android/user-interface/user-profile.md)**  – poskytuje přístup k datům profilu vlastníka zařízení. 
-   **[Kalendář rozhraní API](~/android/user-interface/controls/calendar.md)**  – poskytuje přístup k kalendářních dat od poskytovatele kalendáře. 

## <a name="x86-emulators"></a>x86 emulátorů

ICS zatím nepodporuje vývoj pomocí x86 emulátor. x86 emulátorů jsou podporovány pouze s Android 2.3.3, rozhraní API úrovně 10. V tématu [konfigurace x86 emulátoru](~/android/get-started/installation/android-emulator/index.md) Další informace.

## <a name="summary"></a>Souhrn

Tento článek zahrnutých celou řadu nové technologie, které jsou nyní k dispozici v systému Android 4. Jsme přečetli nové funkce uživatelského rozhraní, jako *GridLayout*, *PopupMenu*, a *přepínač* pomůcky. Můžeme také nalezený na některé z nové podpory pro řízení systému uživatelského rozhraní, jak dobře pro práci s *TextureView*. Potom jsme probrali celou řadu sdílení nové technologie. Jsme zahrnutých jak *Android světla* umožňuje sdílení informací mezi zařízeními, které používají *NFC*, popsané nové *kalendáře API*a také vám ukázal, jak pomocí předdefinovaných v  *ShareActionProvider*.
Nakonec jsme se zaměřili na tom, jak používat *ContactsContract* zprostředkovatele pro přístup k datům profilu uživatele.



## <a name="related-links"></a>Související odkazy

- [Zmrzlinová Sandwichovy – ukázky](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (sample)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Karta rozložení kurzu](~/android/user-interface/layouts/tab-layout/index.md)
- [Zmrzlinová Sandwichovy](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Platformu Android 4.0](http://developer.android.com/about/versions/android-4.0.html)
