---
title: Nativní iOS, Mac a vazby projekty odkazy
description: Nativní odkazy vám dává možnost pro vložení nativní rozhraní do Xamarin.iOS, Xamarin.Mac nebo vazby projektu.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3a497d0bb4674014b8063cb1fbc91eec6e7ae5ea
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781715"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>Nativní odkazy v iOS, Mac a vazby projekty

_Nativní odkazy vám dává možnost pro vložení nativní rozhraní do projektu Xamarin.iOS nebo Xamarin.Mac nebo vazby projektu._

Od sady iOS 8.0 ji bylo možné vytvořit představuje embedded rozhraní sdílet kód mezi přípony aplikace a hlavní aplikace v Xcode. Pomocí funkce nativní referenční dokumentace ho bude možné využívat tyto vložené architektury (vytvořeny s Xcode) v Xamarin.iOS.
 
> [!IMPORTANT]
> Nebude možné vytvořit vložený rozhraní z jakéhokoli typu Xamarin.iOS nebo Xamarin.Mac projektů, nativní odkazy Povolit jenom pro používání existující nativní rozhraní (Objective-C).

<a name="Terminology" />

## <a name="terminology"></a>Terminologie

V iOS 8 (nebo novější) **vložené architektury** může být obě vložených staticky propojené a dynamicky propojené architektury. K distribuci je správně, musíte je provést do rozhraní "fat", které zahrnuty všechny jejich _řezy_ pro každou architekturu zařízení, která chcete podporovat s vaší aplikací.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>Statické vs. Dynamické architektury

**Statické architektury** jsou propojeny v době kompilace kde **dynamické architektury** jsou propojeny v době běhu a nelze jej pro ně změnit bez znovu propojení. Pokud jste použili libovolnou architekturu 3. stran před iOS 8, jste používali **statické Framework** který se zkompiluje do vší aplikace. Najdete v článku společnosti Apple [dynamické programování knihovny](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) dokumentace pro další podrobnosti.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>Vložené vs. Rozhraní systému

**Vložené architektury** jsou součástí vaší sady aplikací a jsou přístupné pro konkrétní aplikaci přes jeho izolovaného prostoru. **Rozhraní systému** jsou uložené na úrovni operačního systému a jsou dostupné pro všechny aplikace na zařízení. V současné době pouze Společnost Apple má schopnost vytvářet architektury úroveň operačního systému.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>Dynamické vs. FAT architektury

**Dynamicky architektury** obsahovat pouze zkompilovaný kód pro konkrétní systém architekturu kde **Fat architektury** obsahovat kód, který víc architektur. Každý specifické pro architekturu codebase zkompilovat do rozhraní se označuje jako _řez_. Tak například jestliže jsme, že rozhraní, které bylo kompilováno dvě iOS simulátoru architektury (i386 a X86_64), měl by obsahovat dvě řezy.

Pokud jste se pokusili distribuovat Tato ukázka Framework s vaší aplikací, by mohla správně spouštět v simulátoru, ale protože rozhraní neobsahuje žádné řezy kód specifický pro zařízení s iOS selhání v zařízení. Aby se zajistilo, že rozhraní bude fungovat ve všech instancích, by také nutné zahrnout řezy konkrétní zařízení, například arm64, armv7 a armv7s.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>Práce s vložené architektury

Existují dva kroky, které je třeba provést pro práci s vložené architektury v aplikaci Xamarin.iOS nebo Xamarin.Mac: vytváření rozhraní Fat a vložení rozhraní.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Vytváření Fat Framework

Jak jsme uvedli výše, abyste mohli využívat představuje Embedded rozhraní v aplikaci, musí být Fat rozhraní, které obsahuje všechny systémových architektur řezy pro zařízení, která vaše aplikace poběží na.

Pokud rozhraní a náročné aplikace jsou ve stejném projektu Xcode, to není problém jako Xcode bude sestavení rozhraní Framework a aplikace pomocí stejné nastavení sestavení. Vzhledem k tomu, že aplikace Xamarin nelze vytvořit vložený rozhraní, tento postup nelze použít.

Chcete-li tento problém vyřešit `lipo` nástroj pro příkazový řádek umožňuje sloučit dva nebo více rozhraní do jednoho Fat Framework obsahující všechny nezbytné řezy. Další informace o práci s `lipo` příkaz, prohlédněte si prosím naše [propojení nativní knihovny](~/ios/platform/native-interop.md) dokumentaci.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>Vložení rozhraní

Postupujte podle kroku jsou nezbytné pro vložení rozhraní v projektu Xamarin.iOS nebo Xamarin.Mac pomocí nativní odkazy:

1. Vytvoření nového nebo otevření existujícího projektu Xamarin.iOS, Xamarin.Mac nebo vazby.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **přidat** > **přidat nativní referenční dokumentace**: 

    [![](native-references-images/ref01.png "V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a vyberte přidat nativní referenční dokumentace")](native-references-images/ref01.png#lightbox)
3. Z **otevřete** dialogové okno, vyberte název nativní Framework, který chcete vložit a klikněte na tlačítko **otevřete** tlačítko: 

    [![](native-references-images/ref02.png "Vyberte název nativní Framework vložení a klikněte na tlačítko Otevřít")](native-references-images/ref02.png#lightbox)
4. Rozhraní framework budou přidány do stromu projektu: 

    [![](native-references-images/ref03.png "Rozhraní framework budou přidány do stromu projekty")](native-references-images/ref03.png#lightbox)

Při kompilaci projektu nativní Framework budou vloženy do aplikace sady.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>Rozšíření aplikace a vložené architektury

Interně Xamarin.iOS může využít této funkce lze propojit s modulem runtime Mono jako rozhraní (Pokud je cílem nasazení > = iOS 8.0), tedy zmenšení velikosti aplikace výrazně pro aplikace s příponami (protože Mono runtime budou zahrnuty pouze jednou pro celá aplikace sady, místo s jednou registrací u kontejneru aplikace a jednou pro každé rozšíření).

Rozšíření se propojit s modulem runtime Mono jako rozhraní, protože všechna rozšíření vyžadují iOS 8.0.

Aplikace, které nemají rozšíření a aplikace iOS tento cíl 

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout nativní rozhraní vložení do aplikace Xamarin.iOS nebo Xamarin.Mac trvá.

