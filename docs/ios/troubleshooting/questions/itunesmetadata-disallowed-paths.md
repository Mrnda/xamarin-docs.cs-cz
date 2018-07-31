---
title: 'Proč moje odesílání aplikace selže s: zakázáno cesty (iTunesMetadata.plist) najdete na...?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: c243dc84f696a5ac08f11a5e74f41c5c0914d0ea
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351109"
---
# <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>Proč moje odesílání aplikace selže s: zakázáno cesty (iTunesMetadata.plist) najdete na...?

> Chyba: Chyba ITMS-90047: "zakázáno cesty ("iTunesMetadata.plist") na: Payload/iPhoneApp1.app"

Tato chyba je výsledkem změny v procesu ověřování App Store společnosti Apple k uživatelům zabránit v dosažení problémy, jako jsou [chyb 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180). Tento konkrétní chyby _není_ související s konkrétní verzi Xamarin, které jste nainstalovali, takže downgradu bude _není_ pomoct.

Viz diskuze fóra [tady](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) pro informace o řešení a nejnovější aktualizace pro tento problém.
