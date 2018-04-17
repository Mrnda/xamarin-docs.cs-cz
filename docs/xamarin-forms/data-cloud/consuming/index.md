---
title: Využívání webových služeb
description: Tato příručka ukazuje, jak komunikovat se službami jiných webových vám umožňuje vytvářet, číst, aktualizovat a odstraňovat funkce (CRUD) k aplikaci Xamarin.Forms. Obsahuje následující témata komunikaci se službami ASMX, služby WCF, služby REST a Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a4c842ea7fd37ade9be0a9cb3e3ff7e50a6d1491
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="consuming-web-services"></a>Využívání webových služeb

_Tento příručka ukazuje, jak komunikovat se službami jiných webových vám umožňuje vytvářet, číst, aktualizovat a odstraňovat funkce (CRUD) k aplikaci Xamarin.Forms. Obsahuje následující témata komunikaci se službami ASMX, služby WCF, služby REST a Azure Mobile Apps.

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Využívání ASP.NET webové služby (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

ASP.NET – webové služby (ASMX) umožňují vytvářet webové služby, které odesílání zpráv přes protokol HTTP pomocí objektu přístup protokolu SOAP (Simple). SOAP je nezávislé na platformě a nezávislé na jazyku protokol pro vytváření a přístup k webovým službám. Příjemci služby ASMX není potřeba nic vědět o platformu, objektový model nebo programovací jazyk používaný k implementaci služby. Potřebují pouze pochopit, jak odesílat a přijímat zprávy protokolu SOAP. Tento článek ukazuje, jak využívají webové služby ASMX z Xamarin.Forms aplikace.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Využívají webové služby systému Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF je společnosti Microsoft jednotné rozhraní pro vytváření aplikací orientovaných na služby. Umožňuje vývojářům vytvářet zabezpečeným, spolehlivým, zpracovaných a umožňuje vzájemnou spolupráci distribuované aplikace. Jsou rozdíly mezi webové služby ASP.NET (ASMX) a WCF, ale je důležité si uvědomit, že WCF podporuje stejné funkce, které poskytuje ASMX – protokolu SOAP zprávy přes protokol HTTP. Tento článek ukazuje, jak používat služby WCF SOAP z Xamarin.Forms aplikace.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md)

Přenos REST (Representational State) je styl architektury pro vytváření webových služeb. REST jsou vytvářeny požadavky prostřednictvím protokolu HTTP použití stejných příkazů HTTP, které webových prohlížečů slouží k načtení webové stránky a k odesílání dat na servery. Tento článek ukazuje, jak využívat RESTful webová služba z Xamarin.Forms aplikace.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Použití Azure mobilní aplikace](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure Mobile Apps umožňují vývoj aplikací s škálovatelné back-EndY hostované v Azure App Service, se podpora pro mobilní ověřování, offline synchronizace a nabízených oznámení. Tento článek, který je pouze pro Azure Mobile Apps, použít back-end Node.js, vysvětluje, jak pro dotazování, vložit, aktualizovat a odstranit data uložená v tabulce v instanci Azure Mobile Apps.

## <a name="related-links"></a>Související odkazy

- [Úvod k webovým službám](~/cross-platform/data-cloud/web-services/index.md)
- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
