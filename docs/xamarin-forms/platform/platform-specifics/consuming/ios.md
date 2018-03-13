---
title: iOS specifika platformy
description: "Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak používat iOS platformy – podrobnosti, které jsou součástí Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: 798bb2b15534a620acbe76080e171af1a548ac25
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="ios-platform-specifics"></a>iOS specifika platformy

_Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak používat iOS platformy – podrobnosti, které jsou součástí Xamarin.Forms._

V systému iOS obsahuje Xamarin.Forms jsou specifikace následující platformy:

- Rozostření podporu pro žádné [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Další informace najdete v tématu [použití rozostření](#blur).
- Řízení, zda se název stránky se zobrazí velký záhlaví stránky navigačním panelu. Další informace najdete v tématu [zobrazení velké názvy](#large_title).
- Zajistit, že stránka obsahu je nastavený na oblast obrazovky, který je bezpečný pro všechna zařízení s iOS. Další informace najdete v tématu [povolení průvodci bezpečné rozložení oblasti](#safe_area_layout).
- Průhledné navigačním panelu. Další informace najdete v tématu [provedení navigační panel průhledná](#translucent_navigation_bar).
- Řízení zda barvu, která text stavového řádku na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) se upraví tak, aby odpovídaly světelnost navigačním panelu. Další informace najdete v tématu [úpravě režim barvu textu panelu Stav](#status_bar_color_mode).
- Zajištění, která byla zadána jako text zapadá do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) úpravou velikosti písma. Další informace najdete v tématu [úpravě velikost písma položku](#adjust_font_size).
- Řízení, kdy dochází k výběr položek v [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Další informace najdete v tématu [řízení výběr položek výběr](#picker_update_mode).
- Nastavení stavu viditelnost panelu na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Další informace najdete v tématu [nastavení viditelnost panelu stavu na stránce](#set_status_bar_visibility).
- Řízení zda [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zpracuje gesto touch nebo předává je na jeho obsah. Další informace najdete v tématu [zpozdit obsahu úpravy v ScrollView](#delay_content_touches).

<a name="blur" />

## <a name="applying-blur"></a>Použití rozostření

Tato specifické pro platformu slouží rozostření obsahu objekty pod ním a je v jazyce XAML spotřebovávají nastavení [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) přidružená vlastnost na hodnotu [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) výčtu:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
    <Image Source="monkeyface.png" />
    <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) oboru názvů se používá k aplikování efekt rozostření s [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) poskytuje čtyři – výčet hodnoty: [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/), [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/), [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/), a [ `Dark` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/).

Výsledkem je, že zadané [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) se použije na [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) instance, které rozostření [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) objekty pod ním:

![](ios-images/blur-effect.png "Rozostření vliv specifické platformy")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Zobrazení velké názvy

Tento specifické pro platformu se používá k zobrazení nadpisu stránky velké záhlaví na navigačním panelu, pro zařízení, která používají iOS 11 nebo větší. Velké název je zarovnán vlevo a používá větší písma a přejde na standardní název jako uživatel začne posouvání obsahu, tak, aby se efektivně použít nemovitosti obrazovky. Ale v orientaci na šířku nadpis vrátí k centru navigačního panelu za účelem optimalizace rozložení obsahu. V jazyce XAML spotřebování nastavením `NavigationPage.PrefersLargeTitles` připojené vlastnosti `boolean` hodnotu:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Případně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. `NavigationPage.SetPrefersLargeTitle` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, řídí, jestli jsou povolené názvy velké.

Za předpokladu, že velké názvy jsou povolené na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), všechny stránky v zásobníku navigační zobrazí velký názvy. Toto chování je možné přepsat na stránkách nastavení `Page.LargeTitleDisplay` přidružená vlastnost na hodnotu `LargeTitleDisplayMode` výčtu:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternativně může být přepsáno chování stránky z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. `Page.SetLargeTitleDisplay` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, řídí chování velkých title na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), s `LargeTitleDisplayMode` poskytuje tři možné – výčet hodnoty:

- `Always` – Vynutit navigačním panelu a písma velikost velké formát je možné použít.
- `Automatic` – použít stejný styl (velké nebo malé) jako předchozí položce v zásobníku navigace.
- `Never` – Vynutí použití formátu regulární, malé navigačním panelu.

Kromě toho `SetLargeTitleDisplay` metoda slouží k přepnutí hodnoty výčtu ve volání `LargeTitleDisplay` metoda, která vrátí aktuální `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Výsledkem je, že zadané `LargeTitleDisplayMode` se použije na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), které řídí chování velkých název:

![](ios-images/large-title.png "Rozostření vliv specifické platformy")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Povolení v Průvodci oblast pro rozložení

Tato specifické pro platformu se používá k Ujistěte se, zda je umístěn obsah stránky na oblast obrazovky, který je bezpečný pro všechna zařízení, která používají iOS 11 a větší. Konkrétně vám pomůže zajistit, že obsah není oříznut rozích zaokrouhlené zařízení, domácí indikátoru nebo krytu senzor v zařízení iPhone X. V jazyce XAML spotřebování nastavením `Page.UseSafeArea` připojené vlastnosti `boolean` hodnotu:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. `Page.SetUseSafeArea` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, řídí, zda je povoleno průvodci bezpečné oblasti rozložení.

Výsledkem je, že obsah stránky je možné umístit na oblast obrazovky, který je bezpečný pro všechny Iphony:

[![](ios-images/safe-area-layout.png "Bezpečné oblasti rozložení průvodce")](ios-images/safe-area-layout-large.png#lightbox "Průvodce oblast pro rozložení")

> [!NOTE]
> K nastavení je použit v Xamarin.Forms bezpečné oblasti určené Apple [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) vlastnost a potlačí všechny předchozí hodnoty této vlastnosti, které byly nastaveny.

Bezpečné oblasti lze přizpůsobit načtením jeho [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) hodnotu s `Page.SafeAreaInsets` metoda z [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) oboru názvů. Potom ji můžete upravit jako požadované a znovu přiřazen `Padding` vlastnost v konstruktoru stránky nebo [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) přepsání:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>Provedení průhledná navigačního panelu

Tento specifické pro platformu se používá ke změně průhlednost navigačním panelu a je v jazyce XAML spotřebovávají nastavení [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) připojené vlastnosti `boolean` hodnotu:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) oboru názvů se používá k zajištění průhledná navigačním panelu. Kromě toho [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) třídy v `Xamarin.Forms.PlatformConfiguration.iOSSpecific` oboru názvů má také [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metoda, která obnoví do výchozího stavu navigačním panelu a [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) metodu, která slouží k přepnutí navigační panel Průhlednost voláním [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metoda:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Výsledkem je, že lze změnit průhlednost navigačním panelu:

![](ios-images/translucent-navigation-bar.png "Průhledné navigační panel specifické platformy")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Úprava na stavovém řádku režimu barvu textu

Tento ovládací prvky specifické pro platformu zda barvu, která text stavového řádku na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) se upraví tak, aby odpovídaly světelnost navigačním panelu. V jazyce XAML spotřebování nastavením [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) přidružená vlastnost na hodnotu [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) výčtu:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, ovládací prvky zda barvu, která text stavového řádku na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) se upraví tak, aby odpovídala světlost navigačním panelu s [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) výčtu poskytuje dvě možné hodnoty:

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) – Označuje, by neměl být upravena na stavovém řádku barvy.
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) – Označuje, že se na stavovém řádku barvy shodovat s světelnost navigačním panelu.

Kromě toho [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) metoda slouží k načtení aktuální hodnota [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) výčet, který se použije pro [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

Výsledkem je, že se na stavovém řádku barvy textu na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) lze upravit tak, aby odpovídaly světelnost navigačním panelu. V tomto příkladu se na stavovém řádku text se změní barvu jako uživatel přepíná mezi [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) a [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stránkách [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Stavový řádek textu barvu režimu specifické pro platformu")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Úprava velikosti písma položky

Tato specifické pro platformu slouží ke změně měřítka velikost písma [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) zajistit, že vyhovuje zadaném textu v ovládacím prvku. V jazyce XAML spotřebování nastavením [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) připojené vlastnosti `boolean` hodnotu:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) škálování velikost písma zajistit, že vyhovuje zadaném textu se používá obor názvů, [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Kromě toho [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) třídy v `Xamarin.Forms.PlatformConfiguration.iOSSpecific` oboru názvů má také [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metoda, která zakáže tento konkrétní platformu, a [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) metodu, která slouží k přepnutí škálování voláním velikost písma [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metoda:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Výsledkem je, že velikost písma [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) je škálovat zajistit, že vyhovuje zadaném textu v ovládacím prvku:

![](ios-images/entry-font-size.png "Upravit položku písma velikost specifické pro platformu")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Řízení výběr položek výběr.

Tento specifické pro platformu řídí, kdy dochází k výběr položek v [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), které uživateli umožňují určit, že výběr položek k procházení položky v ovládacím prvku, nebo pouze po **provádí** stisknutí tlačítka. V jazyce XAML spotřebování nastavením `Picker.UpdateMode` přidružená vlastnost na hodnotu `UpdateMode` výčtu:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. `Picker.SetUpdateMode` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) oboru názvů se používá k řízení, pokud dojde k výběru položek, s `UpdateMode` výčtu poskytuje dvě možné hodnoty:

- `Immediately` – Výběr položek nastane jako uživatel prohlíží položky v [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Toto je výchozí chování v Xamarin.Forms.
- `WhenFinished` – Výběr položek dochází pouze po uživatel má stisknutí **provádí** v tlačítko [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Kromě toho `SetUpdateMode` metoda slouží k přepnutí hodnoty výčtu ve volání `UpdateMode` metoda, která vrátí aktuální `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Výsledkem je, že zadané `UpdateMode` se použije na [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), který určuje, kdy dojde k výběr položek:

[![](ios-images/picker-updatemode.png "Výběr UpdateMode specifické pro platformu")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Nastavení stavového řádku viditelnost na stránce.

Tato specifické pro platformu se používá k nastavit viditelnost panelu Stav na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), a zahrnuje možnost řídit způsob, jakým stavový řádek zadá nebo ponechá `Page`. V jazyce XAML spotřebování nastavením `Page.PrefersStatusBarHidden` přidružená vlastnost na hodnotu `StatusBarHiddenMode` výčtu a volitelně `Page.PreferredStatusBarUpdateAnimation` přidružená vlastnost na hodnotu `UIStatusBarAnimation` – výčet:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. `Page.SetPrefersStatusBarHidden` Metoda v `Xamarin.Forms.PlatformConfiguration.iOSSpecific` oboru názvů se používá k nastavit viditelnost panelu Stav na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) zadáním jedné z `StatusBarHiddenMode` hodnot výčtu: `Default`, `True` , nebo `False`. `StatusBarHiddenMode.True` a `StatusBarHiddenMode.False` viditelnost panelu stavu bez ohledu na to orientace zařízení, nastavit hodnoty a `StatusBarHiddenMode.Default` hodnota skryje stavový řádek v prostředí svisle compact.

Výsledkem je, že viditelnost stavového řádku [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) lze nastavit:

![](ios-images/hide-status-bar.png "Stavový řádek viditelnost specifické pro platformu")

> [!NOTE]
> Na [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), zadaný `StatusBarHiddenMode` hodnota výčtu, bude také aktualizovat stavového řádku všechny podřízené stránky. V jiných [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-odvozených typů, zadaný `StatusBarHiddenMode` hodnota výčtu bude aktualizovat pouze stavový řádek na aktuální stránce.

`Page.SetPreferredStatusBarUpdateAnimation` Metoda se používá k nastavení jak stavový řádek zadá nebo ponechá [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) zadáním jedné z `UIStatusBarAnimation` hodnot výčtu: `None`, `Fade`, nebo `Slide`. Pokud `Fade` nebo `Slide` zadat hodnotu výčtu, a 0,25 sekundu animace provede jako stavový řádek zadá nebo ponechá `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Zdržení obsahu úpravy v ScrollView

Implicitní časovač se aktivuje, když gesto touch začíná v [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) na iOS a `ScrollView` rozhodne, podle akce uživatele v rámci časovače span, zda by měl zpracovat gesta nebo předejte ji do jeho obsah. Ve výchozím nastavení iOS `ScrollView` zpoždění úpravy obsahu, ale to může způsobit problémy v některých případech se `ScrollView` obsah není vyhrál gesta, pokud by měl. Proto tento ovládací prvky specifické pro platformu zda `ScrollView` zpracuje gesto touch nebo předává je na jeho obsah. V jazyce XAML spotřebování nastavením `ScrollView.ShouldDelayContentTouches` připojené vlastnosti `boolean` hodnotu:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v iOS. `ScrollView.SetShouldDelayContentTouches` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, je slouží ke kontrole, jestli [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zpracuje gesto touch nebo předává je na jeho obsah. Kromě toho `SetShouldDelayContentTouches` metoda slouží k přepnutí, čímž dochází ke zpoždění obsahu úpravy voláním `ShouldDelayContentTouches` metoda vrátí, zda jsou odložené obsahu úpravy:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Výsledkem je, že [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) můžete zakázat zpozdit přijetí úpravy obsahu, takže v tomto scénáři [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) obdrží gesta místo [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stránky [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Obsah ScrollView zpoždění dotykem specifické pro platformu")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat iOS platformy – podrobnosti, které jsou součástí Xamarin.Forms. Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky.


## <a name="related-links"></a>Související odkazy

- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
