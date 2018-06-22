---
title: Návod - pomocí Touch v Android
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: 625ba800ce498f80c0344c67e26bd79360de4002
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050556"
---
# <a name="walkthrough---using-touch-in-android"></a>Návod - pomocí Touch v Android

Dejte nám zjistit, jak použít koncepty z předchozí části v funkční aplikaci. Pomocí čtyř aktivity vytvoříme aplikaci. První aktivitu bude nabídky nebo přepínací panel, který se spustí ostatní aktivity k předvedení různých rozhraních API. Následující snímek obrazovky znázorňuje hlavní aktivity:

[![Příklad – snímek obrazovky s Touch tlačítko](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

První aktivitu Touch ukázka zobrazí používání obslužných rutin událostí pro dotykové ovládání zobrazení. Aktivita gesto rozpoznávání rukopisu se ukazují, jak podtřídami `Android.View.Views` a zpracování událostí a také ukazují, jak zpracovat roztahováním gesta. Třetí a poslední aktivitu, **vlastní gesto**, bude zobrazení jak používat vlastních gest. Aby usnadnily a postupujte podle vyrovná se se zatížením, jsme budete rozdělit tento návod na část se každá část zaměřený na jednu z aktivit.

## <a name="touch-sample-activity"></a>Vzorová aktivita dotykového ovládání

-   Otevřete projekt **TouchWalkthrough\_spustit**. **MainActivity** je nastavené přejdete &ndash; je až do us implementovat dotykového ovládání chování v aktivitě. Spusťte aplikaci a klikněte na tlačítko **Touch ukázka**, má následující aktivitu spuštění:

    [![Snímek obrazovky aktivity Touch začne zobrazí](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Teď, když bylo potvrzeno, že spuštění aktivity, otevřete soubor **TouchActivity.cs** a přidání obslužné rutiny `Touch` události `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Dál přidejte následující metodu do **TouchActivity.cs**:

    ```csharp
    private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
    {
        string message;
        switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
        {
            case MotionEventActions.Down:
            case MotionEventActions.Move:
            message = "Touch Begins";
            break;

            case MotionEventActions.Up:
            message = "Touch Ends";
            break;

            default:
            message = string.Empty;
            break;
        }

        _touchInfoTextView.Text = message;
    }
    ```

Ve výše uvedeném kódu všimněte, že jednáme `Move` a `Down` akci, jako stejný. Důvodem je, že i když uživatel nemusí navýšení jejich prstem `ImageView`, může pohyb nebo tlak vyvíjený uživatel může změnit. Tyto typy změn způsobí vygenerování `Move` akce.

Pokaždé, když uživatel úpravy `ImageView`, `Touch` událost se vyvolá a naše obslužná rutina zobrazí zprávu **Touch začne** na obrazovce, jak je znázorněno na následujícím snímku obrazovky:

[![Snímek obrazovky aktivity Touch začne](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Tak dlouho, dokud uživatel je klepnou `ImageView`, **Touch začne** se zobrazí v `TextView`. Když je uživatel již klepnou `ImageView`, zpráva **Touch končí** se zobrazí v `TextView`, jak je znázorněno na následujícím snímku obrazovky:

[![Snímek obrazovky aktivity Touch končí](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Gesto pro rozpoznávání aktivity

Teď umožňuje implementovat pro rozpoznávání gesto aktivity. Tato aktivita se ukazují, jak přetáhněte zobrazení po obrazovce a ilustrují, jeden způsob, jak implementovat roztahováním přiblížení.

-   Přidat novou aktivitu do aplikace s názvem `GestureRecognizer`.
    Upravte kód pro tuto aktivitu tak, aby je podobná následující kód:

    ```csharp
    public class GestureRecognizerActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            View v = new GestureRecognizerView(this);
            SetContentView(v);
        }
    }
    ```

-   Přidat nové Android zobrazit do projektu a pojmenujte ji `GestureRecognizerView`. Přidejte následující proměnné pro tuto třídu:

    ```csharp
    private static readonly int InvalidPointerId = -1;

    private readonly Drawable _icon;
    private readonly ScaleGestureDetector _scaleDetector;

    private int _activePointerId = InvalidPointerId;
    private float _lastTouchX;
    private float _lastTouchY;
    private float _posX;
    private float _posY;
    private float _scaleFactor = 1.0f;
    ```

-   Přidejte následující konstruktor `GestureRecognizerView`. Tento konstruktor přidá `ImageView` naše aktivity. V tomto okamžiku nebude stále zkompilovat kód &ndash; musíme vytvořit třídu `MyScaleListener` který vám pomůže s Změna velikosti `ImageView` když ho uživatel pinches:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Kreslení bitovou kopii na našem aktivity, musíme přepsat `OnDraw` metoda zobrazení třídy, jak je znázorněno v následujícím fragmentu kódu. Tento kód se přesune `ImageView` na pozici určeného `_posX` a `_posY` také jako změní velikost obrázku podle násobek velikosti:

    ```csharp
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        canvas.Save();
        canvas.Translate(_posX, _posY);
        canvas.Scale(_scaleFactor, _scaleFactor);
        _icon.Draw(canvas);
        canvas.Restore();
    }
    ```

-   Dále je potřeba aktualizovat proměnnou instance `_scaleFactor` jako uživatel pinches `ImageView`. Přidáme třídy s názvem `MyScaleListener`. Tato třída bude naslouchat škálování události, které bude vyvolána Android, když uživatel pinches `ImageView`.
    Přidejte následující vnitřní třídu do `GestureRecognizerView`. Tato třída je `ScaleGesture.SimpleOnScaleGestureListener`. Tato třída je třída pohodlí, moduly pro naslouchání můžete podtřídami, pokud vás zajímá podmnožinu gesta:

    ```csharp
    private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
    {
        private readonly GestureRecognizerView _view;

        public MyScaleListener(GestureRecognizerView view)
        {
            _view = view;
        }

        public override bool OnScale(ScaleGestureDetector detector)
        {
            _view._scaleFactor *= detector.ScaleFactor;

            // put a limit on how small or big the image can get.
            if (_view._scaleFactor > 5.0f)
            {
                _view._scaleFactor = 5.0f;
            }
            if (_view._scaleFactor < 0.1f)
            {
                _view._scaleFactor = 0.1f;
            }

            _iconview.Invalidate();
            return true;
        }
    }
    ```

-   Další metodou musíme potlačení v `GestureRecognizerView` je `OnTouchEvent`. Následující kód uvádí úplné implementace této metody. Existuje mnoho kódu tady, takže umožňuje trvat několik minut a podívejte se na co se děje sem. První věc nemá tato metoda je škálování na ikonu v případě potřeby &ndash; to se provádí volání `_scaleDetector.OnTouchEvent`. Potom jsme zkuste zjistit, jaké akce volat tuto metodu:

    - Pokud uživatel dotýkal na obrazovce s, jsme záznam pozice X a Y a Identifikátor první ukazatele, který dotýkal na obrazovce.

    - Pokud se uživatel přesune jejich touch na obrazovce, pak jsme rozmyslete si, jak daleko uživatel přesunout ukazatel.

    - Pokud uživatel má zrušeno jeho prstem z obrazovky, potom jsme se zastaví sledování gesta.

    ```csharp
    public override bool OnTouchEvent(MotionEvent ev)
    {
        _scaleDetector.OnTouchEvent(ev);

        MotionEventActions action = ev.Action & MotionEventActions.Mask;
        int pointerIndex;

        switch (action)
        {
            case MotionEventActions.Down:
            _lastTouchX = ev.GetX();
            _lastTouchY = ev.GetY();
            _activePointerId = ev.GetPointerId(0);
            break;

            case MotionEventActions.Move:
            pointerIndex = ev.FindPointerIndex(_activePointerId);
            float x = ev.GetX(pointerIndex);
            float y = ev.GetY(pointerIndex);
            if (!_scaleDetector.IsInProgress)
            {
                // Only move the ScaleGestureDetector isn't already processing a gesture.
                float deltaX = x - _lastTouchX;
                float deltaY = y - _lastTouchY;
                _posX += deltaX;
                _posY += deltaY;
                Invalidate();
            }

            _lastTouchX = x;
            _lastTouchY = y;
            break;

            case MotionEventActions.Up:
            case MotionEventActions.Cancel:
            // We no longer need to keep track of the active pointer.
            _activePointerId = InvalidPointerId;
            break;

            case MotionEventActions.PointerUp:
            // check to make sure that the pointer that went up is for the gesture we're tracking.
            pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
            int pointerId = ev.GetPointerId(pointerIndex);
            if (pointerId == _activePointerId)
            {
                // This was our active pointer going up. Choose a new
                // action pointer and adjust accordingly
                int newPointerIndex = pointerIndex == 0 ? 1 : 0;
                _lastTouchX = ev.GetX(newPointerIndex);
                _lastTouchY = ev.GetY(newPointerIndex);
                _activePointerId = ev.GetPointerId(newPointerIndex);
            }
            break;

        }
        return true;
    }
    ```

-   Nyní spusťte aplikaci a spusťte pro rozpoznávání gesto aktivity.
    Když se spustí na obrazovce by měl vypadat podobně jako na následující snímek obrazovky:

    [![Gesto pro rozpoznávání úvodní obrazovka ikonou Android](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Nyní touch ikonu a přetáhněte ji na obrazovce. Zkuste gesto roztahováním přiblížení. V určitém okamžiku obrazovky může vypadat podobně jako na následujícím snímku obrazovky:

    [![Ikona přesunutí gesta na obrazovce](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

V tomto okamžiku je třeba přiřadit sami pat na pozadí: jste implementovali roztahováním přiblížení jenom v aplikaci Android! Zalomení rychlý a přejít k třetí a poslední aktivita v tomto návodu &ndash; pomocí vlastních gest.

## <a name="custom-gesture-activity"></a>Vlastní gesto aktivity

Poslední obrazovka v tomto názorném postupu bude používat vlastních gest.

Pro účely tohoto návodu knihovně gesta již byla vytvořena pomocí nástroje gesto a přidat do projektu v souboru **prostředky nebo nezpracovanou nebo gesta**. Pomocí tohoto bitu spouštění stranou umožňuje získat na s poslední aktivita v návodu.

-   Přidejte soubor rozložení s názvem **vlastní\_gesto\_layout.axml** do projektu s tímto obsahem. Projekt už má všechny bitové kopie **prostředky** složky:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
        <ImageView
            android:src="@drawable/check_me"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="3"
            android:id="@+id/imageView1"
            android:layout_gravity="center_vertical" />
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
    </LinearLayout>
    ```

-   V dalším do projektu přidejte nová aktivita a pojmenujte ji `CustomGestureRecognizerActivity.cs`. Přidejte dvě proměnné instance do třídy, jako zobrazuje v následující dva řádky kódu:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Upravit `OnCreate` metoda tohoto aktivity, které se podobá následující kód. Umožňuje trvat několik minut, který vysvětluje, co se děje v tomto kódu. První věc provedeme, je vytvoření instance `GestureOverlayView` a nastavte ho jako kořenovému zobrazení aktivity.
    Můžeme také přiřadit obslužnou rutinu události a `GesturePerformed` události `GestureOverlayView`. V dalším jsme zvýšilo rozložení souboru, který byl vytvořen již dříve a přidat jako podřízené zobrazení `GestureOverlayView`. Posledním krokem je můžete inicializovat `_gestureLibrary` a nahrajte soubor gesta z prostředky aplikace. Pokud z nějakého důvodu nelze načíst soubor gesta, není, které můžete provést tuto aktivitu, tak, aby byl vypnutí:

    ```csharp
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
        SetContentView(gestureOverlayView);
        gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

        View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
        _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
        gestureOverlayView.AddView(view);

        _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
        if (!_gestureLibrary.Load())
        {
            Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
            Finish();
        }
    }
    ```

-   Poslední věcí, musíme implementovat metodu `GestureOverlayViewOnGesturePerformed` jak je znázorněno v následující fragment kódu. Když `GestureOverlayView` zjistí gesto, zavolá zpátky k této metodě. První věc, kterou jsme pokusí získat `IList<Prediction>` objekty, které odpovídají gesta voláním `_gestureLibrary.Recognize()`. Používáme kousek LINQ získat `Prediction` který má nejvyšší skóre gesta.

    Pokud se žádné odpovídající gesty s vysokou dostatek skóre a potom obslužné rutiny události ukončí bez jakékoli akce. V opačném případě zkontrolujte název předpovědi a změnit bitovou kopii se zobrazuje na základě názvu gesta:

    ```csharp
    private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
    {
        IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
        orderby p.Score descending
        where p.Score > 1.0
        select p;
        Prediction prediction = predictions.FirstOrDefault();

        if (prediction == null)
        {
            Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
            return;
        }

        Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

        if (prediction.Name.StartsWith("checkmark"))
        {
            _imageView.SetImageResource(Resource.Drawable.checked_me);
        }
        else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
        {
            // Match one of our "erase" gestures
            _imageView.SetImageResource(Resource.Drawable.check_me);
        }
    }
    ```

-   Spusťte aplikaci a spuštění pro rozpoznávání gesto vlastní aktivity. Měl by vypadat nějak podobně jako na následujícím snímku obrazovky:

    [![Snímek obrazovky s zkontrolujte bitové kopie](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Nyní kreslení zaškrtnout na obrazovce a rastrový obrázek se zobrazuje by měl vypadat podobně jako zobrazená na další snímky obrazovky:

    [![Je rozpoznán vykresleného zaškrtnutí, zaškrtnutí](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    Nakonec kreslení scribble na obrazovce. Zaškrtávací políčko měli změnit zpátky na jeho původní image, jak je znázorněno v tyto snímky obrazovky:

    [![Scribble na obrazovce, původní image se zobrazí.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Nyní máte představu o tom, jak integrovat touch a gest v Android aplikace Xamarin.Android pomocí.


## <a name="related-links"></a>Související odkazy

- [Android Touch Start (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch konečné (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
