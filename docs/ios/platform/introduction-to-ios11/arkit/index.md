---
title: Úvod do ARKit v Xamarin.iOS
description: Tento dokument popisuje rozšířená skutečnost v iOS 11 s ARKit. Popisuje, jak přidat 3D model do aplikace, konfigurace zobrazení, implementovat delegáta relace, umístěte 3D modelu na světě a pozastavení rozšířená skutečnosti relace.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 55ef2004f66cb808f878b2215dfdd59a45015877
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787162"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Úvod do ARKit v Xamarin.iOS

_Rozšířená skutečnost pro iOS 11_

ARKit umožňuje širokou škálu rozšířená skutečnosti aplikace a hry. Tato část obsahuje následující témata:

- [Začínáme s ARKit](#gettingstarted)
- [Pomocí ARKit UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Začínáme s ARKit

Začít s rozšířená skutečnosti podle následujících pokynů provede jednoduchou aplikaci: umístění 3D modelu, takže ARKit zachovat modelu v místě s jeho funkce sledování.

![3D modelu Jet číslo s plovoucí čárkou s vyloučením bitové kopie](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Přidání 3D modelu

Prostředky musí být přidaní do projektu s **SceneKitAsset** akce sestavení.

![SceneKit prostředky v projektu](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Konfigurace zobrazení

V kontroleru zobrazení `ViewDidLoad` metoda, asset scény načíst a nastavit `Scene` vlastnost v zobrazení:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Volitelně můžete implementovat delegáta relace

Ačkoli to není požadováno pro jednoduché případy, implementace delegáta relace může být užitečné při ladění stavu relace ARKit (a v aplikacích skutečné poskytnutí zpětné vazby pro uživatele). Vytvoření jednoduchého delegáta pomocí následující kód:

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

Přiřadit delegáta v v `ViewDidLoad` metoda:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Pozice 3D modelu na světě.

V `ViewWillAppear`, následující kód vytvoří relaci ARKit a nastaví pozici 3D modelu v prostoru relativně k fotoaparátu zařízení:

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

Pokaždé, když je aplikace spustit nebo obnovit, 3D modelu bude umístěna před fotoaparát. Jakmile je nastavený model, přesuňte kamera a sledujte ARKit udržuje modelu umístěný.

### <a name="5-pause-the-augmented-reality-session"></a>5. Pozastavení rozšířená skutečnosti relace

Je dobrým zvykem pozastavit ARKit relace, pokud není viditelná řadiče zobrazení (v `ViewWillDisappear` metoda:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Souhrn

Výše uvedený kód za následek jednoduchou aplikaci ARKit. Složitější příklady by uživatel očekával řadiče zobrazení hostování rozšířená skutečnosti relace k implementaci `IARSCNViewDelegate`, a další metody implementovat.

ARKit nabízí spoustu sofistikovanější funkce, jako je prostor sledování a interakci s uživatelem. Najdete v článku [UrhoSharp ukázku](urhosharp.md) příklad kombinování ARKit sledování s UrhoSharp.


## <a name="related-links"></a>Související odkazy

- [Rozšířená skutečnosti (Apple)](https://developer.apple.com/arkit/)
- [Pomocí ARKit UrhoSharp](urhosharp.md)
- [Ukázka jednoduché ARKit (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit umístění objekty (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Představení ARKit - rozšířená skutečnost pro iOS (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/602/)
