---
title: Platforma Windows – podrobnosti
description: Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak používat Windows platform podrobností, které jsou součástí Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 7299de658a3491928e9bbeaa4dd192a8e95c435e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732798"
---
# <a name="windows-platform-specifics"></a>Platforma Windows – podrobnosti

_Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky. Tento článek ukazuje, jak používat Windows platform podrobností, které jsou součástí Xamarin.Forms._

Na univerzální platformu Windows (UWP), Xamarin.Forms obsahuje následující platformy specifické:

- Nastavení možností umístění panelu nástrojů. Další informace najdete v tématu [Změna umístění panelu nástrojů](#toolbar_placement).
- Sbalení [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) navigačním panelu. Další informace najdete v tématu [sbalení navigační panel MasterDetailPage](#collapsable_navigation_bar).
- Povolení [ `WebView` ](xref:Xamarin.Forms.WebView) zobrazení výstrah JavaScript v dialogu UWP zprávy. Další informace najdete v tématu [zobrazení výstrahy JavaScript](#webview-javascript-alert).
- Povolení [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) k interakci s modulem kontroly pravopisu. Další informace najdete v tématu [povolení kontrola pravopisu SearchBar](#searchbar-spellcheck).
- Zjišťování pořadí čtení z textového obsahu v [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), a [ `Label` ](xref:Xamarin.Forms.Label) instance. Další informace najdete v tématu [zjišťování pořadí čtení z obsahu](#inputview-readingorder).
- Zakázat režim starší verze barva na podporované [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Další informace najdete v tématu [zakázat režim starší verze barva](#legacy-color-mode).
- Povolení podpory gesto klepněte v [ `ListView` ](xref:Xamarin.Forms.ListView). Další informace najdete v tématu [povolení klepněte na gesto podpory v prvku ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Změna umístění panelu nástrojů

Chcete změnit umístění panelu nástrojů na se používá tato specifické pro platformu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)a je v jazyce XAML spotřebovávají nastavení [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) přidružená vlastnost na hodnotu [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) výčtu:

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

`Page.On<Windows>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v systému Windows. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) obor názvů, je použít k nastavení umístění panelu nástrojů s [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) poskytování – výčet tří hodnot: [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/), [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/), a [ `Bottom` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/).

Výsledkem je, že v umístění zadané nástrojů se použije na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) instance:

[![](windows-images/toolbar-placement.png "Panel nástrojů umístění specifické pro platformu")](windows-images/toolbar-placement-large.png#lightbox "příslušnou platformu umístění panelu nástrojů")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Sbalení MasterDetailPage navigační panel

Tato specifické pro platformu se používá k sbalit na navigačním panelu [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)a je v jazyce XAML spotřebovávají nastavení [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) a [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)přidružené vlastnosti:

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

`MasterDetailPage.On<Windows>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v systému Windows. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) obor názvů, slouží k určení styl sbalit s [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) poskytuje dva – výčet hodnoty: [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/) a [ `Partial` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/). [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Metoda se používá k určení šířku částečně sbalené navigačním panelu.

Výsledkem je, že zadané [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) se použije na [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) instance šířce také zadané:

[![](windows-images/collapsed-navigation-bar.png "Sbalené navigační panel specifické platformy")](windows-images/collapsed-navigation-bar-large.png#lightbox "sbalené navigační panel specifické platformy")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Zobrazení výstrah JavaScript

Umožňuje této specifické pro platformu [ `WebView` ](xref:Xamarin.Forms.WebView) zobrazení výstrah JavaScript v dialogu UWP zprávy. V jazyce XAML spotřebování nastavením [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) připojené vlastnosti `boolean` hodnotu:

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

`WebView.On<Windows>` Metoda určuje, že bude tento specifické pro platformu jenom spustit na univerzální platformu Windows. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, je slouží ke kontrole, jestli jsou povolené JavaScript výstrahy. Kromě toho `WebView.SetIsJavaScriptAlertEnabled` metoda slouží k přepnutí výstrah JavaScript voláním [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) metoda vrátí, zda je povoleno:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Výsledkem je, že JavaScript výstrahy lze zobrazit v dialogu UWP zpráva:

![Webové zobrazení JavaScript výstrahy specifické pro platformu](windows-images/webview-javascript-alert.png "JavaScript webové zobrazení výstrahy specifické pro platformu")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>Povolení kontrolu pravopisu SearchBar

Umožňuje této specifické pro platformu [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) k interakci s modulem kontroly pravopisu. V jazyce XAML spotřebování nastavením [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) připojené vlastnosti `boolean` hodnotu:

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

`SearchBar.On<Windows>` Metoda určuje, že bude tento specifické pro platformu jenom spustit na univerzální platformu Windows. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) oboru názvů změní kontrola pravopisu zapnout a vypnout. Kromě toho `SearchBar.SetIsSpellCheckEnabled` metoda slouží k přepnutí kontrola pravopisu voláním [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) metoda vrátí, zda je povoleno kontrola pravopisu:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Výsledkem je zadaný do této text [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) může být provedena kontrola pravopisu, s nesprávná slova, přičemž uveden uživateli:

![Kontrola specifické platformy pravopisu SearchBar](windows-images/searchbar-spellcheck.png "SearchBar pravopisu kontrola specifické pro platformu")

> [!NOTE]
> `SearchBar` Třídy v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) oboru názvů má také [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) a [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) metody, které slouží k povolení a zákaz Kontrola pravopisu v `SearchBar`, v uvedeném pořadí.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Zjišťování pořadí čtení z obsahu

Tato specifické pro platformu umožňuje čtení pořadí (zleva doprava nebo zprava doleva) obousměrný text v [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), a [ `Label` ](xref:Xamarin.Forms.Label) instance dynamicky za detekovanou. V jazyce XAML spotřebování nastavením [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (pro `Entry` a `Editor` instance) nebo [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) přidružená vlastnost k `boolean` hodnotu:

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

`Editor.On<Windows>` Metoda určuje, že bude tento specifické pro platformu jenom spustit na univerzální platformu Windows. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, je slouží ke kontrole, jestli je zjištěna pořadí čtení z obsahu v [ `InputView` ](xref:Xamarin.Forms.InputView). Kromě toho `InputView.SetDetectReadingOrderFromContent` metoda slouží k určí, zda pořadí čtení je zjištěna z obsahu voláním [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) metoda vrátí aktuální hodnota:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Výsledkem je, že [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), a [ `Label` ](xref:Xamarin.Forms.Label) instance může mít pořadí čtení zjištěno dynamicky obsah:

[![InputView zjišťování pořadí čtení z obsahu specifické pro platformu](windows-images/editor-readingorder.png "InputView zjišťování pořadí čtení z obsahu specifické pro platformu")](windows-images/editor-readingorder-large.png#lightbox "InputView zjišťování pořadí čtení z obsah specifické pro platformu")

> [!NOTE]
> Na rozdíl od nastavení [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost, logiku pro zobrazení, která zjišťují pořadí čtení z jejich textového obsahu nebude mít vliv na zarovnání textu v rámci zobrazení. Místo toho upraví pořadí, ve kterém jsou nastíněny bloky obousměrného textu.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Zakázat režim starší verze barev

Některé z Xamarin.Forms zobrazení funkce režim starší verze barev. V tomto režimu když [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) zobrazení je nastavena na `false`, zobrazení se přepíše barvy nastavený uživatelem s nativní výchozí barvy pro zakázaném stavu. Pro zpětné kompatibility, tento režim starší verze barva zůstane výchozí chování pro podporované zobrazení.

Tato specifické pro platformu zakáže tento režim starší verze barvu, aby barvy nastavený na zobrazení uživatelem zůstanou i v případě, že zobrazení je zakázáno. V jazyce XAML spotřebování nastavením [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) přidružená vlastnost k `false`:

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

`VisualElement.On<Windows>` Metoda určuje, že tento specifické pro platformu lze spustit pouze v systému Windows. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, je slouží ke kontrole, zda režim starší verze barva je zakázán. Kromě toho [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) lze metoda vrátí, zda je zakázána režim starší verze barvy.

Výsledkem je, že lze zakázat režim starší verze barvy, takže barvy nastavený na zobrazení uživatelem zůstanou i když je zakázáno zobrazení:

![](windows-images/legacy-color-mode-disabled.png "Zakázat režim starší verze barvy")

> [!NOTE]
> Při nastavování [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) zobrazení, režim starší verze barva úplně ignorovány. Další informace o vizuálních stavů najdete v tématu [nástroje stavu Manager Visual Xamarin.Forms](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Povolení podpory gesto klepněte v prvku ListView

Na univerzální platformu Windows ve výchozím nastavení Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) používá nativního `ItemClick` událostí reagovat na interakce, místo nativního `Tapped` událostí. To poskytuje funkce pro usnadnění přístupu tak, aby Předčítání Windows a klávesnice mohou komunikovat s `ListView`. Však také vykreslí žádné klepněte na gesta uvnitř `ListView` přestane fungovat.

Tento ovládací prvky specifické pro platformu zda položky v [ `ListView` ](xref:Xamarin.Forms.ListView) může reagovat klepnout na gesta a proto zda nativního `ListView` aktivuje `ItemClick` nebo `Tapped` událostí. V jazyce XAML spotřebování nastavením [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) přidružená vlastnost na hodnotu [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) výčtu:

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

`ListView.On<Windows>` Metoda určuje, že bude tento specifické pro platformu jenom spustit na univerzální platformu Windows. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Metoda v [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) obor názvů, je slouží ke kontrole, jestli položky v [ `ListView` ](xref:Xamarin.Forms.ListView) může reagovat klepnout na gesta, se [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) výčtu poskytuje dvě možné hodnoty:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – Označuje, že `ListView` bude platit nativního `ItemClick` událostí ke zpracování interakce a proto nabízí funkce usnadnění přístupu. Proto Předčítání Windows a klávesnice mohou komunikovat s `ListView`. V však položky `ListView` nemůže reagovat na klepněte na gesta. Toto je výchozí chování pro `ListView` instancí na univerzální platformu Windows.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – Označuje, že `ListView` bude platit nativního `Tapped` událostí pro zpracování interakce. Proto položky v `ListView` může klepnout na gesta reagovat. Však není žádná funkce usnadnění přístupu a proto nelze Předčítání Windows a klávesnice interakci s `ListView`.

> [!NOTE]
> `Accessible` a `Inaccessible` režimy výběru se vzájemně vylučují, a budete muset zvolit přístupné [ `ListView` ](xref:Xamarin.Forms.ListView) nebo `ListView` , klepněte na gesta může reagovat.

Kromě toho [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) metoda lze vrátit aktuální [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Výsledek je, že zadané [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) se použije na [ `ListView` ](xref:Xamarin.Forms.ListView), ovládacích prvků, které zda položky v `ListView` může reagovat klepnout na gesta a proto zda nativního `ListView` aktivuje `ItemClick` nebo `Tapped` událostí.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat Windows platform podrobností, které jsou součástí Xamarin.Forms. Platforma specifika umožňují využívat funkce, která je dostupná pouze na konkrétní platformu, bez implementace vlastní nástroji pro vykreslování nebo účinky.

## <a name="related-links"></a>Související odkazy

- [Vytváření specifik platforem](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
