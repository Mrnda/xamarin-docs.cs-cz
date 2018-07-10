---
title: iOS specifik platforem
description: Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat iOS specifik platforem, které jsou součástí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: be378a60a9d9a7b206b64f07ee70edb432cec8e3
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935651"
---
# <a name="ios-platform-specifics"></a>iOS specifik platforem

_Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat iOS specifik platforem, které jsou součástí Xamarin.Forms._

V systémech iOS Xamarin.Forms obsahuje následující specifika platforem:

- Rozostření podporu pro všechny [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Další informace najdete v tématu [použití rozostření](#blur).
- Řízení, zda se název stránky se zobrazí velké záhlaví na navigačním panelu stránky. Další informace najdete v tématu [zobrazení důležité nadpisy](#large_title).
- Zajištění tuto stránku obsahu je umístěn na oblast na obrazovce, který je bezpečný pro všechna zařízení s Iosem. Další informace najdete v tématu [umožňuje bezpečné vodítko rozložení pro oblast](#safe_area_layout).
- Více průchody průsvitných navigační panel. Další informace najdete v tématu [provádění navigační panel průsvitné](#translucent_navigation_bar).
- Řízení, zda text stavového řádku barva na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) se upraví tak, aby odpovídaly světelnost na navigačním panelu. Další informace najdete v tématu [nastavení režimu barvu textu panelu Stav](#status_bar_color_mode).
- Zajištění, který byl vložen text zapadá do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) úpravou velikosti písma. Další informace najdete v tématu [nastavení velikosti písma položky](#adjust_font_size).
- Řízení při výběru položky probíhá [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Další informace najdete v tématu [řízení výběr položky](#picker_update_mode).
- Nastavení viditelnost panelu stavu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Další informace najdete v tématu [nastavení viditelnost panelu Stav na stránce](#set_status_bar_visibility).
- Řízení, zda [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zpracovává gesta dotykového ovládání a předává je na jeho obsah. Další informace najdete v tématu [zpoždění v obsahu dnešní v ScrollView](#delay_content_touches).
- Nastavení stylu oddělovač na [ `ListView` ](xref:Xamarin.Forms.ListView). Další informace najdete v tématu [nastavení styl oddělovače ListView](#listview-separatorstyle).
- Zakázat režim starší verze barvy na podporované [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Další informace najdete v tématu [zakázání barevný režim starší verze](#legacy-color-mode).

<a name="blur" />

## <a name="applying-blur"></a>Použití rozostření

Tento konkrétní platformy rozostření obsahu vrstvy pod ní se používá a je využívat XAML tak, že nastavíte [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) přidružená vlastnost na hodnotu [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) výčtu:

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

`BoxView.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) obor názvů, se používá k aplikování efekt rozostření s [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) výčet poskytuje čtyři hodnoty: [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight), [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), a [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

Výsledkem je, že zadané [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) platí pro [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) instance, které rozostření [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) vrstvy pod ní:

![](ios-images/blur-effect.png "Rozostření efekt specifické platformy")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Zobrazení důležité nadpisy

Tento konkrétní platformy se používá k zobrazení se název stránky velké záhlaví na navigačním panelu, zařízení, která používají iOS 11 nebo novější. Velké název je zarovnaná doleva a používá větší písmo a přejde do standardního názvu jako uživatel začne posouvání obsahu, tak, aby efektivní využití nemovitosti obrazovky. Ale v orientaci na šířku, název vrátí do středu navigační panel pro optimalizaci rozložení obsahu. V XAML je využívá tak, že nastavíte `NavigationPage.PrefersLargeTitles` připojené vlastnosti `boolean` hodnotu:

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

`NavigationPage.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. `NavigationPage.SetPrefersLargeTitle` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, určuje, zda jsou povoleny důležité nadpisy.

Za předpokladu, že jsou důležité nadpisy zapnuta [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), budou všechny stránky v navigačním zásobníku zobrazit důležité nadpisy. Toto chování můžete přepsat na stránkách s nastavením `Page.LargeTitleDisplay` přidružená vlastnost na hodnotu `LargeTitleDisplayMode` výčtu:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternativně lze přepsat chování stránky z C# s použitím rozhraní fluent API:

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

`Page.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. `Page.SetLargeTitleDisplay` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, řídí chování velkých nadpis na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), s `LargeTitleDisplayMode` výčet poskytuje tři možné hodnoty:

- `Always` – v navigačním panelu a písma velikost velkých formát.
- `Automatic` – stejné styl (velký nebo malý) použít jako předchozí položka v navigačním zásobníku.
- `Never` – Vynutit použití formátu pravidelné a malých navigační panel.

Kromě toho `SetLargeTitleDisplay` metody slouží k přepnutí hodnoty výčtu ve volání `LargeTitleDisplay` metodu, která vrací aktuální `LargeTitleDisplayMode`:

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

Výsledkem je, že zadané `LargeTitleDisplayMode` platí pro [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), která řídí chování velkých název:

![](ios-images/large-title.png "Rozostření efekt specifické platformy")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Povolení vodítko rozložení pro bezpečnou oblast

Tento konkrétní platformy slouží k zajištění, že obsah stránky je umístěn na oblast na obrazovce, který je bezpečný pro všechna zařízení, která používají iOS 11 a vyšší. Konkrétně, pomůže vám to Ujistěte se, že tento obsah není oříznut zařízení zaoblené rohy indikátoru domácí nebo krytu ze senzorů v zařízení iPhone X. V XAML je využívá tak, že nastavíte `Page.UseSafeArea` připojené vlastnosti `boolean` hodnotu:

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

`Page.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. `Page.SetUseSafeArea` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, určuje, zda je povoleno vodítko rozložení pro bezpečnou oblast.

Výsledkem je, že obsah stránky může být umístěné na oblast na obrazovce, který je bezpečný pro všechny Iphony:

[![](ios-images/safe-area-layout.png "Vodítko rozložení pro bezpečnou oblast")](ios-images/safe-area-layout-large.png#lightbox "vodítko rozložení pro bezpečnou oblast")

> [!NOTE]
> Bezpečné oblast, která Společnost Apple v Xamarin.Forms slouží k nastavení [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) vlastnost a přepíše předchozí hodnoty této vlastnosti, které jste nastavili.

Bezpečnou oblast může přizpůsobit načítání jeho [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) hodnotu `Page.SafeAreaInsets` metodu z [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) oboru názvů. Potom ji můžete změnit jako povinné a znovu přiřazen k `Padding` vlastnost v konstruktoru stránky nebo [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) přepsat:

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

## <a name="making-the-navigation-bar-translucent"></a>Provádění více průchody průsvitných navigační panel

Tento konkrétní platformy se používá k změňte průhlednost na navigačním panelu a je využívat XAML tak, že nastavíte [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) připojené vlastnosti `boolean` hodnotu:

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

`NavigationPage.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) oboru názvů je používán k vytváření více průchody průsvitných na navigačním panelu. Kromě toho [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) třídy v `Xamarin.Forms.PlatformConfiguration.iOSSpecific` oboru názvů má také [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metodu, která obnoví do výchozího stavu, navigačním panelu a [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) metodu, která slouží k přepnutí transparentnosti navigační panel voláním [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metody:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Výsledkem je, že průhlednost na navigačním panelu můžete změnit:

![](ios-images/translucent-navigation-bar.png "Více průchody průsvitných navigační panel specifické platformy")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Nastavení stavového řádku textového barvu režimu

Tento ovládací prvky pro konkrétní platformu, zda text stavového řádku barva na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) se upraví tak, aby odpovídaly světelnost na navigačním panelu. V XAML je využívá tak, že nastavíte [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) přidružená vlastnost na hodnotu [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) výčtu:

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

`NavigationPage.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, ovládací prvky, zda text stavového řádku barva na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) se upraví tak, aby odpovídaly Světelnost navigační panel s [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) výčet poskytuje dva možné hodnoty:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) – Označuje, že na stavovém řádku barva textu nesmí upravit.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) – Označuje, že na stavovém řádku textového barvu by měl odpovídat světelnost na navigačním panelu.

Kromě toho [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) metody slouží k získání aktuální hodnoty [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) výčet, který se použije na [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

Výsledkem je, že na stavovém řádku textového barvu na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) je možné upravit tak, aby odpovídaly světelnost na navigačním panelu. V tomto příkladu stavového řádku textového barvu změny jako uživatel přepíná mezi [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) a [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stránky [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Stavový řádek textového barvu režimu specifické pro platformu")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Úprava velikosti písma položky

Toto specifické pro platformu slouží ke škálování velikost písma [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) zajistit, že vyhovuje zadaném textu v ovládacím prvku. V XAML je využívá tak, že nastavíte [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) připojené vlastnosti `boolean` hodnotu:

```xaml
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

`Entry.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, slouží ke škálování velikost písma v zadaném text tak, aby se vešel do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Kromě toho [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) třídy v `Xamarin.Forms.PlatformConfiguration.iOSSpecific` oboru názvů má také [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metodu, která zakáže toto specifické pro platformu, a [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) metodu, která je možné přepnout velikost písma škálování voláním [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metody:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Výsledkem je, že velikost písma [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) škálovat zajistit, že vyhovuje zadaném textu v ovládacím prvku:

![](ios-images/entry-font-size.png "Upravit položku písmo velikost specifické pro platformu")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Řízení výběr položky

Tento specifický pro platformu řídí, kdy dochází k výběru položky v [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), které uživateli umožňují určit, že výběr položek dochází při procházení položek v ovládacím prvku, nebo pouze jednou **provádí** stisknutí tlačítka. V XAML je využívá tak, že nastavíte `Picker.UpdateMode` přidružená vlastnost na hodnotu `UpdateMode` výčtu:

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

`Picker.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. `Picker.SetUpdateMode` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, se používá k řízení, pokud dojde k výběr položek, s `UpdateMode` výčet poskytuje dva možné hodnoty:

- `Immediately` – Výběr položky dojde k jako uživatel prochází položky [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Toto je výchozí chování v Xamarin.Forms.
- `WhenFinished` – Výběr položky pouze nastane, jakmile uživatel stiskne **provádí** tlačítko [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Kromě toho `SetUpdateMode` metody slouží k přepnutí hodnoty výčtu ve volání `UpdateMode` metodu, která vrací aktuální `UpdateMode`:

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

Výsledkem je, že zadané `UpdateMode` platí pro [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), který určuje, vyvolá se při výběru položky:

[![](ios-images/picker-updatemode.png "Výběr UpdateMode specifické pro platformu")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Nastavení stavového řádku viditelnost na stránce.

Tento konkrétní platformy se používá k nastavení, zda se stavový řádek na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), a zahrnuje možnost řídit, jak přejde do stavového řádku, nebo ji opustí `Page`. V XAML je využívá tak, že nastavíte `Page.PrefersStatusBarHidden` přidružená vlastnost na hodnotu `StatusBarHiddenMode` výčtu a volitelně také `Page.PreferredStatusBarUpdateAnimation` přidružená vlastnost na hodnotu `UIStatusBarAnimation` výčet:

```xaml
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

`Page.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. `Page.SetPrefersStatusBarHidden` Metoda v `Xamarin.Forms.PlatformConfiguration.iOSSpecific` obor názvů, se používá k nastavení, zda se stavový řádek na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) zadáním jedné z `StatusBarHiddenMode` hodnot výčtu: `Default`, `True` , nebo `False`. `StatusBarHiddenMode.True` a `StatusBarHiddenMode.False` hodnoty nastavit viditelnost panelu stavu bez ohledu na to orientace zařízení a `StatusBarHiddenMode.Default` hodnotu skryje stavový řádek v prostředí svisle compact.

Výsledkem je, zda se stavový řádek [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) lze nastavit:

![](ios-images/hide-status-bar.png "Stavový řádek viditelnost specifické pro platformu")

> [!NOTE]
> Na [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), zadaný `StatusBarHiddenMode` hodnota výčtu se aktualizuje i stavový řádek na všechny podřízené stránky. Na všechny ostatní [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-odvozené typy, zadaný `StatusBarHiddenMode` hodnota výčtu se pouze aktualizace stavového řádku na aktuální stránce.

`Page.SetPreferredStatusBarUpdateAnimation` Metoda se používá k nastavení jak přejde do stavového řádku, nebo ji opustí [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) zadáním jedné z `UIStatusBarAnimation` hodnot výčtu: `None`, `Fade`, nebo `Slide`. Pokud `Fade` nebo `Slide` není zadána hodnota výčtu, a 0,25 druhé animace spustí, když se zadá na stavovém řádku nebo ji opustí `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>V dnešní zdržení obsahu v ScrollView

Implicitní časovač se aktivuje, když gesta dotykového ovládání v vstoupí v platnost [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) v Iosu a `ScrollView` rozhodne, založené na akci uživatele v rámci rozsahu časovače, zda by měl zpracovat gesta nebo předání na jeho obsah. Ve výchozím nastavení iOS `ScrollView` zpoždění v dnešní obsahu, ale to může způsobit problémy v některých případech se `ScrollView` obsahu není winning gesta, když by měl. Proto tento ovládacích prvků pro konkrétní platformu, jestli `ScrollView` zpracovává gesta dotykového ovládání a předává je na jeho obsah. V XAML je využívá tak, že nastavíte `ScrollView.ShouldDelayContentTouches` připojené vlastnosti `boolean` hodnotu:

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

`ScrollView.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. `ScrollView.SetShouldDelayContentTouches` Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) obor názvů, slouží ke kontrole, jestli [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) zpracovává gesta dotykového ovládání a předává je na jeho obsah. Kromě toho `SetShouldDelayContentTouches` metody slouží k přepnutí zpoždění obsahu v dnešní voláním `ShouldDelayContentTouches` metoda vrátí, zda jsou zpožděné obsahu v dnešní:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Důsledkem toho pak bude [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) můžete zakázat zpoždění příjem obsahu dnešní tak, že v tomto scénáři [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) obdrží gesta místo [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stránku [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Zpoždění ScrollView obsah se dotýká specifické pro platformu")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

<a name="listview-separatorstyle" />

## <a name="setting-the-separator-style-on-a-listview"></a>Nastavení stylu oddělovač na ListView

Tato platforma kontrolních mechanismů specifická pro zda oddělovač mezi buněk v [ `ListView` ](xref:Xamarin.Forms.ListView) využívá celou šířku `ListView`. V XAML je využívá tak, že nastavíte [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) přidružená vlastnost na hodnotu [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) výčtu:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) obor názvů, slouží ke kontrole, zda oddělovač mezi buňky v [ `ListView` ](xref:Xamarin.Forms.ListView) používá úplnou Šířka `ListView`, se [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) výčet poskytuje dva možné hodnoty:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – označuje oddělovač iOS výchozí chování. Toto je výchozí chování v Xamarin.Forms.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – Označuje, že oddělovače budou vycházet z jednomu z okrajů `ListView` na druhý.

Výsledkem je, že zadané [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) hodnota se použije na [ `ListView` ](xref:Xamarin.Forms.ListView), který určuje šířku oddělovač mezi buňkami:

![](ios-images/listview-separatorstyle.png "ListView SeparatorStyle specifické pro platformu")

> [!NOTE]
> Jakmile byla nastavena styl oddělovače `FullWidth`, nejde změnit zpět na `Default` za běhu.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Zakázání barevný režim starší verze

Některé z zobrazení Xamarin.Forms funkce starší verze barevným režimem. V tomto režimu když [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) zobrazení je nastavena na `false`, zobrazení přepíše barvy nastaveného uživatelem výchozí nativní barvy pro zakázaném stavu. Pro zpětnou kompatibilitu, tento režim starší verze barva zůstane výchozí chování pro podporované zobrazení.

Tento konkrétní platformy zakáže tento režim starší verze barvu tak, aby zůstaly barev zobrazení nastavil uživatel i v případě, že zobrazení je zakázáno. V XAML je využívá tak, že nastavíte [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) připojené vlastnosti `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<iOS>` Metody Určuje, že se tento konkrétní platformy spustí pouze v systému iOS. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) obor názvů, slouží ke kontrole, zda režim starší verze barva je zakázán. Kromě toho [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) lze metoda vrátí, zda je zakázána režim starší verze barvy.

Výsledkem je, že lze zakázat režim starší verze barvy tak, aby zůstaly barev zobrazení nastavil uživatel i při zobrazení zakázáno:

![](ios-images/legacy-color-mode-disabled.png "Režim starší verze barva zakázané")

> [!NOTE]
> Při nastavování [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) zobrazení, se starší verze barevným režimem zcela ignoruje. Další informace o vizuálních stavů najdete v tématu [The Xamarin.Forms Visual State Managerem](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak využívat iOS specifik platforem, které jsou součástí Xamarin.Forms. Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky.


## <a name="related-links"></a>Související odkazy

- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
