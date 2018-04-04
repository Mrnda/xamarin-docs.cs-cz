---
title: Práce s zadávání textu
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9dec6f754590abf6db8829f555376b423b7a7da7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-input"></a>Práce s zadávání textu

Apple Watch neposkytuje klávesnice uživatelům zadejte text, ale podporuje některé sledovat friendly alternativy.

- Výběr z předdefinovaného seznamu možností textu
- Siri diktování
- Výběr emoji,
- Klikyháky rozpoznávání rukopisu písmeno písmeno (počínaje watchOS 3).

Simulátor aktuálně nepodporuje diktování ale můžete pořád otestovat zadávání textu řadiče jiné možnosti, například Scribble, jak je vidět tady:

![](text-input-images/textinput-sml.png "Testování možností scribble")

Tak, aby přijímal zadávání textu v aplikaci pro sledování:

1. Vytvořte řetězec předdefinovaných možností.
2. Volání `PresentTextInputController` s polem, jestli se má povolit emoji nebo Ne a `Action` který je volán při dokončení uživatele.
3. Do dokončení akce test pro vstupní výsledek a proveďte příslušné akce v aplikaci (pravděpodobně nastavení textové hodnoty popisku).

Následující fragment kódu uvede tři předdefinované možnosti pro uživatele:

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

`WKTextInputMode` Výčet má tří hodnot:

- Prostý
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>Prostý

Když je prostý režim nastavený, uživatel může vybrat:

- Diktování,
- Klikyháky, nebo
- z předdefinovaného seznamu, který poskytuje aplikace.

[![](text-input-images/plain-scribble-sml.png "Diktování, Scribble, nebo z předdefinovaného seznamu, který poskytuje aplikace")](text-input-images/plain-scribble.png#lightbox)

Výsledek je vždy vrátí jako `NSObject` , může být převeden `string`.

## <a name="emoji"></a>Emoji

Existují dva typy emoji:

- Regulární emoji kódování Unicode
- Animovaný bitové kopie

Když uživatel vybere emoji znakové sady Unicode, se vrátí jako řetězec.

Pokud je vybrána emoji animovaný bitové kopie `result` při dokončení bude obsahovat obslužná rutina `NSData` objekt, který obsahuje emoji `UIImage`.

## <a name="accepting-dictation-only"></a>Přijetí diktování pouze

Provést uživatele přímo k obrazovce diktování bez zobrazení jakékoli návrhy (nebo možnost Scribble):

- předat prázdné pole v seznamu návrhů a
- Nastavit `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Když je uživatel platí, že na obrazovce sledování se zobrazí následující obrazovka, která zahrnuje text rozumí (například "Toto je test") se:

![](text-input-images/dictation.png "Když je uživatel platí, že obrazovce sledovat jako rozumí se zobrazí text")

Jakmile budou stiskněte **provádí** tlačítko bude vrácen text.



## <a name="related-links"></a>Související odkazy

- [Společnosti Apple Text a popisky dokumentů](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Úvod do watchOSu 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
