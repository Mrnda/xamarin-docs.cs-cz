---
title: Tlačítka
description: UIButton třída se používá k reprezentaci různých různé styly tlačítka na iOS obrazovky. Tato část představuje různé možnosti pro práci s tlačítka v iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c2c33103c005a5ed567b1c4703846f887d824ac4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="buttons"></a>Tlačítka

_UIButton třída se používá k reprezentaci různých různé styly tlačítka na iOS obrazovky. Tato část představuje různé možnosti pro práci s tlačítka v iOS._

`UIButton`Třída reprezentuje ovládací prvek tlačítko v iOS. 

Tlačítko Vlastnosti lze upravit v `Properties Pad` návrháře iOS:


![](buttons-images/properties.png "Odsazení vlastnosti návrháře iOS")

## <a name="creating-a-button"></a>Vytváření tlačítek

UIButton lze vytvořit v prostřednictvím jenom pár řádků kódu.

Nejprve vytvořte instanci tlačítko Nový a zadejte typ tlačítko, které budete potřebovat:

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

UIButtonType musí být zadány jako jednu z následujících:

- **Systém** – to je standardní typ používané v iOS a typ, který budete používat nejčastěji.
- **DetailDisclosure** -uvede typu "zapnout" tlačítka pro skrytí nebo zobrazit podrobné informace.
- **InfoDark** -tmavý podrobné informace o zobrazeno tlačítko "i" v kruh.
- **InfoLight** -lehkým podrobné informace o zobrazeno tlačítko "i" v kruh.
- **AddContact** -zobrazí tlačítko jako tlačítko Přidat kontakt.
- **Vlastní** – umožňuje upravit vlastnosti několik tlačítka.

Další informace o typech tlačítko najdete v [tlačítko typy](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/) recepturách.

Potom zadejte na obrazovce velikost a umístění tlačítka. Příklad:

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

Můžete změnit text v tlačítka `SetTitle` vlastnost na tlačítko, které vyžaduje, abyste nastavili textového řetězce a `UIControlStyle`. Příklad:

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

Nastavení jiné vlastnosti pro každý stav umožňuje komunikovat (např Další informace o uživateli. Ujistěte se, barvu textu pro zakázaném stavu šedé). Můžete přepnout mezi stavy jednotlivých pomocí iOS Designer, nebo můžete provést programově. Další informace o nastavení text tlačítka a stavu, najdete v části [nastavit Text tlačítka](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/) recepturách.

## <a name="dealing-with-user-interactions"></a>Práci s uživatelské interakce


Tlačítka nejsou velmi užitečná, pokud budou dělat něco při kliknutí na! 

V iOS události tlačítek jsou téměř vždy touch události, jako použití komunikuje s tlačítkem na jejich obrazovce se ho. Seznam všech možných událostí UIControl [sem](https://developer.apple.com/documentation/uikit/uicontrolevents), ale je nejčastěji používanou událostí v systému iOS `TouchUpInside`. Potom můžete vytvořit obslužnou rutinu události něco udělat po stisknutí tlačítka:


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>Přidávání událostí v iOS návrháře
 
Na kartě události v vlastnost pro přidání události pro ovládací prvky.

Vyberte události a zadejte název nové obslužné rutiny nebo vyberte jednu ze seznamu. To vytvoří nové částečné metody ve třídě řadiče zobrazení.

![Karta událostí](buttons-images/image1.png)

## <a name="styling-a-button"></a>Práce se styly tlačítka

UIButtons jsou jiné než většina UIKit ovládací prvky v tom, že mají stavu, takže nelze stačí jednoduše změnit název, budete muset změnit pro každou `UIControlState`. Nastavení barvy název a barvu stínu se provádí podobným způsobem:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Kromě toho můžete použít s atributy text jako nadpis na tlačítko. Příklad:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Vlastní tlačítka typy


Když nastavíte `Custom` tlačítko typ objektu nemá žádné výchozí vykreslování. Vzhled tlačítka můžete nakonfigurovat nastavení bitovou kopii pro různé stavy. Například následující kód ukazuje, jak přidat jiné bitové kopie pro `Normal`, `Highlighted` a `Selected` stavy:


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


V závislosti na tom, jestli je uživatel klepnou na tlačítko, nebo Ne, bude vykreslen jako jeden z následujících bitových kopií (`Normal`, `Highlighted` a `Selected` stavy v uvedeném pořadí):


![](buttons-images/image22.png "Stav UIButton normální")
![](buttons-images/image23.png "UIButton stavu zvýrazněná")
![](buttons-images/image24.png "UIButton stav vybrané")

Další informace o práci s vlastních tlačítek, najdete v části [Image použít pro tlačítko](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/).


## <a name="related-links"></a>Související odkazy

- [UIButton sešitu](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
