---
title: Souhrn kapitoly 16. Datová vazba
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0f200e0c482402813ac7051255dd7c27da93d6dc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-16-data-binding"></a>Souhrn kapitoly 16. Datová vazba

Programátory v jazyce často najít sami zápis obslužné rutiny, které rozpoznat, kdy došlo ke změně vlastností jeden objekt a použijte ke změně hodnoty vlastnosti v jiném objektu. Tento proces je možné automatizovat pomocí technika *datová vazba*. Datové vazby jsou obvykle definovány v jazyce XAML a stanou se součástí definice uživatelského rozhraní.

Tyto datové vazby velmi často připojit objektů uživatelského rozhraní na základní data. Toto je postup, který je ve více prozkoumali [ **kapitoly 18. MVVM**](chapter18.md). Datové vazby však může připojit i dvě nebo více elementům uživatelského rozhraní. Většina příkladů časné vazby dat v této kapitole ukazují tento postup.

## <a name="binding-basics"></a>Základy vazby

Datová vazba podílí několik vlastností, metod a třídy:

- [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) Třída odvozená z [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) a zapouzdří mnoho vlastností datová vazba
- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Vlastnost je definována [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) – třída
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Metoda je také definován ve [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) – třída
- [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) Třída definuje tři další `SetBinding` metody

Následující dvě třídy podporují XAML – rozšíření značek pro vazby:

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) podporuje `Binding` – rozšíření značek
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) podporuje `x:Reference` – rozšíření značek

Datová vazba podílí dvě rozhraní:

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) v `System.ComponentModel` je obor názvů pro implementaci oznámení při změně vlastnosti
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) se používá k definování malé třídy, které převést hodnoty z jednoho typu do druhého v datové vazby

Datová vazba připojí dvě vlastnosti stejného objektu nebo (běžně) dva různé objekty. Tyto dvě vlastnosti se označují jako *zdroj* a *cíl*. Obecně platí změny ve zdrojové vlastnosti způsobí změnu dochází k výskytu vlastnost target, ale někdy je směr obrácený. Bez ohledu na to:

- *cíl* musí být vlastnost založenou [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- *zdroj* vlastnost obvykle se jedná o člena třídy, která implementuje [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

Třídu, která implementuje `INotifyPropertyChanged` aktivuje [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) události při změně hodnoty vlastnosti. `BindableObject` implementuje `INotifyPropertyChanged` a automaticky aktivuje `PropertyChanged` událost v případě, že vlastnost založenou `BindableProperty` změny hodnot, ale může zapisovat vaše vlastní třídy implementující `INotifyPropertyChanged` bez odvozování z `BindableObject`.

## <a name="code-and-xaml"></a>Kód a XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) příklad ukazuje, jak nastavit datové vazby v kódu:

- Zdroj je `Value` vlastnost `Slider`
- Cíl je `Opacity` vlastnost `Label`

Tyto dva objekty jsou připojené nastavením `BindingContext` z `Label` do objektu `Slider` objektu. Připojení dvě vlastnosti voláním [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) rozšiřující metody na `Label` odkazující na `OpacityProperty` vazbu vlastnosti a `Value` vlastnost `Slider` vyjádřený jako řetězec.

Manipulace s `Slider` pak způsobí, že `Label` k objevovat a deaktivovat zobrazení.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) je stejný program s datové vazby nastavit v jazyce XAML. `BindingContext` z `Label` je nastaven na `x:Reference` odkazující na rozšíření značek `Slider`a `Opacity` vlastnost `Label` je nastaven na `Binding` – rozšíření značek s jeho [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) odkazování na vlastnost `Value` vlastnost `Slider`.

## <a name="source-and-bindingcontext"></a>Zdroj a vazby

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) příklad ukazuje alternativní způsob v kódu. A `Binding` objektu je vytvořen nastavením [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) vlastnost, která má `Slider` objektu a [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) vlastnost "Value". [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Metodu `BindableObject` se potom volá na `Label` objektu.

[ `Binding` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) také byl použit k definování `Binding` objektu.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) příklad ukazuje porovnatelný z hlediska techniku v jazyce XAML. `Opacity` Vlastnost `Label` je nastaven na `Binding` – rozšíření značek s [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) nastavena na `Value` vlastnost a [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) nastavena na vložené `x:Reference` – rozšíření značek.

V souhrnu existují dva způsoby, jak odkazovat na zdrojový objekt vazby:

- Prostřednictvím `BindingContext` vlastnost cíle
- Prostřednictvím `Source` vlastnost `Binding` samotného objektu

Pokud jsou zadány oba, druhý přednost. Výhodou `BindingContext` je rozšířena prostřednictvím vizuálním stromu. Toto je *velmi* užitečný, pokud více vlastností cíle, které jsou vázány na stejný zdrojový objekt.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) program ukazuje tato technika s [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) element. Dva `Button` prvky pro navigaci zpět a vpřed dědění `BindingContext` ze svého nadřazeného objektu, který odkazuje `WebView`. `IsEnabled` Vlastnosti pro dvě tlačítka pak mít jednoduché `Binding` rozšíření značek, které cílí na tlačítko `IsEnabled` vlastnosti podle nastavení [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) a [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) vlastnosti jen pro čtení `WebView`.

## <a name="the-binding-mode"></a>Režim vazby

Nastavit [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) vlastnost `Binding` k členem [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) výčtu:

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) tak, aby změny ve zdrojové vlastnosti ovlivnit cíl
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) tak, aby se změny v vlastnost target ovlivní zdroj
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) tak, aby změny ve zdrojové a cílové vzájemně ovlivňují.
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) použít [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) zadaný při cíl `BindableProperty` byla vytvořena. Pokud byl zadán žádný výchozí hodnota je `OneWay` pro běžné vlastnosti vazbu, a `OneWayToSource` pro vazbu vlastnosti jen pro čtení.

Vlastnosti, které by mohly být cíle vazby dat ve scénářích rozhraní MVVM obvykle mají `DefaultBindingMode` z `TwoWay`. Jsou to:

- `Value` Vlastnost `Slider` a `Stepper`
- `IsToggled` Vlastnost `Switch`
- `Text` Vlastnost `Entry`, `Editor`, a `SearchBar`
- `Date` Vlastnost `DatePicker`
- `Time` Vlastnost `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) příklad znázorňuje čtyři vazby režimy s vazbu dat, kde je cílem `FontSize` vlastnost `Label` a zdroj je `Value` Vlastnost `Slider`. To umožňuje každý `Slider` k řízení odpovídající velikost písma `Label`. Ale `Slider` elementy nejsou inicializovat, protože `DefaultBindingMode` z `FontSize` vlastnost je `OneWay`.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) ukázka nastaví vazby `Value` vlastnost `Slider` odkazující na `FontSize` vlastnost jednotlivých `Label`. To zdá být zpětné, ale funguje lépe initialzing `Slider` elementy protože `Value` vlastnost `Slider` má `DefaultBindingMode` z `TwoWay`.

[![Trojitá snímek obrazovky zpětná vazba](images/ch16fg06-small.png "zpětná vazba")](images/ch16fg06-large.png#lightbox "zpětná vazba")

Toto je podobná definici vazby v rozhraní MVVM a použijete tento typ vazby často.

## <a name="string-formatting"></a>Řetězec formátování

Pokud vlastnost target je typu `string`, můžete použít [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) vlastnosti definované `BindingBase` převést zdroj k `string`. Nastavte `StringFormat` vlastnost .NET formátování řetězce, který byste použili s statických [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) formát, který se zobrazí objekt. Při použití tohoto formátování řetězce v rámci rozšíření značek, uzavřete ji do s jednoduché uvozovky, složené závorky nebudou omylem považovány za rozšíření embedded značek.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) příklad ukazuje způsob použití `StringFormat` v jazyce XAML.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) příklad znázorňuje zobrazení velikost stránky s vazby na `Width` a `Height` vlastnosti `ContentPage`.

## <a name="why-is-it-called-path"></a>Proč se nazývá "Cesta"?

[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Vlastnost `Binding` je proto volat, protože může být řadu vlastnostmi a indexery, které jsou odděleny tečkami. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) příklad ukazuje několik příkladů.

## <a name="binding-value-converters"></a>Převodníky hodnot vazby

Pokud zdrojové a cílové vlastnosti vazby jsou různé typy, můžete převést mezi typy pomocí převaděče vazby. Toto je třída, která implementuje [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) rozhraní a obsahuje dvě metody: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) převést zdroji na cíl, a [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) Převést cíl ke zdroji.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna je příklad pro převod `int` k `bool`. Je ukázán pomocí [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) vzorku, který umožňuje pouze `Button` Pokud zadal alespoň jeden znak do `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) Třídy převede `bool` k `string` a definuje dvě vlastnosti k určení, jaký text má být vrácen pro `false` a `true` hodnoty.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) Je podobný. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) příklad znázorňuje zobrazení různých texty v různých barev, na základě pomocí těchto dvou převaděče `Switch` nastavení.

Obecná [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) můžete nahradit `BoolToStringConverter` a `BoolToColorConverter` a sloužit jako zobecněný `bool`-na-objektu převodník jiného typu.

## <a name="bindings-and-custom-views"></a>Vazby a vlastních zobrazení

Vlastní ovládací prvky použití vazby dat můžete zjednodušit. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Souboru kódu definuje `Text`, `TextColor`, `FontSize`, `FontAttributes`, a `IsChecked` vlastnosti, ale nemá žádné logiku vůbec pro vizuální prvky ovládacího prvku.
Místo toho [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) soubor obsahuje všechny značky ovládacího prvku vizuály prostřednictvím datové vazby v `Label` elementy podle vlastností definovaných v souboru kódu na pozadí.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) příklad ukazuje `NewCheckBox` vlastního ovládacího prvku.



## <a name="related-links"></a>Související odkazy

- [Úplný text 16 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Ukázky kapitoly 16](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
