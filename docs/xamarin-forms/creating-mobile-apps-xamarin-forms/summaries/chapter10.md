---
title: Souhrn kapitoly 10. Rozšíření značek XAML
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 10. Rozšíření značek XAML'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 74f7e2846a9e8d8390a8322c57db0845718bbba7
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39157000"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Souhrn kapitoly 10. Rozšíření značek XAML

Za normálních okolností analyzátoru XAML převede jakýkoli řetězec, nastavit jako hodnotu atributu pro typ vlastnosti založen na standardní převody na základní datové typy rozhraní .NET, nebo [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) odvozených děl na základě přiřazená vlastnost nebo její typ [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

Ale někdy je vhodné nastavit atribut z jiného zdroje, například položky ve slovníku, nebo hodnotu statickou vlastnost nebo pole, nebo z výpočtů s nějakým.

To je práce *– rozšíření značek XAML*. Bez ohledu na název, jsou rozšíření značek XAML *není* rozšíření XML. XAML je vždy právní XML.

## <a name="the-code-infrastructure"></a>Kód infrastruktury

Rozšíření značek XAML je třída, která implementuje [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) rozhraní. Tato třída často obsahuje slovo `Extension` na konci názvu ale obvykle se zobrazí v XAML bez této přípony.

Následující rozšíření značek XAML podporuje všechny implementace XAML:

- `x:Static` podporuje [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)
- `x:Reference` podporuje [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)
- `x:Type` podporuje [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)
- `x:Null` podporuje [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)
- `x:Array` podporuje [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)

Mnoho implementací XAML, včetně Xamarin.Forms podporuje tyto čtyři rozšíření značek XAML:

- `StaticResource` podporuje [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)
- `DynamicResource` podporuje [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)
- `Binding` podporuje [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) &mdash;podrobněji [kapitola 16. Datová vazba](#chapter16)
- `TemplateBinding` podporuje [ `TemplateBindingExtension` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) &mdash;nejsou zahrnuta v knize

Další rozšíření značek XAML je součástí Xamarin.Forms v souvislosti s [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;nejsou zahrnuta v knize

## <a name="accessing-static-members"></a>Přistupování ke statickým členům

Použití [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) prvek, který chcete nastavit atribut na hodnotu veřejného statického členu vlastnosti, pole nebo výčet. Nastavte [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) vlastnost statického členu. Je obvykle jednodušší určit `x:Static` a název člena ve složených závorkách. Název `Member` vlastnost nemusí být zahrnut, stačí samotný člen. Tato běžné syntaxe je zobrazena ve [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) vzorku. Statická pole, samotné jsou definované v [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) třídy. Tato technika umožňuje navázat konstanty používané prostřednictvím programu.

Pomocí dalších deklarace oboru názvů XML, můžete odkazovat veřejné statické vlastnosti, pole nebo členy výčtu definované v rozhraní .NET framework, jak je ukázáno v [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) vzorku .

## <a name="resource-dictionaries"></a>Slovníky sloučených prostředků

`VisualElement` Třída definuje vlastnost s názvem [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) , kterou můžete nastavit pro objekt typu [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). V rámci XAML, můžete ukládat položky tohoto slovníku a s jejich identifikaci `x:Key` atribut. Položek uložených ve slovníku prostředků jsou sdílena mezi všechny odkazy na položky.

### <a name="staticresource-for-most-purposes"></a>Pro většinu účelů StaticResource

Ve většině případů budete používat [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) – rozšíření značek pro odkažte na položku ze slovníku prostředků, jak je uvedeno ve [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) vzorku . Můžete použít `StaticResourceExtension` element nebo `StaticResource` ve složených závorkách:

[![Trojitá snímek sdílení prostředků](images/ch10fg03-small.png "sdílení prostředků")](images/ch10fg03-large.png#lightbox "sdílení prostředků")

Nepleťte si `x:Static` – rozšíření značek a `StaticResource` – rozšíření značek.

### <a name="a-tree-of-dictionaries"></a>Strom slovníky

Když analyzátor XAML narazí `StaticResource`, začíná hledání nahoru ve vizuálním stromu odpovídajícího klíče a poté hledá v `ResourceDictionary` v aplikačním `App` třídy. To umožňuje položky ve slovníku prostředků hlubší ve vizuálním stromu přepsat slovník prostředků, která je vyšší ve vizuálním stromu. To je patrné [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) vzorku.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource pro zvláštní účely

`StaticResource` – Rozšíření značek položku, kterou chcete načíst ze slovníku při vytváření během vizuálním stromu způsobí, že `InitializeComponent` volání. Alternativa k `StaticResource` je [ `DynamicResource` ](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), který uchovává odkaz na klíč slovníku a aktualizuje cíl, když položka odkazuje změny klíče.

Rozdíl mezi `StaticResource` a `DynamicResource` je patrné [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) vzorku.

Nastavit vlastnost `DynamicResource` musí být se opírá o vlastnost s vazbou jak je popsáno v [kapitoly 11, infrastruktura s podporou vazeb](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Rozšíření značek nižší úrovně použít

Použití [ `x:Null` ](xref:Xamarin.Forms.Xaml.NullExtension) – rozšíření značek nastavení vlastnosti `null`.

Použití [ `x:Type` ](xref:Xamarin.Forms.Xaml.TypeExtension) – rozšíření značek k nastavení vlastnosti .NET `Type` objektu.

Použití [ `x:Array` ](xref:Xamarin.Forms.Xaml.ArrayExtension) k definování pole. Zadejte typ členy pole tak, že nastavíte [`Type`] vlastnost `x:Type` – rozšíření značek.

## <a name="a-custom-markup-extension"></a>Rozšíření vlastního kódu

Můžete vytvořit vlastní rozšíření značek XAML napsáním třídu, která implementuje [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) rozhraní se službou [ `ProvideValue` ](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) metoda.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Třídy splňuje tyto požadavky. Vytvoří hodnotu typu `Color` na základě hodnot vlastností s názvem `H`, `S`, `L`, a `A`. Tato třída je první položky v knihovně Xamarin.Forms s názvem [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) , která je vytvořená a použít v průběhu této příručky.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) Ukázka předvádí, jak odkazovat na tuto knihovnu a pomocí rozšíření vlastních značek.

## <a name="related-links"></a>Související odkazy

- [Kapitola 10 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Ukázky kapitoly 10](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [Rozšíření značek XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
