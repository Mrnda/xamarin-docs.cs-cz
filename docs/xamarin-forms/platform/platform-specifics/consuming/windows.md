---
title: Windows specifik platforem
description: Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat Windows specifik platforem, které jsou součástí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 52895564ef327845940d687a58b007fb1502e62b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935113"
---
# <a name="windows-platform-specifics"></a>Windows specifik platforem

_Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat Windows specifik platforem, které jsou součástí Xamarin.Forms._

Na Universal Windows Platform (UWP), Xamarin.Forms obsahuje následující specifika platforem:

- Nastavení možnosti umístění panelu nástrojů. Další informace najdete v tématu [Změna umístění nástrojů](#toolbar_placement).
- Sbalení [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) navigační panel. Další informace najdete v tématu [sbalení navigačního panelu MasterDetailPage](#collapsable_navigation_bar).
- Povolení [ `WebView` ](xref:Xamarin.Forms.WebView) chcete zobrazit výstrahy jazyka JavaScript v dialogovém okně zpráva UPW. Další informace najdete v tématu [zobrazení výstrahy JavaScript](#webview-javascript-alert).
- Povolení [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) k interakci s modulem kontroly pravopisu. Další informace najdete v tématu [umožňuje kontrolu pravopisu SearchBar](#searchbar-spellcheck).
- Zjišťování pořadí čtení z textového obsahu v [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), a [ `Label` ](xref:Xamarin.Forms.Label) instancí. Další informace najdete v tématu [zjišťování pořadí čtení z obsahu](#inputview-readingorder).
- Zakázat režim starší verze barvy na podporované [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Další informace najdete v tématu [zakázání barevný režim starší verze](#legacy-color-mode).
- Povolení podpory klepněte na gesto v [ `ListView` ](xref:Xamarin.Forms.ListView). Další informace najdete v tématu [povolení klepněte na gesto podpory v ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Změna umístění panelu nástrojů

Tento konkrétní platformy se používá ke změně umístění panelu nástrojů na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)a využívat XAML tak, že nastavíte [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) přidružená vlastnost na hodnotu [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) výčtu:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na Windows. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, se používá k nastavení umístění panelu nástrojů s [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) poskytuje výčet tří hodnot: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), a [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Výsledkem je, že je umístění zadané nástrojů platí pro [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) instance:

[![](windows-images/toolbar-placement.png "Panel nástrojů umístění specifické pro platformu")](windows-images/toolbar-placement-large.png#lightbox "nástrojů umístění specifické pro platformu")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Sbalení navigačního panelu MasterDetailPage

Tento konkrétní platformy se používá ke sbalení navigačního panelu na [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)a využívat XAML tak, že nastavíte [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) a [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)přidružené vlastnosti:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na Windows. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) obor názvů, slouží k určení stylu sbalit s [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) výčet poskytuje dvě hodnoty: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) a [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Metoda se používá k určení šířka částečně sbaleným navigačním panelem.

Výsledkem je, že zadané [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) platí pro [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) instance se také specifikovanou šířkou:

[![](windows-images/collapsed-navigation-bar.png "Sbalený navigační panel specifickou platformu")](windows-images/collapsed-navigation-bar-large.png#lightbox "sbalený navigační panel specifické platformy")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Zobrazení výstrah jazyka JavaScript

Povolí toto specifické pro platformu [ `WebView` ](xref:Xamarin.Forms.WebView) chcete zobrazit výstrahy jazyka JavaScript v dialogovém okně zpráva UPW. V XAML je využívá tak, že nastavíte [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) připojené vlastnosti `boolean` hodnotu:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na univerzální platformu Windows. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, se používá k řízení, zda jsou povolena oznámení jazyka JavaScript. Kromě toho `WebView.SetIsJavaScriptAlertEnabled` metodu je možné přepnout JavaScript výstrahy zavoláním [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) metoda vrátí, zda jsou povoleny:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Výsledkem je, že výstrahy JavaScript lze zobrazit v dialogovém okně UPW zpráva:

![Výstraha WebView JavaScript specifické pro platformu](windows-images/webview-javascript-alert.png "výstraha WebView JavaScript specifické pro platformu")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>Povolení kontroly pravopisu SearchBar

Povolí toto specifické pro platformu [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) k interakci s modulem kontroly pravopisu. V XAML je využívá tak, že nastavíte [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) připojené vlastnosti `boolean` hodnotu:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na univerzální platformu Windows. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) zapnutí a vypnutí se změní obor názvů, nástroj pro kontrolu pravopisu. Kromě toho `SearchBar.SetIsSpellCheckEnabled` metodu je možné přepínat nástroj pro kontrolu pravopisu voláním [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) metoda vrátí, zda je povolen nástroj pro kontrolu pravopisu:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Výsledkem je, že text zadaný do [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) může kontrola pravopisu, s nesprávná slova uvedení uživateli:

![Kontrola specifickou platformu pravopisu SearchBar](windows-images/searchbar-spellcheck.png "SearchBar pravopisu kontrola specifické pro platformu")

> [!NOTE]
> `SearchBar` Třídy v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) oboru názvů má také [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) a [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) metody, které slouží k povolení a zákaz Nástroj pro kontrolu pravopisu v `SearchBar`v uvedeném pořadí.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Zjišťování pořadí čtení z obsahu

Tento konkrétní platformy umožňuje čtení pořadí (zleva doprava nebo zprava doleva) obousměrný text v [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), a [ `Label` ](xref:Xamarin.Forms.Label) instance dynamicky zjistit. V XAML je využívá tak, že nastavíte [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (pro `Entry` a `Editor` instance) nebo [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) připojené vlastnosti `boolean` hodnotu:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na univerzální platformu Windows. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, je slouží ke kontrole, jestli se zjistí pořadí čtení z obsahu v [ `InputView` ](xref:Xamarin.Forms.InputView). Kromě toho `InputView.SetDetectReadingOrderFromContent` metodu je možné můžete určit, jestli pořadí čtení zjistí z obsahu volání [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) metoda vrátí aktuální hodnotu:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Důsledkem toho pak bude [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), a [ `Label` ](xref:Xamarin.Forms.Label) instance může mít pořadí čtení svého obsahu dynamicky zjistila:

[![InputView zjišťování pořadí čtení z obsahu pro konkrétní platformu](windows-images/editor-readingorder.png "InputView zjišťování pořadí čtení z obsahu pro konkrétní platformu")](windows-images/editor-readingorder-large.png#lightbox "InputView zjišťování pořadí čtení z obsah specifický pro platformu")

> [!NOTE]
> Na rozdíl od nastavení [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost, logiku pro zobrazení, které zjišťují pořadí čtení z jejich textový obsah nebude mít vliv na zarovnání textu v rámci zobrazení. Místo toho upravuje pořadí, ve kterém jsou rozloženy obousměrné textové bloky.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Zakázání barevný režim starší verze

Některé z zobrazení Xamarin.Forms funkce starší verze barevným režimem. V tomto režimu když [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) zobrazení je nastavena na `false`, zobrazení přepíše barvy nastaveného uživatelem výchozí nativní barvy pro zakázaném stavu. Pro zpětnou kompatibilitu, tento režim starší verze barva zůstane výchozí chování pro podporované zobrazení.

Tento konkrétní platformy zakáže tento režim starší verze barvu tak, aby zůstaly barev zobrazení nastavil uživatel i v případě, že zobrazení je zakázáno. V XAML je využívá tak, že nastavíte [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) připojené vlastnosti `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na Windows. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, slouží ke kontrole, zda režim starší verze barva je zakázán. Kromě toho [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) lze metoda vrátí, zda je zakázána režim starší verze barvy.

Výsledkem je, že lze zakázat režim starší verze barvy tak, aby zůstaly barev zobrazení nastavil uživatel i při zobrazení zakázáno:

![](windows-images/legacy-color-mode-disabled.png "Režim starší verze barva zakázané")

> [!NOTE]
> Při nastavování [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) zobrazení, se starší verze barevným režimem zcela ignoruje. Další informace o vizuálních stavů najdete v tématu [The Xamarin.Forms Visual State Managerem](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Povolení podpory klepněte na gesto v ListView

Na univerzální platformu Windows, ve výchozím nastavení Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) využívá nativní `ItemClick` události a reagovat na interakce, spíše než nativní `Tapped` událostí. To poskytuje funkce usnadnění přístupu tak, aby program Předčítání Windows a klávesnice může zasahovat `ListView`. Ale také vykresluje jakékoli gesta klepněte uvnitř `ListView` nefunkční.

Tento ovládací prvky pro konkrétní platformu, zda položky v [ `ListView` ](xref:Xamarin.Forms.ListView) můžou reagovat na gesta, klepněte na a proto, zda nativní `ListView` aktivuje `ItemClick` nebo `Tapped` událostí. V XAML je využívá tak, že nastavíte [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) přidružená vlastnost na hodnotu [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) výčtu:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na univerzální platformu Windows. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, slouží ke kontrole, jestli položky v [ `ListView` ](xref:Xamarin.Forms.ListView) můžou reagovat na klepněte na gesta, s [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) výčet poskytuje dva možné hodnoty:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – Označuje, že `ListView` se aktivuje nativní `ItemClick` událostí ke zpracování interakce a proto poskytuje funkce pro usnadnění. Proto Předčítání Windows a klávesnice může zasahovat `ListView`. Ale položky `ListView` nemůže reagovat na klepněte na gesta. Toto je výchozí chování pro `ListView` instancí na univerzální platformu Windows.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – Označuje, že `ListView` se aktivuje nativní `Tapped` událostí ke zpracování interakce. Proto se položky v `ListView` můžou reagovat na klepněte na gesta. Však není žádná funkce usnadnění a proto program Předčítání Windows a klávesnice spolu nemůžou komunikovat `ListView`.

> [!NOTE]
> `Accessible` a `Inaccessible` režimy výběru se vzájemně vylučují a je nutné si vybrat mezi dostupný [ `ListView` ](xref:Xamarin.Forms.ListView) nebo `ListView` klepnout na gesta, která může reagovat.

Kromě toho [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) metodu lze použít k vrácení aktuální [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Výsledek je, že zadané [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) platí pro [ `ListView` ](xref:Xamarin.Forms.ListView), které ovládací prvky, zda položky v `ListView` můžou reagovat na klepněte na gesta a proto, zda nativní `ListView` aktivována `ItemClick` nebo `Tapped` událostí.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak využívat Windows specifik platforem, které jsou součástí Xamarin.Forms. Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky.

## <a name="related-links"></a>Související odkazy

- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
