---
title: Úvod do UrhoSharp
description: To poskytuje stručný úvod do Principy UrhoSharp
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 243498e1d5a24a0a6b8d1e911b374df61dfa6971
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="an-introduction-to-urhosharp"></a>Úvod do UrhoSharp

_To poskytuje stručný úvod do Principy UrhoSharp_

![](introduction-images/urhosharp-icon.png "UrhoSharp je výkonný stroj herní 3D pro vývojáře, Xamarin a rozhraní .NET")

UrhoSharp je výkonný stroj herní 3D pro vývojáře, Xamarin a rozhraní .NET.  Je podobná v smyslu SceneKit a SpriteKit společnosti Apple a zahrnují fyziky, navigace, sítě a mnohem víc při stále probíhá pro různé platformy.

Je rozhraní .NET vazbu ke [Urho3D](http://urho3d.github.io/) modul a umožňuje vývojářům psát kód křížové platformy, který může cílit na Android, iOS, Windows a Mac se stejným základu kódu a může vykreslit systémech OpenGL a Direct3D.

UrhoSharp je modul herní s velkým množstvím funkce mimo pole:

 - Efektivní 3D grafický vykreslování
- [Fyzice simulace](https://developer.xamarin.com/api/namespace/Urho.Physics/) (s použitím knihovny odrážek)
- [Zpracování scény](https://developer.xamarin.com/api/type/Urho.Scene/)
- Await nebo asynchronní podpora
- [Popisný akce rozhraní API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [2D integrace do 3D scény](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Vykreslování písma s FreeType](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [Klient a server možnosti sítě](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Import širokou škálu prostředků](https://developer.xamarin.com/api/namespace/Urho.Resources/) (s otevřete prostředky knihovny)
- [Navigace OK a pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (pomocí přepracovat/Detour)
- [Generování konvexní trupu pro detekce kolizí](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (pomocí StanHull)
- [Přehrávání zvuku](https://developer.xamarin.com/api/namespace/Urho.Audio/) (s **libvorbis**)

# <a name="getting-started"></a>Začínáme

UrhoSharp pohodlně je distribuován jako [balíček NuGet](https://www.nuget.org/) a lze ji přidat do vašich projektů jazyka C# nebo F # cílených Windows, Mac, Android nebo iOS.  NuGet se dodává s knihovny potřebné ke spuštění programu, jak základní prostředky (CoreData) používá stroj.

## <a name="urho-as-a-portable-class-library"></a>Urho jako knihovny přenosných tříd

Balíček Urho mohou být využívány buď z projektu specifických pro platformy, nebo z projektu knihovny přenosných tříd, abyste mohli znovu použít veškerý kód pro všechny platformy.  To znamená, že všechny, které budete muset udělat na každou platformu je pro vaše platformy konkrétní vstupní bod zápisu a potom přeneste řízení sdílené herní kódu.

## <a name="samples"></a>Ukázky kódu

Získáte tak, že otevřete v sadě Visual Studio pro Mac nebo Visual Studio ukázkové řešení z chuť pro možnosti Urho:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Výchozí řešení obsahují projekty pro Android, iOS, Windows a macu.  Budeme mít strukturován toto řešení tak, aby máme konkrétní Spouštěče jen nepatrnou platformy a všechny ukázkový kód a herní kód je umístěn v knihovny přenosných tříd, znázorňující způsob maximalizovat opětovné použití kódu pro všechny platformy.

Obrátit [Urho a vaše platforma](~/graphics-games/urhosharp/platform/index.md) stránka Další informace o tom, jak vytvářet vlastní řešení.

Vzhledem k tomu, že všechny ukázky sdílejí společnou sadu prvky uživatelského rozhraní, ukázky mít abstrahované základní nastavení v tomto souboru:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

To poskytuje ukázkové základní třídu, která zpracovává některé základní stisknutí kláves a touch události, nastavení fotoaparátu, poskytuje základní prvky rozhraní a to umožňuje jednotlivé ukázky a zaměřit se na specifické funkce, které je právě showcased.

Následující příklad ukazuje, co modul je schopen to:

- [Samply her](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) jednoduché klon ShootySkies.

Při další ukázky zobrazit jednotlivé vlastnosti každého vzorku.

# <a name="basic-structure"></a>Základní struktura

Podtřídy by vaše hra [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/) třída, to je, kde bude instalační program vaše hra (na [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) metoda) a spusťte vaše hra (v [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) metoda).  Potom můžete vytvořit hlavní uživatelské rozhraní.  Přidáme provede malé vzorku, který ukazuje rozhraní API pro 3D scény, některé prvky uživatelského rozhraní a jednoduché chování se připojuje k němu instalačního programu.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
    // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

Pokud spustíte tuto aplikaci, rychle zjistíte, že je modul runtime stěžovat vaše prostředky nejsou existuje.  Co musíte udělat je vytvoření hierarchie ve vašem projektu, který začíná název speciální adresáře "Data" a uvnitř tohoto by umístíte prostředky, které odkazují v programu.  Pak musíte nastavit ve vlastnostech položky pro každý prostředek "Kopírovat do výstupního adresáře" na "Kopie Pokud novější", bude zajištěno, že vaše data existuje.

Dejte nám popisují, co se děje na sem.

Ke spuštění aplikace volání funkce inicializace modulu, za nímž následuje vytvoření nové instance třídy vaší aplikace, jako to:

    new MySample().Run();

Modul runtime vyvolá `Setup` a `Start` metody pro vás.  Pokud přepíšete `Setup` můžete nakonfigurovat parametry modulu (ne zobrazit v této ukázce).

Je nutné přepsat `Start` jako tím se spustí vaše hra.  Tato metoda se načíst vaše prostředky, připojení obslužné rutiny událostí, instalační program vaše scény a spustit všechny akce, které očekáváte.  Ve výběru jsme obě vytvořit jenom trocha uživatelského rozhraní k zobrazení pro uživatele, jakož i nastavení 3D scény.

Následující část kódu, pomocí uživatelského rozhraní framework vytvořte textový element a přidejte jej do vaší aplikace:

        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from UrhoSharp",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

Rozhraní framework uživatelského rozhraní je zajistit velmi jednoduché ve hře uživatelské rozhraní a funguje přidáním nových uzlů k [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) uzlu.

Druhá část naší příklady způsobu nastavení hlavní scény.  To zahrnuje několik kroků, vytváření 3D scény, vytváření 3D pole v dialogovém okně Přidání lehkým, fotoaparátu a zobrazení.  Tyto jsou prozkoumali podrobněji v části "[scény, uzlů, komponent a fotoaparáty](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)"

Třetí součást naše ukázka aktivuje několik akcí.  Akce jsou recepty, které popisují konkrétní vliv a po jejich vytvoření může provést uzel na vyžádání pomocí volání [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) metodu `Node`.

Je první akcí škáluje pole s skákající účinek a druhý navždy otočí pole:

    await boxNode.RunActionsAsync(
        new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));

Výše ukazuje, jak je první akci, která se nám vytvořit [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) akci, jedná se jenom tajný recept, která určuje, že chcete škálovat pro druhý směrem na hodnotu jedna vlastnost škálování uzlu.  Tato akce je zase zalomen okolo nejvýraznější akce, [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) akce.  Nejvýraznější akce narušují lineární provádění akce a použití efektu, v takovém případě poskytuje účinek skákání na více systémů.
Naše recepturách tak může zapsat jako:

    var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));

Po vytvoření receptury, můžeme provést receptury:

    await boxNode.RunActionsAsync (recipe)

Await znamená, že potřeba po dokončení akce pokračovat v provádění po tomto řádku.  Po dokončení akce se spouští druhý animace.

[Pomocí UrhoSharp](~/graphics-games/urhosharp/using.md) dokumentu prozkoumá podrobněji, další koncepty za Urho a postup struktury kódu k vytvoření hry s.

# <a name="copyrights"></a>Autorská práva

Tato dokumentace obsahuje původní obsah z Xamarin Inc, ale nevykresluje hojně v dokumentaci k s otevřeným zdrojem pro projekt Urho3D a obsahuje snímky obrazovky z projektu Cocos2D.



## <a name="related-links"></a>Související odkazy

- [Sešit planetu země](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Zkoumání souřadnice sešitu](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
