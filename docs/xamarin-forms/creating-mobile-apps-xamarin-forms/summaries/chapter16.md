---
title: Souhrn kapitoly 16. Vytváření datových vazeb
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 16. Vytváření datových vazeb'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: 083cb4ed57df989a55a26394cbf8440d53a9e769
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156656"
---
# <a name="summary-of-chapter-16-data-binding"></a>Souhrn kapitoly 16. Vytváření datových vazeb

> [!NOTE] 
> Poznámky na této stránce označit oblasti, kde se Xamarin.Forms se rozcházela z materiály uvedené v seznamu.

Programátoři často naleznou sami sebe zápis obslužné rutiny událostí, které zjišťují, když je změněna vlastnost jednoho objektu a použijte, chcete-li změnit hodnotu vlastnosti v jiném objektu. Tento proces je možné automatizovat pomocí technika *datové vazby*. Datové vazby jsou obvykle definovány v XAML a se stanou součástí definice uživatelského rozhraní.

Tyto datové vazby velmi často připojit objektů uživatelského rozhraní na podkladová data. Toto je technika, která je ve více prozkoumat [ **kapitola 18. MVVM**](chapter18.md). Datové vazby, ale můžete také připojit dva nebo více prvků uživatelského rozhraní. Většina příkladů předčasné datové vazby v této kapitole ukazují tuto techniku.

## <a name="binding-basics"></a>Základy vytváření vazeb

Několik vlastnosti, metody a třídy jsou součástí datové vazby:

- [ `Binding` ](xref:Xamarin.Forms.Binding) Třída odvozena z [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) a zapouzdřuje mnoho vlastností datovou vazbu
- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Vlastnost je definována [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) třídy
- [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Metoda je také definován [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) třídy
- [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) Třída definuje tři další `SetBinding` metody

Následující dvě třídy podporu rozšíření značek XAML pro vazby:

- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) podporuje `Binding` – rozšíření značek
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) podporuje `x:Reference` – rozšíření značek

V datové vazbě se podílejí dvě rozhraní:

- [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) v `System.ComponentModel` obor názvů je implementace oznámení změn vlastností
- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) slouží k definování malé tříd, které provádějí převod hodnoty z jednoho typu na jiný v datové vazby

Datová vazba spojuje dvě vlastnosti stejného objektu, nebo (častěji) dva různé objekty. Tyto dvě vlastnosti se označují jako *zdroj* a *cílové*. Obecně platí změna ve zdrojové vlastnosti způsobí změnu vyskytuje v cílové vlastnosti, ale v některých případech je obrácený směru. Bez ohledu na to:

- *cílové* musí být vlastnost opírá [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)
- *zdroj* vlastnost obecně je členem třídy, která implementuje [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)

Třídu, která implementuje `INotifyPropertyChanged` aktivuje [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) události při změně hodnoty vlastnosti. `BindableObject` implementuje `INotifyPropertyChanged` a automaticky spustí `PropertyChanged` událost, když se opírá o vlastnost `BindableProperty` změní hodnoty, ale můžete napsat vlastní třídy, které implementují `INotifyPropertyChanged` bez odvozený od `BindableObject`.

## <a name="code-and-xaml"></a>Kód a XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) Ukázka předvádí, jak nastavit datové vazby v kódu:

- Zdroj je `Value` vlastnost `Slider`
- Cílem `Opacity` vlastnost `Label`

Dva objekty jsou připojené tak, že nastavíte `BindingContext` z `Label` objektu `Slider` objektu. Dvě vlastnosti jsou připojené pomocí volání [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) rozšiřující metody na `Label` odkazující `OpacityProperty` vázanou vlastnost a `Value` vlastnost `Slider` vyjádřený jako řetězec.

Manipulace `Slider` poté způsobí, že `Label` která má vyblednout do zobrazení.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) je stejný program s datové vazby v XAML. `BindingContext` z `Label` je nastavena na `x:Reference` odkazující na rozšíření značek `Slider`a `Opacity` vlastnost `Label` je nastavena na `Binding` – rozšíření značek s jeho [ `Path` ](xref:Xamarin.Forms.Binding.Path) vlastnosti odkazující `Value` vlastnost `Slider`.

## <a name="source-and-bindingcontext"></a>Zdroj a BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) příklad ukazuje alternativní způsob v kódu. A `Binding` objekt je vytvořen nastavením [ `Source` ](xref:Xamarin.Forms.Binding.Source) vlastnost `Slider` objektu a [ `Path` ](xref:Xamarin.Forms.Binding.Path) vlastnost "Value". [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Metoda `BindableObject` pak volalo `Label` objektu.

[ `Binding` Konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) může také se používají k definování `Binding` objektu.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) příklad ukazuje srovnatelné technika v XAML. `Opacity` Vlastnost `Label` je nastavena na `Binding` – rozšíření značek s [ `Path` ](xref:Xamarin.Forms.Binding.Path) nastavena na `Value` vlastnost a [ `Source` ](xref:Xamarin.Forms.Binding.Source) nastavena na vložený `x:Reference` – rozšíření značek.

Stručně řečeno existují dva způsoby, jak odkazovat na objekt zdroj vazby:

- Až `BindingContext` vlastnosti cíle
- Až `Source` vlastnost `Binding` samotného objektu

Pokud jsou zadány oba, druhým přednost. Výhodou `BindingContext` je, že se šíří prostřednictvím vizuálního stromu. Toto je *velmi* užitečné, pokud více vlastností cíle jsou svázány se stejným zdrojovým objektem.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) program ukazuje tato technika se [ `WebView` ](xref:Xamarin.Forms.WebView) elementu. Dvě `Button` prvky pro navigaci zpět a vpřed dědit `BindingContext` z nadřazeného webu, který odkazuje `WebView`. `IsEnabled` Vlastnosti dvě tlačítka potom mít jednoduchou `Binding` – rozšíření značek, které se zaměřují na tlačítko `IsEnabled` vlastnosti podle nastavení [ `CanGoBack` ](xref:Xamarin.Forms.WebView.CanGoBack) a [ `CanGoForward` ](xref:Xamarin.Forms.WebView.CanGoForward) vlastnosti jen pro čtení `WebView`.

## <a name="the-binding-mode"></a>Režim vazeb

Nastavte [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) vlastnost `Binding` členovi [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) výčtu:

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) tak, aby změny ve zdrojové vlastnosti ovlivňují cíl
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) tak, aby se změny v cílové vlastnosti ovlivní zdroj
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) tak, aby změny ve zdrojové a cílové vzájemně ovlivňují.
- [`Default`](xref:Xamarin.Forms.BindingMode.Default) použít [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) zadán při cíl `BindableProperty` byl vytvořen. Pokud nebyla zadána žádná výchozí hodnota je `OneWay` pro běžné vlastnosti umožňující vazbu, a `OneWayToSource` pro vlastnosti jen pro čtení s možností vazby.

> [!NOTE]
> `BindingMode` Výčet teď také zahrnuje `OnTime` pro použití vazby pouze v případě, že se změní kontext vazby a ne už při změně vlastnosti zdroje.

Vlastnosti, které by mohly být cíle datové vazby ve scénářích MVVM obecně mít `DefaultBindingMode` z `TwoWay`. Toto jsou:

- `Value` Vlastnost `Slider` a `Stepper`
- `IsToggled` Vlastnost `Switch`
- `Text` Vlastnost `Entry`, `Editor`, a `SearchBar`
- `Date` Vlastnost `DatePicker`
- `Time` Vlastnost `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) ukázce režimy čtyři vazby s datové vazby, kde je cílem `FontSize` vlastnost `Label` a zdroj je `Value` Vlastnost `Slider`. To umožňuje každému `Slider` rozumnou velikost písma k odpovídající položce `Label`. Ale `Slider` prvky nejsou inicializovat, protože `DefaultBindingMode` z `FontSize` vlastnost `OneWay`.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) ukázka nastaví vazby na `Value` vlastnost `Slider` odkazující na `FontSize` vlastnosti každého `Label`. Zdá se být zpětně, ale funguje lépe initialzing `Slider` prvky protože `Value` vlastnost `Slider` má `DefaultBindingMode` z `TwoWay`.

[![Trojitá snímek zpětná vazba](images/ch16fg06-small.png "zpětná vazba")](images/ch16fg06-large.png#lightbox "zpětná vazba")

To je obdobou jak jsou definované vazby v MVVM a použijete tento typ vazby často.

## <a name="string-formatting"></a>Formátování řetězců

Pokud je cílovou vlastnost typu `string`, můžete použít [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) vlastnosti určené `BindingBase` převést zdroj na `string`. Nastavte `StringFormat` vlastnost .NET formátovací řetězec, který byste použili pro statické [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) formát pro zobrazení objektu. Při použití tohoto formátování řetězce v rámci rozšíření značek, uzavřete jej mezi jednoduché uvozovky, složené závorky nesmí být zaměněny za rozšíření vložený kód.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) Ukázka předvádí, jak používat `StringFormat` v XAML.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) ukázce zobrazení velikost stránky pomocí vazby `Width` a `Height` vlastnosti `ContentPage`.

## <a name="why-is-it-called-path"></a>Proč se nazývá "Cesty"?

[ `Path` ](xref:Xamarin.Forms.Binding.Path) Vlastnost `Binding` je proto volat, protože může být řadu vlastnostmi a indexery oddělených tečkami. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) příklad ukazuje několik příkladů.

## <a name="binding-value-converters"></a>Převaděče hodnot vazeb

Pokud jsou zdrojové a cílové vlastnosti vazby různých typů, můžete převést mezi typy použití převaděče vazby. Toto je třída, která implementuje [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) rozhraní a obsahuje dvě metody: [ `Convert` ](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) pro převedení zdroje do cíle, a [ `ConvertBack` ](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) Převod cíle na zdroj.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna je příkladem pro převod `int` k `bool`. Je znázorněn ve [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) vzorku, který umožňuje pouze `Button` Pokud zadal aspoň jeden znak do `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) Třídy převede `bool` k `string` a definuje dvě vlastnosti k určení, jaký text má být vrácen pro `false` a `true` hodnoty.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) Je podobná. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) příklad ukazuje použití těchto dvou převaděče k zobrazení různých textů v odlišných barvách, na základě `Switch` nastavení.

Obecné [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) můžete nahradit `BoolToStringConverter` a `BoolToColorConverter` a slouží jako zobecněný `bool`-na-objekt převaděče libovolného typu.

## <a name="bindings-and-custom-views"></a>Vazby a vlastních zobrazení

Můžete zjednodušit vlastní ovládací prvky pomocí datové vazby. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Soubor kódu definuje `Text`, `TextColor`, `FontSize`, `FontAttributes`, a `IsChecked` vlastnosti, ale nemá žádné logiku vůbec pro vizuály ovládacího prvku.
Místo toho [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) soubor obsahuje všechny značky pro ovládací prvek vizuály prostřednictvím datové vazby na `Label` prvky podle vlastnosti definované v souboru kódu na pozadí.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) příklad ukazuje, `NewCheckBox` vlastního ovládacího prvku.



## <a name="related-links"></a>Související odkazy

- [Kapitola 16 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Ukázky kapitola 16](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
- [Datová vazba](~/xamarin-forms/app-fundamentals/data-binding/index.md)
