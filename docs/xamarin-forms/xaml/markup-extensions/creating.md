---
title: Vytváření rozšíření značek pro jazyk XAML
description: Definovat vlastní vlastní rozšíření značek v jazyce XAML
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d4ae3b42c5c926749310da6e36b6f4e9754d398c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="creating-xaml-markup-extensions"></a>Vytváření rozšíření značek pro jazyk XAML

Na úroveň programová rozšíření značek XAML je třída, která implementuje [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) nebo [ `IMarkupExtension<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension%3CT%3E/) rozhraní. Můžete si prostudovat zdrojový kód rozšíření standardní značek, které jsou popsané níže v [ **MarkupExtensions** directory](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) úložiště Xamarin.Forms GitHub. 

Je také možné definovat vlastní vlastní rozšíření značek v jazyce XAML odvozené z `IMarkupExtension` nebo `IMarkupExtension<T>`. Obecný formulář použijte, pokud rozšíření značek získá hodnotu konkrétního typu. Toto je tomu u několik rozšíření značek Xamarin.Forms:

- `TypeExtension` odvozená z `IMarkupExtension<Type>`
- `ArrayExtension` odvozená z `IMarkupExtension<Array>`
- `DynamicResourceExtension` odvozená z `IMarkupExtension<DynamicResource>`
- `BindingExtension` odvozená z `IMarkupExtension<BindingBase>`
- `ConstraintExpression` odvozená z `IMarkupExtension<Constraint>`

Dva `IMarkupExtension` rozhraní definovat pouze jednu metodu s názvem `ProvideValue`: 

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

Vzhledem k tomu `IMarkupExtension<T>` je odvozena z `IMarkupExtension` i `new` – klíčové slovo na `ProvideValue`, obsahuje oba `ProvideValue` metody.

XAML – rozšíření značek velmi často, definují vlastnosti, které přispívají k návratovou hodnotu. (Je zřejmé výjimka `NullExtension`, ve kterém `ProvideValue` jednoduše vrátí `null`.) `ProvideValue` Metoda má jeden argument typu `IServiceProvider` , bude probírat později v tomto článku.

## <a name="a-markup-extension-for-specifying-color"></a>Rozšíření značek pro zadání barev

Následující rozšíření značek XAML umožňuje vytvořit `Color` hodnotu pomocí hue, sytost a světlost součásti. Definuje vlastnosti čtyři čtyři součástí barvu, včetně alfa součást, který je inicializován na 1. Třída odvozená z `IMarkupExtension<Color>` označíte, `Color` vrátit hodnotu:

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

Protože `IMarkupExtension<T>` je odvozena z `IMarkupExtension`, třída musí obsahovat dvě `ProvideValue` metody, ten, který vrátí `Color` a druhou, která vrací `object`, ale druhá metoda jednoduše volat metodu první.

**HSL barva ukázku** stránka zobrazuje různými způsoby, které `HslColorExtension` se mohou objevit v souboru XAML zadat barvu `BoxView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    
    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

Všimněte si, že když `HslColorExtension` je značky XML, jako atributy jsou nastavené čtyři vlastnosti, ale když se objeví mezi složené závorky, čtyři vlastnosti se oddělují čárkami bez uvozovek. Výchozí hodnoty pro `H`, `S`, a `L` je 0, výchozí hodnota `A` je 1, takže tyto vlastnosti lze vynechat, pokud chcete nastavit výchozí hodnoty. Poslední příklad ukazuje příklad, kdy Světelnost je 0, což obvykle vede k černé, ale alfa kanálu je 0,5, takže je půl transparentní a zobrazí se šedé proti bílé pozadí stránky:

[![HSL – barva ukázkový](creating-images/hslcolordemo-small.png "HSL barva ukázkový")](creating-images/hslcolordemo-large.png#lightbox "HSL barva Demo")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Rozšíření značek pro přístup k rastrové obrázky

Argument `ProvideValue` je objekt, který implementuje [ `IServiceProvider` ](https://developer.xamarin.com/api/type/System.IServiceProvider/) rozhraní, která je definována v .NET `System` oboru názvů. Toto rozhraní má jednoho člena, metodu s názvem `GetService` s `Type` argument. 

`ImageResourceExtension` Třídy, viz následující obrázek ukazuje jedno možné použití `IServiceProvider` a `GetService` získat `IXmlLineInfoProvider` objekt, který může poskytnout řádku a znak informace o tom, kde byla zjištěna konkrétní chyba. V takovém případě je vyvolána výjimka při `Source` nebyla nastavena vlastnost:

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;

        return ImageSource.FromResource(assemblyName + "." + Source);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` je užitečné, pokud soubor XAML potřebuje přístup k souboru bitové kopie uložené jako vložený prostředek v projektu knihovny přenosných tříd. Použije `Source` vlastnost zavolejte statickou `ImageSource.FromResource` metoda. Tato metoda vyžaduje prostředků plně kvalifikovaný název, který se skládá z název sestavení, název složky a název souboru odděleny tečkami. `ImageResourceExtension` Není nutné sestavení název část protože získá název sestavení pomocí reflexe a přidá ji do `Source` vlastnost. Bez ohledu na to `ImageSource.FromResource` od sestavení, které obsahuje rastrového obrázku, což znamená, že tato rozšíření prostředků XAML nemůže být součástí vnější knihovny, pokud jsou bitové kopie i v této knihovně se musí volat. (Viz [ **vložené obrázky** ](~/xamarin-forms/user-interface/images.md#embedded_images) článku Další informace o přístupu k bitmap uložené jako vložené prostředky.) 

I když `ImageResourceExtension` vyžaduje `Source` vlastnost, která má být nastaveno, `Source` vlastnost jako vlastnost obsahu třídy uvedené v atributu. To znamená, že `Source=` lze vynechat část výrazu do složených závorek. V **ukázkový prostředek obrázku** stránky, `Image` elementy načíst dvě bitové kopie pomocí název složky a název souboru odděleny tečkami:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

Tady je programy spuštěné na všech tří platformách:

[![Obrázek ukázkové prostředků](creating-images/imageresourcedemo-small.png "obrázek ukázkové prostředků")](creating-images/imageresourcedemo-large.png#lightbox "obrázek ukázkové prostředků")

## <a name="service-providers"></a>Poskytovatelé služeb

Pomocí `IServiceProvider` argument `ProvideValue`, XAML – rozšíření značek můžete získat přístup k užitečné informace o souboru XAML, ve kterém se právě používá. Ale pro použití `IServiceProvider` argument úspěšně, je potřeba vědět, jaké služby jsou k dispozici v konkrétní kontexty. Nejlepší způsob, jak získat představu o tato funkce je studujete zdrojový kód existující XAML – rozšíření značek v [ **MarkupExtensions** složky](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) v Xamarin.Forms úložišti na Githubu. Upozorňujeme, že jsou interní Xamarin.Forms některé typy služeb.

V některých – rozšíření značek XAML tato služba může být užitečné:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` Rozhraní definuje dvě vlastnosti `TargetObject` a `TargetProperty`. Když se tyto informace získá v `ImageResourceExtension` třídy, `TargetObject` je `Image` a `TargetProperty` je `BindableProperty` objekt pro `Source` vlastnost `Image`. Toto je vlastnost, na kterém je nastavená – rozšíření značek jazyce XAML.

`GetService` Volání s argument `typeof(IProvideValueTarget)` ve skutečnosti vrátí objekt typu `SimpleValueTargetProvider`, která je definována v `Xamarin.Forms.Xaml.Internals` oboru názvů. Pokud přetypování návratová hodnota `GetService` na typ, můžete taky Přejít `ParentObjects` vlastnost, která je pole, které obsahuje `Image` elementu, `Grid` nadřazeným prvkem a `ImageResourceDemoPage` nadřazené položky `Grid`.

## <a name="conclusion"></a>Závěr

XAML – rozšíření značek hrát zásadní roli v jazyce XAML tím, že rozšíří možnost nastavit atributy z různých zdrojů. Kromě toho pokud existující rozšíření značek XAML neposkytují přesně to, co potřebujete, můžete taky napsat vlastní. 


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Kapitola rozšíření značek XAML z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
