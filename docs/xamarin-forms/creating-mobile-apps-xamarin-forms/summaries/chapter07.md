---
title: Souhrn kapitola 7. XAML vs. kód
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 7. XAML vs. kód'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: d04012d5d2ea6a7617d5c7559aa3e1532dad15d1
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156909"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Souhrn kapitola 7. XAML vs. kód

> [!NOTE] 
> Poznámky na této stránce označit oblasti, kde se Xamarin.Forms se rozcházela z materiály uvedené v seznamu.

Xamarin.Forms podporuje založené na XML značky jazyka nazvaného Extensible Application Markup Language nebo XAML (vyslovováno "zaml"). XAML poskytuje alternativu k C# při definování rozložení uživatelského rozhraní aplikace Xamarin.Forms a definování vazeb mezi prvky uživatelského rozhraní a příslušná data.

## <a name="properties-and-attributes"></a>Vlastnosti a atributů

Xamarin.Forms třídy a struktury XML elementů v XAML, a vlastnosti třídy a struktury změní atributy ve formátu XML. Má být vytvořena v XAML, musí třída obvykle mít veřejný konstruktor bez parametrů. Všechny vlastnosti nastavené v XAML musí mít veřejnou `set` přistupující objekty.

Pro vlastnosti základní datové typy (`string`, `double`, `bool`a tak dále), používá standardní analyzátor XAML `TryParse` metody převodu nastavení atributů pro tyto typy. Analyzátor XAML může také snadno zpracovávat výčtové typy a členy výčtu může kombinovat Pokud typ výčtu se značkou `Flags` atribut.

Abychom analyzátoru XAML může obsahovat složitější typy (nebo vlastnosti z těchto typů) [ `TypeConverterAttribute` ](xref:Xamarin.Forms.TypeConverterAttribute) identifikující třídu, která je odvozena z [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) který podporuje počítačový převod z řetězcové hodnoty na tyto typy. Například [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) převede barev názvy a řetězce, jako je například "#rrggbb" do `Color` hodnoty.

## <a name="property-element-syntax"></a>Syntaxe elementů vlastností

V XAML třídy a objekty vytvořené z nich jsou vyjádřeny jako prvky jazyka XML. Toto jsou známé jako *objektu prvky*. Většina vlastností tyto objekty jsou vyjádřeny jako atributy ve formátu XML. Toto nastavení se nazývá *atributy vlastnosti*.

Někdy musí být vlastnost nastavena na objekt, který nelze vyjádřen jako jednoduchým řetězcem. V takovém případě XAML podporuje značku *element vlastnosti* , který se skládá z názvu třídy a název vlastnosti, které jsou oddělené tečkou. Prvek objektu můžete potom zobrazí během pár značek element vlastnosti.

## <a name="adding-a-xaml-page-to-your-project"></a>Přidání stránky XAML do projektu

Xamarin.Forms Přenosná knihovna tříd může obsahovat stránky XAML při prvním vytvoření nebo stránky XAML můžete přidat do existujícího projektu. V dialogovém okně Přidat novou položku, vyberte položku, která odkazuje na stránku XAML nebo `ContentPage` a XAML. (Ne `ContentView`.)

> [!NOTE] 
> Možnosti aplikace Visual Studio změnily od této kapitole byla zapsána.

Se vytvoří dva soubory: soubor XAML s .xaml příponu názvu souboru a soubor jazyka C# s příponou. xaml.cs. Soubor C# se často označuje jako *použití modelu code-behind* souboru XAML. Soubor kódu na pozadí je definicí částečné třídy, která je odvozena z `ContentPage`. V okamžiku sestavení je analyzován XAML a jiné definice částečné třídy je generován pro tutéž třídu. Tato generovaná třída obsahuje metodu s názvem `InitializeComponent` , která je volána z konstruktoru soubor kódu na pozadí.

Za běhu na závěr `InitializeComponent` volat všechny elementy XAML souboru byla vytvořena instance a inicializován tak, jako kdyby měla byly vytvořeny v kódu jazyka C#.

Kořenový element v souboru XAML je `ContentPage`. Kořenová značka obsahuje alespoň dvě deklarace oboru názvů XML, jednu pro Xamarin.Forms prvky a další definování `x` předponu pro prvky a atributy, které jsou přirozené pro všechny implementace XAML. Kořenová značka obsahuje také `x:Class` atribut, který označuje obor názvů a název třídy, která je odvozena z `ContentPage`. To odpovídá názvu oboru názvů a třídy v souboru kódu na pozadí.

Kombinace XAML a kódu je znázorněn ve [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) vzorku.

## <a name="the-xaml-compiler"></a>Kompilátor XAML

Má kompilátor XAML Xamarin.Forms, ale jeho použití je volitelné na základě týkající se použití [ `XamlCompilationAttribute` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute). Pokud není zkompilován XAML, v okamžiku sestavení je analyzován XAML a XAML soubor je vložen PCL, kde je také analyzován za běhu. Pokud XAML je zkompilován, proces sestavení se převede XAML do binárního formátu a zpracování modulu runtime je mnohem efektivnější.

## <a name="platform-specificity-in-the-xaml-file"></a>Platforma specifičnosti v souboru XAML

V XAML [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) třídy lze použít k výběru značek závislého na platformě. Toto je obecná třída a musí být vytvořena pomocí `x:TypeArguments` atribut, který odpovídá cílovým typem. [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) Třída je podobné, ale využité mnohem méně často.

Použití `OnPlatform` změnila knihu publikoval. To byl původně používá ve spojení s vlastností s názvem `iOS`, `Android`, a `WinPhone`. Používá se teď u podřízené [ `On` ](xref:Xamarin.Forms.On) objekty. Nastavte [ `Platform` ](xref:Xamarin.Forms.On.Platform) vlastnost na řetězec, který je konzistentní s veřejně `const` pole [ `Device` ](xref:Xamarin.Forms.Device) třídy. Nastavte [ `Value` ](xref:Xamarin.Forms.On.Value) vlastnost na hodnotu konzistentní s `x:TypeArguments` atribut `OnPlatform` značky.

`OnPlatform` je patrné [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) vzorku, takže volat, protože obsahuje bloky skoro stejné XAML. Existence tohoto automatizujete značek naznačuje, že techniky by možné snížit.

## <a name="the-content-property-attributes"></a>Vlastnost obsahu atributy

Některé prvky vlastnosti docházet poměrně často, jako `<ContentPage.Content>` značku na kořenový element `ContentPage`, nebo `<StackLayout.Children>` značky, který obklopuje podřízené objekty daného `StackLayout`.

Každá třída může identifikovat jednu vlastnost s [ `ContentPropertyAttribute` ](xref:Xamarin.Forms.ContentPropertyAttribute) ve třídě. Pro tuto vlastnost nejsou nutné značky element vlastnosti. `ContentPage` definuje vlastnost obsahu jako `Content`, a `Layout<T>` (třída, ze které `StackLayout` odvozený) definuje jeho vlastnost obsahu jako `Children`. Značky elementů tato vlastnost se nevyžadují.

Prvek vlastnosti `Label` je `Text`.

## <a name="formatted-text"></a>Formátovaný text

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) vzorek obsahuje několik příkladů nastavení `Text` a `FormattedText` vlastnosti `Label`. V XAML `Span` zobrazit objekty jako podřízené objekty `FormattedString` objektu.

 Když Víceřádkový řetězec je nastavený na `Text` vlastnost, na konci řádku znaky jsou převedeny na znaky mezery, ale znaky konec řádku se zachová, i když Víceřádkový řetězec se zobrazí jako obsah `Label` nebo `Label.Text` značky:

 [![Trojitá snímek text variace sdílení](images/ch07fg03-small.png "variace Text ve formátu")](images/ch07fg03-large.png#lightbox "variace formátovaný Text")

## <a name="related-links"></a>Související odkazy

- [Kapitola 7 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Ukázky kapitola 7](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Ukázka kapitola 7 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML – základy](~/xamarin-forms/xaml/xaml-basics/index.md)