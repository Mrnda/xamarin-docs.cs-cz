---
title: Úvod do vlastní Renderery
description: Tento článek obsahuje úvod do vlastní renderery a popisuje proces pro vytvoření vlastní zobrazovací jednotky.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 180a196e0b95854815c8a74ef1d2df63407dd04f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998001"
---
# <a name="introduction-to-custom-renderers"></a>Úvod do vlastní Renderery

_Vlastní renderery poskytují výkonný přístup pro přizpůsobení vzhledu a chování ovládacích prvků Xamarin.Forms. Je možné použít pro používání stylů pro malé změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování. Tento článek obsahuje úvod do vlastní renderery a popisuje proces pro vytvoření vlastní zobrazovací jednotky._

Xamarin.Forms [rozložení stránky a ovládací prvky](~/xamarin-forms/user-interface/controls/index.md) prezentovat společné rozhraní API popsat mobilních multiplatformních uživatelských rozhraní. Jednotlivé stránky, rozložení a ovládací prvek je vykreslen jinak na jednotlivých platformách pomocí `Renderer` třídu, která se pak vytvoří ovládací prvek nativní (odpovídající reprezentaci Xamarin.Forms), uspořádá na obrazovce a přidá chování určené v sdílený kód.

Vývojáři můžou implementovat vlastní vlastní `Renderer` třídy k přizpůsobení vzhledu a chování ovládacího prvku. Vlastní renderery pro daný typ lze přidat do projektu jednu aplikaci zároveň umožní výchozí chování na jiných platformách; přizpůsobení ovládacího prvku na jednom místě nebo jiný vlastní renderery lze přidat do každého projektu aplikace na iOS, Android a univerzální platformu Windows (UPW) vytvořit jiný vzhled a chování. Implementující třída vlastního rendereru provádět přizpůsobení jednoduchých ovládacího prvku je však často odpověď náročné. Účinky zjednodušení tohoto procesu a jsou obvykle používány pro používání stylů pro malé změny. Další informace najdete v tématu [účinky](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Potřeby se zkušební proč vlastními Renderery

Změna vzhledu ovládacího prvku Xamarin.Forms, bez použití vlastní zobrazovací jednotky je dvoustupňový proces, který zahrnuje vytvoření vlastního ovládacího prvku pomocí vytvoření podtřídy a následného využití vlastního ovládacího prvku místo původního ovládacího prvku. Následující příklad kódu ukazuje příklad vytváření podtříd `Entry` ovládacího prvku:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` Je ovládací prvek `Entry` řídí umístění `BackgroundColor` je nastavena na šedé a může být odkazováno v jazyce Xaml pomocí deklarace oboru názvů pro jeho umístění a pomocí předpony oboru názvů na ovládací prvek. Následující příklad kódu ukazuje jak `MyEntry` vlastního ovládacího prvku mohou být využívány službou `ContentPage`:

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Předponu oboru názvů může být cokoli. Ale `namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, předpona, která slouží k odkazování na vlastní ovládací prvek.

> [!NOTE]
> Definování `xmlns` je v .NET Standard projekty knihovny mnohem jednodušší než sdílené projekty. Knihovny .NET Standard se zkompiluje do sestavení tak, aby byl snadno zjistit, co `assembly=CustomRenderer` hodnota by měla být. Při použití sdílené projekty, všechny sdílené prostředky (včetně XAML) jsou zkompilovány do každé odkazujících projektů, což znamená, že pokud iOS, Android a UPW projekty mají svůj vlastní *názvy sestavení* není možné zapsat `xmlns` deklarace vzhledem k tomu, hodnota musí být různé pro jednotlivé aplikace. Každý projekt aplikace nakonfigurovat se stejným názvem sestavení bude vyžadovat vlastní ovládací prvky v XAML pro sdílené projekty.

`MyEntry` Vlastního ovládacího prvku se pak vykreslí na jednotlivých platformách, na šedém pozadí, jak je znázorněno na následujících snímcích obrazovky:

![](introduction-images/screenshots.png "MyEntry vlastní ovládací prvek na jednotlivých platformách")

Změna barvy pozadí ovládacího prvku na jednotlivých platformách bylo dosaženo výhradně pomocí vytvoření podtřídy ovládacího prvku. Tento postup je však omezenou co může dosáhnout jako není možné využít k přizpůsobení a rozšíření specifické pro platformu. Když jsou povinné, musí implementovat vlastní renderery.

## <a name="creating-a-custom-renderer-class"></a>Vytvoření vlastního Rendereru třídy

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořte podtřídu třídy renderer, který vykreslí nativní ovládací prvek.
1. Přepsání metody, která vykreslí nativní ovládací prvek a zapisovat logiku přizpůsobení ovládacího prvku. Často se stává `OnElementChanged` metody slouží k vykreslení nativní ovládací prvek, který se volá, když se vytvoří odpovídající ovládací prvek Xamarin.Forms.
1. Přidat `ExportRenderer` atribut třídy vlastní zobrazovací jednotky a určit tak, že bude používat k vykreslení ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelný poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není zaregistrovaný vlastní zobrazovací jednotky, se používá výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Nicméně vlastní renderery jsou nutné v každém projektu platformy při vykreslování [zobrazení](xref:Xamarin.Forms.View) nebo [ViewCell](xref:Xamarin.Forms.ViewCell) elementu.

Témata v této sérii poskytne ukázek a vysvětlení tohoto procesu pro různé prvky Xamarin.Forms.

## <a name="troubleshooting"></a>Poradce při potížích

Pokud je součástí vlastního ovládacího prvku projekt .NET Standard knihovny, který byl přidán do řešení (tedy ne knihovny .NET Standard vytvořili pomocí sady Visual Studio pro Mac/Visual Studio Xamarin.Forms App šablony projektu), výjimka může dojít v iOS při došlo k pokusu o přístup k vlastní ovládací prvek. Pokud k tomuto problému dochází, se dá vyřešit vytvořením odkazu na vlastní ovládací prvek z `AppDelegate` třídy:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

To vynutí, aby kompilátor rozpoznával `ClassInPCL` typ podle jeho řešení. Další možností `Preserve` atribut lze přidat do `AppDelegate` třídy k dosažení stejného výsledku:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Tím se vytvoří odkaz na `ClassInPCL` typ označující, že je to požadováno za běhu. Další informace najdete v tématu [zachování kódu](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Souhrn

Tento článek poskytuje úvod do vlastní renderery a má uvedených proces pro vytvoření vlastní zobrazovací jednotky. Vlastní renderery poskytují výkonný přístup pro přizpůsobení vzhledu a chování ovládacích prvků Xamarin.Forms. Je možné použít pro používání stylů pro malé změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování.


## <a name="related-links"></a>Související odkazy

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
