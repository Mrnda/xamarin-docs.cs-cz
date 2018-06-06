---
title: SceneKit v Xamarin.iOS
description: Tento dokument popisuje SceneKit 3D scény graph API, která zjednodušuje práci s 3D grafický tím, že poskytuje abstrakci rychle složitosti OpenGL.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: fb72e194e14f903061e1bd2dc6d04ef88ab429d4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786782"
---
# <a name="scenekit-in-xamarinios"></a>SceneKit v Xamarin.iOS

SceneKit je rozhraní API, které usnadňuje práci s 3D grafický graf 3D scény. Bylo poprvé dostupné ve OS X 10.8 a teď zase na iOS 8. S SceneKit vytváření dokonalé 3D vizualizace a běžné 3D hry nevyžaduje odborných znalostí v OpenGL. Sestavování na běžné koncepty grafu scény, SceneKit abstrahuje rychle složitosti OpenGL a ES OpenGL, což velmi usnadňuje přidat 3D obsahu k aplikaci. Pokud jste odborník OpenGL, má SceneKit však podpory pro příkazů přímo s OpenGL také. Také zahrnuje množství funkcí, které doplňují 3D grafiky, jako je například fyziky a velmi dobře se integruje se službou několik dalších rozhraní Apple, například základní animace, základní Image a pohyblivý symbol Kit.

SceneKit je velmi snadné pracovat. Je deklarativní rozhraní API, který má na starosti vykreslování. Jednoduše nastavit scény, přidání vlastnosti do a zpracovává SceneKit vykreslování scény.

Pro práci s SceneKit vytvoříte pomocí graf scény `SCNScene` třídy. Scény obsahuje hierarchii uzlů reprezentována instancí `SCNNode`, definování umístění v 3D prostoru. Každý uzel má vlastnosti, například geometry, osvětlení a materiálů, které ovlivňují její vzhled, jak vidíte na následujícím obrázku:

![](scenekit-images/image7.png "Hierarchie SceneKit") 

## <a name="create-a-scene"></a>Vytvoření scény

Chcete-li scény zobrazí na obrazovce, můžete ho přidat do `SCNView` přiřazením vlastnost scény zobrazení. Kromě toho, pokud provedete změny scény, `SCNView` bude konzola aktualizovat zobrazíte změny.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Scény je možné importovat z soubory exportované prostřednictvím 3d modelování nástroj nebo prostřednictvím kódu programu geometrickou primitiv. To je třeba, jak vytvořit oblasti a přidat jej do scény:

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Přidání světlý

V tomto okamžiku oblasti nezobrazí nic protože v scény neexistuje žádné indikátor. Připojení `SCNLight` instance do uzlů vytvoří indikátory v SceneKit. Existuje několik typů indikátory od různé formy směrovou osvětlení až osvětlení okolí. Například následující kód vytvoří ve všech směrech light stranu oblasti:

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

Ve všech směrech osvětlení vytvoří rozptýlených reflexe výsledkem i osvětlení, seřadit podobný osvětlení výstražná. Vytvoření okolního světla je podobné, i když má žádné směr jako září stejně ve všech směrech. Si ho představit jako vyznění osvětlení :)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

S indikátory v místě je nyní oblasti v scény viditelné.

![](scenekit-images/image8.png "Oblasti se zobrazí na scény lit")
 
## <a name="adding-a-camera"></a>Přidání fotoaparátu

Přidání fotoaparátu (SCNCamera) do scény změní hlediska. Vzor přidat fotoaparát je podobné. Vytvořte fotoaparát, připojte ji k uzlu a přidání uzlu do scény.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Jak vidíte z kódu výše SceneKit objekty mohou být vytvořeny pomocí konstruktorů nebo z metody vytvoření objektu pro vytváření. První umožňuje pomocí syntaxe inicializátoru C#, ale které z nich používat je z velké části řádu předvoleb.

S fotoaparátu na místě se zobrazí uživateli celé oblasti:

![](scenekit-images/image9.png "Celé oblasti je viditelná pro uživatele")
 
Můžete přidat další indikátory scény také. Zde je, jak vypadá s několika další indikátory ve všech směrech:

![](scenekit-images/image10.png "Oblasti s několika další indikátory ve všech směrech")
 
Kromě toho, když nastavení `sceneView.AllowsCameraControl = true`, uživatel může změnit hlediska s gesto dotykového ovládání.

### <a name="materials"></a>Materiály

Materiály jsou vytvořeny pomocí třídy SCNMaterial. Například přidávat bitovou kopii na plochu oblasti nastavení obrázku k těmto informacím *rozptýlené* obsah.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Této vrstvy bitovou kopii do uzlu, jak je uvedeno níže:

![](scenekit-images/image11.png "Vrstvení bitovou kopii do oblasti")
 
Materiálu můžete nastavit reagovat na jiné typy osvětlení příliš. Například objekt můžete provedeny lesklý a jeho zrcadlová obsah nastavili jste zobrazíte zrcadlová reflexe, což vede k jasně pozici na povrchu, jak je uvedeno níže:

![](scenekit-images/image12.png "Objekt provedené lesklý s zrcadlová reflexe, což vede k jasně pozici na povrchu")
 
Materiály jsou velmi flexibilní, což umožňuje mnohem dosáhnout velmi malé kódem. Například místo nastavení bitovou kopii do rozptýlených obsahu, nastavte ji na odrážející obsah místo.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Opic se teď vizuálně nacházejí v rámci oblasti, nezávisle na hlediska.

### <a name="animation"></a>Animace

SceneKit je navržená tak, aby dobře pracoval s animace. Můžete vytvořit implicitního nebo explicitního animace a může vykreslit i scény ze stromu vrstvy základní animace. Při vytváření implicitní animace, SceneKit poskytuje svou vlastní třídu přechod `SCNTransaction`.

Tady je příklad, který otočí oblasti:

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Můžete animace mnohem víc než otočení ale. Mnoho vlastností SceneKit jsou animatable. Například následující kód animuje těmto informacím `Shininess` zvýšit zrcadlová reflexe.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit je velmi jednoduchá používat. Nabízí širokou řadu dalších funkcí, včetně omezení, fyziky, deklarativní akce, 3D text, hloubka pole podpory, integration Kit pohyblivý symbol a integrace základní bitovou kopii k několika název.