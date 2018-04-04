---
title: Práce s ovládací prvek stránky
description: Tento článek se zabývá navrhování a práce s ovládací prvek stránku uvnitř Xamarin.tvOS aplikace.
ms.prod: xamarin
ms.assetid: 19198D46-7BBE-4D04-9BFA-7D1C5C9F9FC6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1cb07c050aeb2ee72e34048baab991df2d5775d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-page-control"></a>Práce s ovládací prvek stránky

_Tento článek se zabývá navrhování a práce s ovládací prvek stránku uvnitř Xamarin.tvOS aplikace._

Někdy je třeba zobrazit řadu stránky nebo bitové kopie v aplikaci Xamarin.tvOS. Ovládací prvek stránku byla navržená tak, aby jasně zobrazení stránky, které je uživatel na mimo maximální počet stránek. Ovládací prvek stránky se zobrazuje řada teček proti tmavý, oval ve tvaru pozadí. Na aktuální stránce se zobrazí vyplněný bod, všechny ostatní stránky zobrazit jako dutý tečky. Ovládací prvek stránky bude oříznutí vnější většina tečky, pokud jsou moc, aby se vešla do jeho pozadí oblasti.

[![](page-controls-images/page01.png "Ukázkové stránky ovládací prvek")](page-controls-images/page01.png#lightbox)

Ovládací prvek stránku v neinteraktivním elementu navržený tak, aby váš názor jenom uživateli. Musíte přidat další ovládací prvky, chcete-li změnit aktuální číslo stránky (například gesta nebo tlačítek).

Při použití ovládacího prvku stránky, Společnost Apple má následující návrhy:

- **Použití v pouze úplná kolekce** – ovládací prvky stránky nejlépe fungovat ve prostředí celou obrazovku zobrazíte více stránek, které existují v jedné kolekce.
- **Omezit počet stránek** – ovládací prvky stránky nejvhodnější pro stránky deset (10) nebo méně a nesmí být delší než dvacet (20) stránky. Pro více než dvacet stránky, zvažte použití [zobrazení kolekce](~/ios/tvos/user-interface/collection-views.md) a zobrazení stránek v mřížce.

<a name="Page-Controls-and-Storyboards" />

## <a name="page-controls-and-storyboards"></a>Ovládací prvky stránky a scénářů

Nejjednodušší způsob, jak pracovat s ovládacími prvky stránky v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. V **řešení Pad**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **ovládací prvek stránku** z **sada nástrojů** na zobrazení: 

    [![](page-controls-images/page02.png "Ovládací prvek stránky")](page-controls-images/page02.png#lightbox)
1. V **pomůcky karta** z **vlastnosti Pad**, můžete upravit několik vlastností ovládacího prvku stránky, jako jeho **aktuální stránku** a **počet stránek**: 

    [![](page-controls-images/page03.png "Na kartě pomůcky")](page-controls-images/page03.png#lightbox)
1. Ovládací prvky nebo gesta v dalším kroku přidejte do zobrazení přesunout zpátky a předávání prostřednictvím kolekce stránek.
1. Nakonec přiřadit **názvy** pro ovládací prvky, aby mohli odpovídat na ně v kódu jazyka C#. Příklad: 

    [![](page-controls-images/page04.png "Název ovládacího prvku")](page-controls-images/page04.png#lightbox)
1. Uložte provedené změny.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **ovládací prvek stránku** z **sada nástrojů** na zobrazení: 

    [![](page-controls-images/page02-vs.png "Ovládací prvek stránky")](page-controls-images/page02-vs.png#lightbox)
1. V **pomůcky karta** z **Explorer vlastnosti**, můžete upravit několik vlastností ovládacího prvku stránky, jako jeho **aktuální stránku** a **počet stránek**: 

    [![](page-controls-images/page03-vs.png "Na kartě pomůcky")](page-controls-images/page03-vs.png#lightbox)
1. Ovládací prvky nebo gesta v dalším kroku přidejte do zobrazení přesunout zpátky a předávání prostřednictvím kolekce stránek.
1. Nakonec přiřadit **názvy** pro ovládací prvky, aby mohli odpovídat na ně v kódu jazyka C#. Příklad: 

    [![](page-controls-images/page04-vs.png "Název ovládacího prvku")](page-controls-images/page04-vs.png#lightbox)
1. Uložte provedené změny.
    

-----

> [!IMPORTANT]
> Když je možné přiřadit události, jako `TouchUpInside` element uživatelského rozhraní v iOS návrháře (například UIButton), se nebude nikdy volat protože Apple TV nemá touch obrazovky nebo podporují touch události. Je třeba použít `Primary Action` události při vytváření obslužných rutin událostí pro tvOS prvky uživatelského rozhraní.




Upravit View Controller (například `ViewController.cs`) souboru a přidat kód pro zpracování na stránkách mění. Příklad:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public nint PageNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            PageView.Pages = 6;
            ShowCat ();
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void NextCat (UIBarButtonItem sender) {

            // Display next Cat
            if (++PageNumber > 5) {
                PageNumber = 5;
            }
            ShowCat();

        }

        partial void PreviousCat (UIBarButtonItem sender) {
            // Display previous cat
            if (--PageNumber < 0) {
                PageNumber = 0;
            }
            ShowCat();
        }
        #endregion

        #region Private Methods
        private void ShowCat() {

            // Adjust UI
            PreviousButton.Enabled = (PageNumber > 0);
            NextButton.Enabled = (PageNumber < 5);
            PageView.CurrentPage = PageNumber;

            // Display new cat
            CatView.Image = UIImage.FromFile(string.Format("Cat{0:00}.jpg",PageNumber+1));
        }
        #endregion
    }
}
```

Podívejme bližší pohled na dvě vlastnosti ovládací prvek stránky. Nejprve zadat maximální počet stránek, použijte následující příkaz:

```csharp
PageView.Pages = 6;
```

Chcete-li změnit aktuální číslo stránky, použijte následující kód:

```csharp
PageView.CurrentPage = PageNumber;
```

`CurrentPage` Vlastnost je nula (0) na základě tak, aby první stránka bude nula a poslední bude jeden minus maximální počet stránek.

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s ovládací prvek stránku uvnitř Xamarin.tvOS aplikace.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
