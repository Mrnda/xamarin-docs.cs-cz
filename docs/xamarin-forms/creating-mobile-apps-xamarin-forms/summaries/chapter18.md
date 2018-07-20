---
title: Souhrn kapitola 18. MVVM
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 18. MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 30fa06364e49294a71d37c29d4b9f1e95deac12f
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156922"
---
# <a name="summary-of-chapter-18-mvvm"></a>Souhrn kapitola 18. MVVM

Jednou z nejlepších způsobů, jak navrhovat aplikace je tak, že oddělíte uživatelského rozhraní od základního kódu, který se někdy označuje jako *obchodní logiky*. Existuje několik postupů, ale ten, který je vytvořený na míru pro prostředí založeného na XAML se označuje jako Model-View-ViewModel nebo MVVM.

## <a name="mvvm-interrelationships"></a>Vztahy MVVM

MVVM aplikace obsahuje tři vrstvy:

- Model poskytuje podkladová data, někdy v souborech nebo přistupuje k webové
- Zobrazení je uživatelské rozhraní nebo prezentační vrstvě, obvykle implementována v XAML
- Připojuje ViewModel modelu a zobrazení

Model je ignorant z ViewModel a je ViewModel ignorant zobrazení. Tyto tři vrstvy obecně připojit k sobě navzájem pomocí následujících mechanismů:

![Zobrazení, zobrazení a ViewModel](images/ch18fg03.png "MVVM")

V mnoha menší programy (a dokonce většími), často modelu chybí nebo její funkce je integrovaná ViewModel.

## <a name="viewmodels-and-data-binding"></a>Modely ViewModels a datové vazby

Provozovat datové vazby ViewModel musí umožňovat upozornění zobrazení při změně vlastnosti ViewModel. ViewModel toho dosahuje pomocí implementace [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) rozhraní v `System.ComponentModel` oboru názvů. Toto je část .NET spíše než Xamarin.Forms. (Obecně modely ViewModels pokusí udržovat nezávislost platformy.)

`INotifyPropertyChanged` Rozhraní deklaruje jednu událost s názvem [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) , který určuje vlastnost, která se změnila.

### <a name="a-viewmodel-clock"></a>Hodiny ViewModel

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) knihovna definuje vlastnost typu `DateTime` , že změny na základě časovače. Třída implementuje `INotifyPropertyChanged` a aktivuje `PropertyChanged` událost pokaždé, když `DateTime` změny vlastností.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) ukázka vytvoří instanci této ViewModel a používá ViewModel vazeb dat k zobrazení informací aktualizovaná data a času.

### <a name="interactive-properties-in-a-viewmodel"></a>Interaktivní vlastnosti ViewModel

Vlastnosti v ViewModel mohou být interaktivní, jak je uvedeno ve [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) třída, která je součástí sady [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) vzorku. Datové vazby zadejte násobenec a multiplikátor hodnoty ze dvou `Slider` elementy a produkt s se zobrazí `Label`. Můžete však provádět rozsáhlé změny této uživatelské rozhraní XAML s žádné následné změny ViewModel nebo soubor kódu na pozadí.

### <a name="a-color-viewmodel"></a>Barva ViewModel

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) knihovny integruje modely barva RGB a HSL. To je ukázáno v [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) vzorku:

[![Trojitá snímek TK](images/ch18fg08-small.png "HSL barevný Model")](images/ch18fg08-large.png#lightbox "HSL barevný Model")

### <a name="streamlining-the-viewmodel"></a>Zjednodušení ViewModel

Kód v modely ViewModels lze zefektivnit tak, že definujete `OnPropertyChanged` pomocí metody [ `CallerMemberName` ](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute) atribut, který automaticky získá název vlastnosti volání. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) knihovna toho dosahuje a poskytuje základní třídu pro modely ViewModel.

## <a name="the-command-interface"></a>Rozhraní příkazového řádku

MVVM funguje s datové vazby a datové vazby fungují s vlastnostmi, takže MVVM zdá být nedostatečné, pokud jde o zpracování `Clicked` události `Button` nebo `Tapped` události `TapGestureRecognizer`. Pokud chcete povolit modely ViewModels zpracování těchto událostí, podporuje Xamarin.Forms *rozhraní příkazového*.

Rozhraní příkazového řádku se projevuje v `Button` s dvě veřejné vlastnosti:

- [`Command`](xref:Xamarin.Forms.Button.Command) typu [ `ICommand` ](xref:System.Windows.Input.ICommand) (definované v `System.Windows.Input` oboru názvů)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) typu `Object`

Pro podporu rozhraní příkazového řádku, musíte ViewModel definovat vlastnost typu `ICommand` , který je pak data vázaná `Command` vlastnost `Button`. `ICommand` Rozhraní deklaruje dvě metody obnáší a jednu událost:

- [ `Execute` ](xref:System.Windows.Input.ICommand.Execute(System.Object)) Metoda s argumentem typu `object`
- A [ `CanExecute` ](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) metoda s argumentem typu `object` , která vrací `bool`
- A [ `CanExecuteChanged` ](xref:System.Windows.Input.ICommand.CanExecuteChanged) událostí

Interně ViewModel nastaví jednotlivé vlastnosti typu `ICommand` do instance třídy, která implementuje `ICommand` rozhraní. Prostřednictvím datové vazby `Button` zpočátku volá `CanExecute` metody a vypne, pokud metoda vrátí `false`. Také nastaví obslužnou rutinu pro `CanExecuteChanged` událostí a volání `CanExecute` vždy, když se událost aktivuje. Pokud `Button` je povoleno, volá `Execute` metoda pokaždé, když `Button` dojde ke kliknutí na.

Může mít některé modely ViewModel, který jsou staršího data než Xamarin.Forms a už tyto může podporovat rozhraní příkazového řádku. Pro nové modely ViewModels měli v úmyslu použít jenom s Xamarin.Forms, poskytuje Xamarin.Forms [ `Command` ](xref:Xamarin.Forms.Command) třídy a [ `Command<T>` ](xref:Xamarin.Forms.Command`1) třídy, které implementují `ICommand` rozhraní. Obecný typ je typ argumentu `Execute` a `CanExecute` metody.

### <a name="simple-method-executions"></a>Jednoduchý způsob spuštění

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) Ukázka předvádí, jak používat rozhraní příkazového v ViewModel. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Třída definuje dvě vlastnosti typu `ICommand` a také definuje dva privátní vlastnosti, které se předá do nejjednodušší [ `Command` konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)). Program obsahuje datové vazby z této ViewModel k `Command` vlastnosti dvou `Button` elementy.

`Button` Prvky je možné snadno nahradit `TapGestureRecognizer` objekty v XAML beze změny kódu.

### <a name="a-calculator-almost"></a>Kalkulačka, téměř

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) ukázkový umožňuje použít i `Execute` a `CanExecute` metody `ICommand`. Používá [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) knihovny. ViewModel obsahuje šest vlastností typu `ICommand`. Tyto jsou inicializovány z [ `Command` konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)) a [ `Command` konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) z `Command` a [ `Command<T>` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) z `Command<T>`. Číselné klávesy sčítací stroj je vázána na vlastnost, která je inicializována s `Command<T>`a `string` argument `Execute` a `CanExecute` identifikuje konkrétní klíč.

## <a name="viewmodels-and-the-application-lifecycle"></a>Modely ViewModels a životního cyklu aplikací

`AdderViewModel` Používané **AddingMachine** ukázka definuje také dvě metody s názvem `SaveState` a `RestoreState`. Tyto metody jsou volány z aplikace, když přejde do režimu spánku a při spuštění znovu.



## <a name="related-links"></a>Související odkazy

- [Kapitola 18 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Ukázky kapitola 18](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [Modely podnikové aplikací pomocí Xamarin.Forms e-knihy](~/xamarin-forms/enterprise-application-patterns/index.md)
