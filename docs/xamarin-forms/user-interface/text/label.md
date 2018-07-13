---
title: Popisek Xamarin.Forms
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms popisek k zobrazení jedné a více řádky textu v aplikacích.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: ce602a84ea1024dc22298a3ec1567a9a34ad4a82
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995963"
---
# <a name="xamarinforms-label"></a>Popisek Xamarin.Forms

_Zobrazení textu v Xamarin.Forms_

`Label` Zobrazení se používá pro zobrazení textu i s více řádky. Popisky může mít vlastní písma (rodiny, velikosti a možnosti) a barevný text. Tento článek obsahuje následující témata:

- **[Zkrácení a zabalení](#Truncation_and_Wrapping)**  &ndash; zkrácení a možnosti zalamování pro zpracování situací, ve kterém se text nemůže vejít na jeden řádek.
- **[Písmo](#Font)**  &ndash; možnosti písma.
- **[Barva](#Color)**  &ndash; XT popisku volby barev.
- **[Formátovaný Text](#Formatted_Text)**  &ndash; možnosti pro zobrazení textu s více formátů a stylů vložené.

## <a name="styling-label"></a>Používání stylů pro popisek

Následující části se věnují nastavení vlastnosti `Label` ručně na základě jednotlivé instance. Všimněte si, že nastaví vlastnosti mohou být seskupeny do jednoho styl, který konzistentně platí pro jeden nebo více zobrazení. To může zvýšení přehlednosti kódu a změny návrhu usnadnil. Zobrazit [styly](~/xamarin-forms/user-interface/text/styles.md) Další informace.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Zkrácení a zabalení

Popisky lze nastavit pro zpracování textu, který se nemůže vejít na jeden řádek v jednom z několika způsobů vystavené `LineBreakMode` vlastnost. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) je výčet z následujících možností:

- **HeadTruncation** &ndash; zkrátí hlavní text zobrazující do konce.
- **CharacterWrap** &ndash; zalomí text na nový řádek na hranici znak.
- **MiddleTruncation** &ndash; zobrazí začátku a konci textu, střední nahradit pomocí tři teček.
- **NoWrap** &ndash; nezalamuje text zobrazení pouze tolik text jako může vešel na jeden řádek.
- **TailTruncation** &ndash; ukazuje začátek textu, zkracování do konce.
- **WordWrap** &ndash; zalomí text na hranici slova.

## <a name="font"></a>Písma

Zobrazit [práce s písmy](~/xamarin-forms/user-interface/text/fonts.md) Další informace.

## <a name="color"></a>Barva

`Label`můžete nastavit s vlastní barvy prostřednictvím umožňujících vazbu `TextColor` vlastnost.

Zvláštní pozornost je třeba zajistit, že bude možné použít na jednotlivých platformách barvy. Protože každá platforma má různé výchozí hodnoty pro barvy textu a pozadí, musíte být opatrní a vyberte výchozí hodnotu, která funguje na každém.

Pomocí následujícího kódu nastavit barvu textu popisku:

V kódu:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

V XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "Příklad popisku TextColor")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formátovaný Text

Zobrazit popisky `FormattedText` vlastnost, která umožňuje zobrazit text s víc písem a barev ve stejném zobrazení.

`FormattedText` Vlastnost je typu [ `FormattedString` ](xref:Xamarin.Forms.FormattedString). Formátované řetězce se skládají z nejméně jednu `Span`s, každý z nich s následujícími vlastnostmi:

- **BackgroundColor** &ndash; umožňuje nastavit vlastní barvu pozadí, třeba když chcete dosáhnout zvýrazňovači nepoužívaného mohou mít vliv.
- **FontAttributes** &ndash; může být nastavenou na tučné, kurzíva nebo ani jeden.
- **FontFamily** &ndash; nastaví písmo, který se má použít.
- **FontSize** &ndash; nastaví velikost textu.
- **ForegroundColor** &ndash; nastaví barvu textu.
- **Text** &ndash; text, který se znovu nezobrazí.

Následující kód jazyka C# ukazuje, kde první slovo je tučný a poslední slovo je červený popisek:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

V XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "Příklad FormattedText popisek")


## <a name="related-links"></a>Související odkazy

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitolu 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Popisek rozhraní API](xref:Xamarin.Forms.Label)
