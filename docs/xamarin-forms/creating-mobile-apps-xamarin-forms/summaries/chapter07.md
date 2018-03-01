---
title: "Shrnutí kapitoly 7. XAML oproti kódu"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1263328a748ac0bacd368da361aeaff57c4cfa20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Shrnutí kapitoly 7. XAML oproti kódu

Xamarin.Forms podporuje jazyka kódu na základě XML nazývaného Extensible Application Markup Language nebo XAML (vyslovováno "zammel"). XAML představuje alternativu ke C# k definování rozložení uživatelské rozhraní aplikace Xamarin.Forms a definování vazby mezi elementy uživatelského rozhraní a základní data.

## <a name="properties-and-attributes"></a>Vlastnosti a atributy

Xamarin.Forms třídy a struktury elementů XML v jazyce XAML, a vlastnosti třídy a struktury změní atributy XML. Chcete-li být vytvořena instance v jazyce XAML, obecně třídu musí mít konstruktor public bez parametrů. Všechny vlastnosti nastavit v jazyce XAML, musí mít veřejný `set` přistupující objekty.

Pro vlastnosti základní datové typy (`string`, `double`, `bool`, a tak dále), XAML analyzátor používá standardní `TryParse` metody převést na tyto typy nastavení atributů. Analyzátor jazyka XAML může také snadno zpracovat výčtové typy a může být kombinací členy výčtu Pokud typ výčtu je označené `Flags` atribut.

Pomáhá analyzátor jazyka XAML, může obsahovat více komplexní typy (nebo vlastnosti těchto typů) [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/) identifikující třídu odvozenou z [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) který podporuje převod z řetězcové hodnoty na tyto typy. Například [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) převede barvu řetězce, jako je například "#rrggbb", s názvy a do `Color` hodnoty.

## <a name="property-element-syntax"></a>Vlastnost element syntaxe

V jazyce XAML třídy a objekty vytvořené z nich jsou vyjádřené jako elementy XML. Toto jsou známé jako *objektu elementy*. Většinu vlastností tyto objekty jsou vyjádřené jako atributy XML. Toto nastavení se nazývá *vlastnost atributy*.

Vlastnost někdy musí být nastavená na objekt, který nemůže být vyjádřena jako jednoduchý řetězec. V takovém případě XAML podporuje značku *element vlastnosti* , se skládá z název třídy a název vlastnosti, které jsou odděleny tečkou. Element objekt lze potom použít v rámci pár značek element vlastnosti.

## <a name="adding-a-xaml-page-to-your-project"></a>Přidání stránky XAML do projektu

Knihovny přenosných tříd Xamarin.Forms může obsahovat stránky XAML, když je poprvé vytvořen nebo XAML – stránka můžete přidat do existujícího projektu. V dialogovém okně Přidat novou položku, vyberte položku, která odkazuje na stránku XAML, nebo `ContentPage` a XAML. (Není `ContentView`.)

Jsou vytvořeny dva soubory: soubor XAML s XAML příponu názvu souboru a soubor C# s příponou. xaml.cs. Soubor C# se často označuje jako *kódu* souboru XAML. Soubor kódu je definice třídu odvozenou od `ContentPage`. V okamžiku sestavení je analyzovat XAML a jinou třídu definici se generuje pro stejnou třídu. Tato generovaná třída obsahuje metodu s názvem `InitializeComponent` která je volána z konstruktoru souboru kódu na pozadí.

Během doby běhu, při ukončení `InitializeComponent` volat, všechny elementy souboru XAML byla vytvořena instance a inicializovat stejně, jako by se měl byly vytvořeny v C# – kód.

Kořenový element v souboru XAML `ContentPage`. Kořenovou značku obsahuje alespoň dva deklarace oboru názvů XML, jeden pro elementy Xamarin.Forms a jiných definování `x` předponu pro elementy a atributy vnitřní všem implementacím XAML. Také obsahuje kořenovou značku `x:Class` atribut, který určuje obor názvů a název třídy, která je odvozena z `ContentPage`. To odpovídá názvu oboru názvů a třídy v souboru kódu na pozadí.

Kombinace XAML a kódu ukazují pomocí [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) ukázka.

## <a name="the-xaml-compiler"></a>Kompilátor jazyka XAML

Xamarin.Forms má kompilátor jazyka XAML, ale jeho použití je volitelné založené na použití [ `XamlCompilationAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/). Pokud není kompilována XAML, XAML je analyzována v okamžiku sestavení a souboru XAML vložené do PCL, kde je také analyzovat za běhu. Pokud kompiluje XAML procesu sestavení převede XAML do binárního formátu a zpracování modulu runtime je efektivnější.

## <a name="platform-specificity-in-the-xaml-file"></a>Specifické platformy podobě v souboru XAML

V jazyce XAML [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) třídy lze použít k výběru značek závislé na platformu. Toto je obecná třída a musí být vytvořena s `x:TypeArguments` atribut, který odpovídá typu cíle. [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/) Třída je podobné ale použít mnohem méně často.

Použití `OnPlatform` od publikování knihy změnila. Byl původně použije ve spojení s vlastností s názvem `iOS`, `Android`, a `WinPhone`. Použije se teď s podřízené [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) objekty. Nastavit [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) na řetězec konzistentní s veřejnosti `const` pole [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) třídy. Nastavte [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/) vlastnost na hodnotu, která je konzistentní s `x:TypeArguments` atribut `OnPlatform` značky.

`OnPlatform` ukazují [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) ukázkové, takže volat, protože obsahuje bloky skoro stejné XAML. Existenci tento automatizujete značek naznačuje, že techniky by měl být k dispozici pro snížit.

## <a name="the-content-property-attributes"></a>Vlastnost obsahu atributy

Některé prvky vlastnost dojít poměrně často, jako `<ContentPage.Content>` značky na kořenový prvek `ContentPage`, nebo `<StackLayout.Children>` značku, která obklopuje podřízené objekty daného `StackLayout`.

Každá třída může identifikovat jednu vlastnost s [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/) na třídu. Pro tuto vlastnost nejsou nutné značky element vlastnosti. `ContentPage` definuje vlastnost jeho obsahu jako `Content`, a `Layout<T>` (třídu, ze které `StackLayout` odvozuje) definuje vlastnost jeho obsahu jako `Children`. Tyto značky elementu vlastností se nevyžaduje.

Vlastnost element `Label` je `Text`.

## <a name="formatted-text"></a>Formátovaný text

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) ukázka obsahuje několik příkladů, nastavení `Text` a `FormattedText` vlastnosti `Label`. V jazyce XAML `Span` objekty se zobrazí jako podřízené objekty `FormattedString` objektu.

 Když je řetězec Víceřádkový nastavená na `Text` vlastnost, konec řádku znaky jsou převedeny na znaky, ale konec řádku znaky se zachová, i když řetězec Víceřádkový se zobrazí jako obsah `Label` nebo `Label.Text` značky:

 [![Trojitá snímek obrazovky text variace sdílení](images/ch07fg03-small.png "formátovaný Text variace")](images/ch07fg03-large.png "variace formátovaný Text")



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 7 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Ukázky kapitoly 7](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Ukázka kapitoly 7 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
