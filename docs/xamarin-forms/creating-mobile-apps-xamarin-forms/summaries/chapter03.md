---
title: "Souhrn kapitoly 3. Hlouběji do textu"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9d283a4136a7cdfe39ea0b2da65273332fd47b00
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Souhrn kapitoly 3. Hlouběji do textu

Jsou zde popsány tato kapitola [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) zobrazení do větší hloubky, včetně barvy, písma a formátování.

## <a name="wrapping-paragraphs"></a>Zabalení odstavců

Když [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnost `Label` obsahuje dlouhý text `Label` automaticky zalomen na více řádků jak je předvedeno podle [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) ukázka. Můžete vložit Unicode kódů, třeba '\u2014' dlouhé pomlčky nebo znaků jazyka C# jako "\r' pro přerušení na nový řádek.

Při [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnosti `Label` jsou nastaveny na `LayoutOptions.Fill`, celková velikost `Label` se řídí prostor, jejímu kontejneru zpřístupní. `Label` Se říká, že *omezené*. Velikost `Label` je velikost svého kontejneru.

Když `HorizontalOptions` a `VerticalOptions` vlastnosti jsou nastaveny na hodnoty jiné než `LayoutOptions.Fill`, velikost `Label` se řídí na místo pro text, až velikost jeho kontejneru zpřístupní pro vykreslení `Label`. `Label` Se říká, že *neomezeným* a určuje vlastní velikost.

(Poznámka: podmínky *omezené* a *neomezeným* může být counter-intuitive, protože je obecně menší než omezené zobrazení neomezeným zobrazení. Navíc tyto podmínky se nepoužívají konzistentně v časná kapitolách knihy.)

Zobrazení například `Label` může být omezené v jednou dimenzí a neomezeného v dalších. A `Label` bude pouze Zalamovat text na více řádků případě, že je omezen vodorovně.

Pokud `Label` je omezené, může zabírají mnohem více místa, než je třeba pro text. Text může být umístěn uvnitř oblasti celkové `Label`. Nastavte [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) vlastnost členem [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) – výčet ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/), nebo [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)) k řízení zarovnání všechny řádky odstavce. Výchozí hodnota je `Start` a zarovná text doleva.

Nastavte [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) vlastnost členem `TextAlignment` výčtu textu v horní části, center nebo dolní části oblasti obsazena `Label`.

Nastavte [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) vlastnost členem [ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) – výčet ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/), [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/), [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/), [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/), [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/), nebo [ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/)) na ovládací prvek, jak více řádků v zalomení odstavce nebo se zkrátí.

## <a name="text-and-background-colors"></a>Barva textu a pozadí

Nastavte [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) a [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) vlastnosti `Label` k [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) hodnoty k řízení barvu textu a pozadí.

`BackgroundColor` Se vztahuje na pozadí oblasti celý obsazena `Label`. V závislosti na tom `HorizontalOptions` a `VerticalOptions` vlastnosti, že velikost může být podstatně větší než oblasti vyžadována k zobrazení textu. Barva můžete experimentovat s různými hodnotami `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, a `VerticalTextAlignment` zobrazíte jejich vliv na velikost a umístění `Label`a velikost a umístění textu v rámci `Label`.

## <a name="the-color-structure"></a>Struktura barev

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Struktura umožňuje určit barvy jako hodnoty červená-zelená-modrá (RGB), nebo hodnoty HSL Hue sytost a jas (–) nebo pomocí názvu barvy. Dále je dostupný k označení průhlednost alfa kanál.

Použití `Color` k určení:

- [šedý stín](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- [hodnoty RGB](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- [hodnoty RGB s průhlednosti](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

Argumenty jsou `double` hodnoty v rozsahu od 0 do 1.

Můžete taky použít několik statické metody pro vytvoření `Color` hodnoty:

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) pro `double` RGB hodnoty od 0 do 1
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) pro hodnoty RGB celé číslo od 0 do 255
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) pro `double` hodnoty RGB s průhlednosti
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) pro hodnoty RGB celé číslo s průhlednosti
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) pro `double` HSL – hodnoty s průhlednosti
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) pro `uint` hodnota se počítá jako (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) pro `string` formátu šestnáctkových číslic ve tvaru "#AARRGGBB" nebo "#RRGGBB" nebo "#ARGB" nebo "#RGB", kde každý písmeno odpovídá šestnáctková číslice pro platformu alpha red, zelené a modré kanály. Tato metoda je primární používá pro převod XAML barvy, jak je popsáno v [kapitoly 7, XAML oproti kód](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Po vytvoření, `Color` hodnota se nedá změnit. Charakteristiky barvy můžete získat z následujících vlastností:

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

Toto jsou všechny `double` hodnoty v rozsahu od 0 do 1.

`Color` také definuje 240 veřejných statických jen pro čtení polí pro běžné barvy. V době, kdy byla zapsána knihy nebyly k dispozici pouze 17 společné barvy.

Jiné veřejné statické jen pro čtení pole definuje barvu, která s všechny kanály barev nastavená na nulovou hodnotu:

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

Několik metod instance povolit úprava existující barvy, která se má vytvořit novou barvu:

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

Nakonec dvě statické vlastnosti jen pro čtení definovat hodnoty barvy speciální:

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), všechny kanály nastavena na & #x 2013; 1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` slouží k vynucení platformě barevné schéma, a proto má jiný význam v různých kontextech na různých platformách. Ve výchozím nastavení jsou platformy barevná schémata:

- iOS: tmavý text na světla pozadí
- Android: Světle text na pozadí Tmavý (v kniha) nebo tmavý text na světle pozadí (návrh materiálu prostřednictvím kompatibility aplikace v **hlavní** větev úložiště ukázka kódu)
- UWP: Tmavý text na světla pozadí
- Windows 8.1: Světla text na tmavý pozadí
- Windows Phone 8.1: Světla text na tmavý pozadí

`Color.Accent` Hodnota má za následek specifické pro platformu (a někdy uživatel vybírat) barvu, která se zobrazí na tmavý nebo světla pozadí.

## <a name="changing-the-application-color-scheme"></a>Změna barevné schéma aplikace

Různých platformách mít barevné schéma výchozí, jak je vidět v seznamu výš.

Pokud je cílem Android, je možné přepnout na schéma světlý na light zadáním motiv světlý v souboru Android.Manifest.xml, případně podle [přidání kompatibility aplikace a návrh materiálu](~/xamarin-forms/platform/android/appcompat.md).

Na platformách systému Windows je barevný motiv normálně vybraný uživatelem, ale můžete přidat `RequestedTheme` atribut nastaven na hodnotu `Light` nebo `Dark` v souboru App.xaml platformu. Ve výchozím nastavení, obsahuje soubor App.xaml v projektu UPW `RequestedTheme` atribut nastaven na `Light`.

## <a name="font-sizes-and-attributes"></a>Velikosti písem a atributy

Nastavte [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) vlastnost `Label` na řetězec, jako je například "Times Roman" a vyberte v dané rodině písem. Však budete muset zadat rodiny písem, která je podporována v konkrétní platformu, a v tomto ohledu nejsou konzistentní platformy.

Nastavte [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) vlastnost `Label` k `double` pro zadání přibližně výška písmo. V tématu [kapitoly 5, plánování práce s velikostí](chapter05.md), další informace o počítači inteligentně výběr velikosti písem.

Případně můžete získat jeden z několika velikostí přednastavené písma závislé na platformu. Statické [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) metoda a [přetížení](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) i vrátit `double` hodnota velikosti písma, které jsou vhodné pro platformu podle členy [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)– výčet ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/), [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/), [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/), [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/),  a [ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/)). Hodnota vrácená z `Medium` člen není nutně stejné jako `Default`. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) ukázka zobrazí text pomocí těchto s názvem velikosti.

Nastavte [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/) vlastnost `Label` na člena z těchto [ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/) výčtu [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/), [ `Italic` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/), nebo [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/). Můžete kombinovat `Bold` a `Italic` členy s C# bitový operátor OR.

## <a name="formatted-text"></a>Formátovaný text

Ve všech příklady dosavadní zobrazí celý text ve `Label` jednotně naformátován. Ke změně formátování v rámci textový řetězec, není nastavený `Text` vlastnost `Label`. Místo toho nastavte [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) vlastností pro objekt typu [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/).

`FormattedString` má [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) vlastnost, která je kolekce [ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) objekty. Každý `Span` objekt má svou vlastní [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), a [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) vlastnosti.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) ukázka ukazuje, jak pomocí `FormattedText` vlastnost pro jeden řádek textu, a [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) technika pro celý odstavec, ukazuje, jak je vidět tady:

[![Trojitá snímek proměnné formátu odstavce](images/ch03fg06-small.png "proměnná formátovaný Text popisku")](images/ch03fg06-large.png "proměnná formátovaný Text popisku")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) program používá jeden `Label` a `FormattedString` objekt, který chcete zobrazit vše z pojmenované velikostí pro každou platformu.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 3 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Ukázky kapitoly 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Kapitola 3 F # – ukázky](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Popisek](~/xamarin-forms/user-interface/text/label.md)
- [Práce s barvy](~/xamarin-forms/user-interface/colors.md)
