---
title: Souhrn kapitoly 15. Interaktivní rozhraní
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 15. Interaktivní rozhraní'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6da3753d723ed44ca640d8c80ae07258a03cbbbc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998664"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Souhrn kapitoly 15. Interaktivní rozhraní

Tato kapitola popisuje osm `View` odvozené konfigurace, které umožňují interakci s uživatelem.

## <a name="view-overview"></a>Zobrazit přehled

Xamarin.Forms obsahuje 20 instantiable třídy, které jsou odvozeny z `View` , ale ne `Layout`. Šest těchto popsaná v předchozích kapitol:

- `Label`: [ **Kapitola 2. Anatomie aplikace**](chapter02.md)
- `BoxView`: [ **Kapitolu 3. Posouvání zásobníku**](chapter03.md)
- `Button`: [ **Kapitola 6. Kliknutí na tlačítko**](chapter06.md)
- `Image`: [ **Kapitola 13. Rastrové obrázky**](chapter13.md)
- `ActivityIndicator`: [ **Kapitola 13. Rastrové obrázky**](chapter13.md)
- `ProgressBar`: [ **Kapitola 14. AbsoluteLayout**](chapter14.md)

Osm zobrazení v této kapitole efektivně umožňují uživateli pracovat s základní datové typy .NET:

|Datový typ|Zobrazení|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Tato zobrazení můžete představit jako vizuální reprezentace interaktivní z podkladových datových typů. Tento koncept je v následující kapitole více prozkoumat [ **kapitola 16. Vytváření datových vazeb**](chapter16.md).

Zbývající pohledy šest jsou popsané v následujících kapitolách:

- `WebView`: [ **Kapitola 16. Vytváření datových vazeb**](chapter16.md)
- `Picker`: [ **Kapitole 19. Zobrazení kolekcí**](chapter19.md)
- `ListView`: [ **Kapitole 19. Zobrazení kolekcí**](chapter19.md)
- `TableView`: [ **Kapitole 19. Zobrazení kolekcí**](chapter19.md)
- `Map`: [ **Kapitola 28. Poloha a mapy**](chapter28.md)
- `OpenGLView`: Není v této příručce (a žádná podpora pro platformy Windows)

## <a name="slider-and-stepper"></a>Posuvník a krokovač

Obě [ `Slider` ](xref:Xamarin.Forms.Slider) a [ `Stepper` ](xref:Xamarin.Forms.Stepper) uživateli umožní výběr z různých číselnou hodnotu. `Slider` Je souvislou při `Stepper` zahrnuje jednotlivých hodnot.

### <a name="slider-basics"></a>Základy posuvníku

[ `Slider` ](xref:Xamarin.Forms.Slider) Je vodorovný pruh reprezentující rozsah hodnot z minimálně na levé straně maximum na pravé straně. Definuje tři veřejné vlastnosti:

- [`Value`](xref:Xamarin.Forms.Slider.Value) typ `double`, výchozí hodnota 0
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) typ `double`, výchozí hodnota 0
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) typ `double`, výchozí hodnota 1

S možností vazby vlastnosti, které zálohují tyto vlastnosti ověřte, že jsou konzistentní vzhledem k aplikacím:

- Pro všechny tři vlastnosti [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) zadaná pro vlastnost podporující vazby zajišťuje, že metoda `Value` mezi `Minimum` a `Maximum`.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Metoda `MinimumProperty` vrátí `false` Pokud `Minimum` je nastavena na hodnotu větší než nebo rovna hodnotě `Maximum`a podobně pro `MaximumProperty`. Vrací `false` z `validateValue` metody způsobí, že `ArgumentException` zapříčinil.

`Slider` je aktivována [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) událost s [ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) argument při `Value` změny vlastností buď programově, nebo když uživatel manipuluje se `Slider`.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) ukázce jednoduché použití `Slider`.

### <a name="common-pitfalls"></a>Běžné nástrahy

V kódu i v XAML `Minimum` a `Maximum` jsou nastaveny ve vámi stanoveném pořadí. Je potřeba inicializovat tyto vlastnosti tak, aby `Maximum` je vždy větší než `Minimum`. Jinak bude vyvolána výjimka.

Inicializace `Slider` vlastnosti může způsobit, že `Value` vlastnost změnit a `ValueChanged` událost se aktivuje. Měli byste zajistit, `Slider` obslužná rutina události nemá přístup k zobrazení, která ještě nejsou vytvořené během inicializace stránky.

`ValueChanged` Události neaktivuje během `Slider` inicializace není-li `Value` změny vlastností. Můžete volat `ValueChanged` obslužná rutina přímo z kódu.

### <a name="slider-color-selection"></a>Výběr barvy posuvníku

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) program obsahuje tři `Slider` prvky, které umožňuje interaktivně vybírat barvy tak, že zadáte jeho hodnoty RGB:

[![Trojitá snímek jezdce R G B](images/ch15fg03-small.png "RGB posuvníky")](images/ch15fg03-large.png#lightbox "RGB posuvníky")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) Ukázka používá dvě `Slider` prvků, které mají dvě přesunout `Label` prvky napříč `AbsoluteLayout` a fade ho do druhé.

### <a name="the-stepper-difference"></a>Rozdíl krokovač

[ `Stepper` ](xref:Xamarin.Forms.Stepper) Definuje stejné vlastnosti a události jako `Slider` ale `Maximum` vlastnost je inicializován na 100 a `Stepper` definuje čtvrtý vlastnost:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) typ `double`, inicializována na hodnotu 1

Vizuálně `Stepper` se skládá ze dvou tlačítek označené **&ndash;** a **+**. Stisknutím klávesy **&ndash;** snižuje `Value` podle `Increment` na minimum `Minimum`. Stisknutím klávesy **+** zvyšuje `Value` podle `Increment` maximálně `Maximum`.

To je patrné podle [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) vzorku.

## <a name="switch-and-checkbox"></a>Přepínač a zaškrtávací políčko

[ `Switch` ](xref:Xamarin.Forms.Switch) Umožňuje uživateli určit logickou hodnotu.

### <a name="switch-basics"></a>Základy přepínače

Vizuálně `Switch` se skládá z přepínací tlačítko, které je možné zapnout odhlásit a znovu přihlásit. Třída definuje jednu vlastnost:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) typu `bool`

`Switch` definuje jednu událost:

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) spolu [ `ToggledEventArgs` ](xref:Xamarin.Forms.ToggledEventArgs) objektu vyvolané při `IsToggled` změny vlastností.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) program ukazuje `Switch`.

### <a name="a-traditional-checkbox"></a>Tradiční zaškrtávací políčko

Někteří vývojáři dát přednost více tradiční `CheckBox` k `Switch`. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna obsahuje `CheckBox` třídu odvozenou od `ContentView`. `CheckBox` je implementováno [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) a [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) soubory. `CheckBox` definuje tři vlastnosti (`Text`, `FontSize`, a `IsChecked`) a `CheckedChanged` událostí.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) ukázce to `CheckBox`.

## <a name="typing-text"></a>Zadávání textu

Xamarin.Forms definuje tři zobrazení, které umožní uživateli zadat a upravit text:

- [`Entry`](xref:Xamarin.Forms.Entry) pro jeden řádek textu
- [`Editor`](xref:Xamarin.Forms.Editor) pro více řádků textu
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) pro jeden řádek textu pro účely vyhledávání.

`Entry` a `Editor` odvozovat [ `InputView` ](xref:Xamarin.Forms.InputView), která je odvozena z `View`. `SearchBar` je odvozena přímo z `View`.

### <a name="keyboard-and-focus"></a>Klávesnice a zaměření

Na telefonech a tabletech bez fyzickou klávesnicích `Entry`, `Editor`, a `SearchBar` všechny elementy způsobovat virtuální klávesnice objevil. Přítomnost této klávesnice na obrazovce se vztahuje na vstupní fokus. Zobrazení musí obsahovat jeho [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) a [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) nastaveny `true` zobrazíte vstupní fokus.

Dvě metody, jednu vlastnost jen pro čtení a dvě události souvisejících s vstupní fokus. Ty jsou definovány `VisualElement`:

- [ `Focus` ](xref:Xamarin.Forms.VisualElement.Focus) Metoda pokusí se nastavit fokus na prvek a vrátí `true` v případě úspěšného ověření
- [ `Unfocus` ](xref:Xamarin.Forms.VisualElement.Unfocus) Metoda odstraní zaměření pro vstup z elementu
- [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) Vlastnost jen pro čtení znamená, pokud prvek má vstupní fokus
- [ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused) Událost označuje, pokud prvek získá vstupní fokus.
- [ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused) Událost označuje, kdy prvek ztratí vstupní fokus.

### <a name="choosing-the-keyboard"></a>Výběr klávesnice

[ `InputView` ](xref:Xamarin.Forms.InputView) Třídu, ze které `Entry` a `Editor` odvodit definuje pouze jednu vlastnost:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) typu [`Keyboard`](xref:Xamarin.Forms.Keyboard)

Označuje typ klávesnice, který se zobrazí. Některé klávesnice jsou optimalizované pro identifikátory URI nebo čísla.

`Keyboard` Třída umožňuje definovat klávesnice s statickou [ `Keyboard.Create` ](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) metoda s argumentem typu [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags), výčet s následující bitové příznaky:

- `None` Nastavte na hodnotu 0
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) Nastavte na 1
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) nastaven na hodnotu 2
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) Nastavte na 4
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) Nastavte na \xFFFFFFFF

Při použití víceřádkového [ `Editor` ](xref:Xamarin.Forms.Editor) odstavce nebo více textu se očekával, volání `Keyboard.Create` je dobrý přístup k výběru klávesnice. Pro jeden řádek [ `Entry` ](xref:Xamarin.Forms.Entry), následující statické jen pro čtení vlastnosti `Keyboard` jsou užitečné:

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) pro kladná čísla s nebo bez něj desetinné čárky.

[ `KeyboardTypeConverter` ](xref:Xamarin.Forms.KeyboardTypeConverter) Umožňuje zadat tyto vlastnosti v XAML, jak je uvedeno ve [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) programu.

### <a name="entry-properties-and-events"></a>Položka vlastnosti a události.

Jeden řádek [ `Entry` ](xref:Xamarin.Forms.Entry) definuje následující vlastnosti:

- [`Text`](xref:Xamarin.Forms.Entry.Text) typ `string`, text, který se zobrazuje `Entry`
- [`TextColor`](xref:Xamarin.Forms.Entry.TextColor) typu `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) typu `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) typu `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) typu `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) typ `bool`, což způsobí, že znaků, které mají být zakryté hvězdičkami
- [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) typu `string`, pro dimly barevný text, který se zobrazí `Entry` předtím, než vše, co zadáte
- [`PlaceholderColor`](xref:Xamarin.Forms.Entry.PlaceholderColor) typu `Color`

`Entry` Definuje také dvě události:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) s [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) objektu, aktivuje vždy, když `Text` změny vlastností
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), že uživatel dokončil a klávesnice je zrušená. Uživatel označuje dokončení způsobem specifické pro platformu

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) ukázka demonstruje tyto dvě události.

### <a name="the-editor-difference"></a>Rozdíl editoru

Víceřádkového [ `Editor` ](xref:Xamarin.Forms.Editor) definuje stejné `Text` a `Font` vlastnosti jako `Entry` , ale nikoli jiné vlastnosti. `Editor` Definuje také dvě stejné vlastnosti jako `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) je program pořizování poznámek volného tvaru, který se uloží a obnoví obsah `Editor`.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Není odvozen od `InputView`, takže se nemusí `Keyboard` vlastnost. Mají všechny, ale `Text`, `Font`, a `Placeholder` vlastnosti, která `Entry` definuje. Kromě toho `SearchBar` definuje tři další vlastnosti:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) typu `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) typu [ `ICommand` ](xref:System.Windows.Input.ICommand) pro použití s datové vazby a rozhraní MVVM
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) typ `Object`, pro použití se službou `SearchCommand`

Konkrétní platformy zrušit tlačítko použita rychlá vymazání textu. `SearchBar` Také obsahuje tlačítko hledání pro konkrétní platformu. Stisknutím jednu z těchto tlačítek vyvolá jednu z těchto dvou událostí, který `SearchBar` definuje:

- [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged) spolu [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) objektu
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) příklad ukazuje, `SearchBar`.

## <a name="date-and-time-selection"></a>Výběr data a času

[ `DatePicker` ](xref:Xamarin.Forms.DatePicker) a [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) zobrazení implementaci ovládacích prvků pro konkrétní platformu, které uživateli umožňují určit datum nebo čas.

### <a name="the-datepicker"></a>Ovládací prvek DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) definuje čtyři vlastnosti:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) typ `DateTime`, inicializována na hodnotu 1. ledna 1900
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) typ `DateTime`, inicializované do 31. prosince 2100
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) typ `DateTime`, inicializována na hodnotu `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) typ `string`, .NET formátovací řetězec inicializován na "d", vzor krátkého formátu data, výsledkem je zobrazení data, jako je "7/20/1969" v USA.

Můžete nastavit `DateTime` vlastnosti v XAML vyjádření vlastnosti storyboards a použitím krátké datum invariantní jazykovou verzi formátu ("7/20/1969").   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) ukázkové vypočítá počet dnů mezi dvěma kalendářními daty vybraný uživatelem.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (nebo se jedná TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) definuje dvě vlastnosti a žádné události:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) je typu `TimeSpan` spíše než `DateTime`, určující čas uplynulých od půlnoci
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) typ `string`, .NET formátovací řetězec inicializován na "t", vzor krátkého formátu času, což vede k zobrazení času jako "1:45 PM" v USA.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) program ukazuje způsob použití `TimePicker` jak určit čas pro časovače. Program funguje jenom v případě zachovat v popředí.

**SetTimer** také ukazuje použití [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) metoda `Page` zobrazit upozornění.



## <a name="related-links"></a>Související odkazy

- [Kapitola 15 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Ukázky kapitole 15](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Položka](~/xamarin-forms/user-interface/text/entry.md)
- [Editor](~/xamarin-forms/user-interface/text/editor.md)
