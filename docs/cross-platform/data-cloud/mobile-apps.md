---
title: Mobilní aplikace Microsoft Azure
description: Ukázky a kód soubory ke stažení pro Azure dokumentaci k portálu.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: 4a6211ac218d914089bf1d0dbcb9956800dde700
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Mobilní aplikace Microsoft Azure

_Ukázky a kód soubory ke stažení pro Azure dokumentaci k portálu._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Tyto odkazy jsou k dispozici v dokumentaci k Xamarin [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) webu.
Přidání funkce Azure do aplikace Xamarin stažením [klienta Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Práce s komponentou Xamarin Azure

Obecnou dokumentací [práce s knihovnou klienta Xamarin (součást)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) k provádění různých úloh s Azure Mobile Apps. Tato stránka obsahuje velké množství fragmenty kódu v ukázkové bez podrobné vysvětlení a příklady, které jsou k dispozici v každé níže uvedené články návod.

## <a name="getting-started"></a>Začínáme

Tento článek obsahuje podrobné pokyny ke zprovoznění první aplikace Xamarin Azure.
Vysvětluje vytvoření nové mobilní aplikace Azure na portálu a pak stažení a spuštění aplikace předem nakonfigurovaná.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Začínáme s uživateli

Poskytuje úplné pokyny pro konfiguraci a kódování obrazovka pro přihlášení pomocí Azure Mobile Services. Zprostředkovatelé ověřování podporovaných patří společnosti Microsoft, Google, Facebook či Twitter.

-  [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Autorizace uživatelů ve skriptech

Ukázkový kód pro back-EndY Javascript

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Začínáme s Push

Úplné pokyny k nakonfigurujte nabízená oznámení na webech společnosti Apple a Google a pak odeslat nabízené oznámení z Azure Mobile Services do zařízení.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Začínáme s Notification Hubs

Úplné pokyny ke konfiguraci nabízená oznámení na webech společnosti Apple a Google, konfigurace centra oznámení Azure a pak generovat nabízená oznámení do zařízení.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>Související odkazy

- [GettingStarted (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure mobilního klienta](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps studijní](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->