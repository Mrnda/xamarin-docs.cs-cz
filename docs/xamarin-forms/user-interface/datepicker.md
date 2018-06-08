---
title: Pomocí ovládací prvek DatePicker
description: Zobrazení Xamarin.Forms, která umožňuje uživateli vybrat datum
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/04/2018
ms.openlocfilehash: 09b0bd788d9ac436e0270b447556ad2b0a848f99
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848561"
---
# <a name="using-datepicker"></a>Pomocí ovládací prvek DatePicker

_Zobrazení Xamarin.Forms, která umožňuje uživateli vybrat datum_

Platformě Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) vyvolá ovládací prvek platformu pro výběr data a umožňuje uživateli vybrat datum. `DatePicker` definuje vlastnosti osm:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) typu [ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/), což výchozí nastavení je první den v roce 1900.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) typu `DateTime`, které výchozí hodnota je poslední den v roce 2100.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) typu `DateTime`, vybraným datem, což je výchozí hodnotou je hodnota [ `DateTime.Today` ](https://developer.xamarin.com/api/property/System.DateTime.Today/).
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) typu `string`, [standardní](/dotnet/standard/base-types/standard-date-and-time-format-strings/) nebo [vlastní](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET formátování řetězce, který se standardně "D", datum na dlouhém vzor.
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) typu [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/), barvu použitou k zobrazení vybraným datem, který se standardně [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) typu [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), což výchozí nastavení [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) typu `string`, což výchozí nastavení `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) typu `double`, což výchozí nastavení-1.0.

`DatePicker` Aktivuje [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) událost, když uživatel vybere datum.

> [!WARNING]
> Při nastavování `MinimumDate` a `MaximumDate`, ujistěte se, že `MinimumDate` je vždy menší než nebo rovna hodnotě `MaximumDate`. V opačném `DatePicker` vyvolá výjimku.

Interně `DatePicker` zajistí, že `Date` mezi `MinimumDate` a `MaximumDate`(včetně). Pokud `MinimumDate` nebo `MaximumDate` je nastaven tak, aby `Date` není mezi nimi, `DatePicker` upraví hodnotu `Date`.

Všechny vlastnosti osm jsou zajišťované [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) objekty, které znamená, že může být ve vlastnostech lze cíle datové vazby. `Date` Vlastnost má režim výchozí vazby [ `BindingMode.TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/), což znamená, že může být cílem datové vazby v aplikaci, která používá [Model-View-ViewModel (modelem MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektura.

## <a name="initializing-the-datetime-properties"></a>Inicializace vlastností data a času

V kódu můžete inicializovat `MinimumDate`, `MaximumDate`, a `Date` vlastnosti hodnoty typu `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Při `DateTime` je zadaná hodnota v jazyce XAML, používá analyzátor XAML `DateTime.Parse` metoda s `CultureInfo.InvariantCulture` argument převést řetězec, který má `DateTime` hodnotu. Kalendářní data musí být zadán v přesné formátu: letopočty měsíců, dnů letopočty a čtyřciferné letopočty oddělených lomítka:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Pokud `BindingContext` vlastnost `DatePicker` je nastaven na instanci ViewModel, obsahující vlastnosti typu `DateTime` s názvem `MinDate`, `MaxDate`, a `SelectedDate` (například) můžete vytvořit instanci `DatePicker` podobné výjimky :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

V tomto příkladu jsou všechny tři vlastnosti inicializovány na odpovídající vlastnosti v ViewModel. Protože `Date` vlastnost má vazba režim `TwoWay`, všechny nové datum, které uživatel vybere automaticky promítnuta ViewModel.

Pokud `DatePicker` neobsahuje vazby na jeho `Date` vlastnost, aplikace by měl připojit obslužnou rutinu do `DateSelected` událost, která má být informováni, když uživatel vybere nové datum.

Informace o nastavení písma vlastnostech najdete v tématu [písem](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>Ovládací prvek DatePicker a rozložení

Je možné použít možnost neomezeným vodorovném rozložení, jako je `Center`, `Start`, nebo `End` s `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Ale toto se nedoporučuje. V závislosti na nastavení jazyka `Format` vybrané vlastnosti kalendářních dat může vyžadovat jiným zobrazením šířky. Například způsobí, že řetězec formátu "D" `DateTime` zobrazení dat ve formátu dlouhé a "Středa, 12 září 2018" vyžaduje větší šířka zobrazení než "Pátek, 4 může 2018". V závislosti na platformě, může dojít k tomuto rozdílu `DateTime` zobrazení změnit šířku v rozložení, nebo pro displej tak, aby se zkrátila.

> [!TIP]
> Je nejvhodnější použít výchozí `HorizontalOptions` nastavení `Fill` s `DatePicker`a nepoužívat šířka `Auto` při uvedení `DatePicker` v `Grid` buňky.

## <a name="datepicker-in-an-application"></a>Ovládací prvek DatePicker v aplikaci

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) ukázka zahrnuje dvě `DatePicker` zobrazení na stránku s jeho. Ty lze vybrat dvě dat a program vypočítá počet dní mezi tato data. Program nemění nastavení `MinimumDate` a `MaximumDate` vlastnosti, takže kalendářní data musí být mezi 1900 a 2100.

Tady je souboru XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Každý `DatePicker` je přiřazen `Format` vlastnost "D" pro formát dlouhého data. Všimněte si také, že `endDatePicker` objekt má vazbu, která je cílena jeho `MinimumDate` vlastnost. Je vybraný zdroj vazby `Date` vlastnost `startDatePicker` objektu. Tím se zajistí, že koncové datum je vždy později než nebo rovna počáteční datum. Kromě dva `DatePicker` objekty, `Switch` označením "Zahrnout i dny celkem".

Dva `DatePicker` zobrazení mají obslužné rutiny, které jsou připojené k `DateSelected` události a `Switch` má obslužná rutina připojené k jeho `Toggled` událostí. Tyto obslužné rutiny událostí v souboru kódu na pozadí a aktivovat nový výpočet dní mezi dvěma kalendářními:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

Pokud ukázce je nejprve spuštěn, obě `DatePicker` zobrazení jsou inicializovány pro dnešní datum. Následující snímek obrazovky ukazuje programy spuštěné na iOS, Android a univerzální platformu Windows:

[![Spustit dní mezi daty](datepicker-images/DaysBetweenDatesStart.png "dní mezi daty spustit")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "spustit dní mezi daty")

Některý z klepnutím `DatePicker` zobrazí vyvolá výběr platformy data. Tři platformy implementovat tento výběr data velmi různými způsoby, ale každý přístup je pro uživatele této platformě:

[![Vyberte dní mezi daty](datepicker-images/DaysBetweenDatesSelect.png "dní mezi daty vyberte")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "vyberte dní mezi daty")

Jakmile jsou vybrané dva kalendářní data, aplikace zobrazí počet dní mezi tato data:

[![Dní mezi daty výsledek](datepicker-images/DaysBetweenDatesResult.png "dní mezi daty výsledek")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "dní mezi daty výsledků")

## <a name="related-links"></a>Související odkazy

- [Ukázka DaysBetweenDates](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [Ovládací prvek DatePicker rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
