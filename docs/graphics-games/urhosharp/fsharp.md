---
title: 'Programování Urhosharpu pomocí jazyka F #'
description: 'Tento dokument popisuje, jak vytvořit jednoduché aplikace hello world pro Urhosharpu pomocí F # v sadě Visual Studio pro Mac.'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: a4e1a31a2591c799a153e1333e4a4a4a0719a107
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111196"
---
# <a name="programming-urhosharp-with-f"></a>Programování Urhosharpu pomocí jazyka F #

Urhosharpu možné jej naprogramovat pomocí pomocí stejné koncepty používané programátory v C# a knihovny jazyka F #. [Používání Urhosharpu](~/graphics-games/urhosharp/using.md) článku s přehledem modul Urhosharpu a byste si měli přečíst před v tomto článku.

Stejně jako mnoho knihoven, které vytvoří se v celém světě C++ řada funkcí Urhosharpu vrací logické hodnoty nebo celých čísel, která udává úspěch nebo neúspěch. Měli byste použít `|> ignore` ignorovat tyto hodnoty.

[Ukázkový program](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) Urhosharpu z jazyka F # je "Hello World".

## <a name="creating-an-empty-project"></a>Vytvořit prázdný projekt

Šablony F # pro Urhosharpu nejsou dosud k dispozici, takže k vytvoření projektu Urhosharpu můžete buď začíná [ukázka](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) nebo postupujte podle těchto kroků:

1. Ze sady Visual Studio pro Mac, vytvořte nový **řešení**. Zvolte **iOS > aplikace > aplikace s jedním zobrazením** a vyberte **F #** jako implementaci jazyka. 
1. Odstranit **Main.storyboard** souboru. Otevřít **Info.plist** soubor a v **iPhone / iPod informace o nasazení** podokně odstranit `Main` řetězce v **hlavní rozhraní** rozevíracího seznamu.
1. Odstranit **ViewController.fs** také soubor.

## <a name="building-hello-world-in-urho"></a>Vytváření aplikace Hello World v Urho

Nyní jste připraveni začít definováním třídy svoji hru. Přinejmenším budete muset definovat podtřída `Urho.Application` a přepsat její `Start` metody. Chcete-li vytvořit tento soubor, klikněte pravým tlačítkem na projekt F #, zvolte **přidat nový soubor...**  a přidat prázdnou třídu F # do projektu. Nový soubor bude přidán na konec seznamu souborů ve vašem projektu, ale je třeba přetáhnout, aby se zobrazovala *před* se používá v **AppDelegate.fs**.

1. Přidejte odkaz na balíček Urho NuGet.
1. Z existujícího projektu Urho kopírování (velké) adresářů **CoreData /** a **Data /** do svého projektu **prostředky /** adresáře. V projektu F #, klikněte pravým tlačítkem na **prostředky** složky a použití **přidat / přidat existující složku** přidat všechny tyto soubory do projektu.

Strukturu projektu by měl nyní vypadat:

![](fsharp-images/solutionpane.png "Struktura projektu by měla vypadat jako")

Definovat jako podtyp vaší nově vytvořené třídy `Urho.Application` a přepsat její `Start` metody:

```fsharp
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

Kód je velmi jednoduché. Používá `Urho.Gui.Text` třídy pro zobrazení zarovnané na střed řetězce s určitou velikostí písma a barvy. 

Než můžete spustit tento kód, ale Urhosharpu musí být inicializován. 

Otevřete soubor AppDelegate.fs a upravit `FinishedLaunching` metodu následujícím způsobem:

```fsharp
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

`ApplicationOptions.Default` Poskytuje výchozí možnosti pro aplikaci režimu na šířku. Předávejte `ApplicationOptions` výchozího konstruktoru pro vaše `Application` podtřídy (Všimněte si, že když jste definovali `HelloWorld` třídy, řádek `inherit Application(o)` volá konstruktor základní třídy). 

`Run` Metodu vaše `Application` program spustí. Je definován jako vracející `int`, což může být směrované do `ignore`. 

Výsledný program by měl vypadat:

![](fsharp-images/helloworldfsharp.png "Výsledný program by měl vypadat.")








## <a name="related-links"></a>Související odkazy

- [Procházet na Githubu (ukázka)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
