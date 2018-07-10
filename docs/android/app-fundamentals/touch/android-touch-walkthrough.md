---
title: Návod – používání dotykového ovládání v Androidu
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/09/2018
ms.openlocfilehash: d379630e3b7fa2b42bd9530e1dccd75e9634dd2f
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935524"
---
# <a name="walkthrough---using-touch-in-android"></a>Návod – používání dotykového ovládání v Androidu

Dejte nám zjistit, jak použít koncepty z předchozí části funkční aplikaci. Vytvoříme aplikaci s čtyři aktivity. První aktivita bude nabídku nebo panel, který se spustí ostatní aktivity k předvedení různá rozhraní API. Následující snímek obrazovky ukazuje v hlavní aktivitě:

[![Ukázkový snímek pomocí Touch tlačítko](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

První aktivita ukázky dotykové vám ukáže, jak používat obslužné rutiny událostí pro zásahu do zobrazení. Aktivita pro rozpoznávání gest vám ukáže, jak podtřídy `Android.View.Views` a zpracování událostí a také ukazují, jak zpracovat gesta stažení. Třetí a poslední aktivita **vlastních gest**, bude zobrazit použití vlastních gest. Aby to bylo jednodušší sledovat a chránit před, jsme budete rozdělte tento návod na oddíly se každá část, zaměřuje se na jednu z aktivit.

## <a name="touch-sample-activity"></a>Aktivita ukázky dotykové ovládání

-   Otevřete projekt **TouchWalkthrough\_Start**. **MainActivity** je nastavené a můžete přejít &ndash; je až nám k implementaci dotykové ovládání chování v rámci aktivity. Pokud spustíte aplikaci a klikněte na tlačítko **ukázky dotykové**, by měl spustit následující aktivity:

    [![Snímek obrazovky aktivity Touch začíná zobrazí](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

-   Teď, když potvrdili jsme, že aktivity spuštění, otevřete soubor **TouchActivity.cs** a přidání obslužné rutiny `Touch` událost `ImageView`:

    ```csharp
    _touchMeImageView.Touch += TouchMeImageViewOnTouch;
    ```

-   Dále přidejte následující metodu do **TouchActivity.cs**:

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

Všimněte si, že ve výše uvedeném kódu umožňuje zacházet `Move` a `Down` akce za stejné. Důvodem je, že i když uživatel nemůže přenést jejich prstem `ImageView`, může pohybovat nebo tlak uživatel může změnit. Tyto typy změn bude generovat `Move` akce.

Pokaždé, když uživatel dnešní `ImageView`, `Touch` událost se vyvolá a naše obslužnou rutinu se zobrazí zpráva **Touch začíná** na obrazovce, jak je znázorněno na následujícím snímku obrazovky:

[![Snímek obrazovky aktivity Touch začíná](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

Za předpokladu, uživatel se dotýká `ImageView`, **Touch začíná** se zobrazí v `TextView`. Když se uživatel již dotýká `ImageView`, zprávy **Touch končí** se zobrazí v `TextView`, jak je znázorněno na následujícím snímku obrazovky:

[![Snímek obrazovky aktivity Touch končí](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)


## <a name="gesture-recognizer-activity"></a>Aktivita pro rozpoznávání gest

Teď umožňuje implementovat aktivitu pro rozpoznávání gest. Tato aktivita ukazuje, jak ukazuje jeden způsob, jak implementovat přiblížení roztažením a přetáhněte ho zobrazit na obrazovce.

-   Přidat novou aktivitu pro aplikaci s názvem `GestureRecognizer`.
    Upravte kód pro tuto aktivitu tak, aby vypadá podobně jako následující kód:

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

-   Přidat novou s Androidem zobrazit do projektu a pojmenujte ho `GestureRecognizerView`. Do této třídy, přidejte následující proměnné:

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

-   Přidejte následující konstruktor k `GestureRecognizerView`. Tento konstruktor se přidá `ImageView` do naší činnosti. V tomto okamžiku nebude nadále kompilovat kód &ndash; musíme vytvořit třídu `MyScaleListener` , který vám pomůže s změnu velikosti `ImageView` když ho uživatel pinches:

    ```csharp
    public GestureRecognizerView(Context context): base(context, null, 0)
    {
        _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
        _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
        _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
    }
    ```

-   Chcete-li nakreslit obrázek v naší činnosti, musíme přepsat `OnDraw` metoda třídy zobrazení, jak je znázorněno v následujícím fragmentu kódu. Tento kód se přesune `ImageView` na určené pozici `_posX` a `_posY` i tak, jak změnit velikost obrázku podle koeficient změny měřítka:

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

-   Dále musíme aktualizovat proměnnou instance `_scaleFactor` jako uživatel pinches `ImageView`. Přidáme třídu s názvem `MyScaleListener`. Tato třída bude naslouchat škálovací události, které bude Android vyvolána, když uživatel pinches `ImageView`.
    Přidat následující vnitřní třídu `GestureRecognizerView`. Tato třída je `ScaleGesture.SimpleOnScaleGestureListener`. Tato třída je třída pohodlí, naslouchacích procesů můžete podtřídy, pokud vás zajímají podmnožinu gesta:

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

            _view.Invalidate();
            return true;
        }
    }
    ```

-   Další metoda, která potřebujeme k přepsání v `GestureRecognizerView` je `OnTouchEvent`. Následující kód obsahuje úplný implementace této metody. Existuje velké množství kódu, proto umožňuje trvat několik minut a podívejte se, co se děje tady. První věc, kterou tato metoda provede se škálování na ikonu v případě potřeby &ndash; tento problém řeší pomocí volání `_scaleDetector.OnTouchEvent`. Dále jsme pokusí zjistit, jaká akce se volá tuto metodu:

    - Pokud uživatel dotyku obrazovky s, zaznamenejte jsme pozice X a Y a ID prvního ukazatel, který dotyku obrazovky.

    - Pokud se uživatel přesune jejich touch na obrazovce, potom jsme zjistěte, jak daleko uživatel přesune ukazatel myši.

    - Pokud uživatel má zrušeno jeho prstem mimo obrazovku, pak zastavíme sledování gest.

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

-   Nyní spusťte aplikaci a spuštění aktivit pro rozpoznávání gest.
    Při spuštění na obrazovce by měla vypadat podobně jako následující snímek obrazovky:

    [![Nástroj pro rozpoznávání gest úvodní obrazovkou s ikonou s Androidem](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

-   Nyní touch ikonu a přetáhněte ho na obrazovce. Zkuste gesta přiblížení roztažením. V určitém okamžiku vaše obrazovka může vypadat podobně jako na následujícím snímku obrazovky:

    [![Ikona přesunutí gesta po obrazovce](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

V tomto okamžiku byste měli sami pat na pozadí: přiblížení roztažením jste implementovali jenom v aplikaci pro Android. Provést rychlé přerušení a umožní přesunout na třetí a poslední aktivita v tomto názorném postupu &ndash; pomocí vlastních gest.

## <a name="custom-gesture-activity"></a>Aktivita vlastních gest

Poslední obrazovka v tomto názorném postupu bude používat vlastních gest.

Pro účely tohoto návodu, gesta knihovna již byla vytvořena pomocí gest nástroje a přidány do projektu v souboru **prostředky/nezpracované/gesta**. Pomocí této bit eliminuje spouštění umožňuje přinese je poslední aktivita v tomto návodu.

-   Přidejte do ní soubor rozložení **vlastní\_gesta\_layout.axml** do projektu s tímto obsahem. Projekt již má všechny image **prostředky** složky:

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

-   Dále přidejte do projektu novou aktivitu a pojmenujte ho `CustomGestureRecognizerActivity.cs`. Přidejte dvě instance proměnné do třídy, jak ukazuje následující dva řádky kódu:

    ```csharp
    private GestureLibrary _gestureLibrary;
    private ImageView _imageView;
    ```

-   Upravit `OnCreate` metodu na tuto aktivitu tak, že se podobá následující kód. Umožňuje trvat několik minut, který vysvětluje, co se děje v tomto kódu. První věc, kterou děláme, je vytvořit instanci `GestureOverlayView` a nastavte ji jako zobrazení kořenové aktivity.
    Můžeme také přiřadit obslužnou rutinu události pro `GesturePerformed` událost `GestureOverlayView`. Dále jsme rozšiřování soubor rozložení, který jste vytvořili a přidat jako podřízené zobrazení `GestureOverlayView`. Posledním krokem je inicializovat proměnnou `_gestureLibrary` a nahrajte soubor gesta z prostředků aplikace. Pokud z nějakého důvodu nelze načíst soubor gesta, není, které tato aktivita může dělat, tak, aby byl vypnutí:

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

-   Poslední věc musíme implementovat metodu `GestureOverlayViewOnGesturePerformed` jak je znázorněno v následujícím fragmentu kódu. Když `GestureOverlayView` zjistí gesto, zavolá zpět do této metody. První věc, kterou se pokusíme získat `IList<Prediction>` objekty, které odpovídají gesta voláním `_gestureLibrary.Recognize()`. K získání používáme hodně LINQ `Prediction` , který má nejvyšší skóre gesta.

    Pokud se žádný odpovídající gesta s vysokou dostatek skóre, pak obslužná rutina události ukončí bez teď zrovna nic nedělá. V opačném případě zkontrolujte název odhadu a změnit na obrázku se zobrazuje na základě názvu gesta:

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

-   Spusťte aplikaci a spustit vlastní nástroj pro rozpoznávání gest aktivity. By měl vypadat přibližně jako na následujícím snímku obrazovky:

    [![Snímek obrazovky s zkontrolujte bitové kopie](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

    Nyní nakreslit značku zaškrtnutí na obrazovce a rastrový obrázek se zobrazí by měla vypadat podobně jako, který je znázorněno v následující snímky obrazovky:

    [![Rozpozná vykresleného značku zaškrtnutí, značky zaškrtnutí](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

    A konečně kreslete scribble na obrazovce. Zaškrtávací políčko by měl změnit zpět na jeho původní image, jak je znázorněno v těchto snímků obrazovky:

    [![Scribble na obrazovce, původní obrázek se zobrazí.](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Teď máte znalosti o tom, jak integrovat dotyky a gesta v aplikaci pro Android pomocí Xamarin.Android.


## <a name="related-links"></a>Související odkazy

- [Android Touch spuštění (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch konečné (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
