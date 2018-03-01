---
title: "Výběr."
description: "Tento průvodce popisuje navrhování a práce s ovládacích prvků výběr v aplikaci pro Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: 402b17ddbb28fb8896ad0f158fe8dbcd17689f40
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="picker"></a>Výběr.

Ovládací prvek výběru zobrazí wheel jako ovládací prvek obsahující posuvného seznam hodnot s se zvýrazněnou vybrané hodnoty. Uživatelé otočit kolečka a vyberte možnost, který jim vyhovuje.

Jeden konkrétní uživatel případu pro výběr ji nastavit datum nebo čas. Zajistit pro toto Apple vytvořil vlastní podtřídou třídy UIPickerView názvem UIDatePicker.

Článek popisuje implementace a pomocí [výběr](#picker) a [výběr data](#datepicker) ovládací prvky.

<a name="picker">

## <a name="picker"></a>Výběr.

### <a name="implementing-a-picker"></a>Implementace výběr.

Výběr je implementováno po vytvoření nové instance [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>Výběr a scénářů

Pokud používáte iOS Designer pro vytvoření uživatelské rozhraní, nástroje pro výběr lze přidat na váš rozložení z panelu nástrojů:

![](picker-images/image1.png)


### <a name="working-with-picker"></a>Práce s výběr.

Jakmile jste vytvořili pro výběr, zda v kódu nebo prostřednictvím scénářů, budete muset přiřadit _modelu_ do něj, aby mohli předat a interagovat s daty;

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

Následující kód ukazuje příklad modelu:

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {   
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

Nejprve budete muset předat některé data pro zajištění různé možnosti pro uživatele k výběru. Při pokusu o možné ponechat tento seznam co nejkratší, nebo pokud potřeby zkuste použít více než jeden 'vytočit. (názvem *součásti*):

![Výběr s dvě součásti](picker-images/image3.png)

Chcete-li nastavit počet součástí, přepište `GetComponentCount` metoda: 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

Návratová hodnota označuje počet vytáčení, který bude mít vaše výběr.

### <a name="customizing-appearance"></a>Přizpůsobení vzhledu
 
Vzhled `UIPickerView` lze přizpůsobit pomocí `UIPickerView.UIPickerViewAppearance` třídy nebo pomocí přepsání `UIPickerViewModel.GetView` a `UIPickerViewModel.GetRowHeight` metody v `UIPickerViewModel`.


<a name="datepicker">

## <a name="date-picker"></a>Výběr data

### <a name="implementing-a-date-picker"></a>Implementace výběr data

Výběr data je implementováno po vytvoření nové instance [ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>Výběr data a scénářů

Pokud používáte iOS návrháře k vytvoření vašeho uživatelského rozhraní **výběr data** mohou být přidány do vaší rozložení z panelu nástrojů. Z panelu pro vlastnosti lze upravit následující vlastnosti:

![](picker-images/image2.png)

* **Režim** – datum a čas režimu. To může být datum, čas, datum a čas nebo časovač odpočítávání. 
* **Národní prostředí** – národního prostředí pro výběr data. Zvolte **výchozí** nastavenou na výchozí systém nebo nastaven jakékoli konkrétní prostředí.
* **Interval** – zobrazuje přírůstek, ve kterém se zobrazí možnosti hodiny.
* **Datum, datum minimální, maximální** – nastaví počáteční datum, které se zobrazí výběr a omezení pro volitelný kalendářní data.

### <a name="configuring-the-datepicker"></a>Konfigurace ovládací prvek DatePicker

Můžete omezit rozsah dat, který může uživatel vybrat z pomocí `MinimumDate` a `MaximumDate` vlastnosti. Následující fragment kódu ukazuje příklad toho, jak nastavit v rozsahu 60 let před a informací o dnešku:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

Alternativně můžete použít taky rozhraní .NET ovládací prvky nastavit minimální a maximální období. Příklad:

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

Můžete také nastavit `MinuteInterval` vlastnost nastavte interval, ve kterém se zobrazí nástroje pro výběr minut. Následující fragment kódu slouží k nastavení modulu pro výběr být nastavena v intervalech 10 minut.

```csharp
datePickerView.MinuteInterval = 10;
```

Existují čtyři režimy, které výběr data lze nastavit pomocí [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/) vlastnost. Seznamu níže ukazuje příklad každé z nich a jak ho implementovat:

#### <a name="time"></a>Čas

Režim doby Zobrazí dobu s selektoru hodin a minut a volitelné označení dop. Je nastavena `UIDatePickerMode.Time` vlastnost. Příklad:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

Následující obrázek ukazuje příklad tento ovládací prvek DatePicker režim:

![](picker-images/image8.png)



#### <a name="date"></a>Datum

Režim data zobrazuje datum, měsíc, den a selektor roku. Je nastavena `UIDatePickerMode.Date` vlastnost. Příklad:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

Následující obrázek ukazuje příklad tento ovládací prvek DatePicker:

![](picker-images/image7.png)

Pořadí na selektory závisí na národním prostředí `UIDatePicker`. Ve výchozím nastavení to se nastaví na výchozí systém. Na obrázku výše vidíte rozložení selektory v `en_US` národním prostředí, ale chcete ho změnit na den | Měsíc | Rozložení roku, můžete použít následující kód k nastavení národního prostředí:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>Datum a čas

Datum a čas režimu zobrazí zobrazení shortend datum, čas v hodinách a minutách a volitelné dop označení dependings na, pokud se používá 12 nebo 24 hodin. Je nastavena `UIDatePickerMode.DateAndTime` vlastnost. Příklad:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

Následující obrázek ukazuje příklad tento ovládací prvek DatePicker:

![](picker-images/image6.png)

Stejně jako u [datum](#Date), pořadí na selektory a použitím 12 nebo 24 hodin, závisí na národním prostředí `UIDatePicker`.

#### <a name="countdown-timer"></a>Časovač odpočítávání

Režim časovač odpočítávání Zobrazí hodinu a minutu hodnoty. Je nastavena `UIDatePickerMode.CountDownTimer` vlastnost. Příklad:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

Následující obrázek ukazuje příklad tento ovládací prvek DatePicker:

![](picker-images/image5.png)

Můžete použít `CountDownDuration` vlastnost k zachycení dispayed hodnotu pomocí nástroje pro výběr data odpočítávání. Například pokud chcete přidat hodnota odpočítávání na aktuální datum, můžete použít následující kód:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>Formátování 

Hodnoty režimů čas, datum a DateAndTime se dají zachytit pomocí `Date` vlastnost UIDatePicker (například: `datePickerView.Date`), která je typu NSDate. K formátování toto datum do něco další číselné vyjádření, použijte [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/). Následující příklady ukazují, jak používat některé z dostupných vlastností na této třídě.

`DateFormat` Nastavena jako řetězec představující, jak mají být zobrazeny datum:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

`TimeStyle` Sady vlastností `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Pole pro `NSDateFormatterStyle` zobrazení takto:

![](picker-images/timestyle.png)

`DateStyle` Sady vlastností `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Pole pro `NSDateFormatterStyle` zobrazení takto:

![](picker-images/datestyle.png)

Výstup naformátovaný NSDate na řetězec, můžete pak pomocí následujícího kódu:

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>Související odkazy

- [PickerControl (ukázka)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
