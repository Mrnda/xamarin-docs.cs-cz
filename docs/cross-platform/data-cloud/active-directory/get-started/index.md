---
title: Azure Active Directory
description: Tento dokument popisuje kroky, které musí být sledována umožňující mobilních aplikací k ověření službou Azure Active Directory.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ca33422817f19dbb0a04e8870800d3f5efa8af2a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780060"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Zaregistrovat aplikaci používat Azure Active Directory_

Azure Active Directory umožňuje vývojářům zabezpečeným prostředkům, jako jsou soubory, odkazy a webová rozhraní API pomocí stejného účtu organizace, která zaměstnanci používají a přihlaste se k jejich systémy, nebo zaškrtněte jejich e-mailů.

Vývoj mobilních aplikací, které můžete ověřit pomocí služby Azure Active Directory zahrnuje tři kroky.
První dva kroky jsou obvykle stejné, bez ohledu na to, jaké služby, kterou plánujete použít. Třetí krok se liší pro každý typ služby:

  1. [Probíhá registrace ve službě Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) na *windowsazure.com* portál, pak
  2. [Konfigurace služby](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Vývoj mobilních aplikací pomocí služby.

Příklady různých služeb, kterého budete mít přístup:

- [Rozhraní Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Webové rozhraní API
- Office365


## <a name="conclusion"></a>Závěr

Pomocí kroků výše můžete může ověřit své mobilní aplikace pro Azure Active Directory. Active Directory Authentication Library (ADAL) je mnohem jednodušší méně řádků kódu, a zajistit přitom ochranu většinu kódu stejné a a tím ke sdílení napříč platformami.



## <a name="related-links"></a>Související odkazy

- [Ukázka Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
