---
title: Android specifik platforem
description: Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat Androidu specifik platforem, které jsou součástí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
ms.openlocfilehash: 4a97bec37c99209fa6de26a08f8bde44753d0f2d
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986197"
---
# <a name="android-platform-specifics"></a>Android specifik platforem

_Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat Androidu specifik platforem, které jsou součástí Xamarin.Forms._

V systému Android se Xamarin.Forms obsahuje následující specifika platforem:

- Nastavuje provozní režim softwarová klávesnice. Další informace najdete v tématu [nastavení obnovitelného režimu vstupu klávesnice](#soft_input_mode).
- Povolení rychlého posouvání v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Další informace najdete v tématu [umožňující rychlé posouvání v ListView](#fastscroll).
- Povolení potažení prstem mezi stránkami [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Další informace najdete v tématu [povolení potažení prstem mezi stránkami v TabbedPage](#enable_swipe_paging).
- Řízení pořadí vykreslování vizuálních prvků pro určení pořadí vykreslování. Další informace najdete v tématu [řízení ke zvýšení úrovně oprávnění vizuální prvky](#elevation).
- Zakazuje [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) a [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) stránce události životního cyklu při pozastavení a obnovení, pro aplikace, které používají AppCompat. Další informace najdete v tématu [zakázání Disappearing a povolí události životního cyklu stránky](#disable_lifecycle_events).
- Řízení, zda [ `WebView` ](xref:Xamarin.Forms.WebView) můžete zobrazit smíšený obsah. Další informace najdete v tématu [povolení smíšený obsah do WebView](#webview-mixed-content).
- Nastavení možností editoru pro softwarová klávesnice pro vstupní metodu [ `Entry` ](xref:Xamarin.Forms.Entry). Další informace najdete v tématu [možnosti editoru IME pro nastavení položky](#entry-imeoptions).
- Zakázat režim starší verze barvy na podporované [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Další informace najdete v tématu [zakázání barevný režim starší verze](#legacy-color-mode).
- Pomocí výchozí odsazení a stínové hodnot Android tlačítek. Další informace najdete v tématu [pomocí Android tlačítka](#button-padding-shadow).
- Nastavení na panelu nástrojů umístění a barvy [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Další informace najdete v tématu [nastavení TabbedPage nástrojů umístění a barvy](#tabbedpage-toolbar).

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>Nastavení režimu vstupní softwarová klávesnice

Tento konkrétní platformy se používá k nastavení provozní režim pro vstupní oblast softwarová klávesnice a je využívat XAML tak, že nastavíte [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) přidružená vlastnost na hodnotu [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) výčet:

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

`Application.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, se používá k nastavení provozní režim softwarová klávesnice vstupní oblastí s [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) poskytuje dvě hodnoty výčtu: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) a [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/). `Pan` Hodnota používá [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) možnost úprav, která není změně velikosti okna, když je ovládací prvek vstupní fokus. Místo toho obsah okna jsou přesunuty tak, aby zvýrazněným není zakrytý softwarová klávesnice. `Resize` Hodnota používá [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) možnost úpravy, což zmenší okno pod při vstupní ovládací prvek má fokus, aby uvolnil prostor pro softwarová klávesnice.

Výsledkem je, že vstup z softwarová klávesnice oblasti, kterou provozní režim můžete nastavit, pokud vstupní ovládací prvek má fokus:

[![](android-images/pan-resize.png "Softwarová klávesnice, provozní režim specifické pro platformu")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>Povolení rychlého posouvání v ListView

Tento konkrétní platformy se používá k povolení rychlého procházení dat v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). V XAML je využívá tak, že nastavíte `ListView.IsFastScrollEnabled` připojené vlastnosti `boolean` hodnotu:

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

`ListView.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. `ListView.SetIsFastScrollEnabled` Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, slouží k povolení rychlého procházení dat v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Kromě toho `SetIsFastScrollEnabled` metodu je možné přepínat rychlé posouvání pomocí volání `IsFastScrollEnabled` metoda vrátí, zda je povoleno rychlé posouvání:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Výsledkem je, že rychlé procházení dat v [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) lze povolit, což změní velikost posouvání thumb:

[![](android-images/fastscroll.png "ListView FastScroll specifické pro platformu")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Povolení potažení prstem mezi stránkami TabbedPage

Tento specifický pro platformu slouží k povolení potažení prstem s gesto vodorovné prstem mezi stránkami [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). V XAML je využívá tak, že nastavíte [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) připojené vlastnosti `boolean` hodnotu:

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

`TabbedPage.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, slouží k povolení potažení prstem mezi stránkami v [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Kromě toho `TabbedPage` třídy v `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` oboru názvů má také [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metodu, která umožňuje toto specifické pro platformu, a [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metodu, která zakáže Tento konkrétní platformu. [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) Přidružená vlastnost, a [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) metody slouží k nastavení počet stránek, které by měla uchovávat ve stavu nečinnosti na obou stranách aktuální stránku.

Výsledkem je stránkování tohoto potáhnutí prstem na stránkách zobrazený [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) zapnutá:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>Řízení ke zvýšení úrovně oprávnění vizuálních prvků

Tento konkrétní platformy se používá k řízení ke zvýšení úrovně oprávnění nebo pořadí vykreslování, vizuálních prvků u aplikací využívajících rozhraní API 21 nebo větší. Zvýšení úrovně vizuální prvek určuje jeho pořadí vykreslování, s vizuální prvky s vyššími hodnotami Z occluding vizuální prvky s nižší hodnoty Z. V XAML je využívá tak, že nastavíte `VisualElement.Elevation` připojené vlastnosti `boolean` hodnotu:

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

`Button.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. `VisualElement.SetElevation` Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, je sloužící k nastavení zvýšení vizuální prvek s povolenou hodnotou Null `float`. Kromě toho `VisualElement.GetElevation` metody slouží k načtení zvýšení hodnoty prvku visual.

Výsledkem je, že ke zvýšení úrovně oprávnění vizuálních prvků, je možné řídit tak, aby vizuální prvky s vyššími hodnotami Z occlude vizuální prvky s nižšími hodnotami Z. Proto se v tomto příkladu druhá [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) vykreslením výše [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) vzhledem k tomu, že má vyšší hodnotu zvýšení oprávnění:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Zakázání zmizení a zobrazení události životního cyklu stránky

Je toto specifické pro platformu používá se k zakázání [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) a [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) události stránky v aplikaci pozastavení a obnovení, pro aplikace, které používají AppCompat. Kromě toho zahrnuje možnost řízení, zda softwarová klávesnice se zobrazí na pokračování, pokud se zobrazí na pozastavení, za předpokladu, že je provozní režim softwarová klávesnice nastavený na [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

> [!NOTE]
> Všimněte si, že tyto události jsou povolené ve výchozím nastavení zachovat stávající chování pro aplikace, které spoléhají na události. Tyto události zakážete, nebude cyklu události AppCompat odpovídat cyklus této události pre-AppCompat.

Tento specifický pro platformu můžou je využívat XAML tak, že nastavíte [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), a [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) připojených vlastností `boolean` hodnoty:

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

`Application.Current.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) obor názvů, slouží k povolení nebo zakázání jeho spuštění [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) událostí stránky, když aplikace Přejde na pozadí. [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda se používá k povolení nebo zakázání jeho spuštění [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) událostí stránky, po obnovení aplikace ze na pozadí. [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda se používá ovládací prvek, zda softwarová klávesnice se zobrazí na pokračování, pokud se zobrazí na pozastavení, k dispozici, že je provozní režim softwarová klávesnice nastavený na [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

Důsledkem toho pak bude [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) a [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) události stránky nesmí být aktivována na pozastavení aplikace a pokračovat v uvedeném pořadí a, že pokud softwarová klávesnice se zobrazí, když aplikace byla pozastavena, také se zobrazí po obnovení aplikace:

[![](android-images/keyboard-on-resume.png "Životní cyklus události specifické pro platformu")](android-images/keyboard-on-resume-large.png#lightbox "životního cyklu události specifické pro platformu")

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>Povolení smíšený obsah do WebView

Tento ovládací prvky pro konkrétní platformu, jestli [ `WebView` ](xref:Xamarin.Forms.WebView) můžete zobrazit smíšený obsah v aplikacích, které cílí na rozhraní API 21 nebo vyšší. Smíšený obsah je obsahem, který je původně načten pomocí připojení HTTPS, ale které načte prostředky (jako jsou obrázky, zvuk, video, šablony stylů, skripty) prostřednictvím připojení HTTP. V XAML je využívá tak, že nastavíte [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) přidružená vlastnost na hodnotu [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) výčtu:

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

`WebView.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) obor názvů, slouží ke kontrole, zda lze zobrazit smíšený obsah, s [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) výčet poskytuje tři možné hodnoty:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – Označuje, že [ `WebView` ](xref:Xamarin.Forms.WebView) vám umožní původ HTTPS k načtení obsahu z původ HTTP.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – Označuje, že [ `WebView` ](xref:Xamarin.Forms.WebView) neumožní původ HTTPS k načtení obsahu z původ HTTP.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – Označuje, že [ `WebView` ](xref:Xamarin.Forms.WebView) pokusí být kompatibilní s přístupem nejnovější webovému prohlížeči v zařízení. Část obsahu HTTP může být povoleno načteny původ HTTPS a jiné typy obsahu se zablokuje. S každou vydanou verzí operačního systému se může změnit typy obsahu, které jsou blokované nebo povolené.

Výsledkem je, že zadané [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) hodnota se použije na [ `WebView` ](xref:Xamarin.Forms.WebView), který určuje, zda lze zobrazit smíšený obsah:

[![WebView smíšeného obsahu zpracování specifické platformy](android-images/webview-mixedcontent.png "WebView smíšeného obsahu zpracování specifické platformy")](android-images/webview-mixedcontent-large.png#lightbox "WebView smíšeného obsahu zpracování specifické platformy")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>Možnosti editoru IME pro nastavení položky

Tento specifický pro platformu Nastaví vstupní metodu možnosti editoru IME pro softwarová klávesnice pro [ `Entry` ](xref:Xamarin.Forms.Entry). Jedná se o nastavení akce tlačítka pro uživatele v dolním rohu softwarová klávesnice a interakce s `Entry`. V XAML je využívá tak, že nastavíte [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) přidružená vlastnost na hodnotu [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) výčtu:

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

`Entry.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) obor názvů, se používá k nastavení možnost vstupní metody akce pro softwarová klávesnice pro [ `Entry` ](xref:Xamarin.Forms.Entry), s [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) výčet poskytuje následující hodnoty:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – Označuje, že žádná konkrétní akce klíč je požadován a, že základní ovládací prvek vytvoří sama, pokud to možné. To bude buď `Next` nebo `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – Označuje, že žádný klíč akce bude k dispozici.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – naznačuje, že klíč akce provede operaci "go", přičemž uživatele k cílovému textu je zadat.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Označuje, že klíč akci provádí operace "Vyhledat", přičemž uživateli výsledky hledání pro text, zadali.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Označuje, že klíč akce provede operaci "Odeslat", doručování text k cíli.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Označuje, že klíč akce provede operaci "následující", s ohledem uživatele na další pole, která bude přijímat textové.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Označuje, že klíč akce provede operaci "Hotovo", zavření softwarová klávesnice.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Označuje, že klíč akcí provede operaci "předchozí" přesměrujeme uživatel, který bude přijímat textové pole předchozí.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – maska a možnosti Akce.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – Označuje, že kontrolu pravopisu bude Učte se od uživatele, ani navrhnout oprav založené na hodnotě má dříve zadané uživatelem.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – Označuje, že uživatelské rozhraní by neměla přejít na celou obrazovku –.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – Označuje, že pro extrahovaný text se nezobrazí žádné uživatelské rozhraní.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – Označuje, že se nezobrazí žádné uživatelské rozhraní pro vlastní akce.

Výsledkem je, že zadané [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) hodnota se použije pro softwarová klávesnice pro [ `Entry` ](xref:Xamarin.Forms.Entry), který nastaví vstupní metodu možnosti editoru:

[![Položka vstupní metoda editor specifické pro platformu](android-images/entry-imeoptions.png "položku vstupní metoda editor specifické pro platformu")](android-images/entry-imeoptions-large.png#lightbox "položku vstupní metoda editor specifické pro platformu")

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Zakázání barevný režim starší verze

Některé z zobrazení Xamarin.Forms funkce starší verze barevným režimem. V tomto režimu když [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) zobrazení je nastavena na `false`, zobrazení přepíše barvy nastaveného uživatelem výchozí nativní barvy pro zakázaném stavu. Pro zpětnou kompatibilitu, tento režim starší verze barva zůstane výchozí chování pro podporované zobrazení.

Tento konkrétní platformy zakáže tento režim starší verze barvu tak, aby zůstaly barev zobrazení nastavil uživatel i v případě, že zobrazení je zakázáno. V XAML je využívá tak, že nastavíte [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) připojené vlastnosti `false`:

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

`VisualElement.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) obor názvů, slouží ke kontrole, zda režim starší verze barva je zakázán. Kromě toho [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) lze metoda vrátí, zda je zakázána režim starší verze barvy.

Výsledkem je, že lze zakázat režim starší verze barvy tak, aby zůstaly barev zobrazení nastavil uživatel i při zobrazení zakázáno:

![](android-images/legacy-color-mode-disabled.png "Režim starší verze barva zakázané")

> [!NOTE]
> Při nastavování [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) zobrazení, se starší verze barevným režimem zcela ignoruje. Další informace o vizuálních stavů najdete v tématu [The Xamarin.Forms Visual State Managerem](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="button-padding-shadow" />

## <a name="using-android-buttons"></a>Pomocí tlačítka s Androidem

Tento specifický pro platformu Určuje, zda Xamarin.Forms tlačítka použijte výchozí odsazení a hodnoty stínové Android tlačítek. V XAML je využívá tak, že nastavíte [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) a [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) připojených vlastností do `boolean` hodnoty:

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

`Button.On<Android>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému Android. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) a[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) metody v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) obor názvů, jsou slouží ke kontrole, jestli Xamarin.Forms tlačítka použít výchozí odsazení a hodnoty stínové Android tlačítek. Kromě toho [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) a [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) metody slouží k vrátí, zda tlačítko používá výchozí odsazení obsahu hodnotu a výchozí stínové hodnotu.

Výsledkem je, že Xamarin.Forms tlačítka můžete použít výchozí odsazení a stínové hodnoty Android tlačítka:

![](android-images/button-padding-and-shadow.png "Režim starší verze barva zakázané")

Všimněte si, že na snímku obrazovky výše každý [ `Button` ](xref:Xamarin.Forms.Button) má stejné definice, s výjimkou, že pravém `Button` používá výchozí odsazení a hodnoty stínové Android tlačítek.

<a name="tabbedpage-toolbar" />

## <a name="setting-tabbedpage-toolbar-placement-and-color"></a>Nastavení nástrojů TabbedPage umístění a barvy

Tyto specifik platforem slouží k nastavení umístění a barvy panelu nástrojů na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Kdy se využijí v XAML tak, že nastavíte [ `TabbedPage.ToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.toolbarplacementproperty?view=xamarin-forms) přidružená vlastnost na hodnotu [ `ToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement?view=xamarin-forms) výčtu a [ `TabbedPage.BarItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.baritemcolorproperty?view=xamarin-forms) a [ `TabbedPage.BarSelectedItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.barselecteditemcolorproperty?view=xamarin-forms) připojené vlastnosti, které chcete [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Můžete také mohou být využívány z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>` Metody Určuje, že tyto specifik platforem se spustí pouze v systému Android. [ `TabbedPage.SetToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.settoolbarplacement?view=xamarin-forms) Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, se používá k nastavení umístění panelu nástrojů na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), s [ `ToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement?view=xamarin-forms) výčet poskytuje následující hodnoty:

- [`Default`](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_ToolbarPlacement_Default) – Označuje, že panelu nástrojů je umístěn ve výchozím umístění na stránce. Je horní části stránky na telefonech a v dolní části stránky na jiné idiomy zařízení.
- [`Top`](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_ToolbarPlacement_Top) – Označuje, že panelu nástrojů je umístěn v horní části stránky.
- [`Bottom`](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_ToolbarPlacement_Bottom) – Označuje, že panelu nástrojů je umístěn v dolní části stránky.

Kromě toho [ `TabbedPage.SetBarItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.setbaritemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_SetBarItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__Xamarin_Forms_Color_) a [ `TabbedPage.SetBarSelectedItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.setbarselecteditemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_SetBarSelectedItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__Xamarin_Forms_Color_) metody se používají k nastavení barvy položky panelu nástrojů a nástrojů vybrané položky v uvedeném pořadí.

> [!NOTE]
> [ `GetToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.gettoolbarplacement?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_GetToolbarPlacement_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__), [ `GetBarItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.getbaritemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_GetBarItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__), A [ `GetBarSelectedItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.getbarselecteditemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_GetBarSelectedItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__) metody slouží k načtení umístění a barvy [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) nástrojů.

Výsledkem je, že na panelu nástrojů umístění, barva položky panelu nástrojů a barvu položky vybrané nástrojů lze nastavit na [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

![](android-images/tabbedpage-toolbar-placement.png)

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak využívat Androidu specifik platforem, které jsou součástí Xamarin.Forms. Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky.

## <a name="related-links"></a>Související odkazy

- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
