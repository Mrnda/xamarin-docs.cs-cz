---
title: Zastaralé oznámení technologie v Xamarin.iOS
description: Tento dokument popisuje technologie oznámení iOS, které jsou zastaralé považuje rozhraní oznámení uživateli, zavedená v iOS 10.
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2016
ms.openlocfilehash: 4becc5e296fb6e2496d9ffd863f7137419480262
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788550"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Zastaralé oznámení technologie v Xamarin.iOS

V této části ukazuje, jak implementovat místních a nabízených oznámení v Xamarin.iOS. Bude popisují různé prvky uživatelského rozhraní oznámení iOS a popisují rozhraní API je zahrnuta s vytváření a zobrazování oznámení.

> [!IMPORTANT]
> Informace v této části se vztahují na iOS 9 a předchozí, ho byla ponechána zde k podpoře starší verze iOS. IOS 10 a novější, najdete v tématu [uživatele oznámení Framework – průvodce](~/ios/platform/user-notifications/index.md) pro podporu místní i vzdálené oznámení na zařízení s iOS.

## <a name="sections"></a>Oddíly

<a name="Local Notifications In iOS" />

##  <a name="local-notifications-in-ioslocal-notifications-in-iosmd"></a>[Místní oznámení v iOS](local-notifications-in-ios.md)

V této části se popisují postup implementovat místní oznámení v Xamarin.iOS. Bude popisují různé prvky uživatelského rozhraní oznámení iOS a popisují rozhraní API je zahrnuta s vytváření a zobrazování oznámení.

<a name="Local Notifications Walkthrough" />

##  <a name="walkthrough---using-local-notifications-in-xamarinioslocal-notifications-in-ios-walkthroughmd"></a>[Návod – Používání místních oznámení v Xamarin.iOSu](local-notifications-in-ios-walkthrough.md)

V této části projdeme použití místního oznámení aplikace pro Xamarin.iOS. Popisuje základní informace o vytváření a publikování oznámení, že se objeví se upozornění, když přijaté aplikací.

<a name="Remote Notifications In iOS" />

##  <a name="remote-notifications-in-iosremote-notifications-in-iosmd"></a>[Vzdálená oznámení v iOS](remote-notifications-in-ios.md)

Tato část se bude zabývat nabízená oznámení v iOS. Zavádí oznámení brány služby APNS (Apple Push) a roli, které splňuje při publikování oznámení pro aplikace iOS. Se vysvětluje, jak vytvořit potřeba povolte nabízená oznámení a popisují certifikáty zabezpečení. Nakonec bude v této části popisují některé housekeeping úlohy, které můžete sledovat mobilní zařízení klientů musí provést aplikační servery.

## <a name="related-links"></a>Související odkazy

- [Oznámení (ukázka)](https://developer.xamarin.com/samples/monotouch/Notifications/)
