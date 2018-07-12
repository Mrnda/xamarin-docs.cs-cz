---
title: Tlačítka v Xamarin.iOS
description: Třída tlačítka uživatelského rozhraní se používá k reprezentaci různých různé styly tlačítka na obrazovkách s Iosem. Tato příručka představuje různé možnosti pro práci s tlačítky v iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2018
ms.openlocfilehash: 32f6330ad2fddc2e8386d6e574918a011f3bebad
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986001"
---
# <a name="buttons-in-xamarinios"></a>Tlačítka v Xamarin.iOS

V Iosu `UIButton` třída reprezentuje ovládací prvek tlačítko.

Vlastnosti tlačítka lze změnit buď programově, nebo **oblasti vlastnosti** IOS Designer:

![Panel Vlastnosti systému iOS Designer](buttons-images/properties.png "The panel Vlastnosti systému iOS Designer")

## <a name="creating-a-button-programmatically"></a>Vytvoření tlačítka prostřednictvím kódu programu

A `UIButton` se dají vytvářet pomocí jenom pár řádků kódu.

- Vytvoření instance tlačítka a určit její typ:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Typ tlačítka je určená `UIButtonType`:

  - `UIButtonType.System` -Tlačítko pro obecné účely
  - `UIButtonType.DetailDisclosure` -Označuje dostupnost s podrobnými informacemi, obvykle o konkrétní položce v tabulce
  - `UIButtonType.InfoDark` -Označuje dostupnost informací o konfiguraci; Tmavý
  - `UIButtonType.InfoLight` -Označuje dostupnost informací o konfiguraci; Světlé
  - `UIButtonType..AddContact` – Označuje, že můžete přidat kontakt
  - `UIButtonType.Custom` -Přizpůsobitelné tlačítko

  Další informace o typech jiné tlačítko podívejte se na:
  
  - [Vlastní typy tlačítek](#custom-button-types) část tohoto dokumentu
  - [Tlačítko typy](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) předpisu
  - Od společnosti Apple [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/).

- Definujte velikost a umístění tlačítka:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Nastavte text tlačítka. Použití `SetTitle` metodu, která vyžaduje text a `UIControlState` hodnotu:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Další informace o používání stylů pro tlačítko a nastavení jeho textu najdete:

  - [Používání stylů pro tlačítko](#styling-a-button) část tohoto dokumentu
  - [Nastavit text na tlačítku](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) předpisu.

## <a name="handling-a-button-tap"></a>Zpracování klepnutím na tlačítko

Reakce na klepnutím na tlačítko, zadejte na tlačítku pro obslužnou rutinu `TouchUpInside` události:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` není událost pouze k dispozici tlačítko. `UIButton` je podřízené třídy `UIControl`, která definuje [mnoho různých událostí](https://developer.xamarin.com/api/type/UIKit.UIControlEvent/).

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>Chcete-li určit obslužné rutiny události tlačítek pomocí v iOS designeru

Použití **události** karty **oblasti vlastnosti** Chcete-li určit obslužné rutiny události pro tlačítko různé události.

Pro příslušnou událost zadejte název nové obslužné rutiny události nebo vyberte ze seznamu. Tím se vytvoří obslužnou rutinu události v kódu pro kontroler zobrazení tlačítka.

![Události kartu panel vlastnosti](buttons-images/image1.png "události kartu panelu Vlastnosti")

## <a name="styling-a-button"></a>Používání stylů pro tlačítko

`UIButton` ovládací prvky mohou existovat v celou řadou různých stavů, každý zadaný ve `UIControlState` hodnota – `Normal`, `Disabled`, `Focused`, `Highlighted`atd. Každý stav mohou být zadány jedinečný stylu, určeného prostřednictvím kódu programu nebo v IOS designeru.

> [!NOTE]
> Úplný seznam všech `UIControlState` hodnoty, se podívejte na [ `UIKit.UIControlState enumeration` ](https://developer.xamarin.com/api/type/UIKit.UIControlState/) dokumentaci.

Chcete-li například nastavit barvu názvu a Barva stínu pro `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Následující kód nastaví název tlačítka na řetězec s atributy (stylizované) pro `UIControlState.Normal` a `UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Typy vlastních tlačítek

Tlačítka s `UIButtonType` z `Custom` mít žádné výchozí styly. Nicméně je možné konfigurovat jeho vzhled pomocí nastavení image pro různé stavy:

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

V závislosti na tom, zda uživatel se dotýká na tlačítko, nebo Ne, se bude vykreslovat jako jeden z následujících imagí (`UIControlState.Normal`, `UIControlState.Highlighted` a `UIControlState.Selected` stavy, v uvedeném pořadí):

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted") 
 ![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

Další informace o práci s vlastní tlačítky, najdete [použít obraz pro tlačítko](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) předpisu.

## <a name="related-links"></a>Související odkazy

- [Sešit tlačítka uživatelského rozhraní](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
