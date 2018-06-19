---
title: Vytváření uživatelského rozhraní s iOS návrháře
description: Tento dokument popisuje, jak používat návrháře Xamarin pro iOS k sestavení aplikace uživatelské rozhraní s scénářů a .xib souborů. Odkazuje na dokumenty, které popisují dostupnosti nástroj, jeho základních funkcí, navrhovatelé ovládací prvky a zadejte návody jeho použití.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/31/2018
ms.openlocfilehash: a931373a6abba3084af3c7aefcdddc903ad1b577
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209229"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Vytváření uživatelského rozhraní s iOS návrháře

_Návrhář Xamarin pro iOS je vizuálního návrháře pro iOS formáty scénáře a Tvůrce rozhraní, která jsou plně integrované s Visual Studio pro Mac a Visual Studio. IOS Návrhář udržuje úplnou kompatibilitu s formáty scénáře a .xib tak, aby soubory lze upravit v sadě Visual Studio pro Mac nebo Visual Studio kromě Tvůrce rozhraní pro Xcode. Kromě toho návrháře Xamarin pro iOS podporuje pokročilé funkce, jako je například vlastní ovládací prvky, které vykreslení v době návrhu v editoru._

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![iOS Návrhář v sadě Visual Studio pro Mac](images/designer-vsmac-sml.png "iOS návrháře")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![iOS Návrhář v sadě Visual Studio](images/designer-vs.png "iOS návrháře")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>Dostupnost

Návrhář Xamarin pro iOS je k dispozici v sadě Visual Studio pro Mac a Visual Studio 2017 v systému Windows.

Tyto příručky předpokládá znalost obsahu zahrnutého v [Xamarin.iOS Začínáme provede](~/ios/get-started/index.md).

## <a name="ios-designer-basicsintroductionmd"></a>[Základy iOS Designeru](introduction.md)

Tento průvodce popisuje funkce návrháře Xamarin iOS. Vysvětluje základy návrháře, jak používat návrháře vizuálně Rozvrhněte ovládací prvky a úprava vlastností.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[Přehled navrhovatelé ovládacích prvků](ios-designable-controls-overview.md)

Tento průvodce hledá v hloubku vlastní ovládací prvky, jak se vytvářejí a jaké požadavky musí splňovat k vykreslení na návrhovou plochu. Kromě toho ukazuje, jak ladit běžné problémy, ke kterým dochází při používání navrhovatelé ovládací prvky.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[Návod - použití vlastních ovládacích prvků s iOS návrháře](ios-designable-controls-walkthrough.md)

Tento článek obsahuje podrobný postup znázorňující postup vytvoření vlastního ovládacího prvku a použít ho v Návrháři iOS. Ukazuje, jak zpřístupnit ovládacího prvku v panelu nástrojů návrháře, přetáhněte nebo vyřazením do zobrazení. Kromě toho ukazuje, jak implementovat ovládacího prvku, vykreslí správně v době návrhu a prostředí runtime, jakož i vytvoření vlastnosti, které lze nastavit v době návrhu.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Automatické rozložení s Xamarin iOS návrháře](designer-auto-layout.md)

Tento průvodce představuje iOS automatického rozložení a nový pracovní postup omezení, které jsou k dispozici v Návrháři iOS.
