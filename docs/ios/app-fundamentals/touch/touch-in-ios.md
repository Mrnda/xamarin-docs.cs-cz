---
title: Touch v iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 838a11f078d735759eda1d45a082ccbad51e2779
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="touch-in-ios"></a>Touch v iOS

Je důležité pochopit touch události a touch rozhraní API v aplikaci iOS, jako jsou důležitá pro všechny fyzické interakce s zařízení. Všechny interakce touch zahrnují `UITouch` objektu. V tomto článku jsme se dozvíte, jak používat `UITouch` třídy a jejích rozhraní API pro podporu dotykového ovládání. Později jsme rozšíří na našem znalostní báze se dozvíte, jak pro podporu gesta.

## <a name="enabling-touch"></a>Povolit dotykové ovládání

Ovládací prvky v `UIKit` – tyto rozčleněné z UIControl – jsou tak závislých na interakci s uživatelem, které mají gesta založená na UIKit a proto není nutné povolit dotykového ovládání. Je již povolen.

Ale řadu zobrazení `UIKit` nemají dotykového ovládání, které jsou ve výchozím nastavení povolené. Existují dva způsoby, jak povolit dotykového ovládání v ovládacím prvku. První způsob je zaškrtnutím políčka Povolit interakci uživatele v panelu pro vlastnost IOS Designer, jak je znázorněno na následujícím snímku obrazovky:

 [![](touch-in-ios-images/image1.png "Zaškrtněte políčko Povolit interakci uživatele v vlastnost pro IOS návrháře")](touch-in-ios-images/image1.png#lightbox)

Používáme řadič můžete také nastavit `UserInteractionEnabled` vlastnost na hodnotu true na `UIView` třídy. To je potřeba, pokud uživatelské rozhraní se vytvoří v kódu.

Následující řádek kódu je příklad:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Touch – události

Existují tři fáze dotykového ovládání, které dojít, když uživatel dotykem obrazovky, přesune jejich prstem nebo odebere jejich prstu. Tyto metody jsou definovány v `UIResponder`, což je základní třídu pro UIView. iOS se přepíše související metody na `UIView` a `UIViewController` pro zpracování touch:

-  `TouchesBegan` – To je volána, když je nejdříve změněny na obrazovce.
-  `TouchesMoved` – To je volána, když je umístění změny touch jako uživatel klouzavé jejich prsty, které po obrazovce.
-  `TouchesEnded` nebo `TouchesCancelled` – `TouchesEnded` je volána, když jsou odvolány prsty, které uživatele z obrazovky.  `TouchesCancelled` Získá volána, pokud iOS zruší touch – například pokud uživatel snímky své prstem směrem od tlačítko Zrušit stiskněte klávesu.


Touch události cesta rekurzivně přes sadu protokolů UIViews ke kontrole, pokud událost touch je v rámci hranice objekt zobrazení. To se často označuje jako _testování podle_. Bude být nejdříve volána na horní `UIView` nebo `UIViewController` a pak ji bude volána při `UIView` a `UIViewControllers` uvedenou pod nimi v hierarchii zobrazení.

A `UITouch` objekt se vytvoří pokaždé, když uživatel dotykem na obrazovce. `UITouch` Objekt zahrnuje data o dotykového ovládání, například když stiskem došlo k chybě, kde k němu došlo, pokud stiskem prstem atd. Události touch získat předána vlastnost úpravy – `NSSet` obsahující jeden nebo více úpravy. Můžeme pomocí této vlastnosti můžete získat odkaz na dotykového ovládání a určit aplikace odpovědi.

Třídy, které přepsat některá z událostí touch měli nejdřív volat základní implementaci a, získat `UITouch` objekt přidružený k události. Pokud chcete získat odkaz na první dotykového ovládání, volání `AnyObject` vlastnost a převést jej jako `UITouch` jako zobrazit v následujícím příkladu:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS automaticky rozpozná následných rychlé dotykem na obrazovce a bude shromažďovat všechny jako jedno klepnutí v jediném `UITouch` objektu. Díky tomu, že kontrola dvojité klepněte na stejně snadná jako kontroluje `TapCount` vlastnost, jak je znázorněno v následujícím kódu:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>Multi-Touch

Na ovládací prvky ve výchozím nastavení není povoleno více touch. Více touch lze povolit v iOS Designer, jak vidíte na následujícím snímku obrazovky:

 [![](touch-in-ios-images/image2.png "Více touch povolené v iOS návrháře")](touch-in-ios-images/image2.png#lightbox)

Je také možné nastavit více touch prostřednictvím kódu programu nastavením `MultipleTouchEnabled` vlastnost, jak je znázorněno na následujícím řádku kódu:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Chcete-li zjistit, kolik prsty dotýkal na obrazovce, použijte `Count` vlastnost `UITouch` vlastnost:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Určení umístění dotykového ovládání

Metoda `UITouch.LocationInView` vrátí objekt CGPoint, který obsahuje souřadnice touch v rámci dané zobrazení. Kromě toho, abychom mohli otestovat zjistit, jestli toto umístění v ovládacím prvku voláním metody `Frame.Contains`. Následující fragment kódu ukazuje příklad tohoto:

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Teď, když máme představu o události touch v iOS, seznámíme se nástroje pro rozpoznávání gesto.

## <a name="gesture-recognizers"></a>Nástroje pro rozpoznávání gesto

Nástroje pro rozpoznávání gesto může výrazně zjednodušit a snížit programovací úsilí na podporu touch v aplikaci. iOS gesto rozpoznávání agregovat řady událostí touch do jednoho touch událostí.

Xamarin.iOS poskytuje třídu `UIGestureRecognizer` jako základní třída pro následující předdefinované gesto rozpoznávání:

-  *UITapGesturesRecognizer* – je to pro jeden nebo více odposlouchávání.
-  *UIPinchGestureRecognizer* – Pinching a šíření prsty, které od sebe.
-  *UIPanGestureRecognizer* – klávesnicí nebo přetahování.
-  *UISwipeGestureRecognizer* – vede libovolným směrem k načtení.
-  *UIRotationGestureRecognizer* – otáčení dvěma prsty pohybem po směru hodinových ručiček nebo proti směru hodinových ručiček.
-  *UILongPressGestureRecognizer* – když stisknete a podržíte, označovaných také jako stiskněte dlouho nebo dlouho kliknutím.


Základní vzor pro použití funkce rozpoznávání gesto vypadá takto:

1.  **Vytvoření instance pro rozpoznávání gesto** – nejprve vytvořit instanci `UIGestureRecognizer` podtřídy. Objekt, který je vytvořena instance bude přidružené zobrazení a paměti shromáždí při zobrazení vyřazen. Není nutné vytvořit toto zobrazení jako proměnné úrovni třídy.
1.  **Konfigurovat žádné nastavení gesto** – dalším krokem je konfigurace gesto rozpoznávání rukopisu. V dokumentaci pro Xamarin `UIGestureRecognizer` a její podtřídy pro seznam vlastností, které lze nastavit pro řízení chování `UIGestureRecognizer` instance.
1.  **Nakonfigurujte cíl** – z důvodu jeho dědictví jazyka Objective-C není Xamarin.iOS vyvolávání událostí při rozpoznávání rukopisu gesto odpovídá gesto.  `UIGestureRecognizer` má metodu – `AddTarget` – který může přijmout delegáta anonymní nebo selektor jazyka Objective-C kódem má provést při rozpoznávání rukopisu gesto je nalezena shoda.
1.  **Povolit pro rozpoznávání gesto** – stejně jako s událostmi touch gesta pouze rozpoznává Pokud jsou povolené touch interakce.
1.  **Přidejte do zobrazení pro rozpoznávání gesto** – posledním krokem je přidání gesta do zobrazení voláním `View.AddGestureRecognizer` a předejte jí objekt gesto rozpoznávání rukopisu.

Odkazovat [gesty při rozpoznávání rukopisu ukázky](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) Další informace o tom, jak implementovat v kódu.

Když je volána gesto cíl, bude předáno odkaz na gesta, která došlo k chybě. Díky tomu cíl gesto ke získání informací o gesta, která došlo k chybě. Rozsah informace, které jsou k dispozici závisí na typu pro rozpoznávání gesta, která byla použita. Naleznete v dokumentaci pro Xamarin informace o datech, která je k dispozici pro každou `UIGestureRecognizer` podtřídy.

Je důležité si pamatovat, že jakmile rozpoznávání rukopisu gesto byl přidán do zobrazení, zobrazení (a všechny zobrazení pod ním) nebude přijímání událostí dotykového ovládání. Povolit dotykové ovládání události současně gest `CancelsTouchesInView` vlastnost musí být nastavena na hodnotu false, jak je znázorněno v následujícím kódu:

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Každý `UIGestureRecognizer` má stav vlastnost, která obsahuje důležité informace o stavu pro rozpoznávání gesto. Pokaždé, když se změní hodnota této vlastnosti, iOS bude volat metodu odběru, která poskytuje aktualizace. Pokud pro rozpoznávání vlastní gesto nikdy aktualizací vlastnost stavu, odběratele nikdy volána, vykreslování pro rozpoznávání gesto nepoužitelné.

Gesta jde vyhodnotit jako jeden ze dvou typů:

1.  *Diskrétní* – tyto gesta pouze ještě efektivněji první čas jejich rozpoznána.
1.  *Průběžné* – tyto gesta nadále fire tak dlouho, dokud se rozpoznána.


Nástroje pro rozpoznávání gesto existuje v jedné z následujících stavů:

-  *Možné* – Toto je počáteční stav všech gesto rozpoznávání. Toto je výchozí hodnota vlastnosti stavu.
-  *Began* – po první rozpoznání gesto průběžné stav je nastavený na Began. To umožňuje jako odběratel u rozlišit mezi při rozpoznávání gesto spuštění a kdy se změnil.
-  *Změněno* – po gesto průběžné zahájení, ale nebyl dokončen, stav je možnost změněno pokaždé, když touch přesune nebo se změní, dokud je stále v rámci očekávané parametry gesta.
-  *Zrušena* – tento stav se nastaví, když nástroj rozpoznávání rukopisu se z Began k změněno, a potom úpravy změnit tak, aby už se vzor gesta.
-  *Rozpoznat* – stav bude nastaven, při rozpoznávání rukopisu gesto odpovídá sadu úpravy a bude informovat odběratele, že gesta byla dokončena.
-  *Byl ukončen* – to je alias Recognized stavu.
-  *Nezdařilo se* – Pokud pro rozpoznávání gesto již není schopen úpravy naslouchá pro stav se změnil na neúspěšně.


Xamarin.iOS představuje v tyto hodnoty `UIGestureRecognizerState` výčtu.

## <a name="working-with-multiple-gestures"></a>Práce s více gesta

Ve výchozím nastavení iOS neumožňuje výchozí gesta můžou běžet současně. Místo toho každý gesto pro rozpoznávání obdrží touch události v pořadí od není deterministický. Následující fragment kódu ukazuje, jak provádět současně spustit nástroj pro rozpoznávání gesto:

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Je také možné zakázat gesto v iOS. Existují dvě vlastnosti delegáta, které umožňují gesto rozpoznávání a zkontrolujte stav aplikace a aktuálním touch události, aby rozhodnutí o tom, a pokud by měl být rozpoznány gesto. Jsou dvě události:

1.  *ShouldReceiveTouch* – tento delegát je volána těsně před pro rozpoznávání gesto je předán touch událostí a dává příležitost, zkontrolujte úpravy a rozhodnout, které úpravy bude zpracovávat gesto rozpoznávání rukopisu.
1.  *ShouldBegin* – to je volána, když rozpoznávání rukopisu se pokusí změnit stav z možných na jiný stav. Vrácení hodnoty false způsobí, že stav pro rozpoznávání gesto změnit tak, aby se nezdařilo.


Můžete také přepsat tyto metody se silnými typy `UIGestureRecognizerDelegate`, slabé delegáta nebo vazbu pomocí syntaxe obslužné rutiny události, které jsou popsány v následující fragment kódu:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Nakonec je možné fronty pro rozpoznávání gesto, tak, že bude úspěšné pouze pokud jiné pro rozpoznávání gesto selže. Například pro rozpoznávání jedním klepnutím gesto měli úspěšně pouze, pokud selže pro rozpoznávání gesto dvojité klepněte na. Následující fragment kódu obsahuje příklady:

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Vytváření vlastních gesto

I když iOS poskytuje určité výchozí gesto rozpoznávání, může být nutné vytvořit vlastní gesto rozpoznávání v určitých případech. Vytváření pro rozpoznávání vlastní gesto zahrnuje následující kroky:

1.  Podtřída `UIGestureRecognizer` .
1.  Přepsání metody odpovídající touch události.
1.  Vyvolat rozpoznávání stav přes vlastnost stavu základní třídy.


Praktické příklad tohoto objektu se budeme v [pomocí Touch v iOS](ios-touch-walkthrough.md) návod.
