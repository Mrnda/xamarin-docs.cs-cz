---
title: "Shrnutí kapitoly 12. Styly"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9563bc811250038e8932067280a8e5292a379077
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-12-styles"></a>Shrnutí kapitoly 12. Styly

Styly v Xamarin.Forms, umožní více zobrazení sdílet kolekce vlastností nastavení. To snižuje značek a umožňuje zachování konzistentní vizuální motivy.

Styly jsou téměř vždy definované a využívat v kódu. Objekt typu [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) je vytvořena instance v slovník prostředků a poté nastavte [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) vlastnost vizuální prvek použití `StaticResource` nebo `DyanamicResource` značek rozšíření.

## <a name="the-basic-style"></a>Základní styl

A `Style` vyžaduje, aby jeho [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) nastavit na typ visual objektu, se vztahuje na. Když `Style` je vytvořena instance ve slovníku prostředků (jako je běžné) taky vyžaduje `x:Key` atribut.

`Style` Obsahu vlastnost typu [ `Setters` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/), což je kolekce [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) objekty. Každý `Setter` přidruží [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) s [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/).

V jazyce XAML `Property` nastavení je název vlastnosti CLR (například `Text` vlastnost `Button`), ale vlastnost stylem musí být zajištěna vazbu vlastnosti. Navíc musí být definována vlastnost v třídě indikován `TargetType` nastavení nebo zděděná podle této třídy.

Můžete zadat `Value` nastavení pomocí elementu vlastnost `<Setter.Value>`. To umožňuje nastavit `Value` na objekt, který nelze vyjádřit v textovém řetězci, nebo na `OnPlatform` objektu nebo objekt vytvořit instance pomocí `x:Arguments` nebo `x:FactoryMethod`. `Value` Vlastnost lze nastavit také s `StaticResource` výraz, který se jiná položka ve slovníku.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) program ukazuje základní syntaxe a ukazuje, jak odkazovat `Style` s `StaticResource` – rozšíření značek:

[![Trojitá snímek obrazovky základní styl](images/ch12fg01-small.png "základní styly")](images/ch12fg01-large.png#lightbox "základní styly")

`Style` Objekt a libovolného objektu vytvořeny v `Style` objekt jako `Value` nastavení jsou sdílena mezi všechny zobrazení odkazující na, který `Style`. `Style` Nemůže obsahovat nic, které nemohou být sdíleny, například `View` odvozených.

Obslužné rutiny událostí nelze nastavit `Style`. `GestureRecognizers` Vlastnost nelze nastavit `Style` vzhledem k tomu, že není podporována pomocí vazbu vlastnosti.

## <a name="styles-in-code"></a>Styly v kódu

I když není společné, můžete vytvořit instanci a inicializujte `Style` objekty v kódu. Tento postup je znázorněn pomocí [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) ukázka.

## <a name="style-inheritance"></a>Styl dědičnosti

`Style` má [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) vlastnost, která můžete nastavit, aby `StaticResource` – rozšíření značek odkazující na jiný styl. To umožňuje styly dědit z předchozí styly a přidat nebo nahradit nastavení vlastností. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) příklad znázorňuje to.

Pokud `Style2` je založena na `Style1`, `TargetType` z `Style2` musí být stejný jako `Style1` nebo odvozené od `Style1`. Slovník prostředků, ve kterém `Style1` ukládána musí být slovníku prostředků jako `Style2` nebo slovník prostředků, která je vyšší ve vizuálním stromu.

## <a name="implicit-styles"></a>Implicitní styly

Pokud `Style` v prostředku nemá slovník `x:Key` atribut nastavení, je přiřazen klíč slovník automaticky a `Style` objektu se změní na *implicitní styl*. Zobrazení bez `Style` nastavení a jejíž typ odpovídá `TargetType` přesně zjistí styl, jako [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) příklad znázorňuje.

Implicitní styl můžete odvozena od `Style` s `x:Key` nastavení, ale ne naopak. Implicitní stylu nelze explicitně odkazovat.

Můžete implementovat tři typy hierarchie s styly a `BasedOn`:

- Z styly definované na `Application` a `Page` dolů styly definované v rozložení nižší ve vizuálním stromu.
- Z styly definované pro základní třídy, jako `VisualElement` a `View` na styly definované pro konkrétní třídy.
- Z styly s klíče explicitní slovníku pro implicitní stylů.

Jsou tyto hierarchií předvedená v [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) ukázka.

## <a name="dynamic-styles"></a>Dynamické styly

Styl ve slovníku prostředků lze odkazovat pomocí `DynamicResource` místo `StaticResource`. Díky tomu styl *dynamické styl*. Pokud tento styl je nahrazena ve slovníku prostředků jiný styl se stejným klíčem, zobrazení odkazující na tuto stylu `DynamicResource` automaticky změnit. Navíc způsobí, že chybí položky slovníku se zadaným klíčem `StaticResource` k vyvolat výjimku, ale ne `DynamicResource`.

Tento postup můžete dynamicky měnit stylů nebo motivy, jako [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) příklad znázorňuje.

Však nelze nastavit `BasedOn` vlastnosti `DynamicResource` způsob vytvoření rozšíření protože `BasedOn` není podporována pomocí vazbu vlastnosti. Odvození styl dynamicky, nenastavujte `BasedOn`. Místo toho nastavte [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) vlastnost slovníku klíč styl, který má být odvozen z. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) příklad znázorňuje tento postup.

## <a name="device-styles"></a>Styly zařízení

[ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) Vnořené třídy definuje dvanáct statická jen pro čtení pole pro šest styly s `TargetType` z `Label` , můžete použít pro běžné typy text použití.

Šest z těchto polí jsou typu `Style` , můžete nastavit přímo na `Style` vlastností v kódu:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

V ostatních polích šesti jsou typu `string` a mohou být použity jako klíče slovníku pro dynamické styly:

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) rovno "BodyStyle"
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) rovno "TitleStyle"
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) rovno "SubtitleStyle"
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) rovno "CaptionStyle"
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) rovno "ListItemTextStyle"
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) rovno "ListItemDetailTextStyle"

Tyto styly jsou zobrazené ve [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) ukázka.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 12 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Ukázky kapitoly 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Styly](~/xamarin-forms/user-interface/styles/index.md)
