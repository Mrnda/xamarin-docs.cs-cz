---
title: "Úvod do vlastní nástroji pro vykreslování"
description: "Přizpůsobení vzhledu a chování ovládacích prvků Xamarin.Forms zadejte vlastní nástroji pro vykreslování efektivní přístup. Mohou být použity pro malé stylů změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování. Tento článek obsahuje úvod do vlastní nástroji pro vykreslování a popisuje proces pro vytvoření vlastní zobrazovací jednotky."
ms.topic: article
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 837d75bd4ecde92d4c375c680a5f5e7ff231f825
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-custom-renderers"></a>Úvod do vlastní nástroji pro vykreslování

_Přizpůsobení vzhledu a chování ovládacích prvků Xamarin.Forms zadejte vlastní nástroji pro vykreslování efektivní přístup. Mohou být použity pro malé stylů změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování. Tento článek obsahuje úvod do vlastní nástroji pro vykreslování a popisuje proces pro vytvoření vlastní zobrazovací jednotky._

Xamarin.Forms [rozložení stránky a ovládací prvky](~/xamarin-forms/user-interface/controls/index.md) k dispozici společné rozhraní API popisují různé platformy mobilních uživatelská rozhraní. Každé stránky, rozložení a řízení vykreslením jinak na jednotlivých platformách pomocí `Renderer` třídu, která zase vytvoří nativní ovládací prvek (odpovídající Xamarin.Forms zastupování), uspořádá na obrazovce a přidá zadané v chování sdílené kód.

Vývojářům můžete implementovat vlastní vlastní `Renderer` třídy k přizpůsobení vzhledu a chování ovládacího prvku. Vlastní nástroji pro vykreslování pro daný typ lze přidat do projektu jedné aplikace k přizpůsobení ovládacího prvku na jednom místě, zatímco výchozí chování na jiných platformách; nebo jiný vlastní nástroji pro vykreslování lze přidat na každý projekt aplikace vytvořit různé vzhled a chování na iOS, Android a Windows Phone. Implementace třídy vlastní zobrazovací jednotky k přizpůsobení jednoduchý ovládací prvek je však často zobrazené – odpověď. Účinky zjednodušení tohoto procesu a jsou obvykle používány pro malé stylů změny. Další informace najdete v tématu [důsledky](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Zkušební proč vlastní nástroji pro vykreslování jsou nezbytné

Změna vzhledu ovládacího prvku Xamarin.Forms, bez použití vlastní zobrazovací jednotky, je dvoustupňový proces, který zahrnuje vytvoření vlastního ovládacího prvku pomocí vytvoření podtřídy a potom využívají vlastního ovládacího prvku místo původního ovládacího prvku. Následující příklad kódu ukazuje příklad vytváření podtříd `Entry` ovládacího prvku:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` Ovládacího prvku `Entry` řídí umístění `BackgroundColor` je nastaven na šedé a může být odkazováno v Xaml deklarace oboru názvů pro umístění a použitím Předpona oboru názvů na elementu ovládacího prvku. Následující příklad kódu ukazuje jak `MyEntry` mohou být spotřebovávána vlastního ovládacího prvku `ContentPage`:

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

`local` Předponu oboru názvů můžete použít jakýkoli. Ale `namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastního ovládacího prvku. Jakmile je deklarován obor názvů, je předpona, která slouží k odkazování vlastního ovládacího prvku.

> [!NOTE]
> Definování `xmlns` v PCLs mnohem jednodušší než sdílených projektů. Kompiluje PCL do sestavení tak, aby byl snadno zjistit, co `assembly=CustomRenderer` hodnota by měla být. Při použití sdílených projektů, jsou všechny sdílené prostředky (včetně XAML) zkompilovány do každé odkazující projektů, které znamená, že pokud iOS, Android a Windows Phone projekty mají svůj vlastní *názvy sestavení* není možné o zápis `xmlns` deklarace vzhledem k tomu, že hodnota musí být různé pro jednotlivé aplikace. Vlastní ovládací prvky v jazyce XAML pro sdílených projektů bude vyžadovat každý projekt aplikace nakonfigurovat se stejným názvem sestavení.

`MyEntry` Vlastní vykreslení ovládacího prvku klikněte na každou platformu, barvou pozadí, jak je vidět na následujících snímcích obrazovky:

![](introduction-images/screenshots.png "MyEntry vlastní ovládací prvek na jednotlivých platformách")

Změna barvy pozadí ovládacího prvku na jednotlivých platformách má být provedeno výhradně prostřednictvím vytvoření podtřídy ovládacího prvku. Tento postup je však omezená co může dosáhnout jako není možné využívat výhod rozšíření specifické pro platformu a přizpůsobení. Pokud jsou požadována, musí být implementované vlastní nástroji pro vykreslování.

## <a name="creating-a-custom-renderer-class"></a>Vytvoření třídy vlastní zobrazovací jednotky

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvořte podtřídou třídy zobrazovací jednotky, která vykreslí nativní ovládací prvek.
1. Potlačí metodu, která vykreslí nativní ovládací prvek a zapisovat logiku pro přizpůsobení ovládacího prvku. Často `OnElementChanged` metoda se používá k vykreslení ovládacího prvku nativní, která je volána, když se vytvoří odpovídající ovládacího prvku Xamarin.Forms.
1. Přidat `ExportRenderer` atributu na vlastní zobrazovací jednotky třídu k určení, že bude použit k vykreslení ovládacího prvku Xamarin.Forms. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> Pro většinu prvků Xamarin.Forms je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud vlastní zobrazovací jednotky není registrované, bude použit výchozí zobrazovací jednotky pro základní třídu ovládacího prvku. Však vlastní nástroji pro vykreslování se vyžadují v každém projektu platformy při vykreslování [zobrazení](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) nebo [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) element.

Témata v této série vám poskytne předvádění a vysvětlení tohoto procesu pro různé prvky Xamarin.Forms.

## <a name="troubleshooting"></a>Poradce při potížích

Pokud PCL projekt, který je přidaný do řešení (tj. není PCL vytvořené sady Visual Studio pro šablony projektu aplikace Xamarin.Forms Mac/Visual Studio) je součástí vlastního ovládacího prvku, k výjimce může dojít v iOS při pokusu o přístup k vlastní ovládací prvek. Pokud tento problém nastane, dají se vyřešit vytvořením odkaz na vlastní ovládací prvek z `AppDelegate` třídy:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Vynutí se tak kompilátoru rozpoznat `ClassInPCL` typu vyřešte je. Případně `Preserve` atribut lze přidat do `AppDelegate` třídu k dosažení stejného výsledku:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Tím se vytvoří odkaz na `ClassInPCL` typu, která určuje, že je to požadováno, v době běhu. Další informace najdete v tématu [zachování kód](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Souhrn

Tento článek poskytl úvod do vlastní nástroji pro vykreslování a má uvedených proces vytvoření vlastní zobrazovací jednotky. Přizpůsobení vzhledu a chování ovládacích prvků Xamarin.Forms zadejte vlastní nástroji pro vykreslování efektivní přístup. Mohou být použity pro malé stylů změny nebo sofistikované rozložení specifické pro platformu a přizpůsobení chování.


## <a name="related-links"></a>Související odkazy

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
