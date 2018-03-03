---
title: Azure Active Directory
description: "Zaregistrovat aplikaci používat Azure Active Directory"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: bf39b394903c3322ee617289ffe583a22a39fb20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
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
