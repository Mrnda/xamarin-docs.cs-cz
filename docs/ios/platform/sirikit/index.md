---
title: SiriKit v Xamarin.iOS
description: Tento článek ukazuje, jak používat SiriKit v aplikaci Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele používání Siri na zařízení s iOS.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 584b694c83d6c66d6e79e1030b2e682cefb64e6a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788049"
---
# <a name="sirikit-in-xamarinios"></a>SiriKit v Xamarin.iOS

_Tento článek ukazuje, jak používat SiriKit v aplikaci Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele používání Siri na zařízení s iOS._

Nové iOS 10 SiriKit povoluje aplikace pro iOS k poskytování služeb, které jsou dostupné pro uživatele pomocí Siri a mapy aplikace na zařízení s iOS pomocí rozšíření aplikace a nové **záměry** a **uživatelského rozhraní záměry** architektury.

Siri pracuje s koncept **domény**, skupiny víte akce pro související úlohy. Každý interakce, která obsahuje aplikaci s Siri musí spadat do jednoho z jeho domény známé služby následujícím způsobem:

- Zvuk nebo video volání.
- Rezervace pravé.
- Správa cvičení nebo.
- Zasílání zpráv.
- Hledání fotografie.
- Odesílání nebo přijímání platby.

Když uživatel odešle žádost siri jeden rozšíření aplikace služby, odešle SiriKit rozšíření **záměr** objekt, který popisuje žádost uživatele spolu s podpůrné daty. Rozšíření aplikace vygeneruje odpovídající **odpovědi** objekt pro v dané **záměr**, s podrobnostmi o tom, jak můžete rozšíření žádost zpracovat.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[Principy SiriKitu](~/ios/platform/sirikit/understanding-sirikit.md)

Tento článek popisuje klíčové koncepty, které budou potřebné pro práci s SiriKit v aplikaci pro Xamarin.iOS. Popisuje nové tříd Intent a body rozšíření tříd Intent uživatelského rozhraní a jak pracují s aplikaci a uživatele termínů otevřete aplikaci k Siri.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[Implementace SiriKitu](~/ios/platform/sirikit/implementing-sirikit.md)

Tento článek popisuje kroky nutné k implementaci SiriKit podpory v aplikacích pro Xamarin.iOS. Vývojář si před pokusem o přidání podpory SiriKit do aplikace, jako klíč, které jsou popsané koncepty, které budou potřebné pro úspěšné dokončení implementace v Průvodci Principy SiriKit koncepty výše.





## <a name="related-links"></a>Související odkazy

- [Ukázka ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Průvodce programováním SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Záměry Framework – referenční informace](https://developer.apple.com/reference/intents)
- [Záměry uživatelského rozhraní Framework – referenční informace](https://developer.apple.com/reference/intentsui)
