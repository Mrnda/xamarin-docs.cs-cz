---
title: Práce s navigační řadiče
description: Tento článek se zabývá navrhování a práce s navigační panely uvnitř Xamarin.tvOS aplikace.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8a9a1c852137a2bcc0d46615e69eef0a245a9768
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-navigation-controllers"></a>Práce s navigační řadiče

_Tento článek se zabývá navrhování a práce s navigační panely uvnitř Xamarin.tvOS aplikace._

Navigační panely mohou být přidány do horní části pohledů pro zobrazení název a volitelný navigačních tlačítek panelu. Obvykle se používají při přešel uživatel z hlavní stránky, jako jsou tabulky zobrazení, kolekce nebo nabídku dílčí zobrazení zobrazující podrobnosti vybrané položky.

[![](navigation-bars-images/navbar01.png "Ukázka navigační panel")](navigation-bars-images/navbar01.png#lightbox)

Kromě toho k titulu (který se zobrazí v centru), navigační panely může obsahovat jeden nebo více tlačítek navigačního panelu (`UIBarButtonItem`) na levé a pravé straně panelu.

> [!IMPORTANT]
> Navigační panely jsou ve výchozím nastavení zcela transparentní. Potřeba dát pozor zajistit, že obsah navigačního panelu zůstane čitelný přes obsah pod ním. Například když obsah v zobrazení tabulky nebo kolekce se posouvá společně v něm.




<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Navigační panely a scénářů

Nejjednodušší způsob, jak pracovat s navigační panely v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. V **řešení Pad**, dvakrát klikněte na `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **navigační panel** z **sada nástrojů** na zobrazení v horní části obrazovky: 

    [![](navigation-bars-images/navbar02.png "Navigační panel")](navigation-bars-images/navbar02.png#lightbox)
1. Dvakrát klikněte na **navigační panel** vybrat volbu **navigační položka**. V **pomůcky** kartě **vlastnosti Pad**, můžete nastavit **název**: 

    [![](navigation-bars-images/navbar03.png "Název nastavení")](navigation-bars-images/navbar03.png#lightbox)
1. Dále můžete přidat jeden nebo víc **položky tlačítko panelu** na libovolném konci panelu: 

    [![](navigation-bars-images/navbar04.png "A tlačítko položku panelu")](navigation-bars-images/navbar04.png#lightbox)
1. Nakonec navázání **položky tlačítko panelu** na akce v **události** kartě **Explorer vlastnosti**: 

    [![](navigation-bars-images/navbar05.png "A panelu Akce položka tlačítka")](navigation-bars-images/navbar05.png#lightbox)
1. Uložte provedené změny.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. V **Průzkumníku řešení**, dvakrát klikněte na `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **navigační panel** z **sada nástrojů** na zobrazení v horní části obrazovky: 

    [![](navigation-bars-images/navbar02-vs.png "Navigační panel")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Dvakrát klikněte na **navigační panel** vybrat volbu **navigační položka**. V **pomůcky** kartě **Explorer vlastnosti**, můžete nastavit **název**: 

    [![](navigation-bars-images/navbar03-vs.png "Název nastavení")](navigation-bars-images/navbar03-vs.png#lightbox)
1. Dále můžete přidat jeden nebo víc **položky tlačítko panelu** na libovolném konci panelu: 

    [![](navigation-bars-images/navbar04-vs.png "A panelu tlačítko položky")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Nakonec navázání **položky tlačítko panelu** na akce v **události** kartě **Explorer vlastnosti**: 

    [![](navigation-bars-images/navbar05-vs.png "A panelu Akce položky tlačítek")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Uložte provedené změny.


-----

> [!IMPORTANT]
> Když je možné přiřadit události, jako `TouchUpInside` element uživatelského rozhraní v iOS návrháře (například UIButton), se nebude nikdy volat protože Apple TV nemá touch obrazovky nebo podporují touch události. Je třeba použít `Primary Action` události při vytváření obslužných rutin událostí pro tvOS prvky uživatelského rozhraní.




Následující kód obsahuje příklad obslužné rutiny události o tři různé BarButtonItems: `ShowFirstHotel`, `ShowSecondHotel`, a `ShowThirdHotel`. Když každá položka po kliknutí na obrázku pozadí `HotelImage` se změnilo. To je upravit v Kontroleru zobrazení (například `ViewController.cs`) souboru:

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

Tak dlouho, dokud na tlačítko `Enabled` vlastnost je `true` a není předmětem jiného ovládacího prvku nebo zobrazení, můžete provedeny položce vybraný pomocí vzdáleného Siri.

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s navigační panely uvnitř Xamarin.tvOS aplikace.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
