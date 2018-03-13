---
title: "Vytváření objektů uživatelského rozhraní"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: e7a61dcf2cf2fabf575e30ef402121db3bea7912
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="creating-user-interface-objects"></a>Vytváření objektů uživatelského rozhraní

Apple skupin souvisejících částí funkce do "rozhraní", které rovnat Xamarin.iOS obory názvů. `UIKit` je obor názvů, který obsahuje všechny uživatelské rozhraní ovládací prvky pro iOS.

Vždy, když je odkazovat na prvek uživatelského rozhraní, jako je například popisek nebo tlačítko kódu nezapomeňte zadat následující příkaz using:

```csharp
using UIKit;
```


Všechny ovládací prvky popsané v této kapitole jsou v oboru názvů UIKit a název třídy ovládacího prvku každý uživatel má `UI` předponu.

Můžete upravit ovládacích prvků uživatelského rozhraní a rozložení třemi způsoby:

-  **[Xamarin iOS Návrhář](~/ios/user-interface/designer/index.md)**  – Návrhář předdefinované rozložení pomocí Xamarin pro návrh obrazovky. Dvakrát klikněte na storyboard nebo soubory XIB upravit pomocí předdefinovaných návrháře.
-  **Xcode rozhraní tvůrce** – přetáhněte ovládací prvky na vaší obrazovce rozložení pomocí rozhraní tvůrce. Otevřít soubor XIB nebo storyboard v Xcode kliknutím pravým tlačítkem myši na soubor v **řešení Pad** a výběr **otevřít v > Xcode rozhraní tvůrce**.
-  **Pomocí jazyka C#** – ovládací prvky lze také prostřednictvím kódu programu sestavený s kódem a přidat do zobrazení hierarchie.

Pravým tlačítkem myši na projekt pro iOS a zvolením lze přidávat nové scénáře a XIB soubory **Přidat > Nový soubor...** .

Bez ohledu na způsob, řídit vlastnosti a události smí uživatel manipulovat stále pomocí C# logiky aplikace.

## <a name="using-xamarin-ios-designer"></a>Pomocí Xamarin iOS návrháře

Pokud chcete začít vytvářet uživatelské rozhraní v iOS Designer, poklikejte na soubor scénáře. Ovládací prvky můžete přetáhnout na návrhovou plochu z **sada nástrojů** jak je uvedeno dále:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [![](creating-ui-objects-images/image2b.png "Sada nástrojů odsazení")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [![](creating-ui-objects-images/image2b-vs.png "Sada nástrojů Pad - sadě Visual Studio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

Pokud je vybrána ovládacího prvku na návrhovou plochu **vlastnosti Pad** zobrazí atributy pro ovládací prvek. **Pomůcky > Identity > název** pole, které je nezadají informace na tomto snímku obrazovky, se používá jako *výstupu* název. Toto je, jak můžete odkazovat na ovládací prvek v C#:

 [![](creating-ui-objects-images/image3b.png "Vlastnosti pomůcky odsazení")](creating-ui-objects-images/image3b.png#lightbox)

Podrobnější prohlídku do pomocí návrháře iOS, najdete v části [Úvod do systému iOS Návrhář](~/ios/user-interface/designer/introduction.md) průvodce.

## <a name="using-xcode-interface-builder"></a>Pomocí Tvůrce Xcode rozhraní

Pokud jste obeznámeni s pomocí rozhraní tvůrce, podívejte se na společnosti Apple [rozhraní tvůrce](https://developer.apple.com/xcode/interface-builder/) dokumenty.

V Xcode otevřete scénáře, klikněte pravým tlačítkem pro přístup k místní nabídky pro scénáře soubor a vyberte a otevřete s **Xcode rozhraní tvůrce**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [![](creating-ui-objects-images/imagexcode.png "Scénáře kontextovou nabídku - Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](creating-ui-objects-images/imagexcode-vs.png "Scénáře kontextovou nabídku - Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Ovládací prvky můžete přetáhnout na návrhovou plochu z **objektu knihovny** znázorněné dole:

 [![](creating-ui-objects-images/image5a.png "Xcode objektu knihovny")](creating-ui-objects-images/image5a.png#lightbox)

Při návrhu vaší uživatelské rozhraní musíte vytvořit tvůrce rozhraní **výstupu** pro každý ovládací prvek, který chcete odkazovat v jazyce C#. K tomu je potřeba zapnout **pomocníka Editor** pomocí centra **Editor** na tlačítka panelu nástrojů Xcode tlačítko:

 [![](creating-ui-objects-images/image6a.png "Tlačítko pomocníka editoru")](creating-ui-objects-images/image6a.png#lightbox)

Klikněte na objekt uživatelského rozhraní; potom **přetáhněte ovládací prvek** do souboru h. K ** řízení přetáhněte **, podržte stisknutou klávesu ovládací prvek pak klikněte na tlačítko a podržte přes rozhraní objektu uživatele které jsou pro vytváření výstupu (nebo akce). Zachovat podržíte stisknutou klávesu řízení a přetáhněte do záhlaví souboru. Dokončit přetahování níže `@interface` definice. Modré řádku by se zobrazit s titulek vložit výstupu nebo kolekce výstupu, jak ukazuje následující snímek obrazovky.

Při vydání a klikněte na vás vyzve k zadání názvu pro výstupu, který bude použit k vytvoření vlastnosti C#, která může být odkazováno v kódu:

 [![](creating-ui-objects-images/image8a.png "Vytváření výstupu")](creating-ui-objects-images/image8a.png#lightbox)

Další informace o tom, jak na Xcode rozhraní tvůrce integruje pomocí sady Visual Studio pro Mac, najdete v části [generování kódu Xib](~/ios/internals/xib-code-generation.md#generated) dokumentu.

##  <a name="using-c"></a>Pomocí jazyka C#

Pokud se rozhodnete prostřednictvím kódu programu vytvořte objekt uživatelského rozhraní pomocí jazyka C# (v zobrazení nebo zobrazení řadič, např.), postupujte takto:

-  Pole třídu úrovně pro objekt uživatele rozhraní deklarujte. Vytvoření ovládacího prvku samotné jednou, v `ViewDidLoad` například. Objekt potom lze používat v průběhu životního cyklu metody řadiče zobrazení (např.
`ViewWillAppear`).
-  Vytvoření `CGRect` rámečku ovládacího prvku (jeho souřadnice X a Y na obrazovce, jakož i jeho šířka a výška), který definuje. Budete muset zajistěte, aby byla `using CoreGraphics` direktivy pro tento.
-  Volejte konstruktor vytvořit a přiřadit ovládacího prvku.
-  Nastavte všechny vlastnosti nebo obslužné rutiny událostí.
-  Volání `Add()` přidání ovládacího prvku do zobrazení hierarchie.

Zde je jednoduchý příklad vytvoření `UILabel` v Kontroleru zobrazení pomocí jazyka C#:

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>Použití jazyka C# a scénářů

Po přidání řadiče zobrazení na návrhovou plochu, odpovídající C# jsou vytvořeny dva soubory v projektu. V tomto příkladu `ControlsViewController.cs` a `ControlsViewController.designer.cs` byly vytvořeny automaticky:

 [![](creating-ui-objects-images/image9b.png "Částečné třídy ViewController")](creating-ui-objects-images/image9b.png#lightbox)

`MainViewController.cs` Soubor je určený pro *kódu*. To je, kdy `View` životního cyklu metody, jako `ViewDidLoad` a `ViewWillAppear` jsou implementované a kde můžete přidat vlastní vlastnosti, pole a metody.

`ControlsViewController.designer.cs` Je generovaný kód obsahující konkrétní třídu. Pokud název ovládacího prvku na návrh surface v sadě Visual Studio pro Mac, nebo vytvořit výstupu nebo akce v Xcode, odpovídající vlastnost nebo metoda částečné, se přidá do souboru návrháře (designer.cs). Následující kód ukazuje příklad kód generovaný pro dvě tlačítka a textového zobrazení, kde jeden tlačítek má také `TouchUpInside` událostí.

Tyto prvky třídu povolit kódu odkazovat na ovládací prvky a reagovat na akce, které jsou deklarované v návrhové ploše:

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

`designer.cs` Souboru by neměla být upravována ručně – rozhraní IDE (Visual Studio pro Mac nebo Visual Studio) zodpovídá za bude synchronizován s scénáři.

Pokud jsou objekty uživatelského rozhraní přidaných do prostřednictvím kódu programu `View` nebo `ViewController`, vytváření instancí a odkazy na objekty spravovat sami, a proto žádné návrháře soubor je povinný.



## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
