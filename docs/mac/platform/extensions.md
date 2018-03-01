---
title: "Podpora Xamarin.Mac rozšíření"
description: "Tento článek se zaměřuje na podporu rozšíření v Xamarin.Mac verze 2.10 (a vyšší)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5ce20322b576b12ff9dfe56ef0bc9d2e1ca27792
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinmac-extension-support"></a>Podpora Xamarin.Mac rozšíření

Ve verzi preview Xamarin.Mac 2.10 přidala se podpora pro více bodů rozšíření systému macOS:

- Metoda Finder
- Sdílené složky
- Dnes

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Omezení a známé problémy

Následující se omezeních a problémů, které mohou nastat při vývoji rozšíření v Xamarin.Mac vědět:

* Aktuálně neexistuje žádná podpora ladění v sadě Visual Studio for Mac. Všechny ladění bude muset být provedeno prostřednictvím **NSLog** a **konzoly**. Najdete v části Tipy níže podrobnosti.
* Rozšíření musí být obsažen v aplikaci hostitele, který v případě spusťte jednou s zaregistrovat pomocí systému. Musí být pak povolené ve **rozšíření** části **předvolbách systému**. 
* Některé havárií rozšíření můžou destabilizovat hostitelskou aplikaci a způsobí neobvyklé chování. Konkrétně **vyhledávací** a **Dnes** části **centru oznámení** může stát "zasekl papír" a přestat reagovat. To bylo zkušenosti s rozšíření projekty v Xcode také a aktuálně se zobrazí vztah k Xamarin.Mac. Často to můžete vidět v protokolu systému (prostřednictvím **konzoly**, podrobnosti naleznete v tématu typy) tisk opakovaných chybových zpráv. Restartování systému macOS se tento problém opravit.

<a name="Tips" />

## <a name="tips"></a>Tipy

Při práci s rozšířením Xamarin.Mac, může být užitečné následující tipy:

- Protože Xamarin.Mac aktuálně nepodporují rozšíření ladění, zkušenosti s laděním především závisí na spuštění a `printf` jako příkazy. Rozšíření spuštění se ale v izolovaném prostoru procesu, proto `Console.WriteLine` nebude fungovat stejně jako v ostatních aplikacích Xamarin.Mac. Vyvolání [ `NSLog` přímo](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) bude výstup ladění zpráv do systémového protokolu.
- Jakékoli výjimky nezachycená dojde k chybě proces rozšíření, poskytuje jenom malé množství užitečné informace v **protokolu systému**. Zabalení problémových kód `try/catch` (výjimek) blokovat, `NSLog`je předtím, než znovu vyvolání můžou být užitečné.
- **Protokolu systému** lze přistupovat z **konzoly** aplikací v **aplikace** > **nástroje**:

    [ ![](extensions-images/extension02.png "Protokol systému")](extensions-images/extension02.png)
- Jak jsme uvedli výše, spuštění rozšíření hostitelskou aplikaci bude registraci ji v systému. Odstranění sady aplikací se zrušit registraci. 
- Pokud jsou registrované "stray" verze rozšíření aplikace, použijte následující příkaz je najít, (, můžete je odstranit): `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Návod a ukázkové aplikace

Vzhledem k tomu, že vývojář se vytváření a práci s Xamarin.Mac rozšíření stejným způsobem jako rozšíření Xamarin.iOS, naleznete v našem [Úvod do rozšíření](~/ios/platform/extensions.md) dokumentace pro další podrobnosti.

Příklad projektu Xamarin.Mac obsahující malé, ukázky pracovní každého typu rozšíření najdete [zde](https://developer.xamarin.com/samples/mac/ExtensionSamples/).

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek trvá rychlý přehled práce se rozšíření ve verzi Xamarin.Max aplikaci 2.10 (a vyšší).

## <a name="related-links"></a>Související odkazy

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
