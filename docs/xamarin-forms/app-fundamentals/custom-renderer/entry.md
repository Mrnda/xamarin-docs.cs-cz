---
title: Přizpůsobení položky
description: Ovládací prvek položky Xamarin.Forms umožňuje jeden řádek textu má být upraven. Tento článek ukazuje, jak vytvořit vlastního rendereru pro ovládací prvek vstupu, umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 30326b8d52f39268015bdcbee1b84b9d9e5516b9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998957"
---
# <a name="customizing-an-entry"></a>Přizpůsobení položky

_Ovládací prvek položky Xamarin.Forms umožňuje jeden řádek textu má být upraven. Tento článek ukazuje, jak vytvořit vlastního rendereru pro ovládací prvek vstupu, umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu._

Každý ovládací prvek Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `Entry` ](xref:Xamarin.Forms.Entry) aplikací Xamarin.Forms v Iosu se vykreslí ovládací prvek `EntryRenderer` je vytvořena instance třídy, což na oplátku vytvoří instanci nativní `UITextField` ovládacího prvku. Na platformu Android `EntryRenderer` vytvoří instanci třídy `EditText` ovládacího prvku. Na Universal Windows Platform (UWP), `EntryRenderer` vytvoří instanci třídy `TextBox` ovládacího prvku. Další informace o nástroj pro vykreslování a nativní ovládací prvek třídy, které ovládací prvky Xamarin.Forms namapovat na najdete v tématu [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku a odpovídající nativní ovládací prvky, které je implementují:

![](entry-images/entry-classes.png "Vztah mezi vstupního ovládacího prvku a implementace nativní ovládací prvky")

Samotný proces vykreslování děláte výhod pro implementaci vlastního nastavení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_Entry_Control) Xamarin.Forms vlastního ovládacího prvku.
1. [Využívání](#Consuming_the_Custom_Control) vlastního ovládacího prvku z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastního rendereru pro ovládací prvek na jednotlivých platformách.

Každá položka nyní probereme zase k implementaci [ `Entry` ](xref:Xamarin.Forms.Entry) ovládací prvek, který má jinou barvu pozadí na jednotlivých platformách.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Vytvoření vlastní položky ovládacího prvku

Vlastní [ `Entry` ](xref:Xamarin.Forms.Entry) ovládací prvek může být vytvořen vytváření podtříd `Entry` řídit, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` Prvek je vytvořen v projektu knihovny .NET Standard, je jednoduše k [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku. Přizpůsobení ovládacího prvku budou prováděny ve vlastní zobrazovací jednotky, tak se vyžaduje další implementaci `MyEntry` ovládacího prvku.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Používání vlastního ovládacího prvku

`MyEntry` Ovládací prvek může být odkazováno v XAML v projektu knihovny .NET Standard deklarace oboru názvů pro jeho umístění a použitím předponu oboru názvů na ovládací prvek. Následující příklad kódu ukazuje jak `MyEntry` ovládacího prvku mohou být spotřebovány stránky XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Předponu oboru názvů může být název cokoli. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů předpona, která slouží k odkazování na vlastní ovládací prvek.

Následující příklad kódu ukazuje jak `MyEntry` ovládacího prvku mohou být spotřebovány stránky jazyka C#:

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Tento kód vytvoří novou [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objekt, který se zobrazí [ `Label` ](xref:Xamarin.Forms.Label) a `MyEntry` ovládací prvek obě vodorovně a svisle na střed stránky.

Vlastní zobrazovací jednotky je nyní přidat do každého projektu aplikace pro přizpůsobení vzhledu ovládacího prvku na jednotlivých platformách.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytvoření vlastního Rendereru na jednotlivých platformách

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořit podtřídu `EntryRenderer` třídu, která vykreslí nativní ovládací prvek.
1. Přepsat `OnElementChanged` metody, která vykreslí nativní ovládací prvek a zapisovat logiku pro přizpůsobení ovládacího prvku. Tato metoda je volána, když se vytvoří odpovídající ovládací prvek Xamarin.Forms.
1. Přidat `ExportRenderer` atribut třídy vlastní zobrazovací jednotky a určit tak, že bude používat k vykreslení ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Zadání je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není zaregistrovaný vlastní zobrazovací jednotky, se používá výchozí zobrazovací jednotky pro základní třídu ovládacího prvku.

Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](entry-images/solution-structure.png "MyEntry vlastní zobrazovací jednotky projektu odpovědnosti")

`MyEntry` Ovládací prvek je vykreslen metodou specifické pro platformu `MyEntryRenderer` třídy, které jsou odvozeny z `EntryRenderer` třídy pro každou platformu. Výsledkem je každý `MyEntry` řídit vykreslované barvou pozadí pro konkrétní platformu, jak je znázorněno na následujících snímcích obrazovky:

![](entry-images/screenshots.png "Ovládací prvek MyEntry na jednotlivých platformách")

`EntryRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když ovládací prvek Xamarin.Forms je vytvořen pro vykreslení odpovídající nativní ovládací prvek. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují elementu Xamarin.Forms, která zobrazovací jednotky *byl* připojené a Xamarin.Forms element, která zobrazovací jednotky *je* připojené položky v uvedeném pořadí. V ukázkové aplikaci `OldElement` bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `MyEntry` ovládacího prvku.

Přepsané verzi `OnElementChanged` metodu `MyEntryRenderer` třída je místem, kde můžete provádět přizpůsobení nativní ovládacího prvku. Zadaný odkaz na nativní ovládací prvek se používají na platformě je přístupná prostřednictvím `Control` vlastnost. Kromě toho lze získat odkaz na ovládací prvek Xamarin.Forms, která se vykresluje přes `Element` vlastnost, i když se nepoužívá v ukázkové aplikaci.

Každá třída vlastní zobrazovací jednotky je doplněn `ExportRenderer` atribut, který registruje vykreslovaný pomocí Xamarin.Forms. Atribut přebírá dva parametry – název typu ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která atribut určuje, že platí atribut pro celé sestavení.

Následující části popisují implementaci každého specifické pro platformu `MyEntryRenderer` třídy vlastní zobrazovací jednotky.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu iOS:

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci iOS `UITextField` ovládacího prvku s odkazem na přiřazení k rendereru pro ovládací prvek `Control` vlastnost. Barva pozadí pak nastavena na světle fialový s `UIColor.FromRGB` metody.

### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu Android:

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci aplikace pro Android `EditText` ovládacího prvku s odkazem na přiřazení k rendereru pro ovládací prvek `Control` vlastnost. Barva pozadí je nastaven na světle zelená s `Control.SetBackgroundColor` metody.

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytvoření vlastního Rendereru na UPW

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro UPW:

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

Volání základní třídy `OnElementChanged` metoda vytvoří instanci `TextBox` ovládacího prvku s odkazem na přiřazení k rendereru pro ovládací prvek `Control` vlastnost. Barva pozadí je nastaven na azurová tak, že vytvoříte `SolidColorBrush` instance.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastní ovládací prvek rendereru pro Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) ovládacího prvku, umožňuje vývojářům přepsat výchozí nativní vykreslení s vykreslováním vlastní specifické pro platformu. Vlastní renderery poskytují výkonný přístup k přizpůsobení vzhledu ovládacího prvku Xamarin.Forms. Je možné použít pro používání stylů pro malé změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování.


## <a name="related-links"></a>Související odkazy

- [CustomRendererEntry (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
