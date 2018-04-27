---
title: Přizpůsobení položku
description: Ovládací prvek Xamarin.Forms položka umožňuje jeden řádek textu k provádění úprav. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro ovládací prvek položku, umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: c120add5a301e440911bd9794da77732e7787cc0
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="customizing-an-entry"></a>Přizpůsobení položku

_Ovládací prvek Xamarin.Forms položka umožňuje jeden řádek textu k provádění úprav. Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro ovládací prvek položku, umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu._

Každý prvek Xamarin.Forms musí doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) vykreslení ovládacího prvku platformě Xamarin.Forms aplikací v iOS `EntryRenderer` vytvoření instance třídy, což na oplátku vytvoří nativní `UITextField` ovládacího prvku. Na platformě Android `EntryRenderer` vytvoří instanci třídy `EditText` ovládacího prvku. Na univerzální platformu Windows (UWP), `EntryRenderer` vytvoří instanci třídy `TextBox` ovládacího prvku. Další informace o zobrazovací jednotky a třídy nativní ovládacích prvků, které ovládací prvky Xamarin.Forms mapování na najdete v tématu [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení a odpovídající nativní ovládací prvky, které implementují ho:

![](entry-images/entry-classes.png "Vztah mezi položka řízení a implementace nativní ovládací prvky")

Proces vykreslování můžete provedeny výhod implementovat přizpůsobení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_Entry_Control) Xamarin.Forms vlastního ovládacího prvku.
1. [Využívat](#Consuming_the_Custom_Control) vlastní ovládací prvek z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastní zobrazovací jednotky pro ovládací prvek na každou platformu.

Každá položka teď probereme pak implementovat [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládací prvek, který má jinou barvu pozadí na každou platformu.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Vytvoření ovládacího prvku vlastní položky

Vlastní [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení lze vytvořit pomocí vytváření podtříd `Entry` řídit, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` Řízení je vytvořen v projektu knihovny (PCL) přenosných tříd a je jednoduše k [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ovládacího prvku. Přizpůsobení ovládacího prvku budou provedeny v vlastní zobrazovací jednotky, aby žádné další implementace je vyžadována v `MyEntry` ovládacího prvku.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Použití vlastního ovládacího prvku

`MyEntry` Řízení může odkazovat v jazyce XAML v projektu PCL deklarace oboru názvů pro umístění a použití Předpona oboru názvů na elementu ovládacího prvku. Následující příklad kódu ukazuje jak `MyEntry` řízení mohou být spotřebovávána stránky XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Předponu oboru názvů můžete pojmenovat. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů je předpona, která slouží k odkazování vlastního ovládacího prvku.

Následující příklad kódu ukazuje jak `MyEntry` řízení mohou být spotřebovávána stránky C#:

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

Tento kód vytvoří novou [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objekt, který se zobrazí [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) a `MyEntry` řízení obě vodorovně a svisle na střed stránky.

Vlastní zobrazovací jednotky lze nyní přidat na každý projekt aplikace k přizpůsobení vzhledu ovládacího prvku na každou platformu.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytváření vlastní zobrazovací jednotky na jednotlivých platformách

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvoření podtřídou třídy `EntryRenderer` třídu, která vykreslí nativní ovládací prvek.
1. Přepsání `OnElementChanged` metody, která vykreslí nativní logika řízení a zápisu pro přizpůsobení ovládacího prvku. Tato metoda je volána, když je vytvořen odpovídající ovládacího prvku Xamarin.Forms.
1. Přidat `ExportRenderer` atributu na vlastní zobrazovací jednotky třídu k určení, že bude použit k vykreslení ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> Zadání je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud vlastní zobrazovací jednotky není registrované, bude použit výchozí zobrazovací jednotky pro základní třídu ovládacího prvku.

Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](entry-images/solution-structure.png "MyEntry vlastní zobrazovací jednotky projektu odpovědnosti")

`MyEntry` Ovládací prvek je vykreslen metodou specifické pro platformu `MyEntryRenderer` třídy, které jsou odvozeny od `EntryRenderer` třídu pro každou platformu. Výsledkem je každý `MyEntry` řízení vykreslované barvou pozadí specifické pro platformu, jak je vidět na následujících snímcích obrazovky:

![](entry-images/screenshots.png "Ovládací prvek MyEntry na jednotlivých platformách")

`EntryRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když ovládacího prvku Xamarin.Forms se vytvoří pro vykreslení odpovídající nativní ovládacího prvku. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují Xamarin.Forms element, zobrazovací jednotky *byla* připojené a Xamarin.Forms element, zobrazovací jednotky *je* připojené k, v uvedeném pořadí. V ukázkové aplikaci `OldElement` , bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `MyEntry` ovládacího prvku.

Přepsané verzi `OnElementChanged` metoda v `MyEntryRenderer` třída je místo, kde provádět přizpůsobení nativní ovládací prvek. Zadaný odkaz na nativní ovládací prvek používá na platformě je možné přistupovat prostřednictvím `Control` vlastnost. Kromě toho nelze získat odkaz na platformě Xamarin.Forms ovládací prvek, který je vykreslované prostřednictvím `Element` vlastnost, i když se nepoužívá v ukázkové aplikaci.

Každá třída vlastní zobrazovací jednotky je upraven pomocí `ExportRenderer` atribut, který registruje zobrazovací jednotky s Xamarin.Forms. Atribut přebírá dva parametry – název typu ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která má atribut určuje atribut, které se vztahují na celou sestavení.

Následující části popisují implementaci každý specifické pro platformu `MyEntryRenderer` třída vlastní zobrazovací jednotky.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

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

Volání základní třídy `OnElementChanged` metoda vytvoří instanci iOS `UITextField` ovládací prvek s odkazem na přiřazeny vykreslení ovládacího prvku `Control` vlastnost. Barva pozadí pak nastavena na světla fialový s `UIColor.FromRGB` metoda.

### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

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

Volání základní třídy `OnElementChanged` metoda vytvoří instanci Android `EditText` ovládací prvek s odkazem na přiřazeny vykreslení ovládacího prvku `Control` vlastnost. Barva pozadí je pak nastavena na hodnotu světle zelená s `Control.SetBackgroundColor` metoda.

### <a name="creating-the-custom-renderer-on-uwp"></a>Vytváření vlastní zobrazovací jednotky na UWP

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

Volání základní třídy `OnElementChanged` metoda vytvoří instanci `TextBox` ovládací prvek s odkazem na přiřazeny vykreslení ovládacího prvku `Control` vlastnost. Barva pozadí je pak nastavena na hodnotu azurová vytvořením `SolidColorBrush` instance.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit vlastního ovládacího prvku renderer pro platformě Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) řízení, umožňuje vývojářům přepsat výchozí nativní vykreslování s vykreslováním vlastní specifické pro platformu. Vlastní nástroji pro vykreslování poskytují výkonný přístup k přizpůsobení vzhledu ovládacího prvku Xamarin.Forms. Mohou být použity pro malé stylů změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování.


## <a name="related-links"></a>Související odkazy

- [CustomRendererEntry (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
