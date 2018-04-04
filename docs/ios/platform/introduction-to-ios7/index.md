---
title: Úvod do systému iOS 7
description: Tento článek se zabývá hlavní nových rozhraní API byla zavedená v iOS 7, včetně View Controller přechody, vylepšení UIView animací, UIKit Dynamics a Text Kit. Také vysvětluje některé změny na uživatelské rozhraní a nové funkce, které enchanced multitasking.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9ae82eba78f099f675d21bf53a250923630a0ff6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-7"></a>Úvod do systému iOS 7

_Tento článek se zabývá hlavní nových rozhraní API byla zavedená v iOS 7, včetně View Controller přechody, vylepšení UIView animací, UIKit Dynamics a Text Kit. Také vysvětluje některé změny na uživatelské rozhraní a nové funkce, které enchanced multitasking._

iOS 7 je hlavní aktualizace do systému iOS. Zavádí zcela nový návrh uživatelského rozhraní, který umísťuje fokus na obsahu spíše než aplikace chrome. Spolu s visual změn iOS 7 přidá nadbytku nové rozhraní API pro vytvoření bohatší interakce a prostředí. Tento dokument průzkumy nové technologie zavedena v systému iOS 7 a slouží jako výchozí bod pro zkoumání Další.

## <a name="uiview-animation-enhancements"></a>Vylepšení UIView animace

iOS 7 rozšiřuje podporu animace v UIKit, povolíte aplikací provádět akce, které dříve vyžadovaly vyřazování přímo do rozhraní základní animace. Například `UIView` teď můžete provádět pružiny animací, jakož i animací @keyframe, které určuje, který dříve `CAKeyframeAnimation` u `CALayer`.

### <a name="spring-animations"></a>Spring animace

 `UIView` Teď podporuje animování změny vlastností s pružiny vliv. To přidat volání `AnimateNotify` nebo `AnimateNotifyAsync` metodu předáním hodnoty pro pružiny tlumícího a rychlosti počáteční pružiny, jak je popsáno níže:

-  `springWithDampingRatio` – Hodnota mezi 0 a 1, kde kmitání zvyšuje pro menší hodnotu.
-  `initialSpringVelocity` – Počáteční pružiny rychlosti jako procento celkové animace vzdálenost za sekundu.


Následující kód vytvoří pružiny vliv při změně středu zobrazení bitové kopie:

```csharp
void AnimateWithSpring ()
{ 
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;
    
    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {
    
        imageView.Center = new CGPoint (imageView.Center.X, 400);   
            
    }, null);
}
```

Tento efekt pružiny způsobí, že obrázek zobrazení uloženo kolísají při dokončování jeho animace do nového umístění center, jak je uvedeno dále:

 ![](images/spring-animation.png "Tento efekt pružiny způsobí, že obrázek zobrazení uloženo kolísají při dokončování jeho animace do nového umístění center")

### <a name="keyframe-animations"></a>Klíčový snímek animace

`UIView` Třída nyní zahrnuje `AnimateWithKeyframes` metodu vytváření @keyframe, které určuje animací na `UIView`. Tato metoda je podobně jako u ostatního `UIView` s výjimkou animace metody, které další `NSAction` se předá jako parametr, který se zahrnují klíčové snímky. V rámci `NSAction`, klíčové snímky jsou přidána voláním `UIView.AddKeyframeWithRelativeStartTime`.

Například následující fragment kódu vytvoří animace klíčových snímků pro animaci také center zobrazit tak, aby otočit zobrazení:

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => { 
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => { 
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

První dva parametry, které chcete `AddKeyframeWithRelativeStartTime` metoda zadejte čas spuštění a trvání klíčový snímek, v uvedeném pořadí, v procentech celkové délky animace. V příkladu výše má za následek bitovou kopii zobrazení animace k jeho nové centrum přes první druhý, za nímž následuje otáčení 90 stupňů přes do příští sekundy. Vzhledem k tomu, že animace Určuje `UIViewKeyframeAnimationOptions.Autoreverse` jako možnost, jak klíčových snímků animace také pozpátku. Nakonec konečné hodnoty jsou nastaveny na počáteční stav v obslužné rutině dokončení.

Snímky obrazovky níže znázorňuje kombinované animace prostřednictvím klíčové snímky:

 ![](images/keyframes.png "Tato snímky obrazovky znázorňuje kombinované prostřednictvím klíčových snímků animace")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics je novou sadu rozhraní API v UIKit, které umožňují aplikacím vytvořit animovaný interakce podle fyziky. UIKit Dynamics zapouzdří modul 2D fyziky Chcete-li to možné.

Rozhraní API je deklarativní ve své podstatě. Deklarovat chování interakce fyziky vytvořením objektů - názvem *chování* – Pokud chcete express fyziky koncepty například gravitace, kolizí, pružiny atd. Pak behavior(s) připojíte k jinému objektu, názvem *dynamické animator*, který zapouzdřuje zobrazení. Dynamické animator trvá zdroje použití chování deklarované fyziky na *dynamické položky* -položky, které implementují `IUIDynamicItem`, například `UIView`.

Existuje několik různých primitivní chování dostupné pro aktivaci komplexní interakce, včetně:

-  `UIAttachmentBehavior` – Připojí dvě dynamické položky tak, aby se přesouvají společně nebo dynamické položky připojí k bodu přílohy.
-  `UICollisionBehavior` – Umožňuje dynamické položky se účastnit kolizí.
-  `UIDynamicItemBehavior` – Určuje sadu obecné vlastnosti, které chcete použít pro dynamické položky, jako jsou pružnost, hustotu a třením.
-  `UIGravityBehavior` -Gravitace se vztahuje na dynamické položky, způsobuje položky k urychlení gravitačním směrem.
-  `UIPushBehavior` – Force se vztahuje na dynamické položky.
-  `UISnapBehavior` – Umožňuje dynamické položky k pozici s pružiny vliv.


I když existují mnoho primitiv, obecný postup pro přidání do zobrazení pomocí UIKit Dynamics na základě fyziky interakce je konzistentní napříč chování:

1.  Vytvořte dynamické animator.
1.  Vytvořte behavior(s).
1.  Přidejte chování dynamické animator.


### <a name="dynamics-example"></a>Příklad Dynamics

Podívejme se na příklad, který přidá gravitace a hranici kolizí k `UIView`.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

Přidání gravitace zobrazení bitové kopie odpovídá 3 kroků uvedených výše.

Budete pracujeme `ViewDidLoad` metoda v tomto příkladu. Nejprve přidejte `UIImageView` instance následujícím způsobem:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Tím se vytvoří zobrazení obrazu zarovnaný na střed na horním okraji obrazovky. Chcete-li "patří" bitovou kopii s gravitace, vytvořte instanci `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

`UIDynamicAnimator` Přijímá instanci odkaz `UIView` nebo `UICollectionViewLayout`, který obsahuje položky, které bude animovaný za připojené behavior(s).

Dále vytvořte `UIGravityBehavior` instance. Můžete předat jeden nebo více objektů implementace `IUIDynamicItem`, například `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Chování je předán pole `IUIDynamicItem`, který v tomto případě obsahuje hodnotu single `UIImageView` instance jsme animace.

Nakonec přidejte chování dynamické animator:

```csharp
dynAnimator.AddBehavior (gravity);
```

Výsledkem je bitovou kopii animace dolů s gravitace jako ukázáno níže:

![](images/gravity2.png "Počáteční umístění obrázku") 
![](images/gravity3.png "koncová umístění obrázku")

Vzhledem k tomu, že není nic chovaly hranice na obrazovce, zobrazení image jednoduše spadá mimo dolní. Chcete-li omezit zobrazení tak, aby bitovou kopii koliduje s okraje obrazovky, jsme můžete přidat `UICollisionBehavior`. Jsme zaměříme v další části.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Budete začneme vytvořením `UICollisionBehavior` a její přidání do dynamické animator stejně, jako jsme to udělali pro `UIGravityBehavior`.

Upravit kód, který patří `UICollisionBehavior`:

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

`UICollisionBehavior` Má vlastnost s názvem `TranslatesReferenceBoundsIntoBoundry`. Toto nastavení na `true` způsobí, že je odkaz na rozsah zobrazení má být použit jako hranice kolizí.

Teď když bitovou kopii animuje dolů s gravitace, ho odrazí mírně od dolní části obrazovky před spuštěním k dispozici rest.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Další jsme můžete řídit chování klesající zobrazení bitové kopie s další chování. Například může přidáme `UIDynamicItemBehavior` zvýšit pružně tyto prostředky, a to způsobuje zobrazení image kolísají informace, když ho koliduje s dolní části obrazovky.

Přidání `UIDynamicItemBehavior` postupuje stejně jako u jiných chování. Nejprve vytvořte chování:

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Potom si do dynamické animator přidejte chování:

 `dynAnimator.AddBehavior (dynBehavior);`

Toto chování na místě odrazí zobrazení bitové kopie více při jeho koliduje s hranice.

## <a name="general-user-interface-changes"></a>Uživatelské rozhraní obecné změny

Kromě nových rozhraní API UIKit například UIKit Dynamics, přechody řadiče a rozšířené UIView animací popsané výše iOS 7 zavádí řadu visual změny uživatelského rozhraní a související změny rozhraní API pro různé zobrazení a ovládací prvky. Další informace najdete v článku [iOS 7 přehled uživatelského rozhraní](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Text Kit

Text Kit je nové rozhraní API, které nabízí výkonné text rozložení a vykreslování funkce. Je postavená na nízkou úroveň základní Text framework, ale je mnohem jednodušší než základní Text.

Další informace najdete v tématu naše [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>Multitasking

iOS 7 se změní, kdy a jak se provádí práce pozadí. Dokončení úkolů v iOS 7 už udržuje aplikace vzhůru Pokud úlohy běží na pozadí a aplikace jsou probuzený pro nesouvislé způsobem zpracování na pozadí. iOS 7 také přidá tři nové rozhraní API pro aktualizaci aplikace pomocí nového obsahu na pozadí:

-  Načítání na pozadí – umožňuje aplikacím aktualizovat obsah na pozadí v pravidelných intervalech.
-  Vzdálená oznámení - umožňuje aplikacím aktualizace obsahu při přijímání nabízených oznámení. Oznámení může být buď tichou nebo můžete zobrazit banner na zamykací obrazovce.
-  Služba přenosu na pozadí – umožňuje odesílání a stahování dat, jako je například velké soubory, bez pevné časový limit.


Další podrobnosti o nových funkcích multitasking najdete v tématu v částech Xamarin iOS [Backgrounding průvodce](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Souhrn

Tento článek se zabývá několik hlavní nově přidané do systému iOS. Nejprve ukazuje, jak přidat vlastní přechody do řadiče zobrazení. Potom ukazuje, jak používat přechody v zobrazení kolekcí, z i v rámci řadič navigační i interaktivně mezi zobrazení kolekcí. V dalším kroku zavádí několik vylepšení provedené UIView animací, znázorňující, jak aplikace použít UIKit pro věcí, které dříve vyžadovaly programování přímo na základní animace. Nakonec novou UIKit Dynamics rozhraní API, které přináší modul fyziky UIKit, uvádíme spolu s formátovaným textem podporu nyní k dispozici v rámci textu Kit.

## <a name="related-links"></a>Související odkazy

- [Úvod do systému iOS 7 (ukázka)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Přehled uživatelského rozhraní iOSu 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Zpracovávání úloh na pozadí](~/ios/app-fundamentals/backgrounding/index.md)
