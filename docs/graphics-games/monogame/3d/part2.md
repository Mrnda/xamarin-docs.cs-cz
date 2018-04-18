---
title: Kreslení 3D grafický s vrcholy v MonoGame
description: MonoGame podporuje použití pole vrcholy k definování vykreslení 3D objektu na základě za bod. Uživatelé mohou využít výhod vrchol pole vytvářet dynamické geometry, implementovat speciální efekty a zlepšit efektivitu jejich vykreslení prostřednictvím brakování.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4736bedd413663af098bbad522cc56f432e36ea0
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Kreslení 3D grafický s vrcholy v MonoGame

_MonoGame podporuje použití pole vrcholy k definování vykreslení 3D objektu na základě za bod. Uživatelé mohou využít výhod vrchol pole vytvářet dynamické geometry, implementovat speciální efekty a zlepšit efektivitu jejich vykreslení prostřednictvím brakování._

Uživatelé, kteří mají pročtěte [příručce na vykreslování modely](~/graphics-games/monogame/3d/part1.md) bude znát vykreslování 3D modelu v MonoGame. `Model` Efektivní způsob, jak vykreslit 3D grafický při práci s daty definovanými v souboru (například .fbx) a při plánování práce s statických dat je třída. Některé hry vyžadují 3D geometrie na nelze definovat ani s nimi manipulovat dynamicky za běhu. V těchto případech můžeme použít pole *vrcholy* definovat a vykreslit geometrie. Vrchol je obecný termín, na bod v 3D místa, která je součástí používá k definování geometrie uspořádaného seznamu. Obvykle jsou v tak, aby definovat řadu trojúhelníčky seřazené vrcholy.

Pomoc při vizualizovat jak vrcholy slouží k vytváření 3D objektů, zvažte následující oblasti:

![](part2-images/image1.png "Chcete-li pomoci vizualizovat jak vrcholy slouží k vytváření 3D objektů, zvažte této oblasti")

Jako v příkladu nahoře, se jasně skládá z několika trojúhelníčky oblasti. Jsme můžete zobrazit obrázek oblasti zobrazíte připojení vrcholy k trojúhelníčky formuláře:

![](part2-images/image2.png "Zobrazit obrázek oblasti zobrazíte připojení vrcholy k trojúhelníčky formuláře")

Tento návod popisuje v následujících tématech:

- Vytvoření projektu
- Vytváření vrcholy
- Přidání kódu kreslení
- Vykreslování texturou
- Úprava souřadnice textury
- Vykreslování vrcholy s modely

Dokončení projektu bude obsahovat šachovnicová mřížka floor, které budou vykreslovat pomocí vrchol pole:

![](part2-images/image3.png "Dokončení projektu bude obsahovat šachovnicová mřížka floor, které budou vykreslovat pomocí vrchol pole")

## <a name="creating-a-project"></a>Vytvoření projektu

Nejdřív jsme budete si stáhněte projekt, který bude sloužit jako naše počáteční bod. Použijeme projektu modelu [které naleznete zde](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/).

Jakmile stažené a rozbalené, otevřete a spusťte projekt. Očekáváme, že zobrazíte šesti robot modely přitahuje na obrazovce:

![](part2-images/image4.png "Šest robot modely přitahuje na obrazovce")

Na konci tohoto projektu jsme budete mít kombinování vlastní vykreslování vlastní vrchol se robota `Model`, takže jsme nebudete odstranit kód vykreslování robot. Místo toho jsme vám právě zrušte si `Game1.Draw` kreslení 6 robotů prozatím o odebrání. Chcete-li to provést, otevřete **Game1.cs** souborů a vyhledejte `Draw` metoda. Upravte jej tak, aby obsahovala následující kód:

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

V důsledku toho se naše herní zobrazení prázdnou obrazovku blue:

![](part2-images/image5.png "Tato akce způsobí herní zobrazení prázdnou obrazovku modrá")

## <a name="creating-the-vertices"></a>Vytváření vrcholy

Vytvoříme pole vrcholy k definování naše geometrie. V tomto návodu budeme budete vytváření 3D roviny (čtverce v 3D prostoru, není letadle). I když naše roviny má čtyři strany a čtyři rohy, se skládá ze dvou trojúhelníky, z nichž každá vyžaduje tři vrcholy. Proto jsme bude možné definování celkem šest bodů.

Pokud jsme jste byla posuzování vrcholy v obecném smyslu, ale MonoGame poskytuje některé standardní struktur, který může být použit pro vrcholy:

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Název každého typu označuje součásti, které obsahuje. Například `VertexPositionColor` obsahuje hodnoty pro pozici a barvu. Podívejme se na všechny komponenty:

- Zahrnout všechny typy vrchol pozice – `Position` součásti. `Position` Hodnoty definovat umístění vrchol v 3D prostoru (X, Y a Z).
- Barva – vrcholy Volitelně můžete zadat `Color` hodnotu k provedení vlastní barevný nádech.
- Normální – normály definovat toho, jak je směřující na povrch objektu. Normály jsou nezbytné, že pokud vykreslování objekt s osvětlení od směr, který povrch čelí ovlivňuje kolik světla obdrží. Normály se obvykle zadává jako *jednotky vektoru* – 3D vektor o délce 1.
- Texture – Texture odkazuje na souřadnice texture – to znamená, jaká část texturou by se měla objevit na danou vrcholu. Texture hodnoty jsou nezbytné, pokud objekt 3D texturou vykreslování. Texture souřadnice jsou normalizovaný souřadnice, což znamená, že bude spadat hodnoty mezi 0 a 1. Jsme zaměříme texture souřadnice podrobněji dál v této příručce.

Naše roviny bude sloužit jako podlaží a jsme budete chtít použít texturou při provádění naše vykreslování, takže použijeme `VertexPositionTexture` typu k definování naše vrcholy.

Nejprve přidáme členem naší třídy Game1:

```csharp
VertexPositionTexture[] floorVerts; 
```

V dalším kroku definovat naše vrcholy v `Game1.Initialize`. Všimněte si, že zadaná šablona odkazované dříve v tomto článku neobsahuje `Game1.Initialize` metoda, takže potřebujeme přidejte celý metodu pro `Game1`:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

Pomoc při vizualizovat vzhled naše vrcholy, vezměte v úvahu následující diagram:

![](part2-images/image6.png "Pomoc při vizualizovat vzhled vrcholy, zvažte tohoto diagramu")

Musíme spoléhají na našich diagram k vizualizaci vrcholy až dokončíme implementace vykreslování kódu.

## <a name="adding-drawing-code"></a>Přidání kódu kreslení

Teď, když máme pozice pro naše geometrie definované, jsme můžete napsat kód naše vykreslování.

Nejprve je třeba definovat `BasicEffect` instanci, která bude obsahovat parametry pro vykreslování například pozici a osvětlení. Chcete-li to provést, přidejte `BasicEffect` člena `Game1` třída níže where `floorVerts` pole není definováno:


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

V dalším kroku změnit `Initialize` metoda definovat účinek:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Teď přidáme můžete kód a proveďte kreslení:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start 
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

Budeme muset volat `DrawGround` v našem `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

Aplikace se zobrazí při spuštění následující:

![](part2-images/image7.png "Aplikace se zobrazí tato při spuštění")

Podívejme se na některé podrobnosti ve výše uvedeném kódu.

### <a name="view-and-projection-properties"></a>Zobrazení a projekce vlastnosti

`View` a `Projection` vlastnosti řídit, jak jsme scény zobrazení. Tento kód jsme budete mít úpravy později, pokud jsme znovu přidejte kód vykreslování modelu. Konkrétně `View` Určuje umístění a orientaci fotoaparátu, a `Projection` ovládací prvky *zobrazovanou* (který lze použít pro přiblížení kamera).

### <a name="techniques-and-passes"></a>Techniky a předává

Jednou přiřadili jsme vlastnosti na našem požadavky, můžete provést skutečné vykreslování. 

Jsme nebude změna `CurrentTechnique` vlastnost tento návod, ale pokročilejší hry může mít jeden vliv, které můžete provádět kreslení různými způsoby (například jak, je použita hodnota barvu). Každý z těchto režimů vykreslování může být reprezentován jako technik, které lze přiřadit před vykreslování. Kromě toho každý technika může vyžadovat více předává k vykreslení správně. Účinky může být nutné několik předává Pokud vykreslování komplexní vizuální prvky, jako je Zářící prostor nebo kožešiny.

Je důležité si pamatovat, že `foreach` smyčky umožňuje stejné kódu C# k vykreslení nijak neprojeví bez ohledu na složitosti základní `BasicEffect`.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` je, kde jsou vykreslovány vrcholy. První parametr informuje metodu, jak budeme mít uspořádané naše vrcholy. Budeme mít je strukturován tak, aby každý trojúhelníček je definována tři seřazené vrcholy, takže používáme `PrimitiveType.TriangleList` hodnotu.

Druhý parametr je pole vrcholy, které jsme definovali dříve.

Třetí parametr určuje první index k vykreslení. Vzhledem k tomu, že chceme, aby naše celý vrchol pole k vykreslení, jsme budete předáte hodnotu 0.

Nakonec jsme určit, kolik trojúhelníčky k vykreslení. Naše vrchol pole obsahuje dvě trojúhelníčky, takže předat hodnotu 2.

## <a name="rendering-with-a-texture"></a>Vykreslování texturou

V tomto okamžiku naše aplikace vykreslí bílé roviny (v Perspektiva). Další přidáme texturou do našich projekt má být použit při vykreslení naše roviny. 

Pro zjednodušení přidáme .png přímo do našich projektu, nikoli pomocí nástroje MonoGame kanálu. Chcete-li to provést, stáhněte [tento soubor .png](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) do vašeho počítače. Po stažení, klikněte pravým tlačítkem na **obsahu** složky v řešení pro a vyberte **Přidat > Přidat soubory...**  . Pokud funguje v systému Android, pak tato složka bude umístěna pod **prostředky** složky v projektu specifické pro Android. Pokud v systému iOS, pak tato složka bude v kořenu projektu pro iOS. Přejděte do umístění, kde **checkerboard.png** je uložit a vyberte tento soubor. Vyberte, chcete-li zkopírujte soubor do adresáře.

V dalším kroku kód k vytvoření přidáme naše `Texture2D` instance. Nejprve přidejte `Texture2D` jako člen skupiny `Game1` pod `BasicEffect` instance:

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Upravit `Game1.LoadContent` následujícím způsobem:


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

V dalším kroku změnit `DrawGround` metoda. Je nezbytné pouze úpravy přiřadit `effect.TextureEnabled` k `true` a nastavit `effect.Texture` k `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

    // new code:
    effect.TextureEnabled = true;
    effect.Texture = checkerboardTexture;

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
                    PrimitiveType.TriangleList,
            floorVerts,
            0,
            2);
    }
}
```

Nakonec je potřeba upravit `Game1.Initialize` metoda také přiřadit texture koordinuje na našem vrcholy:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

Pokud jsme spustit kód, jsme můžete zjistit, že naše roviny nyní zobrazuje šachovnicový vzor:

![](part2-images/image8.png "Rovině teď zobrazuje šachovnicový vzor")

## <a name="modifying-texture-coordinates"></a>Úprava Texture koordinuje

Používá MonoGame normalized texture souřadnice, které jsou souřadnice mezi 0 a 1 místo mezi 0 a textury šířky nebo výšky. Následující diagram vám mohou pomoci vizualizovat normalizovaný souřadnice:

![](part2-images/image9.png "Tento diagram může pomoci vizualizovat normalizovaný souřadnice")

Souřadnice normalizovaný texture povolit změnu velikosti texture bez nutnosti přepisovat kód nebo znovu vytvořit modely (například soubory .fbx). To je možné, protože souřadnice normalizovaný představují poměr, nikoli konkrétní pixelů. Například (1,1) bude vždy představují pravém dolním rohu bez ohledu na velikost texture.

Jsme můžete změnit přiřazení souřadnic texture používat jednu proměnnou pro počet opakování:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Výsledkem je texture opakující se 20krát:

![](part2-images/image10.png "Výsledkem je texture 20krát opakují.")


## <a name="rendering-vertices-with-models"></a>Vykreslování vrcholy s modely

Teď, když je naše roviny vykreslování správně, jsme modely, které budou společně zobrazit vše, co znovu přidat. Nejprve znovu přidáme kód modelu k naší `Game1.Draw` – metoda (s upravené pozic):

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

Vytvoříme i `Vector3` v `Game1` představující pozici naše fotoaparát. Přidáme pole v části našich `checkerboardTexture` deklarace:

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

V dalším kroku odebrat místní `cameraPosition` proměnnou z `DrawModel` metoda:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

Podobně odebrat místní `cameraPosition` proměnnou z `DrawGround` metoda:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

Teď Pokud spustíme kód jsme viděli modely i základů ve stejnou dobu:

![](part2-images/image11.png "Modely a základů se zobrazí ve stejnou dobu")

Pokud jsme upravit fotoaparát pozice (například zvýšením jeho hodnota X který v tomto případě posouvá fotoaparát doleva) uvidíte, že hodnota ovlivňuje základů a modely:

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Tento kód vrátí následující:

![](part2-images/image3.png "Tento kód výsledků v tomto zobrazení")

## <a name="summary"></a>Souhrn

Tento návod vám ukázal, jak využít pole vrchol k vlastní vykreslení. V takovém případě jsme vytvořili šachovnicová mřížka podlaží kombinací naše založené na vrchol vykreslování texturou a `BasicEffect`, ale kód uvedeny v tomto tématu slouží jako základ pro všechny 3D vykreslování. Také ukázalo, že vrchol na základě vykreslování můžete smíšený s modely v stejné scény.

## <a name="related-links"></a>Související odkazy

- [Soubor šachovnice (ukázka)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Dokončené projektu (ukázka)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
