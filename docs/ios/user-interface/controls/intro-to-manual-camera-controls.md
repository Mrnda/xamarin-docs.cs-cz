---
title: Ruční fotoaparát ovládacích prvků v Xamarin.iOS
description: Tento dokument popisuje, jak lze použít rozhraní AVFoundation iOS s Xamarin.iOS povolit ruční ovládací prvky. Ovládací prvky ruční fotoaparát umožňují uživatelům aktivního ovládacího prvku, vyvážení bílé a nastavení ohrožení.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a0f605a38117df87a03801c3b9d86b0b7361c232
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790822"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Ruční fotoaparát ovládacích prvků v Xamarin.iOS

Ovládací prvky fotoaparát ruční poskytované `AVFoundation Framework` v iOS 8, povolení mobilní aplikace, abyste mohli plnou kontrolu nad fotoaparát zařízení s iOS. Tato přesné úrovni řízení lze vytvořit profesionální úrovni fotoaparát aplikace a poskytnout umělcem složení tím, že postupně je upravujte parametry fotoaparát při pořizování stále bitovou kopii nebo video.

Tyto ovládací prvky může být užitečné také při vývoji aplikace scientific nebo průmyslové, výsledky jsou méně zaměřena správnost nebo kosmetické bitové kopie kde jsou více s ohledem na zvýraznění některé funkce nebo element převáděna bitové kopie.

## <a name="avfoundation-capture-objects"></a>Zachycení AVFoundation objekty

Jestli trvá videa nebo obrázky pomocí fotoaparátu v zařízení se systémem iOS, proces používá k zachycení tyto bitové kopie je z velké části stejný. To platí pro aplikace, které používají výchozí automatizované ovládacích prvků kamery nebo ta, která využít výhod nových ovládacích prvků ruční fotoaparát:

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation zaznamenat objekty – přehled")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Vstup je převzat ze `AVCaptureDeviceInput` do `AVCaptureSession` prostřednictvím `AVCaptureConnection`. Výsledkem je buď výstup jako stále bitovou kopii nebo jako datový proud videa. Celý proces řídí `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Ruční ovládacích prvcích

Pomocí nových rozhraní API poskytované iOS 8, aplikace může převzít kontrolu nad následující funkce fotoaparát:

-  **Ruční fokus** – tím, že se koncový uživatel převzít kontrolu nad fokus přímo, můžete aplikaci poskytuje větší kontrolu nad bitovou kopii provedena.
-  **Ruční ohrožení** – tím, že poskytuje Ruční kontrola nad expozici, můžete uživatelům poskytnout dává větší svobodu a mohly dosáhnout stylizované vzhledu aplikace.
-  **Ruční vyvážení bílé** – vyvážení bílé slouží k úpravě barvu v obrázku – často aby vypadal realistické. Různé zdroje světla mít jinou barvu teploty a fotoaparát nastavení používaná pro zachycení image je, upraví se pro tyto rozdíly. Znovu tím, že kontrola uživatele nad vyvážení bílé, mohou uživatelé provádět úpravy, které není možné automaticky.


Poskytuje rozšíření iOS 8 a procesu zachycení vylepšení existující iOS rozhraní API k poskytování této jemně odstupňovanou kontrolu nad bitovou kopii.

## <a name="bracketed-capture"></a>Zaznamenat v závorkách

V závorkách zaznamenání je založen na nastavení z ovládacích prvků fotoaparát ruční uvedené výše a umožňuje aplikaci zaznamenat časový okamžik, v mnoha různými způsoby.

Jednoduše řečeno, v závorkách zaznamenat je shluku stále bitových kopií vytvořených ve řadu nastavení z obrázku na obrázek.

## <a name="requirements"></a>Požadavky

K dokončení kroky uvedené v tomto článku se vyžadují následující text:

-  **Xcode 7 + a iOS 8 nebo novější** – společnosti Apple Xcode 7 a iOS 8 nebo novější rozhraní API muset být nainstalovaná a nakonfigurovaná na počítači pro vývojáře.
-  **Visual Studio pro Mac** – nejnovější verze sady Visual Studio pro Mac musí být nainstalováno a nakonfigurováno na zařízení uživatele.
-  **iOS 8 zařízení** – zařízení s iOS s nejnovější verzí systému iOS 8. Funkce fotoaparátu nelze testovat v simulátoru iOS.


## <a name="general-av-capture-setup"></a>Zachycení AV obecné nastavení

Pokud záznam videa na zařízení s iOS, je to některé obecné nastavení kód, který bude vždy požadován. Tato část se bude zabývat minimální instalace, která je požadována pro záznam videa z fotoaparát zařízení s iOS a zobrazení tohoto videa v reálném čase v `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Delegát vyrovnávací paměti ukázkový výstup

Jedním z nejdůležitějších věcí potřeby bude delegáta ke sledování ukázka výstupní vyrovnávací paměť a zobrazte obrázek převzatý z vyrovnávací paměti pro `UIImageView` v uživatelském rozhraní aplikace.

Následující rutina bude vyrovnávací paměť ukázka sledujte a aktualizujte uživatelské rozhraní:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

Pomocí této rutiny na místě `AppDelegate` můžete upravit tak, aby otevřete relaci zaznamenat AV na záznam živé video informačního kanálu.

### <a name="creating-an-av-capture-session"></a>Vytváření relaci AV zachycení

Relaci zachytávání AV se používá k řízení záznamu živého videa z fotoaparát zařízení s iOS a je potřeba získat video do aplikace pro iOS. Od příkladu `ManualCameraControl` ukázková aplikace používá relaci zachytávání v několika různých místech, bude nakonfigurován v `AppDelegate` a k dispozici v celé aplikaci.

Následujícím postupem změnit aplikace `AppDelegate` a přidání nezbytného kódu:


1. Dvakrát klikněte `AppDelegate.cs` soubor v Průzkumníku řešení otevřete pro úpravy.
1. Přidejte následující příkazy using do horní části souboru:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. Přidejte následující soukromé proměnné a počítané vlastnosti, které chcete `AppDelegate` třídy:
    
    ```
    #region Private Variables
    private NSError Error;
    #endregion
    
    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```
  
1. Přepsat metodu dokončení a změňte ji na:
    
    ```
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;
    
        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }
    
        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }
    
        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);
    
        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();
    
        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }
    
        // Attach input to session
        Session.AddInput (Input);
    
        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;
    
        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);
    
        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```  
  
1. Uložte změny do souboru.


S tímto kódem na místě můžete snadno implementovat ovládací prvky fotoaparát ruční experimenty a testování.

## <a name="manual-focus"></a>Ruční fokusu

Tím, že se koncový uživatel provést ovládací prvky typu fokus přímo, můžete aplikaci poskytují více uměleckého kontrolu nad bitovou kopii provedena.

Například můžete professional fotografa zmírnění fokus bitovou kopii k dosažení [Bokeh vliv](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Vliv Bokeh")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Nebo vytvořte [fokus pro vyžádání obsahu vliv](http://www.mediacollege.com/video/camera/focus/pull.html), jako například:

[![](intro-to-manual-camera-controls-images/image3.png "Vyžádání účinek fokusu")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Vědců nebo zapisovač lékařské aplikace aplikace chtít prostřednictvím kódu programu pohyb přehledu pro experimenty. V obou případech nového rozhraní API umožňuje provedena koncový uživatel nebo aplikace, abyste mohli řídit fokusu v době bitovou kopii.

### <a name="how-focus-works"></a>Jak funguje fokusu

Před hovoříte o podrobnosti o řízení fokusu v aplikaci IOS 8. Podívejme se rychlé na fungování fokusu v zařízení s iOS:

[![](intro-to-manual-camera-controls-images/image4.png "Jak funguje fokusu v zařízení se systémem iOS")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Lehký zadá přehledu fotoaparát v zařízení s iOS a se zaměřuje na senzor bitové kopie. Vzdálenost přehledu z ovládacích prvků senzor kde ústředním bodem (oblast, kde se zobrazí bitovou kopii výzkumných) je ve vztahu k senzoru. Tím dále přehledu pochází z senzoru, objekty vzdálenost pravděpodobně výzkumných a pravděpodobně výzkumných blíže, téměř objekty.

V zařízení se systémem iOS přehledu přesune blíže k nebo dále za senzoru magnets a pružiny. V důsledku toho přesné umístění přehledu není možné, jak se budou lišit od zařízení pro zařízení a může být ovlivněno parametry například orientaci zařízení nebo stáří zařízení a pružiny.

### <a name="important-focus-terms"></a>Důležité fokus podmínky

Při plánování práce s fokusem, existuje několik podmínky, které by měly být obeznámeni s vývojáři:

-  **Hloubka pole** – vzdálenost mezi objekty nejbližší a nejvíce fokus. 
-  **Makro** – to je konci spektra fokus a je nejblíže vzdálenost, kdy se můžou zaměřit přehledu.
-  **Infinity** – to je úplně konci spektra fokus a je nejvíce vzdálenost, kdy se můžou zaměřit přehledu.
-  **Vzdálenost Hyperfocal** – to je bod v spektra fokus, kde je nejvíce objekt v rámečku jenom na úplně konci fokus. Jinými slovy jde ústřední pozice, který maximalizuje Hloubka pole. 
-  **Objektivu pozice** – se co všechny výše uvedené prvky jiné podmínky. To je vzdálenost přehledu z senzoru a tím řadičem fokus.


Tyto podmínky a znalostní báze na paměti se dají implementovat nové ovládací prvky ruční fokusu v aplikaci pro systém iOS 8 úspěšně.

### <a name="existing-focus-controls"></a>Stávající fokus ovládací prvky

iOS 7 a dřívějších verzích, poskytuje existujících ovládacích prvků fokus prostřednictvím `FocusMode`vlastnost jako:

-   `AVCaptureFocusModeLocked` V jednom zaměření je uzamčené – fokus.
-   `AVCaptureFocusModeAutoFocus` – Fotoaparát přesune přehledu prostřednictvím všechny body ústředním, dokud ho najde sharp fokus a pak zůstává existuje.
-   `AVCaptureFocusModeContinuousAutoFocus` – Fotoaparát refocuses vždy, když zjistí podmínku se na fokus.


Existujících ovládacích prvků k dispozici také nastavit bodu zájmu prostřednictvím`FocusPointOfInterest` vlastnost, takže uživatel může klepnout zaměřit se na konkrétní oblast. Aplikace může také sledují pohyb přehledu systémem monitorování `IsAdjustingFocus` vlastnost.

Kromě toho byl poskytnut omezení rozsahu `AutoFocusRangeRestriction` vlastnost jako:

-   `AVCaptureAutoFocusRangeRestrictionNear` – Omezuje autofocus blízkým depths. Užitečné v situacích, například procházení kód QR nebo čárový kód.
-   `AVCaptureAutoFocusRangeRestrictionFar` – Omezuje autofocus vzdáleným depths. Užitečné v situacích, kdy jsou objekty, které jsou známé jako důležité v zobrazení of pole (například rámec okna).


Nakonec je `SmoothAutoFocus` vlastnost, která zpomaluje algoritmus automatického fokus a kroky v menší kroky k Vyhněte se přesouvání artefakty, když záznam videa.

### <a name="new-focus-controls-in-ios-8"></a>Nové ovládací prvky fokusu v iOS 8

Kromě funkcí již v iOS 7 a vyšší následující funkce jsou nyní k dispozici řízení fokusu v iOS 8:

-  Úplné ruční Kontrola přehledu pozice při zamykání fokus.
-  Klíč hodnota pozorování přehledu pozice v libovolném režimu fokus.


K implementaci funkce výše `AVCaptureDevice` třída změnila zahrnout jen pro čtení `LensPosition` vlastnost použít k získání aktuální pozici přehledu fotoaparát.

Provést ruční Kontrola přehledu pozice, musí být zařízení zachycení v detailním režimu uzamčena. Příklad:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked` Metoda zaznamenat zařízení slouží k úpravě pozice přehledu fotoaparát. Rutina volitelné zpětného volání může poskytnout získat oznámení, když se změna projeví. Příklad:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Jak je vidět ve výše uvedeném kódu, musí být zařízení zaznamenat uzamčen pro konfiguraci předtím, než můžete provést změnu v přehledu pozici. Platné hodnoty pozice přehledu jsou mezi 0,0 a 1,0.

### <a name="manual-focus-example"></a>Příklad ruční fokusu

S kódem obecné AV zachycení nastavení na místě `UIViewController` lze přidat do aplikace scénáře a nakonfigurovány takto:

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController můžete přidat do aplikace scénáře a nakonfigurovat, jak je vidět tady")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Zobrazení obsahuje následující hlavní prvky:

-  A `UIImageView` , se zobrazí video informačního kanálu.
-  A `UISegmentedControl` vybraný režim, se změní z automatické na uzamčen.
-  A `UISlider` které zobrazit a aktualizovat na aktuální pozici přehledu.


Následujícím postupem navázání řadiče zobrazení pro ovládací prvek fokus ručně:


1. Přidejte následující příkazy:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Přidejte následující soukromé proměnné:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```  
  
1. Přidejte počítaný následující vlastnosti:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Přepsání `ViewDidLoad` metoda a přidejte následující kód:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Přepsání `ViewDidAppear` metoda a přidejte následující příkaz a spusťte záznam při načtení zobrazení:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. S fotoaparátu v režimu automaticky posuvník se přesune automaticky jako fotoaparát upraví fokus:

    [![](intro-to-manual-camera-controls-images/image6.png "Posuvník se automaticky přesune jako fotoaparát upraví fokusu v této ukázkové aplikace")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Klepněte na segmentu uzamčen a přetáhněte ji pozice posuvníku upravíte pozice přehledu ručně:

    [![](intro-to-manual-camera-controls-images/image7.png "Ruční úprava pozice přehledu")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Zastavte aplikaci.


Výše uvedený kód ukazuje, jak sledovat pozice přehledu, když je kamera v automatickém režimu nebo použít k řízení polohy přehledu, pokud je v režimu uzamčen jezdce.

## <a name="manual-exposure"></a>Ruční ohrožení

Ohrožení odkazuje také průraznost bitové kopie relativně k také průraznost zdroj a je určen podle množství světla dotkne senzoru, jak dlouho a úrovni nárůst senzor (ISO mapování). Tím, že poskytuje Ruční kontrola nad expozici, můžete aplikaci poskytovat dává větší svobodu koncového uživatele a mohly dosáhnout stylizované vzhledu.

Použití ovládacích prvků ohrožení ruční, uživatel může trvat bitové kopie z unrealistically jasně tmavý a moody:

[![](intro-to-manual-camera-controls-images/image8.png "Obrázek znázorňující ohrožení z unrealistically jasně tmavý a moody vzorku")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Znovu to můžete provést automaticky pomocí programovací řízení pro scientific aplikace nebo prostřednictvím ruční ovládací prvky, které poskytuje uživatelské rozhraní aplikace. V obou případech nové iOS 8 rozhraní API ohrožení poskytují jemně odstupňovanou kontrolu nad nastavení fotoaparátu v ohrožení.

### <a name="how-exposure-works"></a>Jak funguje ohrožení

Před hovoříte o podrobnosti o řízení ohrožení v aplikaci IOS 8. Podívejme se rychlé v tom, jak funguje ohrožení:

[![](intro-to-manual-camera-controls-images/image9.png "Jak funguje ohrožení")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Jsou tři základní prvky, které spojit ke kontrole expozice:

-  **Přihrádku rychlost** – to je doba, která spouště fotoaparátu je otevřený umožníte light do senzoru fotoaparát. Kratší čas, kdy je otevřená, spouště fotoaparátu menší světlým je umožňují a úpravu bitová kopie je (méně rozostření pohybu). Další světlým je umožnit v a více pohybu rozostření, ke kterému dochází, tím déle spouště fotoaparátu je otevřeno.
-  **Mapování ISO** – to je termín vždy pouze vypůjčí na film fotografie a odkazuje na velkých a malých písmen chemikálií v film najevo. ISO nízké hodnoty v film mají menší intervalem a jemnějšího reprodukci barev; nízké hodnoty ISO na digitální senzorů mít menší senzor šumu ale menší také průraznost. ISO hodnota vyšší, tím světlejší bitové kopie, ale s další senzor šumu. "ISO" na digitální senzor se rozumí míra [elektronické nárůst](http://en.wikipedia.org/wiki/Gain), ne fyzických funkce. 
-  **Objektivu otvoru** – to je velikost otevírání přehledu. Na všech zařízeních s iOS Clona čočky vyřešen, takže se pouze dvě hodnoty, které lze upravit expozici expozice a ISO.


### <a name="how-continuous-auto-exposure-works"></a>Jak průběžné Auto funguje ohrožení

Před learning jak funguje ruční ohrožení, je správné je vhodné pochopit, jak průběžné Automatická expozice funguje v zařízení se systémem iOS.

[![](intro-to-manual-camera-controls-images/image10.png "Jak funguje průběžné Automatická expozice v zařízení se systémem iOS")](intro-to-manual-camera-controls-images/image10.png#lightbox)

Nejprve je automaticky ohrožení bloku, má úloha Výpočet ideální ohrožení a průběžně se předány měření statistiky. Tyto informace použije k výpočtu optimální kombinaci ISO a expozice získat scény dobře lit. Tento cyklus se označuje jako AE smyčky.

### <a name="how-locked-exposure-works"></a>Jak uzamčení funguje ohrožení

V dalším kroku se podíváme jak uzamčení funguje ohrožení zařízení se systémem iOS.

[![](intro-to-manual-camera-controls-images/image11.png "Jak uzamčeném ohrožení funguje v zařízení s iOS")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Znovu máte blok ohrožení automaticky, který se pokouší k výpočtu optimální iOS a hodnoty typu Duration. V tomto režimu je však bloku AE odpojen od modul měření statistiky.

### <a name="existing-exposure-controls"></a>Stávající ovládací prvky ohrožení

iOS 7 a vyšší, zadejte následující ovládací prvky existující ohrožení prostřednictvím `ExposureMode` vlastnost:

-   `AVCaptureExposureModeLocked` – Ukázky scény jednou a používá tyto hodnoty v rámci scény.
-   `AVCaptureExposureModeContinuousAutoExposure` – Ukázky scény nepřetržitě zajistit, že je dobře lit.


`ExposurePointOfInterest` Lze klepnout na vystavit scény výběrem cílový objekt ke zveřejnění na, a aplikace můžete monitorovat `AdjustingExposure` vlastnost zobrazíte, když probíhá úprava ohrožení.

### <a name="new-exposure-controls-in-ios-8"></a>Nové ovládací prvky ohrožení v iOS 8

Kromě funkcí již v iOS 7 a vyšší následující funkce jsou nyní k dispozici řízení ohrožení v iOS 8:

-  Plně ruční vlastní vystavení.
-  GET, sady a klíč hodnota sledovat IOS a expozice (doba trvání).


K implementaci funkce výše novou `AVCaptureExposureModeCustom` režimu byla přidána. Vlastní režim po fotoaparát v následující kód slouží k nastavení doby trvání ohrožení a ISO:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

V režimech automaticky a uzamčen můžete upravit aplikaci odchylka automatické ohrožení rutiny pomocí následujícího kódu:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Rozsahy minimální a maximální nastavení závisí na zařízení, které je aplikace spuštěna, tak se nikdy pevného kódují se. Místo toho používejte následující vlastnosti získat rozsahy minimální a maximální hodnota:

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Jak je vidět ve výše uvedeném kódu, musí být zařízení zaznamenat uzamčen pro konfiguraci předtím, než můžete provést změnu v ohrožení.

### <a name="manual-exposure-example"></a>Příklad ruční ohrožení

S kódem obecné AV zachycení nastavení na místě `UIViewController` lze přidat do aplikace scénáře a nakonfigurovány takto:

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController můžete přidat do aplikace scénáře a nakonfigurovat, jak je vidět tady")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Zobrazení obsahuje následující hlavní prvky:

-  A `UIImageView` , se zobrazí video informačního kanálu.
-  A `UISegmentedControl` vybraný režim, se změní z automatické na uzamčen.
-  Čtyři `UISlider` ovládací prvky, které budou zobrazovat a aktualizovat posun, doba trvání, ISO a odchylka.


Následujícím postupem navázání řadiče zobrazení pro ruční ovládacímu prvku expozice:


1. Přidejte následující příkazy:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Přidejte následující soukromé proměnné:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```  
  
1. Přidejte počítaný následující vlastnosti:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Přepsání `ViewDidLoad` metoda a přidejte následující kód:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;
    
        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;
    
        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });
    
            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });
    
            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });
    
            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        ISO.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        Bias.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Přepsání `ViewDidAppear` metoda a přidejte následující příkaz a spusťte záznam při načtení zobrazení:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. S kamera v režimu automaticky posuvníků se přesune automaticky jako fotoaparát upraví ohrožení:

    [![](intro-to-manual-camera-controls-images/image13.png "Posuvníků se automaticky přesune jako fotoaparát upraví ohrožení")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Klepněte na segmentu uzamčen a přetáhněte ji odchylka posuvníku upravíte odchylka automatické expozice ručně:

    [![](intro-to-manual-camera-controls-images/image14.png "Úprava odchylka automatické expozice ručně")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Klepněte na vlastní segmentu a přetáhněte jezdce doba trvání a ISO ručně řídit ohrožení:

    [![](intro-to-manual-camera-controls-images/image15.png "Přetáhněte jezdce doba trvání a ISO ručně řízení přístupu")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Zastavte aplikaci.


Ve výše uvedeném kódu má ukazuje postup monitorování nastavení ohrožení, v automatickém režimu se po kamera a jak pomocí posuvníků k řízení expozici, pokud je v režimech uzamčen nebo vlastní.

## <a name="manual-white-balance"></a>Ruční vyvážení bílé

Ovládací prvky vyvážení bílé umožňují uživatelům nastavit zůstatek colosr v obraze vzhled realističtější. Různé zdroje světla mít jinou barvu teploty a nastavení fotoaparát používá k zaznamenání bitové kopie, musí upraví se pro tyto rozdíly. Znovu tím, že kontrola uživatele nad vyvážení bílé udělat professional úpravy, které jsou k dosažení uměleckého důsledky nepodporující automatické rutiny.

[![](intro-to-manual-camera-controls-images/image16.png "Ukázka obrázek znázorňující vyvážení bílé ruční úpravy")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Například letní má blueish přetypování, zatímco žhavé indikátory wolframu využívají odstín teplejší, žlutý oranžová. ("Studených" barvy confusingly, mají vyšší teploty barva než "záložním" barvy. Barva teploty jsou fyzické míry, nikoli vnímání tomu).

Lidské paměti je velmi dobré v kompenzace rozdíly v teplota barev, ale toto je něco, co fotoaparátu nelze provést. Kamera funguje tak, že zvyšovat skóre barev v opačné spektra upravit pro barvu rozdíly.

Nové iOS 8 ohrožení rozhraní API umožňuje aplikaci převzít kontrolu nad proces a poskytují jemně odstupňovanou kontrolu nad nastavení vyvážení bílé kamery.

### <a name="how-white-balance-works"></a>Jak bílé vyrovnávání funguje

Před hovoříte o podrobnosti o řízení vyvážení bílé v aplikaci IOS 8. Podívejme rychlý přehled jak bílé vyrovnávání funguje:

Studie barva dojem [CIE 1931 RGB barva místa a CIE 1931 XYZ barevný prostor](http://en.wikipedia.org/wiki/CIE_1931_color_space) jsou první matematicky definovaných barev prostory. Mezinárodní Komise pro osvětlení (CIE) byly vytvořeny v 1931.

[![](intro-to-manual-camera-controls-images/image17.png "RGB CIE 1931 barevný prostor a CIE 1931 XYZ barevných místa")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Výše uvedené graf zobrazuje nám všechny barvy viditelné pro lidské oko z přímým blue k jasně zelená k jasně červeně. Libovolného bodu v diagramu mohou být vykreslena s hodnotou X a Y, jak je znázorněno v grafu výše.

Jako viditelná v grafu existují hodnoty X a Y, které mohou být vykreslena v grafu, která by byla mimo rozsah vize lidské a v důsledku těchto barvy nelze reprodukovat pomocí fotoaparátu.

Je volána menší křivky v tabulce výše [Planckian místo](http://en.wikipedia.org/wiki/Planckian_locus), která vyjadřuje teplotu barev (ve stupních kelvin), s vyšší čísla na modré straně (hotter) a nižší čísla na straně red (chladicí zařízení). Tyto jsou užitečné pro typické osvětlení situacích.

Ve smíšeném osvětlení podmínky bude nutné úpravy vyvážení bílé odchylují od místo Planckian provést požadované změny. V těchto situacích, které bude nutné úpravou být zapuštěno buď zeleným nebo red/purpurová straně CIE škálování.

Zařízení s iOS kompenzovat tím opačné nárůst barvu pro barvu přetypování. Například pokud scény má příliš mnoho blue, pak red nárůst bude možné boosted odpovídajícím způsobem. Tyto získat hodnoty jsou nastaveny pro konkrétní zařízení, takže jsou závislé na zařízení.

### <a name="existing-white-balance-controls"></a>Stávající vyvážení bílé ovládací prvky

iOS 7 a vyšší poskytuje následující vyrovnávání prázdné prvky prostřednictvím `WhiteBalanceMode` vlastnost:

-   `AVCapture WhiteBalance ModeLocked` – Ukázky scény jednou a pomocí těchto hodnot v rámci scény.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – Ukázky scény nepřetržitě zajistit jeho dobře vyrovnáváním.


A aplikace můžete monitorovat `AdjustingWhiteBalance` vlastnost zobrazíte, když probíhá úprava ohrožení.

### <a name="new-white-balance-controls-in-ios-8"></a>Nové ovládací prvky White vyrovnávání v iOS 8

Kromě funkcí již v iOS 7 a vyšší jsou následující funkce nyní k dispozici na ovládací prvek vyvážení bílé v iOS 8:

-  Získá plně Ruční kontrola zařízení RGB.
-  GET, sady a sledovat klíč-hodnota získá zařízení RGB.
-  Podpora pro vyvážení bílé pomocí karty šedá.
-  Rutiny převodu do a z zařízení nezávislé barevné prostory.


K implementaci funkce výše `AVCaptureWhiteBalanceGain` struktura byla přidána s následující členy:

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Získat maximální vyvážení bílé je aktuálně čtyři (4) a může být z připravené `MaxWhiteBalanceGain` vlastnost. Takže povolený rozsah je od jedna (1) do `MaxWhiteBalanceGain` (4) aktuálně.

`DeviceWhiteBalanceGains` Vlastnosti lze sledovat aktuální hodnoty. Použití `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` upravit rovnováhu mezi získá při kamera je v uzamčeném vyvážení bílé režimu.

#### <a name="conversion-routines"></a>Rutiny převodu

Rutiny převodu byly přidány do systému iOS 8, které pomáhají při převodu do a z zařízení nezávislé barevné prostory. K implementaci rutiny převodu `AVCaptureWhiteBalanceChromaticityValues` struktura byla přidána s následující členy:

-   `X` -je hodnotu od 0 do 1.
-   `Y` -je hodnotu od 0 do 1.


`AVCaptureWhiteBalanceTemperatureAndTintValues` Struktura také byla přidána s následující členy:

-   `Temperature` -je plovoucí bodu hodnota ve stupních Kelvin.
-   `Tint` -je posun od zelenou nebo fialové od 0 do 150 s kladné hodnoty směrem zelená směr a v purpurová negativní směrem k.


Použití `CaptureDevice.GetTemperatureAndTintValues`a `CaptureDevice.GetDeviceWhiteBalanceGains`metod pro převod mezi teploty a TINT –, barevnosti a RGB získat barevné prostory.

> [!NOTE]
> Rutiny převodu jsou přesnější, že Čím bližší je hodnota, která má být převeden na Planckian místo.




#### <a name="gray-card-support"></a>Podpora šedé karet

Apple používá k odkazování na podpora šedá karet, které jsou součástí iOS 8 termín šedá World. Umožňuje uživatelům zaměřit se na fyzickou kartu šedá, který obsahuje alespoň 50 % středu rámečku a použije ho k nastavit zůstatek prázdné. Účelem karty šedá je prázdné, který se zobrazí neutrální dosáhnout.

To může být implementována v aplikaci uživatele vyzve k umístit fyzickou kartu šedé před fotoaparátu, monitorování `GrayWorldDeviceWhiteBalanceGains` vlastnost a počkejte, až hodnoty vyrovnat.

Aplikace by pak zamknout zvýšení vyvážení bílé pro `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` metoda z hodnot `GrayWorldDeviceWhiteBalanceGains` vlastnosti tak, aby se změny projevily.

Zaznamenat zařízení musí být uzamčen pro konfiguraci předtím, než můžete provést změny v rovnováze prázdné.

### <a name="manual-white-balance-example"></a>Příklad Ruční vyvážení bílé

S kódem obecné AV zachycení nastavení na místě `UIViewController` lze přidat do aplikace scénáře a nakonfigurovány takto:

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController můžete přidat do aplikace scénáře a nakonfigurovat, jak je vidět tady")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Zobrazení obsahuje následující hlavní prvky:

-  A `UIImageView` , se zobrazí video informačního kanálu.
-  A `UISegmentedControl` vybraný režim, se změní z automatické na uzamčen.
-  Dva `UISlider` ovládací prvky, které budou zobrazovat a aktualizovat teploty a odstín.
-  A `UIButton` vzorků místo šedá karty (šedá World) a nastavit vyvážení bílé pomocí těchto hodnot.


Následujícím postupem navázání řadiče zobrazení pro ruční Kontrola zůstatku prázdné:


1. Přidejte následující příkazy:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Přidejte následující soukromé proměnné:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    #endregion
    ```
  
1. Přidejte počítaný následující vlastnosti:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Přidejte následující metodu privátní nastavit nové prázdné vyvážit teploty a TINT –:

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }
    
    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```   
  
1. Přepsání `ViewDidLoad` metoda a přidejte následující kód:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ``` 
  
1. Přepsání `ViewDidAppear` metoda a přidejte následující příkaz a spusťte záznam při načtení zobrazení:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Uložit změny kód a spusťte aplikaci.
1. S kamera v režimu automaticky posuvníků se přesune automaticky jako fotoaparát upraví vyvážení bílé:

    [![](intro-to-manual-camera-controls-images/image19.png "Posuvníků se automaticky přesune jako fotoaparát upraví vyvážení bílé")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Klepněte na segmentu uzamčen a přetáhněte jezdce dočasné a TINT – upravit vyvážení bílé ručně:

    [![](intro-to-manual-camera-controls-images/image20.png "Přetáhněte jezdce dočasné a TINT – ručně upravit vyvážení bílé")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Vybraný segmentu uzamčen umístěte fyzickou kartu šedé vpředu z fotoaparátu a klepněte na tlačítko šedá karty upravit vyvážení bílé World šedá:

    [![](intro-to-manual-camera-controls-images/image21.png "Klepněte na tlačítko šedá karty upravit vyvážení bílé šedá World")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Zastavte aplikaci.

Výše uvedený kód ukazuje, jak monitorovat nastavení vyvážení bílé, když fotoaparát v automatickém režimu nebo pomocí posuvníků ke kontrole bílé zůstatku, pokud je v režimu uzamčen.

## <a name="bracketed-capture"></a>Zaznamenat v závorkách

V závorkách zaznamenání je založen na nastavení z ovládacích prvků fotoaparát ruční uvedené výše a umožňuje aplikaci zaznamenat časový okamžik, v mnoha různými způsoby.

Jednoduše řečeno, v závorkách zaznamenat je shluku stále bitových kopií vytvořených ve řadu nastavení z obrázku na obrázek.

[![](intro-to-manual-camera-controls-images/image22.png "Jak funguje v závorkách zachycení")](intro-to-manual-camera-controls-images/image22.png#lightbox)

Použití v závorkách zachycení v iOS 8, aplikace můžete přednastavení řadu ovládacích prvků ruční fotoaparát, jeden příkaz vystavení a mít aktuální scény vrátit řadu bitových kopií pro každý z ruční přednastavení.

### <a name="bracketed-capture-basics"></a>Základní informace o zachycení v závorkách

V závorkách zaznamenat je opět shluku stále bitových kopií prováděné s různým nastavením z obrázku na obrázek. Typy v závorkách zachycení k dispozici jsou následující:

-  **Automaticky ohrožení závorky** – kde všechny Image mají rozmanitých odchylka velikost.
-  **Ruční závorky ohrožení** – Jestliže mají všechny Image rozmanitých expozice (doba trvání) a ISO velikost.
-  **Jednoduché závorky Burst** – řadu stále obrázky prováděné rychle za sebou.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>Nové v závorkách zaznamenat ovládací prvky v iOS 8

Všechny příkazy v závorkách zaznamenat jsou implementované v `AVCaptureStillImageOutput` třídy. Použití `CaptureStillImageBracket`metoda získat řadu bitové kopie s danou poli nastavení.

Byly implementovány dvě nové třídy pro zpracování nastavení:

-   `AVCaptureAutoExposureBracketedStillImageSettings` – Obsahuje jednu vlastnost `ExposureTargetBias`používané nastavit posun ohrožení závorky automaticky. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` – Má dvě vlastnosti `ExposureDuration` a `ISO`používané nastavit expozice a ISO pro ruční ohrožení závorky. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>Ovládací prvky v závorkách zachycení doporučené a nedoporučené postupy

#### <a name="dos"></a>Úkoly

Následuje seznam skutečností, které se má provést při použití v závorkách Capture ovládací prvky v iOS 8:

-  Příprava aplikace pro situaci nejhorší zachycení voláním `PrepareToCaptureStillImageBracket` metoda.
-  Předpokládejme, že se chystáte pocházet ze stejné sdílený fond vyrovnávací paměti ukázkové.
-  Chcete-li uvolnit paměť, která byla přidělena předchozím voláním Příprava, volání `PrepareToCaptureStillImageBracket` znovu a jeho odeslání pole jednoho objektu.


#### <a name="donts"></a>Proti

Následuje seznam skutečností, které by neměly být provést při použití v závorkách Capture ovládací prvky v iOS 8:

-  Nekombinujte v závorkách zachycení nastavení typy v jediné zachycení.
-  Nemáte požádat o víc než `MaxBracketedCaptureStillImageCount` obrázků v jediné zachycení.


### <a name="bracketed-capture-details"></a>Podrobnosti o zachycení v závorkách

Následující podrobnosti by měly vzít v úvahu při práci s v závorkách zachycení v iOS 8:

-  V závorkách nastavení dočasně změnit `AVCaptureDevice` nastavení.
-  Flash a stále nastavení ustálení bitové kopie jsou ignorovány.
-  Všechny Image musí používat stejný formát výstupu (jpeg, png, atd.)
-  Náhled videa může vyřadit rámce.
-  V závorkách zachycení je podporována na všech zařízeních, které jsou kompatibilní s iOS 8.


Tyto informace v paměti a Podívejme se na příklad použití v závorkách zachycení v iOS 8.

### <a name="bracket-capture-example"></a>Příklad zachycení závorky

S kódem obecné AV zachycení nastavení na místě `UIViewController` lze přidat do aplikace scénáře a nakonfigurovány takto:

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController můžete přidat do aplikace scénáře a nakonfigurovat, jak je vidět tady")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Zobrazení obsahuje následující hlavní prvky:

-  A `UIImageView` , se zobrazí video informačního kanálu.
-  Tři `UIImageViews` , se zobrazí výsledky zachytávání.
-  A `UIScrollView` určený video informační kanál a zobrazení výsledků.
-  A `UIButton` využít v závorkách zaznamenat s některými přednastavené nastaveními.


Následujícím postupem navázání řadiče zobrazení pro zachycení v závorkách:


1. Přidejte následující příkazy:

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```  
  
1. Přidejte následující soukromé proměnné:

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```    
  
1. Přidejte počítaný následující vlastnosti:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```  
  
1. Přidejte následující metodu privátní k vytvoření bitové kopie zobrazení požadované výstup:

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {
    
        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));
    
        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");
    
        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);
    
        // Add to scrolling view
        ScrollView.AddSubview (imageView);
    
        // Return new image view
        return imageView;
    }
    #endregion
    ```  
  
1. Přepsání `ViewDidLoad` metoda a přidejte následující kód:
    
    ```
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);
    
        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));
    
        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };
    
        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;
    
            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });
    
            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);
    
                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);
    
                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```
    
  
1. Přepsání `ViewDidAppear` metoda a přidejte následující kód:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
        }
    }
    
    ```  
    
1. Uložit změny kód a spusťte aplikaci.
1. Rámce scény a klepněte na tlačítko zaznamenat závorky:

    [![](intro-to-manual-camera-controls-images/image24.png "Rámce scény a klepněte na tlačítko zaznamenat závorky")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Prstem zprava doleva zobrazíte tři provedenou v závorkách zachycení bitové kopie:

    [![](intro-to-manual-camera-controls-images/image25.png "Prstem zprava doleva zobrazíte tři provedenou v závorkách zachycení bitové kopie")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Zastavte aplikaci.


Výše uvedený kód ukazuje, jak nakonfigurovat a provést automatické ohrožení v závorkách zachycení v iOS 8.

## <a name="summary"></a>Souhrn

V tomto článku jsme zahrnutých Úvod do o nové prvky fotoaparát ruční poskytované iOS 8 a zahrnutých základy toho, co dělají a jak pracují. Udělili jsme příklady ruční fokus, Ruční expozice a vyvážení bílé ručně. Nakonec jsme vám Dal příklad trvá, v závorkách zaznamenat použití dříve popsaných ovládacích prvků ruční fotoaparát

## <a name="related-links"></a>Související odkazy

- [ManualCameraControls (ukázka)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [Úvod do iOSu 8](~/ios/platform/introduction-to-ios8.md)
