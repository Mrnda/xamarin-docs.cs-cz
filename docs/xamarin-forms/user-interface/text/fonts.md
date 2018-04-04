---
title: Písma
description: Nastavení písem v Xamarin.Forms
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e492bee2b43f2be54f450550e3f44e7da3de258e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="fonts"></a>Písma

Tento článek popisuje, jak Xamarin.Forms umožňuje určit atributy písma (včetně váhy a velikost) na ovládací prvky, které zobrazení textu. Písmo informace může být [zadaný v kódu](#Setting_Font_in_Code) nebo [zadaný v jazyce Xaml](#Setting_Font_in_Xaml).
Je také možné použít [vlastní písma](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Nastavení písma v kódu

Použijte tři písma související vlastnosti všech ovládacích prvků, které zobrazují text:

- **FontFamily** &ndash; `string` název písma.
- **Velikost písma** &ndash; velikost písma jako `double`.
- **FontAttributes** &ndash; řetězec určující informace o stylu jako *Kurzíva* a **Bold** (pomocí `FontAttributes` výčet v jazyce C#).

Tento kód ukazuje, jak vytvořit popisek a určete velikost písma a váhu k zobrazení:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Velikost písma

`FontSize` Může být nastavena na hodnotu double, například:

```csharp
label.FontSize = 24;
```

Můžete také `NamedSize` výčet, který obsahuje čtyři integrované možnosti; Xamarin.Forms vybere nejlepší velikost pro každou platformu.

-  **Micro**
-  **Malá**
-  **Medium**
-  **Velká**


`NamedSize` Výčtu lze použít kdekoli `FontSize` je možné zadat pomocí `Device.GetNamedSize` způsobů, jak převést hodnotu na `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Atributy písma

Styly písma, jako **tučné** a *Kurzíva* lze nastavit u `FontAttributes` vlastnost. Aktuálně jsou podporovány následující hodnoty:

-  **None**
-  **Bold**
-  **Kurzíva**

`FontAttribute` Výčtu je možné následujícím způsobem (můžete určit jeden atribut nebo `OR` je společně):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="formattedstring"></a>FormattedString

Některé ovládací prvky Xamarin.Forms (například `Label`) podporovat i jiné písmo atributy v rámci řetězec pomocí `FormattedString` třídy. A `FormattedString` se skládá z jedné nebo více `Span`s, z nichž každá může mít svůj vlastní formátování atributy.

`Span` Třída má následující atributy:

* **Text** &ndash; hodnota pro zobrazení
* **FontFamily** &ndash; název písma
* **Velikost písma** &ndash; velikost písma
* **FontAttributes** &ndash; jako informace o stylu *Kurzíva* a **tučně**
* **ForegroundColor** &ndash; barvy
* **BackgroundColor** &ndash; barva pozadí

Příklad vytváření a zobrazování `FormattedString` jsou uvedeny níže &ndash; Všimněte si, že je přiřazena k zobrazí popisky `FormattedText` vlastnost a ne `Text` vlastnost.

```csharp
var labelFormatted = new Label ();
var fs = new FormattedString ();
fs.Spans.Add (new Span { Text="Red, ", ForegroundColor = Color.Red, FontSize = 20, FontAttributes = FontAttributes.Italic });
fs.Spans.Add (new Span { Text=" blue, ", ForegroundColor = Color.Blue, FontSize = 32 });
fs.Spans.Add (new Span { Text=" and green!", ForegroundColor = Color.Green, FontSize = 12 });
labelFormatted.FormattedText = fs;
```


### <a name="setting-font-info-per-platform"></a>Informace o nastavení písma na každou platformu

Případně `Device.RuntimePlatform` vlastnost lze nastavit různé názvy písem pro každou platformu, jak je předvedeno v tento kód:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Je dobré zdroj informací písma pro iOS [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Nastavení písma v jazyce Xaml

Tento zobrazovaný text všechny ovládací prvky Xamarin.Forms `Font` vlastnost, která můžete nastavit v jazyce Xaml. Nejjednodušší způsob, jak nastavit písmo v jazyce Xaml je použití hodnoty výčtu s názvem velikost, jak je uvedeno v následujícím příkladu:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Je integrované převaděč pro `Font` vlastnost, která umožňuje všechna nastavení písma být vyjádřena jako hodnotu řetězce v jazyce Xaml. Následující příklady ukazují, jak můžete zadat atributy písma a velikosti v jazyce Xaml:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Chcete-li určit více `Font` nastavení zkombinovat požadovaná nastavení do atribut řetězce písma. Řetězec atributu písma musí být formátována jako `"[font-face],[attributes],[size]"`. Je důležité pořadí parametrů, všechny parametry jsou volitelné a více `attributes` lze zadat, například:

```xaml
<Label Text="Small bold text" FontAttributes="Bold" FontSize="Micro" />
<Label Text="Really big italic text" FontAttributes="Italic" FontSize="72" />
```

`FormattedString` Třídu lze také použít v jazyce XAML, jak je vidět tady:

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <FormattedString.Spans>
                <Span Text="Red, " ForegroundColor="Red" FontAttributes="Italic" FontSize="20" />
                <Span Text=" blue, " ForegroundColor="Blue" FontSize="32" />
                <Span Text=" and green! " ForegroundColor="Green" FontSize="12"/>
            </FormattedString.Spans>
        </FormattedString>
    </Label.FormattedText>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) Můžete také použít v jazyce XAML k vykreslení jiné písmo na každou platformu. Následující příklad používá řez vlastní písmo v systému iOS (<span style="font-family:MarkerFelt-Thin">dynamicky MarkerFelt</span>) a určuje pouze velikost/atributy na jiných platformách:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Při zadávání řez vlastní písma, vždycky je vhodné použít `OnPlatform`, protože je obtížné vyhledat písmo, které je k dispozici na všech platformách.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Pomocí vlastního písma

Použití písem než předdefinované řezů písma vyžaduje některé specifické pro platformu kódování. Tento snímek obrazovky ukazuje vlastní písmo **severského** z [Google open-source písem](https://www.google.com/fonts) vykreslen v systému iOS, Android a Windows Phone pomocí Xamarin.Forms.

 [![Vlastní písma na iOS a Android](fonts-images/custom-sml.png "vlastní písem příklad")](fonts-images/custom.png#lightbox "příklad vlastní písem")

Níže jsou uvedeny kroky potřebné pro každou platformu. Při zahrnutí souborů vlastní písma s aplikací, ujistěte se, že jste ověřte, jestli písma licence poskytuje pro distribuci.

### <a name="ios"></a>iOS

Je možné zobrazit vlastní písma tak, že nejdřív zajistit, že je načtena, pak na ně odkazovat podle názvu pomocí Xamarin.Forms `Font` metody.
Postupujte podle pokynů v [tomto příspěvku na blogu](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Přidejte soubor písma s **akce sestavení: BundleResource**, a
2. Aktualizace **Info.plist** souboru (**písem poskytované aplikace**, nebo `UIAppFonts`, klíče), pak
3. Na ni odkazuje podle názvu bez ohledu na definujete písmo v Xamarin.Forms!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms pro Android můžete odkazovat vlastní písma, který byl přidán do projektu podle konkrétní standardní pojmenování. Nejprve přidat soubor písma **prostředky** složky v projektu aplikace a sadu *akce sestavení: AndroidAsset*. Pak použijte úplnou cestu a *název písma* oddělených křížku (#) jako název písma v Xamarin.Forms, jak ukazuje následující fragment kódu:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Vlastní písma, který byl přidán do projektu podle konkrétní standardní pojmenování můžete odkazovat Xamarin.Forms na platformách systému Windows. Nejprve přidat soubor písma **/Assets/písem/** složky v projektu aplikace a sadu <span class="UIItem">sestavení akce: obsah</span>. Pak použijte úplnou cestu a písma filename, za nímž následuje křížku (#) a <span class="UIItem">název písma</span>, jak ukazuje následující fragment kódu:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Poznámka: název souboru písma a název písma může lišit. Chcete-li zjistit název písmo v systému Windows, klikněte pravým tlačítkem na soubor ttf a vyberte **Preview**. Název písma můžete určit pak z okna náhledu.

Společný kód aplikace je nyní dokončen. Kód programu Telefon specifické pro platformu phone se teď implementuje jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Můžete také použít [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) v jazyce XAML pro vykreslení vlastního písma:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>Souhrn

Xamarin.Forms poskytuje jednoduché výchozí nastavení, umožňují velikost textu snadno pro všechny podporované platformy. Také umožňuje vám určit vzhled písma a velikost &ndash; i pro každou platformu &ndash; po požadované jemněji odstupňované řízení. `FormattedString` Třídu lze použít k vytvoření řetězec obsahující specifikace jiné písmo pomocí `Span` třídy.

Písmo informace lze zadat také v jazyce Xaml pomocí atributů správně formátovaného písma nebo `FormattedString` element s `Span` podřízené objekty.


## <a name="related-links"></a>Související odkazy

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
