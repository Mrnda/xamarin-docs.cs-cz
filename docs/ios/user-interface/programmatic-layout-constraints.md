---
title: Omezení programový rozložení v Xamarin.iOS
description: Tato příručka uvede práci s iOS automatického rozložení omezení v kódu jazyka C# místo jejich vytváření v iOS Designer.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a40a3c66369902d2d6f8dbee5a6a7e9bad8a9e05
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790598"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Omezení programový rozložení v Xamarin.iOS

_Tato příručka uvede práci s iOS automatického rozložení omezení v kódu jazyka C# místo jejich vytváření v iOS Designer._

Automatické rozložení (také nazývané "adaptivní rozložení") je přizpůsobivý návrh přístup. Na rozdíl od přechodném rozložení systému, kde každý element umístění je pevně zakódovaná na bod na obrazovce, je automaticky rozložení o *vztahy* -pozice elementů relativně k další prvky na návrhovou plochu. Jádrem automatického rozložení je představu o omezení nebo pravidla, která definují umístění element nebo sadu elementů v kontextu jiných prvků na obrazovce. Protože elementy nejsou vázaný na konkrétní pozici na obrazovce, omezení pomůže vytvořit adaptivní rozložení, který bude vypadat dobře v různých obrazovek velikosti a orientace zařízení.

Obvykle při práci s automatického rozložení v iOS, budete používat iOS Návrhář graficky zadat rozložení omezení na své uživatelské rozhraní položky. Může však nastat situace, když potřebujete k vytvoření a použití omezení v kódu jazyka C#. Například při použití dynamicky vytvoření elementy uživatelského rozhraní, které jsou přidané do `UIView`.

Tento průvodce vám ukáže, jak vytvořit a pracovat s omezeními použití kódu jazyka C# místo vytvoření graficky v iOS Designer.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>Vytváření omezení prostřednictvím kódu programu

Jak jsme uvedli výše, obvykle můžete budete pracovat s automatického rozložení omezení v iOS Designer. Pro případy, které budete muset vytvořit vaše omezení prostřednictvím kódu programu máte na výběr tři možnosti:

* [Rozložení kotvy](#Layout-Anchors) – tato rozhraní API poskytuje přístup k vlastnostem anchor (například `TopAnchor`, `BottomAnchor` nebo `HeightAnchor`) položek uživatelského rozhraní se omezené.
* [Omezení rozložení](#Layout-Constraints) – můžete vytvořit omezení přímo pomocí `NSLayoutConstraint` třídy.
* [Jazyk Visual formátování](#Visual-Format-Language) – poskytuje grafiku ve formátu ASCII jako metoda definovat vaše omezení.

V následujících částech se jednotlivé možnosti podrobně projít.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>Kotvy rozložení

Pomocí `NSLayoutAnchor` třídy, máte rozhraní fluent pro vytváření omezení podle vlastností ukotvení položek uživatelského rozhraní se omezené. Například řadič zobrazení horní a dolní rozložení provede zpřístupňuje `TopAnchor`, `BottomAnchor` a `HeightAnchor` ukotvení vlastnosti při zobrazení zpřístupní edge, center, velikost a standardních hodnot vlastnosti.

> [!IMPORTANT]
> Kromě standardní sadu vlastností anchor, zobrazení iOS také zahrnovat `LayoutMarginsGuides` a `ReadableContentGuide` vlastnosti. Tyto vlastnosti vystavit `UILayoutGuide` objekty pro práci s okraji zobrazení, které lze načíst obsah příručky v uvedeném pořadí.

Rozložení kotvy poskytují několik metod pro vytváření omezení ve formátu-čtení, compact:

- **ConstraintEqualTo** -definuje vztahu kde `first attribute = second attribute + [constant]` s volitelně zadaný `constant` hodnota odchylky.
- **ConstraintGreaterThanOrEqualTo** -definuje vztahu kde `first attribute >= second attribute + [constant]` s volitelně zadaný `constant` hodnota odchylky.
- **ConstraintLessThanOrEqualTo** -definuje vztahu kde `first attribute <= second attribute + [constant]` s volitelně zadaný `constant` hodnota odchylky.

Příklad:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Typické rozložení omezení může být vyjádřený jednoduše jako výraz lineární. Proveďte v následujícím příkladu:

[![](programmatic-layout-constraints-images/graph01.png "Omezení rozložení vyjádřený jako výraz lineární")](programmatic-layout-constraints-images/graph01.png#lightbox)

Které bude převedena na následující řádek kódu jazyka C# pomocí rozložení kotvy:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

Kde části kódu C#, odpovídají zadanými částmi rovnice následujícím způsobem:

|Rovnice|Kód|
|---|---|
|Položka 1|PurpleView|
|Atribut 1|LeadingAnchor|
|Relace|ConstraintEqualTo|
|Násobitel|Výchozí hodnota je proto není zadaný 1.0|
|Položka 2|OrangeView|
|Atribut 2|TrailingAnchor|
|Konstanta|10.0|

Kromě jenom parametry, které jsou nutné k řešení daný rozložení omezení rovnice, každá z metod rozložení ukotvení vynutit zabezpečení typů parametrů je předán. Proto vodorovné omezení kotvy například `LeadingAnchor` nebo `TrailingAnchor` lze použít pouze s další vodorovné ukotvení typy a multiplikátory poskytnuta pouze omezení velikosti.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>Omezení rozložení

Automatické rozložení omezení můžete přidat ručně přímo vytvořením `NSLayoutConstraint` v kódu jazyka C#. Na rozdíl od použití kotvy rozložení, musíte zadat hodnotu pro každý parametr i v případě, že bude mít žádný vliv na omezení definovaný. V důsledku toho budou skončili generovala značné množství těžko čitelný, často používaný kód. Příklad:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

Kde `NSLayoutAttribute` výčtu definuje hodnotu pro zobrazení okraj a odpovídat `LayoutMarginsGuide` vlastnosti, jako `Left`, `Right`, `Top` a `Bottom` a `NSLayoutRelation` výčet Určuje relaci, Vytvoří se mezi danou atributy jako `Equal`, `LessThanOrEqual` nebo `GreaterThanOrEqual`.

Na rozdíl od s rozhraním API ukotvení rozložení, `NSLayoutConstraint` metody vytváření není zvýrazněte důležité aspekty konkrétní omezení a neexistují žádné kompilace čas kontroly provést v omezení. V důsledku toho je snadné vytvořit neplatné omezení, která bude vyvolána výjimka za běhu.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>Jazyk Visual formátu

Visual formát jazyka umožňuje definovat omezení pomocí grafiku ve formátu ASCII jako řetězce, které poskytují vizuální reprezentace omezení vytváří. To má tyto výhody a nevýhody:

- Jazyk Visual formátu vynucuje vytvoření pouze platné omezení.
 - Automatické rozložení výstupy omezení ke konzole pomocí Visual jazyka formátu, takže zprávy ladění bude vypadat podobně jako kód použitý k vytvoření omezení.
 - Jazyk Visual formátu vám umožní vytvořit více omezení současně s velmi compact výrazem.
 - Vzhledem k tomu, že neexistuje žádné ověření na straně kompilace jazyka Visual formátu řetězců, problémy lze zjistit pouze za běhu.
 - Vzhledem k tomu, že Visual formát jazyka klade důraz vizualizace přes úplnost nelze vytvořit některé typy omezení s ním (například poměr).

Při vytváření omezení pomocí Visual formát jazyka, proveďte následující kroky:

1. Vytvoření `NSDictionary` obsahující objekty zobrazení a rozložení příručky a klíč řetězec, který se použije při definování formáty.
2. Volitelně můžete vytvořit `NSDictionary` , který definuje sadu klíče a hodnoty (`NSNumber`) použít jako hodnota konstanty pro omezení.
3. Řetězec formátu, který má rozložení vytvořte jeden sloupec nebo řádek položek.
4. Volání `FromVisualFormat` metodu `NSLayoutConstraint` k vygenerování omezení.
5. Volání `ActivateConstraints` metodu `NSLayoutConstraint` třída aktivace a použití omezení.

Například pokud chcete vytvořit jako úvodní i koncové omezení v jazyce Visual formátu, můžete použít následující:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Vzhledem k tomu, že Visual formát jazyka vždy vytvoří nulové omezení bodu připojen k okraje nadřazeného zobrazení při použití výchozí mezery, tento kód vytvoří identické výsledky příkladů uvedených výše.

Jazyk Visual formát pro složitější návrhy uživatelského rozhraní, jako je například více podřízených zobrazení na jeden řádek, Určuje vodorovný rozestup a svislé zarovnání. Jako v příkladu výše, určuje, kde `AlignAllTop` `NSLayoutFormatOptions` zarovnává všechna zobrazení v řádku nebo sloupec, který se jejich tolní počítače.

Najdete v článku společnosti Apple [příloha jazyk Visual formátu](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) pro několik příkladů běžných omezení a Visual gramatika řetězec formátu.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento průvodce zobrazí vytváření a práci s automatického rozložení omezení v jazyce C# a vytváření graficky v iOS Designer. Nejprve hledá pomocí rozložení kotvy (`NSLayoutAnchor`) pro zpracování automatického rozložení. V dalším kroku se vám ukázal, jak pracovat s rozložení omezení (`NSLayoutConstraint`). Nakonec se zobrazí pomocí jazyka Visual formátu automatického rozložení.

## <a name="related-links"></a>Související odkazy

- [Úvod do scénářů](~/ios/user-interface/storyboards/index.md)
- [iOS navrhovatelé návod ovládací prvky](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Automatické rozložení pomocí návrháře Xamarin pro iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple – vytváření omezení prostřednictvím kódu programu](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple – dodatek pro jazyk Visual formátu](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
