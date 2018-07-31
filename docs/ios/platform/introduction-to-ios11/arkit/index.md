---
title: Úvod do arkit, která v Xamarin.iosu
description: Tento dokument popisuje v Iosu 11 s ARKit rozšířené realitě. Popisuje, jak přidat 3D modelu do mobilní aplikace, konfigurace zobrazení, relace delegáta implementujete, umístěte na 3D model na světě a pozastavit relaci rozšířené realitě.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 14bbb35477c098738e9cd7e2cb92154422d394ee
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350612"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Úvod do arkit, která v Xamarin.iosu

_Rozšířená realita pro iOS 11_

Arkit, která umožňuje širokou škálu rozšířené realitě aplikací a her. Tato část obsahuje následující témata:

- [Začínáme s ARKit](#gettingstarted)
- [Pomocí ARKit Urhosharpu](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Začínáme s ARKit

Abyste mohli začít s rozšířené realitě, postupujte podle následujících pokynů provede jednoduchou aplikaci: umístění na 3D model, takže ARKit zachovat modelu v místě s jeho funkce sledování.

![Jet s plovoucí desetinnou čárkou s vyloučením obrazu 3D modelu](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Přidání 3D modelu

Prostředky by měly být přidány do projektu s **SceneKitAsset** akci sestavení.

![Prostředků sady scén v projektu](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Konfigurace zobrazení

V kontroleru zobrazení `ViewDidLoad` metodu načtení prostředku scény a nastavte `Scene` vlastnost pro zobrazení:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. Volitelně můžete implementovat delegáta relace

I když není nutné pro jednoduché případy, implementace delegáta relace může být užitečné při ladění stavu relace ARKit (a aplikace skutečný, poskytování zpětné vazby na uživatele). Vytvoření jednoduché delegáta pomocí níže uvedeného kódu:

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

Přiřaďte delegáta v v `ViewDidLoad` metody:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Umístěte na 3D model na světě

V `ViewWillAppear`, následující kód vytvoří relaci Arkitem a nastavuje pozici 3D modelu do prostoru relativně k fotoaparát zařízení:

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

Pokaždé, když je spuštění nebo obnovení, aplikace na 3D model bude umístěn před fotoaparátu/kamery. Jakmile model je umístěn, posunutí kamery a podívejte se, jak arkit, která zajišťuje model umístěn.

### <a name="5-pause-the-augmented-reality-session"></a>5. Pozastavit relaci rozšířená realita

Je dobrým zvykem pozastavit ARKit relace, když kontroler zobrazení není viditelné (v `ViewWillDisappear` metody:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Souhrn

Výše uvedený kód za následek jednoduché arkit, která aplikace. Složitější příklady byste očekávali kontroler zobrazení hostuje relaci rozšířené realitě k implementaci `IARSCNViewDelegate`, a implementovat další metody.

Arkit, která poskytuje velké množství složitější funkce, jako je surface sledování a interakce uživatelů. Najdete v článku [Urhosharpu ukázka](urhosharp.md) příklad kombinování ARKit sledováním Urhosharpu.


## <a name="related-links"></a>Související odkazy

- [Rozšířené realitě (Apple)](https://developer.apple.com/arkit/)
- [Pomocí ARKit Urhosharpu](urhosharp.md)
- [Ukázka jednoduché ARKit (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [Uvedení objektů ARKit (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Představujeme ARKit – rozšířená realita pro iOS (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/602/)
