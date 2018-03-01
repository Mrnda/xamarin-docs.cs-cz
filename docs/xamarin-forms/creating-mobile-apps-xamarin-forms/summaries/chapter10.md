---
title: "Souhrn kapitoly 10. XAML – rozšíření značek"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 8ded1dba0e1d4d1a9062d0f75935b3d748a83370
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Souhrn kapitoly 10. XAML – rozšíření značek

Za normálních okolností analyzátor XAML převede libovolný řetězec nastavená jako hodnota atributu typ vlastnosti podle standardní převody pro základní datové typy .NET, nebo [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) odvozených připojen ke vlastnost nebo typ s [`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

Ale v některých případech je vhodné nastavit atribut z jiného zdroje, například na položku ve slovníku nebo hodnotu statickou vlastnost nebo pole, nebo z výpočtu některé řazení.

Toto je práci *XAML – rozšíření značek*. Bez ohledu název, jsou rozšíření značek XAML *není* rozšíření XML. XAML je vždy právní XML.

## <a name="the-code-infrastructure"></a>Kód infrastruktury

Rozšíření značek XAML je třída, která implementuje [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) rozhraní. Tato třída má často slovo `Extension` na konci názvu ale obvykle se zobrazí v jazyce XAML bez tuto příponu.

Následující rozšíření značek XAML jsou podporovány všechny implementacemi XAML:

- `x:Static` nepodporuje [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` nepodporuje [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` nepodporuje [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` nepodporuje [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` nepodporuje [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

Mnoho implementací XAML, včetně Xamarin.Forms podporuje tyto čtyři rozšíření značek v jazyce XAML:

- `StaticResource` nepodporuje [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` nepodporuje [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` nepodporuje [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) &mdash;popsané v [kapitoly 16. Datová vazba](#chapter16)
- `TemplateBinding` nepodporuje [ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) &mdash;nejsou zahrnuté v knize

Další rozšíření značek XAML je součástí Xamarin.Forms ve spojení s [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;nejsou zahrnuté v knize

## <a name="accessing-static-members"></a>Přístup k statické členy

Použití [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) elementu, který chcete nastavit atribut na hodnotu veřejné statický člen vlastnost, pole nebo výčet. Nastavte [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) vlastnost statický člen. Je obvykle jednodušší zadejte `x:Static` a název člena do složených závorek. Název `Member` vlastnost nemusí být zahrnut, právě člena samotného. Běžné čárkami v [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) ukázka. Statická pole sami jsou definovány v [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) třídy. Tento postup umožňuje vytvořit konstanty používá prostřednictvím programu.

S další deklarace oboru názvů XML, můžete odkazovat veřejné statické vlastnosti, pole nebo členové výčtu definované v rozhraní .NET framework, jak je předvedeno v [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) vzorku .

## <a name="resource-dictionaries"></a>Slovnících prostředků

`VisualElement` Třída definuje vlastnost s názvem [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) , kterou můžete nastavit pro objekt typu [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). V jazyce XAML, můžete uložit položky do tohoto slovníku a identifikovat pomocí `x:Key` atribut. Položky uložené ve slovníku prostředků jsou sdílena mezi všechny odkazy na položky.

### <a name="staticresource-for-most-purposes"></a>StaticResource pro většinu účelů

Ve většině případů budete používat [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) – rozšíření značek k odkazování položku ze slovníku prostředků, jak je předvedeno pomocí [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) ukázka . Můžete použít `StaticResourceExtension` element nebo `StaticResource` v rámci složené závorky:

[![Trojitá snímek obrazovky sdílení prostředků](images/ch10fg03-small.png "sdílení prostředků")](images/ch10fg03-large.png "sdílení prostředků")

Nezaměňujte `x:Static` – rozšíření značek a `StaticResource` – rozšíření značek.

### <a name="a-tree-of-dictionaries"></a>Strom slovník

Pokud analyzátor XAML nalezne `StaticResource`, začne hledání se ve vizuálním stromu odpovídající klíč a pak hledá v `ResourceDictionary` do aplikace `App` třídy. To umožňuje položky v slovník prostředků, který je umístěný hlouběji ve stromové struktuře visual přepsat slovník prostředků, která je vyšší ve vizuálním stromu. Tento postup je znázorněn v [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) ukázka.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource pro zvláštní účely

`StaticResource` – Rozšíření značek způsobí, že položku, kterou chcete načíst ze slovníku při vizuálním stromu vychází během `InitializeComponent` volání. Alternativu k `StaticResource` je [ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/), který udržuje odkaz na klíče slovníku a aktualizuje cíl, když položka odkazuje změn klíče.

Rozdíl mezi `StaticResource` a `DynamicResource` ukazují [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) ukázka.

Vlastnost, která nastavuje `DynamicResource` musí zálohovaný pomocí vazbu vlastnosti, jak je popsáno v [kapitoly 11, vazbu infrastruktury](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Rozšíření méně používaný značek

Použití [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) – rozšíření značek pro nastavení vlastnosti `null`.

Použití [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) – rozšíření značek pro nastavení vlastnosti na .NET `Type` objektu.

Použití [ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) k definování pole. Zadejte typ pole členů nastavením [`Type`] vlastnost, která má `x:Type` – rozšíření značek.

## <a name="a-custom-markup-extension"></a>Rozšíření vlastního kódu

Můžete vytvořit vlastní rozšíření značek v jazyce XAML zápis třídu, která implementuje [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) rozhraní s [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/) metoda.

[ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Třída splňuje tyto požadavky. Vytvoří hodnotu typu `Color` na základě hodnot vlastností s názvem `H`, `S`, `L`, a `A`. Tato třída představuje první položku v knihovně Xamarin.Forms s názvem [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) , je vytvářet a používat v průběhu této příručky.

[ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) ukázka ukazuje, jak odkazovat na tuto knihovnu a používání rozšíření vlastních značek.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 10 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Kapitola 10 – ukázky](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
