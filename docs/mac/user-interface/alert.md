---
title: Upozornění
description: Tento článek se zabývá práce s výstrahami v aplikaci Xamarin.Mac. Popisuje vytváření a zobrazování výstrah z kódu jazyka C# a reagovat na akce uživatelů.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a451d0a5535915d9e52f687ae07ea028c0ccd5ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="alerts"></a>Upozornění

_Tento článek se zabývá práce s výstrahami v aplikaci Xamarin.Mac. Popisuje vytváření a zobrazování výstrah z kódu jazyka C# a reagovat na akce uživatelů._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup ke stejné výstrahy které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. 

Výstraha je zvláštní druh dialog, který se zobrazí, když dojde k vážný problém (například chybu) nebo jako upozornění (například příprava k odstranění souboru). Vzhledem k tomu, že výstraha je dialogové okno, taky vyžaduje odpověď uživatele předtím, než je možné uzavřít.

[![](alert-images/alert06.png "Příklad výstrahy")](alert-images/alert06.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s výstrahami v aplikaci Xamarin.Mac. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Úvod do výstrahy

Výstraha je zvláštní druh dialog, který se zobrazí, když dojde k vážný problém (například chybu) nebo jako upozornění (například příprava k odstranění souboru). Vzhledem k tomu výstrahy přerušit uživatele, protože musí být odmítnuta před pokračováním na uživatele s jejich úkolů, vyhněte se zobrazení výstrahy, pokud to není nezbytně nutné.

Apple navrhnout podle následujících pokynů:

- Nepoužívejte výstrahu jenom uživatelům informace.
- Nezobrazují výstrahy pro běžné vrátit akcí. I když tato situace může způsobit ztrátu dat.
- Pokud situaci worthy výstrahy, nepoužívejte další prvek uživatelského rozhraní nebo metoda můžete ho zobrazit.
- Ikona upozornění: měli šetřit.
- Popis výstrahy situace jasně a stručně v upozornění.
- Výchozí tlačítko název by měl odpovídat akci, kterou popisují v výstražné zprávy.

Další informace najdete v tématu [výstrahy](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Anatomie výstrahu

Jak jsme uvedli výše, aplikace uživatele, když dojde k závažné chybě nebo upozornění na potenciální ztrátě dat (například zavření souboru neuložené) se mají výstrahy. V Xamarin.Mac výstraha se například vytvoří v kódu jazyka C#:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

Výše uvedený kód zobrazí výstrahu s ikonou aplikace přes ikony upozornění, název, upozornění a jedinou **OK** tlačítko:

[![](alert-images/alert01.png "Výstraha s tlačítko OK")](alert-images/alert01.png#lightbox)

Apple poskytuje několik vlastností, které lze použít k přizpůsobení výstrahu:

- **AlertStyle** definuje typ výstrahy jako jednu z následujících:
    - **Upozornění** – slouží jako upozornit uživatele aktuální nebo brzké událost, která není důležité. Toto je výchozí styl.
    - **Informační** – slouží jako uživatele varuje ohledně aktuální nebo brzké událost. V současné době není žádný viditelný rozdíl mezi **upozornění** a **informativní**
    - **Kritické** – slouží jako uživatele varuje ohledně závažnosti důsledků na blížící se událost (například odstranění souboru). Tento typ upozornění by měl být šetřit.
- **MessageText** -Toto je hlavní zprávě, nebo název výstrahy a rychle měli definovat situaci uživatele.
- **InformativeText** -tělo výstrahy, kde by měl jasně definovat situaci a nabízet použitelnou možnosti pro uživatele.
- **Ikona** -umožňuje vlastní ikonu, která se zobrazí uživateli.
- **HelpAnchor** & **ShowsHelp** -umožňuje výstrahu, kterou chcete být vázáno do aplikace HelpBook a zobrazit nápovědu pro výstrahy.
- **Tlačítka** – ve výchozím nastavení, výstrahu má jenom **OK** tlačítko ale **tlačítka** kolekce umožňuje přidat další možnosti podle potřeby.
- **ShowsSuppressionButton** – Pokud `true` zobrazí zaškrtávací políčko, který uživatel může použít pro potlačení výstrahy pro následné výskyty události, která aktivuje jej.
- **AccessoryView** -umožňuje připojit jiné dílčí zobrazení výstrahy poskytnout doplňující informace, jako je například přidávání **textové pole** pro zadávání dat. Pokud nastavíte nové **AccessoryView** nebo upravit existující, je třeba volat `Layout()` metoda viditelné rozložení výstrahy můžete upravit.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Zobrazení výstrahy

Existují dva různé způsoby, výstrahu lze zobrazit, plovoucí nebo jako list. Následující kód zobrazí výstrahu jako plovoucí:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
Pokud tento kód běží, se zobrazí následující:

[![](alert-images/alert02.png "Upozornění na jednoduchý")](alert-images/alert02.png#lightbox)

Následující kód zobrazí stejné výstrahu jako list:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

Pokud tento kód běží, následující se zobrazí:

[![](alert-images/alert03.png "Zobrazí jako list upozornění")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Práce s výstrahy tlačítka

Ve výchozím nastavení, zobrazí pouze upozornění **OK** tlačítko. Však můžete nejsou nesmí být, že můžete vytvořit další tlačítka připojením, aby **tlačítka** kolekce. Následující kód vytvoří plovoucí výstrahu s **OK**, **zrušit** a **možná** tlačítko:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Úplně první tlačítko Přidat bude _výchozí tlačítko_ , bude aktivováno, pokud uživatel stiskne klávesu Enter. Vrácená hodnota bude celé číslo představující, které uživatel stisknutí tlačítka. V našem případě budou vráceny následující hodnoty:

- **OK** - 1000.
- **Zrušit** – 1001.
- **Možná** – 1002.

Pokud jsme spustit kód, následující se zobrazí:

[![](alert-images/alert04.png "Výstraha s tři možnosti tlačítko")](alert-images/alert04.png#lightbox)

Tady je kód pro stejnou výstrahu jako list:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
Pokud tento kód běží, následující se zobrazí:

[![](alert-images/alert05.png "Upozornění na tři tlačítko zobrazí jako list")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> Měli byste nikdy přidat více než tři tlačítka na výstrahu.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Tlačítko potlačit zobrazení

Pokud výstraha `ShowSuppressButton` vlastnost je `true`, oznámení zobrazí zaškrtávací políčko, který uživatel může použít pro potlačení výstrahy pro následné výskyty události, která aktivuje jej. Následující kód zobrazí upozornění na plovoucí s tlačítkem na potlačit:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Pokud hodnota `alert.SuppressionButton.State` je `NSCellStateValue.On`, uživatel má zaškrtnuté políčko potlačit, jinak mají není.

Pokud je kód spuštěn, následující se zobrazí:

[![](alert-images/alert06.png "Výstraha s potlačit tlačítko")](alert-images/alert06.png#lightbox)

Tady je kód pro stejnou výstrahu jako list:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Pokud tento kód běží, následující se zobrazí:

[![](alert-images/alert07.png "Výstraha s potlačit tlačítko Zobrazit jako list")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Přidání vlastních dílčí zobrazení

Výstrahy mají `AccessoryView` vlastnost, která slouží k přizpůsobení další výstrahy a přidejte jako věcí **textové pole** na vstup uživatele. Následující kód vytvoří upozornění na plovoucí s vstupního pole přidaný text:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Klíče řádky představují `var input = new NSTextField (new CGRect (0, 0, 300, 20));` který vytvoří nový **textové pole** , přidáme výstrahy. `alert.AccessoryView = input;` které připojí **textové pole** výstrahy a volání `Layout()` metoda, která je vyžadována ke změně velikosti výstraha, aby se vešla do nové dílčí zobrazení.

Pokud jsme spustit kód, následující se zobrazí:

[![](alert-images/alert08.png "Pokud jsme spustit kód, následující se zobrazí")](alert-images/alert08.png#lightbox)

Tady je stejná výstraha jako list:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Pokud jsme spustit tento kód, následující se zobrazí:

[![](alert-images/alert09.png "Výstraha s vlastní zobrazení")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce s výstrahami v aplikaci Xamarin.Mac trvá. Jsme viděli různé typy a používá výstrah, jak vytvářet a upravovat výstrahy a jak pracovat s výstrahami v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacWindows (ukázka)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s Windows](~/mac/user-interface/window.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do systému Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
