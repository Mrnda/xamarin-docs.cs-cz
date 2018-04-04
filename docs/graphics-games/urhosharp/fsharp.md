---
title: 'UrhoSharp programování s F #'
description: 'Jak vytvořit jednoduchou aplikaci UrhoSharp pomocí F # v sadě Visual Studio pro Mac'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 1fff90056f39660695c1e6d9ed307fad575b5127
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="programming-urhosharp-with-f"></a>UrhoSharp programování s F #

_Jak vytvořit jednoduchou aplikaci UrhoSharp pomocí F # v sadě Visual Studio pro Mac_

UrhoSharp lze naprogramovat se F # pomocí stejné knihovny a koncepty používané programátory v jazyce C#. [Pomocí UrhoSharp](~/graphics-games/urhosharp/using.md) článek nabízí přehled modul UrhoSharp a byste si měli přečíst před v tomto článku.

Například mnoho knihoven, které pochází na světě C++ vrátí mnoho funkcí UrhoSharp logických výrazů nebo celá čísla, která úspěšná nebo neúspěšná. Měli byste použít `|> ignore` ignorovat tyto hodnoty.

[Ukázka programu](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) je pro UrhoSharp z F # "Hello, World".

## <a name="creating-an-empty-project"></a>Vytváření prázdného projektu

Šablony F # pro UrhoSharp nejsou zatím k dispozici, takže k vytvoření projektu UrhoSharp můžete buď začněte s [ukázkové](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) nebo postupujte podle těchto kroků:

1. Ze sady Visual Studio pro Mac, vytvořte novou **řešení**. Zvolte **iOS > aplikace > jediné zobrazení aplikace** a vyberte **F #** jako jazyk implementace. 
1. Odstranit **Main.storyboard** souboru. Otevřete **Info.plist** souboru a v **iPhone nebo iPod informace o nasazení** podokně odstranit `Main` řetězce v **hlavní rozhraní** rozevíracího seznamu.
1. Odstranit **ViewController.fs** také soubor.

## <a name="building-hello-world-in-urho"></a>Vytváření Hello World v Urho

Nyní jste připraveni začít definice tříd vaše hra. Minimálně je třeba definovat podtřídou třídy `Urho.Application` a přepsat její `Start` metoda. K vytvoření tohoto souboru, klikněte pravým tlačítkem na projekt F #, zvolte **přidat nový soubor...**  a do projektu přidejte prázdné třídy F #. Nový soubor bude přidán na konec seznamu souborů ve vašem projektu, ale je třeba přetáhnout tak, aby se *před* se používá v **AppDelegate.fs**.

1. Přidáte odkaz na balíček Urho NuGet.
1. Z existujícího projektu Urho kopírování (velká) adresářů **CoreData /** a **dat nebo** do vašeho projektu **prostředky nebo** adresáře. V F # projektu, klikněte pravým tlačítkem na **prostředky** složku a použití **přidat nebo přidat existující složku** přidat všechny tyto soubory do projektu.

Strukturu projekt by měl nyní vypadat podobně jako:

![](fsharp-images/solutionpane.png "Struktura projektu by teď měl vypadat jako")

Definovat vlastní nově vytvořené třídy jako podtypem `Urho.Application` a přepsat její `Start` metoda:

```csharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Kód je velmi jednoduché. Použije `Urho.Gui.Text` třída zobrazíte střed řetězec s určitou velikostí písma a barvy. 

Před spuštěním tohoto kódu, ale UrhoSharp musí být inicializován. 

Otevřete soubor AppDelegate.fs a upravit `FinishedLaunching` metoda následujícím způsobem:

```csharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default` Poskytuje výchozí možnosti pro aplikaci na šířku režimu. Předejte tyto `ApplicationOptions` výchozího konstruktoru pro vaše `Application` podtřídami (Upozorňujeme, že pokud jste definovali `HelloWorld` třídy, řádek `inherit Application(o)` volá konstruktor základní třídy). 

`Run` Metodu vaší `Application` program spustí. Je definována jako návrat `int`, které lze předat do `ignore`. 

Výsledný program by měl vypadat podobně jako:

![](fsharp-images/helloworldfsharp.png "Výsledný program by měl vypadat jako")








## <a name="related-links"></a>Související odkazy

- [Přejděte na Githubu (ukázka)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
