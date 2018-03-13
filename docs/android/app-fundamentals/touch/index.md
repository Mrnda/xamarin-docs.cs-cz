---
title: "Dotykové ovládání"
description: "Dotykové obrazovky na množství dnešních zařízení umožňují uživatelům rychle a efektivně využívat zařízení přirozené a intuitivní způsobem. Tato interakce se neomezuje jenom na jednoduchý touch detekce – je možné pomocí gest také. Například gesto roztahováním přiblížení, je velmi běžné příklad tohoto roztáhnout součástí obrazovky se dvěma prsty, které uživatel může zvětšit nebo zmenšit. Tato příručka prozkoumá touch a gesta v Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 791dd25796e1f9ba4d73b99dea0fd043431f0218
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="touch"></a>Dotykové ovládání

_Dotykové obrazovky na množství dnešních zařízení umožňují uživatelům rychle a efektivně využívat zařízení přirozené a intuitivní způsobem. Tato interakce se neomezuje jenom na jednoduchý touch detekce – je možné pomocí gest také. Například gesto roztahováním přiblížení, je velmi běžné příklad tohoto roztáhnout součástí obrazovky se dvěma prsty, které uživatel může zvětšit nebo zmenšit. Tato příručka prozkoumá touch a gesta v Android._

## <a name="touch-overview"></a>Touch – přehled

iOS a Android jsou podobné způsoby dotykového ovládání, které zpracovávají. Obě může podporovat více touch - mnoho body obraťte se na obrazovce – a komplexní gesta. Tento průvodce uvádí některé podobnosti v koncepty a také zvláštnosti implementace touch a gesta na obě platformy.

Používá Android `MotionEvent` objektu k zapouzdření touch data a metody pro objekt zobrazení tak, aby naslouchala pro úpravy.

Kromě zaznamenání dat dotykového ovládání, iOS a Android prostředkem ke interpretace vzorce úpravy do gesta. Tyto nástroje pro rozpoznávání gesto zase umožňuje interpretovat příkazy specifické pro aplikace, jako je například otočení bitové kopie nebo zapnout stránky. Android poskytuje několik podporované gesta, jakož i prostředky, aby přidání vlastních gest komplexní snadné.

Zda pracujete na Android nebo iOS, může být volba mezi úpravy a nástroje pro rozpoznávání gesto matoucí jeden. Tento průvodce doporučuje, aby obecně by měl dává gesto rozpoznávání. Nástroje pro rozpoznávání gesto jsou implementované jako diskrétní třídy, které poskytují větší oddělení otázky a lepší zapouzdření. To umožňuje snadno sdílet logiku mezi různá zobrazení, minimalizovat dobu kód napsaný.

Tato příručka následuje podobně jako formát pro každý operační systém: první, platforma touch rozhraní API jsou umístěna a vysvětlení, protože se jedná o foundation, na které touch jsou postaveny interakce. Potom jsme ponořte do světa gesto rozpoznávání – nejprve zkoumat některé běžné gesta a dokončení s vytvářením vlastních gest pro aplikace. Nakonec uvidíte, jak sledovat jednotlivé prsty pomocí sledování nízké úrovně touch vytvoříte finger-paint program.

## <a name="sections"></a>Oddíly

-  [Dotykové ovládání v Androidu](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Návod: Použití Touch v Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Sledování několika dotykového ovládání](touch-tracking.md)

## <a name="summary"></a>Souhrn

V této příručce jsme se zaměřili touch v Android. Pro oba operační systémy jsme se dozvěděli, jak povolit dotykové ovládání a způsob reakce na události dotykového ovládání. V dalším kroku jsme zjistili, o gesta a některé nástroje pro rozpoznávání gesto, Android a iOS, zadejte pro zpracování některého z následujících běžných scénářů. Jsme se zaměřili na tom, jak vytvořit vlastní gesta a implementovat je v aplikacích. Návod ukázán koncepty a rozhraní API pro každý operační systém v akci, a také viděli, jak sledovat jednotlivé prsty.



## <a name="related-links"></a>Související odkazy

- [Android Touch Start (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch konečné (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [FingerPaint (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
