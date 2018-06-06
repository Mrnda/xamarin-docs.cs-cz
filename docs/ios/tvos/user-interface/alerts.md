---
title: Práce s výstrahami tvOS v Xamarinu
description: Tento dokument popisuje, jak pracovat s výstrahami tvOS v Xamarin. Popisuje zobrazení výstrahy, přidání textových polí a pomocná třída.
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b5125f150a4d57ed27041da2944f4c161434cf93
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789080"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Práce s výstrahami tvOS v Xamarinu

_Tento článek se zabývá práci s UIAlertController chcete uživateli v Xamarin.tvOS zobrazit zpráva s upozorněním._

Pokud potřebujete získat pozornost tvOS uživatele nebo se zeptejte oprávnění k provedení destruktivní akce (například odstranění souboru), může znamenat k upozornění pomocí `UIAlertViewController`:

[![](alerts-images/alert01.png "Příklad UIAlertViewController")](alerts-images/alert01.png#lightbox)

Pokud navíc k zobrazení zprávy můžete přidat tlačítka a textová pole na výstrahu umožňující uživateli reagovat na akce a poskytnout zpětnou vazbu.

<a name="About-Alerts" />

## <a name="about-alerts"></a>O výstrahách

Jak jsme uvedli výše, výstrah se používají k získat pozornost uživatele a informujte je stav vaší aplikace nebo žádosti o zpětnou vazbu. Výstrahy musí představovat název, můžete volitelně může mít zprávu a jeden nebo více tlačítek nebo textové pole.

[![](alerts-images/alert04.png "Příklad výstrahy")](alerts-images/alert04.png#lightbox)

Společnost Apple má následující návrhy pro práci s výstrahami:

- **Používejte opatrně výstrahy** -výstrahy přerušit tok uživatele s aplikací a přerušení činnost koncového uživatele a jako takový musí být použit pouze pro důležité situace, jako je například oznámení o chybě, nákupy v aplikaci a destruktivní akce. 
- **Poskytuje užitečné volby** – Pokud je výstraha nabízí možnosti pro uživatele, vaše by měl zajistěte, aby každá možnost důležitých informací nabízí a poskytuje užitečné akce uživateli.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Výstrahy názvy a zpráv

Společnost Apple má následující návrhy pro prezentování název a volitelný zprávu na výstrahu:

- **Použití názvů Multiword** -upozornění na název by měl získat bodem situace napříč jasně při zbývající jednoduché. Název jednoho slova zřídka poskytuje dostatek informací.
- **Používat popisné názvy, které nevyžadují zprávu** – Pokud je to možné, zvažte provedení popisný název výstrah, dostatek, který volitelné text zprávy není potřeba. 
- **Zkontrolujte zprávu krátké kompletní věta** – Pokud volitelné zprávy je potřeba získat bodem výstrahy napříč, zachovat co nejjednodušší a nastavit jej jako kompletní věta s správné malá a velká písmena a interpunkce.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Výstrahy tlačítka

Společnost Apple má následující návrhu pro přidání tlačítek na výstrahu:

- **Limit pro dvě tlačítka** – Pokud je to možné, omezte výstrahu, kterou chcete maximálně dvě tlačítka. Jeden tlačítko výstrahy poskytují informace, ale žádné akce. Dva tlačítko výstrahy poskytují jednoduché Ano/ne volba akce.
- **Použít Succinct logické názvy tlačítko** -jednoduché jednu až dvě aplikace word tlačítko produkty, které jasně popisují akce na tlačítko nejlépe fungovat. Další informace najdete v tématu naše [práce s tlačítka](~/ios/tvos/user-interface/buttons.md) dokumentaci.
- **Jasně označit destruktivní tlačítka** – pro tlačítka, které provádějí destruktivní akce (například odstranění souboru) jasně označit je pomocí `UIAlertActionStyle.Destructive` stylu.

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Zobrazení výstrahy

Pokud chcete výstrahu zobrazit, vytvořit instanci `UIAlertViewController` a nakonfigurujte ji přidáním akce (tlačítka) a výběr styl výstrahy. Například následující kód zobrazí výstrahu OK/zrušit:

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...
        
var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

Podívejme se na tento kód podrobně. Nejdříve vytvoříme novou výstrahu s daného názvu a zprávou:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

V dalším kroku pro každé tlačítko, které chcete zobrazit ve výstraze vytvoříme definování název tlačítko, jeho stylu a akci, kterou chceme využít při stisknutí tlačítka akce:

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle` Výčtu umožňují nastavit styl tlačítko jako jednu z následujících:

- **Výchozí** – tlačítko bude výchozí tlačítko vybrána, když se zobrazí upozornění.
- **Zrušit** – tlačítko je tlačítko Zrušit pro výstrahu.
- **Destruktivní** -klade důraz na tlačítko jako destruktivní akce, jako je například odstranění souboru. V současné době tvOS vykreslí destruktivní tlačítko s červeným pozadím.

`AddAction` Metoda přidá dané akce, která `UIAlertViewController` a nakonec `PresentViewController (alertController, true, null)` metoda zobrazí danou výstrahu uživateli.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Přidání textová pole

Kromě přidání akce (tlačítka) výstrahy, můžete přidat textové pole na výstrahu, kterou chcete povolit uživatelům vyplňte informace, jako je ID a heslo uživatele:

[![](alerts-images/alert02.png "Textové pole ve výstraze")](alerts-images/alert02.png#lightbox)

Pokud uživatel vybere textové pole, klávesnice standardní tvOS zobrazí což jim umožní zadejte hodnotu pro pole:

[![](alerts-images/alert03.png "Zadávání textu")](alerts-images/alert03.png#lightbox)

Následující kód zobrazuje výstrahy OK nebo zrušit pomocí jedné textové pole pro zadání hodnotu:

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

`AddTextField` Metoda přidá nové textové pole na výstrahu, která pak můžete nakonfigurovat nastavením vlastnosti, například zástupný text (text, který se zobrazí, když je pole prázdné), výchozí hodnota text a typ klávesnice. Příklad:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Tak, aby jsme může fungovat na základě hodnoty později textové pole, jsme se také uložit kopii pomocí následujícího kódu:

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

Co uživatel zadá hodnotu v textovém poli, můžeme použít `field` proměnnou pro přístup k této hodnotě.

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>Pomocná třída řadiče zobrazení výstrah

Protože zobrazení jednoduché, běžné typy výstrah pomocí `UIAlertViewController` může mít za následek s malou část kódu, duplicitní, pomocnou třídu můžete použít ke snížení objemu opakovaných kódu. Příklad:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

Pomocí této třídy, zobrazení a zpracování výstrah jednoduché lze provést následujícím způsobem:

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```


<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých práci s `UIAlertController` chcete uživateli v Xamarin.tvOS zobrazit zpráva s upozorněním. Nejprve se vám ukázal, jak zobrazit upozornění na jednoduchý a přidání tlačítek. V dalším kroku se vám ukázal, jak přidat textové pole na výstrahu. Nakonec se vám ukázal, jak pomocí pomocná třída snížíte počet opakovaných kódu, který zobrazí výstrahu.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
