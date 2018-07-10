---
title: Třída zařízení Xamarin.Forms
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms zařízení pro detailní kontrolu nad funkce a rozložení na základě podle platformy.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: ff707cdf73665ae07881d2d17ec837a4cfacaca0
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935368"
---
# <a name="xamarinforms-device-class"></a>Třída zařízení Xamarin.Forms

[ `Device` ](xref:Xamarin.Forms.Device) Třída obsahuje několik vlastností a metod, což vývojářům umožňuje přizpůsobení rozložení a funkce na základě podle platformy.

Kromě metod a vlastností na cílový kód na konkrétní typy hardwaru a velikosti `Device` třída zahrnuje [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) metodu, která má být použit při interakci v uživatelském rozhraní ovládacích prvků z vlákna na pozadí.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Zadání hodnot pro konkrétní platformu

Před Xamarin.Forms 2.3.4 platformu aplikace byla spuštěna na může získat prozkoumáním [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) vlastnost a porovnání na [ `TargetPlatform.iOS` ](xref:Xamarin.Forms.TargetPlatform.iOS), [ `TargetPlatform.Android` ](xref:Xamarin.Forms.TargetPlatform.Android), [ `TargetPlatform.WinPhone` ](xref:Xamarin.Forms.TargetPlatform.WinPhone), a [ `TargetPlatform.Windows` ](xref:Xamarin.Forms.TargetPlatform.Windows) hodnot výčtu. Podobně, jeden z [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) přetížení lze zadat hodnoty specifické pro platformu k ovládacímu prvku.

Nicméně od Xamarin.Forms 2.3.4 Tato rozhraní API byly zastaralé a nahradit. [ `Device` ](xref:Xamarin.Forms.Device) Třída nyní obsahuje veřejné řetězcové konstanty, které identifikují platformy – [ `Device.iOS` ](xref:Xamarin.Forms.Device.iOS), [ `Device.Android` ](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`() zastaralé), `Device.WinRT` (zastaralé), [ `Device.UWP` ](xref:Xamarin.Forms.Device.UWP), a [ `Device.macOS` ](xref:Xamarin.Forms.Device.macOS). Podobně [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) přetížení se nahradily [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) a [ `On` ](xref:Xamarin.Forms.On) rozhraní API.

V jazyce C#, lze zadat hodnoty specifické pro platformy tak, že vytvoříte `switch` příkaz na [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) vlastnost a potom zadat `case` příkazy pro požadované platformy:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) a [ `On` ](xref:Xamarin.Forms.On) třídy poskytují stejné funkce v XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Třída je obecná třída a musí být vytvořena pomocí `x:TypeArguments` atribut, který odpovídá cílovým typem. V [ `On` ](xref:Xamarin.Forms.On) třídy, [ `Platform` ](xref:Xamarin.Forms.On.Platform) atribut může přijmout jediného `string` hodnotu nebo více oddělených čárkou `string` hodnoty.

> [!IMPORTANT]
> Poskytuje nesprávné `Platform` hodnotu v atributu `On` třídy nesmí dojít k chybě. Místo toho kód se spustí bez použití hodnoty specifické pro platformu.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Lze použít ke změně rozložení nebo funkce podle aplikace běží na zařízení. [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom) Výčet obsahuje následující hodnoty:

-  **Phone** – iPhone, iPod touch a zařízení s Androidem užší než 600 vyhrazené IP adresy ^
-  **Tablet** – iPad, zařízení s Windows a androidem širší než 600 vyhrazené IP adresy ^
-  **Desktop** – pouze pro vrácené v [aplikací pro UWP](~/xamarin-forms/platform/windows/installation/index.md) na stolní počítače s Windows 10 (vrátí `Phone` na mobilních zařízeních Windows, včetně v situacích Continuum)
-  **TV** – Tizen TV zařízení
-  **Nepodporovaná** – nepoužívané

*^ vyhrazené IP adresy není nutně počet fyzických pixelů*

`Idiom` je užitečná zejména při vytváření rozložení, které budou využívat větší obrazovek, následujícím způsobem:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

## <a name="deviceflowdirection"></a>Device.FlowDirection

[ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Hodnota načte [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) hodnota výčtu, která představuje aktuální směr toku se použité v zařízení. Směr toku je směr, ve kterém jsou prvky uživatelského rozhraní na stránce skenovalo okem. Hodnoty výčtu jsou:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

V XAML [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) hodnotu můžete získat pomocí `x:Static` – rozšíření značek:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Ekvivalentní kód v jazyce C# je:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Další informace o směr toku, najdete v části [lokalizace zprava doleva](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Vlastnost](~/xamarin-forms/user-interface/styles/index.md) obsahuje integrované definice, které mohou být použity některé ovládací prvky (například `Label`) `Style` vlastnost. Jsou dostupné styly:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` je možné při nastavování [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) v kódu jazyka C#:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

`OpenUri` Metodu můžete použít k aktivaci operací na základní platformě, jako je například otevřít adresu URL do nativního webového prohlížeče (**Safari** v Iosu nebo **Internet** v systému Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[WebView ukázka](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) obsahuje příklad použití `OpenUri` k otevírání adres URL a také aktivovat telefonní hovory.

[Vzorku mapy](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) také používá `Device.OpenUri` k zobrazení mapy a pokynů, pomocí nativní **mapuje** aplikací pro iOS a Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Třída má také `StartTimer` metodu, která poskytuje jednoduchý způsob, jak aktivovat úlohy závislé na čase, která funguje v Xamarin.Forms společný kód, včetně knihovny .NET Standard. Předat `TimeSpan` nastavte interval a vrátíte se `true` zachovat časovač s nebo `false` zastavit po aktuální volání.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Pokud kód uvnitř časovač spolupracuje s uživatelským rozhraním (například nastavení text `Label` nebo zobrazení výstrahy) by měl provést uvnitř příkazu `BeginInvokeOnMainThread` výrazu (viz níže).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Prvky uživatelského rozhraní by měl mít nikdy přístup vláken na pozadí, jako je například kód spuštěný v časovač nebo obslužné rutiny dokončení asynchronních operací, jako je webových požadavků. Jakýkoli kód na pozadí, který je potřeba aktualizovat uživatelské rozhraní by měl být uzavřen v [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Jde o ekvivalent `InvokeOnMainThread` v Iosu `RunOnUiThread` v Androidu a `Dispatcher.RunAsync` na univerzální platformu Windows.

Xamarin.Forms kód je:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Poznámka: Tento metod pomocí `async/await` není nutné používat `BeginInvokeOnMainThread` spuštěné z hlavního vlákna uživatelského rozhraní.

## <a name="summary"></a>Souhrn

Xamarin.Forms `Device` třída umožňuje detailní kontrolu nad funkce a rozložení na základě podle platformy – i v běžných kódu (projekty .NET Standard knihovny nebo sdílených projektech).


## <a name="related-links"></a>Související odkazy

- [Ukázkové zařízení](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Ukázka styly](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Zařízení](xref:Xamarin.Forms.Device)
