---
title: Hello, watchOS – návod
description: Tento dokument poskytuje návod k vytvoření jednoduché watchOS aplikace pomocí Xamarin. Popisuje, jak fungují v sadě Visual Studio a Visual Studio pro Mac, pracovat s scénářů a reakce na události v kódu.
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 00d6080429450dce2c0491fa385cf4f179befba6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790994"
---
# <a name="hello-watchos--walkthrough"></a>Hello, watchOS – návod

Po vytvoření řešení kroků v [instalační program a instalace](~/ios/watchos/get-started/installation.md), budete mít 3 projekty:

- Nadřazené aplikace iOS, která se používá k instalaci nebo další úlohy správy na zařízení. (S jinými typy iOS rozšíření, to se často označuje jako "Kontejner" aplikace.) Sledování aplikací, budou se uživatelé mohou spustit sledování aplikace bez **někdy** spuštění nadřazená aplikace;
- Rozšíření sledování, které obsahuje kód programu pro sledování aplikace; a
- Sledování aplikace, která obsahuje scénáře a bitové kopie prostředky, které jsou vykreslovány na hodinek.

Zkontrolujte, že vaše [odkazy jsou správné](~/ios/watchos/get-started/project-references.md): že nadřazená aplikace obsahuje odkaz na rozšíření a, že rozšíření má odkaz na aplikaci sledovat.

Potvrďte, podívejte se na vaší sady identifikátory \*.watchkitextension \*.watchkitapp konvence a že souboru Info.plist toto rozšíření je má **ID sady WKApp** hodnota nastavena na identifikátor sady Sledování aplikace.

Nyní byste měli mít teď spuštění aplikace sledovat, ale protože soubor scénáře v aplikaci sledování je prázdná, nebude moct zjistit.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/projectstructure.png "Průzkumníku řešení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "Průzkumníku řešení")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
Dvakrát klikněte na Interface.storyboard v aplikaci sledování spusťte Xamarin iOS návrháře (Pokud jste na Macu jste můžete také kliknout pravým tlačítkem a **otevřít v > Xcode rozhraní tvůrce**)


1.  Ujistěte se, **sada nástrojů** a **vlastnosti** jsou viditelné, dotyková zařízení
1.  Kliknutím vyberte řadič rozhraní
1.  Nastavit identifikátor a název tohoto rozhraní řadiče **interfaceController** a **HIS použití sledovat**,
1.  Ověřte **třída** je nastaven na **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "Nastavit identifikátor a název kontroleru rozhraní interfaceController a sledovat HIS použití")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dvakrát klikněte na Interface.storyboard v aplikaci sledování upravit s Xamarin iOS Návrhář v sadě Visual Studio:

1.  Otevřete podokno vlastnosti;
1.  Změňte třídu k **InterfaceController**;
1.  Klikněte na řadič, rozhraní; a
1.  Nastavit identifikátor a název tohoto rozhraní řadiče **interfaceController** a **HIS použití sledovat**.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "Nastavit identifikátor a název kontroleru rozhraní interfaceController a sledovat HIS použití")

-----


Vytvořte uživatelské rozhraní:

1. Z **sada nástrojů** pad
1. Přetáhnout myší **tlačítko** a **popisek** do scény, a
1. Nastavte text a atributy ovládacích prvků, jak je znázorněno:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/draganddrop.png "Nastavit text a atributy ovládacích prvků, jak je znázorněno")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "Nastavit text a atributy ovládacích prvků, jak je znázorněno")

-----

1. Nastavte **název** v **vlastnosti** pad pro každý ovládací prvek. V tomto příkladu jsme použili jsme `myButton` a `myLabel`.

1. Kliknutím na tlačítko ve scénáři, přejděte na **vlastnosti** pad na **události** seznamu, pak

1. Vytvořte novou **akce** zadáním `OnButtonPress` a stisknutím klávesy **Enter**.
  Akce se zobrazí v seznamu a částečné metoda vytvoří automaticky v jazyce C#.

![](hello-watch-images/buttonaction.png "Přidána OnButtonPress akce na tlačítko")

Po uložení scénáři **InterfaceController.designer.cs** získá aktualizován s názvy ovládacích prvků a akce... Pokud otevřete tento soubor po byl aktualizován, uvidíte jak `RegisterAttribute` odpovídá kontroleru a jak odpovídat ovládacích prvků uživatelského rozhraní jazyka C# instance proměnné označené jako `OutletAttribute` a jak akce se mapují na částečné metody označené `ActionAttribute`:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

Nyní otevřete **InterfaceController.cs** (*není* InterfaceController.designer.cs) a přidejte následující kód:

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

Tento kód by měl být poměrně transparentní: proměnnou instance `clickCount` se zvýší pokaždé, když funkce `OnButtonPress` je volána. Text `myLabel` se změnilo tak, aby odrážela tuto count; `myLabel`, samozřejmě, je název jednoho z výstupy jste vytvořili v XCode. `partial` Funkce je implementace funkce přidružené k názvu akce, které jste zadali.

Pokud ještě není projekt po spuštění

1. Klikněte pravým tlačítkem na projekt rozšíření sledování a zvolte **nastavit jako spouštěný projekt**,

1. Nastavit cíl nasazení do bitové kopie simulátoru sledovat kompatibilnímu Kit (například iPhone 6 iOS 8.2),

1. Stiskněte **ladění** tlačítko pro aktivaci sestavení a simulátoru spuštění.

    [![](hello-watch-images/readytodebug-sml.png "Prvky rozhraní sady Visual Studio")](hello-watch-images/readytodebug.png#lightbox)

Při spuštění simulátoru, klikněte na tlačítko se zvýší popisku.
Blahopřejeme, máte k dispozici sami sledování aplikace.

![](hello-watch-images/running.png "Aplikaci spuštěnou v simulátoru")


## <a name="related-links"></a>Související odkazy

- [Začínáme (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [Nastavení a instalace](~/ios/watchos/get-started/installation.md)
- [První aplikaci sledovat video](http://blog.xamarin.com/your-first-watch-kit-app/)
