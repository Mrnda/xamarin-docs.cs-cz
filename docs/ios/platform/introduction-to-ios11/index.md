---
title: Úvod do Iosu 11
description: Tento dokument obsahuje odkazy na různá vodítka, které popisují funkce Iosu 11, včetně ARKit, CoreML, Mapkitu, PDFKit, Sirikitu, rozhraní pro zpracování obrazu a dalších.
ms.prod: xamarin
ms.assetid: 22C38EA6-6DA9-4B92-B41B-814E589033F6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2017
ms.openlocfilehash: e4e544679dbed2701c50d5596d4907451653090e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351054"
---
# <a name="introduction-to-ios-11"></a>Úvod do Iosu 11

_Vyzkoušejte nové iOS 11 rozhraní API s využitím kódu Xamarin._

> [!NOTE]
> Než začnete, projděte si [Začínáme](get-started.md) stránky pokyny o tom, jak nainstalovat Xcode 9.

![Příklad ARKit](images/arkit.png) ![Uvedení objektů AR](images/arkit2.png) ![Příklad CoreML](images/coreml.png) ![Příklad Mapkitu](images/mapkit.png) ![Příklad obdélníky pro zpracování obrazu](images/vision1.png) ![Příklad tváří pro zpracování obrazu](images/vision2.png) ![Příklad přetažení](images/drag-drop.png) ![Příklad přetažení](images/drag-drop2.png) ![Příklad Sirikitu](images/sirikit.png)

iOS 11 obsahuje mnoho zcela nové funkce a vylepšení napříč celou řadu architektur:

## <a name="preparing-your-app-for-ios-11updating-your-appindexmd"></a>[Příprava aplikace pro iOS 11](updating-your-app/index.md)

Apple přináší aktualizace architektury, nové vizuální změny a o aktualizovanou iTunes Connect proces pro iOS 11. Tento průvodce vám Ujistěte se, že aplikace Xamarin.iOS připravený na novou verzi.

## <a name="arkitarkitindexmd"></a>[ARKit](arkit/index.md)

Arkit, která přináší rozšířené Reality pro iOS, umožňuje uživatelům interakci s ostatními prostřednictvím fotoaparát zařízení.
S využitím kódu Xamarin, můžete také použít [ARKit s Urhosharpu](arkit/urhosharp.md).

## <a name="coremlcoremlmd"></a>[CoreML](coreml.md)

Modely strojového učení je možné integrovat do aplikací pro iOS 11 pomocí CoreML. Architektura CoreML poskytuje jednoduché rozhraní API začlenit stávající modely do aplikace projekty povolit problémy analyzovat přímo v aplikaci.

## <a name="corenfccorenfcmd"></a>[CoreNFC](corenfc.md)

iPhone 7 a novější zařízení může číst téměř Field Communication (NFC) značky, které umožňuje aplikacím zjistit příznakem produktů, místa nebo akce na světě, co s nimi souvisí.

## <a name="drag-and-dropdrag-and-dropmd"></a>[Přetažení](drag-and-drop.md)

Přetažení framework přináší podporu iOS celou přesouvat data modelem touch. Na Ipadu můžete přetáhnout uvnitř i mezi různé aplikace; Když jste na zařízení iPhone, můžete přetáhnout pouze ve stejné aplikaci. Není poskytována podpora pro mnoho typů vlastního nastavení, včetně bohaté datové typy, animace a gesta zpracování vícedotykového ovládání.

## <a name="mapkitmapkitmd"></a>[MapKit](mapkit.md)

Mapkitu má celou řadu vylepšení, včetně podpory pro automatické značky, seskupení a přidání kompas zobrazení.

## <a name="pdfkit"></a>PDFKit

PDFKit je teď dostupná v Iosu 11 přináší PDF vytváření a úpravy do aplikací funkce.

## <a name="sirikitsirikitmd"></a>[SiriKit](sirikit.md)

Siri teď podporuje i další interakce, včetně seznamů a poznámky a dalších vylepšení, jako je například aplikace alternativní názvy.

## <a name="visionvisionmd"></a>[Vision](vision.md)

Přináší širokou škálu image funkce zpracování a analýz do zařízení iOS, včetně rozpoznávání tváří a rozpoznávání, modely CoreML, nové detekce čárového kódu rozhraní API, text a časový horizont detekce a obecnější zjišťování objektů a sledování.

## <a name="samples"></a>Ukázky kódu

Máme několik jazyka C# [ukázky](https://developer.xamarin.com/samples/ios/iOS11/) vám pomůžou začít:

* [Ukázka ARKit](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
* [Uvedení objektů ARKit](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
* [Arkitem a Urhosharpu](arkit/urhosharp.md)
* [Ukázka CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreML)
* [Ukázka rozpoznávání obrázků CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition)
* [CoreML pomocí vlastního modelu Azure](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
* [Ukázka čtečky CoreNFC značky](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
* [Přetažením zobrazení tabulky](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView)
* [Přetažením zobrazení kolekce](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
* [Přetažením vlastního zobrazení](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCustomView)
* [Přetáhněte DragBoard & vzorku přetažení](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropDragBoard)
* [Ukázka Mapkitu](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample)
* [Ukázka Sirikitu](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
* [Aktualizovaný ukázkový framework fotografie](https://developer.xamarin.com/samples/monotouch/ios11/SamplePhotoApp/)
* [Vision a CoreML vzorku](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision)
* [Ukázka zjišťování obdélníky pro zpracování obrazu](https://developer.xamarin.com/samples/monotouch/ios11/VisionRects)
* [Ukázka zjišťování tváří pro zpracování obrazu](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces)
* [Ukázka PDKFit Widgetů](https://developer.xamarin.com/samples/monotouch/ios11/PDFAnnotationWidgetsAdvanced)
* [Ukázka PDFKit vodoznak](https://developer.xamarin.com/samples/monotouch/ios11/PDFDocumentWatermark)

## <a name="more-information"></a>Další informace

Další informace v Iosu 11, najdete na webu společnosti Apple [co je nového v Iosu 11](https://developer.apple.com/ios/) dokumentaci.


## <a name="related-links"></a>Související odkazy

- [Co je nového v Iosu 11 (Apple)](https://developer.apple.com/ios/)
- [Ukázky Xamarin iOS 11](https://developer.xamarin.com/samples/ios/iOS11/)
