---
title: Zařízení – třída
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 471616dffc700cf93a9f6435565222d7628bf165
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="device-class"></a>Zařízení – třída

[ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Třída obsahuje řadu vlastností a metod, což vývojářům přizpůsobení rozložení a funkce na základě na platformu.

Kromě metod a vlastností pro cílový kód na konkrétní typy hardwaru a velikosti `Device` třída zahrnuje [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) metoda, která se má použít při interakci s uživatelským rozhraním z ovládací prvky vlákna na pozadí.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Zadáním hodnot specifických pro platformy

Před Xamarin.Forms 2.3.4, může získat platforma byla aplikace spuštěná v tak, že prověří [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) vlastnost a porovná je do [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/), [ `TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/), [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), a [ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) hodnot výčtu. Podobně, jeden z [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) přetížení může poskytovat specifické pro platformu hodnot do ovládacího prvku.

Ale protože Xamarin.Forms 2.3.4 Tato rozhraní API byly zastaralé a nahradit. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Třída teď obsahuje veřejné řetězcové konstanty, které identifikují platformy – [ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), [ `Device.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/) (zastaralý), [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/) (zastaralý), [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), a [ `Device.macOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/). Podobně [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) přetížení nahradil [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) a [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) rozhraní API.

V jazyce C#, specifické pro platformu hodnoty lze zadat tak, že vytvoříte `switch` příkaz na [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) vlastnosti a pak zadají `case` příkazy pro požadované platformy:

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

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) a [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) třídy poskytují stejnou funkcionalitu v jazyce XAML:

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

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Třída je obecné třídy a musí být vytvořena s `x:TypeArguments` atribut, který odpovídá typu cíle. V [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) třídy, [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) atribut může přijmout jeden `string` hodnotu, nebo více oddělených čárkou `string` hodnoty.

> [!IMPORTANT]
> Poskytnutí nesprávné `Platform` hodnotu v atributu `On` třída nebude mít za následek chybu. Místo toho kód spustí bez hodnoty specifické pro platformu bylo použito.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Lze použít ke změně rozložení nebo funkce podle aplikace běží na zařízení. [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/) Výčet obsahuje následující hodnoty:

-  **Phone** – iPhone, iPod touch a zařízení se systémem Android užší než 600 vyhrazené IP adresy ^
-  **Tablet** – iPad, zařízení se systémem Windows a zařízení se systémem Android širší než 600 vyhrazené IP adresy ^
-  **Plocha** – jen, vrátí se v [aplikace UWP](~/xamarin-forms/platform/windows/installation/index.md) na stolní počítače s Windows 10 (vrátí `Phone` na mobilní zařízení s Windows, včetně v situacích poměrně)
-  **TV** – Tizen TV zařízení
-  **Nepodporované** – nepoužívané

*^ vyhrazené IP adresy není nutně počet fyzických pixelů*

`Idiom` je užitečná zejména při vytváření rozložení, které využít větší obrazovky takto:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Vlastnost](~/xamarin-forms/user-interface/styles/index.md) obsahuje definice předdefinovaný styl, které mohou být použity pro některé ovládací prvky (jako například `Label`) `Style` vlastnost. Styly k dispozici jsou:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` lze použít při nastavení [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) v kódu jazyka C#:

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

`OpenUri` Metoda umožňuje spouštět operace na základní platformě, jako je například otevřete a adresy URL v nativní webový prohlížeč (**Safari** v systému iOS nebo **Internet** v systému Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[Ukázkové webové zobrazení](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) obsahuje příklad použití `OpenUri` k otevírání adres URL a taky aktivovat telefonní hovory.

[Mapy ukázka](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) také používá `Device.OpenUri` k zobrazení mapy a pokynů pomocí nativního **mapuje** aplikace v iOS a Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Třída také obsahuje `StartTimer` metodu, která poskytuje jednoduchý způsob k aktivaci úlohy závislá na čase, který funguje v Xamarin.Forms společný kód (včetně PCLs). Předat `TimeSpan` nastavit interval a vrátíte se `true` zachovat časovač spuštěná nebo `false` zastavit po aktuální volání.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Pokud kód uvnitř časovač komunikuje s uživatelským rozhraním (například nastavení text `Label` nebo zobrazení výstrahy) by mělo být provedeno uvnitř `BeginInvokeOnMainThread` výrazu (viz níže).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Prvky uživatelského rozhraní by měly být dostupné nikdy vlákna na pozadí, jako je například kód spuštěný v časovač nebo dokončení obslužnou rutinu pro asynchronní operace, jako jsou například webové žádosti. Pozadí kód, který je potřeba aktualizovat uživatelské rozhraní by měl být uzavřen uvnitř [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Jedná se o ekvivalent `InvokeOnMainThread` v systému iOS, `RunOnUiThread` v systému Android, a `Dispatcher.RunAsync` na univerzální platformu Windows.

Kód Xamarin.Forms je:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Poznámka: Tento metod pomocí `async/await` nemusíte používat `BeginInvokeOnMainThread` spuštěné z hlavního vlákna uživatelského rozhraní.

## <a name="summary"></a>Souhrn

Platformě Xamarin.Forms `Device` třída umožňuje jemně odstupňovanou kontrolu nad funkce a rozložení na základě na platformu – společné i v kódu (PCL nebo sdílených projektů).


## <a name="related-links"></a>Související odkazy

- [Ukázka zařízení](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Ukázka styly](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Zařízení](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
