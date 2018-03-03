---
title: "Předávání argumentů v jazyce XAML"
description: "Tento článek ukazuje použití XAML atributy, které slouží k předání argumentů do jiné než výchozí konstruktory, volání metod vytváření a k určení typu Obecné argumentu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: a30dd9b33466ac6907322f8c6b586c012452a44f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="passing-arguments-in-xaml"></a>Předávání argumentů v jazyce XAML

_Tento článek ukazuje použití XAML atributy, které slouží k předání argumentů do jiné než výchozí konstruktory, volání metod vytváření a k určení typu Obecné argumentu._

## <a name="overview"></a>Přehled

Často je nezbytné k vytváření instancí objektů s konstruktory, které vyžadují argumenty nebo voláním metody pro vytvoření statické. Toho lze dosáhnout v jazyce XAML pomocí `x:Arguments` a `x:FactoryMethod` atributy:

- `x:Arguments` Atribut slouží k určení argumenty konstruktoru pro jiné než výchozí konstruktor, nebo na prohlášení metoda objektu factory. Další informace najdete v tématu [předávání argumenty konstruktoru](#constructor_arguments).
- `x:FactoryMethod` Atribut slouží k určení metoda factory, která slouží k inicializaci objektu. Další informace najdete v tématu [volání metody vytváření](#factory_methods).

Kromě toho `x:TypeArguments` atribut lze použít k určení obecného typu argumenty pro konstruktor obecného typu. Další informace najdete v tématu [zadáním obecný typ argumentu](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Předávání argumentů – konstruktor

Argumenty se dá předat do jiné než výchozí konstruktor pomocí `x:Arguments` atribut. Každý argument konstruktoru musí být oddělené v rámci elementu XML, který představuje typ argumentu. Xamarin.Forms podporuje následující prvky pro základní typy:

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

Následující příklad kódu ukazuje, jak pomocí `x:Arguments` atribut s třemi [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) konstruktory:

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

Počet elementů v rámci `x:Arguments` značky a typy tyto prvky, musí shodovat s jedním z [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) konstruktory. `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/) pomocí jediného parametru vyžaduje ve stupních šedi hodnotu od 0 (černá) na hodnotu 1 (prázdný). `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) se třemi parametry vyžaduje červené, zelené a modré hodnotu rozsahu od 0 do 1. `Color` [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) pomocí čtyř parametrů přidá alfa kanálu jako čtvrtého parametru.

Na následujících snímcích obrazovky zobrazit na výsledek volání každý [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) konstruktor s hodnotami zadaný argument:

![](passing-arguments-images/passing-arguments.png "BoxView.Color zadaným x: Arguments")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Volání metody objektu pro vytváření

Metody vytváření lze volat v jazyce XAML zadáním metody název pomocí `x:FactoryMethod` atribut a jeho argumenty pomocí `x:Arguments` atribut. Metoda factory je `public static` metodu, která vrátí objekty nebo hodnoty stejného typu jako třídu nebo strukturu, která definuje metody.

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Struktura definuje počet metodami pro vytváření a následující příklad kódu ukazuje volání těchto tří možností:

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

Počet elementů v rámci `x:Arguments` značky a typy tyto prvky, musí odpovídat argumenty volaná metoda objektu factory. [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) Metoda factory vyžaduje čtyři [ `Int32` ](https://developer.xamarin.com/api/type/System.Int32/) parametry, které představují červené, zelené, modré a alfa hodnot rozsahu od 0 do 255 v uvedeném pořadí. [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) Metoda factory vyžaduje čtyři [ `Double` ](https://developer.xamarin.com/api/type/System.Double/) parametry, které představují hue, sytost, světlost a hodnoty alfa rozsahu od 0 do 1 v uvedeném pořadí. [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) Vyžaduje metoda factory [ `String` ](https://developer.xamarin.com/api/type/System.String/) představující šestnáctkový (A) barva RGB.

Na následujících snímcích obrazovky zobrazit na výsledek volání každý [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) metoda factory zadaného argumentu hodnotami:

![](passing-arguments-images/factory-methods.png "BoxView.Color zadaným x: factorymethod – a x: Arguments")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Určení Argument obecného typu

Obecný typ argumenty pro konstruktor obecného typu je možné zadat pomocí `x:TypeArguments` atributu, jak je ukázáno v následujícím příkladu kódu:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="WinPhone, Windows" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Třída je obecné třídy a musí být vytvořena s `x:TypeArguments` atribut, který odpovídá typu cíle. V [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) třídy, [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) atribut může přijmout jeden `string` hodnotu, nebo více oddělených čárkou `string` hodnoty. V tomto příkladu [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) je nastavena na příslušnou platformu [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/).

## <a name="summary"></a>Souhrn

Tento článek ukázal pomocí XAML atributy, které slouží k předání argumentů do jiné než výchozí konstruktory, volání metod vytváření a k určení typu Obecné argumentu.


## <a name="related-links"></a>Související odkazy

- [Obory názvů jazyka XAML](~/xamarin-forms/xaml/namespaces.md)
- [Předání argumentů – konstruktor (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Volání metod vytváření (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
