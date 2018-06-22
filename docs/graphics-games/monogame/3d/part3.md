---
title: 3D souřadnic v MonoGame
description: Principy 3D systém souřadnic je důležitým krokem při vývoji 3D hry. MonoGame poskytuje několik tříd pro umístění, nasměrovat a škálování objektů v 3D prostoru.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 2f14d21302ed4295d16baa28723df6ef79863686
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921635"
---
# <a name="3d-coordinates-in-monogame"></a>3D souřadnic v MonoGame

_Principy 3D systém souřadnic je důležitým krokem při vývoji 3D hry. MonoGame poskytuje několik tříd pro umístění, nasměrovat a škálování objektů v 3D prostoru._

Vývoj 3D hry vyžaduje představu o tom, jak pracovat s objekty v 3D systém souřadnic. Tento názorný postup se zabývá k manipulaci s vizuální objekty (konkrétně modelu). Budete využijeme koncepty řízení model pro vytvoření třídy 3D fotoaparát.

Koncepty uvedené pocházejí z lineární algebra, ale tak, aby libovolného uživatele bez silné matematické pozadí můžete použít tyto koncepty ve svých vlastních hry provedeme praktické přístup.

Jsme vám, že se která obsahuje následující témata:

- Vytvoření projektu
- Vytváření Robot Entity
- Přesunutí Robot Entity
- Násobení matic
- Vytváření entit fotoaparát
- Přesunutí kamera se vstupem

Po dokončení budeme mít projektu s robot Přesun na kruh a fotoaparát, který se dá nastavit podle dotykové ovládání:

![](part3-images/image1.gif "Po dokončení aplikace budou obsahovat projektu s robot Přesun na kruh a fotoaparát, který se dá nastavit podle dotykové ovládání")


## <a name="creating-a-project"></a>Vytvoření projektu

Tento názorný postup se zaměřuje na přesouvání objektů v 3D prostoru. Začneme projekt pro vykreslování modely a pole vrchol [které naleznete zde](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/). Po stažení, rozbalte a otevřete projekt, který má zkontrolujte, zda je spuštěna a bychom měli vidět následující:

![](part3-images/image2.png "Po stažení, rozbalte a otevřete projekt a ujistěte se, spuštění a musí toto zobrazení")


## <a name="creating-a-robot-entity"></a>Vytváření Robot Entity

Než začneme přesunutí naše robot kolem, vytvoříme `Robot` třída obsahuje logiku pro vykreslování a pohyb. Herní vývojáři odkazovat na tuto zapouzdření logiku a data jako *entity*.

Přidat nový soubor prázdné třídy k **MonoGame3D** Přenosná knihovna tříd (ne ModelAndVerts.Android specifické pro platformu). Pojmenujte ji **Robot** a klikněte na tlačítko **nový**:

![](part3-images/image3.png "Název Robot a klikněte na nový")

Upravit `Robot` třídy následujícím způsobem:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

`Robot` Kódu je v podstatě stejný kód v `Game1` pro vykreslování `Model`. Pro kontrolu na `Model` načítání a kreslení, najdete v části [Tato příručka o práci s modely](~/graphics-games/monogame/3d/part1.md). Nyní jsme můžete odebrat všechny `Model` načítání a generování kódu z `Game1`a nahraďte ho `Robot` instance:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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
    }
}
```

Pokud jsme spustit kód teď pomůžeme scény s pouze jeden robot, který je vykreslen většinou pod podlaží:

![](part3-images/image4.png "Pokud kód je spustit nyní, aplikace se zobrazí scény s pouze jeden robot, který je vykreslen většinou pod podlaží")

## <a name="moving-the-robot"></a>Přesunutí robota

Teď, když máme `Robot` třída, přidáme logiku Přesun do robota. V takovém případě stačí vytočit robot přesunout v kruh dle herní času. Toto je poněkud nepraktické implementace pro skutečné hry, protože znak může obvykle reagovat na vstupu nebo umělé inteligence, ale poskytuje prostředí pro nám prozkoumat 3D umístění a oběh.

Veškeré informace, budeme potřebovat z mimo `Robot` třída je aktuální čas herní. Přidáme `Update` metoda, která má být `GameTime` parametr. To `GameTime` se zvýší na proměnnou úhlu, který použijeme k určení poslední pozice pro robota bude použit parametr.

Nejprve přidáme pole úhlu, které chcete `Robot` třídy v části `model` pole:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Nyní jsme můžete tuto hodnotu v zvýšit `Update` funkce:

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Musíme Ujistěte se, že `Update` metoda je volána z `Game1.Update`:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Samozřejmě úhel pole v tomto okamžiku se nic nestane – potřebujeme psaní kódu pro použití. Upravíme `Draw` metoda tak, aby jsme můžete vypočítat na světě `Matrix` vyhrazené metoda: 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

V dalším kroku budete implementaci `GetWorldMatrix` metoda v `Robot` třídy:

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

Výsledek spuštění tento kód z výsledkem robot v kruh přesunutím:

![](part3-images/image5.gif "Spuštění této výsledky kódu v robot v kruh přesunutím")

## <a name="matrix-multiplication"></a>Násobení matic

Výše uvedený kód otočí robota tak, že vytvoříte `Matrix` v `GetWorldMatrix` metoda. `Matrix` Struktura obsahuje 16 float hodnoty, které se dají použít k převede (nastavení pozice), otočit a škálování (nastavit velikost). Když jsme přiřadit `effect.World` vlastnost nám informace o tom, základní vykreslování systému jak pozice, velikost a orientaci ať jsme dojít k být kreslení ( `Model` nebo geometrie z vrcholy). 

Naštěstí `Matrix` struktura zahrnuje několik metod, které zjednodušují vytváření běžné typy matice. První použít ve výše uvedeném kódu je `Matrix.CreateTranslation`. Matematický výraz *překlad* odkazuje na operaci, takže v bodu (nebo v našem případě a modelu) přechod z jednoho umístění do druhého bez dalších úprav (například rotaci nebo změně velikosti). Funkce přebírá hodnotu X, Y a pro překlad. Pokud jsme zobrazit naše scény od shora dolů, naše `CreateTranslation` – metoda (v izolace) provádí následující:

![](part3-images/image6.png "Tato akce provede metodu CreateTranslation v izolaci")

Druhý přehled, který vytváříme je otočení matice pomocí `CreateRotationZ` matice. Toto je jednou ze tří metod, které se dají použít k vytvoření otočení:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Každá metoda vytvoří otočení matice otáčení dané osy. V našem případě jsme rotaci kolem osy Z, který ukazuje "nahoru". Následující může pomoci vizualizovat otočení osy jak na základě funguje:

![](part3-images/image7.png "To může pomoct vizualizovat otočení osy jak na základě funguje")

Také se používá `CreateRotationZ` metoda s polem úhlu, které zvýší časem kvůli naše `Update` volané metodě. Výsledkem je, že `CreateRotationZ` metoda způsobí, že naše robot k Oběžná dráha kolem počátku po uplynutí určitého času.

Poslední řádek kódu kombinuje dvě matice do jedné:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

To se označuje jako násobení matic, který funguje poněkud liší od regulární násobení. *Komutativní vlastnost násobení* stavy, že pořadí čísel v rámci operace násobení nezmění výsledek. To znamená 3 * 4 je ekvivalentní 4 * 3. Násobení matic se liší, že není komutativní. Výše uvedené řádku lze tedy přečíst jako "Použít translationMatrix přesunout modelu a potom vše rotate s použitím rotationMatrix". Jsme může vizualizovat způsobem, že výše řádku ovlivňuje pozice a oběh následujícím způsobem:

![](part3-images/image8.png "Vizualizace pf způsobem, že výše řádku ovlivňuje pozice a otočení")

Chcete-li pochopit, jak pořadí násobení matic může mít vliv na výsledek, zvažte následující, kde je obrácený násobení matic:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Výše uvedený kód by nejprve otočit modelu na místě, a poté přeloží ji:

![](part3-images/image9.png "Výše uvedený kód by nejprve otočit modelu na místě, a poté přeloží ji")

Pokud jsme spustit kód s obráceným násobení, jsme si všimnout, že vzhledem k tomu, že je oběh použije první, ovlivní jenom orientaci modelu a pozice modelu zůstává stejný. Jinými slovy otočí modelu na místě:

![](part3-images/image10.gif "Model otočí na místě")

## <a name="creating-the-camera-entity"></a>Vytváření entit fotoaparát

`Camera` Entity bude obsahovat všechny potřebné k provedení na základě vstup pohyb a zadejte vlastnosti pro přiřazení vlastnosti na logiky `BasicEffect` třídy.

Nejdřív jsme budete implementovat statické fotoaparát (bez přesouvání s založené na vstup) a jeho integraci do našich existující projekt. Přidejte novou třídu do **MonoGame3D** Přenosná knihovna tříd (stejná projektu s `Robot.cs`) a pojmenujte ji **fotoaparát**. Obsah souboru nahraďte následujícím kódem:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Výše uvedený kód je velmi podobný kód z `Game1` a `Robot` v přiřadit matic `BasicEffect`. 

Nyní budeme integrovat nové `Camera` třída do našich existující projekty. Nejprve upravíme `Robot` třídy trvat `Camera` instance v jeho `Draw` metody, která bude eliminovat velké množství duplicitních kódu. Nahraďte `Robot.Draw` metoda následujícím kódem:

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

V dalším kroku změnit `Game1.cs` souboru:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

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
    }
}
```

Změny `Game1` z předchozí verze (které jsou označeny `// New camera code` ) jsou:

- `Camera` pole v `Game1`
- `Camera` vytváření instancí v `Game1.Initialize`
- `Camera.Update` volání v `Game1.Update`
- `Robot.Draw` Nyní trvá `Camera` parametr
- `Game1.Draw` Teď používá `Camera.ViewMatrix` a `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Přesunutí kamera se vstupem

Zatím jsme přidali `Camera` entity, ale ještě nepracovali ho chcete změnit chování za běhu. Chování, které umožňuje uživateli přidáme na:

- Touch levé straně obrazovky fotoaparát směrem doleva
- Touch pravé straně obrazovky fotoaparát vpravo
- Dotykové ovládání center obrazovky přejít kamera

### <a name="making-lookat-relative"></a>Provádění lookAt relativní

Nejprve budete aktualizujeme `Camera` třída zahrnout `angle` pole, které použije se nastavení směru `Camera` směrem k. V současné době naše `Camera` Určuje směr, se kterým čelí prostřednictvím místní `lookAtVector`, který je přiřazen k `Vector3.Zero`. Jinými slovy naše `Camera` vždy vypadá v původu. Pokud se přesune fotoaparát, pak úhlu, se kterým se setkávají fotoaparát změní také:

![](part3-images/image11.gif "Pokud se přesune fotoaparát, pak úhlu, se kterým se setkávají fotoaparát změní také")

Chceme, aby `Camera` na stejném směru bez ohledu na jeho pozice – alespoň připojena dokud jsme implementují logiku pro otáčení `Camera` pomocí vstup. První změna bude upravit `lookAtVector` proměnnou být založen na našem aktuální umístění, nikoli na absolutní pozici vypadající:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

Výsledkem `Camera` zobrazení přímo na celém světě. Všimněte si, že počáteční `position` hodnotu bylo upraveno, aby `(0, 20, 10)` proto `Camera` je umístěn na střed na ose X. Spuštění herní zobrazí:

![](part3-images/image12.png "Spuštění hra zobrazí toto zobrazení")

### <a name="creating-an-angle-variable"></a>Vytváření úhlu proměnné

`lookAtVector` Proměnné řídí úhlu, který naše fotoaparát je zobrazení. Momentálně je nastaven na zobrazení dolů záporné osy Y a mírně nakloněná dolů (z `-.5f` hodnota Z). Vytvoříme `angle` proměnné, která se použije k úpravě `lookAtVector` vlastnost. 

V předchozích částech tohoto návodu jsme vám ukázal, že matice slouží k otáčení, jak jsou vykreslovány objekty. Také můžeme použít matic otočení vektory jako `lookAtVector` pomocí `Vector3.Transform` metoda. 

Přidat `angle` pole a upravovat `ViewMatrix` vlastnost následujícím způsobem:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>Načítání vstupu

Naše `Camera` entity lze nyní plně řídit prostřednictvím jeho pozice a proměnné úhel – potřebujeme je změnit podle vstup.

Nejprve budete získáme `TouchPanel` stavu najít, kde je uživatel klepnou na obrazovce. Další informace o používání `TouchPanel` třídy najdete v tématu [referenční dokumentace rozhraní API TouchPanel](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Pokud je uživatel klepnou na levém třetí pak jsme vám nastavit `angle` hodnota proto `Camera` otočí vlevo, a pokud je uživatel klepnou na pravém třetí, jsme budete otočit jiným způsobem. Pokud uživatel zasahuje do střední třetího na obrazovce a potom přesunete `Camera` dál.

Nejprve přidejte pomocí příkazu, aby se dosáhlo nároku `TouchPanel` a `TouchCollection` třídy v `Camera.cs`:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

V dalším kroku změnit `Update` metoda číst panelu touch a upravit `angle` a `position` proměnné odpovídajícím způsobem:

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

Nyní `Camera` bude odpovídat touch vstup:

![](part3-images/image1.gif "Nyní bude odpovídat touch vstup kamera")

Metoda aktualizace začne voláním `TouchPanel.GetState`, která vrátí kolekci úpravy. I když `TouchPanel.GetState` může vrátit více bodů dotykového ovládání, jsme budete pouze starat o první z nich pro jednoduchost.

Pokud uživatel je klepnou na obrazovce, kód kontroluje zda je první touch v levé, střední nebo právo třetí obrazovky. Levý a pravý třetin otočit fotoaparát zvýšením nebo snížením `angle` proměnné podle `TotalSeconds` hodnotu (tak, aby hra se chová stejně bez ohledu na to obnovovací frekvence).

Pokud uživatel je dotykové ovládání centru třetí obrazovky, pak fotoaparát přesune dál. To lze provést nejdřív získání předat dál vektor, který se původně definován jako směřující na ose Y záporná, pak je otáčet o matice vytvořený `Matrix.CreateRotationZ` a `angle` hodnotu. Nakonec `forwardVector` se použije pro `position` pomocí `unitsPerSecond` koeficient.

## <a name="summary"></a>Souhrn

Tento názorný postup obsahuje informace o přesunutí a otočit `Models` v 3D prostoru pomocí `Matrices` a `BasicEffect.World` vlastnost. Tato forma přesunu poskytuje základ pro přesouvání objektů v 3D hry. Tento postup platí i pro implementaci `Camera` entity pro zobrazení na světě z jakékoli pozice a úhlu.

## <a name="related-links"></a>Související odkazy

- [Odkaz MonoGame rozhraní API](http://www.monogame.net/documentation/?page=api)
- [Dokončení projektu (ukázka)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
