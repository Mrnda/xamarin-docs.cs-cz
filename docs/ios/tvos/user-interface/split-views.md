---
title: "Práce s řadiče zobrazení rozdělení"
description: "Tento článek se zabývá navrhování a práce s řadiče zobrazení rozdělení uvnitř Xamarin.tvOS aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 86a7690d4cf7291a4e44507a6250e3469c8f7ed2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-split-view-controllers"></a>Práce s řadiče zobrazení rozdělení

_Tento článek se zabývá navrhování a práce s řadiče zobrazení rozdělení uvnitř Xamarin.tvOS aplikace._


Řadič zobrazení rozdělení uvede a spravuje hlavní a řadiče zobrazení podrobností-souběžného, na obrazovce ve stejnou dobu. Rozdělení zobrazení řadiče jsou použít k zobrazení obsahu trvalé, může získat fokus v zobrazení předlohy (menší části na levé straně) a související podrobnosti v zobrazení podrobností (větší oddílu na pravé straně).

[![](split-views-images/intro01.png "Ukázkové zobrazení rozdělení")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>O rozdělení řadiče zobrazení

Jak jsme uvedli výše, spravuje řadič zobrazení rozdělení hlavní a řadiče zobrazení podrobností, který-souběžného, zobrazí se hlavní probíhá menší zobrazení na levé straně, podrobností větší na pravé straně. 

Kromě toho může hlavní View Controller byla skrytý nebo zobrazený podle potřeby: 

[![](split-views-images/intro02.png "Skrytý řadiče zobrazení předlohy")](split-views-images/intro02.png#lightbox)

Řadiče zobrazení rozdělení se často používají k seznam filtrování obsahu představovat kategorií v hlavní zobrazení a filtrované výsledky v zobrazení podrobností. To je obvykle představují zobrazení tabulky na levé straně a [zobrazení kolekce](~/ios/tvos/user-interface/collection-views.md) na pravé straně.

Při navrhování uživatelské rozhraní, které vyžaduje řadič zobrazení rozdělení, Apple navrhuje pomocí hlavní a řadiče zobrazení podrobností, který se nemění (pouze obsahu změny, není struktura). Pokud potřebujete odkládacího souboru se na více systémů řadiče zobrazení, je nejvhodnější použít řadič navigační jako základní řadiče zobrazení, které je potřeba změnit (nebo podrobností).

Společnost Apple má následující návrhy pro práci s rozdělení řadiče zobrazení:

- **Použijte správný procento rozdělení** – ve výchozím nastavení řadiče zobrazení rozdělení používá jednu třetinu obrazovky pro zobrazení řadič hlavní a 2-/ 3 pro řadič zobrazení podrobností. Volitelně můžete 50/50 rozdělení. Vyberte správný procento aby vašeho obsahu se zobrazil vyrovnáváním na obrazovce.
- **Zachovat výběr hlavní** – při obsah na podrobné zobrazení můžete změnit je odpověď na výběr uživatele v zobrazení předlohy, nutné opravit hlavního zobrazení obsahu. Kromě toho by měl jasně zobrazit aktuálně vybrané položky v zobrazení předlohy.
- **Použít jedinou Unified název** -obvykle budete chtít použít název jedné, na střed v zobrazení podrobností místo název podrobností a hlavního zobrazení.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Rozdělení řadiče zobrazení a scénářů

Nejjednodušší způsob, jak pracovat s rozdělení řadiče zobrazení v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V **řešení Pad**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **řadiče zobrazení rozdělení** z **sada nástrojů** na zobrazení: 

    [![](split-views-images/activity01.png "Řadič zobrazení rozdělení")](split-views-images/activity01.png#lightbox)
1. Ve výchozím nastavení nainstaluje iOS Návrhář řadič navigační a View Controller v hlavního zobrazení. Pokud to nevejde požadavky vaší aplikace, jednoduše je odstraníte.
1. Pokud odeberete výchozí hlavního zobrazení, přetáhněte na návrhovou plochu nového řadiče zobrazení: 

    [![](split-views-images/activity02.png "Řadič zobrazení")](split-views-images/activity02.png#lightbox)
1. Ovládací prvek klikněte na tlačítko a přetáhněte ji z řadiče zobrazení rozdělení na nový řadič hlavního zobrazení. 
1. Vyberte **hlavní** z **místní nabídky**: 

    [![](split-views-images/activity03.png "Vyberte hlavní z místní nabídky")](split-views-images/activity03.png#lightbox)
1. Návrh obsah hlavní a zobrazení podrobností: 

    [![](split-views-images/activity04.png "Příklad rozložení")](split-views-images/activity04.png#lightbox)
1. Přiřadit **názvy** v **pomůcky karta** z **Pad vlastnosti** pro práci s ovládacími prvky uživatelského rozhraní v kódu jazyka C#.
1. Uložte změny a vrátit k sadě Visual Studio for Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **řadiče zobrazení rozdělení** z **sada nástrojů** na zobrazení: 

    [![](split-views-images/activity01-vs.png "Řadič zobrazení rozdělení")](split-views-images/activity01-vs.png#lightbox)
1. Ve výchozím nastavení přidá iOS Návrhář navigační řadiče a View Controller v zobrazení předlohy. Pokud to nevejde požadavky vaší aplikace, jednoduše je odstraníte.
1. Pokud odeberete výchozí hlavního zobrazení, přetáhněte na návrhovou plochu nového řadiče zobrazení: 

    [![](split-views-images/activity02-vs.png "Řadič zobrazení")](split-views-images/activity02-vs.png#lightbox)
1. Ovládací prvek klikněte na tlačítko a přetáhněte ji z řadiče zobrazení rozdělení na nový řadič hlavního zobrazení. 
1. Vyberte **hlavní** z **místní nabídky**: 

    [![](split-views-images/activity03-vs.png "Vyberte hlavní z místní nabídky")](split-views-images/activity03-vs.png#lightbox)
1. Návrh obsah hlavní a zobrazení podrobností: 

    [![](split-views-images/activity04.png "Rozložení obsahu")](split-views-images/activity04.png#lightbox)
1. Přiřadit **názvy** v **pomůcky karta** z **Explorer vlastnosti** pro práci s ovládacími prvky uživatelského rozhraní v kódu jazyka C#.
1. Uložte provedené změny.
    
-----

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Práce s řadiče zobrazení rozdělení

Jak jsme uvedli výše, řadič zobrazení rozdělení se často používá v situacích, kde jsou zobrazení filtrované obsah pro uživatele. Hlavních kategorií se zobrazí na levé straně v hlavní zobrazení a filtrované výsledky na pravé straně v zobrazení podrobností na základě uživatele výběru.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>Přístup k seznamu a podrobností

Pokud potřebujete přístup k hlavní a řadiče zobrazení podrobností prostřednictvím kódu programu, použijte `ViewControllers ` vlastnost řadiče zobrazení rozdělení. Příklad:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

Zobrazí se jako pole, kde je první prvek v řadiče zobrazení předlohy (0) a druhý prvkem (1) podrobností.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>Přístup k podrobností z hlavní

Vzhledem k tomu, že jsou obvykle zobrazení podrobné informace v zobrazení podrobností na základě výběru uživatele v hlavní, budete potřebovat přístup k podrobností z hlavního serveru.

Nejjednodušším způsobem je třeba vystavit vlastnost v třídě hlavní View Controller:

```csharp
public DetailViewController DetailController { get; set;}
```

V zobrazení řadičem rozdělení přepsat `ViewDidLoad` metoda a vazbě dvě zobrazení najednou. Příklad:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

Vlastnosti a metody můžete vystavit do řadiče zobrazení podrobností, je hlavní server můžete použít k dispozici nová data podle potřeby.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Zobrazení a skrytí hlavní

Volitelně lze zobrazit nebo skrýt hlavní View Controller pomocí `PreferredDisplayMode` vlastnost řadiče zobrazení rozdělení. Příklad:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode` Výčtu definuje, jak řadiče zobrazení hlavní zobrazí jako jedna z následujících akcí:

- **Automatické** -tvOS bude řídit prezentace hlavní a zobrazení podrobností.
- **PrimaryHidden** -skryje řadičem hlavního zobrazení.
- **AllVisible** -zobrazí hlavní i řadiče zobrazení podrobností-souběžného. To je normální, výchozí prezentace.
- **PrimaryOverlay** -The řadiče zobrazení podrobností rozšiřuje pod a je předmětem je hlavní server.

Chcete-li získat aktuální stav prezentace, použijte `DisplayMode` vlastnost řadiče zobrazení rozdělení.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s řadiče zobrazení rozdělení uvnitř Xamarin.tvOS aplikace.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
