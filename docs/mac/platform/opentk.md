---
title: "Úvod do OpenTK"
description: "Tento článek obsahuje úvod do práce s OpenTK v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu okno hra, vykreslování jednoduchého objektu a zobrazení objektu pro uživatele."
ms.topic: article
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f5383465f7bc5c4529eebefca02718c83a653e9f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-opentk"></a>Úvod do OpenTK

OpenTK (sada nástrojů otevřete) je k rozšířené, nízké úrovně C# knihovny, která usnadňuje práci s OpenGL a OpenCL OpenAL. OpenTK lze použít pro hry, vědecké aplikace nebo jiné projekty, které vyžadují 3D grafiky, zvuk nebo výpočetní funkce. Tento článek poskytuje stručný úvod do používání OpenTK v Xamarin.Mac aplikace.

[![](opentk-images/intro01.png "Příklad aplikace spustit")](opentk-images/intro01.png#lightbox)

V tomto článku vám nabídneme základy OpenTK v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>O OpenTK

Jak jsme uvedli výše, je OpenTK (sada nástrojů otevřete) k rozšířené, nízké úrovně C# knihovny, která usnadňuje práci s OpenGL a OpenCL OpenAL. Použití OpenTK v aplikaci Xamarin.Mac poskytuje následující funkce:

- **Rychlý vývoj** -OpenTK poskytuje silné datové typy a vložené dokumentace k zlepšení kódování pracovního postupu a zpracovávat chyby snadněji a rychleji.
- **Snadná integrace** -OpenTK byla navržená tak, aby snadnou integraci s aplikací .NET.
- **Licencí MIT** -OpenTK se distribuuje v rámci licencí MIT/X11 a je zcela zdarma.
- **Bohaté, bezpečnost typů vazby** -OpenTK podporuje nejnovější verze OpenGL OpenGL | ES, OpenAL a OpenCL s automatické rozšíření načítání chyba kontrolu a vložené dokumentaci.
- **Flexibilní možnosti s grafickým rozhraním** -OpenTK poskytuje nativního, vysoce výkonné herní okno určený speciálně pro hry a Xamarin.Mac.
- **Plně spravovaná, kompatibilní se specifikací CLS kód** -OpenTK podporuje 32bitové a 64bitové verze systému macOS s žádné nespravované knihovny.
- **3D matematické Toolkit** OpenTK poskytuje `Vector`, `Matrix`, `Quaternion` a `Bezier` struktury přes jeho 3D matematické Toolkit.

OpenTK lze použít pro hry, vědecké aplikace nebo jiné projekty, které vyžadují 3D grafiky, zvuk nebo výpočetní funkce.

Další informace najdete v tématu [sada nástrojů otevřete](http://www.opentk.com) webu.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK rychlý start

Jako rychlý úvod do používání OpenTK v aplikaci Xamarin.Mac jsme se chystáte vytvořit jednoduchou aplikaci, která otevře zobrazení hra, vykreslí jednoduchý trojúhelník v zobrazení a attachs zobrazení HRA do okna hlavní aplikace Mac zobrazíte trojúhelníku uživateli.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Spuštění nového projektu

Spuštění sady Visual Studio pro Mac a vytvořte nové řešení Xamarin.Mac. Vyberte **Mac** > **aplikace** > **Obecné** > **kakao aplikace**:

[![](opentk-images/sample01.png "Přidání nové aplikace kakao")](opentk-images/sample01.png#lightbox)

Zadejte `MacOpenTK` pro **projektu název**:

[![](opentk-images/sample02.png "Nastavení názvu projektu")](opentk-images/sample02.png#lightbox)

Klikněte **vytvořit** tlačítko k vytvoření nového projektu.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Včetně OpenTK

Než otevřete TK můžete použít v aplikaci Xamarin.Mac, budete muset obsahovat odkaz na sestavení OpenTK. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **odkazy** složky a vyberte **upravit odkazy...** .

Zaškrtněte podle `OpenTK` a klikněte na **OK** tlačítko:

[![](opentk-images/sample03.png "Úpravy odkazů projektu")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Pomocí OpenTK

S nově vytvořených projektů dvakrát klikněte `MainWindow.cs` souboru v **Průzkumníku řešení** otevřete pro úpravy. Ujistěte se, `MainWindow` třída vypadá takto:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Vraťme se přes tento kód rozpis naleznete níže.

<a name="Required_APIs" />

### <a name="required-apis"></a>Požadované rozhraní API

K použití OpenTK v třídě Xamarin.Mac se vyžaduje několik odkazů. Na začátku definice jsme zahrnuli následující `using` příkazy:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
Tato minimální sada budou potřebné pro všechny třídy pomocí OpenTK.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Přidání herní zobrazení

Dále je potřeba vytvořit zobrazení herní obsahovat všechny naše interakci s OpenTK a zobrazit výsledky. Jsme použili následující kód:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Zde jsme provedli zobrazení herní stejnou velikost jako naše hlavní okno Mac a nahradit zobrazení obsahu okna s novým `MonoMacGameView`. Protože jsme nahradit existující obsah okna, naše Dal zobrazení bude automaticky nastavena velikost při změně velikosti hlavní Windows.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Reagování na události

Nejsou k dispozici několik výchozí události, které by měl odpovídat jednotlivých herní zobrazení. V této části popisuje požadované hlavní události.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Událost zatížení

`Load` Událostí je místo, které načíst prostředky z disku, například bitové kopie, textury nebo Hudba. Pro naše jednoduché testování aplikace nejsou používáme `Load` událostí, ale mít zahrnuté pro referenci:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Událost změny velikosti

`Resize` Události by měla být volána pokaždé, když se změnila velikost zobrazení hra. Pro naše ukázková aplikace vydáváme zobrazení GL stejnou velikost jako naše herní zobrazení (který se automaticky změní velikost okna Hlavní Mac) s následujícím kódem:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Událost UpdateFrame

`UpdateFrame` Událost se používá ke zpracování uživatelského vstupu, aktualizaci objektu pozice, spusťte fyziky nebo AI výpočty. Pro naše jednoduché testování aplikace nejsou používáme `UpdateFrame` událostí, ale mít zahrnuté pro referenci: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Implementace Xamarin.Mac OpenTK nezahrnuje `Input API`, takže budete muset použít Apple zadaný podporují rozhraní API pro přidání klávesnice a myši. Volitelně můžete vytvořit vlastní instanci systému `MonoMacGameView` a přepsat `KeyDown` a `KeyUp` metody.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Událost RenderFrame

`RenderFrame` Událostí obsahuje kód, který se použije k vykreslení (Kreslení) grafiky. Pro náš příklad aplikaci jsme jsou vyplnění herní zobrazení pomocí jednoduchý trojúhelník: 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

Obvykle bude kód vykreslování vrácení pomocí volání `GL.Clear` odebrat existující prvky před vykreslovat nové prvky.

> [!IMPORTANT]
> Verze Xamarin.Mac OpenTK **nepodporují** volání `SwapBuffers` metodu vaší `MonoMacGameView` instance na konci vykreslování kódu. To způsobí, že zobrazení herní zábleskové rychle místo zobrazení vykreslené zobrazení.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Spuštění herní zobrazení

Všechny požadované události definovat a zobrazit herní připojené do okna Hlavní Mac naše aplikace, jsme se čtou spustíte herní zobrazení a zobrazíte naše grafiky. Použijte následující kód:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Jsme předat požadované obnovovací frekvence které chceme herní zobrazení aktualizovat v, pro náš příklad jsme vybrali `60` snímků za sekundu (stejné obnovovací frekvence jako normální TV).

Umožňuje spuštění vaší aplikace a zobrazí se výstup:

[![](opentk-images/intro01.png "Ukázkový výstup aplikace")](opentk-images/intro01.png#lightbox)

Pokud jsme velikost naše okno, herní zobrazení bude také nacházet a trojúhelníku budou po změně velikosti a aktualizovat také v reálném čase.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Kde další?

S základní informace o práci s OpenTk v aplikaci Xamarin.mac Hotovo zde je několik návrhů co můžete vyzkoušet na další:

- Zkuste změnit barvu trojúhelníku a barvu pozadí herní zobrazení v `Load` a `RenderFrame` události.
- Ujistěte se, trojúhelníček změnit barvu, když uživatel klávesu v `UpdateFrame` a `RenderFrame` události nebo se přesvědčte, vlastní vlastní `MonoMacGameView` třídy a přepsat `KeyUp` a `KeyDown` metody.
- Ujistěte se, trojúhelníček přesouvat mezi obrazovky pomocí vědět klíče ve `UpdateFrame` událostí. Pomocný parametr: použijte `Matrix4.CreateTranslation` metodu pro vytvoření překlad matice a volání `GL.LoadMatrix` metoda načtete ji v `RenderFrame` událostí.
- Použití `for` smyčky k vykreslení několik trojúhelníčky v `RenderFrame` událostí.
- Otočte fotoaparát poskytnout jiné zobrazení trojúhelníku v 3D prostoru. Návod: použití `Matrix4.CreateTranslation` metodu pro vytvoření překlad matice a volání `GL.LoadMatrix` metoda načtete ji. Můžete také `Vector2`, `Vector3`, `Vector4` a `Matrix4` třídy pro manipulaci s fotoaparát.

Další příklady naleznete [OpenTK ukázky Githubu](https://github.com/opentk/opentk/tree/master/Source/Examples) úložišti. Obsahuje oficiální seznam příklady použití OpenTK. Budete muset tyto příklady pro použití s verzí Xamarin.Mac OpenTK přizpůsobit.

Příklad složitější Xamarin.Mac OpenTK implementaci, najdete v tématu naše [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) ukázka.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek trvá rychlý přehled práce s OpenTK v aplikaci Xamarin.Mac. Jsme viděli, jak vytvořit okno hra, jak připojit okno HRA do okna Mac a jak k vykreslení jednoduchý obrazec v okně hra.

## <a name="related-links"></a>Související odkazy

- [MacOpenTK (ukázka)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (ukázka)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Práce s Windows](~/mac/user-interface/window.md)
- [Otevřete sadu nástrojů](http://www.opentk.com)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do systému Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
