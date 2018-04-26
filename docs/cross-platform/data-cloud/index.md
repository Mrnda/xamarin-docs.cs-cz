---
title: Microsoft Azure
description: Dokumentaci a ukázkový kód soubory ke stažení pro Azure.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: ab449a58cc87699b97a1ade7721a08f771c4f55d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="microsoft-azure"></a>Microsoft Azure

_Dokumentaci a ukázkový kód soubory ke stažení pro Azure._

[ ![](images/evolve-mikej-azure-sml.png "Funkce Azure App Services lze snadno přidávat do aplikace Xamarin, včetně datové úložiště v cloudu a různé platformy nabízená oznámení")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Momentální 2016: Vývoj připojených aplikací pomocí Azure a Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Připojené služby v sadě Visual Studio pro Mac

Nové [připojené služby](connected-services.md) funkce sady Visual Studio pro Mac pomáhá vývojářům snadno a rychle přidat Azure funkce mobilní aplikace z prostředí IDE. Aktuálně k dispozici pro testování v kanálu alfa.


## <a name="azure-app-services"></a>Azure App Services

Je kolekce [dokumentace Azure Mobile Apps](~/cross-platform/data-cloud/mobile-apps.md) který vás provede procesem implementace [klienta Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin také nabízí balíčky NuGet pro zasílání zpráv Azure pro [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) a [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) pomohou implementovat nabízených oznámení napříč platformami.

Konfigurace aplikace na [portál Azure App Services](https://portal.azure.com/) pro přístup k Mobile Apps, webová rozhraní API, úložiště a mnoho dalšího. Další informace o [jak aplikační služby se liší](http://azure.microsoft.com/updates/whats-new-with-azure-app-service/) a podívejte se na v [tyto videa z Microsoft](http://azure.microsoft.com/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Ověřování služby Active Directory

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) lze použít pro přihlášení uživatele v aplikacích Xamarin prostřednictvím [Xamarin.Auth součást](https://www.nuget.org/packages/Xamarin.Auth/).
Aplikace můžete poté přistoupit další služby, jako je Office 365.

## <a name="webapi"></a>WebAPI

Společnosti Microsoft webového rozhraní API zpřístupňuje rozhraní REST typu, který můžete snadno využívat aplikace Xamarin.
Je možné snadno roztočení [webu Azure](https://trywebsites.azurewebsites.net/) a vytvořit aplikaci na základě WebAPI pro připojení k aplikace Xamarin.


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Úvod k webovým službám](~/cross-platform/data-cloud/web-services/index.md)

Tento kurz představuje jak integrovat REST, WCF a SOAP webové technologie služby s mobilní aplikace Xamarin. Prozkoumá různé implementace služby, vyhodnotí dostupnou nástroje a knihovny je integrovat a poskytuje ukázkové vzory pro použití dat služby. Navíc poskytuje základní přehled vytvoření RESTful webovou službu pro spotřebu pomocí mobilní aplikace Xamarin.

## <a name="samples"></a>Ukázky kódu

Kromě [dokumentace ukázky](https://github.com/xamarin/mobile-samples/tree/master/Azure), následující dokončení aplikace ukazují různé funkce Azure, které jsou součástí aplikace Xamarin:

- [Sport](https://github.com/xamarin/Sport) – popisný sportu ligy sledování aplikaci, která používá data úložiště & nabízená oznámení.
- [Chvil](https://github.com/pierceboggan/Moments) – rychlých fotografií sdílení, která používá Azure Storage pro bitové kopie.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – používá webového rozhraní API pro back-end.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -Azure Mobile Apps.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) – pro ukázku [architektura řady](https://www.microsoft.com/net/learn/architecture) z e-knih.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IoT ukázkové z Build 2016.


## <a name="related-links"></a>Související odkazy

- [Příklad Azure PCL (podle @paulbatum) (ukázka)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Portál Azure](http://azure.microsoft.com/)
- [Mobilního klienta pro Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
