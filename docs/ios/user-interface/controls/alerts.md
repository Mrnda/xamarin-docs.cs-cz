---
title: Zobrazení oznámení v Xamarin.iosu
description: Tento dokument popisuje, jak zobrazit oznámení v Xamarin.iosu pomocí UIAlertController zavedené v Iosu 8 rozhraní API.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 788e62b30dbf533df059b0c3805e04ecf7b857aa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241331"
---
# <a name="displaying-alerts-in-xamarinios"></a>Zobrazení oznámení v Xamarin.iosu

Od verze iOS 8, má UIAlertController dokončené nahrazené UIActionSheet a UIAlertView z nich jsou už zastaralé.

Na rozdíl od tříd, které nahradilo, které jsou podtřídami UIView UIAlertController je podtřídou třídy UIViewController.

Použití `UIAlertControllerStyle` k označení typu výstrahy k zobrazení. Tyto typy výstrah jsou:

- **UIAlertControllerStyleActionSheet**
    * IOS před 8 to by byl UIActionSheet
- **UIAlertControllerStyleAlert**
    * IOS před 8 to bylo UIAlertView 

Existují tři kroky nezbytné pro při vytvoření Kontroleru oznámení:

- Vytvoření a konfigurace upozornění pomocí a:
    * Název
    * – zpráva
    * preferredStyle
    
- (Volitelné) Přidat textové pole
- Přidejte požadované akce
- K dispozici kontroler zobrazení

Nejjednodušší upozornění obsahuje jedno tlačítko, jak je znázorněno na tomto snímku obrazovky:

 ![Upozornění s jedno tlačítko](alerts-images/alert1.png)

Kód k zobrazení oznámení na jednoduché vypadá takto:

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

Zobrazení výstrahy s několika možnostmi, se provádí podobným způsobem, ale přidejte dvě akce. Například následující snímek obrazovky ukazuje výstrahy s dvě tlačítka:

 ![ Upozornění s dvě tlačítka](alerts-images/alert2.png)

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

Výstrahy můžete zobrazit také akce list, podobně jako na následujícím snímku obrazovky:

 ![Akce upozornění list](alerts-images/alert3.png)

Tlačítka jsou přidány do upozornění s `AddAction` metody:

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
- [Upozornění Kontroleru](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
