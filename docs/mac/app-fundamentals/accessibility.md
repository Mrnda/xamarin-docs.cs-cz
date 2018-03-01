---
title: "Usnadnění přístupu v systému macOS"
description: "Tato příručka popisuje funkce pro vytváření přístupné Xamarin.Mac aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0117364f02302add1f8788de1a79e4c4210fd07b
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="accessibility-on-macos"></a>Usnadnění přístupu v systému macOS

Tato stránka popisuje, jak pomocí rozhraní API pro usnadnění přístupu systému macOS můžete vytvářet aplikace podle [kontrolní seznam usnadnění](~/cross-platform/app-fundamentals/accessibility.md).
Odkazovat [Android usnadnění](~/android/app-fundamentals/accessibility.md) a [iOS usnadnění](~/ios/app-fundamentals/accessibility.md) stránky pro jinou platformu rozhraní API.

Abyste pochopili, jak funguje usnadnění rozhraní API v systému macOS (dříve se označovaly jako OS X), první kontrola [OS X usnadnění modelu](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Popisuje prvky uživatelského rozhraní

Používá AppKit `NSAccessibility` protokol, který se zveřejňují rozhraní API, která bude přístupná uživatelské rozhraní. To zahrnuje výchozí chování, které se pokouší nastavit smysluplný hodnoty pro usnadnění vlastnosti, jako je třeba nastavení tlačítko `AccessibilityLabel`. Popisek je obvykle jednoho slova nebo krátká fráze popisující ovládací prvek nebo zobrazení.

### <a name="storyboard-files"></a>Scénáře soubory

Xamarin.Mac používá rozhraní tvůrce Xcode k úpravám storyboard soubory.
Informace o usnadnění přístupu lze upravit v **Identity inspector** při je vybraný ovládací prvek na návrhovou plochu (jak je znázorněno na tomto snímku obrazovky):

[![Přidání usnadnění v Xcode na rozhraní tvůrce](accessibility-images/xcode.png "přidání usnadnění v Tvůrci rozhraní na Xcode")](accessibility-images/xcode-large.png)

### <a name="code"></a>Kód

Xamarin.Mac nevystavuje aktuálně jako `AccessibilityLabel` setter.  Přidejte následující metodu helper nastavit popisek usnadnění přístupu:

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

Tato metoda se pak použít v kódu, jak je znázorněno:

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp` Vlastnost je vysvětlení co ovládací prvek nebo zobrazení umožňuje a měli přidat pouze při popisek nemusí poskytovat dostatečné informace. Text nápovědy stále ukládat co nejkratší, pro příklad "odstraní dokumentu".

Některé prvky uživatelského rozhraní nejsou relevantní pro přístupné přístup (například štítku vedle vstup, který má vlastní popisek usnadnění a nápovědu).
V těchto případech nastavit `AccessibilityElement = false` tak, aby tyto ovládací prvky nebo zobrazení se přeskočí čtečky obrazovky nebo jiné nástroje pro usnadnění práce.

Poskytuje Apple [pokyny pro usnadnění přístupu](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) vysvětlující osvědčené postupy pro usnadnění popisky a text nápovědy.

## <a name="custom-controls"></a>Vlastní ovládací prvky

Odkazovat na společnosti Apple [pokyny pro přístupné vlastní ovládací prvky](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) podrobnosti o další kroky.

## <a name="testing-accessibility"></a>Testování usnadnění přístupu

poskytuje systému macOS **usnadnění Inspector** , pomáhá otestovali funkce usnadnění přístupu. Kontrolor je součástí Xcode.

Při prvním spuštění, **usnadnění Inspector** bude vyžadovat oprávnění k řízení počítači přes usnadnění přístupu:

![Usnadnění Inspector vyžaduje oprávnění ke spuštění](accessibility-images/accessibility-inspector-1.png "usnadnění Inspector vyžaduje oprávnění ke spuštění")

Odemknutí obrazovky nastavení (Pokud je nutný, v levém dolním) a značek **usnadnění Inspector**:

![Obrazovka nastavení Povolit usnadnění přístupu Inspector](accessibility-images/accessibility-inspector-2.png "obrazovka nastavení Povolit usnadnění přístupu Inspector")

Po povolení kontrolor zobrazí jako plovoucího okna, které lze přesunout po obrazovce. Následující snímek obrazovky ukazuje inspector systémem vedle Mac ukázkovou aplikaci. Pohybu kurzoru nad okno kontrolor zobrazí všechny dostupné vlastnosti každého ovládacího prvku:

[![Příklad usnadnění Inspector spuštění](accessibility-images/accessibility-example.png "příklad usnadnění Inspector spuštěná")](accessibility-images/accessibility-example-large.png)

Další informace najdete v tématu [testování usnadnění pro OS X průvodce](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>Související odkazy

- [Usnadnění přístupu a platformy](~/cross-platform/app-fundamentals/accessibility.md)
- [Usnadnění Mac](https://www.apple.com/accessibility/mac/)
