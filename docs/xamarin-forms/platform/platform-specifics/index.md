---
title: Specifika platforem
description: Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 10adb46493a1cdbb6bc6a2fd67b5191633d7eeeb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997296"
---
# <a name="platform-specifics"></a>Specifika platforem

_Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky._

Následující funkce specifické pro platformu je integrovaná do Xamarin.Forms:

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[MasterDetailPage.CollapsedPaneWidth a MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|[SearchBar.IsSpellCheckEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#searchbar-spellcheck)
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[Application.SendDisappearingEventOnPause Application.SendAppearingEventOnResume a Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|[InputView.DetectReadingOrderFromContent Label.DetectReadingOrderFromContent](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#inputview-readingorder)
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#legacy-color-mode)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)|[ListView.SelectionMode](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#listview-selectionmode)|
|[Page.PrefersStatusBarHidden a Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#legacy-color-mode)|[TabbedPage.HeaderIconsEnabled a TabbedPage.HeaderIconsSize](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#tabbedpage-icons)|
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|[Button.UseDefaultPadding a Button.UseDefaultShadow](~/xamarin-forms/platform/platform-specifics/consuming/android.md#button-padding-shadow)|[VisualElement.AccessKey, VisualElement.AccessKeyPlacement, VisualElement.AccessKeyHorizontalOffset a VisualElement.AccessKeyVerticalOffset](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|[TabbedPage.ToolbarPlacement TabbedPage.BarItemColor a TabbedPage.BarSelectedItemColor](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)|
|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#legacy-color-mode)|
|[VisualElement.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#drop-shadow)|
|[Application.PanGestureRecognizerShouldRecognizeSimultaneously](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#simultaneous-pan-gesture)|

Proces pro využívání specifické pro platformu prostřednictvím XAML nebo prostřednictvím rozhraní API fluent kódu vypadá takto:

1. Přidat `xmlns` prohlášení nebo `using` směrnice pro [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) oboru názvů.
1. Přidat `xmlns` prohlášení nebo `using` direktivu pro obor názvů, který obsahuje funkce specifické pro platformu:
    1. V systémech iOS, jedná [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) oboru názvů.
    1. V Androidu, jedná [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) oboru názvů. Pro Android AppCompat. to je [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) oboru názvů.
    1. Na univerzální platformu Windows, je to [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) oboru názvů.
1. Použít specifickou platformu z XAML nebo z kódu pomocí `On<T>` rozhraní fluent API. Hodnota `T` může být [ `iOS` ](xref:Xamarin.Forms.PlatformConfiguration.iOS), [ `Android` ](xref:Xamarin.Forms.PlatformConfiguration.Android), nebo [ `Windows` ](xref:Xamarin.Forms.PlatformConfiguration.Windows) typy [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) oboru názvů.

> [!NOTE]
> Všimněte si, že pokusu o zpracování konkrétní platformy na platformě, kde je k dispozici nesmí dojít k chybě. Místo toho kód se spustí bez použití konkrétní platformu.

Specifika platforem spotřebovaná `On<T>` fluent kódu rozhraní API vrátit [ `IPlatformElementConfiguration` ](xref:Xamarin.Forms.IPlatformElementConfiguration`2) objekty. To umožňuje více specifik platforem má být volána na stejný objekt s metoda CSS.

Další informace o specifik platforem, naleznete v tématu [používání specifik platforem](~/xamarin-forms/platform/platform-specifics/consuming/index.md) a [vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md).


## <a name="related-links"></a>Související odkazy

- [Používání specifik platforem](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](xref:Xamarin.Forms.PlatformConfiguration)
