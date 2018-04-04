---
title: Úvod do systému iOS 11
description: Zkuste nové iOS 11 rozhraní API s funkcí Xamarin.
ms.prod: xamarin
ms.assetid: 22C38EA6-6DA9-4B92-B41B-814E589033F6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 0b3993f09b6516c9c0d4f13a82d97d3cf289330c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-11"></a>Úvod do systému iOS 11

_Zkuste nové iOS 11 rozhraní API s funkcí Xamarin._

> [!NOTE]
> Než začnete, zkontrolujte [Začínáme](get-started.md) stránky pokyny k instalaci Xcode 9.

![Příklad ARKit](images/arkit.png) ![Uvedení objektů AR](images/arkit2.png) ![Příklad CoreML](images/coreml.png) ![Příklad MapKit](images/mapkit.png) ![Příklad vize obdélníků](images/vision1.png) ![Příklad řezy vize](images/vision2.png) ![Příklad přetažení](images/drag-drop.png) ![Příklad přetažení](images/drag-drop2.png) ![Příklad SiriKit](images/sirikit.png)

iOS 11 obsahuje mnoho zcela nové funkce a vylepšení pro různé architektury:

## <a name="preparing-your-app-for-ios-11updating-your-appindexmd"></a>[Příprava aplikace pro iOS 11](updating-your-app/index.md)

Architektura aktualizací, nové visual změny a o aktualizované iTunes připojit proces pro iOS 11, má společnost Apple vydala. Tohoto průvodce použijte, abyste měli jistotu, že aplikace Xamarin.iOS připravený na novou verzi.

## <a name="arkitarkitindexmd"></a>[ARKit](arkit/index.md)

ARKit přináší rozšířen skutečnosti pro iOS, což umožňuje uživatelům interakci s na světě prostřednictvím fotoaparát zařízení.
V Xamarin, můžete také použít [ARKit s UrhoSharp](arkit/urhosharp.md).

## <a name="coremlcoremlmd"></a>[CoreML](coreml.md)

Modely Machine learning se dají integrovat do aplikací pro iOS 11 s CoreML. Rozhraní framework CoreML poskytuje jednoduché rozhraní API do aplikace projekty umožňující problémy, které má být analyzován zahrnout stávající modely přímo do aplikace.

## <a name="corenfccorenfcmd"></a>[CoreNFC](corenfc.md)

iPhone 7 a novější zařízení může číst značky Near Field Communication (NFC), povolení aplikací pro zjištění s příznakem produkty, místech nebo akcí na světě je obcházet.

## <a name="drag-and-dropdrag-and-dropmd"></a>[Přetažení](drag-and-drop.md)

Přetažení framework přináší iOS celou podporu pro přesun dat modelem touch. Na Ipadu můžete přetáhnout uvnitř i mezi různé aplikace; Když na zařízení iPhone, můžete přetáhnout pouze v rámci stejné aplikaci. Není poskytována podpora pro mnoho typů přizpůsobení, včetně bohaté datové typy, animace a multitouch gesta zpracování.

## <a name="mapkitmapkitmd"></a>[MapKit](mapkit.md)

MapKit má několik vylepšení, včetně podpory pro automatické značku seskupení a přidání kompas zobrazení.

## <a name="pdfkit"></a>PDFKit

PDFKit je k dispozici v systému iOS 11, přináší PDF vytváření a úpravy funkce, které vaše aplikace.

## <a name="sirikitsirikitmd"></a>[SiriKit](sirikit.md)

Siri teď podporuje i další interakce, včetně seznamy a poznámky a další vylepšení, například aplikace alternativní názvy.

## <a name="visionvisionmd"></a>[Vision](vision.md)

Přináší řadu image zpracování a analýza funkce pro iOS, včetně detekce vzhled a rozpoznávání, modely CoreML, nové zjištění čárový kód rozhraní API, text a horizon zjišťování a další obecné zjišťování objektu a sledování.

## <a name="samples"></a>Ukázky kódu

Máme několik C# [ukázky](https://developer.xamarin.com/samples/ios/iOS11/) jak začít:

* [Ukázka ARKit](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
* [ARKit umístění objektů](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
* [ARKit a UrhoSharp](arkit/urhosharp.md)
* [Ukázka CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreML)
* [Ukázka rozpoznávání CoreML obrázku](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition)
* [CoreML s Azure vlastní modelu](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
* [Ukázka čtečky CoreNFC značky](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
* [Přetažením zobrazení tabulky](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView)
* [Přetažením zobrazení kolekce](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
* [Přetažením vlastního zobrazení](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCustomView)
* [Přetáhněte DragBoard & rozevírací ukázka](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropDragBoard)
* [Ukázka MapKit](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample)
* [Ukázka SiriKit](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
* [Aktualizovaný ukázkový framework fotografie](https://developer.xamarin.com/samples/monotouch/ios11/SamplePhotoApp/)
* [Vize & CoreML vzorku](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision)
* [Ukázka zjišťování vize obdélníků](https://developer.xamarin.com/samples/monotouch/ios11/VisionRects)
* [Ukázka zjišťování řezy vize](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces)
* [Ukázka PDKFit pomůcky](https://developer.xamarin.com/samples/monotouch/ios11/PDFAnnotationWidgetsAdvanced)
* [Ukázka PDFKit vodoznak](https://developer.xamarin.com/samples/monotouch/ios11/PDFDocumentWatermark)

## <a name="more-information"></a>Další informace

Další informace o iOS 11 najdete na webu společnosti Apple [co je nového v iOS 11](https://developer.apple.com/ios/) dokumentaci.


## <a name="related-links"></a>Související odkazy

- [Co je nového v iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Ukázky Xamarin iOS 11](https://developer.xamarin.com/samples/ios/iOS11/)
