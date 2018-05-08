---
title: Lokalizace zprava doleva
description: Lokalizace zprava doleva přidává podporu pro směr toku zprava doleva na platformě Xamarin.Forms aplikace.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/18/2018
ms.openlocfilehash: 001c9f6f26fc96ca0fd0d25e150c5ec9409e552a
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="right-to-left-localization"></a>Lokalizace zprava doleva

_Lokalizace zprava doleva přidává podporu pro směr toku zprava doleva na platformě Xamarin.Forms aplikace._

> [!NOTE]
> Lokalizace zprava doleva vyžaduje použití iOS 9 nebo vyšší a rozhraní API 17 nebo vyšší v systému Android.

Směr toku je směr, ve kterém jsou prvky uživatelského rozhraní na stránce skenovalo oko. Některé jazyky, jako je arabština a hebrejština, vyžadují, aby prvky uživatelského rozhraní jsou nastíněny v směr toku zprava doleva. Toho lze dosáhnout nastavením [ `VisualElement.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost. Tato vlastnost získá nebo nastaví směr, ve které toku elementy uživatelského rozhraní v rámci žádnému nadřazenému prvku, který řídí jejich rozložení a musí být nastavena na jednu z [ `FlowDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlowDirection/) hodnot výčtu:

- [`LeftToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.LeftToRight/)
- [`RightToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.RightToLeft/)
- [`MatchParent`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.MatchParent/)

Nastavení [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost [ `RightToLeft` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.RightToLeft/) v elementu obecně Nastaví zarovnání vpravo, pořadí čtení zprava doleva a rozložení ovládacího prvku, které jsou předávány z zprava doleva:

[![TodoItemPage v arabské s směr toku zprava doleva](rtl-images/TodoItemPage-Arabic.png "TodoItemPage v arabské s směr toku zprava doleva")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage v arabské s směr toku zprava doleva")

> [!TIP]
> Byste měli nastavit pouze [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost na počáteční rozložení. Tato změna za běhu způsobí, že nákladné rozložení proces, který bude mít vliv na výkon.

Výchozí hodnota [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) je hodnota vlastnosti pro element bez nadřazený [ `LeftToRight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.LeftToRight/), při výchozí `FlowDirection` pro element s nadřazeným [ `MatchParent`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.MatchParent/). Proto dědí element `FlowDirection` hodnotu vlastnosti z nadřazené ve vizuální strojové struktuře a libovolný element, můžete přepsat hodnotu získá z nadřazené.

> [!TIP]
> Při lokalizaci aplikace pro zprava doleva jazyky, nastavte [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost na stránce nebo kořenové rozložení. To způsobí, že všechny elementy, které obsahuje stránky, nebo kořenový rozložení, odpovídajícím způsobem odpovědět na směr toku.

## <a name="respecting-device-flow-direction"></a>Bere ohledy na zařízení směr toku

Respektováním směr toku zařízení založená na vybraný jazyk a oblast je na volbu explicitní developer a neprobíhá automaticky. Jde dosáhnout nastavením [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost na stránce nebo kořenové rozložení k `static` [ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.FlowDirection/) hodnotu:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Všechny podřízené elementy stránky nebo kořenové rozložení, bude ve výchozím nastavení pak dědit [ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.FlowDirection/) hodnotu.

## <a name="platform-setup"></a>Instalační program platformy

Nastavení konkrétní platformy je potřeba povolit národní prostředí zprava doleva.

### <a name="ios"></a>iOS

Požadované národní prostředí zprava doleva musí být přidaní jako podporovaném jazyce do položky pole `CFBundleLocalizations` klíče v **Info.plist**. Následující příklad ukazuje, arabské přidána do pole pro `CFBundleLocalizations` klíč:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Jazyky podporované info.plist](rtl-images/ios-locales.png "Info.plist podporované jazyky")

Další informace najdete v tématu [základy lokalizace v iOS](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Lokalizace zprava doleva může být testována pak změnou jazyk a oblast v simulátoru zařízení nebo na zprava doleva národního prostředí, která byla zadaná v **Info.plist**.

> [!WARNING]
> Pamatujte, že při změně jazyka a oblasti do zprava doleva národního prostředí v systému iOS, všechny [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) zobrazení bude vyvolána výjimka, pokud neuvedete prostředky požadované pro národní prostředí. Například při testování aplikace v arabština, který má `DatePicker`, ujistěte se, že **mideast** je vybraný v **internacionalizace** části **iOS sestavení** podokně.

### <a name="android"></a>Android

Aplikace **AndroidManifest.xml** souboru by měly být aktualizovány tak, aby `<uses-sdk>` uzel nastaví `android:minSdkVersion` atribut 17 a `<application>` uzel nastaví `android:supportsRtl` atribut `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Lokalizace zprava doleva může být testována pak změnou zařízení nebo emulátoru použití jazyka zprava doleva nebo povolením **směru rozložení Force RTL** v **Nastavení > Možnosti pro vývojáře**.

### <a name="universal-windows-platform-uwp"></a>Univerzální platforma Windows (UPW)

Prostředky požadované jazyka musí být zadán v `<Resources>` uzlu **Package.appxmanifest** souboru. Následující příklad ukazuje, arabské přidané do `<Resources>` uzlu:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Kromě toho UWP vyžaduje, že výchozí jazykovou verzi aplikace byl explicitně definován v knihovně standardní rozhraní .NET. To můžete udělat nastavením `NeutralResourcesLanguage` atribut `AssemblyInfo.cs`, nebo v jiné třídě, na výchozí jazykovou verzi:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Lokalizace zprava doleva můžete otestovat pak můžete změnit jazyk a oblast na zařízení k příslušné národní prostředí zprava doleva.

## <a name="limitations"></a>Omezení

Lokalizace zprava doleva Xamarin.Forms aktuálně má několik omezení:

- [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) umístění tlačítka panelu nástrojů položky umístění a přechodu animace se řídí zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.
- [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Směr prstem není překlopit.
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) vizuální obsah není překlopit.
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) a [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) řídí orientace zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.
- [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) obsah nedodržuje [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.
- A `TextDirection` vlastnost musí být přidán, řídit zarovnání textu.

### <a name="ios"></a>iOS

- [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) orientace se řídí zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) zarovnání textu se řídí zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.
- [`ContextActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) gesta a zarovnání nejsou vrátit zpět.

### <a name="android"></a>Android

- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) orientace se řídí zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.
- [`ContextActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) umístění se řídí zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.

### <a name="uwp"></a>UWP

- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) zarovnání textu se řídí zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.
- [`FlowDirection`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) Vlastnost není zdědí [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) podřízené objekty.
- [`ContextActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) zarovnání textu se řídí zařízení národní prostředí, místo [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) vlastnost.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Právo na levém jazyk podporovat Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 zprava doleva podporují nástrojem [univerzity Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [TodoLocalizedRTL ukázkové aplikace](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
