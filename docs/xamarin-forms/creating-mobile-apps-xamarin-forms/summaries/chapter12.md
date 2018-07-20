---
title: Souhrn kapitole 12. Styly
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitole 12. Styly'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 8ee169d15c4b5060f2a7696bfebd314ed7029570
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156938"
---
# <a name="summary-of-chapter-12-styles"></a>Souhrn kapitole 12. Styly

V Xamarin.Forms styly povolit více zobrazení sdílet kolekce vlastností nastavení. Tím se sníží značek a povolí zachování konzistentní vizuální motivy.

Styly jsou téměř vždy definována a využívat v kódu. Objekt typu [ `Style` ](xref:Xamarin.Forms.Style) vytvořena ve slovníku prostředků a poté nastavte [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) vlastnost vizuální prvek pomocí `StaticResource` nebo `DyanamicResource` značek rozšíření.

## <a name="the-basic-style"></a>Základní styl

A `Style` vyžaduje, aby jeho [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) nastavit typ vizuální objekty, které se vztahuje na. Když `Style` je vytvořena instance ve slovníku prostředků (což je běžné) také budete potřebovat `x:Key` atribut.

`Style` Vlastnost content typu [ `Setters` ](xref:Xamarin.Forms.Style.Setters), což je kolekce [ `Setter` ](xref:Xamarin.Forms.Setter) objekty. Každý `Setter` přidruží [ `Property` ](xref:Xamarin.Forms.Setter.Property) s [ `Value` ](xref:Xamarin.Forms.Setter.Value).

V XAML `Property` nastavení je název vlastnosti CLR (například `Text` vlastnost `Button`), ale upravený vlastnost musí být se opírá o vlastnost s vazbou. Také, vlastnost musí být definována ve třídě indikován `TargetType` nastavení nebo zděděná podle této třídy.

Můžete zadat `Value` nastavení pomocí elementu vlastnosti `<Setter.Value>`. Díky tomu můžete nastavit `Value` na objekt, který nelze vyjádřen v textovém řetězci nebo do `OnPlatform` objektu nebo objekt vytvořit instanci pomocí `x:Arguments` nebo `x:FactoryMethod`. `Value` Vlastnost lze nastavit také pomocí `StaticResource` výraz s jinou položkou ve slovníku.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) program ukazuje základní syntaxe a ukazuje, jak odkazovat `Style` s `StaticResource` – rozšíření značek:

[![Trojitá snímek základního styl](images/ch12fg01-small.png "základní styly")](images/ch12fg01-large.png#lightbox "základní styly")

`Style` Objekt a všechny objekt vytvořený v `Style` objektu jako `Value` nastavení jsou odkazy sdíleny mezi všechna zobrazení, která odkazuje `Style`. `Style` Nemůže obsahovat cokoli, co nelze sdílet, například `View` odvozených děl na základě.

Obslužné rutiny událostí nemohou být nastaveny `Style`. `GestureRecognizers` Nelze nastavit vlastnost `Style` vzhledem k tomu, že není podporovaný službou vázanou vlastnost.

## <a name="styles-in-code"></a>Styly v kódu

I když není běžné, můžete konkretizovat a inicializovat `Style` objekty v kódu. To je patrné podle [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) vzorku.

## <a name="style-inheritance"></a>Dědičnost stylů

`Style` má [ `BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) vlastnost, která můžete nastavit `StaticResource` – rozšíření značek odkazující na jiný styl. To umožňuje styly a dědí z předchozí styly a přidat nebo odebrat nastavení vlastností. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) příklad ukazuje to.

Pokud `Style2` vychází `Style1`, `TargetType` z `Style2` musí být stejný jako `Style1` nebo odvozený od `Style1`. Slovník prostředků, ve kterém `Style1` uložená musí být slovníku prostředků jako `Style2` nebo slovníku prostředků, která je vyšší ve vizuálním stromu.

## <a name="implicit-styles"></a>Implicitní styly

Pokud `Style` v prostředku nemá slovníku `x:Key` atribut nastavení, je automaticky přiřazen klíč slovníku a `Style` objekt se stane *implicitní styl*. Zobrazení bez `Style` nastavení a jehož typ odpovídá typu `TargetType` přesně najdou tento styl, jako [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) ukázce.

Implicitní styl lze odvodit z `Style` s `x:Key` nastavení, ale ne naopak. Nemůže explicitně odkazovat na implicitní styl.

Můžete implementovat tři typy hierarchie se styly a `BasedOn`:

- Z styly definované na `Application` a `Page` dolů styly definované v rozložení nižší ve vizuálním stromu.
- Z styly definované pro základní třídy, jako `VisualElement` a `View` na styly definované pro konkrétní třídy.
- Ze stylů pomocí explicitní slovník klíčů na implicitní styly.

Tato hierarchie je ukázán v [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) vzorku.

## <a name="dynamic-styles"></a>Dynamické styly

Styl ve slovníku prostředků lze odkazovat pomocí `DynamicResource` spíše než `StaticResource`. Díky tomu styl *dynamické styl*. Pokud tento styl je nahrazena ve slovníku prostředků jiný styl se stejným klíčem, zobrazení odkazující na tento styl s `DynamicResource` automaticky změnit. Navíc způsobí, neexistence položky slovníku se zadaným klíčem `StaticResource` pro vyvolání výjimky, ale ne `DynamicResource`.

Tento postup můžete použít dynamicky měnit styly nebo motivy, jako [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) ukázce.

Však nelze nastavit `BasedOn` vlastnost `DynamicResource` strukturu rozšíření protože `BasedOn` není se opírá o vlastnost s vazbou. Abyste odvodili styl dynamicky, nenastavujte `BasedOn`. Místo toho nastavte [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) vlastnost ke klíči slovníku styl, který má být odvozen od. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) ukázka demonstruje tento postup.

## <a name="device-styles"></a>Styly zařízení

[ `Device.Styles` ](xref:Xamarin.Forms.Device.Styles) Vnořené třídy definuje dvanáct statické pole jen pro čtení pro šest stylů `TargetType` z `Label` , můžete použít pro běžné typy z použití textu.

Šest tato pole jsou typu `Style` , můžete nastavit přímo `Style` vlastností v kódu:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

Šest polí jsou typu `string` a může sloužit jako klíče slovníku pro dynamické styly:

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) rovno "BodyStyle"
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) rovno "TitleStyle"
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) rovno "SubtitleStyle"
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) rovno "CaptionStyle"
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) rovno "ListItemTextStyle"
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) rovno "ListItemDetailTextStyle"

Tyto styly jsou znázorněny podle [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) vzorku.

## <a name="related-links"></a>Související odkazy

- [Kapitola 12 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Ukázky kapitole 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Styly](~/xamarin-forms/user-interface/styles/index.md)
