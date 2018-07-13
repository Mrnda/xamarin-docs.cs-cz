---
title: Lokalizace zprava doleva
description: Lokalizace zprava doleva přidává podporu pro směr zprava doleva toku do aplikace Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 6c0de68f974c704b5f43232a1fc98065c90ee4f7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995719"
---
# <a name="right-to-left-localization"></a>Lokalizace zprava doleva

_Lokalizace zprava doleva přidává podporu pro směr zprava doleva toku do aplikace Xamarin.Forms._

> [!NOTE]
> Lokalizace zprava doleva vyžaduje použití zařízení s Iosem 9 nebo vyšší a rozhraní API 17 nebo vyšší na Androidu.

Směr toku je směr, ve kterém jsou prvky uživatelského rozhraní na stránce skenovalo okem. Některé jazyky, jako je arabština nebo hebrejština, vyžadovat, že prvky uživatelského rozhraní jsou nastíněny v směr toku zprava doleva. Toho lze dosáhnout nastavením [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost. Tato vlastnost získá nebo nastaví směr, ve kterých tok prvky uživatelského rozhraní v jakémkoliv nadřazeném prvku, který ovládá jejich rozložení a musí být nastavena na jednu z [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) hodnot výčtu:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Nastavení [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) v elementu obecně Nastaví zarovnání na pravé straně, pořadí čtení zprava doleva a rozložení ovládacího prvku směrovat z zprava doleva:

[![TodoItemPage v arabštině ve směru zprava doleva tok](rtl-images/TodoItemPage-Arabic.png "TodoItemPage v arabštině ve směru zprava doleva tok")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage v arabštině s směr toku zprava doleva")

> [!TIP]
> Měli byste nastavit pouze [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost na počáteční rozložení. Změna za běhu této hodnoty způsobí, že nákladné rozložení proces, který bude mít vliv na výkon.

Výchozí hodnota [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) je hodnota vlastnosti pro prvek bez nadřazených [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), zatímco výchozí `FlowDirection` pro element s nadřazeným je [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Proto element dědí `FlowDirection` hodnota vlastnosti z nadřazeného ve vizuálním stromu a libovolný element lze přepsat hodnotu získá ze svého nadřazeného objektu.

> [!TIP]
> Při lokalizaci aplikace pro jazyky zprava doleva, nastavte [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnosti rozložení stránky nebo root. To způsobí, že všechny prvky obsažené v rámci stránky nebo kořenové rozložení správně reagovat na směru toku.

## <a name="respecting-device-flow-direction"></a>Ale současně zachovává směr toku zařízení

Ale současně zachovává směr toku zařízení založené na vybraný jazyk a oblast, na volbu explicitní pro vývojáře a neprobíhá automaticky. Můžete dosáhnout nastavením [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost na stránce nebo kořenového rozložení do `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) hodnotu:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Všechny podřízené prvky stránky nebo kořenové rozložení, ve výchozím nastavení pak zdědí [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) hodnotu.

## <a name="platform-setup"></a>Nastavení platformy

Nastavení konkrétní platformy je potřeba povolit národní prostředí zprava doleva.

### <a name="ios"></a>iOS

Požadovaného národního prostředí zprava doleva přidaly jako podporovaný jazyk pro položky pole pro `CFBundleLocalizations` klíče v **Info.plist**. Následující příklad ukazuje arabština s byl přidán do pole pro `CFBundleLocalizations` klíč:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Jazyky podporované v souboru info.plist](rtl-images/ios-locales.png "Info.plist podporované jazyky")

Další informace najdete v tématu [základy lokalizace v iOS](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Lokalizace zprava doleva pak můžete otestovat tak, že změníte jazyka a oblasti na zařízení nebo simulátor na zprava doleva národní prostředí, který byl zadán v **Info.plist**.

> [!WARNING]
> Upozorňujeme, že při změně jazyka a oblasti na národním prostředí zprava doleva v Iosu, všechny [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) zobrazení vyvolá výjimku, pokud není zadána do prostředků potřebný pro národní prostředí. Například při testování aplikace v arabštině, který má `DatePicker`, ujistěte se, že **mideast** výběru v **internacionalizace** část **iOS Build** podokně.

### <a name="android"></a>Android

Aplikace **AndroidManifest.xml** souboru by se měla aktualizovat tak, aby `<uses-sdk>` sad uzlů `android:minSdkVersion` atribut 17 a `<application>` sad uzlů `android:supportsRtl` atribut `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Lokalizace zprava doleva lze potom testovat tak, že změníte zařízení nebo emulátor používat jazyk zprava doleva nebo tím, že **směr rozložení zprava doleva. platnost** v **Nastavení > Možnosti pro vývojáře**.

### <a name="universal-windows-platform-uwp"></a>Univerzální platforma Windows (UPW)

Požadované jazykové prostředky musí být zadán v `<Resources>` uzlu **Package.appxmanifest** souboru. Následující příklad ukazuje arabština s byl přidán do `<Resources>` uzlu:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Kromě toho UWP vyžaduje, že výchozí jazyková verze aplikace není výslovně uveden v knihovně .NET Standard. Toho můžete docílit tak, že nastavíte `NeutralResourcesLanguage` atribut `AssemblyInfo.cs`, nebo v jiné třídy, pro výchozí jazykovou verzi:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Lokalizace zprava doleva lze potom testovat tak, že změníte jazyka a oblasti na zařízení na odpovídající národní prostředí zprava doleva.

## <a name="limitations"></a>Omezení

Lokalizace zprava doleva Xamarin.Forms aktuálně má několik omezení:

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) umístění tlačítka panelu nástrojů položku umístění a přechod animace se řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) Směr potáhnutí prstem není Převrátit na ose.
- [`Image`](xref:Xamarin.Forms.Image) vizuální obsah není Převrátit na ose.
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) a [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) orientaci se řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.
- [`WebView`](xref:Xamarin.Forms.WebView) obsah nerespektuje [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.
- A `TextDirection` vlastnost musí být přidán, k řízení zarovnání textu.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) orientace řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) zarovnání textu se řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) gesta a zarovnání nejsou v obráceném pořadí.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) orientace řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) umístění se řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) zarovnání textu se řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Vlastnost není děděný [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) podřízené položky.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) zarovnání textu se řídí zařízení národní prostředí, spíše než [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) vlastnost.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Právo na levém jazykové podpory s Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Podpora Xamarin.Forms 3.0 zprava doleva, podle [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [TodoLocalizedRTL ukázkové aplikace](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
