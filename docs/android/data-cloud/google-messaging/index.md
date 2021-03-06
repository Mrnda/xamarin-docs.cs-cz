---
title: Zasílání zpráv Google
description: Tato část obsahuje pokyny, které popisují, jak implementovat aplikace Xamarin.Android pomocí služby Google zasílání zpráv.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: cf1eaec3dfee7c3457a4614147c9b5564843b2a7
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044662"
---
# <a name="google-messaging"></a>Zasílání zpráv Google

_Tato část obsahuje pokyny, které popisují, jak implementovat aplikace Xamarin.Android pomocí služby Google zasílání zpráv._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Firebase Cloud Messaging](firebase-cloud-messaging.md)

Zasílání zpráv cloudu firebase (FCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. FCM je nástupcem Google Google Cloud Messaging. Tento článek obsahuje přehled toho, jak funguje FCM a poskytuje podrobný postup pro získání přihlašovacích údajů, aby se vaše aplikace používat FCM služby.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Vzdálená oznámení s Firebase cloudu zasílání zpráv](remote-notifications-with-fcm.md)

Tento názorný postup obsahuje podrobné vysvětlení způsobu implementace vzdáleného oznámení (také nazývané nabízená oznámení) pomocí služby zasílání zpráv cloudu Firebase v aplikaci Xamarin.Android. Ilustruje způsob implementace různých tříd, které jsou potřebné pro komunikaci s zasílání zpráv cloudu Firebase (FCM), obsahuje příklady, jak nakonfigurovat Android Manifest pro přístup k FCM a ukazuje podřízené zasílání zpráv pomocí Firebase Konzola.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Google Cloud Messaging](google-cloud-messaging.md)

Tato část obsahuje souhrnné informace o tom, jak zasílání zpráv cloudu Google (GCM) směrování zpráv mezi vaší aplikace a aplikačního serveru a poskytuje podrobný postup pro získání přihlašovacích údajů, aby vaše aplikace bude moci pomocí služby GCM. (Všimněte si, že GCM byla nahrazena FCM.)

> [!NOTE]
> Byl nahrazen GCM [zasílání zpráv cloudu Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serverových a klientských rozhraní API [jsou zastaralé](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) a bude k dispozici co nejdříve 11. dubna 2019.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Vzdálená oznámení s zasílání zpráv cloudu Google](remote-notifications-with-gcm.md)

Tato část obsahuje podrobné vysvětlení, jak implementovat vzdáleného oznámení v Xamarin.Android pomocí Google Cloud Messaging.
Vysvětluje různé součásti, které musí být využít pro povolení Google Cloud Messaging v aplikaci pro Android.


