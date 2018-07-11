---
title: Nastavení platformy Mac
description: Tento článek vysvětluje, jak přidat projekt Mac do projektu Xamarin.Forms, která bude vytvářet aplikace fungovat v systému macOS Sierra a macOS El Capitan.
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: ae0fbfc7862a0d2147b2c3bbdbae7dd53dfce78f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831686"
---
# <a name="mac-platform-setup"></a>Nastavení platformy Mac

![Náhled](~/media/shared/preview.png)

Než začnete, vytvořit (nebo použijte existující) projektu Xamarin.Forms.
Můžete přidat jenom aplikace Mac pomocí sady Visual Studio pro Mac.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Přidání objektu project s macOS do Xamarin.Forms, podle [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Přidání aplikací pro Mac

Postupujte podle těchto pokynů pro přidání aplikací pro Mac, který se spustí v systému macOS Sierra a macOS El Capitan:

1. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na existující řešení Xamarin.Forms a zvolte **Přidat > Přidat nový projekt...**

2. V **nový projekt** okna zvolte **Mac > aplikace > aplikace Cocoa** a stiskněte klávesu **Další**.

3. Typ **název aplikace** (a volitelně vyberte jiný název pro položku Dock), stiskněte klávesu **Další**.

4. Zkontrolujte konfiguraci a stiskněte klávesu **vytvořit**. Tyto kroky je znázorněno níže:

  ![Animovaný pokyny ukazující, jak přidat aplikace Cocoa](mac-images/add-macos-proj.gif)

5. V projektu Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...**  přidáte [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Měli byste také aktualizovat ostatních projektů se na tuto verzi.

6. V projektu Mac, klikněte pravým tlačítkem na **odkazy** a přidejte odkaz na projekt Xamarin.Forms (sdílet projekt nebo .NET Standard knihovny projektu).

  ![Přidejte odkaz na projekt sdíleného kódu Xamarin.Forms](mac-images/references-sml.png)

7. Aktualizace **Main.cs** inicializovat `AppDelegate`:

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

8. Aktualizace `AppDelegate` inicializovat Xamarin.Forms, vytvořit časové období a zatížení aplikace Xamarin.Forms (zapamatování nastavení odpovídající `Title`). _Pokud máte další závislosti, které je potřeba inicializovat to udělat tady také._

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

9. Dvakrát klikněte na panel **Main.storyboard** upravit v Xcode. Vyberte **okno** a _zrušte zaškrtnutí políčka_ **je počáteční Kontroleru** zaškrtávacího políčka (Toto je vzhledem k tomu, že výše uvedený kód vytvoří okno):

  [![Zrušte zaškrtnutí políčka je počáteční řadič v Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  Můžete upravit systém nabídek ve scénáři, chcete-li odebrat nepotřebné položky.

10. Nakonec přidejte všechny místní prostředky (např.) soubory obrázků) z existujících projektů platformy, které jsou požadovány.

11. Projekt Mac by se měl spustit kódu Xamarin.Forms v systému macOS!

## <a name="next-steps"></a>Další kroky

### <a name="styling"></a>Práce se styly

S nejnovější změny provedené `OnPlatform` nyní můžete cílit na libovolný počet platformy. To zahrnuje macOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Mějte na paměti, může se na platformách, jako je to také dvakrát: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Velikost a polohu okna

Můžete upravit počáteční velikost a umístění okna `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Známé problémy

Toto je náhled, proto byste měli očekávat, že ne vše, co je připraveno na produkční. Níže je několik věcí, na které můžete narazit při přidávání do projektů macOS:

### <a name="not-all-nugets-are-ready-for-macos"></a>Ne všechny balíčky Nuget jsou připraveny pro macOS

Balíčky musí jako cíl "xamarinmac20" pro práci v projektu s macOS. Může se stát, že některé z knihoven, které používáte zatím ještě nepodporují macOS.

V takovém případě budete muset odeslat žádost o funkci Maintainer projektu a přidejte ji. Dokud mají podporu, budete muset hledejte alternativy.

### <a name="missing-xamarinforms-features"></a>Chybějící funkce Xamarin.Forms

Ne všechny funkce Xamarin.Forms jsou dokončeny v této verzi preview; Tady je seznam některých funkcí, které ještě není naimplementovaný:

* Zápatí
* Obrázek – aspekt
* ListView – ScrollTo, UnevenRows podpory, aktualizace, SeparatorColor, SeparatorVisibility
* MasterDetailPage – BackgroundColor
* Navigace – InsertPageBefore
* OpenGLRenderer
* Výběr – implementace Bindable/pozorovat
* BarTextColor TabbedPage – BarBackgroundColor,
* Zobrazení Tabulka – UnevenRows
* ForceUpdateSize ViewCell – hodnotu IsEnabled,
* WebView – většina WebNavigationEvents


## <a name="related-links"></a>Související odkazy

- [Xamarin.Mac](~/mac/index.yml)
