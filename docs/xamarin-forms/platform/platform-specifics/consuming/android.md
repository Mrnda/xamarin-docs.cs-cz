---
title: Android platformy – podrobnosti
description: Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak využívat jsou Android platformy – specifikace integrovaných do Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 05f1fc6158e9a20892ab4a4b49b33e4eac6bc5e5
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733058"
---
# <a name="android-platform-specifics"></a>Android platformy – podrobnosti

_Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak využívat jsou Android platformy – specifikace integrovaných do Xamarin.Forms._

V systému Android se Xamarin.Forms obsahuje následující platformy specifické:

- Nastavení provozní režim logicky klávesnice. Další informace najdete v tématu [nastavení logicky režimu vstupu klávesnice](#soft_input_mode).
- Povolení rychlého posouvání [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Další informace najdete v tématu [povolení rychlého posouvání v prvku ListView](#fastscroll).
- Povolení mezi stránky v k načtení [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Další informace najdete v tématu [povolení k načtení mezi stránky v TabbedPage](#enable_swipe_paging).
- Řízení pořadí vizuálních prvků k určení pořadí vykreslování. Další informace najdete v tématu [řízení zvýšení vizuální prvky](#elevation).
- Zakázání [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) a [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) stránky události životního cyklu na pozastavení a obnovení, pro aplikace, které používají kompatibility aplikace. Další informace najdete v tématu [zakázání Disappearing a zobrazování událostí životního cyklu stránky](#disable_lifecycle_events).
- Řízení zda [ `WebView` ](xref:Xamarin.Forms.WebView) můžete zobrazit smíšený obsah. Další informace najdete v tématu [povolení smíšený obsah webové zobrazení](#webview-mixed-content).
- Nastavení možností editoru pro softwarová klávesnice pro vstupní metodu [ `Entry` ](xref:Xamarin.Forms.Entry). Další informace najdete v tématu [možnosti nastavení editoru IME položka](#entry-imeoptions).
- Zakázat režim starší verze barva na podporované [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Další informace najdete v tématu [zakázat režim starší verze barva](#legacy-color-mode).
- Pomocí výchozí odsazení a hodnoty stínu Android tlačítek. Další informace najdete v tématu [pomocí Android tlačítka](#button-padding-shadow).

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>Nastavení režimu vstupní softwarová klávesnice

Tato specifické pro platformu slouží k nastavení provozním režimu pro vstupní oblast logicky klávesnice a je v jazyce XAML spotřebovávají nastavení [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) přidružená vlastnost na hodnotu [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) výčet:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) oboru názvů se používá k nastavení provozním režimu logicky klávesnice vstupní oblasti s [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) poskytuje dvě hodnoty výčtu: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) a [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/). `Pan` Hodnota se používá [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) úpravu možnost, která není velikost okna, pokud má právě fokus, vstupního ovládacího prvku. Místo toho obsah okna se posouvá v okně tak, aby aktuální fokus nebyl zakrytý logicky klávesnice. `Resize` Hodnota se používá [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) možnost úpravy, které změní velikost okna, když má právě fokus, aby uvolnil prostor pro softwarová klávesnice, vstupního ovládacího prvku.

Výsledkem je, že softwarová klávesnice vstupní oblast, kterou provozním režimu můžete nastavit, pokud má právě fokus, vstupního ovládacího prvku:

[![](android-images/pan-resize.png "Softwarová klávesnice provozní režim specifické pro platformu")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>Povolení rychlého posouvání v prvku ListView

Tato specifické pro platformu slouží k povolení rychlé procházení dat v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). V jazyce XAML spotřebování nastavením `ListView.IsFastScrollEnabled` připojené vlastnosti `boolean` hodnotu:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. `ListView.SetIsFastScrollEnabled` Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, slouží k povolení rychlé procházení dat v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Kromě toho `SetIsFastScrollEnabled` metoda slouží k přepnutí rychlé posouvání voláním `IsFastScrollEnabled` metoda vrátí, zda je povoleno rychlé posouvání:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Výsledkem je, že rychlé procházení dat v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) lze povolit, které změní velikost úchytu posuvníku:

[![](android-images/fastscroll.png "ListView FastScroll specifických pro platformy")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Povolení mezi stránky v TabbedPage k načtení

Tato specifické pro platformu slouží k povolení k načtení s gesto vodorovné prstem mezi stránky v [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). V jazyce XAML spotřebování nastavením [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) připojené vlastnosti `boolean` hodnotu:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, slouží k povolení mezi stránky v k načtení [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Kromě toho `TabbedPage` třídy v `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` oboru názvů má také [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metoda, která umožňuje tento konkrétní platformu, a [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metoda, která zakáže Tato specifické pro platformu. [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) Přidružená vlastnost, a [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) metody slouží k nastavení počtu stránek, které se uchovávají do nečinného stavu na obou stranách aktuální stránky.

Výsledkem je, že prstem stránkování prostřednictvím stránky zobrazí [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) zapnutá:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>Řízení zvýšení úrovně vizuální prvky

Tato specifické pro platformu se používá k řízení zvýšení oprávnění, nebo pořadí vykreslování, vizuální prvky v aplikacích, že cíl 21 rozhraní API nebo větší. Zvýšení úrovně vizuální prvek určuje jeho pořadí vykreslování, s vizuální prvky s vyššími hodnotami Z occluding vizuální prvky s nižšími hodnotami Z. V jazyce XAML spotřebování nastavením `VisualElement.Elevation` připojené vlastnosti `boolean` hodnotu:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. `VisualElement.SetElevation` Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, je použít k nastavení zvýšení úrovně vizuální prvek s možnou hodnotou Null `float`. Kromě toho `VisualElement.GetElevation` metoda slouží k načtení hodnoty zvýšení visual elementu.

Výsledkem je, že tak, aby vizuální prvky s vyššími hodnotami Z occlude vizuální prvky s nižšími hodnotami Z se dá řídit zvýšení úrovně vizuální prvky. Proto v tomto příkladu druhý [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) vykreslením výše [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) vzhledem k tomu, že má vyšší hodnotu zvýšení oprávnění:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Zakázání zmizení a vypadající události životního cyklu stránky

Tato specifické pro platformu slouží k zakázání [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) a [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) události stránky v aplikaci pozastavení a obnovení, pro aplikace, které používají kompatibility aplikace. Kromě toho zahrnuje možnost řízení zda logicky klávesnice se zobrazí na obnovit, pokud se zobrazí na pozastavení, za předpokladu, že provozní režim logicky klávesnice je nastaven na [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

> [!NOTE]
> Všimněte si, že tyto události jsou povolené ve výchozím nastavení se zachovat stávající chování pro aplikace, které závisí na události. Tyto události zakážete, nebude cyklus události kompatibility aplikace odpovídat cyklus této události pre-kompatibility aplikace.

Tato specifické pro platformu mohou být spotřebovávána v jazyce XAML nastavení [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), a [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) přidružené vlastnosti pro `boolean` hodnoty:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) oboru názvů se používá k povolení nebo zakázání pálení [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) událostí stránky, když aplikace Přejde na pozadí. [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda se používá k povolení nebo zakázání pálení [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) událostí stránky, když aplikace obnoví z na pozadí. [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda se používá řízení zda logicky klávesnice se zobrazí na obnovit, pokud se zobrazí na pozastavení, poskytuje zda provozní režim logicky klávesnice nastaveno [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

Výsledkem je, že [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) a [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) události stránky nebude aktivováno u aplikace pozastavení a obnovení v uvedeném pořadí a že pokud byl logicky klávesnice zobrazí, když aplikace byla pozastavena, také se zobrazí po návratu aplikace:

[![](android-images/keyboard-on-resume.png "Životní cyklus události specifické pro platformu")](android-images/keyboard-on-resume-large.png#lightbox "životního cyklu události specifické pro platformu")

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>Povolení smíšený obsah webové zobrazení

Tento ovládací prvky specifické pro platformu zda [ `WebView` ](xref:Xamarin.Forms.WebView) můžete zobrazit smíšený obsah v aplikacích této cílové rozhraní API 21 nebo vyšší. Smíšený obsah je obsah, který je původně načtený přes připojení HTTPS, ale který načte prostředky (například obrázky, zvuk, video, předlohy se styly, skripty) pomocí připojení HTTP. V jazyce XAML spotřebování nastavením [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) přidružená vlastnost na hodnotu [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) výčtu:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) obor názvů, je slouží ke kontrole, zda lze zobrazit smíšený obsah, se [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) výčet poskytování třemi možnými hodnotami:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – Označuje, že [ `WebView` ](xref:Xamarin.Forms.WebView) vám umožní počátek HTTPS se načíst obsah z počátek HTTP.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – Označuje, že [ `WebView` ](xref:Xamarin.Forms.WebView) neumožní počátek HTTPS se načíst obsah z počátek HTTP.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – Označuje, že [ `WebView` ](xref:Xamarin.Forms.WebView) se pokusí o být kompatibilní s přístupem nejnovější webový prohlížeč zařízení. Nějaký obsah HTTP může být povoleno načteny počátek HTTPS a jiné typy obsahu se zablokuje. Typy obsahu, které jsou blokované nebo povolené může změnit při každém vydání operačního systému.

Výsledkem je, že zadané [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) k, je použita hodnota [ `WebView` ](xref:Xamarin.Forms.WebView), který určuje, zda lze zobrazit smíšený obsah:

[![Webové zobrazení smíšeného obsahu zpracování specifické platformy](android-images/webview-mixedcontent.png "webové zobrazení smíšeného obsahu zpracování specifické platformy")](android-images/webview-mixedcontent-large.png#lightbox "webové zobrazení smíšeného obsahu zpracování specifické platformy")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>Možnosti nastavení editoru IME položka

Tato specifické pro platformu Nastaví vstupní metoda editor IME možnosti pro softwarové klávesnice pro [ `Entry` ](xref:Xamarin.Forms.Entry). To zahrnuje nastavení tlačítko akce uživatele v dolním rohu logicky klávesnici a interakce s `Entry`. V jazyce XAML spotřebování nastavením [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) přidružená vlastnost na hodnotu [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) výčtu:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) oboru názvů se používá k nastavení možnosti vstupní metoda akce pro softwarové klávesnice pro [ `Entry` ](xref:Xamarin.Forms.Entry), pomocí [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) výčtu poskytuje následující hodnoty:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – Označuje, že žádné konkrétní akce klíč je požadován, a že základní ovládacího prvku vytvoří vlastní Pokud můžete. To bude buď `Next` nebo `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – Označuje, že žádný klíč akce bude k dispozici.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – Označuje, že klíč akce se provést operaci "jít" trvá uživatele k cílovému textu je zadat.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Označuje, že klíč akce provede operaci "Vyhledat" trvá uživateli výsledky hledání textu se, že jste zadali.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Označuje, že klíč akce se provést operaci "Odeslat", doručování text k cíli.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Označuje, že klíč akce bude "Další" operaci, trvá uživatele na následující pole, která bude přijímat text.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Označuje, že klíč akce bude "done" operaci zavření logicky klávesnice.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Označuje, že klíč akce bude "předchozí" operace, trvá uživateli na předchozí pole, která bude přijímat text.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – Maska vybrat možnosti Akce.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – označuje kontroly pravopisu bude ani zjistěte od uživatele ani opravy založené na co se uživatel dříve zadal zobrazovat.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – Označuje, že rozhraní by neměl celé obrazovce.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – Označuje, že extrahované textu budou zobrazeny žádné uživatelské rozhraní.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – Označuje, že se zobrazí žádné uživatelské rozhraní pro vlastní akce.

Výsledkem je, že zadané [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) hodnota se použije pro softwarové klávesnice pro [ `Entry` ](xref:Xamarin.Forms.Entry), která nastaví vstupní metoda možností editoru:

[![Položka vstupní metoda editor specifické pro platformu](android-images/entry-imeoptions.png "položka vstupní metoda editor specifické pro platformu")](android-images/entry-imeoptions-large.png#lightbox "položka vstupní metoda editor specifické pro platformu")

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Zakázat režim starší verze barev

Některé z Xamarin.Forms zobrazení funkce režim starší verze barev. V tomto režimu když [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) zobrazení je nastavena na `false`, zobrazení se přepíše barvy nastavený uživatelem s nativní výchozí barvy pro zakázaném stavu. Pro zpětné kompatibility, tento režim starší verze barva zůstane výchozí chování pro podporované zobrazení.

Tato specifické pro platformu zakáže tento režim starší verze barvu, aby barvy nastavený na zobrazení uživatelem zůstanou i v případě, že zobrazení je zakázáno. V jazyce XAML spotřebování nastavením [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) přidružená vlastnost k `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) obor názvů, je slouží ke kontrole, zda režim starší verze barva je zakázán. Kromě toho [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) lze metoda vrátí, zda je zakázána režim starší verze barvy.

Výsledkem je, že lze zakázat režim starší verze barvy, takže barvy nastavený na zobrazení uživatelem zůstanou i když je zakázáno zobrazení:

![](android-images/legacy-color-mode-disabled.png "Zakázat režim starší verze barvy")

> [!NOTE]
> Při nastavování [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) zobrazení, režim starší verze barva úplně ignorovány. Další informace o vizuálních stavů najdete v tématu [nástroje stavu Manager Visual Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="button-padding-shadow" />

## <a name="using-android-buttons"></a>Pomocí Android tlačítka

Tato specifické pro platformu řídí, zda Xamarin.Forms tlačítka používat výchozí odsazení a hodnoty stínu Android tlačítek. V jazyce XAML spotřebování nastavením [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) a [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) přidružené vlastnosti pro `boolean` hodnoty:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) a[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) metody v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) obor názvů, jsou slouží ke kontrole, jestli Xamarin.Forms tlačítka použít výchozí odsazení a hodnoty stínu Android tlačítek. Kromě toho [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) a [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) metody slouží k vrátí, zda tlačítko používá výchozí odsazení hodnotu a výchozí hodnotou stínové, v uvedeném pořadí.

Výsledkem je, že Xamarin.Forms tlačítka můžete použít výchozí odsazení a hodnoty stínu tlačítek Android:

![](android-images/button-padding-and-shadow.png "Zakázat režim starší verze barvy")

Všimněte si, že se na snímku obrazovky nad každou [ `Button` ](xref:Xamarin.Forms.Button) má stejné definice, vyjma toho, že pravém `Button` používá výchozí odsazení a hodnoty stínu Android tlačítek.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak využívat jsou Android platformy – specifikace integrovaných do Xamarin.Forms. Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky.

## <a name="related-links"></a>Související odkazy

- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
