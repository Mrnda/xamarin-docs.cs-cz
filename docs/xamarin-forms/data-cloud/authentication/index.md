---
title: "Ověřování přístupu k webovým službám"
description: "Tato příručka vysvětluje, jak integrovat ověřovací služby do Xamarin.Forms aplikace umožňuje uživatelům sdílet back-end přitom jenom má přístup k svá vlastní data. Obsahuje následující témata Základní ověřování pomocí služby REST, pomocí součásti Xamarin.Auth k ověřování na základě poskytovatelů identit OAuth, a pomocí předdefinované ověřovací mechanismy, které nabízí zprostředkovatelé jiný."
ms.topic: article
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 0139a7a921861b5d1c9a3639ee2c7e25ee6cf5fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-access-to-web-services"></a>Ověřování přístupu k webovým službám

_Tato příručka vysvětluje, jak integrovat ověřovací služby do Xamarin.Forms aplikace umožňuje uživatelům sdílet back-end přitom jenom má přístup k svá vlastní data. Obsahuje následující témata Základní ověřování pomocí služby REST, pomocí součásti Xamarin.Auth k ověřování na základě poskytovatelů identit OAuth, a pomocí předdefinované ověřovací mechanismy, které nabízí zprostředkovatelé jiný._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[Ověřování RESTful webová služba](rest.md)

HTTP podporuje několik ověřovací mechanismy pro řízení přístupu k prostředkům. Základní ověřování poskytuje přístup k prostředkům jenom klienty, kteří mají správné přihlašovací údaje. Tento článek ukazuje, jak používat základní ověřování k ochraně přístupu k prostředkům RESTful webová služba.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Ověřování uživatelů pomocí zprostředkovatele Identity](oauth.md)

Xamarin.Auth je SDK a platformy pro ověřování uživatelů a ukládání svých účtů. Zahrnuje ověřovací data OAuth, které poskytují podporu pro použití poskytovatelů identit, jako je například Google, Microsoft, Facebook či Twitter. Tento článek vysvětluje, jak používat ke správě procesu ověřování v aplikaci Xamarin.Forms Xamarin.Auth.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Ověřování uživatelů s Azure Mobile Apps](azure.md)

Azure Mobile Apps pomocí různých zprostředkovatelů externí identity pro podporu ověřování a autorizaci uživatelů aplikace. Oprávnění lze nastavit pak u tabulky omezit přístup jenom ověřené uživatele. Tento článek vysvětluje, jak používat Azure Mobile Apps ke správě procesu ověřování v aplikaci Xamarin.Forms.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Ověřování uživatelů s Azure Active Directory B2C](azure-ad-b2c.md)

Azure Active Directory B2C je cloudové řešení správy identity pro určených webové a mobilní aplikace. Tento článek ukazuje, jak integrovat správu identit uživatelů na aplikaci Xamarin.Forms pomocí Microsoft ověřování knihovny (MSAL) a Azure Active Directory B2C.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Integrace Azure Active Directory B2C s Azure Mobile Apps](azure-ad-b2c-mobile-app.md)

Azure Active Directory B2C můžete použít ke správě ověřování pracovního postupu pro Azure Mobile Apps. S tímto přístupem prostředí správy identit je plně definována v cloudu a můžete změnit, aniž by měnili kód mobilní aplikace. Tento článek ukazuje, jak používat Azure Active Directory B2C k ověřování a autorizace na instanci Azure Mobile Apps s Xamarin.Forms.

## <a name="authenticating-users-with-an-amazon-simpledb-serviceawsmd"></a>[Ověřování uživatelů pomocí služby Amazon SimpleDB](aws.md)

Amazon SimpleDB nenabízí svůj vlastní systém oprávnění na základě prostředků. Místo toho ověřování proti zprostředkovatele identity lze zajistit, že uživatelé v doméně SimpleDB mají jenom přístup k svá vlastní data. Tento článek vysvětluje, jak omezit přístup uživatelů k svá vlastní data SimpleDB.


## <a name="related-links"></a>Související odkazy

- [Úvod k webovým službám](~/cross-platform/data-cloud/web-services/index.md)
- [Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)
