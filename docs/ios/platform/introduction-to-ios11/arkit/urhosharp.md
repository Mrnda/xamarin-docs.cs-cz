---
title: Pomocí ARKit UrhoSharp
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 95c9c602d0bfe1b77fda453a137dfdfc12a975c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="using-arkit-with-urhosharp"></a>Pomocí ARKit UrhoSharp

Se zavedením [ARKit](https://developer.apple.com/arkit/), Apple má provedené u vývojářům vytvářet aplikace aktualizovaná skutečnosti. ARKit můžete sledovat přesnou pozici vašeho zařízení a zjištění různých ploch na světě a je pak až developer a přizpůsobte dat pocházejících mimo ARKit do vašeho kódu.

[UrhoSharp](~/graphics-games/urhosharp/index.md) poskytuje komplexní a snadno použitelný 3D rozhraní API, můžete použít k vytvoření 3D aplikací.   Obě tyto může být společně smíšení ARKit zajistit fyzické informace o na světě a Urho k vykreslení výsledky.

Tato stránka vysvětluje, jak připojit tyto dvě světů dohromady a vytvoří skvělé rozšířená skutečnosti aplikace.


## <a name="the-basics"></a>Základní informace

Co chcete udělat je přítomen 3D obsah na celém světě, registrovaného aplikací pro iPhone.   Cílem je, a přizpůsobte obsah pocházejících z fotoaparátu telefonu s 3D obsahem a jako uživatel telefonu přesune kolem místnosti zajistit, aby objekt 3D tak, jak je jejich součástí této místnosti – k tomu je potřeba ukotvení objekty do tohoto world.

![Animovaný obrázek v ARKit](urhosharp-images/image1.gif)


Jsme pomocí knihovny Urho načtení naše 3D prostředků a umístit je na světě a použijeme ARKit získat datový proud videa z fotoaparátu, jakož i umístění telefonu na světě.   Jako uživatel přesune s jeho telefon, budeme používat změny v umístění aktualizace souřadnicový systém, který je modul Urho zobrazení.

Tímto způsobem, když umístíte objekt v 3D prostoru a uživatel přesune, umístění 3D objektu odráží místo a umístění, kde je umístěn.

## <a name="setting-up-your-application"></a>Nastavení aplikace

### <a name="ios-application-launch"></a>Spuštění aplikace iOS

Aplikace iOS musí vytvořit a spustit 3D obsah, můžete provést vytvořením implementace podtřídou třídy [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) a zadejte instalační kód přepsáním `Start` metoda.  Toto je, kde získá vaše scény naplněný daty, událost obslužné rutiny jsou nastavení a tak dále.

Zavedli jsme `Urho.ArkitApp` třídy, která je podtřídou `Urho.Application` a na jeho `Start` metoda nemá lifting náročné.   Stačí udělat vaší existující Urho aplikace je změňte základní třídu být typu `Urho.ArkitApp` a máte aplikaci, která se spustí vaše urho scény na světě.

### <a name="the-arkitapp-class"></a>ArkitApp – třída

Tato třída poskytuje sadu vhodnou výchozí hodnoty, jak scény s některé objekty klíče a zpracování událostí ARKit dodaným v operačním systému.

Instalace probíhá `Start` virtuální metoda.   Pokud tuto metodu přepíšete na vaše podtřída, budete muset zkontrolujte, zda řetězec nadřazenému pomocí `base.Start()` na vlastní implementaci.

`Start` Metoda nastaví scény, zobrazení, fotoaparát a směrové a poskytuje tyto veřejné vlastnosti:

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) pro uložení vašich objektů
- směru [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) s stíny a jejichž umístění je k dispozici prostřednictvím `LightNode` vlastnost
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) jejichž součástí jsou aktualizovány při ARKit přináší aktualizaci aplikace a
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) zobrazení výsledků.


### <a name="your-code"></a>Váš kód

Pak musíte podtřídou `ArkitApp` třídy a přepsat `Start` metoda.   První věc, kterou metodu měli udělat je řetězec až `ArkitApp.Start` voláním `base.Start()`.  Potom můžete žádné z nastavení vlastnosti podle ArkitApp Pokud chcete přidat objekty do scény, přizpůsobit indikátory, stínů nebo události, které chcete zpracovat.

Ukázka ARKit/UrhoSharp načte znakem animovaný s textury a hraje animace za následující implementaci:

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

A který je opravdu že všechno, co musíte udělat v tuto chvíli 3D obsahu serveru zobrazí ve skutečnosti aktualizovaná.

Urho používá vlastní formáty pro 3D modely a animací, proto je třeba exportovat vaše prostředky do tohoto formátu.   Můžete použít nástroje, například [Urho3D digestoru Add-in](https://github.com/reattiva/Urho3D-Blender) a [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , můžete převést tyto prostředky z oblíbených formátů DBX, DAE, OBJ, Blend, vyžadují Urho 3D-Max do formátu.

Další informace o vytváření 3D aplikací s použitím Urho, najdete [Úvod do UrhoSharp](~/graphics-games/urhosharp/introduction.md) průvodce.

## <a name="arkitapp-in-depth"></a>ArkitApp podrobněji

> [!NOTE]
> Tato část je určena pro vývojáře, které chcete přizpůsobit výchozí možností UrhoSharp a ARKit nebo chcete získat podrobnější přehled o tom, jak funguje integrace.   Není nutné ke čtení v této části.

Rozhraní API ARKit je poměrně jednoduché, můžete vytvořit a nakonfigurovat [ARSession](https://developer.apple.com/documentation/arkit/arsession) objektu, který pak spusťte doručování [ARFrame](https://developer.apple.com/documentation/arkit/arframe) objekty.   Ty obsahují image zachycenou kamera a také odhadované pozice skutečné zařízení.

Jsme bude skládání bitové kopie se doručil fotoaparát nám s náš 3D obsah a upravte kamera v UrhoSharp tak, aby odpovídaly šance v umístění zařízení a místo.

Následující diagram znázorňuje, co probíhá `ArkitApp` třídy:

[![Diagram třídy a obrazovky v ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Vykreslování rámce

Cílem je jednoduché, kombinací video vycházejících z fotoaparátu s naše 3D grafiky k vytvoření kombinované bitové kopie.     Jsme získávání řadu tyto zaznamenané bitové kopie v pořadí, a tento vstup jsme se kombinovat s scény Urho.

Nejjednodušší způsob, jak to provést, je k vložení [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) do hlavní [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Toto je sadu příkazů, které se provádí k vykreslení jeden snímek.  Tento příkaz se vyplní zobrazení se všechny textury, kterou jsme předat.    Jsme toto nastavení na první snímek, který je proces a skutečné definice se provádí v tý **ARRenderPath.xml** soubor, který je nyní načtena.

Ale jsme se potýkají s dva problémy k mísí tyto dvě světů:


1. V systému iOS, musí mít GPU textury řešení, která je druhou mocninou dva, ale rámců, které nám se získat z fotoaparátu nemají řešení, které jsou power dva, například: 1280 × 720.
2. Snímky jsou v kódování [YUV](https://en.wikipedia.org/wiki/YUV) formátu, která je reprezentována dvě bitové kopie - luma a sytost.

Rámce YUV mají dvě různá řešení.  bitovou kopii 1280 × 720 představující světlostí (v podstatě šedé bitová kopie) a mnohem menší 640 x 360 pro komponentu chrominance:

![Ukázka kombinování Y a UV součástí bitové kopie](urhosharp-images/image3.png)


Kreslení úplné barevnou image pomocí OpenGL ES máme zápisu malých shaderu, která přebírá z sloty texture světlostí (součást Y) a chrominance (UV roviny).  V UrhoSharp budou mít názvy - "sDiffMap" a "sNormalMap" a převést na formát RGB:

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

K vykreslení texture, na kterém není mocninou dvě řešení máme definovat Texture2D s následujícími parametry:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Snažíme se proto může vykreslit zaznamenané bitové kopie jako na pozadí a vykreslit všechny scény nad ním jako je například strach mutanta.

### <a name="adjusting-the-camera"></a>Úprava kamera

`ARFrame` Objekty obsahovat také odhadované zařízení pozici.  Jsme teď musíme přesunout herní fotoaparát ARFrame – před ARKit nebyla big pozornosti orientace zařízení sledovat (kumulativní, výšky a úhlu natočení) a vykreslování definovaného hologram nad video -, ale pokud přesunete zařízení trochu - hologramy bude soubor.

K tomu dojde, protože není schopný sledovat pohybů předdefinované senzorů, jako je například volný setrvačník, mohou pouze akcelerace.  ARKit analýzy jednotlivých funkcí rámec a extrahuje bodů ke sledování a proto je schopen poskytnout nám přesný transformace matice obsahující data o přesouvání a otočení.

Například to je, jak jsme můžete získat aktuální pozici:

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Používáme `-row.Z` protože ARKit používá pravou rukou souřadnicový systém.


### <a name="plane-detection"></a>Detekce roviny

ARKit je schopna zjistit vodorovné roviny a tato možnost vám umožňuje komunikovat s reálném světě, například mutovanými můžete umístit na skutečné tabulky nebo podlaží. Nejjednodušší způsob, jak to udělat, je použití hitTest – metoda (raycasting). Převede ji souřadnice obrazovky (0,5; 0,5 je centrem) do skutečných souřadnice (0; 0; 0 je první rámečku umístění).

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

mutovanými jsme teď můžete umístit na vodorovný prostor v závislosti na tom, kde na obrazovce zařízení, které jsme klepněte na:

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![Animovaný obrázek změna rovin jako přesune zobrazení](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Realistické osvětlení

V závislosti na osvětlení podmínkách reálného světa musí být virtuální scény světlejší nebo tmavší tak, aby lépe odpovídaly okolí. ARFrame obsahuje LightEstimate vlastnost, která jsme lze upravit okolního světla Urho děje se to jako:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>Nad rámec iOS – HoloLens

UrhoSharp [spustí pro všechny hlavní operační systémy](~/graphics-games/urhosharp/platform/index.md), takže můžete opakovaně použít váš stávající kód jinde.

HoloLens je jedním z nejvíce zajímavých platformy, které běží na.   To znamená, že můžete snadno přepínat mezi iOS a HoloLens vytvářet úžasné rozšířen skutečnosti aplikace pomocí UrhoSharp.

Můžete najít zdroj MutantDemo [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Související odkazy

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (s UrhoSharp) (ukázka)](https://github.com/EgorBo/ARKitXamarinDemo)
