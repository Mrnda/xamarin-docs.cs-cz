---
title: Ovládací prvek Xamarin.Forms DatePicker
description: Ovládací prvek DatePicker je zobrazení Xamarin.Forms, které umožňuje uživateli vybrat datum. Tento článek vysvětluje, jak využívat prvkem DatePicker v aplikaci Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/04/2018
ms.openlocfilehash: 553957bfa06c7b7a9c5261e426ebee4190de5ebb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994923"
---
# <a name="xamarinforms-datepicker"></a>Ovládací prvek Xamarin.Forms DatePicker

_Zobrazení Xamarin.Forms, která umožňuje uživateli vybrat datum_

Xamarin.Forms [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) vyvolá ovládacího prvku Výběr data platformy a umožňuje uživateli vybrat datum. `DatePicker` definuje vlastnosti osm:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) typu [ `DateTime` ](xref:System.DateTime), která má výchozí hodnotu prvního dne roku 1900.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) typ `DateTime`, které výchozí hodnota je poslední den v roce 2100.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) typu `DateTime`, vybraným datem, kde je použit výchozí hodnotu [ `DateTime.Today` ](xref:System.DateTime.Today).
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) typu `string`, [standardní](/dotnet/standard/base-types/standard-date-and-time-format-strings/) nebo [vlastní](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET formátovací řetězec, výchozí nastavení je "D", long date vzor.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) typu [ `Color` ](xref:Xamarin.Forms.Color), barva použitá k zobrazení vybrané datum, kde je použit výchozí [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) typu [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), která má výchozí hodnotu [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) typu `string`, která má výchozí hodnotu `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) typ `double`, která má výchozí hodnotu-1.0.

`DatePicker` Aktivuje [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) událost, když uživatel vybere datum.

> [!WARNING]
> Při nastavování `MinimumDate` a `MaximumDate`, ujistěte se, že `MinimumDate` je vždy menší než nebo rovna hodnotě `MaximumDate`. V opačném případě `DatePicker` vyvolá výjimku.

Interně `DatePicker` zajišťuje, že `Date` mezi `MinimumDate` a `MaximumDate`včetně. Pokud `MinimumDate` nebo `MaximumDate` nastavená tak, aby `Date` není mezi nimi, `DatePicker` upraví hodnotu `Date`.

Všechny vlastnosti osm využívají [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objekty, které znamená, že může být styl a vlastnosti může být terčem datové vazby. `Date` Vlastnost má režim výchozí vazby [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), což znamená, že může být cílem příkazu datové vazby v aplikaci, která se používá [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektura.

## <a name="initializing-the-datetime-properties"></a>Inicializuje vlastnosti data a času

V kódu, můžete inicializovat `MinimumDate`, `MaximumDate`, a `Date` vlastnosti na hodnoty typu `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Při `DateTime` uvedené v XAML, používá analyzátoru XAML `DateTime.Parse` metodu s `CultureInfo.InvariantCulture` argument pro převod řetězce na `DateTime` hodnotu. Data je nutné zadat přesný formát: dvěma číslicemi měsíců, dnů dvěma číslicemi a čtyřciferné letopočty odděleny lomítky:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Pokud `BindingContext` vlastnost `DatePicker` je nastaven na instanci ViewModel, který obsahuje vlastnosti typu `DateTime` s názvem `MinDate`, `MaxDate`, a `SelectedDate` (například), můžete vytvořit instanci `DatePicker` tímto způsobem :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

V tomto příkladu jsou všechny tři vlastnosti inicializovat na odpovídající vlastnosti v ViewModel. Protože `Date` vlastnost má režim vazby `TwoWay`, nové datum, který uživatel vybere se automaticky projeví v ViewModel.

Pokud `DatePicker` neobsahuje vazby na jeho `Date` vlastnost, aplikace by měla připojit obslužná rutina `DateSelected` událostí bude informovat, když uživatel vybere nové datum.

Informace o nastavení vlastností písma, naleznete v tématu [písma](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>Ovládací prvek DatePicker a rozložení

Je možné použít možnost neomezeným vodorovném rozložení, jako `Center`, `Start`, nebo `End` s `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Ale to nedoporučujeme. V závislosti na nastavení `Format` vybrána vlastnost data můžou vyžadovat jiným zobrazením šířky. Například řetězec formátu "D" způsobí, že se `DateTime` zobrazení dat v dlouhém formátu a "Wednesday, září 12 2018" vyžaduje větší šířka zobrazení než "Pátek, květen 4. 2018". V závislosti na platformě, může způsobit tento rozdíl `DateTime` zobrazení změnit šířku v rozložení, nebo pro zobrazení k oříznutí.

> [!TIP]
> Je nejvhodnější použít výchozí `HorizontalOptions` nastavení `Fill` s `DatePicker`a nepoužívat šířku `Auto` při vložení `DatePicker` v `Grid` buňky.

## <a name="datepicker-in-an-application"></a>Ovládací prvek DatePicker v aplikaci

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) ukázka zahrnuje dvě `DatePicker` zobrazení na stránce. Ty je možné vybrat dvěma kalendářními daty a program vypočítá počet dnů mezi těmito daty. Program nezmění nastavení `MinimumDate` a `MaximumDate` vlastnosti, takže kalendářní data musí být v rozmezí 1900 a 2100.

Tady je soubor XAML:

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

Každý `DatePicker` je přiřazen `Format` vlastnost "D" dlouhého formátu data. Všimněte si také, že `endDatePicker` objekt má vazbu, která cílí na jeho `MinimumDate` vlastnost. Je vybraný zdroj vazby `Date` vlastnost `startDatePicker` objektu. Tím se zajistí, že koncové datum je vždy vyšší nebo rovna počáteční datum. Kromě dva `DatePicker` objekty, `Switch` má popisek "Zahrnují obou dnech celkem".

Dva `DatePicker` zobrazení je k dispozici obslužnými rutinami připojenými k `DateSelected` události a `Switch` obslužná rutina se připojilo k jeho `Toggled` událostí. Tyto obslužné rutiny událostí v souboru kódu na pozadí a aktivovat nový výpočet dny v období mezi dvěma kalendářními daty:

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

Při prvním spuštění ukázky, obě `DatePicker` zobrazení jsou inicializovány na dnešní datum. Následující snímek obrazovky ukazuje program spuštěný v iOS, Android a univerzální platformu Windows:

[![Počet dní mezi daty počátečním](datepicker-images/DaysBetweenDatesStart.png "dní mezi daty spusťte")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Start dní mezi daty")

Klepnutím na některou z `DatePicker` zobrazí vyvolá výběr data platform. Tři platformy implementovat tento výběr data zcela novými způsoby, ale každý přístup je znají uživatelé z této platformy:

[![Vyberte dní mezi daty](datepicker-images/DaysBetweenDatesSelect.png "vyberte dní mezi daty")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "vyberte dní mezi daty")

Po výběru se dvěma kalendářními daty, aplikace zobrazí počet dní mezi tato data:

[![Dní od data výsledek](datepicker-images/DaysBetweenDatesResult.png "dní od data výsledek")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "dní od data výsledků")

## <a name="related-links"></a>Související odkazy

- [Ukázka DaysBetweenDates](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker – rozhraní API](xref:Xamarin.Forms.DatePicker)
