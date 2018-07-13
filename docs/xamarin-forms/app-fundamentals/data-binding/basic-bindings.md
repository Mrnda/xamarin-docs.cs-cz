---
title: Vazby Xamarin.Forms Basic
description: Tento článek vysvětluje, jak pomocí Xamarin.Forms datové vazby, která odkazuje alespoň dvojici vlastností mezi dvěma objekty, z nichž jeden je obvykle objekt uživatelského rozhraní. Tyto dva objekty se nazývají cíl a zdroj.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 16d1970b5e9d8f9c2b7c8be875c81136525c4fb7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998069"
---
# <a name="xamarinforms-basic-bindings"></a>Vazby Xamarin.Forms Basic

Datová vazba Xamarin.Forms propojí dvojici vlastností mezi dvěma objekty nejmíň jedno z nich je obvykle objekt uživatelského rozhraní. Tyto dva objekty jsou volány *cílové* a *zdroj*:

- *Cílové* je objekt (a vlastností) na datové vazby je nastavena.
- *Zdroj* je objekt (a vlastností) odkazuje datovou vazbu.

Toto rozlišení někdy může být trochu matoucí: V nejjednodušším případě data proudí ze zdroje do cíle, což znamená, že je nastavena hodnota vlastnosti cílové z hodnoty vlastnosti zdroje. Ale v některých případech můžete případně toku dat z cíle ke zdroji nebo v obou směrech. Aby nedocházelo k záměně, nezapomínejte, že cíl je vždy objektu, na kterém je nastavena datové vazby i v případě, je poskytování dat spíše přijímá data.

## <a name="bindings-with-a-binding-context"></a>Vazby s kontextem vazby

I když datové vazby jsou obvykle určené výhradně v XAML, je poučné naleznete v tématu datové vazby v kódu. **Vazby základní kód** stránky obsahuje soubor XAML `Label` a `Slider`:

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

`Slider` Nastavený pro rozsah 0 až 360. Cílem tohoto programu je otočit `Label` manipulací `Slider`.

Bez vazby dat, nastavíte `ValueChanged` událost `Slider` pro obslužnou rutinu události, který přistupuje k `Value` vlastnost `Slider` a nastaví tuto hodnotu na `Rotation` vlastnost `Label`. Datová vazba automatizuje úlohy; Obslužná rutina události a kód v ní už nejsou potřebné.

Můžete nastavit vazbu na instanci třídy, která je odvozena od [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), což zahrnuje `Element`, `VisualElement`, `View`, a `View` vy.  Vazba je vždycky nastavený na cílovém objektu. Vazba odkazuje na zdrojový objekt. K nastavení datové vazby, použijte následující dva členy třídy cíle:

- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Vlastnost určuje zdrojový objekt.
- [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Metody určuje vlastnost target a source – vlastnost.

V tomto příkladu `Label` je cíl vazby a `Slider` je zdroj vazby. Se změnami `Slider` zdroj ovlivnit otáčení `Label` cíl. Toky dat ze zdroje do cíle.

`SetBinding` Metody definované `BindableObject` má argument typu [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) ze kterého [ `Binding` ](xref:Xamarin.Forms.Binding) třída odvozena, ale existují další `SetBinding` metody určené [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) třídy. Soubor kódu na pozadí v **základní kód vazby** ukázce se používá jednodušší [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) rozšiřující metoda z této třídy.

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

`Label` Objekt je cíl vazby tak, aby se objektu, na kterém je tato vlastnost nastavena a na kterém je volána metoda. `BindingContext` Vlastnost určuje zdroj vazby, který je `Slider`.

`SetBinding` Metoda je volána na cíl vazby, ale určuje vlastnost target a vlastnost source. Vlastnost target je zadán jako `BindableProperty` objektu: `Label.RotationProperty`. Vlastnost source je zadán jako řetězec a označuje, `Value` vlastnost `Slider`.

`SetBinding` Metoda zobrazí jedno z vašich nejdůležitějších pravidel datové vazby:

*Vlastnost target musí být podporovaný službou vázanou vlastnost.*

Toto pravidlo předpokládá, že cílový objekt musí být instancí třídy, která je odvozena z `BindableObject`. Zobrazit [ **vlastnosti umožňující vazbu** ](~/xamarin-forms/xaml/bindable-properties.md) článku Přehled vytvořil objekty a vlastnosti umožňující vazbu.

Neexistuje žádné takové pravidlo pro vlastnost source, který je zadán jako řetězec. Reflexe interně, slouží k přístupu k skutečné vlastnost. V tomto konkrétním případě však `Value` vlastnost je také založená na vlastnost s vazbou.

Kód může být trochu zjednodušená: `RotationProperty` vázanou vlastnost je definován `VisualElement`a dědí `Label` a `ContentPage` stejně, takže název třídy nevyžadoval `SetBinding` volání:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Včetně názvu třídy je však vhodné připomenutí cílového objektu.

Při manipulaci s `Slider`, `Label` otočí odpovídajícím způsobem:

[![Kód Basice vazby](basic-bindings-images/basiccodebinding-small.png "základní kód vazby")](basic-bindings-images/basiccodebinding-large.png#lightbox "základní vazby")

**Základní vazby Xaml** stránky je stejný jako **základní kód vazby** s tím rozdílem, že definuje celé datové vazby v XAML:

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

Stejně jako u kódu, datové vazby je nastavena na cílový objekt, který je `Label`. Se podílejí dvě rozšíření značek XAML. Jedná se pozná okamžitě podle oddělovače složených závorek:

- `x:Reference` – Rozšíření značek je vyžadovaný pro odkaz na zdrojový objekt, který je `Slider` s názvem `slider`.
- `Binding` Odkazy na rozšíření značek `Rotation` vlastnost `Label` k `Value` vlastnost `Slider`.

Přečtěte si článek [– rozšíření značek XAML](~/xamarin-forms/xaml/markup-extensions/index.md) Další informace o rozšíření značek XAML. `x:Reference` – Rozšíření značek je podporován [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) třídy; `Binding` je podporován [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) třídy. Předpony oboru názvů označit jako soubor XML, `x:Reference` je součástí specifikace XAML 2009, zatímco `Binding` je součástí Xamarin.Forms. Všimněte si, že žádné uvozovky uvnitř složených závorek objevit.

Je snadné zapomenout `x:Reference` – rozšíření značek při nastavení `BindingContext`. Je běžné omylem nastavte vlastnost přímo na název zdroje připojení následujícím způsobem:

```xaml
BindingContext="slider"
```

Ale to není správné. Tento kód nastaví `BindingContext` vlastnost `string` objekt, jehož znaků pravopisu "posuvníku"!

Všimněte si, že je zadána vlastnost zdroje s [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) vlastnost `BindingExtension`, který odpovídá [ `Path` ](xref:Xamarin.Forms.Binding.Path) vlastnost [ `Binding` ](xref:Xamarin.Forms.Binding) třídy.

Kód zobrazený na **základní vazby XAML** stránky se dá zjednodušit: rozšíření značek XAML, jako `x:Reference` a `Binding` může mít *Vlastnost ContentProperty* definované atributy, které je pro XAML – rozšíření značek znamená, že název vlastnosti nemusí zobrazit. `Name` Vlastností je vlastnost content `x:Reference`a `Path` vlastností je vlastnost content `Binding`, což znamená, že může být odstraněny z výrazů:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Vazby bez kontextu vazby

`BindingContext` Vlastnost je jejich důležitou součástí nad vázáním dat, ale není vždy nutné. Zdrojový objekt lze zadat místo toho `SetBinding` volání nebo `Binding` – rozšíření značek.

To je patrné **alternativní vazby kód** vzorku. Soubor XAML je podobný **základní kód vazby** ukázkový s výjimkou, že `Slider` je definován na ovládací prvek `Scale` vlastnost `Label`. Z tohoto důvodu `Slider` nastavena pro celou řadu &ndash;2 až 2:

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

Použití modelu code-behind souboru nastaví vazbu s [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metody definované `BindableObject`. Argument je [konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) pro [ `Binding` ](xref:Xamarin.Forms.Binding) třídy:

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

`Binding` Konstruktor má 6 parametrů, proto `source` parametr není zadán s pojmenovaný argument. Argument je `slider` objektu.

Spuštění tohoto programu může být trochu překvapivé:

[![Alternativní kód vazby](basic-bindings-images/alternativecodebinding-small.png "alternativní kód vazby")](basic-bindings-images/alternativecodebinding-large.png#lightbox "alternativní kód vazby")

Na obrazovce iOS na levé straně se zobrazí, vzhled obrazovky, když se nejprve zobrazí na stránce. Pokud je `Label`?

Problém je, že `Slider` má počáteční hodnotu 0. To způsobí, že `Scale` vlastnost `Label` být také nastavena na 0, přepíše jeho výchozí hodnotu 1. Výsledkem `Label` se zpočátku neviditelné. Jak ukazují, snímky obrazovky pro Android a univerzální platformu Windows (UPW), můžete upravit `Slider` provést `Label` zobrazí znovu, ale jeho počáteční zmizení je zneklidňovat.

Zjistíte v [dalšímu článku](binding-mode.md) jak tomuto problému zabráníte tak inicializace `Slider` z výchozí hodnoty `Scale` vlastnost.

**Alternativní XAML vazby** stránka zobrazuje ekvivalentní vazby zcela XAML:

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

Nyní `Binding` – rozšíření značek má dvě vlastnosti nastavit, `Source` a `Path`, oddělená čárkou. Na stejném řádku se může zobrazit, pokud dáváte přednost:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` Je nastavena na vložený `x:Reference` rozšíření značek, které mají stejnou syntaxi jako nastavení `BindingContext`. Všimněte si, že žádné uvozovky uvnitř složených závorek objevit, a že dvě vlastnosti musí být odděleny čárkou.

Vlastnost obsahu `Binding` – rozšíření značek je `Path`, ale `Path=` součástí rozšíření značek lze odstranit pouze pokud je první vlastnost ve výrazu. Chcete-li odstranit `Path=` část, musíte se Prohodit dvě vlastnosti:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

I když rozšíření značek XAML jsou odděleny obvykle složených závorek, je také lze vyjádřit jako objekt prvky:

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

Nyní `Source` a `Path` vlastnosti jsou pravidelné atributy XAML: Zobrazí hodnoty v uvozovkách a atributy nejsou oddělené čárkou. `x:Reference` – Rozšíření značek zároveň může stát elementu objektu:

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

Tato syntaxe není běžné, ale v některých případech je nutné po složité objekty souvisejí.

Nastavte příkladů uvedených zatím `BindingContext` vlastnost a `Source` vlastnost `Binding` do `x:Reference` – rozšíření značek pro odkazovat na jiné zobrazení na stránce. Tyto dvě vlastnosti jsou typu `Object`, je možné nastavit na libovolný objekt, který obsahuje vlastnosti, které jsou vhodné pro vytvoření vazby zdroje.

V článcích dopředu, zjistíte, že můžete nastavit `BindingContext` nebo `Source` vlastnost `x:Static` – rozšíření značek tak, aby odkazovaly hodnotu statickou vlastnost nebo pole, nebo `StaticResource` – rozšíření značek k odkazování objekt uložený v slovník prostředků, nebo přímo na objekt, který je obvykle (ale ne vždy) instance ViewModel.

`BindingContext` Vlastnost může být také nastavena na `Binding` objektu tak, aby `Source` a `Path` vlastnosti `Binding` definovat kontextu vazby.

## <a name="binding-context-inheritance"></a>Dědičnost kontextu vazby

V tomto článku jste se seznámili, můžete zadat zdrojový objekt pomocí `BindingContext` vlastnost nebo `Source` vlastnost `Binding` objektu. Pokud obě nastaveny `Source` vlastnost `Binding` má přednost před `BindingContext`.

`BindingContext` Vlastnost má velmi důležitou vlastnost:

*Nastavení jazyka `BindingContext` vlastnost je zděděná prostřednictvím vizuálního stromu.*

Jak se zobrazí, to může být velmi užitečný pro zjednodušení vazbové výrazy a v některých případech &mdash; zejména ve scénářích Model-View-ViewModel (MVVM) &mdash; je nezbytné.

**Dědičnosti kontextu vazby** ukázka je jednoduché ukázku dědičnosti kontextu vazby:

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

`BindingContext` Vlastnost `StackLayout` je nastavena na `slider` objektu. Tento kontext vazby zdědí i `Label` a `BoxView`, obě sady, u nichž jejich `Rotation` nastaveny `Value` vlastnost `Slider`:

[![Vazba kontextu dědičnosti](basic-bindings-images/bindingcontextinheritance-small.png "dědičnosti kontextu vazby")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "vazby kontextu dědičnosti")

V [dalšímu článku](binding-mode.md), zobrazí se vám jak *vazby režimu* můžete změnit tok dat mezi zdrojové a cílové objektů.


## <a name="related-links"></a>Související odkazy

- [Ukázky vazby dat (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Data vazby kapitola z knihy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
