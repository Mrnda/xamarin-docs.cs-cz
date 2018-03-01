---
title: "Android platformy – podrobnosti"
description: "Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak využívat jsou Android platformy – specifikace integrovaných do Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: e26fcb81bb99e5a49d16731777171b4efa4163c6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="android-platform-specifics"></a>Android platformy – podrobnosti

_Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak využívat jsou Android platformy – specifikace integrovaných do Xamarin.Forms._

V systému Android se Xamarin.Forms obsahuje následující platformy specifické:

- Nastavení provozní režim logicky klávesnice. Další informace najdete v tématu [nastavení logicky režimu vstupu klávesnice](#soft_input_mode).
- Povolení rychlého posouvání [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Další informace najdete v tématu [povolení rychlého posouvání v prvku ListView](#fastscroll).
- Povolení mezi stránky v k načtení [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Další informace najdete v tématu [povolení k načtení mezi stránky v TabbedPage](#enable_swipe_paging).
- Řízení pořadí vizuálních prvků k určení pořadí vykreslování. Další informace najdete v tématu [řízení zvýšení vizuální prvky](#elevation).
- Zakázání [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) a [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) stránky události životního cyklu na pozastavení a obnovení, pro aplikace, které používají kompatibility aplikace. Další informace najdete v tématu [zakázání Disappearing a zobrazování událostí životního cyklu stránky](#disable_lifecycle_events).

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

[![](android-images/pan-resize.png "Softwarová klávesnice provozní režim specifické pro platformu")](android-images/pan-resize-large.png "Soft Keyboard Operating Mode Plaform-Specific")

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

[![](android-images/fastscroll.png "ListView FastScroll specifických pro platformy")](android-images/fastscroll-large.png "ListView FastScroll Plaform-Specific")

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

Tato specifické pro platformu se používá k řízení zvýšení oprávnění, nebo pořadí vykreslování, vizuální prvky v aplikacích, že cíl 21 rozhraní API nebo větší. Zvýšení úrovně vizuální prvek určuje jeho pořadí vykreslování, s vizuální prvky s vyššími hodnotami Z occluding vizuální prvky s nižšími hodnotami Z. V jazyce XAML spotřebování nastavením `Elevation.Elevation` připojené vlastnosti `boolean` hodnotu:

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
            <Button Text="Button Above BoxView - Click Me" android:Elevation.Elevation="10"/>
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

`Button.On<Android>` Metoda určuje, že bude tento specifické pro platformu jenom spustit v systému Android. `Elevation.SetElevation` Metoda v [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) obor názvů, je použít k nastavení zvýšení úrovně vizuální prvek s možnou hodnotou Null `float`. Kromě toho `Elevation.GetElevation` metoda slouží k načtení hodnoty zvýšení visual elementu.

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

[![](android-images/keyboard-on-resume.png "Životní cyklus události specifické pro platformu")](android-images/keyboard-on-resume-large.png "životního cyklu události specifické pro platformu")

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak využívat jsou Android platformy – specifikace integrovaných do Xamarin.Forms. Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky.


## <a name="related-links"></a>Související odkazy

- [Vytváření specifika platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
