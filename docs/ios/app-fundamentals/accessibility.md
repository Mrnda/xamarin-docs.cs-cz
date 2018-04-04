---
title: Usnadnění přístupu v systému iOS
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: af28d0866337c769d1d6102317fc186c49ec259e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility-on-ios"></a>Usnadnění přístupu v systému iOS

Tato stránka popisuje, jak používat iOS rozhraní API pro usnadnění přístupu k sestavení aplikací podle [kontrolní seznam usnadnění](~/cross-platform/app-fundamentals/accessibility.md).
Odkazovat [Android usnadnění](~/android/app-fundamentals/accessibility.md) a [usnadnění OS X](~/mac/app-fundamentals/accessibility.md) stránky pro jinou platformu rozhraní API.

## <a name="describing-ui-elements"></a>Popisuje prvky uživatelského rozhraní

poskytuje iOS `AccessibilityLabel` a `AccessibilityHint` vlastnosti pro vývojáře, které chcete přidat popisný text, který mohou využívat VoiceOver obrazovky čtečky zpřístupnit ovládacích prvků. Ovládací prvky můžete taky označené jeden nebo více vlastnosti, které poskytují další kontext v režimech přístupné.

Některé ovládací prvky nemusí musí být přístupné (pro příklad, štítek na textový vstup nebo image, která je čistě dekorativní) – `IsAccessibilityElement` zakázat usnadnění v těchto případech je k dispozici.

**Návrhář uživatelského rozhraní**

**Vlastnosti Pad** obsahuje usnadnění oddíl, který umožňuje toto nastavení upravit, pokud je vybrána ovládacího prvku v iOS návrhář uživatelského rozhraní:

![](accessibility-images/ios-designer-sml.png "Nastavení usnadnění")

**C#**

Tyto vlastnosti můžete nastavit také přímo v kódu:

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>Co je AccessibilityIdentifier?

`AccessibilityIdentifier` Se používá k nastavení jedinečný klíč, který slouží k odkazování na elementy uživatelského rozhraní pomocí rozhraní API vlastnosti UIAutomation.

Hodnota `AccessibilityIdentifier` je nikdy přečteno nebo zobrazovat uživateli.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification` Metoda umožňuje událostí má být aktivována uživateli mimo přímé interakce (například když komunikují s určitý ovládací prvek).

### <a name="announcement"></a>Oznámení

Oznámení nelze odesílat z kódu pro uživatele informuje, že některé stav se změnil (například operaci na pozadí byla dokončena). To může být doplněny vizuální označení v uživatelském rozhraní:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged` Oznámení se používá při rozvržení obrazovky:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>Usnadnění přístupu a lokalizace

Usnadnění vlastnosti jako popisek a pomocný parametr je možné lokalizovat stejně jako další text v uživatelském rozhraní.

**MainStoryboard.strings**

Pokud je uživatelské rozhraní nastíněny v scénáře, můžete zadat překladů pro usnadnění vlastnosti stejným způsobem jako ostatní vlastnosti. V příkladu níže `UITextField` má **lokalizace ID** z `Pqa-aa-ury` a dvě vlastnosti usnadnění se nastavuje ve španělštině:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Tento soubor umístěn do **es.lproj** adresář pro španělské obsah.

**Localizable.Strings**

Alternativně lze přidat překlady do **Localizable.strings** soubor v lokalizovaném obsahu adresáři (např. **ES.lproj** pro španělštinu):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Tyto překlady je možné v jazyce C# prostřednictvím `LocalizedString` metoda:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Odkazovat [iOS Lokalizace průvodce](~/ios/app-fundamentals/localization/index.md) Další informace o lokalizaci obsahu.

<a name="testing" />

## <a name="testing-accessibility"></a>Testování usnadnění přístupu

VoiceOver je povolena v **nastavení** aplikace přechodem na **Obecné > usnadnění > VoiceOver**:

![](accessibility-images/settings-sml.png "Nastavení rychlost řeči")

**Usnadnění** obrazovky také poskytuje nastavení pro přiblížení, velikost textu, Barva & kontrast možnosti, řeči nastavení a další možnosti konfigurace.

Postupujte podle těchto [VoiceOver pokyny](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) k testování usnadnění na zařízeních s iOS.


## <a name="simulator-testing"></a>Simulátor testování

Při testování v simulátoru, **usnadnění Inspector** je k dispozici k ověření usnadnění vlastností a událostí jsou správně nakonfigurovány. Zapnout nástroj inspector v **nastavení** aplikace přechodem na **Obecné > usnadnění > usnadnění Inspector**:

![](accessibility-images/settings-inspector-sml.png "Povolit usnadnění přístupu Inspector")

Po povolení bude umístěn okně kontroly nad obrazovce iOS za všech okolností.
Tady je příklad výstupu, pokud je vybrána zobrazení řádku tabulky – Všimněte si **popisek** obsahuje větě, která poskytuje obsah řádek, a také že "dokončí" (ie. značek je viditelná):

![](accessibility-images/tableview-a11y-sml.png "Pomocí nástroje usnadnění Inspector")

Během kontrolor je viditelná, použijte ikonu "X" v levé horní části na dočasně zobrazení a skrytí překrytí a povolí nebo zakáže nastavení usnadnění.



## <a name="related-links"></a>Související odkazy

- [Usnadnění přístupu a platformy](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS usnadnění (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
