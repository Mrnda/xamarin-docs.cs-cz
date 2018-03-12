---
title: "Platforma – podrobnosti"
description: "Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: e1598241e64e6ed3a77fec3803e7fa4d94ee7433
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="platform-specifics"></a>Platforma – podrobnosti

_Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky._

Následující funkce specifické pro platformu je součástí Xamarin.Forms:

<table>
  <thead>
    <tr>
      <td><strong>iOS</strong></td>
      <td><strong>Android</strong></td>
      <td><strong>Windows</strong></td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur">VisualElement.BlurEffect</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode">Application.WindowSoftInputModeAdjust</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement">Page.ToolbarPlacement</a></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title">NavigationPage.PrefersLargeTitles</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll">ListView.IsFastScrollEnabled</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar">MasterDetailPage.CollapsedPaneWidth a MasterDetailPage.CollapseStyle</a></td>
    </tr>    
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout">Page.UseSafeArea</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging">TabbedPage.IsSwipePagingEnabled</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar">NavigationPage.IsNavigationBarTranslucent</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation">Elevation.Elevation</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode">NavigationPage.StatusBarTextColorMode</a></td>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events">Application.SendDisappearingEventOnPause, Application.SendAppearingEventOnResume a Application.ShouldPreserveKeyboardOnResume</a></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size">Entry.AdjustsFontSizeToFitWidth</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode">Picker.UpdateMode</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility">Page.PrefersStatusBarHidden and Page.PreferredStatusBarUpdateAnimation</a></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td><a href="~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches">ScrollView.ShouldDelayContentTouches</a></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

Proces pro použití specifické pro platformu prostřednictvím XAML nebo fluent kódu rozhraní API je následující:

1. Přidat `xmlns` deklarace nebo `using` direktivy pro [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) oboru názvů.
1. Přidat `xmlns` deklarace nebo `using` direktivy pro obor názvů, který obsahuje funkce specifické pro platformu:
    1. V systému iOS, to je [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) oboru názvů.
    1. V systému Android, jde [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) oboru názvů. Pro Android kompatibility aplikace, jde [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) oboru názvů.
    1. V do univerzální platformy Windows, je to [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) oboru názvů.
1. Použít specifické platformy z XAML nebo z kódu pomocí `On<T>` rozhraní fluent API. Hodnota `T` může být [ `iOS` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/), [ `Android` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/), nebo [ `Windows` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/) typy z [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) oboru názvů.

> [!NOTE]
> Všimněte si, že pokus o využívat specifické platformy na platformě tam, kde je k dispozici nebude výsledkem chyba. Místo toho kód spustí bez použití příslušnou platformu.

Platforma specifika spotřebováno přes `On<T>` fluent kódu rozhraní API vrátí [ `IPlatformElementConfiguration` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/) objekty. To umožňuje více podrobností platformy má být volána pro stejný objekt pomocí kaskádových metoda.

Další informace o specifika platformy najdete v tématu [využívání platformy specifika](~/xamarin-forms/platform/platform-specifics/consuming/index.md) a [vytváření platformy specifika](~/xamarin-forms/platform/platform-specifics/creating.md).


## <a name="related-links"></a>Související odkazy

- [Využívání specifika platformy](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Vytváření specifika platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)