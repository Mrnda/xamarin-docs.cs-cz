---
title: Funkce
description: "Využití výhod funkcí specifických pro platformy s Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/20/2017
ms.openlocfilehash: 950cc4a8611b05c22825ef89a85827fa0d3e5f7b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="platform-features"></a>Funkce

Xamarin.Forms rozšiřitelný a umožňuje vám začlenění specifické pro platformu funkcí s použitím [důsledky](~/xamarin-forms/app-fundamentals/effects/index.md), [vlastní nástroji pro vykreslování](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)a další.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

Tato příručka popisuje, jak k implementaci návrhu materiálu při aktualizaci existující aplikace Xamarin.Forms Android.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[Indexování aplikace a přímé propojení](deep-linking.md)

Indexování aplikace umožňuje aplikacím, které by jinak zapomenete po pár používá zůstane relevantní, ve které jsou uvedeny ve výsledcích hledání. Přímé propojení umožňuje aplikacím reagovat na výsledek hledání, který obsahuje data aplikací, obvykle tak, že přejdete na stránku na něj odkazovat z přímý odkaz.

## <a name="device-classdevicemd"></a>[Zařízení – třída](device.md)

Postup použití `Device` třídy za účelem vytvoření specifické pro platformu chování v sdíleného kódu a uživatelského rozhraní (včetně pomocí XAML). Platí i pro `BeginInvokeOnMainThread` které je nezbytné při úpravě ovládacích prvků uživatelského rozhraní z vlákna na pozadí.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

Některé stylů iOS je možné provádět prostřednictvím **Info.plist** a `UIAppearance` rozhraní API. Tato příručka obsahuje příklady, jak se zahrnuje do aplikace pro iOS řešení Xamarin.Forms, včetně vyhledávání Spotlight základní funkce iOS 9.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms má nyní preview podporu pro aplikace systému macOS.

## <a name="native-formsnative-formsmd"></a>[Nativní formulářů](native-forms.md)

Nativní formuláře umožňují Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, které se spotřebovávají nativní projekty Xamarin.iOS, Xamarin.Android a univerzální platformu Windows (UWP).

## <a name="native-viewsnative-viewsindexmd"></a>[Nativní zobrazení](native-views/index.md)

Nativní zobrazení z iOS, Android a univerzální platformu Windows můžete přímo na něj odkazovat z Xamarin.Forms. Vlastnosti a obslužné rutiny událostí můžete nastavit na nativní zobrazení, a mohou komunikovat s Xamarin.Forms zobrazení.

## <a name="platform-specificsplatform-specificsindexmd"></a>[Platforma – podrobnosti](platform-specifics/index.md)

Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, aniž byste museli vlastní nástroji pro vykreslování nebo účinky.

## <a name="pluginspluginsmd"></a>[Moduly plug-in](plugins.md)

Nejsou k dispozici na Githubu, Nuget a úložišti součástí Xamarin pomáhají prodloužit Xamarin.Forms aplikace širokou škálu open-source moduly plug-in.

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms má podporu pro čtyři různé typy projektu pro Windows:

* Windows Phone 8 Silverlight (původní platformu Windows nepodporuje Xamarin.Forms),
* Windows Phone 8.1 (WinRT)
* Windows 8.1 (WinRT), a
* Univerzální platformu Windows (Windows 10).

Tato část popisuje rozdíly mezi nimi a jak je přidat do existujícího řešení Xamarin.Forms.
