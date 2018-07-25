---
title: 2D Matematika s Cocossharpem
description: Tento průvodce popisuje 2D Matematika pro vývoj her. Ukazují, jak provádět běžné úlohy vývoj her pomocí Cocossharpu a vysvětluje matematické za tyto úlohy.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 63c722b74c7dc7e034475e539f38204aca87763e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242050"
---
# <a name="2d-math-with-cocossharp"></a>2D Matematika s Cocossharpem

_Tento průvodce popisuje 2D Matematika pro vývoj her. Ukazují, jak provádět běžné úlohy vývoj her pomocí Cocossharpu a vysvětluje matematické za tyto úlohy._

Přesun objektů s kódem a umístit je představují základní součást vývoj her všech velikostí. Umístění a přesouvání vyžadují použití matematické, jestli hru vyžaduje přesouvání objektu podél rovné čáry, nebo použijte trigonometrických pro otáčení. Tento dokument se týká následujících tématech:

 - Rychlost
 - Akcelerace
 - Otáčení objektů Cocossharpu
 - Otočení pomocí rychlosti

Vývojáři, kteří nemají silné matematické pozadí nebo kteří long zapomněli tato témata školy, nemusíte se obávat – tento dokument se tedy člení koncepty pobavení kusů a doprovodný teoretické vysvětlení praktických příkladech. Stručně řečeno, tento článek se odpovědět na otázku, letitý matematické studenta: "Když se ve skutečnosti potřebuji použít to?"


## <a name="requirements"></a>Požadavky

I když tento dokument se zaměřuje především na straně matematické Cocossharpu, ukázky kódu předpokládají práci s objekty dědění formuláře `CCNode`. Kromě toho od `CCNode` neobsahuje hodnoty pro rychlost a akceleraci kód předpokládá práci s entitami, které obsahují hodnoty, jako je VelocityX, VelocityY, AccelerationX a AccelerationY. Další informace o entitách, najdete v našem návodu na [entity v Cocossharpu](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Rychlost

Vývojáři her použijte termín *rychlosti* popisující, jak se objekt pohybuje – konkrétně jak rychle něco pohybuje a směr se pohybuje. 

Rychlost je definován pomocí dvou typů jednotek: jednotka pozice a časovou jednotku. Například rychlost automobilu je definován jako mil za hodinu nebo kilometrů za hodinu. Přesouvá často použití pixelů za sekundu definovat jak rychle objekt vývojáři her. Například může přesunout odrážky rychlostí 300 pixelů za sekundu. To znamená pokud odrážky pohybuje v 300 pixelů za sekundu, pak ho budou přesunuty 600 jednotky ve dvou sekund a 900 jednotky do tří sekund a tak dále. Obecně platí, hodnota rychlosti *přidá* na pozici objektu (jak jsme uvidíte níže).

I když jsme rychlost použít k vysvětlení jednotky rychlosti, rychlost termín obvykle odkazuje na hodnotu nezávisle na směru, zatímco termín rychlosti odkazuje na rychlost a směr. Proto přiřazení odrážek rychlost (za předpokladu, že odrážka je třída, která obsahuje potřebné vlastnosti) může vypadat takto:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Implementace rychlosti

Cocossharpu neimplementuje rychlosti, tedy objekty, které vyžadují přesouvání bude nutné implementovat své vlastní logiky pohyb. Nové vývojáři her implementace rychlosti často uděláte zpřístupnění jejich rychlost závisí na snímkovou frekvenci. To znamená, následující *nesprávná implementace* se zdá se, že poskytuje správné výsledky, ale budou založeny na hry Snímková frekvence:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Pokud hra trvá na vyšší frekvence snímků (například 60 snímků za sekundu místo 30 snímků za sekundu), se zobrazí objekt do postupovat rychleji, než pokud běží na nižší Snímková frekvence. Podobně pokud hry nedokáže zpracovat snímků na jako vysokou frekvenci (která může být způsobeno procesy na pozadí pomocí prostředky zařízení), hru zobrazí zpomalovat.

Aby se zohlednily to, rychlost je často implementované, využívá hodnotu času. Například pokud `seconds` proměnná představuje číslo (nebo zlomek) sady sekund od poslední čas rychlosti, pak následující kód by způsobil objektu s bez ohledu na to, frekvence snímků konzistentní vzhledem k aplikacím přesunu:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Vezměte v úvahu, že hry, který se spouští s nižší rychlostí rámce aktualizuje umístění své objekty méně často. Jednotlivé aktualizace proto způsobí objekty přesunu dále, než kdyby je sdíleli Pokud hra se aktualizuje častěji. `seconds` Hodnota účty v takovém případě se vytváření sestav, kolik času uplynulo od poslední aktualizace.

Příklad toho, jak přidat podle času přesunu, naleznete v tématu [tento předpisu pokrývající čas na základě pohybu](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/game_development/time_based_movement).


### <a name="calculating-positions-using-velocity"></a>Výpočet pozice pomocí rychlosti

Rychlost je možné předvídáme kde objekt budou po určitou dobu předá nebo pomoct vyladit chování objektů, aniž byste museli spouštět hry. Třeba vývojář, který implementuje přesun aktivace odrážky musí nastavit odrážky rychlosti po je vytvořena instance. Velikost obrazovky lze použít jako základ pro nastavení rychlosti. To znamená pokud vývojář, který zná odrážka by měl přesuňte výška obrazovky do 2 sekund a pak rychlosti by mělo být nastavené výšky na obrazovce, děleno 2. Pokud obrazovky 800 pixelů na výšku, by od odrážky rychlost nastavit na 400 (což je 800/2).

Logika hry podobně, může být potřeba vypočítat, jak dlouho bude trvat objektu k dosažení cíle zadané její rychlosti. To lze vypočítat vydělením vzdálenost projít podle rychlosti provozní objekt. Například následující kód ukazuje, jak přiřadit text popisku, který zobrazuje, jak dlouho dokud nedosáhne střel cíli:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Akcelerace

*Akcelerace* je běžným pojmem vývoj her a sdílí je mnoho podobností s rychlosti. Akcelerace kvantifikuje, zda je objekt urychlení nebo zpomalení (jak hodnota rychlosti změny v průběhu času). Akcelerace *přidá* ukazateli rychlost, stejně jako rychlosti přidá k umístění. Běžné aplikace zrychlení obsahují závažnosti, automobilu urychlení a místo dodání ohlásí jeho thrusters. 

Podobné ukazateli rychlost, akcelerace je definována v pozici a jednotkou času; ale zrychlení vaší časovou jednotku se označuje jako *kvadratických* jednotku, která odráží, jak matematicky definuje akcelerace. To znamená, herní akcelerace se často počítá v *pixelů za sekundu spolehlivosti*.

Pokud má objekt zrychlení X 10 jednotek za sekundu spolehlivosti, pak to znamená, že její rychlosti zvýší o 10 za sekundu. Pokud od úplně paralyzuje, po sekundě bude Přesun na 10 jednotek za sekundu, dvě po sekundách 20 jednotek za druhé, a tak dále.

Akcelerace ve dvou dimenzích vyžaduje komponentu X a Y, takže je možné přiřadit takto:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Zrychlení a zpomalení

Přestože zrychlení a zpomalení jsou rozlišené někdy v každodenní řeči, neexistuje žádné technické rozdíly mezi nimi. Závažnosti je platnost, což vede k akceleraci. Pokud objekt je vyvolána směrem nahoru pak závažnosti povede ke zpomalení ho (zpomaluje), ale jakmile se zastavila, tato hodnota roste a ve stejném směru jako závažnosti se opožďuje objektu pak závažnosti zrychlují ho (zrychlení). Jak je znázorněno níže, aplikace zrychlení je stejný, ať se právě používá ve stejném směru směr nebo naopak pohybu. 


### <a name="implementing-acceleration"></a>Implementovat acceleration

Akcelerace je podobné ukazateli rychlost při implementaci – není implementováno automaticky pomocí Cocossharpu a akcelerace podle času je požadovaného nasazení (na rozdíl od založených na snímcích akcelerace). Implementace jednoduchého akcelerace (spolu s rychlosti) proto může vypadat jako:

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Výše uvedený kód je, co se říká *lineární aproximace* pro zrychlení provádění. Efektivně implementuje akcelerace se poměrně zavřít úrovní přesnosti, ale není zcela přesné typu akcelerace. Vysvětlení konceptu jak implementovat acceleration je zahrnutý výše.

Následující implementaci je akcelerace a rychlosti matematicky přesné aplikace:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Nejobvyklejšími rozdíl na výše uvedeném kódu je `halfSecondsSquared` proměnnou a její používání použít akcelerace na pozici. Matematické důvodem je nad rámec tohoto návodu, ale vývojáři chtějí matematické za to, najdete další informace najdete v [této diskuse o integraci akcelerace.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Praktický dopad `halfSecondSquare` je, že akcelerace se bude chovat matematicky přesně a předvídatelným způsobem bez ohledu na to snímkovou frekvenci. Lineární aproximace akcelerace se může frekvence snímků – Čím nižší snímkové frekvence zahodí, tím méně přesné sblížení, stane. Pomocí `halfSecondsSquared` záruky, že kód se bude chovat stejně, bez ohledu na to snímkovou.


## <a name="angles-and-rotation"></a>Jedná o úhly a otočení

Vizuál objekty, jako `CCSprite` podporují otočení prostřednictvím `Rotation` proměnné. To můžete přiřadit na hodnotu nastavení otočení ve stupních. Například následující kód ukazuje, jak otočit `CCSprite` instance:


```csharp
CCSprite unrotatedSprite = new CCSprite("star.png");
unrotatedSprite.IsAntialiased = false;
unrotatedSprite.PositionX = 100;
unrotatedSprite.PositionY = 100;
this.AddChild (unrotatedSprite);

CCSprite rotatedSprite = new CCSprite("star.png");
rotatedSprite.IsAntialiased = false;
// This sprite is moved to the right so it doesn’t overlap the first
rotatedSprite.PositionX = 130;
rotatedSprite.PositionY = 100;
rotatedSprite.Rotation = 45;
this.AddChild (rotatedSprite); 
```

Výsledkem je následující:

![](math-images/image1.png "Výsledkem je tento snímek obrazovky")

Všimněte si, že je otáčení 45 stupňů doprava (což je z historických důvodů je opakem otočení použití matematicky):

![](math-images/image2.png "Všimněte si, že otočení je 45 stupňů po směru hodinových ručiček která z historických důvodů je opakem otočení použití matematicky")

Otočení Cocossharpu a matematického regulární systému obecně lze vizualizovat takto:

![](math-images/image3.png "Obecně lze vizualizovat otočení Cocossharpu a matematického regulární systému tímto způsobem")

![](math-images/image4.png "Obecně lze vizualizovat otočení Cocossharpu a matematického regulární systému tímto způsobem")

Toto rozlišení je důležité, protože `System.Math` třída používá proti směru hodinových ručiček otočení, takže třeba Invertovat úhly při práci s vývojáři, kteří znají Tato třída `CCNode` instancí.

Je třeba poznamenat, že výše uvedené diagramy zobrazit otočení ve stupních; ale některé matematické funkce (jako je například funkce v `System.Math` oboru názvů) očekávají a návratové hodnoty v *radiány* místo stupňů. Podíváme se na tom, jak převod mezi typy dvě jednotky o něco později v tomto průvodci.


### <a name="rotating-to-face-a-direction"></a>Otáčení směr pro rozpoznávání tváře

Jak je uvedeno výše, `CCSprite` můžete otočit pomocí `Rotation` vlastnost. `Rotation` Poskytuje vlastnost `CCNode` (základní třída pro `CCSprite`), což znamená, že otočení lze použít k entitám, které dědí nastavení z `CCNode` také. 

Některé hry vyžadují objekty obměnit tak čelí cíl. Mezi příklady patří nepřítele spravovanými počítači potíží na cíl přehrávač nebo místo příjemce letíte bodu, kde je uživatel klepnou na obrazovce. Ale hodnota otočení musí nejprve se počítají na základě umístění entity střídajících a umístění cíle pro rozpoznávání tváře.

Tento proces vyžaduje několik kroků:

 - Identifikace *posun* cíle. Posun odkazuje na X a Y vzdálenost mezi entitou otáčení a Cílová entita.
 - Výpočet úhlu z posunu pomocí trigonometrické funkce Arkus tangens (vysvětlení obsahuje podrobnější rozpis naleznete níže).
 - Nastavení pro rozdíl mezi směřující na pravé straně a směr, ve kterém body otáčení entity při zrušení otočený 0 stupňů.
 - Nastavení pro rozdíl mezi matematické otočení (proti směru hodinových ručiček) a otočení Cocossharpu (po směru hodinových ručiček).

Následující funkce (předpokládá se, že má být zapsán v entitě) otočí entita, která má cíl pro rozpoznávání tváře:


```csharp
// This function assumes that it is contained in a CCNode-inheriting object
public void FacePoint(float targetX, float targetY)
{
    // Calculate the offset - the target's position relative to "this"
    float xOffset = targetX - this.PositionX;
    float yOffset = targetY - this.PositionY;

    // Make sure the target isn't the same point as "this". If so,
    // then rotation cannot be calculated.
    if (targetX != this.PositionX || targetY != this.Position.Y)
    {

        // Call Atan2 to get the radians representing the angle from 
        // "this" to the target
        float radiansToTarget = (float)System.Math.Atan2 (yOffset, xOffset);

        // Since CCNode uses degrees for its rotation, we need to convert
        // from radians
        float degreesToTarget = CCMathHelper.ToDegrees (radiansToTarget);

        // The direction that the entity faces when unrotated. In this case
        // the entity is facing "up", which is 90 degrees 
        const float forwardAngle = 90;

        // Adjust the angle we want to rotate by subtracting the
        // forward angle.
        float adjustedForDirecitonFacing = degreesToTarget - forwardAngle;

        // Invert the angle since CocosSharp uses clockwise rotation
        float cocosSharpAngle = adjustedForDirecitonFacing * -1;

        // Finally assign the rotation
        this.Rotation = rotation = cocosSharpAngle;
    }
} 
```

Výše uvedený kód může použít otočí entitu, aby směřovala k bodu, kde uživatel se dotýká obrazovky, následujícím způsobem:


```csharp
private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    if(touches.Count > 0)
    {
        CCTouch firstTouch = touches[0];
        FacePoint (firstTouch.Location.X, firstTouch.Location.Y);
    }
} 
```

Tento kód vrátí následující chování:

![](math-images/image5.gif "Tento kód má za následek toto chování")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>Použití Atan2 pro převod posuny úhly

`System.Math.Atan2` je možné převést posun úhlu. Název funkce `Atan2` pochází z Arkus tangens trigonometrické funkce. "2" přípona rozlišuje tuto funkci ze standardní `Atan` funkce, který přesně odpovídá matematické chování Arkus tangens. Arkus tangens je funkce, která vrátí hodnotu v rozsahu od -90 a + 90 degrees (nebo ekvivalent v radiánech). Mnoho aplikací včetně počítačové hry, často vyžadují úplné 360 stupňů hodnot, takže `Math` třída zahrnuje `Atan2` pro splnění tohoto požadavku.

Všimněte si, že výše uvedený kód předá parametr Y nejprve, pak parametr X při volání `Atan2` metody. Toto je zpětně od obvyklého X, Y řazení souřadnice polohy. Další informace [najdete v článku dokumentace Atan2](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx).

Je také vhodné poznamenat, že návratová hodnota z `Atan2` je v radiánech, který se používá k měření úhly jinou jednotku. Tato příručka nepodporuje pokrývat podrobnosti radiány, ale mějte na paměti, který všechny trigonometrické funkce v `System.Math` obor názvů použijte radiány, tak všechny hodnoty musí být převeden na stupně před v Cocossharpu objekty používají. Můžete najít další informace o radiány [stránce Wikipedie radián](http://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>Dopředné úhel

Jednou `FacePoint` metoda převede úhel na radiány, definuje `forwardAngle` hodnotu. Tato hodnota představuje úhel, ve kterém entita je otočena směrem k při jeho otočení hodnota se rovná 0. V tomto příkladu předpokládáme, že entity je otočena směrem nahoru, což je 90 stupňů při použití matematických otočení (na rozdíl od Cocossharpu otočení). Tady používáme matematické otočení vzhledem k tomu, že jsme dosud nebyly obrácený otočení pro Cocossharpu.

Následující příklad zobrazuje co entita se `forwardAngle` 90 stupňů může vypadat například takto:

![](math-images/image6.png "To ukazuje, jak může vypadat entitu s forwardAngle 90 stupňů")


### <a name="angled-velocity"></a>Zalomená rychlosti

Zatím jsme se zabývali jak převést posun na úhlu. Tato část platí jiným způsobem – přebírá úhel a převede jej na X a Y. Běžné příklady automobilu přesouvání směr, ve kterém je otočena nebo místo příjemce potíží odrážky, které přesouvá v směr, ve kterém je otočena směrem k lodě. 

Rychlost, koncepčně dá vypočítat jako první definování požadovanou rychlost při zrušení otočený a otočení tohoto rychlosti úhlu, který je otočena směrem k entitě. Chcete-li pomoci s vysvětlením tento koncept, rychlost (a akceleraci) lze vizualizovat jako trojrozměrné na 2 *vektoru* (což je obvykle vykreslen jako šipka). Vektor pro hodnotu rychlosti s X = 100 a Y = 0 lze vizualizovat takto:

![](math-images/image7.png "Vektor pro hodnotu rychlosti s X = 100 a Y = 0 lze vizualizovat takto")

Tento problém můžete otočen za následek nové rychlosti. Například otáčení vektoru o 45 stupňů (pomocí otočení proti směru hodinových ručiček) vrátí následující:

![](math-images/image8.png "Otáčení vektoru o 45 stupňů proti směru hodinových ručiček otočení výsledky použití v tomto")

Naštěstí rychlosti, zrychlení a dokonce i umístění lze všechny otočit se stejným kódem bez ohledu na to, jak se používají hodnoty. Následující funkce pro obecné účely lze hodnota Cocossharpu otočení obměna vektoru:


```csharp
// Rotates the argument vector by degrees specified by
// cocosSharpDegrees. In other words, the rotation
// value is expected to be clockwise.
// The vector parameter is modified, so it is both an in and out value
void RotateVector(ref CCVector2 vector, float cocosSharpDegrees)
{
    // Invert the rotation to get degrees as is normally
    // used in math (counterclockwise)
    float mathDegrees = -cocosSharpDegrees;

    // Convert the degrees to radians, as the System.Math
    // object expects arguments in radians
    float radians = CCMathHelper.ToRadians (mathDegrees);

    // Calculate the "up" and "right" vectors. This is essentially
    // a 2x2 matrix that we'll use to rotate the vector
    float xAxisXComponent = (float)System.Math.Cos (radians);
    float xAxisYComponent = (float)System.Math.Sin (radians);
    float yAxisXComponent = (float)System.Math.Cos (radians + CCMathHelper.Pi / 2.0f);
    float yAxisYComponent = (float)System.Math.Sin (radians + CCMathHelper.Pi / 2.0f);

    // Store the original vector values which will be used
    // below to perform the final operation of rotation.
    float originalX = vector.X;
    float originalY = vector.Y;

    // Use the axis values calculated above (the matrix values)
    // to rotate and assign the vector.
    vector.X = originalX * xAxisXComponent + originalY * yAxisXComponent;
    vector.Y = originalX * xAxisYComponent + originalY * yAxisYComponent;
} 
```

Úplné povědomí o `RotateVector` metoda vyžaduje právě znáte trigonometrické funkce kosinus a sinus spolu s některé algebraický lineární, což je nad rámec tohoto článku. Ale jednou implementované `RotateVector` metody slouží k otočení jakékoli vektoru, včetně rychlosti vektoru. Například následující kód může vyvolat odrážek v určeném směru `rotation` hodnotu:


```csharp
// Create a Bullet instance
Bullet newBullet = new Bullet();

// Define the velocity of the bullet when 
// rotation is 0
CCVector2 velocity = new CCVector2 (0, 100);

// Modify the velocity according to rotation
RotateVector (ref velocity, rotation);

// Assign the newBullet's velocity using the
// rotated vector
newBullet.VelocityX = velocity.X;
newBullet.VelocityY = velocity.Y;

// Set the bullet's rotation so it faces
// the direction that it's flying
newBullet.Rotation = rotation; 
```

Tento kód může vytvořit něco jako:

![](math-images/image9.png "Tento kód může vytvořit něco jako tento snímek obrazovky")


## <a name="summary"></a>Souhrn

Tento průvodce popisuje běžné matematické vzorce vývoji 2D her. Ukazuje, jak přiřadit a implementovat rychlosti a akcelerace a popisuje, jak otočit objekty a vektory pro přesun vede libovolným směrem.
