---
title: "iOS herní rozhraní API"
description: "Tento článek se zabývá novými rozšířeními herní poskytované iOS 9, která můžete použít ke zlepšení vaší Xamarin.iOS herní grafiky a zvukové funkce."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 6c735c68ee61032d8dfe3a74e6858bc058c6fa47
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="ios-gaming-apis"></a>iOS herní rozhraní API

_Tento článek se zabývá novými rozšířeními herní poskytované iOS 9, která můžete použít ke zlepšení vaší Xamarin.iOS herní grafiky a zvukové funkce._

Apple má provedli několik zlepšení technologické k herní rozhraní API v iOS 9, které usnadňují implementaci herní grafiky a zvuku v aplikaci pro Xamarin.iOS.
Mezi ně patří i usnadnění vývoje prostřednictvím základní architektury a využití možností GPU zařízení s iOS pro vylepšené rychlost a grafické dalo.

[ ![](images/flocking01.png "Příklad aplikace spuštěna vločkování")](images/flocking01.png)

To společně s novou, rozšířené funkce operačního systému, SceneKit a SpriteKit zahrnuje GameplayKit, ReplayKit, Model vstupně-výstupních operací, MetalKit a shadery výkonu operačního systému.

Tento článek vás seznámí všechny způsobů, jak zlepšit vaše hra Xamarin.iOS iOS 9 na nové herní vylepšení:

## <a name="introducing-gameplaykit"></a>Představení GameplayKit

Nové architektury GameplayKit společnosti Apple poskytuje sadu technologií, která usnadňuje vytvoření hry pro zařízení s iOS snížením opakují, běžné kódu vyžadovaného pro implementaci. GameplayKit poskytuje nástroje pro vývoj herní mechanismů, které lze poté snadno kombinovat s modul grafické (například SceneKit nebo SpriteKit) k rychlému poskytování dokončené hru.

GameplayKit obsahuje několik běžných, hra přehrávat algoritmů, například:

- Chování na základě, simulace agenta, který umožňuje definovat pohybů a cíle, kterých AI bude automaticky pokračovat.
- Minmax umělé inteligence pro hraní her na základě vypněte.
- Pravidlo systém pro datové herní logiku s fuzzy logikou reasoning zajistit vznikající chování.

Kromě toho GameplayKit trvá stavebním blokem přístup vývoj her s využitím modulární architekturu, která poskytuje následující funkce:

- Pro komplexní, procedurální kód pro zpracování stavu počítače na základě systémy v hraní her.
- Nástroje pro zajištění náhodně mění, hraní her a nepředvídatelnost aniž by to způsobilo ladění problémů.
- Opakovaně použitelný, komponentních entit na základě architektury.

Další informace o GameplayKit, najdete v tématu společnosti Apple [Průvodce programováním Gameplaykit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) a [GameplayKit Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>Příklady GameplayKit

Podívejme rychlý přehled implementace některé mechanismy jednoduché hraní her v aplikace Xamarin.iOS pomocí hraní her kit.

### <a name="pathfinding"></a>Pathfinding

Pathfinding je element AI hry schopnost najít jeho způsobem řešení herní panelu.
Například 2D enemy hledání skrze bludiště nebo 3D znak prostřednictvím geologické struktury první osoba střelec world.

Vezměte v úvahu následující mapa:

[ ![](images/gkpathfindpath.png "Příklad pathfinding mapování")](images/gkpathfindpath.png)

Pomocí pathfinding tento kód C# můžete najít způsob, jak pomocí mapy:

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>Klasického Expert systému

Následující fragment kódu jazyka C# ukazuje, jak lze pomocí GameplayKit implementovat klasického expert systém:

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

Na základě dané sady pravidel (`GKRule`) a se známou sadou vstupy, systém expert (`GKRuleSystem`) vytvoří předvídatelný výstup (`fizzbuzz` pro náš příklad výše).

### <a name="flocking"></a>Vločkování

Skupina AI řídí herní entity se bude chovat, jako hejna, kde skupině odpoví na pohybů a akce realizace entity jako hejna kusů na cestě nebo škole rybího plaveckých vločkování umožňuje.

Následující fragment kódu jazyka C# implementuje vločkování chování pomocí GameplayKit a SpriteKit pro grafické zobrazení:

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

V dalším kroku Implementujte toto scény v kontroleru zobrazení:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

Při spuštění, trochu animovaný _"Boids"_ bude hejno kolem naše odposlouchávání prstu:

[ ![](images/flocking01.png "Trochu animovaný Boids bude hejno kolem odposlouchávání prstu")](images/flocking01.png)

### <a name="other-apple-examples"></a>Další příklady Apple

Kromě ukázky popsané výše Apple poskytl následující ukázkových aplikací, které může být převodu jazyka C# a Xamarin.iOS:

- [FourInARow: Použití Strategický plánovač GameplayKit Minmax pro soupeř AI](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: Při použití v GameplayKit systém agentů](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: Vytváření křížové platformy hry s SpriteKit a GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

V systému iOS 9 udělal Apple několik změnami a dodatky do operačního systému k poskytování přístupu nízké nároky na grafický procesor. Použití operačního systému můžete zvýšit grafika a výpočetní potenciál aplikace pro iOS.

Rozhraní systému zahrnuje následující nové funkce:

- Nový privátní a hloubku vzorníku textury pro OS X.
- Vylepšené stínové kvality pomocí hloubka montážní a samostatné front-end a back vzorníku hodnoty.
- Vylepšení systému stínování jazyk a standardní knihovny operačního systému.
- Výpočetní shadery podporovat širší škálu formátů pixelů.

### <a name="the-metalkit-framework"></a>Rozhraní MetalKit

Rozhraní framework MetalKit poskytuje sadu nástrojů třídy a funkce, která snižují množství práce potřebné k použití operačního systému v aplikaci pro iOS. MetalKit podporuje tři klíčové oblasti:

1. Načítání z různých zdrojů včetně běžné formáty například PNG, JPEG, KTX a PVR asynchronní texture.
2. Snadný přístup k modelu vstupně-výstupních operací na základě prostředků pro zpracování systému konkrétní modelu. Tyto funkce jsou vysoce optimalizované zajistit přenosu efektivní dat mezi oka modelu vstupně-výstupních operací a systému vyrovnávací paměti.
3. Předdefinovaných zobrazení systému a zobrazení správy, který výrazně snížit objem kódu, který zobrazí grafické omítky v aplikaci pro iOS.

Další informace o MetalKit, najdete v tématu společnosti Apple [MetalKit Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [Průvodce programováním v systému](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [systému Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) a [operačního systému Stínování příručka jazyka](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Výkon systému shadery Framework

Rozhraní framework shaderu výkonu operačního systému poskytuje vysoce optimalizovaný sadu grafiky a výpočetní na základě shadery pro použití ve vašem systému na základě aplikací pro iOS. Každý shaderu v shaderu výkonu operačního systému, framework byla konkrétně přizpůsobená zajistit vysoký výkon v systému podporováno iOS grafickými procesory.

Pomocí třídy shaderu výkonu operačního systému, můžete dosáhnout nejvyšší možný na každou konkrétní iOS GPU výkon bez nutnosti cíle a udržovat základů jednotlivých kódu. Úplnému shadery výkonu lze použít s jakýmikoli prostředky systému například textury a vyrovnávací paměti.

Rozhraní framework shaderu výkonu operačního systému poskytuje sadu běžných shadery jako:

- **Gaussovské rozostření** (`MPSImageGaussianBlur`)
- **Detekce okraje Sobel** (`MPSImageSobel`)
- **Obrázek Histogram** (`MPSImageHistogram`)

Další informace najdete v tématu společnosti Apple [příručka jazyka stínování systému](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Úvod do modelu vstupně-výstupních operací

Společnosti Apple modelu vstupně-výstupních operací framework poskytuje hluboké znalosti 3D prostředky (například modely a jejich související prostředky). Vstupně-výstupních operací modelu poskytuje svým hrám iOS se na základě fyzické materiály, modely a osvětlení, který lze použít s GameplayKit, operačního systému a SceneKit.

Pomocí modelu vstupně-výstupních operací může podporovat následující typy úloh:

- Importovat osvětlení, materiály, OK sítě dat, fotoaparát nastavení a další informace na základě scény z různých oblíbených softwaru a formáty herní stroje.
- Zpracování nebo vygenerovat informace scény, například jako vytvořit procedurálně texturou sky komor nebo cukrárna osvětlení do mřížky.
- Funguje s MetalKit, SceneKit a GLKit efektivně načtení herní prostředky do vyrovnávací paměti grafického procesoru pro vykreslování.
- Exportujte informace na základě scény celou řadu oblíbených softwaru a formáty herní stroje.

Další informace o modelu vstupně-výstupních operací, najdete v tématu společnosti Apple [modelu vstupně-výstupních operací Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>Představení ReplayKit

Nové architektury ReplayKit společnosti Apple umožňuje snadno přidat záznam hraní her pro vaše hra iOS a povolit uživatelům rychle a snadno upravit a sdílet toto video z aplikace.

Další informace najdete v tématu společnosti Apple [budete sociálních s ReplayKit a herní Centrum video](https://developer.apple.com/videos/wwdc/2015/?id=605) a jejich [DemoBots: vytváření křížové platformy hry s SpriteKit a GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) ukázkovou aplikaci.

## <a name="scenekit"></a>SceneKit

Scény Kit je rozhraní API, které usnadňuje práci s 3D grafický graf 3D scény. Bylo poprvé dostupné ve OS X 10.8 a teď zase na iOS 8. S scény Kit vytváření dokonalé 3D vizualizace a běžné 3D hry nevyžaduje odborných znalostí v OpenGL. Sestavování na běžné koncepty grafu scény, scény Kit abstrahuje rychle složitosti OpenGL a ES OpenGL, což velmi usnadňuje přidat 3D obsahu k aplikaci. Pokud jste odborník OpenGL, má scény Kit však podpory pro příkazů přímo s OpenGL také. Také zahrnuje množství funkcí, které doplňují 3D grafiky, jako je například fyziky a velmi dobře se integruje se službou několik dalších rozhraní Apple, například základní animace, základní Image a pohyblivý symbol Kit.

Další informace najdete v tématu naše [SceneKit](~/ios/platform/gaming/scenekit.md) dokumentaci.

### <a name="scenekit-changes"></a>SceneKit změny

Apple přidala SceneKit následující nové funkce pro iOS 9:

- Xcode teď poskytuje scény Editor, který umožňuje rychle vytvářet hry a interaktivní aplikace pro 3D scény přímo z v rámci Xcodu úpravou.
- `SCNView` a `SCNSceneRenderer` třídy lze použít pro povolení systému zpracování (na zařízeních s iOS podporované).
- `SCNAudioPlayer` a `SCNNode` třídy lze použít k přidání prostorových zvuk účinky, které automaticky sledovat player pozice aplikace pro iOS.

Další informace najdete v tématu naše [SceneKit dokumentace](~/ios/platform/introduction-to-ios8.md#scenekit) a společnosti Apple [SceneKit Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) a [už: vytváření SceneKit hry s Xcode scény Editor](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)ukázkový projekt.

## <a name="spritekit"></a>SpriteKit

Pohyblivý symbol Kit, 2D herní framework od společnosti Apple, obsahuje některé zajímavé nové funkce v iOS 8 a OS X Yosemite. Mezi ně patří integrace s Kit scény, podporu shaderu, osvětlení, stínů, omezení, normální mapa generování a fyziky vylepšení. Konkrétně nové funkce fyziky usnadňují velmi přidat realistické efekty do hru.

Další informace najdete v tématu naše [SpriteKit](~/ios/platform/gaming/spritekit.md) dokumentaci.

### <a name="spritekit-changes"></a>SpriteKit změny

Apple přidala SpriteKit následující nové funkce pro iOS 9:

- Automaticky sledovat zobrazujícím pozicí prostorových zvuk vliv `SKAudioNode` třídy.
- Xcode teď nabízí Editor scény a Editor akce pro snadné 2D vytváření hry a aplikace.
- Easy posouvání herní podporu pro nové uzly fotoaparát (`SKCameraNode`) objekty.
- Na zařízeních s iOS, které podporují operačního systému SpriteKit, budou automaticky používat vykreslování, i když už jste používali vlastní shadery OpenGL ES.

Další informace najdete v tématu naše [SpriteKit dokumentace](~/ios/platform/introduction-to-ios8.md#spritekit) společnosti Apple [SpriteKit Framework – referenční informace](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) a jejich [DemoBots: vytváření křížové platformy hru s SpriteKit a GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) ukázkovou aplikaci.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých nové funkce herní této iOS 9 poskytuje pro aplikace pro Xamarin.iOS.
Je zavedená GameplayKit a vstupně-výstupních operací modelu; hlavní vylepšení do operačního systému; a nové funkce SceneKit a SpriteKit.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
