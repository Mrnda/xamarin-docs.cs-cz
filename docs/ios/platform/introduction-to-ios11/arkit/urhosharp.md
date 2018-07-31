---
title: Pomocí ARKit Urhosharpu v Xamarin.iosu
description: Tento dokument popisuje, jak nastavit arkit, která aplikaci Xamarin.iOS, poté vyhledá jak vykreslením snímků jsou, jak upravit fotoaparátu/kamery, jak detekovat rovin, jak pracovat s osvětlení a další. Popisuje také Urhosharpu a psaní kódu pro HoloLens.
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2017
ms.openlocfilehash: 728082eb27684c2176feb2038b7948986ce6a694
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351688"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>Pomocí ARKit Urhosharpu v Xamarin.iosu

Se zavedením [ARKit](https://developer.apple.com/arkit/), Apple bylo jednoduché pro vývojáře umožňující vytváření aplikací v rozšířené realitě. Arkit, která umožňuje přesnou polohu vašeho zařízení a zjištění různých ploch na celém světě a je pak až po vývojáře dat pocházejících mimo arkit, která do kódu a přizpůsobte.

[Urhosharpu](~/graphics-games/urhosharp/index.md) poskytuje komplexní a snadné použití 3D rozhraní API, můžete použít k vytvoření 3D aplikací.   Obě tyto může být prolnuty společně arkit, která poskytují fyzické informace o celém světě a Urho vykreslovat výsledky.

Tato stránka vysvětluje, jak se připojit tyto dva světů dohromady a vytvoří aplikace skvělé rozšířené realitě.


## <a name="the-basics"></a>Základní informace

Co chcete udělat je k dispozici 3D obsah na celém světě, jak je vidět v Iphonu.   Cílem je obsah pocházející z fotoaparátu telefonu s 3D obsahem a přizpůsobte a jako uživatel telefonu přesune po celé místnosti zajistit, aby 3D objekt chovat dle jejich je součástí tohoto místa – to se provádí ukotvení objektů v tomto světě.

![Animovaný obrázek v ARKit](urhosharp-images/image1.gif)


Můžeme pomocí knihovny Urho načtení naše 3D prostředků a umístit je na celém světě a použijeme ARKit zobrazíte datový proud videa z fotoaparátu/kamery, jakož i umístění na telefonu v celém světě.   Jak uživatel pohybuje pomocí telefonu, použije k aktualizaci souřadnicový systém, který zobrazuje modul Urho změny v umístění.

Tímto způsobem, při umístit objekt v 3D prostoru a uživatel přesune, umístění 3D objektu odpovídá místo a umístění, ve kterém byl umístěn.

## <a name="setting-up-your-application"></a>Nastavení aplikace

### <a name="ios-application-launch"></a>Spouštění aplikace iOS

Vaše aplikace pro iOS je potřeba vytvořit a spustit 3D obsahu, můžete to provést tak, že vytvoříte implementace podtřídou třídy [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) a poskytnutí ověřovacího kódu instalační program tak, že přepíšete `Start` metody.  To je, pokud vaše Scéna získá naplněný daty, událost obslužné rutiny jsou nastavení a tak dále.

Zavedli jsme `Urho.ArkitApp` třídy, která je podtřídou `Urho.Application` a na jeho `Start` metoda provede rutinní.   Všechno, co potřebujete udělat pro vaše stávající Urho aplikace je změnit základní třídu typu `Urho.ArkitApp` a budete mít aplikaci, která se spustí vaše urho scény na světě.

### <a name="the-arkitapp-class"></a>Třída ArkitApp

Tato třída poskytuje sadu vhodné výchozí hodnoty, oba scény pomocí některé klíčové objekty, stejně jako zpracování ARKit události dodaným v operačním systému.

Instalace probíhá `Start` virtuální metody.   Když je na vaší podtřídy přepsat tuto metodu, je třeba Ujistěte se, že ke sledu k Tvému pomocí `base.Start()` na vlastní implementaci.

`Start` Metoda nastaví scény, zobrazení, kamera a směrové světlo a poskytuje informace o těch jako veřejné vlastnosti:

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) držet objekty,
- směrové [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) stíny a jehož umístění je k dispozici prostřednictvím `LightNode` vlastnost
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) jehož součástí jsou aktualizovány při arkit, která poskytuje aktualizace aplikace a
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) zobrazení výsledků.


### <a name="your-code"></a>Váš kód

Potom budete potřebovat podtřídy `ArkitApp` třídy a přepsat `Start` metody.   První věc, kterou metodu měli udělat, je řetězec až `ArkitApp.Start` voláním `base.Start()`.  Potom můžete použít žádné z nastavení vlastnosti pomocí ArkitApp Chcete-li přidat objekty do scény, přizpůsobit světla, stíny nebo události, které chcete zpracovat.

Ukázka ARKit/Urhosharpu načte animovaný znak s texturami a přehraje animace, následující implementaci:

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

A to je opravdu že všechno, budete muset udělat, v tuto chvíli 3D obsahu zobrazují v rozšířené realitě.

Urho používá vlastní formáty pro 3D modely a animace, proto je nutné exportovat vaše prostředky v tomto formátu.   Můžete používat nástroje, například [doplněk blenderu Urho3D](https://github.com/reattiva/Urho3D-Blender) a [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , který lze převést tyto prostředky z oblíbených formátů, jako jsou DBX, DAE, OBJ, programu Blend, 3D-Max do formátu vyžadované Urho.

Další informace o vytváření 3D aplikací pomocí Urho, přejděte [Úvod do Urhosharpu](~/graphics-games/urhosharp/introduction.md) průvodce.

## <a name="arkitapp-in-depth"></a>ArkitApp do hloubky

> [!NOTE]
> V této části je určený pro vývojáře, které chcete přizpůsobit výchozí možnosti Urhosharpu a ARKit nebo chcete získat podrobnější přehledy o tom, jak integrace funguje.   Není nutné v tomto tématu.

Rozhraní API arkit, která je docela jednoduché, vytvoření a konfigurace [ARSession](https://developer.apple.com/documentation/arkit/arsession) objekt, který pak začněte poskytovat [ARFrame](https://developer.apple.com/documentation/arkit/arframe) objekty.   Ty obsahují image nezachytává fotoaparátu/kamery, jakož i odhadovanou pozici skutečná zařízení.

Budeme se vytváření bitové kopie distribuována pomocí fotoaparátu/kamery nám naše 3D obsahu a upravit fotoaparátu/kamery v Urhosharpu tak, aby odpovídaly pravděpodobnost v umístění zařízení a umístění.

Následující diagram znázorňuje, co probíhá `ArkitApp` třídy:

[![Diagram tříd a obrazovky ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Vykreslování rámce

Cílem je jednoduché, kombinovat videu přicházejícím z fotoaparátu/kamery s naší 3D grafiky k vytvoření kombinované bitové kopie.     Jsme získání řadu těchto zaznamenané bitové kopie v pořadí a tento vstup jsme se kombinovat s Urho scény.

Nejjednodušší způsob, jak to udělat, je vložit [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) do hlavní [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Toto je sadu příkazů, které se provádí pro kreslení jeden snímek.  Tento příkaz vyplní zobrazení se všechny textury, který předáme do něj.    Jsme toto nastavení na první snímek, který je proces a skutečnou definici se provádí v th **ARRenderPath.xml** soubor, který je v tuto chvíli načíst.

Ale jsme se potýkají s společně a přizpůsobte tyto dvě světů dva problémy:


1. V systémech iOS, musí mít GPU textury řešení, která je mocninou čísla 2, ale počet snímků, které získáme z fotoaparátu/kamery nemají rozlišení, které jsou mocninou čísla 2, například: 1280 × 720.
2. Snímky jsou zakódovány [YUV](https://en.wikipedia.org/wiki/YUV) formátu reprezentována dvě Image - luma a sytost.

Snímky YUV se dělí na dvě různá řešení.  bitovou kopii 1280 × 720 představující světelnost (v podstatě obrázku šedé) a mnohem menší 640 × 360 chrominance komponenty:

![Ukázka kombinování Y a komponenty UV obrázek](urhosharp-images/image3.png)


Chcete-li nakreslit úplné barevné image pomocí OpenGL ES máme zápisu malých shader, který přijímá světelnost (součást Y) a chrominance (UV roviny) ze slotů textury.  V Urhosharpu mají názvy - "sDiffMap" a "sNormalMap" a je převést na formát RGB:

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

K vykreslení textury, který nemá mocninou dvou řešení budeme muset definovat Texture2D s následujícími parametry:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Proto jsme schopní vykreslování zaznamenané Image na pozadí a vykreslení tímto způsobem scary mutanta jakékoli scény nad ním.

### <a name="adjusting-the-camera"></a>Nastavení fotoaparátu/kamery

`ARFrame` Objekty obsahovat také odhadovaný zařízení pozici.  Jsme teď budeme muset přesunout her fotoaparát ARFrame – před arkit, která nebyla důležitá záležitost orientace zařízení sledování (vrácení, výšku a úhlu natočení) a vykreslování připnuté hologram nad video -, ale pokud přesunete zařízení trochu - vntana bude odchylek.

K tomu dojde, protože předdefinovaných snímače, například volný setrvačník není schopen sledovat pohybů plb typu, mohou pouze akcelerace.  Analýzy ARKit jednotlivé funkce rámce a extrahuje odkazuje na sledování a proto je schopen poskytnout nám přesný transformace matice obsahující data o přesunu a otočení.

To je například jak nám můžete získat aktuální pozice:

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Používáme `-row.Z` vzhledem k tomu arkit, která používá pravoruký systém souřadnic.


### <a name="plane-detection"></a>Rovina detekce

Arkit, která je schopna zjistit horizontální rovin a tato možnost umožňuje interakci s reálným světem, například mutovanými můžete umístit na skutečné tabulky nebo a podlaží. Nejjednodušší způsob, jak to udělat, je použití hitTest – metoda (raycastingu). Převede souřadnice obrazovky (0,5; 0,5 je centrem) do reálného světa souřadnice (0; 0; 0 je umístění prvního rámce).

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

Nyní jsme lze umístit mutovanými na vodorovný ploše v závislosti na tom, kde na obrazovce zařízení, které jsme klepněte na:

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

V závislosti na podmínkách osvětlení reálného světa by měl být virtuální scény světlejší nebo tmavší tak, aby lépe odpovídala jeho okolí. ARFrame obsahuje LightEstimate vlastnost, která můžeme použít k úpravě Urho okolí světlo, Uděláte to takto:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>Nad rámec iOS – HoloLens

Urhosharpu [běží na všech hlavních operačních systémech](~/graphics-games/urhosharp/platform/index.md), takže můžete znovu použít jinde svůj existující kód.

HoloLens je jednou z nejskvělejších platformy, na kterých se spouští.   To znamená, že lze snadno přepínat mezi systémy iOS a vytvářet úžasné aplikace rozšířené Reality používání Urhosharpu HoloLens.

Můžete najít zdroje MutantDemo v [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Související odkazy

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (s Urhosharpu) (ukázka)](https://github.com/EgorBo/ARKitXamarinDemo)
