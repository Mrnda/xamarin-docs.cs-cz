---
title: SiriKit
description: "Tento článek ukazuje, jak používat SiriKit v aplikaci Xamarin.iOS k poskytování služeb, které jsou dostupné pro uživatele používání Siri na zařízení s iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c4fdf61b35ca28af82e3890242d54a75e50d2f82
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="sirikit"></a>SiriKit

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
