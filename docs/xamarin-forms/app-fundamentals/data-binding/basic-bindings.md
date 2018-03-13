---
title: "Základní vazby"
description: "Datová vazba cíle, zdroje a kontextu vazby"
ms.topic: article
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b82c0471985306962133c3bf7b084b49d5588bb6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="basic-bindings"></a>Základní vazby

Datová vazba Xamarin.Forms propojí dvojici vlastností mezi dvěma objekty, alespoň jeden z nich je obvykle objekt uživatelského rozhraní. Tyto dva objekty se nazývají *cíl* a *zdroj*:

- *Cíl* je tento objekt (a vlastnost) na který je nastaven datové vazby.
- *Zdroj* je tento objekt (a vlastnost), odkazuje datová vazba.

Tento rozdíl v některých případech může být trochu matoucí: V nejjednodušším případě data proudí ze zdroje do cíle, což znamená, že je hodnota vlastnosti cílového nastavena z hodnoty vlastnosti zdroje. Ale v některých případech můžete případně toku dat z cíle ke zdroji nebo v obou směrech. Pokud chcete předejít nejasnostem, mějte na paměti, že cíl je vždy objekt, na kterém je datová vazba nastavena i v případě, že poskytuje data místo bude přijímá data.

## <a name="bindings-with-a-binding-context"></a>Vazby s kontextem vazby

Datové vazby jsou zadané obvykle zcela v jazyce XAML, sice významné zobrazíte datové vazby v kódu. **Základní vazby kódu** stránka obsahuje soubor XAML s `Label` a `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` Je nastaven pro řadu 0 až 360. Tohoto programu je cílem otočit `Label` manipulací `Slider`.

Bez vazby na data, byste měli nastavit `ValueChanged` události `Slider` k obslužné rutině událostí, který přistupuje k `Value` vlastnost `Slider` a nastaví tuto hodnotu `Rotation` vlastnost `Label`. Datová vazba automatizuje tuto úlohu; obslužné rutiny události a kód v něm již nejsou potřebné. 

Můžete nastavit vazbu na instanci třídy, která je odvozena z [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), což zahrnuje `Element`, `VisualElement`, `View`, a `View` odvozené konfigurace.  Vazba je vždycky nastavený na cílový objekt. Vazba odkazuje na zdrojový objekt. Pokud chcete nastavit vazby na data, použijte následující dva členy cílové třídy:

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Vlastnost určuje zdrojový objekt.
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Metoda určuje vlastnost target a source – vlastnost.

V tomto příkladu `Label` je cílem vazby a `Slider` je zdrojem vazby. Změny v `Slider` zdroj ovlivnit natočení `Label` cíl. Toky dat ze zdroje do cíle.

`SetBinding` Metoda definované `BindableObject` má argument typu [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) ze kterého [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) třída odvozena, ale existují další `SetBinding` metody definované [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) třídy. Soubor kódu v **základní vazby kódu** ukázce se používá jednodušší [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) metoda rozšíření z této třídy.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

`Label` Objektu je cílem vazby, který je objekt, na kterém je tato vlastnost nastavena a na kterém je volána metoda. `BindingContext` Vlastnost určuje vazby zdroj, který je `Slider`.

`SetBinding` Metoda je volána v cíli vazby, ale určuje vlastnost target i pro vlastnost zdroje. Vlastnost target je zadán jako `BindableProperty` objekt: `Label.RotationProperty`. Zdrojová vlastnost je zadán jako řetězec a označuje `Value` vlastnost `Slider`. 

`SetBinding` Metoda zjistí jedním z nejdůležitějších pravidel vazeb dat:

*Vlastnost target musí být zálohovaný pomocí vazbu vlastnosti.*

Toto pravidlo znamená, že cílový objekt musí být instancí třídy, která je odvozena z `BindableObject`. Najdete v článku [ **vazbu vlastnosti** ](~/xamarin-forms/xaml/bindable-properties.md) článku Přehled vazbu objektů a vazbu vlastnosti.

Neexistuje žádné takové pravidlo pro vlastnost zdroj, který je zadán jako řetězec. Reflexe se interně používá pro přístup skutečné vlastnost. V tomto případě, ale `Value` vlastnost je také zálohovaný pomocí vazbu vlastnosti.

Kód může být zjednodušené poněkud: `RotationProperty` je definována vazbu vlastnosti `VisualElement`a zdědí `Label` a `ContentPage` i, takže název třídy nevyžaduje v `SetBinding` volání:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Včetně název třídy je však dobré připomenutí cílového objektu.

Při manipulaci s `Slider`, `Label` otočí odpovídajícím způsobem:

[![Kód Basice vazby](basic-bindings-images/basiccodebinding-small.png "základní kódu vazby")](basic-bindings-images/basiccodebinding-large.png#lightbox "základní vazby")

**Základní vazby Xaml** stránky je stejný jako **základní vazby kód** s tím rozdílem, že definuje celé datové vazby v jazyce XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Stejně jako kódu, datové vazby u cílový objekt, který je nastavený `Label`. Se jedná o dvě rozšíření značek XAML. Toto jsou okamžitě rozpoznatelném ve složených závorek oddělovače:

- `x:Reference` – Rozšíření značek je potřeba odkazovat na zdrojový objekt, který je `Slider` s názvem `slider`.
- `Binding` Odkazy rozšíření značek `Rotation` vlastnost `Label` k `Value` vlastnost `Slider`. 

Najdete v článku [XAML – rozšíření značek](~/xamarin-forms/xaml/markup-extensions/index.md) Další informace o rozšíření značek XAML. `x:Reference` Podporuje – rozšíření značek [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) třídy; `Binding` podporuje [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) třídy. Jako soubor XML – předpony oboru názvů indikuje, `x:Reference` je součástí specifikace jazyka XAML 2009, zatímco `Binding` je součástí Xamarin.Forms. Všimněte si, že žádné uvozovky se objeví do složených závorek.

Je snadné zapomněli `x:Reference` – rozšíření značek při nastavení `BindingContext`. Je běžné omylem nastavte vlastnost přímo na název zdroje vazba takto:

```xaml
BindingContext="slider"
```

Ale není pravé. Nastaví tento kód `BindingContext` vlastnosti `string` objekt, jehož znaky pravopisu "posuvník"!

Všimněte si, že zdrojová vlastnost se zadaným [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) vlastnost `BindingExtension`, který odpovídá [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) vlastnost [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) třídy.

Kód zobrazí na **základní vazby XAML** stránky můžete zjednodušit: XAML – rozšíření značek, jako `x:Reference` a `Binding` může mít *obsahu vlastnost* definované atributy, které pro jazyk XAML rozšíření značek znamená, že název vlastnosti nemusí zobrazit. `Name` Vlastnost je vlastnost obsahu `x:Reference`a `Path` vlastnost je vlastnost obsahu `Binding`, což znamená, že jde je eliminovat z výrazů:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Vazby bez kontextu vazby

`BindingContext` Vlastnost je důležitou součástí vazeb data, ale není vždy nutné. Zdrojový objekt lze zadat místo toho v `SetBinding` volání nebo `Binding` – rozšíření značek.

Tento postup je znázorněn v **alternativní kód vazby** ukázka. Je podobná souboru XAML **základní vazby kódu** ukázkové vyjma toho, že `Slider` není definován pro ovládací prvek `Scale` vlastnost `Label`. Z tohoto důvodu `Slider` je nastaven pro řadu &ndash;2 až 2:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Nastaví vazbu s souboru kódu [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metoda definované `BindableObject`. Argument je [konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) pro [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) třídy:

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

`Binding` Konstruktor má 6 parametry, proto `source` parametr zadán s argumentem. Argument je `slider` objektu. 

Spuštění tohoto programu, může být trochu překvapivé:

[![Vazba alternativní kód](basic-bindings-images/alternativecodebinding-small.png "alternativní kód vazby")](basic-bindings-images/alternativecodebinding-large.png#lightbox "alternativní kód vazby")

Na obrazovce iOS na levé straně ukazuje, jak vypadá obrazovky po první zobrazení stránky. Kde je `Label`? 

Problém je, že `Slider` má počáteční hodnotu 0. To způsobí, že `Scale` vlastnost `Label` být také nastavena na 0, přepsání jeho výchozí hodnotu 1. Výsledkem `Label` se původně neviditelná. Jak ukazují na snímcích obrazovky Android a univerzální platformu Windows (UWP), můžete upravit `Slider` aby `Label` objeví znovu, ale její počáteční zrušení je zneklidňovat.

Dozvíte v [následující článek](binding-mode.md) jak tomuto problému nedošlo podle inicializace `Slider` z výchozí hodnota `Scale` vlastnost.

**Alternativní XAML vazby** stránka zobrazuje ekvivalentní vazby zcela v jazyce XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Nyní `Binding` – rozšíření značek má dvě vlastnosti nastavena, `Source` a `Path`, oddělených čárkou. Mohou se zobrazit na stejné přímce. Pokud dáváte přednost:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` Je nastavena na embedded `x:Reference` – rozšíření značek, jinak se stejnou syntaxí, jako nastavení `BindingContext`. Všimněte si, že žádné uvozovky se objeví do složených závorek a že dvě vlastnosti musí být odděleny čárkami.

Vlastnost obsahu `Binding` – rozšíření značek je `Path`, ale `Path=` součástí rozšíření značek lze odstranit pouze pokud je první vlastnost ve výrazu. Omezit `Path=` část, potřebujete Prohodit dvě vlastnosti:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

I když XAML – rozšíření značek jsou obvykle oddělená složené závorky, může se také vyjádřený jako objekt prvky:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
``` 

Nyní `Source` a `Path` vlastnosti jsou regulární atributy XAML: hodnoty jsou uvedeny v uvozovkách a atributy nejsou oddělených čárkou. `x:Reference` – Rozšíření značek se může stát také element objektu:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

Tuto syntaxi není běžné, ale v některých případech je nutné při se podílejí na komplexní objekty.

Příklady uvedené dosavadní nastavit `BindingContext` vlastnost a `Source` vlastnost `Binding` k `x:Reference` – rozšíření značek k odkazování jiného zobrazení na stránce. Tyto dvě vlastnosti se typu `Object`, a můžete je nastavit pro libovolný objekt, který obsahuje vlastnosti, které jsou vhodné pro vytvoření vazby zdroje. 

V článcích dopředu, zjistíte, že můžete nastavit `BindingContext` nebo `Source` vlastnost, která má `x:Static` – rozšíření značek Chcete-li hodnota statickou vlastnost nebo pole, nebo `StaticResource` – rozšíření značek k odkazování uložené v objektu slovník prostředků, nebo přímo na objekt, který je obvykle (ale ne vždy) instance ViewModel. 

`BindingContext` Vlastnost může být také nastavena na `Binding` objektu tak, aby `Source` a `Path` vlastnosti `Binding` definovat kontext vazby.

## <a name="binding-context-inheritance"></a>Dědičnost kontext vazby

V tomto článku jste se seznámili, můžete zadat zdrojový objekt pomocí `BindingContext` vlastnost nebo `Source` vlastnost `Binding` objektu. Pokud jsou obě nastaveny, `Source` vlastnost `Binding` má přednost před `BindingContext`.

`BindingContext` Vlastnost má velmi důležitou vlastností:

*Nastavení jazyka `BindingContext` vlastnost je zděděná prostřednictvím vizuálním stromu.*

Jak zjistíte, to může být velmi užitečný pro zjednodušenou výrazy vazby a v některých případech &mdash; zvlášť ve Model-View-ViewModel (modelem MVVM) scénářích &mdash; je nezbytné.

**Dědičnosti kontext vazby** ukázka je jednoduchá ukázka dědičnosti třídy kontextu vazby:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">
            
            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider" 
                Maximum="360" />
        
    </StackLayout>
</ContentPage>
```

`BindingContext` Vlastnost `StackLayout` je nastaven na `slider` objektu. Tento kontext vazby zdědí i `Label` a `BoxView`, které mít jejich `Rotation` vlastnosti nastavit na `Value` vlastnost `Slider`: 

[![Vazba kontextu dědičnosti](basic-bindings-images/bindingcontextinheritance-small.png "vazby kontextu dědičnosti")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "vazby kontextu dědičnosti")

V [následující článek](binding-mode.md), uvidíte jak *vazby režimu* tok dat mezi zdrojové a cílové objekty, můžete změnit.


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Kapitola vazby dat z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
