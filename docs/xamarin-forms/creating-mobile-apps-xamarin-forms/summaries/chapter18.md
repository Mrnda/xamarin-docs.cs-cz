---
title: "Shrnutí kapitoly 18. MVVM"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: cadab2432d0b6c29ead9cde1f4220bb64e1e1886
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-18-mvvm"></a>Shrnutí kapitoly 18. MVVM

Jedním z nejlepší způsoby architektury aplikace je oddělením uživatelské rozhraní z kódu, která se někdy nazývá *obchodní logiky*. Existuje několik postupů, ale ten, který je přizpůsobené pro prostředí založených na XAML se označuje jako Model-View-ViewModel nebo rozhraní MVVM.

## <a name="mvvm-interrelationships"></a>Vzájemné vztahy rozhraní MVVM

Aplikace rozhraní MVVM má tři vrstvy:

- Model někdy poskytuje základní data v souborech nebo má přístup k webové
- Zobrazení je uživatelské rozhraní nebo prezentační vrstvě, obecně implementována v jazyce XAML
- ViewModel připojí modelu a zobrazení

Model je které ignorují z ViewModel a ViewModel je zobrazení, které ignorují. Tyto tři vrstvy obecně připojit k sobě navzájem pomocí následujících mechanismů:

![Zobrazení, ViewModel a zobrazení](images/ch18fg03.png "rozhraní MVVM")

V mnoha menší programy (a i větší ty), často modelu chybí nebo jeho funkce je integrovaná do ViewModel.

## <a name="viewmodels-and-data-binding"></a>ViewModels a datová vazba

K páchání vazby dat, musí být schopný zobrazení upozornění, když došlo ke změně vlastností ViewModel ViewModel. ViewModel dosahuje tím, že implementace [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) rozhraní v `System.ComponentModel` oboru názvů. Toto je část .NET, nikoli Xamarin.Forms. (Obvykle ViewModels pokusí udržovat platformy nezávislost.)

`INotifyPropertyChanged` Rozhraní deklaruje jednu událost s názvem [ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) určující vlastnost, která se změnila.

### <a name="a-viewmodel-clock"></a>Hodiny ViewModel

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) knihovny definuje vlastnost typu `DateTime` , změny podle časovač. Třída implementuje `INotifyPropertyChanged` a aktivuje se `PropertyChanged` událost vždy, když `DateTime` změny vlastností.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) ukázka vytvoří tento ViewModel a používá ViewModel vazby na data pro zobrazení informací aktualizovaná data a času.

### <a name="interactive-properties-in-a-viewmodel"></a>Interaktivní vlastnosti v ViewModel

Vlastnosti v ViewModel může být interaktivní, jak je předvedeno podle [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) třída, která je součástí z [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) ukázka. Datové vazby zadejte násobenec a násobitel hodnoty ze dvou `Slider` elementy a zobrazit produktu s `Label`. Můžete však provést rozsáhlé změny tohoto uživatelského rozhraní v jazyce XAML bez následný změn ViewModel nebo souboru kódu na pozadí.

### <a name="a-color-viewmodel"></a>Barva ViewModel

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) knihovny integruje modelů barva RGB a HSL. Je znázorněn v [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) ukázka:

[![Trojitá snímek obrazovky TK](images/ch18fg08-small.png "HSL barevný Model")](images/ch18fg08-large.png#lightbox "HSL barva modelu")

### <a name="streamlining-the-viewmodel"></a>Zjednodušení ViewModel

Kód v ViewModels můžete zjednodušený definováním `OnPropertyChanged` metoda pomocí [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/) atribut, který automaticky získá název vlastnosti volání. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) knihovny tomu a poskytuje základní třídu pro ViewModels.

## <a name="the-command-interface"></a>Rozhraní příkazového

Rozhraní MVVM pracuje s datové vazby a datové vazby pracovat s vlastnostmi, takže modelem MVVM zdá se, že se nedostatečný, pokud jde o zpracování `Clicked` události `Button` nebo `Tapped` události `TapGestureRecognizer`. Povolit ViewModels ke zpracování těchto událostí, podporuje Xamarin.Forms *rozhraní příkazového*.

Rozhraní příkazového se projevuje v `Button` s dvě veřejné vlastnosti:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) typu [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (definované v `System.Windows.Input` oboru názvů)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) typu `Object`

Pro podporu rozhraní příkaz, musíte definovat ViewModel vlastnost typu `ICommand` který je pak data vázaná na `Command` vlastnost `Button`. `ICommand` Rozhraní deklaruje dvě metody a jedné události:

- [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/) Metoda s parametrem typu `object`
- A [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/) metoda s parametrem typu `object` , který vrací `bool`
- A [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/) událostí

Interně ViewModel nastaví každou vlastnost typu `ICommand` na instanci třídy, která implementuje `ICommand` rozhraní. Prostřednictvím datové vazby `Button` původně volá `CanExecute` metody a vypne, pokud metoda vrátí `false`. Nastaví taky obslužnou rutinu pro `CanExecuteChanged` událostí a volání `CanExecute` vždy, když je aktivována, že událost. Pokud `Button` je povoleno, zavolá `Execute` metoda vždy, když `Button` po kliknutí na.

Může mít některé ViewModels, která je starší než toto Xamarin.Forms, a to už může podporovat rozhraní příkazového. Pro nové ViewModels se mají použít pouze s Xamarin.Forms, poskytuje Xamarin.Forms [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) třídy a [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) třídy implementující `ICommand` rozhraní. Obecný typ je typ argumentu `Execute` a `CanExecute` metody.

### <a name="simple-method-executions"></a>Jednoduchý způsob spuštění

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) příklad ukazuje způsob použití rozhraní příkazového v ViewModel. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Třída definuje dvě vlastnosti typu `ICommand` a také definuje dva privátní vlastnosti, které pak předá nejjednodušším [ `Command` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/). Program obsahuje vazby dat z této ViewModel k `Command` vlastnosti dva `Button` elementy.

`Button` Elementy lze snadno nahradit `TapGestureRecognizer` objekty v jazyce XAML beze změn kódu.

### <a name="a-calculator-almost"></a>Kalkulátor, téměř

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) ukázkové díky použít i `Execute` a `CanExecute` metody `ICommand`. Používá [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) knihovny. ViewModel obsahuje šest vlastností typu `ICommand`. Tyto jsou inicializovány z [ `Command` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/) a [ `Command` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) z `Command` a [ `Command<T>` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) z `Command<T>`. Číselné klávesy přidání počítače je vázána na vlastnost, která je inicializován s `Command<T>`a `string` argument `Execute` a `CanExecute` identifikuje konkrétní klíč.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels a životního cyklu aplikací

`AdderViewModel` Používány **AddingMachine** ukázka také definuje dvě metody s názvem `SaveState` a `RestoreState`. Tyto metody jsou volány z aplikace při přechodu do režimu spánku a při spuštění znovu.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 18 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Ukázky kapitoly 18](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
