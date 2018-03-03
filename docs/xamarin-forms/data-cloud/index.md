---
title: "Datové a cloudové služby"
description: "Xamarin.Forms aplikace můžete využívání webových služeb, které jsou implementované pomocí různých technologií, a tato příručka se prozkoumat jak na to."
ms.topic: article
ms.prod: xamarin
ms.assetid: "0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß"
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 28007ee702c66f3b819430b544465d3470d571d9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="data--cloud-services"></a>Datové a cloudové služby

_Xamarin.Forms aplikace můžete využívání webových služeb, které jsou implementované pomocí různých technologií, a tato příručka se prozkoumat jak na to._

Úvod do používání napříč platformami webové služby na platformě Xamarin, najdete v části [Úvod k webovým službám](~/cross-platform/data-cloud/web-services/index.md).

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[Porozumění ukázce](~/xamarin-forms/data-cloud/walkthrough.md)

Tento článek obsahuje návod ukázkovou aplikaci Xamarin.Forms, která ukazuje, jak komunikovat s jinou webové služby. Obsahuje následující témata anatomie aplikace, stránky, datový model a vyvolání operace webové služby.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Používání webových služeb](~/xamarin-forms/data-cloud/consuming/index.md)

Tato příručka ukazuje, jak komunikovat se službami jiných webových vám umožňuje vytvářet, číst, aktualizovat a odstraňovat funkce (CRUD) k aplikaci Xamarin.Forms. Obsahuje následující témata komunikaci s [ASMX služby](consuming/asmx.md), [služby WCF](consuming/wcf.md), [služby REST](consuming/rest.md), [Azure Mobile Apps](consuming/azure.md)a [ Amazon Web Services](consuming/aws.md).

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Ověřování přístupu k webovým službám](~/xamarin-forms/data-cloud/authentication/index.md)

Tato příručka vysvětluje, jak integrovat ověřovací služby do Xamarin.Forms aplikace umožňuje uživatelům sdílet back-end přitom jenom má přístup k svá vlastní data. Obsahuje následující témata [základní ověřování pomocí služby REST](authentication/rest.md), [pomocí součásti Xamarin.Auth k ověřování na základě poskytovatelů identit OAuth](authentication/oauth.md)a pomocí integrované ověřování mechanismy nabízených [Azure Mobile Apps](authentication/azure.md), a [Amazon Web Services](authentication/aws.md).

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Synchronizace dat s webovými službami](sync/index.md)

Tento článek vysvětluje, jak přidat funkci offline synchronizace Xamarin.Forms aplikace. Offline synchronizace umožňuje uživatelům interakci s mobilních aplikací, zobrazení, přidání nebo úprava dat, i tam, kde není k dispozici síťové připojení. Změny se ukládají do místní databáze, a když je zařízení online, změny mohou být synchronizovány s webovou službou.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[Odesílání nabízených oznámení](push-notifications/index.md)

Tento článek ukazuje, jak přidat nabízená oznámení do aplikace Xamarin.Forms. Azure Notification Hubs poskytuje infrastrukturu škálovatelné nabízených pro odesílání mobilní nabízená oznámení z jakéhokoli back-endu na libovolnou mobilní platformu, přičemž složitost back-end museli komunikovat s různých systémů oznámení platforem.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[Ukládání souborů v cloudu](storage/index.md)

Tento článek ukazuje, jak používat Xamarin.Forms k uložení textové a binární data ve službě Azure Storage a jak získat přístup k datům. Azure Storage je řešení škálovatelné cloudové úložiště, které lze použít pro ukládání nestrukturovaných a strukturovaných dat.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[Vyhledávání dat v cloudu](search/index.md)

Tento článek ukazuje, jak integrovat Azure Search na aplikaci Xamarin.Forms pomocí knihovnu Microsoft Azure Search. Vyhledávání systému Azure je Cloudová služba, která poskytuje indexování a dotazování možnosti pro odeslaná data. Touto akcí odeberete, požadavky na infrastrukturu a vyhledávací algoritmus složité kroky tradičně spojené s implementací funkce vyhledávání v aplikaci.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[Ukládání dat v databázi dokumentů](cosmosdb/index.md)

Tato příručka ukazuje, jak integrovat databázi dokumentů Azure Cosmos DB do Xamarin.Forms aplikací pomocí Microsoft Azure DocumentDB Client Library. Databázi dokumentů Azure Cosmos DB je databáze NoSQL, která poskytuje přístup s nízkou latencí k dokumentů JSON, nabízí rychlý, vysoce dostupných, škálovatelných databázová služba pro aplikace, které vyžadují bezproblémové škálování a globální replikace.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Zvyšování sofistikovanosti službami Cognitive Services](cognitive-services/index.md)

Tato příručka vysvětluje, jak používat některé z rozhraní API kognitivní služby společnosti Microsoft v aplikaci Xamarin.Forms. Kognitivní služby jsou sadu rozhraní API, sady SDK a službách, které jsou k dispozici pro vývojáře k jejich aplikacím inteligentnější přidáním funkce, jako je rozpoznávání obličeje, rozpoznávání řeči a znalosti jazyka.
