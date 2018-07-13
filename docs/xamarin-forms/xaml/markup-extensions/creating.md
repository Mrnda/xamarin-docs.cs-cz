---
title: Vytváření rozšíření značek XAML
description: Tento článek vysvětluje, jak definovat vlastní vlastní rozšíření značek XAML Xamarin.Forms. Rozšíření značek XAML je třída, která implementuje rozhraní IMarkupExtension IMarkupExtension.
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d4b3d5c65ddf8be433d1f8e182774aa839f60357
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995593"
---
# <a name="creating-xaml-markup-extensions"></a>Vytváření rozšíření značek XAML

Na úroveň programová rozšíření značek XAML je třída, která implementuje [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) nebo [ `IMarkupExtension<T>` ](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) rozhraní. Zdrojový kód je popsáno níže v rozšíření standardních značek můžete prozkoumat [ **MarkupExtension** directory](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) úložiště Xamarin.Forms GitHub.

Je také možné definovat vlastní vlastní rozšíření značek XAML tak, že odvozený od `IMarkupExtension` nebo `IMarkupExtension<T>`. Obecný formulář použijte, pokud rozšíření značek získá hodnoty určitého typu. Toto je tomu u několika Xamarin.Forms rozšíření značek:

- `TypeExtension` je odvozen od `IMarkupExtension<Type>`
- `ArrayExtension` je odvozen od `IMarkupExtension<Array>`
- `DynamicResourceExtension` je odvozen od `IMarkupExtension<DynamicResource>`
- `BindingExtension` je odvozen od `IMarkupExtension<BindingBase>`
- `ConstraintExpression` je odvozen od `IMarkupExtension<Constraint>`

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

Protože `IMarkupExtension<T>` je odvozena z `IMarkupExtension` a zahrnuje `new` – klíčové slovo na `ProvideValue`, obsahuje `ProvideValue` metody.

Rozšíření značek XAML velmi často definují vlastnosti, které přispívají návratovou hodnotu. (Je zřejmé výjimka `NullExtension`, ve kterém `ProvideValue` jednoduše vrací `null`.) `ProvideValue` Metoda má jeden argument typu `IServiceProvider` , které budou popsány dále v tomto článku.

## <a name="a-markup-extension-for-specifying-color"></a>Rozšíření značek pro určení barvy

Následující rozšíření značek XAML umožňuje vytvářet `Color` hodnotu použití hue, sytosti a světlosti komponent. Definuje čtyři vlastnosti pro čtyři součásti barvy, včetně alfa složkou, který je inicializován na hodnotu 1. Třída je odvozena z `IMarkupExtension<Color>` k označení `Color` návratovou hodnotu:

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

Protože `IMarkupExtension<T>` je odvozena z `IMarkupExtension`, třída musí obsahovat dvě `ProvideValue` metody, který vrátí `Color` a další vlastnost, která vrátí `object`, ale druhá metoda můžete jednoduše zavolejte metodu první.

**Ukázka barvy HSL** stránka zobrazuje celou řadu způsobů, který `HslColorExtension` se mohou objevit v souboru XAML pro určení barvy pro `BoxView`:

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

Všimněte si, že `HslColorExtension` je značka XML, jsou čtyři vlastnosti nastavena jako atributy, ale když se objeví mezi složenými závorkami, čtyři vlastnosti jsou odděleny čárkami bez uvozovek. Výchozí hodnoty pro `H`, `S`, a `L` jsou 0 a výchozí hodnota `A` je 1, takže tyto vlastnosti lze vynechat, pokud chcete, aby je nastavené na výchozí hodnoty. Poslední příklad ukazuje příklad, kdy Světelnost je 0, což je normálně ve výsledku black, ale kanál alfa je 0,5, takže je částečně transparentní a zobrazí se šedé bílé pozadí stránky:

[![Ukázka barvy HSL](creating-images/hslcolordemo-small.png "ukázka barvy HSL")](creating-images/hslcolordemo-large.png#lightbox "HSL – ukázka barvy")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Rozšíření značek pro přístup k rastrových obrázků

Argument `ProvideValue` je objekt, který implementuje [ `IServiceProvider` ](xref:System.IServiceProvider) rozhraní, která je definována v rozhraní .NET `System` oboru názvů. Toto rozhraní má jeden člen metodu s názvem `GetService` s `Type` argument.

`ImageResourceExtension` Níže uvedená třída ukazuje jeden využití `IServiceProvider` a `GetService` získat `IXmlLineInfoProvider` objekt, který může poskytnout řádku a znak informace o tom, kde byla zjištěna konkrétní chyba. V takovém případě je vyvolána výjimka při `Source` nebyla nastavena vlastnost:

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

`ImageResourceExtension` je užitečné, když soubor XAML je potřeba přístup k souboru bitové kopie uložené jako vložený prostředek v projektu knihovny .NET Standard. Používá `Source` volat statickou vlastnost `ImageSource.FromResource` metody. Tato metoda vyžaduje prostředků plně kvalifikovaný název, který se skládá z názvu sestavení, název složky a název souboru oddělených tečkami. `ImageResourceExtension` Není nutné název sestavení část protože získá název sestavení pomocí reflexe a připojí na začátek `Source` vlastnost. Bez ohledu na to `ImageSource.FromResource` musí být volána v sestavení, které obsahuje rastrový obrázek, což znamená, že tato rozšíření prostředků XAML nemůže být součástí externí knihovny, pokud obrázky jsou také v této knihovně. (Viz [ **vložené obrázky** ](~/xamarin-forms/user-interface/images.md#embedded_images) najdete další informace o přístupu k rastrové obrázky uložené jako vložené prostředky.)

I když `ImageResourceExtension` vyžaduje `Source` vlastnosti chcete nastavit, `Source` vlastnost je uvedeno v atributu obsahu vlastnost třídy. To znamená, že `Source=` součástí výrazu ve složených závorkách lze vynechat. V **ukázku prostředek obrázku** stránky, `Image` načíst prvky dvou obrázků s využitím název složky a název souboru oddělených tečkami:

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

Tady je program spuštěn na všech třech platformách:

[![Ukázka prostředků obrázků](creating-images/imageresourcedemo-small.png "obrázku ukázku prostředků")](creating-images/imageresourcedemo-large.png#lightbox "obrázku ukázku prostředků")

## <a name="service-providers"></a>Poskytovatelé služeb

S použitím `IServiceProvider` argument `ProvideValue`, rozšíření značek XAML můžete získat přístup k užitečné informace o souboru XAML, ve kterém se právě používají. Ale pro použití `IServiceProvider` argument úspěšně, je potřeba vědět, jaké služby jsou k dispozici v konkrétním kontextu. Nejlepší způsob, jak pomůžou pochopit, tato funkce je při zkoumání zdrojový kód existující rozšíření značek XAML v [ **MarkupExtension** složky](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) v Xamarin.Forms úložišti na Githubu. Mějte na paměti, že některé typy služeb jsou interní v Xamarin.Forms.

V některých rozšíření značek XAML tato služba může být užitečné:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` Rozhraní definuje dvě vlastnosti `TargetObject` a `TargetProperty`. Když se tyto informace získá v `ImageResourceExtension` třídy `TargetObject` je `Image` a `TargetProperty` je `BindableProperty` objekt pro `Source` vlastnost `Image`. Toto je vlastnost, na kterém je nastavená – rozšíření značek XAML.

`GetService` Volání s argumentem `typeof(IProvideValueTarget)` ve skutečnosti vrátí objekt typu `SimpleValueTargetProvider`, který je definován v `Xamarin.Forms.Xaml.Internals` oboru názvů. Pokud přetypovávat návratovou hodnotu z `GetService` pro tento typ se dá dostat taky `ParentObjects` vlastnost, která je pole obsahující `Image` elementu, `Grid` nadřazeným prvkem a `ImageResourceDemoPage` nadřazeného člena `Grid`.

## <a name="conclusion"></a>Závěr

Rozšíření značek XAML hrát zásadní roli v XAML rozšířením možnost nastavit atributy z různých zdrojů. Kromě toho pokud existující rozšíření značek XAML neposkytují přesně to, co potřebujete, můžete také napsat vlastní.


## <a name="related-links"></a>Související odkazy

- [Rozšíření značek (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Kapitola rozšíření značek XAML Xamarin.Forms knihy](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
