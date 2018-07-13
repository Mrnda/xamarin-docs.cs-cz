---
title: Funkce platformy Xamarin.Forms
description: Tato příručka vysvětluje, jak využít výhod funkce specifické pro platformu v aplikacích Xamarin.Forms pomocí různých technik.
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2018
ms.openlocfilehash: 9bac53f71178ac321dea162d346295556a8f7adb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998749"
---
# <a name="xamarinforms-platform-features"></a>Funkce platformy Xamarin.Forms

Xamarin.Forms je rozšiřitelný a umožňuje zahrnovat funkcemi konkrétní platformy pomocí [účinky](~/xamarin-forms/app-fundamentals/effects/index.md), [vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)a provádění dalších akcí.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

Tato příručka popisuje, jak implementovat Material Design prostřednictvím aktualizace existujících aplikací Xamarin.Forms s Androidem.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[Indexování aplikací a přímé odkazování](deep-linking.md)

Indexování aplikací umožňuje aplikacím, které by jinak budou vymazány po pár používá udržujte podle zobrazování ve výsledcích hledání. Přímé odkazování umožňuje aplikacím reakce na výsledky hledání, který obsahuje data aplikací, obvykle tak, že přejdete na stránku na něj odkazovat z přímý odkaz.

## <a name="device-classdevicemd"></a>[Třída zařízení](device.md)

Jak používat `Device` třídy za účelem vytvoření chování specifické pro platformu v sdílenému kódu a uživatelského rozhraní (včetně pomocí XAML). Věnuje se také `BeginInvokeOnMainThread` což je důležité při úpravě ovládacích prvků uživatelského rozhraní z vlákna na pozadí.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

Některé stylování iOS je možné provádět prostřednictvím **Info.plist** a `UIAppearance` rozhraní API. Tato příručka obsahuje příklady toho, jak zahrnout do aplikace pro iOS řešení Xamarin.Forms, včetně vyhledávání Spotlight základní funkce iOS 9.

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms teď nabízí podporu verze preview pro GTK # aplikace.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms teď nabízí podporu verze preview pro aplikace pro macOS.

## <a name="native-formsnative-formsmd"></a>[Nativní formuláře](native-forms.md)

Nativní formuláře umožňují Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-odvozené stránky, které využívat nativní projekty Xamarin.iOS, Xamarin.Android a univerzální platformu Windows (UPW).

## <a name="native-viewsnative-viewsindexmd"></a>[Nativní zobrazení](native-views/index.md)

Nativní zobrazení v iOS, Android a univerzální platformu Windows můžete přímo odkazovanými z Xamarin.Forms. Vlastnosti a obslužných rutin událostí můžete nastavit na nativní zobrazení, a může komunikovat s Xamarin.Forms zobrazení.

## <a name="platform-specificsplatform-specificsindexmd"></a>[Specifika platforem](platform-specifics/index.md)

Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, aniž by bylo nutné vlastní renderery nebo účinky.

## <a name="pluginspluginsmd"></a>[Moduly plug-in](plugins.md)

Nejsou k dispozici na Githubu, Nuget a Store komponenty Xamarin pomáhají rozšířit u aplikací Xamarin.Forms širokou škálu open source moduly plug-in.

## <a name="tizentizenmd"></a>[Tizen](tizen.md)

Tizen .NET umožňuje vytvářet aplikace .NET pomocí Xamarin.Forms a Tizen .NET framework.

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms obsahuje podporu pro univerzální platformu Windows (UPW) na Windows 10. Tento článek popisuje, jak přidat projektu UPW do existujícího řešení Xamarin.Forms.

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms teď nabízí podporu verze preview pro aplikace Windows Presentation Foundation (WPF).
