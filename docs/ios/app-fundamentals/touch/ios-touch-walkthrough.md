---
title: Návod – pomocí Touch v iOS
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 58066ef0071c8105658f0d766e8f038b2bd3ddf2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough--using-touch-in-ios"></a>Návod – pomocí Touch v iOS

Tento návod ukazuje, jak napsat kód, který reaguje na různé druhy událostí dotykového ovládání. Každý příklad se nachází v samostatné obrazovky:

- [Touch – ukázky](#Touch_Samples) – jak reagovat na touch události.
- [Ukázky pro rozpoznávání gesty](#Gesture_Recognizer_Samples) – postup použití nástroje pro rozpoznávání integrované gesto.
- [Vlastní rozpoznávání rukopisu gesto ukázka](#Custom_Gesture_Recognizer) – jak vytvořit vlastní gesto rozpoznávání.

Každá část obsahuje pokyny pro zápis kódu od začátku.
[Spuštění ukázkového kódu](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) již obsahuje kompletní scénáře a nabídky obrazovky:

 [![](ios-touch-walkthrough-images/image3.png "Ukázka zahrnuje obrazovky nabídky")](ios-touch-walkthrough-images/image3.png#lightbox)

Postupujte podle pokynů níže přidejte kód do scénáře a další informace o různých typech touch události, které jsou k dispozici v iOS. Můžete taky otevřít [dokončení ukázkové](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final) zobrazíte vše funguje.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Touch – ukázky

V této ukázce jsme se ukazují některé touch rozhraní API. Použijte následující postup přidání je kód potřebný k implementaci touch události:


1. Otevřete projekt **Touch_Start**. Nejprve spusťte projekt zajistěte, aby vše, co je v pořádku a touch **Touch ukázky** tlačítko. (I když bude fungovat žádné tlačítek) byste měli vidět obrazovky, který je podobný následujícímu:
    
    [![](ios-touch-walkthrough-images/image4.png "Ukázkovou aplikaci spustit s mimo pracovní tlačítka")](ios-touch-walkthrough-images/image4.png#lightbox)


1. Upravte soubor **TouchViewController.cs** a přidejte následující proměnné na dvě instance do třídy `TouchViewController`:

    ```csharp 
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```


1. Implementace `TouchesBegan` metoda, jak je znázorněno v následujícím kódu:

    ```csharp 
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);
    
        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);
    
        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```
    
    Tato metoda funguje tak, že kontrola `UITouch` objektu a pokud existuje provedení několika akcí podle kde stiskem došlo k chybě:

    * _Uvnitř TouchImage_ – zobrazit text `Touches Began` v popisku a změňte bitovou kopii.
    * _Uvnitř DoubleTouchImage_ – změnit obrázek, pokud byla gesta poklepání zobrazí.
    * _Uvnitř DragImage_ – nastavte příznak označující, že byla spuštěna dotykového ovládání. Metoda `TouchesMoved` použije tento příznak k určení, zda `DragImage` by měl pohybovat po obrazovce, nebo Ne, jak jsme je vidět v dalším kroku.

    Ve výše uvedeném kódu pouze zabývá jednotlivé úpravy, je stále žádné chování Pokud uživatel přechází jejich prstem na obrazovce. Reagovat na přesun, implementovat `TouchesMoved` jak je znázorněno v následujícím kódu:

    ```csharp 
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    Tato metoda získá `UITouch` objekt a zkontroluje, kde došlo k chybě dotykového ovládání. Pokud touch došlo k chybě v `TouchImage`, pak text přesunout úpravy se zobrazí na obrazovce. 

    Pokud `touchStartedInside` hodnotu true, abychom věděli, že uživatel má jejich prstem `DragImage` a je pohyb. Kód přesune `DragImage` jako uživatel přesune jejich prstem po obrazovce.

1. Musíme zpracování případu, pokud uživatel zruší své prstem z obrazovky nebo iOS zruší událost dotykového ovládání. V takovém případě bude implementaci `TouchesEnded` a `TouchesCancelled` jak je uvedeno níže:

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);
    
        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }
    
    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```
    
    Obě tyto metody se obnoví `touchStartedInside` příznak na hodnotu false. `TouchesEnded` Odstraní také zobrazení `TouchesEnded` na obrazovce.

1. Na obrazovce Touch ukázky v tomto okamžiku je dokončena. Všimněte si, jak na obrazovce změní, když budete používat s jednotlivými bitové kopie, jak je znázorněno na následujícím snímku obrazovky:
        
    [![](ios-touch-walkthrough-images/image4.png "Počáteční obrazovky aplikace")](ios-touch-walkthrough-images/image4.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image5.png "Na obrazovce po uživatel nastavuje tažením tlačítka")](ios-touch-walkthrough-images/image5.png#lightbox)
 

<a name="Gesture_Recognizer_Samples" />

##  <a name="gesture-recognizer-samples"></a>Ukázky pro rozpoznávání gesto

[Předchozí části](#Touch_Samples) ukázal, jak lze pomocí touch události přetáhněte objekt po obrazovce.
V této části jsme se zbavit touch události a ukazují, jak použít následující nástroje pro rozpoznávání gesto:

-  `UIPanGestureRecognizer` Pro přetažení myší bitovou kopii po obrazovce.
-  `UITapGestureRecognizer` Reagovat na dvojité odposlouchávání na obrazovce.

Pokud spustíte [spuštění ukázkového kódu](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start) a klikněte na **ukázky pro rozpoznávání gesto** tlačítko, byste měli vidět následující obrazovka:

 [![](ios-touch-walkthrough-images/image6.png "Kliknutím na tlačítko ukázky pro rozpoznávání gesto zobrazí tuto obrazovku")](ios-touch-walkthrough-images/image6.png#lightbox)

Postupujte podle těchto kroků k implementaci gesto rozpoznávání:


1. Upravte soubor **GestureViewController.cs** a přidejte následující proměnnou instance:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Je nutné tuto proměnnou instance ke sledování předchozí umístění bitové kopie.
Budou používat pro rozpoznávání gesto panoramování `originalImageFrame` hodnotě k výpočtu posun potřeba ho překreslit bitovou kopii na obrazovce.

1. Přidejte následující metodu k řadiči:

    ```csharp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();
    
        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined
    
        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    Tento kód vytvoří `UIPanGestureRecognizer` instance a přidává ji k zobrazení.
Všimněte si, jsme přiřadit cíl gesto ve formě metodu `HandleDrag` – tato metoda je k dispozici v dalším kroku.

1. Pokud chcete implementovat HandleDrag, přidejte následující kód k řadiči:

    ```csharp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }
    
        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    Výše uvedený kód bude nejprve zkontrolujte stav pro rozpoznávání gesto a poté přesuňte bitovou kopii na obrazovce. S tímto kódem na místě řadičem může nyní podporovat, přetáhněte jeden obrázek po obrazovce.


1. Přidat `UITapGestureRecognizer` , změní se zobrazuje v DoubleTouchImage image. Přidejte následující metodu do `GestureViewController` řadiče:

    ```csharp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;
    
        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));
    
            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };
    
        tapGesture = new UITapGestureRecognizer(action);
    
        // Configure it
        tapGesture.NumberOfTapsRequired = 2;
    
        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    Je velmi podobný kód pro tento kód `UIPanGestureRecognizer` , ale místo použití delegáta pro cíl se používá `Action`. 

1. Poslední věcí, je potřeba udělat, je upravit `ViewDidLoad` tak, aby volá metody, kterou jsme právě přidali. Změna ViewDidLoad, takže je podobná následující kód:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        Title = "Gesture Recognizers";
    
        // Save initial state
        originalImageFrame = DragImage.Frame;
    
        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    Všimněte si také, že jsme inicializaci hodnoty `originalImageFrame`.


1. Spusťte aplikaci a interakci s dvě bitové kopie.
Na následujícím snímku obrazovky je příkladem tyto akce:
    
    [![](ios-touch-walkthrough-images/image7.png "Tento snímek obrazovky ukazuje interakce přetažení")](ios-touch-walkthrough-images/image7.png#lightbox)



<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Pro vlastní gesto rozpoznávání

V této části použijeme koncepty z předchozí části k sestavení pro rozpoznávání vlastní gesto. Pro vlastní gesto rozpoznávání bude podtřídy `UIGestureRecognizer`, bude rozpoznat když uživatel nakreslí "V" na obrazovce a potom přepněte rastrový obrázek. Na následujícím snímku obrazovky je příklad této obrazovce:

 [![](ios-touch-walkthrough-images/image8.png "Aplikaci rozpozná, když uživatel na obrazovce nakreslí "V"")](ios-touch-walkthrough-images/image8.png#lightbox)

Postupujte podle těchto kroků můžete vytvořit vlastní gesto rozpoznávání:


1. Přidejte novou třídu do projektu s názvem `CheckmarkGestureRecognizer`, vypadá podobně jako následující kód:

    ```csharp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion
    
            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();
    
                strokeUp = false;
                midpoint = CGPoint.Empty;
            }
    
            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }
    
            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }
    
                Console.WriteLine(base.State.ToString());
            }
    
            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);
    
                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }
    
                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Resetování metoda je volána, když `State` vlastnost se změní na `Recognized` nebo `Ended`. Toto je čas resetovat všechny vnitřní stav, nastavte v vlastní gesto rozpoznávání rukopisu.
Třída teď můžete začít pracovat, při příštím uživatel pracuje s aplikací a připravená na rozpozná gesta zkuste znovu.



1. Teď, když jsme jste definovali pro rozpoznávání vlastní gesto (`CheckmarkGestureRecognizer`) upravit **CustomGestureViewController.cs** souboru a přidejte následující dvě instance proměnné:

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Vytváření instancí a nakonfigurovat pro naše gesto rozpoznávání, přidejte k řadiči následující metodu:

    ```csharp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();
    
        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });
    
        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. Upravit `ViewDidLoad` tak, aby zavolá `WireUpCheckmarkGestureRecognizer`, jak ukazuje následující fragment kódu:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
    
        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Spusťte aplikaci a zkuste kreslení "V" na obrazovce. Měli byste vidět, že se obrázek zobrazuje změny, jak je vidět na následujících snímcích obrazovky:
    
    [![](ios-touch-walkthrough-images/image9.png "Tlačítko zaškrtnutí")](ios-touch-walkthrough-images/image9.png#lightbox)
    
    [![](ios-touch-walkthrough-images/image10.png "Tlačítko nezaškrtnuto")](ios-touch-walkthrough-images/image10.png#lightbox)



Výše tři části předvedenou různé způsoby, jak reagovat na touch události v iOS: pomocí touch události, integrované gesto rozpoznávání, nebo pomocí funkce rozpoznávání vlastní gesto.



## <a name="related-links"></a>Související odkazy

- [iOS Touch Start (ukázka)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS Touch konečné (ukázka)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
