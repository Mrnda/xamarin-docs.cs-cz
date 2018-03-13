---
title: "Instalační program platformy Mac"
description: "Xamarin.Forms má nyní preview podporu pro platformu Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: e8487dc06b3512a0ec0bb1b30393faeab506df60
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="mac-platform-setup"></a>Instalační program platformy Mac

![Náhled](~/media/shared/preview.png)

Než začnete, vytvořit (nebo použijte existující) Xamarin.Forms projektu.
Můžete přidat pouze Mac aplikace pomocí sady Visual Studio for Mac.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Přidání systému macOS projektu do Xamarin.Forms, pomocí [univerzity Xamarin](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Přidání aplikace Mac

Postupujte podle těchto pokynů můžete přidat aplikaci Mac, který se spustí v systému macOS Sierra a Mac OS X El Capitan:

1. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na existující řešení Xamarin.Forms a zvolte **Přidat > Přidat nový projekt...**

2. V **nový projekt** okno zvolte **Mac > aplikace > kakao aplikace** a stiskněte klávesu **Další**.

3. Typ **název aplikace** (a volitelně vybrat jiný název pro položku ukotvení), stiskněte **Další**.

4. Zkontrolujte konfiguraci a stiskněte klávesu **vytvořit**. Níže jsou uvedeny v těchto kroků:

  ![Animovaný pokyny znázorňující postup přidání kakao aplikace](mac-images/add-macos-proj.gif)

5. V projektu Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...**  přidat [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Ostatní projekty by měl aktualizovat také na tuto verzi.

6. V projektu Mac, klikněte pravým tlačítkem na **odkazy** a přidejte odkaz na projekt Xamarin.Forms (sdílených projektů nebo PCL).

  ![Přidat odkaz na projektu sdíleného kódu Xamarin.Forms](mac-images/references-sml.png)

7. Aktualizace **Main.cs** k chybě při inicializaci `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. Aktualizace `AppDelegate` k chybě při inicializaci Xamarin.Forms, vytvoření okna a načíst aplikaci Xamarin.Forms (Nezapomeňte nastavit odpovídající `Title`). _Pokud máte další závislosti, které je třeba inicializovat, to udělat tady také._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. Klikněte dvakrát na **Main.storyboard** upravit v Xcode. Vyberte **okno** a _zrušte zaškrtnutí políčka_ **je počáteční řadiče** políčko (totiž výše uvedený kód vytvoří okno):

  [![Zrušte zaškrtnutí políčka je počáteční řadiče v Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  Můžete upravit systém nabídek ve scénáři, odebrat položky nežádoucí.

10. Nakonec přidejte místním prostředkům (např. soubory obrázků) z existující projekty platformy, které jsou požadovány.

11. Projekt Mac by měla spouštět Xamarin.Forms kódu v systému macOS!

## <a name="next-steps"></a>Další kroky

### <a name="styling"></a>Práce se styly

S poslední změny `OnPlatform` nyní můžete cílit na libovolný počet platformy. Systému macOS, který zahrnuje.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Poznámka: může se na platformách, jako je to také dvakrát: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Velikost a umístění okna

Můžete upravit původní velikost a umístění okna v `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Známé problémy

Toto je náhled, takže byste měli očekávat, že není vše produkční připraven. Níže je několik věcí, které se můžete setkat při přidávání systému macOS do vašich projektů:

### <a name="not-all-nugets-are-ready-for-macos"></a>Ne všechny NuGets jsou připravené ke systému macOS

Balíčky musí být "xamarinmac20" pro práci v systému macOS projektu. Můžete zjistit, že některé z knihoven, které můžete použít zatím nepodporují systému macOS.

V takovém případě budete muset odeslat požadavek na funkce maintainer projektu přidat. Dokud budou mít podporu, budete muset najít alternativy.

### <a name="missing-xamarinforms-features"></a>Chybějící funkce Xamarin.Forms

Ne všechny funkce Xamarin.Forms jsou dokončeny v této verzi preview; Tady je seznam některých funkcí, které není dosud implementována:

* Zápatí stránky
* Obrázek – aspekt
* ListView – ScrollTo, UnevenRows podpory, aktualizace, SeparatorColor, SeparatorVisibility
* MasterDetailPage – BackgroundColor
* Navigace – InsertPageBefore
* OpenGLRenderer
* Výběr – implementace Bindable/lze zobrazit
* TabbedPage – BarBackgroundColor, BarTextColor
* Zobrazení Tabulka – UnevenRows
* ViewCell – IsEnabled, ForceUpdateSize
* Webové zobrazení – většina WebNavigationEvents


## <a name="related-links"></a>Související odkazy

- [Xamarin.Mac](~/mac/index.yml)
