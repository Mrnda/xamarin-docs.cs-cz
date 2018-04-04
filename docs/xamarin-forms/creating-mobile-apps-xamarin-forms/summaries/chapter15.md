---
title: Shrnutí kapitoly 15. Interaktivní rozhraní
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c5b2bc00c4337969322193966f26ce0e151f426e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Shrnutí kapitoly 15. Interaktivní rozhraní

Tato kapitola prozkoumá osm `View` odvozené konfigurace, které umožňují interakci s uživatelem.

## <a name="view-overview"></a>Zobrazit přehled

Xamarin.Forms obsahuje 20 instantiable třídy, které jsou odvozeny od `View` ale ne `Layout`. Šest z těchto popsaná v předchozích kapitol:

- `Label`: [ **Kapitoly 2. Anatomie aplikace**](chapter02.md)
- `BoxView`: [ **Kapitoly 3. Procházení zásobníku**](chapter03.md)
- `Button`: [ **Kapitoly 6. Kliknutí na tlačítko**](chapter06.md)
- `Image`: [ **Kapitoly 13. Rastrové obrázky**](chapter13.md)
- `ActivityIndicator`: [ **Kapitoly 13. Rastrové obrázky**](chapter13.md)
- `ProgressBar`: [ **Kapitoly 14. AbsoluteLayout**](chapter14.md)

Osm zobrazení v této kapitole efektivně povolit uživatelům interakci s základní datové typy .NET:

|Datový typ|zobrazení|
|--- |--- |
|`Double`|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/), [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|
|`Boolean`|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|
|`String`|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/), [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|
|`DateTime`|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/), [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|

Tato zobrazení si můžete představit jako interaktivní vizuální reprezentace základní datové typy. Tento koncept je více prozkoumali v další kapitoly [ **kapitoly 16. Datová vazba**](chapter16.md).

Zbývající šesti zobrazení jsou popsané v následujících kapitolách:

- `WebView`: [ **Kapitoly 16. Datová vazba**](chapter16.md)
- `Picker`: [ **Kapitoly 19. Zobrazení kolekce**](chapter19.md)
- `ListView`: [ **Kapitoly 19. Zobrazení kolekce**](chapter19.md)
- `TableView`: [ **Kapitoly 19. Zobrazení kolekce**](chapter19.md)
- `Map`: [ **Kapitoly 28. Umístění a mapy**](chapter28.md)
- `OpenGLView`: Není popsaná v této příručce (a žádná podpora pro platformy systému Windows)

## <a name="slider-and-stepper"></a>Posuvník a krokovač

Obě [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) a [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) umožnit uživatelům zvolit číselná hodnota z rozsahu. `Slider` Je nepřetržitá rozsah při `Stepper` zahrnuje diskrétními hodnotami.

### <a name="slider-basics"></a>Základy posuvníku

[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) Je vodorovných panelu reprezentující rozsah hodnot z minimálně na levé straně na maximální na pravé straně. Definuje tři veřejné vlastnosti:

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) typu `double`, výchozí hodnota 0
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) typu `double`, výchozí hodnota 0
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) typu `double`, výchozí hodnota 1

Vazbu vlastnosti, které zpět tyto vlastnosti Ujistěte se, že jsou konzistentní:

- Pro všechny tři vlastnosti [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) zadaná pro vlastnost vazbu zajišťuje, že metoda `Value` mezi `Minimum` a `Maximum`.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Metodu `MinimumProperty` vrátí `false` Pokud `Minimum` je nastavena na hodnotu větší než nebo rovna hodnotě `Maximum`a podobné pro `MaximumProperty`. Vrácení `false` z `validateValue` metody způsobí, že `ArgumentException` má být aktivována.

`Slider` Aktivuje se [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) událost s [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) argument při `Value` změny vlastností buď programově, nebo když uživatel manipuluje `Slider`.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) příklad znázorňuje jednoduché použití `Slider`.

### <a name="common-pitfalls"></a>Běžné nástrahy

V kódu i v jazyce XAML `Minimum` a `Maximum` vlastnosti jsou nastaveny v zadaném pořadí. Ujistěte se, k chybě při inicializaci tyto vlastnosti tak, aby `Maximum` je vždy větší než `Minimum`. V opačném případě bude vyvolána výjimka.

Inicializace `Slider` může způsobit vlastnosti `Value` vlastnosti chcete změnit a `ValueChanged` událost, která má být aktivována. Ujistěte se, že `Slider` obslužné rutiny události nemá přístup k zobrazení, která ještě nejsou vytvořené během inicializace.

`ValueChanged` Událostí není fire během `Slider` inicializace Pokud `Value` změny vlastností. Můžete volat `ValueChanged` obslužná rutina přímo z kódu.

### <a name="slider-color-selection"></a>Výběr barvy posuvníku

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) program obsahuje tři `Slider` elementy, které vám umožní interaktivní výběr barvy zadáním jeho hodnoty RGB:

[![Trojitá snímek obrazovky R G B posuvníky](images/ch15fg03-small.png "RGB posuvníky")](images/ch15fg03-large.png#lightbox "RGB posuvníky")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) Ukázka používá dva `Slider` elementy přesunout dva `Label` elementy napříč `AbsoluteLayout` a vykreslit ho do druhé.

### <a name="the-stepper-difference"></a>Rozdíl krokovač

[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Definuje stejné vlastnosti a události jako `Slider` ale `Maximum` vlastnost inicializovala na 100 a `Stepper` definuje čtvrtý vlastnost:

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) typu `double`, inicializovaného 1

Vizuální `Stepper` se skládá ze dvou tlačítka označená **&ndash;** a **+**. Stisknutím **&ndash;** snižuje `Value` podle `Increment` minimálně na `Minimum`. Stisknutím **+** zvyšuje `Value` podle `Increment` maximálně `Maximum`.

Tento postup je znázorněn pomocí [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) ukázka.

## <a name="switch-and-checkbox"></a>Přepínač a zaškrtávací políčko

[ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) Umožňuje uživateli zadat logickou hodnotu.

### <a name="switch-basics"></a>Základy přepínače

Vizuální `Switch` se skládá z přepínač, který je možné zapnout a zapněte. Třída definuje jednu vlastnost:

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) typu `bool`

`Switch` definuje jednu událost:

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) spolu [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/) objektu, když je aktivována `IsToggled` změny vlastností.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) program ukazuje `Switch`.

### <a name="a-traditional-checkbox"></a>Tradiční zaškrtávací políčko

Někteří vývojáři preferovat více tradiční `CheckBox` k `Switch`. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje `CheckBox` třídu odvozenou od `ContentView`. `CheckBox` je implementované [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) a [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) soubory. `CheckBox` definuje vlastnosti tři (`Text`, `FontSize`, a `IsChecked`) a `CheckedChanged` událostí.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) příklad znázorňuje to `CheckBox`.

## <a name="typing-text"></a>Zadávání textu

Xamarin.Forms definuje tři zobrazení, které umožňují uživateli zadat a upravit text:

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) pro jeden řádek textu
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) pro více řádků textu
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) pro jeden řádek textu pro účely vyhledávání.

`Entry` a `Editor` odvozena od [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/), která je odvozena z `View`. `SearchBar` Odvozená přímo z `View`.

### <a name="keyboard-and-focus"></a>Klávesnice a zaměřit

Z telefonů a tabletů bez fyzické klávesnice `Entry`, `Editor`, a `SearchBar` způsobit, že všechny elementy virtuální klávesnici objevil. Přítomnost této klávesnice na obrazovce se týká vstupní fokus. Zobrazení musí mít obě jeho [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) a [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) vlastnosti nastavit na `true` získat zaměření pro vstup.

Dvě metody, jednu vlastnost jen pro čtení a dvě události se podílejí s zaměření pro vstup. Tyto jsou všechny definované `VisualElement`:

- [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/) Metoda pokusí se nastavit zaměření pro vstup na prvek a vrátí `true` v případě úspěchu
- [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/) Metoda odebere element vstupního fokusu
- [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) Vlastnost určenou jen pro čtení označuje, pokud má element vstupu fokusu
- [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/) Událost označuje, pokud element získá vstupního fokusu
- [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/) Událost označuje, pokud element ztratí zaměření pro vstup

### <a name="choosing-the-keyboard"></a>Výběr klávesnice

[ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) Třídu, ze které `Entry` a `Editor` odvozena definuje pouze jednu vlastnost:

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) typu [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

To znamená typ klávesnice, který se zobrazí. Některé klávesnice jsou optimalizované pro identifikátory URI nebo čísla.

`Keyboard` Třída umožňuje definovat klávesnice s statického [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/) metoda s parametrem typu [ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/), výčet s následující bitové příznaky:

- `None` Nastavte na hodnotu 0
- [`CapitalizeSentence`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.CapitalizeSentence/) nastavena na hodnotu 1
- [`Spellcheck`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Spellcheck/) nastaven na hodnotu 2
- [`Suggestions`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Suggestions/) Nastavte na 4
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.All/) Nastavte na \xFFFFFFFF

Při použití víceřádkových [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) při odstavec nebo více textu je očekávané, volání `Keyboard.Create` je dobré přístupem při výběru klávesnici. Pro jeden řádek [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), následující statické jen pro čtení vlastnosti `Keyboard` jsou užitečné:

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) pro kladná čísla s nebo bez desetinné čárky.

[ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/) Umožňuje zadání tyto vlastnosti v jazyce XAML, jak je předvedeno pomocí [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) programu.

### <a name="entry-properties-and-events"></a>Položka vlastnosti a události

Jeden řádek [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) definuje následující vlastnosti:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) typu `string`, text, který se zobrazí v `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) typu `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) typu `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) typu `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) typu `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) typu `bool`, což způsobí, že znaky maskování
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) typu `string`, pro dimly barevný text, který se zobrazí v `Entry` předtím, než je vše, co zadáte
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) typu `Color`

`Entry` Také definuje dvě události:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) s [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) objekt, vždy, když je aktivována `Text` změny vlastností
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/), aktivována, jestliže uživatel dokončení a klávesnice se zavře. Uživatel označuje dokončení způsobem specifické pro platformu

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) příklad znázorňuje tyto dvě události.

### <a name="the-editor-difference"></a>Rozdíl editoru

Víceřádkových [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) definuje stejné `Text` a `Font` vlastnosti jako `Entry` , ale není ostatní vlastnosti. `Editor` Definuje také stejné dvě vlastnosti jako `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) je program poznámky k pořízení volného formátu, který uloží a obnoví obsah `Editor`.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Není odvozena od `InputView`, takže nemá `Keyboard` vlastnost. Mají všechny, ale `Text`, `Font`, a `Placeholder` vlastnosti, `Entry` definuje. Kromě toho `SearchBar` definuje tři další vlastnosti:

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) typu `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) typu [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) pro použití s datové vazby a rozhraní MVVM
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) typu `Object`, pro použití s `SearchCommand`

Specifické platformy zrušit tlačítko vymazáním text. `SearchBar` Má také tlačítko Hledat specifické pro platformu. Kliknutím na některou z těchto tlačítek vyvolá jednu z těchto dvou událostí, `SearchBar` definuje:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) spolu [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) objektu
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) příklad ukazuje `SearchBar`.

## <a name="date-and-time-selection"></a>Výběr data a času

[ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) a [ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) zobrazení implementovat kontroly specifických pro platformy, které umožní uživateli zadat datum nebo čas.

### <a name="the-datepicker"></a>Ovládací prvek DatePicker

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) definuje čtyři vlastnosti:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) typu `DateTime`, inicializované do 1. ledna 1900
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) typu `DateTime`, inicializované do 31. prosince 2100
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) typu `DateTime`, inicializované do `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) typu `string`, .NET formátování řetězce inicializována tak, aby "d", vzoru krátkého data, což vede k zobrazení data, jako je "7/20/1969" v USA.

Můžete nastavit `DateTime` vlastnosti v jazyce XAML vyjadřující vlastnosti jako vlastnosti elementy a použitím krátkého data neutrální jazykovou verzi formátu ("7/20/1969").   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) ukázka vypočítá počet dní mezi dvěma kalendářními vybraný uživatelem.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (nebo je TimeSpanPicker?)

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) definuje dvě vlastnosti a žádné události:

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) je typu `TimeSpan` místo `DateTime`, která určuje čas uběhlých od půlnoci
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) typu `string`, .NET formátování řetězce inicializována tak, aby "t", vzoru krátkého času, což vede k zobrazení času jako "1:45 odp" v USA.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) program ukazuje, jak používat `TimePicker` chcete zadat čas pro časovač. Program funguje jenom v případě byste mít na popředí.

**SetTimer** také ukazuje, jak pomocí [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) metodu `Page` zobrazit upozornění.



## <a name="related-links"></a>Související odkazy

- [Úplný text 15 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Ukázky kapitoly 15](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Položka](~/xamarin-forms/user-interface/text/entry.md)
- [Editor](~/xamarin-forms/user-interface/text/editor.md)
