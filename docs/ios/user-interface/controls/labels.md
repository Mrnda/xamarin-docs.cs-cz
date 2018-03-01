---
title: Popisky
ms.topic: article
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 695d02c5fa0477053cd95d73e1b738332d14f0f9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="labels"></a>Popisky

`UILabel` Řízení se používá pro zobrazení jeden a více řádků číst pouze text. Tento článek obsahuje následující témata:

- [Implementace štítek](#Implementing_a_Label)
- [Zkracování a zabalení](#Truncating_and_Wrapping)
- [Formátování textu a popisku](#Formatting_Text_and_Label)

## <a name="implementing-a-label"></a>Implementace štítek

Po vytvoření instance se vytvoří nový štítek [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Popisky a scénářů

Při použití návrháře iOS, můžete přidat také popisek pro uživatelské rozhraní. Vyhledejte **popisek** v **sada nástrojů** a přetáhněte jej do zobrazení:

![Návěští v sadě nástrojů](labels-images/image3.png)

Na panelu pro vlastnosti lze upravit následující vlastnosti:

![Popisek vlastnost panely](labels-images/image2.png)

- **Text kontextu** – prostý nebo s atributy. Prostý text můžete nastavit [atributů formátování](#Formatting_Text_and_Label) pro celý řetězec. S atributy texty umožňuje nastavit formátování pro jiné znaky nebo slova v řetězci.
- **Barvy, písma, zarovnání** – atributy formátování, který lze použít k popisku.
- **Řádky** – nastaví počet řádků, které může mít rozsah popisku. Tuto možnost nastavíte na 0 pro povolení štítek, který chcete použít libovolný počet řádků, podle potřeby.
- **Chování** – můžete nastavit na hodnotu povoleno nebo Highlighted. Povolena je ve výchozím nastavení zakázaný text se zobrazí v světlejší šedou barvu. Zvýrazněná ve výchozím nastavení vypnutá a umožňuje štítek, který chcete překreslen pomocí zvýrazněná stavu, pokud je vybraná uživatelem.
- **Baselane a koncem řádku** – 
    - Basline Určuje, jak tento text bude umístěna, pokud se liší od verze zadaná velikost písma.
    - Konce řádků určit, jak bude řetězec zabalené nebo zkrácen, pokud je delší než jeden řádek.
- **Autoshrink** – Určuje, jak bude minimalizovat velikost písma v rámci štítku, v případě potřeby.
- **Zvýrazní, stín, posun** – umožňuje nastavit barvu Hightlighted a stínové a posun stínu.

## <a name="truncating-and-wrapping"></a>Zkracování a zabalení

Informace o používání řádku dělí na iOS, naleznete na [zkrátit a zalamování řádků](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/) recepturách.

## <a name="formatting-text-and-label"></a>Formátování textu a popisku

Formátování řetězce, který používáte v popisku můžete buď sadu atributů pro celý řetězec formátování nebo můžete použít s atributy řetězce. Následující příklady ukazují, jak implementovat tyto:

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Další informace o používání stylů text `NSAttributedString` odkazovat [styl textu](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/) recepturách.

Ve výchozím nastavení mají štítky `Enabled` nastaven na hodnotu true, ale je možné nastavit na zakázána a přidělit uživateli nápovědu, aby bylo zakázané určité ovládacího prvku:

```csharp
label.Enabled = false;
```

Toto nastaví popisek světla šedou barvu, jak je znázorněno na následujícím obrázku Příklad obrazovky omezení v iOS:

![Zakázané tlačítko v iOS](labels-images/image1.png)

Můžete také nastavit zvýraznění a stín barvy textu na text popisku pro další důsledky:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Zobrazuje text takto:

![Zvýrazněte a stínové sadu na text](labels-images/image4.png)

Další informace o Změna písma UILabel, najdete v části [Změna písma](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/) recepturách.





