---
title: Práce s tvOS tlačítka v Xamarinu
description: Tento dokument popisuje, jak pracovat s tlačítka v tvOS aplikace vytvořené s nástroji Xamarin. Popisuje, jak pracovat s tlačítka v kódu a scénářů a zkontroluje postupy k úpravě stylu tlačítko.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: 3de732e9eee696ce21ffc5526afd44f29695a313
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789382"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Práce s tvOS tlačítka v Xamarinu

Použít instanci systému `UIButton` třídy za účelem vytvoření může získat fokus, které lze vybírat tlačítka v okně tvOS. Když uživatel vybere tlačítko, odešle zprávu akce na objekt cíle povolit vstup vaší Xamarin.tvOS reakce aplikace pro uživatele.

[![](buttons-images/buttons01.png "Příklad tlačítka")](buttons-images/buttons01.png#lightbox)

Další informace o práci s fokusem a navigaci se vzdálenými Siri, najdete v tématu naše [práce s navigační a výběr](~/ios/tvos/app-fundamentals/navigation-focus.md) a [Siri vzdálené a Bluetooth řadiče](~/ios/tvos/platform/remote-bluetooth.md) dokumentaci.

<a name="About-Buttons" />

## <a name="about-buttons"></a>O tlačítkách

Tlačítka v tvOS, se používají pro akce specifické pro aplikace a může obsahovat název, ikonou nebo obojí. Jako uživatel přejde aplikace pomocí uživatelského rozhraní [Siri vzdálené](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), deaktivaci k dané tlačítko díky tomu změnit barvu textu a pozadí. Stín platí také pro tlačítko přidáním 3D efektu proto to vypadá, zvýšení výše zbytek uživatelské rozhraní.

[![](buttons-images/buttons01.png "Příklad tlačítka")](buttons-images/buttons01.png#lightbox)

Společnost Apple má následující návrhy pro práci s tlačítka:

- **Použití a název nebo ikonu** – při na obou ikonu a název můžou být součástí tlačítka, místo je omezená, zkuste není kombinací obou možností.
- **Jasně označit destruktivní tlačítka** – Pokud se provádí destruktivní tlačítko akce (například odstranění souboru), jasně označte ji jako takový pomocí textu a ikona. Destruktivní akce by měla vždy k dispozici [výstrahy](~/ios/tvos/user-interface/alerts.md) požadavek uživatele umožňují omezit akce.
- **Nemáte pomocí tlačítka Zpět** – tlačítko nabídce na vzdáleném Siri se používá k vrácení na předchozí obrazovku. Jedinou výjimkou tohoto pravidla se pro nákupy v aplikaci nebo destruktivní akce, kde **zrušit** tlačítko má být zobrazen.

Další informace o práci se fokus a navigace, najdete v tématu naše [práce s navigační a výběr](~/ios/tvos/app-fundamentals/navigation-focus.md) dokumentaci.

<a name="Button-Icons" />

### <a name="button-icons"></a>Ikony pro tlačítko

Apple doporučuje použít jednoduchá, snadno rozpoznatelném obrázků pro ikony tlačítko. Jsou příliš složité ikony těžko rozpoznat na obrazovce TV napříč místnosti na pohovce, proto zkuste použít můžete získat na nápad nejjednodušší reprezentace. Kdykoli je to možné, používat standardní, také znát obrázků pro ikony (například Lupa hledaný).

<a name="Button-Titles" />

### <a name="button-titles"></a>Tlačítko názvy

Při vytváření názvy pro vaše tlačítka, Společnost Apple má následující návrhy:

- **Zobrazit popisný Text pod ikony tlačítka** – kde je to možné, místní jasným a popisný text pod ikonou pouze tlačítka pro další get na tlačítko účel napříč.
- **Používat příkazy nebo akce frází pro nadpis** -jasně stavu umístit akci, která bude trvat, když uživatel klikne na tlačítko.
- **Použijte název stylu malá a velká písmena** – s výjimkou články, spojky nebo předložky (čtyři písmena nebo méně), všechna slova na tlačítko název by měl zadat velkými písmeny.
- **Použít krátké Title k bodu** -nejkratší možné tento problém používat k popisu tlačítka akce.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Tlačítka a scénářů

Nejjednodušší způsob, jak pracovat s tlačítka v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře Xamarin pro iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **tlačítko** z **knihovny** na zobrazení: 

    [![](buttons-images/storyboard01.png "Tlačítko")](buttons-images/storyboard01.png#lightbox)
1. V **Explorer vlastnosti**, můžete upravit několik vlastností tlačítka jeho **název** a **barvy**: 

    [![](buttons-images/storyboard02.png "Vlastnosti tlačítka")](buttons-images/storyboard02.png#lightbox)
1. V dalším kroku přepnout **kartu události** a navázání **událostí** z **tlačítko** a pojmenujte ji `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "Na kartě události")](buttons-images/storyboard03.png#lightbox)
1. Se bude automaticky přepnout do `ViewController.cs` zobrazení, kde můžete umístit nová akce kódu pomocí **až** a **dolů** klávesy se šipkami: 

    [![](buttons-images/storyboard04.png "Uvedení Nová akce kódu")](buttons-images/storyboard04.png#lightbox)
1. Stiskněte **Enter** a vyberte umístění: 

    [![](buttons-images/storyboard05.png "Editor kódu")](buttons-images/storyboard05.png#lightbox)
1. Uložte změny do všechny soubory.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **tlačítko** z **knihovny** na zobrazení: 

    [![](buttons-images/storyboard01vs.png "Tlačítko")](buttons-images/storyboard01vs.png#lightbox)
1. V **Explorer vlastnosti**, můžete upravit několik vlastností tlačítka jeho **název** a **barvy**: 

    [![](buttons-images/storyboard02vs.png "Průzkumník vlastnosti")](buttons-images/storyboard02vs.png#lightbox)
1. V dalším kroku přepnout **kartu události** a navázání **událostí** z **tlačítko** a pojmenujte ji `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "Na kartě události")](buttons-images/storyboard03vs.png#lightbox)
1. Uložte změny do všechny soubory.



Upravit View Controller (například `ViewController.cs`) souboru a přidejte následující kód pro zpracování na tlačítko Vybrat:


```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

Tak dlouho, dokud na tlačítko `Enabled` vlastnost je `true` a není předmětem jiného ovládacího prvku nebo zobrazení, můžete provedeny položce vybraný pomocí vzdáleného Siri. Pokud uživatel vybere tlačítko a klikne na povrch Touch `ButtonPressed` by byl proveden akce definovaná výše.

> [!IMPORTANT]
> I když je možné přiřadit akce, jako `TouchUpInside` k `UIButton` v iOS Návrhář při vytváření **obslužné rutiny události**, ho nebude nikdy volat protože Apple TV nemá touch obrazovky nebo podporují touch události. Je třeba použít výchozí **typ akce** při vytváření **akce** pro tvOS prvky uživatelského rozhraní.




Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Tlačítka a kódu

Volitelně můžete `UIButton` lze vytvořit v kódu jazyka C# a přidat do aplikace tvOS zobrazení. Příklad:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

Když vytvoříte novou `UIButton` v kódu, můžete zadat jeho `UIButtonType` jako jednu z následujících:

- **Systém** – to je standardní typ tlačítka předložený tvOS a typ, který budete používat nejčastěji.
- **DetailDisclosure** -uvede typu "zapnout" tlačítka pro skrytí nebo zobrazit podrobné informace.
- **InfoDark** -tmavý podrobné informace o zobrazeno tlačítko "i" v kruh.
- **InfoLight** -lehkým podrobné informace o zobrazeno tlačítko "i" v kruh.
- **AddContact** -zobrazí tlačítko jako tlačítko Přidat kontakt.
- **Vlastní** – umožňuje upravit vlastnosti několik tlačítka.

Dále můžete definovat na obrazovce velikost a umístění tlačítka. Příklad:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Potom nastavte název pro tlačítko. `UIButtons` jiné než u většiny `UIKit` ovládací prvky v tom, že mají stav, stačí jednoduše nemůžete změnit název, budete muset změnit ho danou `UIControlState`. Příklad:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Pak pomocí `AllEvents` událost zobrazíte, když uživatel klikne na tlačítko. Příklad:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Nakonec přidejte tlačítko do zobrazení a zobrazit ji:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> I když je možné přiřadit akce, jako `TouchUpInside` k `UIButton`, se nebude nikdy volat protože Apple TV nemá touch obrazovky nebo podporují touch události. Je třeba použít události, jako **AllEvents** nebo **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Práce se styly tlačítka

tvOS poskytuje několik vlastností `UIButton` , lze použít k poskytování její název a stylu s věcmi, jako barvu pozadí a obrázků.

<a name="Button-Titles" />

### <a name="button-titles"></a>Tlačítko názvy

Jak jsme viděli výše, `UIButtons` jsou jiné než u většiny `UIKit` ovládací prvky v tom, že mají stav, stačí jednoduše nemůžete změnit název, budete muset změnit pro danou `UIControlState`. Příklad:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Můžete nastavit název barvu pro tlačítko pomocí `SetTitleColor` metoda. Příklad:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

A můžete upravit název stínové pomocí `SetTitleShadowColor`. Příklad:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Můžete nastavit stínové název chcete změnit z *Engraved* k *reliéf* při tlačítko zvýrazní pomocí následujícího kódu:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Kromě toho můžete použít s atributy text jako nadpis na tlačítko. Příklad:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Obrázky tlačítka

A `UIButton` může mít bitovou kopii k němu připojen a můžete použít bitovou kopii jako jeho pozadí.

Nastavit obrázek pozadí pro tlačítka danou `UIControlState`, použijte následující kód:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Nastavte `AdjustsImageWhenHiglighted` vlastnost `true` k vykreslení světlejší bitovou kopii, když je označený na tlačítko (Toto je výchozí hodnota). Nastavte `AdjustsImageWhenDisabled` vlastnost `true` k vykreslení tmavšího bitovou kopii, když je zakázáno na tlačítko (znovu, to je výchozí hodnota).

Chcete-li nastavení obrázku zobrazovaného na tlačítko, použijte následující kód:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Použití `TintColor` vlastnost nastavující odstín barvy, který se použije pro nadpis a obrázek na tlačítko. Pro tlačítka z `Custom` typu, tato vlastnost nemá žádný vliv, je nutné implementovat `TintColor` chování sami.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s tlačítka uvnitř Xamarin.tvOS aplikace. Vám ukázal, jak pracovat s tlačítka v iOS Designer a postup vytvoření tlačítka v kódu jazyka C#. Nakonec se vám ukázal, jak upravit název tlačítko a změňte jeho styl a vzhled.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
