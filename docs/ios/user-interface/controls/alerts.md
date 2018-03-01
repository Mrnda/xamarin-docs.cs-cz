---
title: "Zobrazení výstrah"
ms.topic: article
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 09f4178d5d6e12388771fa63875fbe1f489c959a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="displaying-alerts"></a>Zobrazení výstrah

Od verze iOS 8, UIAlertController má dokončené nahrazené UIActionSheet a UIAlertView z nich jsou nyní zastaralé.

Na rozdíl od třídy nahrazováno, které jsou podtřídy UIView, UIAlertController je podtřídou třídy UIViewController.

Použití `UIAlertControllerStyle` k označení typu výstrahy k zobrazení. Jsou tyto typy výstrah:

- **UIAlertControllerStyleActionSheet**
    * IOS před 8 to by byl UIActionSheet
- **UIAlertControllerStyleAlert**
    * IOS před 8 to by byl UIAlertView 

Existují tři kroky potřebné při vytváření řadič výstrah:

- Vytvoření a konfigurace upozornění s a:
    * Název
    * – zpráva
    * preferredStyle
    
- (Volitelné) Přidat textové pole
- Přidejte požadované akce
- Řadiče zobrazení k dispozici

Nejjednodušší výstrahu obsahuje jedno tlačítko, jak je vidět na tomto snímku obrazovky:

 ![Upozorní na jedno tlačítko](alerts-images/alert1.png)

Kód, který zobrazí upozornění na jednoduchý vypadá takto:

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

Zobrazení výstrahy v několika variantách, se provádí podobným způsobem, ale přidat dvě akce. Například následující snímek obrazovky ukazuje výstraha s dvě tlačítka:

 ![ Upozorní na dvě tlačítka](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

Výstrahy lze také zobrazit akce listu, podobně jako tento snímek obrazovky:

 ![Výstraha list akce](alerts-images/alert3.png)

Tlačítka se přidají do výstraha s `AddAction` metoda:

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
- [Výstrahy řadiče](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
