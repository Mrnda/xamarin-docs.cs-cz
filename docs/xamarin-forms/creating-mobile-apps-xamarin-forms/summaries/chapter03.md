---
title: Souhrn kapitolu 3. Hlouběji do textu
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitolu 3. Hlouběji do textu'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3ef8f14bd60cf612408bb9e3885ef319d3efc8c5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998332"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Souhrn kapitolu 3. Hlouběji do textu

Tato kapitola popisuje [ `Label` ](xref:Xamarin.Forms.Label) zobrazení do větší hloubky, včetně barvy, písma a formátování.

## <a name="wrapping-paragraphs"></a>Zabalení odstavců

Když [ `Text` ](xref:Xamarin.Forms.Label.Text) vlastnost `Label` obsahuje dlouhý text `Label` automaticky zabalí jej do více řádků jak je uvedeno ve [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) vzorku. Můžete vložit Unicode kódy, jako je například "\u2014" pro dlouhé pomlčky nebo znaky jazyka C#, jako je '\r; k rozdělení na nový řádek.

Při [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti `Label` jsou nastaveny na `LayoutOptions.Fill`, celková velikost `Label` se řídí prostor, který jeho kontejneru zpřístupňuje. `Label` Je označen jako *omezené*. Velikost `Label` je velikost svého kontejneru.

Když `HorizontalOptions` a `VerticalOptions` vlastnosti jsou nastaveny na hodnoty jiné než `LayoutOptions.Fill`, velikost `Label` se řídí místa vyžadovaného k vykreslení textu, až do velikosti, který zpřístupňuje jeho kontejneru pro `Label`. `Label` Je označen jako *neomezeným* a určuje vlastní velikost.

(Poznámka: podmínky *omezené* a *neomezeným* může být counter-intuitive, protože je obecně menší než omezené zobrazení bez omezení zobrazení. Kromě toho tyto podmínky nejsou používány konzistentně v rané fázi kapitoly knihy.)

Zobrazení jako `Label` můžete omezené v jedné dimenzi a vstupy bez omezení v jiném. A `Label` bude pouze Zalamovat text na více řádcích případě, že je omezen vodorovně.

Pokud `Label` je omezené, může být zabírají mnohem více místa, než se požaduje pro text. Text může být umístěné v rámci oblasti celkové `Label`. Nastavte [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) vlastnost člena [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) výčet ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [ `Center` ](xref:Xamarin.Forms.TextAlignment.Center), nebo [ `End` ](xref:Xamarin.Forms.TextAlignment.Center)) k řízení zarovnání všechny řádky odstavce. Výchozí hodnota je `Start` a zarovná text doleva.

Nastavte [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) vlastnost člena `TextAlignment` výčet umístění textu v horní části, center nebo dolní části zabraná `Label`.

Nastavte [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) vlastnost člena [ `LineBreakMode` ](xref:Xamarin.Forms.LineBreakMode) výčet ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [ `CharacterWrap` ](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation), nebo [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) do ovládací prvek, jak více řádků v bodu přerušení nebo se zkrátí.

## <a name="text-and-background-colors"></a>Barva textu a pozadí

Nastavte [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) a [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) vlastnosti `Label` k [ `Color` ](xref:Xamarin.Forms.Color) hodnoty barvy textu a pozadí.

`BackgroundColor` Se vztahuje na pozadí je celá oblast obsazena `Label`. V závislosti na tom `HorizontalOptions` a `VerticalOptions` vlastnosti, že velikost může být výrazně větší než oblasti vyžadována k zobrazení textu. Barvu můžete experimentovat s různými hodnotami `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, a `VerticalTextAlignment` zobrazíte, jak ovlivňují velikost a umístění `Label`a velikost a umístění textu v rámci `Label`.

## <a name="the-color-structure"></a>Struktura barva

[ `Color` ](xref:Xamarin.Forms.Color) Struktura umožňuje zadat barvy jako červená zelená modré (RGB) hodnoty nebo hodnoty odstín, sytost, jas (HSL), nebo název barvy. Kanál alfa je také k dispozici k označení průhlednost.

Použití `Color` konstruktor k určení:

- [šedý stín](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [hodnota RGB](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [RGB hodnotu průhlednosti](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

Argumenty jsou `double` hodnoty od 0 do 1.

Několik statických metod můžete také použít k vytvoření `Color` hodnoty:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) pro `double` hodnoty RGB od 0 do 1
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) pro hodnoty RGB celé číslo od 0 do 255
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) pro `double` hodnoty RGB transparentnosti
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) pro celočíselné hodnoty RGB s transparentnosti
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) pro `double` HSL – hodnoty s transparentnosti
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) pro `uint` hodnota se vypočítá jako (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) pro `string` formát šestnáctkových číslic ve formě "#AARRGGBB" nebo "#RRGGBB" nebo "#ARGB" nebo "#RGB", kde každé písmeno odpovídá šestnáctková číslice pro platformu alpha červené, zelené a modré kanály. Tato metoda se používá pro převody barva XAML, jak je popsáno v primární [kapitola 7, XAML vs. kód](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Po vytvoření klikněte `Color` hodnota je neměnná. Vlastnosti barvy můžete získat následující vlastnosti:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Toto jsou všechny `double` hodnoty od 0 do 1.

`Color` Definuje také 240 veřejné statické pole jen pro čtení pro běžné barvy. V době, kdy byla zapsána knihy byly k dispozici pouze 17 běžné barvy.

Jiné veřejné statické pole jen pro čtení definuje barvu, která se všechny barevného kanálu nastaven na hodnotu nula:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Několik metod instance povolit úpravy existujících barva vytvořit novou barvu:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

Nakonec definujte dvě statické vlastnosti jen pro čtení hodnoty speciální barvy:

- [`Color.Default`](xref:Xamarin.Forms.Color.Default), nastavte všechny kanály na &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` slouží k vynucení platformy barevné schéma, a proto má odlišný význam v různých kontextech na různých platformách. Ve výchozím nastavení jsou platformy barevná schémata:

- iOS: tmavý text na pozadí světla
- Android: Světle v tmavém pozadí (v knize) nebo tmavý textu na pozadí světla (pro Material Design prostřednictvím AppCompat v **hlavní** větve v úložišti ukázka kódu)
- UPW: Tmavý text na pozadí světla
- Windows 8.1: Světle text v tmavém pozadí
- Windows Phone 8.1: Světle text v tmavém pozadí

`Color.Accent` Hodnota výsledky specifické pro platformu (a někdy uživatelsky volitelných) barvou, který je viditelný na tmavý a světlý na pozadí.

## <a name="changing-the-application-color-scheme"></a>Změna barevného schématu aplikace

Různé platformy mají barevné schéma výchozí, jak je znázorněno v seznamu výše.

Při cílení na Android, je možné se přepnout na schéma dark na light zadáním motiv světlý v souboru Android.Manifest.xml nebo podle [přidání AppCompat a Material Design](~/xamarin-forms/platform/android/appcompat.md).

Pro platformy Windows barevný motiv obvykle vybraný uživatelem, ale můžete přidat `RequestedTheme` atribut nastaven na hodnotu `Light` nebo `Dark` v souboru App.xaml platformy. Ve výchozím nastavení, obsahuje soubor App.xaml v projektu UWP `RequestedTheme` atribut nastaven na `Light`.

## <a name="font-sizes-and-attributes"></a>Velikost písma a atributy

Nastavte [ `FontFamily` ](xref:Xamarin.Forms.Label.FontFamily) vlastnost `Label` na řetězec, jako je "Times Roman" vybrat rodinu písem. Však budete muset zadat rodiny písem, který je podporovaný na konkrétní platformě a v tomto ohledu nejsou konzistentní platformy.

Nastavte [ `FontSize` ](xref:Xamarin.Forms.Label.FontSize) vlastnost `Label` k `double` pro zadání přibližně výška písma. Zobrazit [kapitola 5 řešení velikostí](chapter05.md), podrobné informace o volbě inteligentně velikostí písma.

Případně můžete získat jeden z několika velikostí písma přednastavených závislého na platformě. Statické [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) metoda a [přetížení](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) oba vracejí `double` hodnota velikosti písma pro platformu podle členů [ `NamedSize` ](xref:Xamarin.Forms.NamedSize)výčet ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [ `Micro` ](xref:Xamarin.Forms.NamedSize.Micro), [ `Small` ](xref:Xamarin.Forms.NamedSize.Small), [ `Medium` ](xref:Xamarin.Forms.NamedSize.Medium),  a [ `Large` ](xref:Xamarin.Forms.NamedSize.Large)). Hodnota vrácená z `Medium` člen není nutně stejné jako `Default`. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) ukázka zobrazí text s těmito s názvem velikosti.

Nastavte [ `FontAttributes` ](xref:Xamarin.Forms.Label.FontAttributes) vlastnost `Label` členovi těchto [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes) výčet, [ `Bold` ](xref:Xamarin.Forms.FontAttributes.Bold), [ `Italic` ](xref:Xamarin.Forms.FontAttributes.Italic), nebo [ `None` ](xref:Xamarin.Forms.FontAttributes.None). Můžete kombinovat `Bold` a `Italic` členy s C# bitového operátoru OR.

## <a name="formatted-text"></a>Formátovaný text

Ve všech příkladech zatím zobrazením celý text `Label` rovnoměrně formátována. Postup obměny formátování v rámci textový řetězec, nemají nastavený `Text` vlastnost `Label`. Místo toho nastavte [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) vlastnost na objekt typu [ `FormattedString` ](xref:Xamarin.Forms.FormattedString).

`FormattedString` má [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) vlastnost, která je kolekce [ `Span` ](xref:Xamarin.Forms.Span) objekty. Každý `Span` objekt má své vlastní [ `Text` ](xref:Xamarin.Forms.Span.Text), [ `FontFamily` ](xref:Xamarin.Forms.Span.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Span.FontSize), [ `FontAttributes` ](xref:Xamarin.Forms.Span.FontAttributes), [ `ForegroundColor` ](xref:Xamarin.Forms.Span.ForegroundColor), a [ `BackgroundColor` ](xref:Xamarin.Forms.Span.BackgroundColor) vlastnosti.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) ukázka znázorňuje použití `FormattedText` vlastnost pro jeden řádek textu, a [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) techniku pro celý odstavec, ukazuje, jak je znázorněno zde:

[![Trojitá snímek proměnné ve formátu odstavec](images/ch03fg06-small.png "proměnné ve formátu textu popisku")](images/ch03fg06-large.png#lightbox "proměnné ve formátu textu popisku")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) program používá jednu `Label` a `FormattedString` objektu pro zobrazení všech velikostí s názvem písma pro každou platformu.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitolu 3 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Ukázky kapitolu 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Ukázky kapitolu 3 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Popisek](~/xamarin-forms/user-interface/text/label.md)
- [Práci s barvami](~/xamarin-forms/user-interface/colors.md)
