---
title: Pomocí UrhoSharp
description: Přehled UrhoSharp stroje
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 8eed81817620b3f68510ab2e043c3aeaafb6e78a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="using-urhosharp"></a>Pomocí UrhoSharp

_Přehled UrhoSharp stroje_

Než napíšete první hru, kterou chcete získat familiarized se základy: jak nastavit vaše scény, jak načíst prostředky (obsahuje kresby) a jak vytvořit jednoduché interakce pro vaše hra.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>Děje, uzlů, komponent a kamery

Model scény lze popsat jako na základě součástí scény grafu. Scény se skládá z hierarchie scény uzly od kořenového uzlu, který také představuje celou scény. Každý [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) má 3D transformace (pozice, otáčení a škálování), název, ID a libovolný počet součástí.  Součásti Oživte uzel, udělat přidat vizuální reprezentace ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), se může vydávat zvuk ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), mohou poskytnout hranici kolizí a tak dále.

Můžete vytvořit scény a instalační program uzlů pomocí [Urho Editor](#UrhoEditor), nebo může provádět akce v kódu jazyka C#.  V tomto dokumentu se podíváme na nastavení věcí si pomocí kódu, protože jejich ilustrují prvků potřebných k získání věcí objeví na obrazovce

Kromě nastavení vaší scény, budete muset instalační program [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), je to, co určuje, co bude získat zobrazí uživateli.

### <a name="setting-up-your-scene"></a>Nastavení vaší scény

Tento formulář byste obvykle vytvořili způsob spuštění:

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

### <a name="components"></a>Součásti

Vykreslování 3D objekty, přehrávání zvuku, fyziky a skriptované logiku aktualizace jsou všechny povolené vytvořením různých komponent na uzly voláním [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Instalační program například uzel a světla součást takto:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Jsme vytvořili výše uzel s názvem "`DirectionalLight`" a nastavte směr ji, ale nic jiného.  Nyní jsme můžete zapnout uzlu výše do výstupu světla uzlu, připojením [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) součásti, s `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Komponenty vytvořené do `Scene` mít sama speciální role: implementace scény celou funkce. By měl být vytvořen před všemi ostatními komponentami a zahrnují následující:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): implementuje prostorových vytváření oddílů a accelerated viditelnost dotazy. Bez této 3D nelze vykreslit objekty.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): implementuje fyziky simulace. Fyzice součásti, jako [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) nebo [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) bez nemůže správně fungovat.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): implementuje ladění geometrie vykreslování.

Obyčejnou komponenty, jako [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) nebo [ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel) by neměly být vytvářeny přímo do [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), ale místo do podřízených uzlů.

Knihovny se dodává s celou řadu součástí, které můžete provést připojení k uzly k Oživte je: viditelné pro uživatele prvky (modelů), zvuky, pevné subjekty, kolizí tvarů, kamery, zdroje světla, částice vysílače a mnohé další.

### <a name="shapes"></a>Obrazce

V zájmu usnadnění jsou k dispozici jako jednoduchý uzly v oboru názvů Urho.Shapes různých tvarů.  Ty zahrnují polí, oblasti, hlávky, válců a roviny.

### <a name="camera-and-viewport"></a>Fotoaparát a zobrazení

Stejně jako světlým, kamery jsou součásti, takže bude potřeba pro připojení k uzlu komponentu, to se provádí podobné výjimky:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

S tím, jste vytvořili fotoaparátu a jste umístili fotoaparát 3D světě, dalším krokem je informovat o tom, `Application` je fotoaparát, který chcete použít, to se provádí následujícím kódem:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

A teď musí být možné zobrazit výsledky vašeho vytvoření.

### <a name="identification-and-scene-hierarchy"></a>Identifikace a scény hierarchie

Na rozdíl od uzly součásti nemají názvů; součásti ve stejném uzlu jsou identifikovány pouze podle jejich typu a index v seznamu součástí uzlu, který je vyplněno vytvoření pořadí, například můžete načíst [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) součásti z `lightNode` objektu vyšší podobné výjimky:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Můžete také získat seznam všech součástí načtením [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) vlastnost, která vrátí hodnotu `IList<Component>` , kterou můžete použít.

Při vytváření uzlů a komponent získat ID globální scény celé číslo. Se může být dotazován z scény pomocí funkce [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) a [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Toto je mnohem rychlejší než například provádění rekurzivní dotazy na základě názvu scény uzlu.

Neexistuje žádná koncepce předdefinované entitu nebo herní objektu; je spíše až programátorů rozhodnout uzlu hierarchie a které uzly umístit veškeré skriptované logiky. Bez přesouvání objekty 3D světě by obvykle vytvořit jako podřízené objekty kořenového uzlu. Uzly lze vytvořit s nebo bez názvu pomocí [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Jedinečnost názvů uzlu se nevynucuje.

Vždy, když se některé hierarchické složení, je doporučené (a je ve skutečnosti nezbytné, protože komponenty nemají své vlastní 3D transformací) k vytvoření podřízený uzel.

Například pokud znak byl podržíte objekt v jeho ručička, objektu by mělo mít svůj vlastní uzlu, který by nadřazena k ruční kost je znak (také [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Jedinou výjimkou je fyziky [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), což může být offsetted a otočený jednotlivě ve vztahu k uzlu.

Všimněte si, že [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)na vlastní transformace záměrně ignorováno jako optimalizace při výpočtu transformací world odvozené z podřízených uzlů, takže ho změna nemá vliv a by mělo být ponecháno, protože je (pozici v počátek, bez otočení bez měřítka.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) uzly můžete volně nadřazeny. Na rozdíl od součásti vždy patřit do uzlu se připojit k a nelze jej přesunout mezi uzly. Zadejte uzlů a komponent [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) funkce toho chcete dosáhnout, aniž by museli projít nadřazený. Jakmile bude uzel odebrán, žádné operace na uzel nebo součást dotyčném jsou bezpečné po volání této funkce.

Je také možné vytvořit `Node` , nepatří do scény. To je užitečné, například s fotoaparátu přesunutí v scény, který může načíst nebo uložit, protože pak kamera se neuloží společně s skutečné scény a nebude být zničený, když je načten scény. Všimněte si však vytváření geometry, fyziky nebo skript součásti odpojit uzel a pak ho později přesouvání do scény způsobí těchto součástí nebude fungovat správně.

### <a name="scene-updates"></a>Aktualizace scény

Scény jehož aktualizace jsou povolené (výchozí), bude automaticky aktualizovat na každé iteraci smyčky hlavní.  Aplikace [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) ho je volána obslužná rutina události.

Uzly a součásti mohou být vyloučeny z aktualizace scény zakázáním je najdete v tématu [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Chování závisí na konkrétní součást, ale například zakázání komponentu drawable také umožňuje neviditelná, při zakázání zvukové zdrojovou součástí ztlumí. Pokud uzel je zakázaná, všech jeho součástí jsou považovány jako zakázané bez ohledu na jejich vlastní stav povolit nebo zakázat.

## <a name="adding-behavior-to-your-components"></a>Přidání chování u součástí

Nejlepší způsob, jak struktury vaše hra je provádět vlastní komponenty, která zapouzdřovat objektu actor nebo element na příslušnou hru.  Díky funkci vlastní obsažené z prostředků slouží k zobrazení, její chování.

Nejjednodušší způsob přidání chování pro komponentu je pomocí akcí, které jsou pokyny, můžete ve frontě a kombinovat s C# – programování asynchronních.  To umožňuje chování pro příslušné součásti být velmi vysoké úrovni a je jednodušší zjistit, co se děje.

Alternativně můžete řídit, přesně co se stane příslušné součásti aktualizací vaší vlastnosti součásti na každý snímek (popsané v části chování na základě rámečkem).

### <a name="actions"></a>Akce

Chování můžete přidat do uzlů velmi snadno pomocí akce.  Akce můžete změnit různé vlastnosti uzlu a je provedena v časovém intervalu ani je opakovat několikrát s danou animace křivky.

Představte si třeba uzlem "cloud" na vaše scény, můžete ho objevovat takto:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Akce jsou neměnné objekty, které vám umožní znova využít akce pro řízení různé objekty.

Běžné stylu se vytvoří akci, která provede operaci reverzní:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

V následujícím příkladu by pro vás vykreslit objekt po dobu tří sekund.  Můžete taky spustit jednu akci po druhém, například může nejprve přesunout cloudu a poté jej skrýt:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Pokud chcete, aby obě akce proběhla ve stejnou dobu, můžete použít paralelní akce a zadejte všechny akce, které chcete provést paralelní:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

V předchozím příkladu cloudu přesune a vykreslit ve stejnou dobu.

Si všimnete, že tyto pomocí jazyka C# await, která umožňuje lineárně myslíte o chování, které chcete dosáhnout.

### <a name="basic-actions"></a>Základní operace

Toto jsou podporované v UrhoSharp akce:

* Přesunutí uzly: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Otáčení uzly: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Škálování uzly: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Pozvolného vysouvání uzly: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Barevný nádech: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* Okamžiky: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Ve smyčce: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Další funkce zahrnují kombinaci [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) a [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) akce.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Usnadnění – řízení rychlosti vaše akce

Usnadnění je tak, aby přesměruje způsobu, jakým bude unfold animace a mohl zajistit své animace mnohem víc příjemný.  Ve výchozím nastavení vaše akce bude mít lineární chování, například [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) akce by měla mít velmi automatické pohyb.  Vaše akce do náběh a doběh akci, kterou chcete změnit chování, například ten, který by pomalu spuštění pohyb, zvýšení a pomalu dosažení poslední může obtékat ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

To provedete zabalení existující akci do nejvýraznější akce, například:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Existuje mnoho nejvýraznější režimy, následující tabulka ukazuje různé typy nejvýraznější a jejich chování na základě hodnoty objektu, který se jsou řízení přes časové období od začátku do konce:

![Usnadnění režimy](using-images/easing.png "tento graf znázorňuje různé typy nejvýraznější a jejich chování na základě hodnoty objektu jsou řízení v časovém období")

### <a name="using-actions-and-async-code"></a>Pomocí akce a asynchronních kódu

Ve vašem [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) podtřídami, je vhodné zavést použití asynchronní metody, který připraví chování vaší součásti a řídí funkce pro ni.
Pak by volat tuto metodu, pomocí jazyka C# `await` – klíčové slovo z jiné části vašeho programu buď vaše `Application.Start` metoda nebo v reakci na bod uživatele nebo scénáře v aplikaci.

Příklad:

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

V `Launch` jsou spuštěna metoda výše tři akce: robota stává scény, tato akce změní umístění uzlu za období 0,6 sekund.  Vzhledem k tomu, že se jedná o možnost asynchronní, dojde k ní souběžně jako další pokyny, které jsou volání do `MoveRandomly`.  Tato metoda změní pozici robota paralelně s náhodném umístění.  Toho dosáhnete tak, že provedete dvě složené akce Přesun do nového umístění a po návratu do původní pozice a tento postup opakujte, dokud robota zůstane aktivní.  A aby věcí zajímavějšího, bude udržovat robota čistá současně.  Čistá pouze spustí každých 0,1 sekund.

### <a name="frame-based-behavior-programming"></a>Na základě rámce chování programování

Pokud chcete řídit chování příslušné součásti na základě jednotlivých snímcích místo použití akce, co by se má přepsat [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) metodu vaše [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) podtřídy.  Tato metoda je volána jednou za snímek a je volána, pouze pokud nastavíte vlastnost ReceiveSceneUpdates na hodnotu true.

Následující ukazuje, jak vytvořit `Rotator` součást, pak připojený k uzlu, což způsobí, že uzel otočit:

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

A je to, jak by tato součást připojení k uzlu:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Kombinování styly

Můžete použít asynchronní akce nebo na základě modelu pro programování většinu chování, což je výhodné pro ještě efektivněji a zapomněli styl programování, ale můžete také podrobně upravit chování příslušné součásti spustit také některé kód aktualizace na každý snímek.

Například v ukázkové SamplyGame slouží v `Enemy` třída kóduje akce, které používá základní chování, ale také zajišťuje, aby součásti bodu směrem k uživatele nastavením směr uzlu se `Node.LookAt`:

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

## <a name="loading-and-saving-scenes"></a>Načítání a ukládání scény

Scény můžete načíst a uložit ve formátu XML; najdete v části funkce [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) a [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Při načítání scény je nejdřív odstranit všechny existující obsah (podřízené uzly a součásti). Uzly a součásti, které jsou označené dočasné s `Temporary` vlastnosti se neuloží. Serializátor zpracovává všechny integrované komponenty a vlastnosti, ale není dostatečně inteligentní zpracování vlastní vlastnosti a pole definovaná v podtřídách vaší součásti. Ale nabízí dvě virtuální metody pro toto:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) kde je můžete zaregistrovat můžete vlastní stavy pro serializaci

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) kde lze získat vaše uložené vlastní stavy.

Obvykle vlastní součást bude vypadat takto:

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

### <a name="object-prefabs"></a>Objekt Prefabs

Právě načítání nebo ukládání celou scény není dostatečně flexibilní pro hry kdy je potřeba dynamicky vytvořit nové objekty. Na druhé straně vytváření složitých objektů a nastavování jejich vlastností v kódu bude také zdlouhavé. Z tohoto důvodu je také možné uložit scény uzel, který bude obsahovat jeho podřízené uzly, součásti a atributy. Ty lze načíst později pohodlně jako skupina.  Uložený objekt se často označuje jako prefab. Existují tři způsoby, jak to udělat:

- V kódu voláním [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) na uzlu
- V editoru výběrem uzlu v okně hierarchie a zvolením "uložit uzlu jako" z nabídky "Soubor".
- Pomocí příkazu "uzel" v `AssetImporter`, který bude ukládat scény uzlu hierarchie a žádné modely obsaženy v assetu vstupní (např. soubor standardu Collada)

Chcete-li vytvořit instanci uzlu uložené do scény, volejte [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Uzel se vytvoří jako podřízené scény, ale můžete volně nadřazeny potom. Pozice a otočení pro umístění uzlu musí být zadán. Následující kód ukazuje, jak vytvořit instanci prefab `Ninja.xm` k scény s požadovanou pozice a oběh:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Události

UrhoObjects zvýšit počet událostí, ty jsou prezentované jako událostí jazyka C# na různé třídy, které je generovat.  Kromě jazyka C# – model na základě událostí je také možné použít `SubscribeToXXX` metody, které vám umožní přihlášení k odběru a udržovat předplatné token, který můžete později použít k odhlášení odběru.  Rozdílem je, že první bude mnoho volajícím povolit přihlášení k odběru, druhý pouze jeden umožňuje, ale umožňuje nicer lambda stylu přístupu má být použit a ještě umožňují snadno odebrání předplatného.  Budou se vzájemně vylučují.

Když se přihlásíte k události, je nutné zadat metodu, která používá argument s argumenty příslušné události.

Například to je, jak se přihlásíte k odběru tlačítko myši událost vypnutí:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

Se stylem lambda:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Někdy budete chtít zastavit přijímání oznámení pro události, v těchto případech uložit vrácená hodnota z volání `SubscribeTo` metoda a vyvolá metodu Unsubscribe:

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

Parametr přijatých obslužné rutiny události je třída silného typu události argumenty, která bude specifické pro každou jednotlivou událost a obsahuje datová část události.

## <a name="responding-to-user-input"></a>Reagovat na vstup uživatele

Přihlášení k odběru události a reagovat na vstup doručován lze přihlásit různé události, jako je stisknutí kláves dolů:

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

Ale v mnoha případech budete chtít vaší obslužné rutiny aktualizace scény zkontrolovat aktuální stav klíčů při jejich aktualizované a aktualizujte kód odpovídajícím způsobem.  Například následující lze použít k aktualizaci fotoaparát umístění na vstup z klávesnice na základě:

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>Prostředky (prostředky)

Zdroje informací v UrhoSharp většinu věcí, které jsou načteny z velkokapacitních paměťových zařízení během inicializace nebo modul runtime:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -použít pro kosterního animace
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -představuje ukládány v různých grafické formátů obrázků
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D modelů
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -použitých k vykreslení modely materiálů.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [Popisuje](http://urho3d.github.io/documentation/1.4/_particles.html) jak částice vysílače funguje, najdete v části "[částice](#particles)" níže.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -vlastní shadery
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -zvuků k přehrávání, najdete v části "[zvuk](#sound)" níže.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -techniky podstatným vykreslování
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2D textury
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D textury
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) – Texture datové krychle
- `XmlFile`

Jsou spravovaná a načíst pomocí [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) subsystému (k dispozici jako [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Prostředky sami jsou identifikovány jejich cesty k souborům, relativně k registrované prostředků adresáře nebo soubory balíčku. Ve výchozím nastavení, zaregistruje modul adresáře prostředků `Data` a `CoreData`, nebo balíčky `Data.pak` a `CoreData.pak` Pokud existují.

Pokud načítání prostředku nezdaří, bude zaznamenána chyba a je vrácen odkaz s hodnotou null.

Následující příklad ukazuje typické způsob načítání prostředku z mezipaměti prostředků.  V takovém případě texture pro element uživatelského rozhraní, se používá `ResourceCache` vlastnost z `Application` třídy.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Prostředky můžete také ručně vytvořit a uložit do mezipaměti prostředků, jako by se měl načtená z disku.

Paměť rozpočty lze nastavit pro typ prostředku: Pokud prostředků vyžaduje více paměti, než je povoleno, nejstarší prostředky se odeberou z mezipaměti v opačném případě používán už. Ve výchozím nastavení rozpočty paměti jsou nastaveny na neomezený.

### <a name="bringing-3d-models-and-images"></a>Přináší 3D modely a obrázků

Urho3D pokusí použít existující formáty souborů, kdykoli je to možné a definujte vlastních formátů souborů pouze v případě, že je nezbytně nutné, jako pro modely (*.mdl) a pro animace (*.ani). Pro tyto typy prostředků Urho poskytuje převaděč - [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) které můžou využívat mnoho oblíbených formátů 3D například fbx, dae, 3ds a obj atd.

Existuje také užitečné add-in pro digestoru [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) , můžete exportovat vaše prostředky digestoru ve formátu, který je vhodný pro Urho3D.

### <a name="background-loading-of-resources"></a>Načítání na pozadí prostředků

Za normálních okolností žádosti o prostředky pomocí jedné z `ResourceCache`na `Get` metoda, jsou načteny hned v hlavní vlákno, které může trvat několik milisekund pro všechny požadované kroky (načíst soubor z disku, analyzovat data, odešlete do GPU v případě potřeby ) a proto může způsobit vyřazuje kmitočet snímků.

Pokud znáte předem jaké prostředky musíte, můžete požádat, je třeba načíst ve vláknu na pozadí při volání `BackgroundLoadResource()`. Můžete přihlásíte k odběru událostí prostředků pozadí načíst pomocí `SubscribeToResourceBackgroundLoaded` metoda. Zobrazí se informace, pokud načítání ve skutečnosti se úspěch nebo selhání. V závislosti na prostředek, pouze část procesu načítání mohou být přesunuty do vlákna na pozadí, třeba nepřipojujte nahrávání krok GPU vždy musí provést v hlavní vlákno. Všimněte si, že pokud je volání jednoho prostředku načítání metody pro prostředek, který je ve frontě pro pozadí načítání, hlavní vlákno bude stalace až do dokončení jeho načítání.

Asynchronní scény načítání funkcí `LoadAsync()` a `LoadAsyncXML()` má možnost pozadí zatížení než přistoupíte k načtení obsahu scény prostředky. Lze také pouze načíst prostředky beze změny scény, zadáním `LoadMode.ResourcesOnly`. To umožňuje připravit scény nebo objekt prefab soubor pro rychlé vytváření instancí.

Nakonec maximální dobu (v milisekundách) strávený každý snímek na dokončení pozadí načíst prostředky se dá nakonfigurovat nastavení `FinishBackgroundResourcesMs` vlastnost `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Zvuk

Zvuk, který je důležitou součástí hraní her a rozhraní UrhoSharp poskytuje způsob přehrávání zvuku ve hře.  Přehrávání zvuků připojením [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/) součásti na [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) a pak přehrávání soubor s názvem z vašich prostředků.

Toto je, jak se provádí:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Částice

Částice poskytují jednoduchý způsob přidání některé jednoduché a nenákladné efekty do vaší aplikace.  Můžete využívat částice uložený ve formátu nástroje PEX pomocí nástroje, například [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Částice jsou komponenty, které mohou být přidány do uzlu.  Je třeba volat uzlu `CreateComponent<ParticleEmitter2D>` metoda vytvoření částice a pak nastavte částice pomocí nastavení vlastnosti vliv na 2D vliv, který je načten z mezipaměti prostředků.

Například můžete vyvolat tuto metodu na příslušné součásti zobrazíte některé částice, které jsou generovány jako prudký nárůst počtu při volání:

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

Výše uvedený kód vytvoří do uzlu prudký nárůst počtu, který je připojen k příslušné aktuální součásti, v tomto uzlu prudký nárůst počtu jsme 2D částice vysílače vytvořit a nakonfigurovat ji nastavením vlastnosti vliv.  Spustíme dvě akce, jednu, která je škálovatelná uzlu, který má být menší a ten, který zůstává při této velikosti 0,5 sekund.  Potom jsme odebrat prudký nárůst počtu, tím se také odstraní částice vliv na obrazovce.

Výše uvedené částice vykreslí následujícím způsobem, při použití textury oblasti:

![Částice texturou oblasti](using-images/image-1.png "výše částice vykreslí takto při použití textury oblasti")

A je to, co vypadá, pokud používáte zablokovaného texture:

![Částice texturou pole](using-images/image-2.png "a to v souboru se zobrazí při použití zablokovaného textury")

## <a name="multithreading-support"></a>Podpora více vláken

UrhoSharp je jediná zařazování knihovna.  To znamená, že byste neměli k vyvolání metody v UrhoSharp z vlákna na pozadí, nebo riziko poškozování stavu aplikace a pravděpodobně k chybě aplikace.

Pokud chcete spouštět nějaký kód na pozadí a aktualizujte Urho součásti na hlavní uživatelské rozhraní, můžete použít [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain) metoda.  Kromě toho můžete pomocí jazyka C# await a .NET úloh zkontrolujte, zda je na správné vlákno kód rozhraní API.

## <a name="urhoeditor"></a>UrhoEditor

Urho Editor si můžete stáhnout pro vaši platformu z [Urho webu](http://urho3d.github.io/), přejděte na soubory ke stažení a vyberte nejnovější verzi.

## <a name="copyrights"></a>Autorská práva

Tato dokumentace obsahuje původní obsah z Xamarin Inc, ale nevykresluje hojně v dokumentaci k s otevřeným zdrojem pro projekt Urho3D a obsahuje snímky obrazovky z projektu Cocos2D.

## <a name="related-links"></a>Související odkazy

- [Sešit planetu země](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Zkoumání souřadnice sešitu](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
