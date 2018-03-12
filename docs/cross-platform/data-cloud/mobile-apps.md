---
title: Microsoft Azure Mobile Apps
description: "Ukázky a kód soubory ke stažení pro Azure dokumentaci k portálu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fb3c26b7d090ca42328c61192c794dec1544d1d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

_Ukázky a kód soubory ke stažení pro Azure dokumentaci k portálu._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
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


Tyto odkazy jsou k dispozici v dokumentaci k Xamarin [Azure Mobile Apps](https://azure.microsoft.com/en-us/documentation/services/app-service/mobile/) webu.
Přidání funkce Azure Xamarin aplikace je jednoduché, stahování [klienta Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Práce s komponentou Xamarin Azure

Obecnou dokumentací [práce s knihovnou klienta Xamarin (součást)](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/) k provádění různých úloh s Azure Mobile Apps. Tato stránka obsahuje velké množství fragmenty kódu v ukázkové bez podrobné vysvětlení a příklady, které jsou k dispozici v každé níže uvedené články návod.

## <a name="getting-started"></a>Začínáme

Tento článek obsahuje podrobné pokyny ke zprovoznění první aplikace Xamarin Azure.
Vysvětluje vytvoření nové mobilní aplikace Azure na portálu a pak stažení a spuštění aplikace předem nakonfigurovaná.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-forms-get-started/)

## <a name="validate-modify-and-augment-data-in-scripts"></a>Ověření, upravit a rozšířit Data ve skriptech

Ukazuje, jak přidat skripty na straně serveru do datových tabulek Azure Mobile Services pro implementaci ověřování na straně serveru a další funkce.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)


## <a name="add-paging-to-data"></a>Přidat stránkování k datům

Zběžný příklad stránkování velké sady dat pomocí Skip() a Take().

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)


## <a name="get-started-with-users"></a>Začínáme s uživateli

Poskytuje úplné pokyny pro konfiguraci a kódování obrazovka pro přihlášení pomocí Azure Mobile Services. Zprostředkovatelé ověřování podporovaných patří společnosti Microsoft, Google, Facebook či Twitter.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Autorizace uživatelů ve skriptech

Ukázkový kód pro back-EndY Javascript

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Začínáme s Push

Úplné pokyny k nakonfigurujte nabízená oznámení na webech společnosti Apple a Google a pak odeslat nabízené oznámení z Azure Mobile Services do zařízení.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-push/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-push/)


## <a name="get-started-with-notification-hubs"></a>Začínáme s Notification Hubs

Úplné pokyny ke konfiguraci nabízená oznámení na webech společnosti Apple a Google, konfigurace centra oznámení Azure a pak generovat nabízená oznámení do zařízení.

-  [iOS](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-ios-get-started/)
-  [Android](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-android-get-started/)



## <a name="related-links"></a>Související odkazy

- [GettingStarted (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [ValidateModifyData (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
- [NotificationHubs (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Příklad Azure PCL (podle @paulbatum) (ukázka)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure mobilního klienta](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps studijní](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)