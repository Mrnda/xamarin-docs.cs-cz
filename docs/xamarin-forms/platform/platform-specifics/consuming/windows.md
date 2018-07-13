---
title: Windows specifik platforem
description: Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat Windows specifik platforem, které jsou součástí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 43a681350035c3e965798bd63f49cd39f472ebfd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998411"
---
# <a name="windows-platform-specifics"></a>Windows specifik platforem

_Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky. Tento článek ukazuje, jak využívat Windows specifik platforem, které jsou součástí Xamarin.Forms._

Na Universal Windows Platform (UWP), Xamarin.Forms obsahuje následující specifika platforem:

- Nastavení možnosti umístění panelu nástrojů. Další informace najdete v tématu [Změna umístění panelu nástrojů stránky](#toolbar_placement).
- Sbalení [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) navigační panel. Další informace najdete v tématu [sbalení navigačního panelu MasterDetailPage](#collapsable_navigation_bar).
- Povolení [ `WebView` ](xref:Xamarin.Forms.WebView) chcete zobrazit výstrahy jazyka JavaScript v dialogovém okně zpráva UPW. Další informace najdete v tématu [zobrazení výstrahy JavaScript](#webview-javascript-alert).
- Povolení [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) k interakci s modulem kontroly pravopisu. Další informace najdete v tématu [umožňuje kontrolu pravopisu SearchBar](#searchbar-spellcheck).
- Zjišťování pořadí čtení z textového obsahu v [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), a [ `Label` ](xref:Xamarin.Forms.Label) instancí. Další informace najdete v tématu [zjišťování pořadí čtení z obsahu](#inputview-readingorder).
- Zakázat režim starší verze barvy na podporované [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Další informace najdete v tématu [zakázání barevný režim starší verze](#legacy-color-mode).
- Povolení podpory klepněte na gesto v [ `ListView` ](xref:Xamarin.Forms.ListView). Další informace najdete v tématu [povolení klepněte na gesto podpory v ListView](#listview-selectionmode).
- Povolení ikon stránek zobrazený na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) nástrojů. Další informace najdete v tématu [povolení ikon na TabbedPage](#tabbedpage-icons).
- Přístupový klíč pro nastavení [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Další informace najdete v tématu [přístupových klíčů k nastavení VisualElement](#visualelement-accesskeys).

<a name="toolbar_placement" />

## <a name="changing-the-page-toolbar-placement"></a>Změna umístění panelu nástrojů stránky

Tento konkrétní platformy se používá ke změně umístění panelu nástrojů na [ `Page` ](xref:Xamarin.Forms.Page)a využívat XAML tak, že nastavíte [ `Page.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) přidružená vlastnost na hodnotu [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) výčtu:

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

`Page.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na Windows. [ `Page.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, se používá k nastavení umístění panelu nástrojů s [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) poskytuje výčet tří hodnot: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), a [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Výsledkem je, že je umístění zadané nástrojů platí pro [ `Page` ](xref:Xamarin.Forms.Page) instance:

[![](windows-images/toolbar-placement.png "Panel nástrojů umístění specifické pro platformu")](windows-images/toolbar-placement-large.png#lightbox "nástrojů umístění specifické pro platformu")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Sbalení navigačního panelu MasterDetailPage

Tento konkrétní platformy se používá ke sbalení navigačního panelu na [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)a využívat XAML tak, že nastavíte [ `MasterDetailPage.CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) a [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty)přidružené vlastnosti:

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

`MasterDetailPage.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na Windows. [ `Page.SetCollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, slouží k určení stylu sbalit s [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) výčet poskytuje dvě hodnoty: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) a [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). [ `MasterDetailPage.CollapsedPaneWidth` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},System.Double)) Metoda se používá k určení šířka částečně sbaleným navigačním panelem.

Výsledkem je, že zadané [ `CollapseStyle` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) platí pro [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) instance se také specifikovanou šířkou:

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

<a name="tabbedpage-icons" />

## <a name="enabling-icons-on-a-tabbedpage"></a>Povolení ikon na TabbedPage

Ikony stránek zobrazený na umožňuje toto specifické pro platformu [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) nástrojů a umožňuje volitelně zadat velikost ikony. V XAML je využívá tak, že nastavíte [ `TabbedPage.HeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) připojené vlastnosti `true`a volitelně můžete nastavením [ `TabbedPage.HeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) připojené vlastnosti [ `Size` ](xref:Xamarin.Forms.Size) hodnotu:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" Icon="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" Icon="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" Icon="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
    {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", Icon = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", Icon = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", Icon = "contacts.png" });
  }
}
```

`TabbedPage.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na univerzální platformu Windows. [ `TabbedPage.SetHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, slouží k zapnutí nebo vypnutí záhlaví ikony. [ `TabbedPage.SetHeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size)) Metoda volitelně určuje velikost ikony záhlaví s [ `Size` ](xref:Xamarin.Forms.Size) hodnotu.

Kromě toho `TabbedPage` třídy v `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` oboru názvů má také [ `EnableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) metodu, která umožňuje záhlaví ikony [ `DisableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) metodu, která zakáže záhlaví ikony, a [ `IsHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) metodu, která vrací `boolean` hodnotu, která určuje, jestli jsou povolené hlavičky ikony.

Výsledkem je této stránce lze zobrazit ikony na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) panel nástrojů s velikost ikony je volitelně nastavit požadovaná velikost:

![TabbedPage ikony povolené specifické pro platformu](windows-images/tabbedpage-icons.png "TabbedPage ikony povolené specifické pro platformu")

<a name="visualelement-accesskeys" />

## <a name="setting-visualelement-access-keys"></a>Nastavení VisualElement přístupové klíče

Přístupové klíče jsou klávesové zkratky, které zlepšují použitelnost a přístupnost aplikace na univerzální platformě Windows tím, že poskytuje intuitivní způsob, jak uživatelé mohli rychle procházení a interakce s viditelné Uživatelském rozhraní aplikace pomocí klávesnice místo přes touch nebo myši. Jsou kombinací klávesy Alt a jeden nebo více alfanumerické klíčů, obvykle stisknutí postupně. Klávesové zkratky umožňují automaticky přístupové klíče, které používají jeden alfanumerický znak.

Popisy kláves přístupu jsou plovoucí odznáčků zobrazí vedle ovládacích prvků, které zahrnují přístupové klíče. Každý přístup kláves obsahuje alfanumerické klíče, které aktivovat přidružený ovládací prvek. Pokud uživatel stiskne klávesu Alt, zobrazí se tipy klíčů přístup.

Tento konkrétní platformy se používá k určení přístupového klíče pro [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). V XAML je využívá tak, že nastavíte [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) přidružená vlastnost na hodnotu alfanumerické znaky a volitelně můžete nastavením [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) přidružená vlastnost na hodnotu [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) výčet, [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) připojené vlastnosti `double`a [ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) přidružená vlastnost `double`:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Alternativně může být používán z C# s použitím rozhraní fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

`VisualElement.On<Windows>` Metody Určuje, že toto specifické pro platformu lze spustit pouze na univerzální platformu Windows. [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, se používá k nastavení hodnotě klíče pro přístup `VisualElement`. [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) Metoda, volitelně určuje umístění pro zobrazení popisu klávesy přístup pomocí [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) výčet poskytuje následující možné hodnoty:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – Označuje, že umístění kláves přístupu určí podle operačního systému.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) – Označuje, že přístup kláves budou zobrazovat nad horním okrajem `VisualElement`.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) – Označuje, že přístup kláves zobrazí pod spodní okraj `VisualElement`.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) – Označuje, že přístup kláves zobrazí napravo od pravého okraje `VisualElement`.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) – Označuje, že přístup kláves zobrazí nalevo od levého okraje `VisualElement`.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) – Označuje, že přístup kláves zobrazí překryté na střed `VisualElement`.

> [!NOTE]
> Obvykle [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto) kláves umístění je dostačující, která zahrnuje podporu pro adaptivní uživatelská rozhraní.

[ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) a [ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) metody lze použít pro podrobnější řízení přístup k poloze popisu klávesy. Argument `SetAccessKeyHorizontalOffset` metoda označuje, jak daleko přejdete přístup k popisu klávesy vlevo nebo vpravo a argument `SetAccessKeyVerticalOffset` metody Určuje, jak daleko přesunout přístup kláves nahoru nebo dolů.

>[!NOTE]
> Přístup k popisu klávesy posuny nelze nastavit, je-li nastavit umístění klíčů přístup `Auto`.

Kromě toho [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), a [ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) metody lze použít k načtení přístupového klíče hodnotu a jeho umístění.

Výsledkem je, že popisy kláves přístup je možné zobrazit vedle některého [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) instancí, které určují přístupové klíče, stisknutím klávesy Alt:

![VisualElement přístupových klíčů specifické pro platformu](windows-images/visualelement-accesskeys.png "VisualElement přístupových klíčů pro konkrétní platformu")

Když uživatel aktivuje přístupový klíč, stisknutím klávesy Alt, za nímž následuje přístup klíče, výchozí akce pro `VisualElement` se spustí. Například když uživatel aktivuje přístupového klíče na [ `Switch` ](xref:Xamarin.Forms.Switch), `Switch` přepne. Když uživatel aktivuje na přístupový klíč [ `Entry` ](xref:Xamarin.Forms.Entry), `Entry` získá fokus. Když uživatel aktivuje na přístupový klíč [ `Button` ](xref:Xamarin.Forms.Button), obslužné rutiny události pro [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) provedla událost.

Další informace o přístupových klíčů najdete v tématu [přístupové klíče](/windows/uwp/design/input/access-keys#key-tip-positioning).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak využívat Windows specifik platforem, které jsou součástí Xamarin.Forms. Specifika platforem umožňují používat funkce, která je dostupná jenom na konkrétní platformě, bez implementace vlastní renderery nebo účinky.

## <a name="related-links"></a>Související odkazy

- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
