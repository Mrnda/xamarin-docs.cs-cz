---
title: Touch v Android
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: f1ccc86a20cb441bfdda864b8c7e263a691f5ab7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="touch-in-android"></a>Touch v Android

Podobně jako iOS, Android vytvoří objekt, který uchovává data o fyzické interakce uživatele s obrazovky &ndash; `Android.View.MotionEvent` objektu. Tento objekt obsahuje data, jako je například jaké akce se provádí, kde stiskem trvalo umístit, kolik přetížení bylo použito, atd. A `MotionEvent` objekt rozpis Přesun do na následující hodnoty:

-  Kód akce, která odpovídá typu pohybu, jako je například počáteční touch touch přesouvání mezi obrazovky nebo ukončování dotykového ovládání.

-  Sadu osy hodnoty, které popisují pozici `MotionEvent` a dalších vlastností přesun například kde stiskem probíhá, když někdo touch a kolik přetížení byl použit.
   Hodnoty osy může lišit v závislosti na zařízení, tak z předchozího seznamu nepopisuje všechny hodnoty osy.


`MotionEvent` Objektu se předá odpovídající metody v aplikaci. Pro aplikace pro Xamarin.Android reagovat na událost touch třemi způsoby:

-  *Přiřadit obslužné rutiny události pro `View.Touch`*  – `Android.Views.View` třída má `EventHandler<View.TouchEventArgs>` aplikace, které můžete přiřadit aby obslužná rutina. Toto chování je obvyklé .NET.

-  *Implementace `View.IOnTouchListener`*  -instance tohoto rozhraní může být přiřazen objekt zobrazení pomocí zobrazení. `SetOnListener` Metoda. Toto je funkčně srovnatelný obslužné rutiny události pro přiřazení `View.Touch` událostí. Pokud některé běžné nebo sdíleného logiky, která mnoho různých zobrazení může být nutné po dotyku, bude efektivnější vytvořit třídu a implementujte tuto metodu, než se přiřadit každého zobrazení vlastní obslužné rutiny události.

-  *Přepsání `View.OnTouchEvent`*  -všech zobrazení v Android podtřídami `Android.Views.View`. Při zobrazení je dotýkal, Android zavolá `OnTouchEvent` a předejte ji `MotionEvent` objektu jako parametr.


> [!NOTE]
> Ne všechna zařízení se systémem Android podporují dotykové obrazovky. 

Přidání následující značky k souboru manifestu způsobí, že Google Play k zobrazení pouze taková zařízení, která jsou povolena touch v aplikaci:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>Gesta

Gesto je vykreslen ruční tvar na dotykovou obrazovku. Gesto může mít jeden nebo více tahy, každý tahu skládající se z posloupnost body vytvořené do jiného bodu v kontaktu s obrazovky. Android může podporovat mnoho různých typů gesta, od jednoduchého fling po obrazovce pro komplexní gesta, které zahrnují více touch.

Poskytuje Android `Android.Gestures` obor názvů speciálně pro správu a odpovídá na požadavky gesta. Na srdcem všechny gesta je speciální třídu s názvem `Android.Gestures.GestureDetector`. Jak již název napovídá, bude tato třída naslouchání gesta a události na základě `MotionEvents` součástí operačního systému.

K implementaci gesto detektor, musí vytvořit instanci aktivitu `GestureDetector` třídy a poskytovat instance `IOnGestureListener`, které jsou popsány v následující fragment kódu:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

Aktivita musí také implementovat OnTouchEvent a předat MotionEvent detektor gesto. Následující fragment kódu ukazuje příklad tohoto:

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

Pokud instance `GestureDetector` identifikuje gesto zájmu, zobrazí se upozornění aktivity nebo aplikace pomocí vyvolání události nebo prostřednictvím zpětné volání poskytované `GestureDetector.IOnGestureListener`.
Toto rozhraní obsahuje šest metody pro různé gesta:

-  *OnDown* -volána, když dojde k klepněte na, ale se neuvolní.

-  *OnFling* -volána, když dojde k fling a poskytuje data na začátku a konce dotykového ovládání, který spustil událost.

-  *OnLongPress* -volána, když dojde k dlouhé stisknutím klávesy.

-  *OnScroll* -volána, když dojde k události scroll.

-  *OnShowPress* – volána po došlo OnDown a přesunutí nebo si událost nebyla provedena.

-  *OnSingleTapUp* -volána, když dojde k jedním klepnutím.


V mnoha případech může být zájem o podmnožinu gesta pouze aplikace. V takovém případě by měla aplikace rozšířit třídu GestureDetector.SimpleOnGestureListener a přepsání metody, které odpovídají události, které mají zájem.

## <a name="custom-gestures"></a>Vlastních gest

Gesta jsou skvělý způsob, jak uživatelům interakci s aplikací. Rozhraní API, které jsme viděli jste, pokud by stačit pro jednoduchá gesta, ale může prokázat pro složitější gesta připadat obtížné. Abyste složitější gest Android poskytuje další sadu rozhraní API Android.Gestures obor názvů, který bude usnadňují některé zatížení přidružené vlastních gest.

### <a name="creating-custom-gestures"></a>Vytváření vlastních gest

Od verze Android 1.6 SDK pro Android dodává s předinstalovaným v emulátoru názvem gesta Tvůrce aplikací. Tato aplikace umožňuje vývojáři k vytvoření předem definované gesta, která může být vložen do aplikace. Následující snímek obrazovky ukazuje příklad gesta Tvůrce:

[![Snímek obrazovky gesta Tvůrce gest příklad](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

Vylepšené verzi této aplikaci s názvem gesto nástroj lze nalézt Google Play. Gesto nástroj je hodně podobá gesta Tvůrce s tím rozdílem, že ji můžete otestovat gesta po jejich vytvoření. Tato další snímek obrazovky ukazuje gesta Tvůrce:

[![Snímek obrazovky gesto nástroj gest příklad](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

Nástroj gesto bitu užitečnější pro vytváření vlastních gest jako umožňuje gesta, která má být testována jako během vytváření a je snadno dostupné prostřednictvím webu Google Play.

Gesto nástroj umožňuje že vytvořit gesto kreslení na obrazovce a přiřazením název. Po vytvoření gesta jsou uloženy v binárním souboru na kartu SD zařízení. Tento soubor musí být načtena ze zařízení, a poté zabalené pomocí aplikace v /Resources/raw složky. Tento soubor můžete načíst z emulátoru serveru pomocí most ladění Android. Následující příklad ukazuje kopírování souboru z Galaxy Nexus k adresáři prostředků aplikace:

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

Po načtení souboru musí být zabalené s vaší aplikací uvnitř directory /Resources/raw. Nejjednodušší způsob, jak používat tento soubor gesto je načíst soubor do GestureLibrary, jak je znázorněno v následujícím fragmentu kódu:

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>Pomocí vlastních gest

Chcete-li rozpoznat vlastních gest v aktivitě, musí mít Android.Gesture.GestureOverlay objekt přidán do jeho rozložení. Následující fragment kódu ukazuje, jak programově přidat GestureOverlayView do aktivity:

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

Následující fragment kódu XML ukazuje, jak přidat GestureOverlayView deklarativně:

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

`GestureOverlayView` Má několik událostí, které během procesu kreslení gesto, bude vyvolána. Většina zajímavé událost `GesturePeformed`. Tato událost se vyvolá, když uživatel dokončil kreslení jejich gesto.

Pokud tato událost je aktivována, požádá aktivity `GestureLibrary` a zkuste to odpovídat gesto uživatele s jedním z gesta vytvořit nástrojem gesto. `GestureLibrary` Vrátí seznam objektů předpovědi.

Každý objekt předpovědi obsahuje skóre a název jednoho gest v `GestureLibrary`. Tím vyšší skóre, je pravděpodobnější gesto s názvem v předpovědi odpovídá gesto vykreslované uživatelem.
Skóre nižší než 1.0 obecně řečeno, jsou považovány za špatné odpovídá.

Následující kód ukazuje příklad odpovídajících gesto:

```csharp
private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
{
    // In this example _gestureLibrary was instantiated in OnCreate
    IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
    orderby p.Score descending
    where p.Score > 1.0
    select p;
    Prediction prediction = predictions.FirstOrDefault();

    if (prediction == null)
    {
        Log.Debug(GetType().FullName, "Nothing matched the user's gesture.");
        return;
    }

    Toast.MakeText(this, prediction.Name, ToastLength.Short).Show();
}
```

To provést byste měli mít představu o tom, jak používat dotykové ovládání a gest v aplikaci Xamarin.Android. Dejte nám teď přejít k návod a zobrazit všechny koncepty v ukázkové aplikaci práci.



## <a name="related-links"></a>Související odkazy

- [Android Touch Start (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch konečné (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
