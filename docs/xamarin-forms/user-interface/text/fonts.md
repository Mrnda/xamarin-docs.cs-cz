---
title: Písma v Xamarin.Forms
description: Tento článek vysvětluje, jak k určení informace písma pro ovládací prvky, které se zobrazí text v aplikacích Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e6635bc13214a5a4e728fa3e71db86a8ea1c39d6
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202952"
---
# <a name="fonts-in-xamarinforms"></a>Písma v Xamarin.Forms

Tento článek popisuje, jak Xamarin.Forms umožňuje zadat atributy písma (včetně váhy a velikosti) na ovládací prvky, které se zobrazí text. Písmo informace mohou být [zadat v kódu](#Setting_Font_in_Code) nebo [zadané v XAML](#Setting_Font_in_Xaml).
Je také možné použít [vlastní písmo](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Nastavení písma v kódu

Použijte tři vlastnosti související s písma všech ovládacích prvků, které se zobrazí text:

- **FontFamily** &ndash; `string` název písma.
- **FontSize** &ndash; velikost písma jako `double`.
- **FontAttributes** &ndash; řetězec určující styl informace, například *Kurzíva* a **tučné** (pomocí `FontAttributes` výčtu v jazyce C#).

Tento kód ukazuje, jak vytvořit popisek a určete velikost písma a váhy k zobrazení:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Velikost písma

`FontSize` Vlastnost lze nastavit na hodnotu double pro instanci:

```csharp
label.FontSize = 24;
```

Můžete také použít `NamedSize` výčet, který obsahuje čtyři integrované možnosti; Xamarin.Forms zvolí ideální velikost pro každou platformu.

-  **Micro**
-  **Malé**
-  **Střední**
-  **Velké**

`NamedSize` Výčtu lze použít všude, kde `FontSize` lze pomocí `Device.GetNamedSize` způsobů, jak převést hodnotu na `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Atributy písma

Styly písma, jako **tučné** a *Kurzíva* lze nastavit na `FontAttributes` vlastnost. Aktuálně jsou podporovány následující hodnoty:

-  **None**
-  **Tučné**
-  **Kurzíva**

`FontAttribute` Výčtu je možné následujícím způsobem (můžete zadat jeden atribut nebo `OR` dohromady):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>Informace o nastavení písma jednotlivé platformy

Další možností `Device.RuntimePlatform` vlastnost lze nastavit různé názvy na jednotlivých platformách, jak je ukázáno v tomto kódu:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Dobrým zdrojem informací písma pro iOS je [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Nastavení písma v XAML

Tento text zobrazení všech určuje Xamarin.Forms `Font` vlastnost, která je možné nastavit v XAML. Nejjednodušší způsob, jak nastavit písmo v XAML je použití hodnoty výčtu s názvem velikost, jak je znázorněno v tomto příkladu:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Je integrované převaděč pro `Font` vlastnost, která umožňuje všechny nastavení písma a být vyjádřen jako hodnotu řetězce v XAML. Následující příklady ukazují, jak můžete zadat atributy písma a velikosti v XAML:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Chcete-li zadat více `Font` nastavení, zkombinovat do jednoho požadovaná nastavení `Font` atribut řetězců. Atribut řetězců písmo by měl být ve formátu `"[font-face],[attributes],[size]"`. Je důležité pořadí parametrů, všechny parametry jsou volitelné a více `attributes` lze upravit, například:

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) Můžete také použít v XAML k vykreslení písmo na jednotlivých platformách. V následujícím příkladu vlastní písmo plochy v systému iOS (<span style="font-family:MarkerFelt-Thin">dynamicky MarkerFelt</span>) a určuje pouze velikost/atributy na jiných platformách:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Při zadávání řez vlastní písma, je vždy vhodné použít `OnPlatform`, protože je obtížné najít písma, která je k dispozici na všech platformách.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Pomocí vlastní písma

Pomocí písma než integrované řezů písma vyžaduje kódování specifické pro platformu. Tento snímek obrazovky ukazuje toto písmo **Lobster** z [písma open source od Googlu](https://www.google.com/fonts) vykreslen pomocí Xamarin.Forms.

 [![Vlastní písmo v Iosu a Androidu](fonts-images/custom-sml.png "vlastní písma příklad")](fonts-images/custom.png#lightbox "příkladu vlastní písma")

Níže jsou uvedené kroky potřebné pro každou platformu. Při zahrnutí souborů vlastního písma do aplikace, nezapomeňte si ověřit, že licence písma umožňuje distribuci.

### <a name="ios"></a>iOS

Je možné zobrazit vlastní písma tak, že nejdřív se ujistíme, že je načtena, pak na ni odkazuje podle názvu pomocí Xamarin.Forms `Font` metody.
Postupujte podle pokynů v [tento příspěvek na blogu](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Přidat soubor písma s **Build Action: BundleResource**, a
2. Aktualizace **Info.plist** souboru (**písma poskytnutá aplikací**, nebo `UIAppFonts`, key), pak
3. Na ni odkazovat podle názvu bez ohledu na to definujete písmo v Xamarin.Forms!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms pro Android můžete odkazovat na vlastní písmo, které byly přidány do projektu podle konkrétní standardní pojmenování. Nejprve přidat soubor písmo **prostředky** složky v projektu aplikace a nastavte *Build Action: AndroidAsset*. Potom použít úplnou cestu a *název písma* oddělené hodnoty hash (#) jako název písma v Xamarin.Forms, jak ukazuje následující fragment kódu:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms pro platformy Windows můžete odkazovat na vlastní písmo, které byly přidány do projektu podle konkrétní standardní pojmenování. Nejprve přidat soubor písmo **/Assets/písma/** složky v projektu aplikace a nastavte <span class="UIItem">sestavení: obsah akce</span>. Potom použijte úplnou cestu a písma názvu souboru, za nímž následuje hodnoty hash (#) a <span class="UIItem">název písma</span>, jak ukazuje následující fragment kódu:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Všimněte si, že písmo názvu souboru a název písma může být jiný. Chcete-li zjistit název písma ve Windows, klikněte pravým tlačítkem na soubor ttf a vyberte **ve verzi Preview**. Název písma se potom dá určit z okna náhledu.

Společný kód aplikace je nyní dokončena. Specifické pro platformu telefonní vytáčecí zařízení kódu budou nyní implementovány jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Můžete také použít [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) v XAML pro vykreslení vlastního písma:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>Souhrn

Xamarin.Forms poskytuje jednoduchý výchozí nastavení, které umožňují upravit velikost textu u všech podporovaných platformách. Můžete ho taky určit vzhled písma a velikost &ndash; i jinak pro každou platformu &ndash; při vyžádáním jemněji odstupňovanou kontrolu.

Písmo informace lze zadat také v XAML pomocí atributů správně formátované písma.

## <a name="related-links"></a>Související odkazy

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
