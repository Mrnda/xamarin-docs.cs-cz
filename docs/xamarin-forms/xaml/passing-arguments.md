---
title: Předávání argumentů v XAML
description: Tento článek ukazuje použití XAML atributy, které je možné předat argumenty do jiné než výchozí konstruktory, volání metody pro vytváření objektů a určení typu obecný argument.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: 51b72d9143895543715c519a65cf8c82aa4d12f7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996545"
---
# <a name="passing-arguments-in-xaml"></a>Předávání argumentů v XAML

_Tento článek ukazuje použití XAML atributy, které je možné předat argumenty do jiné než výchozí konstruktory, volání metody pro vytváření objektů a určení typu obecný argument._

## <a name="overview"></a>Přehled

Často je potřeba vytvořit instanci objektů pomocí konstruktorů, které vyžadují argumenty nebo voláním metody pro vytvoření statické. Toho lze dosáhnout v XAML pomocí `x:Arguments` a `x:FactoryMethod` atributy:

- `x:Arguments` Atribut se používá k určení argumenty konstruktoru pro jiné než výchozí konstruktor, nebo pro deklaraci objektu factory metody. Další informace najdete v tématu [předávání argumentů konstruktoru](#constructor_arguments).
- `x:FactoryMethod` Atribut se používá k určení metoda factory, který slouží k inicializaci objektu. Další informace najdete v tématu [volání metody pro vytváření objektů](#factory_methods).

Kromě toho `x:TypeArguments` atribut lze použít k určení obecného typu argumentů konstruktoru obecného typu. Další informace najdete v tématu [určující obecný Argument typu](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Předávání argumentů konstruktoru

Argumenty je možné předat do jiného než výchozího konstruktoru pomocí `x:Arguments` atribut. V rámci elementu XML, který reprezentuje typ argumentu musí být oddělené každý argument konstruktoru. Xamarin.Forms pro základní typy podporuje následující prvky:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

Následující příklad kódu ukazuje použití `x:Arguments` atribut s třemi [ `Color` ](xref:Xamarin.Forms.Color) konstruktory:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Počet elementů v rámci `x:Arguments` značku a typy tyto prvky musí odpovídat jedné ze [ `Color` ](xref:Xamarin.Forms.Color) konstruktory. `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double)) pomocí jediného parametru vyžaduje ve stupních šedi hodnotu od 0 (černá) na hodnotu 1 (prázdné). `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) se třemi parametry vyžaduje červené, zelené a modré hodnotu od 0 do 1. `Color` [Konstruktor](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) s čtyři parametry přidá alfa kanál jako čtvrtý parametr.

Na následujících snímcích obrazovky zobrazit výsledek každé volání [ `Color` ](xref:Xamarin.Forms.Color) konstruktor s hodnotami zadaného argumentu:

![](passing-arguments-images/passing-arguments.png "BoxView.Color zadaným x: Arguments")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Volání metody pro vytváření objektů

V XAML lze volat metody pro vytváření objektů zadáním metody pojmenovat pomocí `x:FactoryMethod` atribut a jeho argumentů pomocí `x:Arguments` atribut. Výrobní metoda je `public static` metodu, která vrací objekty nebo hodnoty stejného typu jako třída nebo struktura, která definuje metody.

[ `Color` ](xref:Xamarin.Forms.Color) Struktury definuje několik metod objekt pro vytváření a následující příklad kódu ukazuje volání těchto tří možností:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Počet elementů v rámci `x:Arguments` značku a typy tyto prvky musí odpovídat argumentů volané metody objekt pro vytváření. [ `FromRgba` ](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) Výrobní metoda vyžaduje čtyři [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) parametry, které představují hodnoty červené, zelená, modrá a alfa od 0 do 255 v uvedeném pořadí. [ `FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) Výrobní metoda vyžaduje čtyři [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) parametry, které představují odstín, sytost, světelnost a hodnoty alfa, od 0 do 1 v uvedeném pořadí. [ `FromHex` ](xref:Xamarin.Forms.Color.FromHex(System.String)) Vyžaduje výrobní metoda [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) , která představuje šestnáctkového (A) barva RGB.

Na následujících snímcích obrazovky zobrazit výsledek každé volání [ `Color` ](xref:Xamarin.Forms.Color) výrobní metoda hodnotami zadaného argumentu:

![](passing-arguments-images/factory-methods.png "BoxView.Color zadaným x: FactoryMethod a x: Arguments")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Určení Argument obecného typu

Argumenty obecného typu pro konstruktor obecného typu se dá nastavit pomocí `x:TypeArguments` atributu, jak je ukázáno v následujícím příkladu kódu:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) Třída je obecná třída a musí být vytvořena pomocí `x:TypeArguments` atribut, který odpovídá cílovým typem. V [ `On` ](xref:Xamarin.Forms.On) třídy, [ `Platform` ](xref:Xamarin.Forms.On.Platform) atribut může přijmout jediného `string` hodnotu nebo více oddělených čárkou `string` hodnoty. V tomto příkladu [ `StackLayout.Margin` ](xref:Xamarin.Forms.View.Margin) je nastavena na konkrétní platformu [ `Thickness` ](xref:Xamarin.Forms.Thickness).

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali, pomocí atributů XAML, které je možné předat argumenty do jiné než výchozí konstruktory, volání metody pro vytváření objektů a určení typu obecný argument.


## <a name="related-links"></a>Související odkazy

- [Obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md)
- [Předávání argumentů konstruktoru (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Volání metody pro vytváření objektů (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
