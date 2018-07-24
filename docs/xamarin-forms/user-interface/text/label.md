---
title: Popisek Xamarin.Forms
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms popisek k zobrazení jedné a více řádky textu v aplikacích.
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: b5069381126db0859508480df5596ed5ec85686f
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203033"
---
# <a name="xamarinforms-label"></a>Popisek Xamarin.Forms

_Zobrazení textu v Xamarin.Forms_

[ `Label` ](xref:Xamarin.Forms.Label) Zobrazení se používá pro zobrazení textu i s více řádky. Popisky může mít vlastní písma (rodiny, velikosti a možnosti) a barevný text.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Zkrácení a zabalení

Popisky lze nastavit pro zpracování textu, který se nemůže vejít na jeden řádek v jednom z několika způsobů vystavené `LineBreakMode` vlastnost. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) je výčet s použitím následujících hodnot:

- **HeadTruncation** &ndash; zkrátí hlavní text zobrazující do konce.
- **CharacterWrap** &ndash; zalomí text na nový řádek na hranici znak.
- **MiddleTruncation** &ndash; zobrazí začátku a konci textu, střední nahradit pomocí tři teček.
- **NoWrap** &ndash; nezalamuje text zobrazení pouze tolik text jako může vešel na jeden řádek.
- **TailTruncation** &ndash; ukazuje začátek textu, zkracování do konce.
- **WordWrap** &ndash; zalomí text na hranici slova.

## <a name="fonts"></a>Písma

Další informace o určení písma na `Label`, naleznete v tématu [písma](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="colors"></a>Barvy

`Label`můžete nastavit s vlastní barvy prostřednictvím umožňujících vazbu [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) vlastnost.

Zvláštní pozornost je třeba zajistit, že bude možné použít na jednotlivých platformách barvy. Protože každá platforma má různé výchozí hodnoty pro barvy textu a pozadí, musíte být opatrní a vyberte výchozí hodnotu, která funguje na každém.

Pomocí následujících XAML můžete nastavit barvu textu popisku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
</ContentPage>
```

Ekvivalentní kód jazyka C# je:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

Na následujících snímcích obrazovky zobrazit výsledek nastavení `TextColor` vlastnost:

![](label-images/textcolor.png "Příklad popisku TextColor")

Další informace o barvách najdete v tématu [barvy](~/xamarin-forms/user-interface/colors.md).

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Formátovaný Text

Zobrazit popisky [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) vlastnost, která umožňuje prezentace text s více písem a barev ve stejném zobrazení.

`FormattedText` Vlastnost je typu [ `FormattedString` ](xref:Xamarin.Forms.FormattedString), který se skládá z jednoho nebo více [ `Span` ](xref:Xamarin.Forms.Span) instancí, které jsou nastavené přes [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) vlastnost . Každý `Span` má následující vlastnosti:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – Barva pozadí značky span.
- [`Font`](xref:Xamarin.Forms.Span.Font) – písmo pro text v rozsahu.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – atributy písma pro text v rozsahu.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – rodina písem, ke kterému patří písmo pro text v rozsahu.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – velikost písma textu v rozpětí.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – Barva textu v rozpětí. Tato vlastnost je zastaralá a nahradila ji `TextColor` vlastnost.
- [`Style`](xref:Xamarin.Forms.Span.Style) – styl, který má použít pro značku span.
- [`Text`](xref:Xamarin.Forms.Span.Text) – text značky span.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – Barva textu v rozpětí.

Ukazuje následující příklad XAML `FormattedText` vlastnost, která se skládá ze tří `Span` instancí:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

Ekvivalentní kód jazyka C# je:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> [ `Text` ](xref:Xamarin.Forms.Span.Text) Vlastnost `Span` můžete nastavit prostřednictvím datové vazby. Další informace najdete v tématu [datové vazby](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Na následujících snímcích obrazovky zobrazit výsledek nastavení `FormattedString` vlastnost na tři `Span` instancí:

![](label-images/formattedtext.png "Příklad FormattedText popisek")

## <a name="styling-a-label"></a>Používání stylů pro popisek

Nastavení popsané v předchozích částech [ `Label` ](xref:Xamarin.Forms.Label) vlastnosti na základě jednotlivé instance. Nastaví vlastnosti však mohou být seskupeny do jednoho styl, který konzistentně platí pro jeden nebo více zobrazení. To může zvýšení přehlednosti kódu a změny návrhu usnadnil. Další informace najdete v tématu [styly](~/xamarin-forms/user-interface/text/styles.md).

## <a name="related-links"></a>Související odkazy

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitolu 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Popisek rozhraní API](xref:Xamarin.Forms.Label)
