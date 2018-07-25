---
title: Popisky v Xamarin.iosu
description: Tento dokument popisuje, jak použít popisky v Xamarin.iOS. Popisuje postup vytvoření popisky prostřednictvím kódu programu a v IOS designeru.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: b52bdbd41eaafbc5e6c78e1f8514b701fd78bd6b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241910"
---
# <a name="labels-in-xamarinios"></a>Popisky v Xamarin.iosu

`UILabel` Ovládací prvek se používá pro zobrazení jedné a více řádky, přečtěte si pouze text. 

## <a name="implementing-a-label"></a>Implementace popisek

Po vytvoření instance se vytvoří nový popisek [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Popisky a scénářů

Popisek můžete také přidat do vašeho uživatelského rozhraní při použití v iOS designeru. Vyhledejte **popisek** v **nástrojů** a přetáhněte jej do zobrazení:

![Popisek v panelu nástrojů](labels-images/image3.png)

Na panelu Vlastnosti je možné upravit následující vlastnosti:

![Panel vlastnosti popisku](labels-images/image2.png)

- **Kontext text** – prostý nebo s atributy. Prostý text vám umožní nastavit [atributů formátování](#Formatting_Text_and_Label) na celý řetězec. S atributy texty vám umožní nastavit formátování pro jiné znaky nebo slova v řetězci.
- **Barva písma, zarovnání** – formátování atributů, který lze použít pro popisek.
- **Řádky** – nastaví počet řádků, které může mít rozsah popisku. Nastavíte na 0 pro povolení popisek, který můžete použít libovolný počet řádků podle potřeby.
- **Chování** – můžete nastavit na povoleno nebo zvýrazněný. Je povolené ve výchozím nastavení zakázaný text se zobrazí v světlejší šedou barvu. Zvýrazněný je ve výchozím nastavení zakázané a umožňuje tento popisek se měl překreslit zvýrazněné stavem při výběru uživatelem.
- **Baselane a zalomení řádku** – 
    - Basline Určuje, jak tento text bude umístěn, pokud se liší od verze zadaná velikost písma.
    - Konce řádků zjistěte, jak bude řetězec zabalené nebo zkrácen, pokud je delší než jeden řádek.
- **Automatické zmenšování** – Určuje, jak se v rámci popisku, minimalizovat velikost písma v případě potřeby.
- **Zvýrazní, stín posun** – umožňuje nastavit barvu zvýrazněnou a stínové a Posunutí stínu.

## <a name="truncating-and-wrapping"></a>Zkracování a balení

Pro informace o používání řádku přestane fungovat v iOS, najdete [Truncate and Zalamovat text](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) předpisu.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Formátování textu a popisek

Chcete-li formátovací řetězec, který používáte v popisku můžete buď sadu atributů na celý řetězec formátování nebo můžete použít s atributy řetězce. Následující příklady ukazují, jak je implementovat:

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

Další informace o stylu textu s využitím `NSAttributedString` odkazovat [styl textu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) předpisu.

Ve výchozím nastavení mají popisky `Enabled` nastavena na hodnotu true, ale je možné ji nastavit na hodnotu zakázáno a přidělit uživateli pokyn, že je zakázána určité ovládacího prvku:

```csharp
label.Enabled = false;
```

Tím se nastaví popisek světla šedou barvu, jak je znázorněno na následujícím obrázku obrazovky omezení v iOS:

![Zakázané tlačítko v Iosu](labels-images/image1.png)

Můžete také nastavit text barvy zvýraznění a stínové textu popisku pro další efekty:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Zobrazuje text takto:

![Nastavení zvýraznění a stínové v textu](labels-images/image4.png)

Další informace o změně písma UILabel najdete [Změna písma](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) předpisu.





