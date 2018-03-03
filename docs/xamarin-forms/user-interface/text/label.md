---
title: Popisek
description: "Zobrazení textu v Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 84b1a32435d6ce103ca540d7c9db4fce30bc0322
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="label"></a>Popisek

_Zobrazení textu v Xamarin.Forms_

`Label` Zobrazení se používá pro zobrazení textu, jednou či více řádků. Popisky může mít vlastní písem (rodiny, velikosti a možnosti) a barevný text. Tento článek obsahuje následující témata:

- **[Zkrácení a zabalení](#Truncation_and_Wrapping)**  &ndash; zkrácení a zabalení možnosti pro zpracování situacích, kde text nevejdou na jeden řádek.
- **[Písmo](#Font)**  &ndash; možnosti písma.
- **[Barva](#Color)**  &ndash; label text volby barev.
- **[Formátovaný Text](#Formatted_Text)**  &ndash; možnosti pro zobrazení textu s více formátů/styly vložené.

## <a name="styling-label"></a>Styl popisku

Následující části se věnují nastavení vlastnosti `Label` ručně na základě jednotlivých instancí. Všimněte si, že nastaví vlastnosti je možné seskupit do jednoho stylu konzistentně použitý pro jednu nebo více zobrazení. To můžete zvýšit čitelnost kódu a snadněji implementovat změn v návrhu. V tématu [styly](~/xamarin-forms/user-interface/text/styles.md) Další informace.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Zkrácení a zabalení

Popisky lze nastavit pro zpracování text, který se nemůže vejít na jeden řádek v několika způsoby, vystavené `LineBreakMode` vlastnost. [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) je výčet z následujících možností:

- **HeadTruncation** &ndash; zkrátí hlavního textu, zobrazující end.
- **CharacterWrap** &ndash; nezalamuje text na novém řádku na hranici znak.
- **MiddleTruncation** &ndash; zobrazí začátek a konec textu s prostředním nahradit pomocí tři tečky.
- **NoWrap** &ndash; nezalamuje text, zobrazení pouze tolik text jako můžete umístit na jeden řádek.
- **TailTruncation** &ndash; ukazuje na začátek textu, zkrátit end.
- **Zalamování řádků** &ndash; nezalamuje text v hranice slova.

## <a name="font"></a>Písma

V tématu [práce s písem](~/xamarin-forms/user-interface/text/fonts.md) Další informace.

## <a name="color"></a>Barva

`Label`s může být nastaven na použití vlastní barvy prostřednictvím vazbu `TextColor` vlastnost.

Zvláštní pozornost je třeba zajistit, že barvy bude možné použít na každou platformu. Protože každá platforma má jiné výchozí hodnoty pro barvu textu a pozadí, budete muset pečlivě vyberte výchozí, který funguje na všech.

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

V jazyce XAML:

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

![](label-images/textcolor.png "Příklad TextColor popisek")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formátovaný Text

Popisky vystavit `FormattedText` vlastnost, která umožňuje text představovat víc písem a barvy ve stejném zobrazení.

`FormattedText` Vlastnost je typu [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/). Formátované řetězce se skládají z jedné nebo více `Span`s a každý z nich s následujícími vlastnostmi:

- **BackgroundColor** &ndash; můžete použít k nastavení barvu pozadí, třeba k dosažení zvýraznění vliv.
- **FontAttributes** &ndash; může být nastaven na tučné, kurzíva nebo ani jeden z nich.
- **FontFamily** &ndash; nastaví písmo, který se má použít.
- **Velikost písma** &ndash; nastaví velikost textu.
- **ForegroundColor** &ndash; nastaví barvu textu.
- **Text** &ndash; text, který má být uvedené.

Následující kód C# ukazuje štítek, kde je první slovo tučné a poslední slovo je red:

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

V jazyce XAML:

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

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitoly 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Popisek rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
