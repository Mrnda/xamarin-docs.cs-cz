---
title: "Texture ukládání do mezipaměti pomocí CCTextureCache"
description: "Třída CCTextureCache na CocosSharp poskytuje standardní způsob, jak uspořádat, mezipaměť a uvolnit obsah. Je užitečné zejména pro velké hry, které nemusí zcela do paměti RAM, zjednodušuje proces seskupování a uvolnění textury vhodné."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 365e343a55a208b63f4dc52999e8857b5f0ec1f4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="texture-caching-using-cctexturecache"></a>Texture ukládání do mezipaměti pomocí CCTextureCache

_Třída CCTextureCache na CocosSharp poskytuje standardní způsob, jak uspořádat, mezipaměť a uvolnit obsah. Je užitečné zejména pro velké hry, které nemusí zcela do paměti RAM, zjednodušuje proces seskupování a uvolnění textury vhodné._

`CCTextureCache` Třída je nedílnou součást vámi vyžádaných vývoj her pro CocosSharp. Většina CocosSharp hry použití `CCTextureCache` objektu, i když není explicitně tolik CocosSharp metody používají interně *sdílené* texture mezipaměti.

Tento průvodce popisuje `CCTextureCache` a proto je důležité pro vývoj her. Konkrétně obsahuje:

 - Proč texture záleží ukládání do mezipaměti
 - Texture životnost
 - Pomocí SharedTextureCache
 - Opožděného načítání oproti předběžné načítání s AddImage
 - Uvolnění textury


# <a name="why-texture-caching-matters"></a>Proč Texture záleží ukládání do mezipaměti

Texture ukládání do mezipaměti je důležitý faktor v vývoj her pro načítání texture je časově náročná operace a textury vyžadovat značné množství paměti RAM při spuštění.

Stejně jako u jakékoli operace souboru načítání textury z disku může být nákladná operace. Texture načítání může trvat delší dobu, pokud soubor načítá vyžaduje zpracování, např. Probíhá dekomprimovat (stejně jako v případě pro bitové kopie, png a jpg). Ukládání do mezipaměti Texture můžete snížit počet pokusů, že aplikace musí načíst soubory z disku.

Jak je uvedeno nahoře, textury také zabírají velké množství paměti modulu runtime. Obrázek na pozadí, velikost, aby překlad zařízení typu iPhone 6 (1344 x 750) by například zabírají 4 MB paměti RAM – i když je soubor PNG pouze několika kilobajtů událostí velikost. Ukládání do mezipaměti Texture poskytuje způsob, jak sdílet texture odkazy v aplikaci a také snadný způsob, jak uvolnit veškerý obsah, když přechod mezi různých herní stavů.


# <a name="texture-lifespan"></a>Texture životnost

CocosSharp textury může uchovávat v paměti pro celou délku spuštění aplikace, nebo mohou být zkrátí. Chcete-li minimalizovat paměti by měla využití aplikace odstranění textury, když už nepotřebují. Samozřejmě to znamená, že může textury zrušen a znovu načíst na pozdější dobu, která můžete zvýšit časů načtení nebo narušit výkon při zatížení. 

Texture načítání často vyžaduje kompromis mezi časy využití a zatížení paměti nebo výkonu modulu runtime. Hry, které využívají malé množství paměti texture můžete ponechat všechny textury v paměti podle potřeby, ale větší hry muset uvolnit textury uvolnit potřebné místo.

Následující diagram znázorňuje jednoduché hry, která načte textury podle potřeby a zajišťuje jejich v paměti pro celou délku spuštění:

![](texture-cache-images/image1.png "Tento diagram zobrazuje jednoduchá hra, který načte textury podle potřeby a zajišťuje jejich v paměti pro celou délku provádění")

První dva řádky představují textury, které jsou potřeba okamžitě po provedení příslušnou hru. Následující tři řádky představují textury pro každou úroveň načíst podle potřeby.

Pokud hra byla velká dostatečně ho by nakonec načíst dostatek textury k vyplnění všech RAM poskytované zařízení a operačního systému. Chcete-li tento problém vyřešit, hru uvolnit texture data, když už ho nepotřebují. Například následující diagram znázorňuje hry, což uvolní Level1Texture, pokud již nepotřebujete, následně načte Level2Texture pro další úroveň. Konečným výsledkem je, že jenom tři textury jsou uložené v paměti v každém okamžiku: 

![](texture-cache-images/image2.png "Konečným výsledkem je, že jenom tři textury jsou uložené v paměti v každém okamžiku")


Na obrázku výše uvedeném označuje texture využití paměti může snížit uvolnění, že pokud se rozhodne přehrávač opakování úrovní to může vyžadovat další načítání časy. Je také vhodné poznamenat, že jsou textury UITexture a MainCharacter načíst a nikdy odpojeno. To znamená, že tyto textury jsou potřebné na všech úrovních, takže se vždycky nacházejí v paměti. 


# <a name="using-sharedtexturecache"></a>Pomocí SharedTextureCache

CocosSharp automaticky ukládá do mezipaměti textury při načítání je prostřednictvím `CCSprite` konstruktor. Například následující kód vytvoří pouze jedna instance texture:


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

Automaticky ukládá do mezipaměti CocosSharp `star.png` texture předejdete nákladné alternativní vytváření množství identické `CCTexture2D` instance. To lze provést `AddImage` volané na sdílenou `CCTextureCache` instance, konkrétně `CCTextureCache.SharedTextureCache.Shared`. Zjistit, jak `SharedTextureCache` slouží podíváme na následující kód, který je stejně jako volání `CCSprite` konstruktor s parametr řetězce:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` ověří, zda soubor argument (v tomto případě `star.png`) již byla načtena. Pokud ano, se vrátí instanci uloženou v mezipaměti. Pokud není, pak je načíst ze systému souborů, a odkaz na textury je uložen interně pro následné `AddImage` volání. Jinými slovy `star.png` bitové kopie je načteny pouze jednou a následující volání vyžadují žádné další disk přístupu nebo další texture paměti.


# <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Opožděného načítání vs. Předběžné načítání s AddImage

`AddImage` Umožňuje kód, který má být zapsán stejné zda požadovaný texture už je načtený, nebo ne. To znamená, že obsahu nebudou načteny, dokud ho nepotřebují; To však může také způsobit problémy s výkonem za běhu z důvodu nepředvídatelným obsah načítání.

Zvažte například hra, kde může být upgradována player zbraně. Při upgradu zbraně a střelami viditelně změní, což vede k nové textury používá. Pokud je obsah opožděné načíst pak textury přidružené k upgradované zbraní se nenačte původně, ale spíše v pozdějším čase, když přehrávač zakoupí upgrady. 

Tato mid hraní her načítání může způsobit HRA do *pop*, což je krátký ale znatelné zablokování při provádění. Chcete-li tomu zabránit, můžete kód předpovědi, které textury může být potřeba dopředu a předběžné načtení je. Například následující slouží k předem načíst textury:


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Toto předběžné načítání může mít za následek nevyužité paměti a může prodloužit dobu spuštění. Například Přehrávač může získat nikdy skutečně zapnutí reprezentována `powerup3.png` texture, takže bude možné zbytečně načíst. Samozřejmě to může být nutné náklady platit, aby se zabránilo potenciální pop v hraní her, takže je většinou nejlepší předběžného načítání obsahu, pokud se vejde do paměti RAM.


# <a name="disposing-textures"></a>Uvolnění textury

Pokud hra nevyžaduje další texture paměti, než je k dispozici na minimální specifikace zařízení pak textury nemusí být zrušen. Na druhé straně větší hry muset uvolnění paměti texture, aby uvolnil prostor pro nový obsah. Hry mohou například používat velké množství paměti ukládání textury pro prostředí. Když v prostředí se používá pouze v konkrétní úroveň pak měla by být uvolněn při ukončení úroveň.


## <a name="disposing-a-single-texture"></a>Uvolnění jeden textury

Odebrání jednoho texture nejprve vyžaduje volání `Dispose` metoda ručního odebrání z a `CCTextureCache`.

Následující ukazuje, jak chcete úplně odebrat pohyblivý symbol pozadí společně s jeho texture:


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Při práci s malým počtem textury, ale to se může stát k chybám při plánování práce s větší sady texture může být účinné přímo uvolnění textury.

Textury je možné seskupit do vlastní (nesdílené) `CCTextureCache` instance zjednodušit texture čištění.

Představte si třeba příklad tam, kde je obsah se předem načtou pomocí konkrétního úroveň `CCTextureCache` instance. `CCTextureCache` Instance může být definovaný ve třídě, definování úroveň (které mohou být `CCLayer` nebo `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

`levelTextures` Instance lze poté přednačtení textury specifické úrovně: 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Nakonec při ukončení úroveň, textury může být najednou prostřednictvím všechny uvolněné `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Metoda Dispose bude dispose všechny interní textury vymazání se množství paměti používané tyto textury. Kombinování `CCTextureCache.Shared` v úrovni nebo herní režimu konkrétních `CCTextureCache` instance výsledkem některé textury uložením přes celou hru a některé odpojení podle úrovně ukončení, podobně jako na obrázku uvedené na začátku tohoto průvodce: 

![](texture-cache-images/image2.png "Kombinování CCTextureCache.Shared se úrovně nebo herní režimu CCTextureCache instance výsledků v některé textury uložením přes celou hru a některé odpojení podle úrovně ukončení, podobně jako na obrázku uvedené na začátku tohoto průvodce")




# <a name="summary"></a>Souhrn

Tato příručka ukazuje, jak používat `CCTextureCache` třídy vyrovnávání výkonu modulu runtime a využití paměti. `CCTexturCache.SharedTextureCache` může být explicitně nebo implicitně používá k načtení a textury po celou dobu životnosti aplikace do mezipaměti, když `CCTextureCache` instance slouží k uvolnění textury ke snížení využití paměti.

## <a name="related-links"></a>Související odkazy

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
