---
title: Používání třídy modelu
description: Třídy modelu výrazně se zjednodušuje vykreslování komplexní 3D objekty v porovnání s tradiční způsob vykreslení 3D grafický. Objekty modelů jsou vytvořeny z obsahu souborů, což umožňuje snadnou integraci obsahu s žádné vlastní kód.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 7e778df7fa6dd27aee8282154c99faf5ca5791ce
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="using-the-model-class"></a>Používání třídy modelu

_Třídy modelu výrazně se zjednodušuje vykreslování komplexní 3D objekty v porovnání s tradiční způsob vykreslení 3D grafický. Objekty modelů jsou vytvořeny z obsahu souborů, což umožňuje snadnou integraci obsahu s žádné vlastní kód._

Zahrnuje rozhraní API MonoGame `Model` třída, která slouží k ukládání dat načíst ze souboru obsahu a k provádění vykreslování. Soubory modelu může být velmi jednoduché (například plnou barevnou trojúhelník) nebo může zahrnovat informace o komplexní vykreslování, včetně tvarování a osvětlení.

Tento návod používá [3D model robot](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) a obsahuje následující:

- Spuštění nového projektu herní
- Vytváření XNBs pro model a jeho textury
- Včetně XNBs v herní projektu
- Kreslení 3D modelu
- Kreslení více modelů

Po dokončení naše projektu bude vypadat takto:

![Dokončení ukázkové zobrazující šesti robotů](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>Vytvoření prázdné herní projektu

Budeme potřebovat nastavit herní projektu nejdříve volána MonoGame3D. Informace o vytvoření nového projektu MonoGame najdete v tématu [tento návod na vytvoření projektu křížové platformy Monogame](~/graphics-games/monogame/introduction/part1.md).

Než budete pokračovat jsme měli ověřte, zda projekt otevře a nasadí správně. Jednou nasazené bychom měli vidět prázdnou obrazovku blue:

![Prázdná obrazovka blue herní](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>Včetně XNBs v herní projektu

Formát souboru .xnb je standardní příponou integrovaný obsahu (obsah, který byl vytvořen [MonoGame kanálu nástroj](http://www.monogame.net/documentation/?page=Pipeline)). Všechny vytvořené obsah obsahuje zdrojový soubor (což je soubor .fbx v případě našeho modelu) a cílový soubor (soubor .xnb). Běžné formátu 3D modelu, který lze vytvořit pomocí aplikací, jako je formát .fbx [Maya](http://www.autodesk.com/products/maya/overview) a [digestoru](http://www.blender.org/). 

`Model` Třída konstruovat načtením soubor .xnb z disku, který obsahuje data 3D geometrie.   Tento soubor .xnb je vytvořen pomocí obsahu projektu. Šablony Monogame automaticky zahrnují obsahu projektu (s příponou .mgcp) v našem obsahu složky. Podrobné informace o nástroji MonoGame kanálu, najdete v článku [obsahu kanálu průvodce](~/graphics-games/cocossharp/content-pipeline/index.md).

Tato příručka jsme budete přeskočit MonoGame zřetězením příkazů nástroje a používá. XNB soubory obsažené v tomto poli. Všimněte si, že. Soubory XNB se liší podle platformy, takže je nutné použít správné sady XNB soubory pro kteroukoli platformu pracujete s.

Jsme budete rozbalte [Content.zip soubor](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) tak, aby používáme soubory obsažené .xnb v našem hra. Pokud funguje na projekt pro Android, klikněte pravým tlačítkem na **prostředky** složku **WalkingGame.Android** projektu. Pokud funguje na projekt pro iOS, klikněte pravým tlačítkem na **WalkingGame.iOS** projektu. Vyberte **Přidat -> Přidat soubory...**  a vyberte oba .xnb soubory ve složce pro platformu pracujete.

Dva soubory musí být součástí našich projektu nyní:

![Složky obsahu Průzkumník řešení se xnb soubory](part1-images/xnbsinxs.png)

Visual Studio pro Mac, nemusí nastavovat automaticky akce sestavení pro nově přidané XNBs. Pro iOS, klikněte pravým tlačítkem na všechny soubory a vyberte **akce sestavení -> BundleResource**. Pro Android, klikněte pravým tlačítkem na všechny soubory a vyberte **akce sestavení -> AndroidAsset**.

## <a name="rendering-a-3d-model"></a>Vykreslování 3D modelu

Nezbytné zobrazíte modelu na obrazovce posledním krokem je přidání načítání a kreslení kódu. Konkrétně jsme budete mít následujícím způsobem:

- Definování `Model` instance v našem `Game1` – třída
- Načítání `Model` instance v `Game1.LoadContent`
- Kreslení `Model` instance v `Game1.Draw`

Nahraďte `Game1.cs` souboru kódu (který se nachází ve **WalkingGame** PCL) následujícím kódem:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

Pokud jsme spustit tento kód jsme zobrazí na obrazovce se model:

![Model zobrazené na obrazovce](part1-images/image8.png "Pokud tento kód běží, model se zobrazí na obrazovce")

### <a name="model-class"></a>Třídy modelu

`Model` Třída je základní třída pro provádění 3D vykreslování z obsahu souborů (například soubory .fbx). Obsahuje všechny informace potřebné pro vykreslování, včetně 3D geometrie texture odkazy a `BasicEffect` instancí, které řídí hodnoty umístění, osvětlení a fotoaparát.

`Model` Vlastní třídy nemá přímo proměnných pro umístění vzhledem k tomu, že instance jednotný model mohou být vykresleny v několika umístěních, jako ukážeme v této příručce.

Každý `Model` se skládá z jedné nebo více `ModelMesh` instance, které jsou k dispozici prostřednictvím `Meshes` vlastnost. I když lze považovat za `Model` jako jeden herní objektu (například robot nebo automobilu), každý `ModelMesh` lze rozlišovat jiné `BasicEffect` hodnoty. Například oko částí může představovat úsecích robot nebo souborů Wheel na automobilu a jsme může přiřadit `BasicEffect` hodnoty tak, aby typu číselník souborů Wheel nebo nohy přesunout. 

### <a name="basiceffect-class"></a>BasicEffect – třída

`BasicEffect` Třída poskytuje vlastnosti pro řízení možnosti vykreslování. První úpravy provedeme `BasicEffect` je volání `EnableDefaultLighting` metoda. Jak již název napovídá, to umožňuje osvětlení výchozí, což je velmi užitečný pro ověření, který `Model` se zobrazí ve hře podle očekávání. Pokud jsme komentář `EnableDefaultLighting` volat, pak ukážeme modelu vykreslen pomocí právě jeho texture, ale pomocí žádné stínování nebo zrcadlová záře:

```csharp
//effect.EnableDefaultLighting ();
```

![Modelu vykreslen pomocí právě jeho texture, ale pomocí žádné stínování nebo zrcadlová záře](part1-images/image9.png "modelu vykreslen pomocí právě jeho texture, ale pomocí žádné stínování nebo zrcadlová záře")

`World` Vlastnost lze upravit pozice, otáčení a škálování modelu. Kód výše používá `Matrix.Identity` hodnotu, která znamená, že `Model` vykreslí ve hře přesně tak, jak v souboru .fbx. Jsme budete pokrývajících 3D souřadnice v podrobněji a matic [část 3](~/graphics-games/monogame/3d/part3.md), ale jako příklad Změníme pozici `Model` změnou `World` vlastnost následujícím způsobem:

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Tento kód vrátí objekt přesouvání 3 jednotkami world:

![Tento kód výsledkem objekt přesouvání 3 jednotkami world](part1-images/image10.png "tento kód výsledkem objekt přesouvání 3 world jednotkami")

Poslední dvě vlastnosti přiřadit `BasicEffect` jsou `View` a `Projection`. Jsme budete pokrývajících 3D kamery v [část 3](~/graphics-games/monogame/3d/part3.md), ale jako příklad jsme lze změnit pozice kamery změnou místní `cameraPosition` proměnné:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Uvidíte, kamera přesunul další zpět, což vede k `Model` zobrazování menší kvůli perspektivy:

![Kamera přesunul další zpět, což vede k modelu zobrazování menší kvůli perspektivy](part1-images/image11.png "kamera přesunul další zpět, což vede k modelu zobrazování menší kvůli perspektivy")

## <a name="rendering-multiple-models"></a>Vykreslování více modelů

Jak je uvedeno výše, jedním `Model` lze rozlišovat vícekrát. Pro tuto činnost usnadnit jsme bude přesunutí `Model` kreslení kódu do své vlastní metody, která přijímá požadovanou `Model` pozici jako parametr. Po dokončení naše `Draw` a `DrawModel` metody bude vypadat podobně jako:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
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
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

Výsledkem je robot modelu přitahuje šestkrát:

![Výsledkem je robot modelu přitahuje šestkrát](part1-images/image1.png "výsledkem robot přitahuje šestkrát modelu")

## <a name="summary"></a>Souhrn

Tento návod představil na MonoGame `Model` třídy. Pokrývá převodu soubor .fbx .xnb, který je naopak možné načíst do `Model` třídy. Také ukazuje, jak změny `BasicEffect` instance může mít vliv na `Model` kreslení.

## <a name="related-links"></a>Související odkazy

- [Odkaz na MonoGame modelu](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Dokončené projektu (ukázka)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
