---
title: 2D matematické s CocosSharp
description: Tento průvodce pokrývá 2D Matematika pro vývoj her. Pomocí CocosSharp ukazují, jak provádět běžné úkoly vývoj her a vysvětluje výpočty za tyto úlohy.
ms.prod: xamarin
ms.assetid: 5C241AB4-F97E-4B61-B93C-F5D307BCD517
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 13279df69b7a22117c10d74f1a15c082aff13264
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="2d-math-with-cocossharp"></a>2D matematické s CocosSharp

_Tento průvodce pokrývá 2D Matematika pro vývoj her. Pomocí CocosSharp ukazují, jak provádět běžné úkoly vývoj her a vysvětluje výpočty za tyto úlohy._

Přesun objektů s kódem a umístit je základní součástí vývoj her všech velikostí. Umístění a přesunutí vyžadují použití matematické, zda hru vyžaduje přesunutí objektu podél přímku nebo použití trigonometrické pro otočení. Tento dokument popisuje v následujících tématech:

 - Rychlost
 - Akcelerace
 - Otáčení CocosSharp objektů
 - Otočení pomocí rychlosti postupu

Vývojáři, kteří nemají silné matematické pozadí nebo který dlouho zapomněli jste tato témata z školy, nemusíte si dělat starosti – tento dokument se rozdělte koncepty na velikost jimiž části a doprovodný teoretické vysvětlení s praktické příklady. Stručně řečeno, bude v tomto článku odpověď na otázku student pokládáme staletí starou matematické: "Pokud se ve skutečnosti třeba použít tento vlastní položky?"


## <a name="requirements"></a>Požadavky

I když tento dokument se zaměřuje především na matematické straně CocosSharp, ukázky kódu předpokládá pracovat s objekty dědění formuláře `CCNode`. Kromě toho od `CCNode` nezahrnuje hodnoty pro rychlost a akcelerace kód předpokládá práci s entitami, které poskytují hodnoty například VelocityX, VelocityY, AccelerationX a AccelerationY. Další informace o entity, najdete v našem návodu v [entity v CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="velocity"></a>Rychlost

Herní vývojáři použít termín *rychlosti* popisují, jak je objekt přesunutí – konkrétně jak rychlé něco přesunutí a směr to je přesunutí. 

Rychlosti je definován pomocí dva typy jednotek: jednotka pozice a časovou jednotku. Například rychlost automobilu je definován jako paliva za hodinu nebo kilometrech za hodinu. Herní vývojáři přesune často použití pixelů za sekundu definovat jak rychlé objekt. Odrážka může například přesouvat rychlostí 300 pixelů za sekundu. To znamená pokud odrážku přesunutí na 300 pixelů za sekundu, pak mají přesune 600 jednotky ve dvou sekund a 900 jednotky v tři sekundy a tak dále. Obecně platí, hodnota rychlosti *přidá* na pozici objektu (jak jsme najdete níže).

I když jsme použili rychlost vysvětlit jednotky rychlosti, rychlosti termín obvykle odkazuje na hodnotu nezávisle na směru, při rychlosti výraz odkazuje na rychlost a směr. Proto přiřazení odrážka rychlosti (za předpokladu, že je třída, která obsahuje nezbytné vlastnosti odrážka) může vypadat takto:


```csharp
// This bullet is not moving horizontally, so set VelocityX to 0:
bulletInstance.VelocityX = 0;
// Positive Y is "up" so move the bullet up 300 units per second:
bulletInstance.VelocityY = 300;
```


### <a name="implementing-velocity"></a>Implementace rychlosti postupu

CocosSharp neimplementuje rychlosti, objekty, které vyžadují přesun ho tedy musí implementovat vlastní přesun logiku. Nové herní vývojáři implementace rychlosti často, aby chybu při jejich rychlosti závislé na obnovovací frekvence. To znamená, následující *nesprávné implementace* zajistit správné výsledky budou vypadat, ale budou založeny na příslušnou hru obnovovací frekvence:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX;
this.PositionY += this.VelocityY;
```

Pokud hra běží na vyšší frekvence snímků (například 60 snímků za sekundu místo 30 snímků za sekundu), objekt zobrazí na rychlejší než pokud máte s nižší rychlostí rámce. Podobně pokud hra se nepodařilo zpracovat rámce v jako vysokou obnovovací frekvence (který může být způsobeno procesy na pozadí pomocí prostředků zařízení), se zobrazí hra zpomalovat.

Aby se zohlednily to, rychlosti je často implementovaná, pomocí hodnotu času. Například pokud `seconds` proměnná představuje číslo (nebo část) sekund vzhledem k tomu, že bylo použito poslední čas rychlosti, pak následující kód by způsobilo objekt s konzistentní Přesun bez ohledu na to obnovovací frekvence:

```csharp
// VelocityX and VelocityY will be added every time this code executes
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Zvažte, že bude hra, který se spouští s nižší rychlostí rámce aktualizovat pozici jeho objektů méně často. Každá aktualizace proto způsobí objekty přesunutí další, než kdyby je sdíleli Pokud hra byly aktualizace častěji. `seconds` Hodnotu účty v takovém případě generování sestav, jak dlouhá doba od poslední aktualizace uplynul.

Příklad toho, jak přidat založené na čase přesun, naleznete v části [tento tajný recept pokrývajících čas na základě přesun](https://developer.xamarin.com/recipes/cross-platform/game_development/time_based_movement/).


### <a name="calculating-positions-using-velocity"></a>Výpočet pozice pomocí rychlosti postupu

Rychlosti lze provádět předpovědi o kde objekt budou po určitou dobu předává nebo pomáhají ladit chování objektu bez nutnosti spustit hru. Například vývojáři, který je implementace pohyb přímým odrážka je potřeba nastavit odrážka rychlosti po vytvoření instance. Velikost obrazovky lze použít jako základ pro nastavení rychlosti. To znamená pokud vývojář zná, který odrážkou přesuňte výšku obrazovky v sekundách 2 a potom rychlosti musí být nastavena na výšku obrazovce dělený 2. Pokud obrazovky je 800 pixelů vysoký, by odrážka rychlost nastavit na 400 (což je 800/2).

Podobně ve hře logiku potřebovat vypočítat, jak dlouho bude trvat objekt pro dosažení cíle zadané jeho rychlosti. To lze vypočítat vydělením vzdálenosti projít, podle rychlosti provozní objekt. Například následující kód ukazuje, jak přiřadit text popisku, který zobrazuje, jak dlouho dokud střel neobsahuje požadovanou hodnotu:


```csharp
// We'll assume only the X axis for this example
float distanceX = target.PositionX - missile.PositionX;

float secondsToReachTarget = distanceX / missile.VelocityX;

label.Text = secondsToReachTarget + " seconds to reach target"; 
```


## <a name="acceleration"></a>Akcelerace

*Akcelerace* je běžným pojmem v vývoj her a sdílí mnoho společného s rychlosti. Akcelerace kvantifikuje, zda je objekt urychlení nebo zpomalení (jak hodnota rychlosti změny v čase). Akcelerace *přidá* na rychlosti, stejně jako rychlosti přidá na pozici. Běžné aplikace akcelerace patří gravitace automobilu urychlení a aktivuje její thrusters lodě místa. 

Podobně jako u rychlosti, akcelerace je definována v pozici a časovou jednotku; ale na akcelerace časovou jednotku se označuje jako *kvadratických* jednotky, která zobrazuje, jak je matematický definovaný akcelerace. To znamená, herní akcelerace často měřená v *spolehlivosti pixelů za sekundu*.

Pokud objekt má zrychlení X 10 jednotek za sekundu spolehlivosti, pak to znamená, že se jeho rychlosti zvýšit 10 každou sekundu. Pokud od k nečinnosti, po jednu sekundu bude přesunutí na 10 jednotek za sekundu, dvě po sekund 20 jednotek za sekundu, a tak dále.

Akcelerace v dvěma rozměry vyžaduje komponentu X a Y, takže může být přiřazen následujícím způsobem:


```csharp
// No horizontal acceleration:
icicle.AccelerationX = 0;
// Simulate gravity with Y acceleration. Negative Y is down, so assign a negative value:
icicle.AccelerationY = -50; 
```


### <a name="acceleration-vs-deceleration"></a>Akcelerace oproti zpomalení

I když jsou v každodenní řeči někdy rozlišené zrychlení a zpomalení, není žádný technické rozdíl mezi nimi. Gravitace je force, což vede k akcelerace. Pokud objekt je vyvolána nahoru pak gravitace bude zpomalit ho (zpomaluje), ale po objekt zastavil stoupající a je použit ve stejném směru jako gravitace pak gravitace je urychlení ho (urychlení). Jak vidíte níže, aplikace zrychlení je stejný, zda je používán ve stejném směru nebo opak směru pohybu. 


### <a name="implementing-acceleration"></a>Implementace akcelerace

Akcelerace je podobná rychlosti při implementaci – není implementována automaticky podle CocosSharp a založené na čase akcelerace k požadované implementace (na rozdíl od na základě rámce akcelerace). Implementace jednoduchého akcelerace (spolu s rychlosti) proto může vypadat podobně jako:

```csharp
this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds;
this.PositionX += this.VelocityX * seconds;
this.PositionY += this.VelocityY * seconds;
```

Výše uvedený kód je, co se označuje jako *lineární aproximace* pro implementaci akcelerace. Efektivní implementuje akcelerace vcelku close stupeň přesnost, ale není perfektně přesné model akcelerace. Vysvětlení koncept způsob implementace akcelerace je zahrnutá výše.

Za následující implementaci je matematický přesné aplikace akcelerace a rychlosti:


```csharp
float halfSecondsSquared = (seconds * seconds) / 2.0f;

this.PositionX += 
    this.Velocity.X * seconds + this.AccelerationX * halfSecondsSquared;
this.PositionY += 
    this.Velocity.Y * seconds + this.AccelerationY * halfSecondsSquared;

this.VelocityX += this.AccelerationX * seconds;
this.VelocityY += this.AccelerationY * seconds; 
```

Nejobvyklejší rozdíl na výše uvedeném kódu je `halfSecondsSquared` proměnnou a její využití použít akcelerace na pozici. Matematické důvodem je nad rámec tohoto kurzu, ale vývojáři zájem o výpočty za to můžete najít další informace v [toto pojednání o integraci akcelerace.](http://www.cliffsnotes.com/math/calculus/calculus/integration/distance-velocity-and-acceleration)

Praktický dopad `halfSecondSquare` je, že akcelerace budou chovat matematicky přesně a předvídatelné bez ohledu na to obnovovací frekvence. Lineární aproximace zrychlení podléhá obnovovací frekvence – Čím nižší snímkový kmitočet klesne méně přesný stane sblížení. Pomocí `halfSecondsSquared` záruky kódu budou chovat stejný bez ohledu na kmitočet snímků.


## <a name="angles-and-rotation"></a>Úhly a oběh

Visual objekty, jako `CCSprite` podporu otočení prostřednictvím `Rotation` proměnné. To může být přiřadit hodnotu k nastavení jeho otočení ve stupních. Například následující kód ukazuje, jak otočit `CCSprite` instance:


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

Všimněte si, že je oběh je 45 stupňů doprava (což je historických důvodů je opakem otočení použití matematicky):

![](math-images/image2.png "Všimněte si, že je oběh je 45 stupňů po směru hodinových ručiček který historických důvodů je opakem otočení použití matematicky")

Otočení CocosSharp a regulární Matematika obecně můžete vizualizovat následujícím způsobem:

![](math-images/image3.png "Obecně můžete vizualizovat otočení CocosSharp a regulární Matematika takto")

![](math-images/image4.png "Obecně můžete vizualizovat otočení CocosSharp a regulární Matematika takto")

Tento rozdíl je důležité, protože `System.Math` třída používá proti směru hodinových ručiček kolem, takže vývojáři, kteří znají Tato třída musí být Invertovat úhly při práci s `CCNode` instance.

Je třeba poznamenat, že výše diagramy zobrazit otočení ve stupních; ale některé matematické funkce (jako je například funkce v `System.Math` oboru názvů) očekávat a návratové hodnoty v *radiánech* místo stupňů. Podíváme postupy pro převod mezi typy dvě jednotky se může dál v této příručce.


### <a name="rotating-to-face-a-direction"></a>Otáčení čelit směr

Jako v příkladu nahoře, `CCSprite` lze otáčet pomocí `Rotation` vlastnost. `Rotation` Vlastnost zajišťuje `CCNode` (základní třída pro `CCSprite`), což znamená, že otočení lze použít pro entity, které dědí `CCNode` také. 

Některé hry vyžadují objekty, které se otáčet, se kterými se cíl. Mezi příklady patří počítače řízené enemy, čistá na cíl player nebo místa lodě plující pod směrem bodu, kde je uživatel klepnou na obrazovce. Ale hodnota otočení nejprve se počítá na základě na umístění entity otáčení a umístění cíle pro práci.

Tento proces vyžaduje několik kroků:

 - Identifikace *posun* cíle. Posun odkazuje na X a Y vzdálenost mezi rotační entity a Cílová entita.
 - Výpočet úhel z posun pomocí funkce trigonometrické Arkus tangens (podrobně popsány níže).
 - Úprava pro rozdíl mezi směřující na doprava a směr, který rotační entity body při zrušení otočený 0 stupňů.
 - Úprava pro rozdíl mezi matematickém otočení (proti směru hodinových ručiček) a CocosSharp otočení (po směru hodinových ručiček).

Následující funkce (předpokládá se, že má být zapsán v entitě) otočí entity, která má čelí cíl:


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

Výše uvedený kód může otočit entitu, aby směřovala bodu, kde uživatel je klepnou na obrazovce takto:


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

![](math-images/image5.gif "Tento kód výsledkem toto chování")

#### <a name="using-atan2-to-convert-offsets-to-angles"></a>Použití Atan2 pro převod posune úhly

`System.Math.Atan2` Umožňuje převést posun úhlu. Název funkce `Atan2` pochází z Arkus trigonometrické funkce. "2" přípona odlišuje tuto funkci od standardní `Atan` funkce, který by odpovídal výhradně matematickém chování Arkus. Arkus tangens je funkci, která vrátí hodnotu v rozsahu od -90 a + 90 stupňů (nebo ekvivalent v radiánech). Mnoho aplikací, včetně počítače hry, často vyžadují úplné 360 stupňů hodnot, proto `Math` třída zahrnuje `Atan2` ke splnění tohoto požadavku.

Všimněte si, že výše uvedený kód předá parametr Y nejprve, pak parametr X při volání metody `Atan2` metoda. Toto je zpětně z obvyklé X, Y řazení souřadnice polohy. Další informace [najdete v části dokumentace Atan2](https://msdn.microsoft.com/library/system.math.atan2(v=vs.110).aspx).

Je také vhodné poznamenat, že z hodnoty návratový `Atan2` je v radiánech, který se používá k měření úhly jiné jednotce. Tato příručka nepodporuje pokrývat podrobnosti radiánech, ale mějte na paměti, všechny trigonometrické funkce v `System.Math` obor názvů používá radiánech, takže všechny hodnoty musí být převedena na stupňů před na objekty CocosSharp používá. Další informace o radiánech můžete najít [na stránce Wikipedia radián](http://en.wikipedia.org/wiki/Radian).

#### <a name="forward-angle"></a>Předat dál úhlu

Jednou `FacePoint` metoda převede úhel radiánech, definuje `forwardAngle` hodnotu. Tato hodnota představuje úhlu, ve kterém je entita setkávají při jeho otočení hodnota rovná 0. V tomto příkladu předpokládáme, že entity směrem k směrem nahoru, což je 90 stupňů, při použití matematickém otočení (na rozdíl od CocosSharp otočení). Tady používáme matematickém otočení vzhledem k tomu, že jsme ještě nebyly obrácený otočení pro CocosSharp.

Následující příklad zobrazuje jaké entity s `forwardAngle` 90 stupňů může vypadat například:

![](math-images/image6.png "Ukazuje to, jak může vypadat entity s forwardAngle 90 stupňů.")


### <a name="angled-velocity"></a>Úhlové rychlosti

Jsme, pokud jste již hledali v tom, jak převést posun úhlu. Tato část přejde jiný způsob – použije úhel a převede jej na X a Y hodnoty. Běžné příklady automobilu přesunutí v směr, kterým se setkávají nebo čistá odrážka, která přesune ve směru, se kterým se setkávají lodě lodě místa. 

Rychlost, lze vypočítat koncepčně, tak, že první definování požadované rychlosti při zrušení otočený pak otáčení této rychlosti k úhlu, se kterým se setkávají entity. VYSVĚTLENÍ Tento koncept, rychlosti (a akcelerace) můžete vizualizovat jako 2dimenzí *vektoru* (což je obvykle vykresluje jako šipka). Vektor pro hodnotu rychlosti s X = 100 a Y = 0 můžete vizualizovat následujícím způsobem:

![](math-images/image7.png "Vektor pro hodnotu rychlosti s X = 100 a Y = 0 můžete vizualizovat následujícím způsobem")

Tento problém lze otáčet za následek nové rychlosti. Například otáčení vektoru o 45 stupňů (pomocí proti směru hodinových ručiček kolem) způsobí následující:

![](math-images/image8.png "Otáčení vektoru o 45 stupňů pomocí proti směru hodinových ručiček kolem výsledky v tomto")

Naštěstí rychlosti, akcelerace a to i v pozici lze všechny otáčet se stejným kódem bez ohledu na to, jak se používají hodnoty. Následující funkce pro obecné účely lze hodnotu CocosSharp otočení otočení vektor:


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

Úplné povědomí o `RotateVector` metoda vyžaduje, se seznamte s trigonometrické funkce kosinus a sinus společně s některé lineární algebra, což je nad rámec tohoto článku. Ale jednou implementována `RotateVector` metoda slouží k otočit žádné vektoru, včetně rychlosti vektoru. Například následující kód může aktivovat odrážku směrem určeného `rotation` hodnotu:


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

Tento kód může vytvořit něco podobného jako:

![](math-images/image9.png "Tento kód může vytvořit snímek podobný následujícímu")


## <a name="summary"></a>Souhrn

Tento průvodce popisuje běžné matematické koncepty v 2D vývoj her. Ukazuje, jak přiřadit a implementovat rychlosti a akcelerace a vysvětluje postup otočit objekty a vektory pro přesun vede libovolným směrem.
