---
title: "Krok 1. Zaregistrovat aplikaci používat Azure Active Directory"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 52d06dc6125f91f98e8f3ee8b4f91ad7b52347a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Krok 1. Zaregistrovat aplikaci používat Azure Active Directory

1. Přejděte na [windowsazure.com](https://manage.windowsazure.com) a přihlaste se pomocí svého Account Microsoft nebo účtu organizace na portálu Azure. Pokud nemáte předplatné Azure, můžete získat zkušební verze z [azure.com](http://www.azure.com)

2. Po přihlášení, přejděte na **služby Active Directory** (1) oddílu a vyberte adresář, ve které chcete zaregistrovat aplikaci (2)

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "část a vyberte adresář, ve které chcete zaregistrovat aplikaci")](register-images/01.-active-directory-in-azure-portal.jpg)

3. Klikněte na tlačítko **přidat** Pokud chcete vytvořit novou aplikaci, pak vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**

  [ ![](register-images/02.-add-new-application-sml.jpg "Přidat aplikaci, kterou vyvíjí Moje organizace")](register-images/02.-add-new-application.jpg)

4. Na další obrazovce pojmenujte aplikace (např. XAM-DEMO).
  Zkontrolujte, zda jste vybrali **nativní klientská aplikace** jako typ aplikace.

  ![](register-images/03.-app-name.jpg "Ujistěte se, že vyberete nativní klientská aplikace jako typ aplikace")

5. Na obrazovce konečné poskytují **identifikátor URI pro přesměrování* , je pro vaši aplikaci jedinečný, protože se vraťte na tento identifikátor URI po dokončení ověření.

  ![](register-images/04.-app-redirect.jpg "Na obrazovce konečné zadejte identifikátor URI přesměrování, které jsou jedinečné pro aplikace, jako je vrátit do tento identifikátor URI, po dokončení ověření")

6. Po vytvoření aplikace přejděte do **konfigurace** kartě. Zapište **ID klienta** který použijeme v naší aplikaci později. Na této obrazovce můžete také poskytnout přístup k vaší mobilní aplikaci do služby Active Directory nebo přidat jiná aplikace, například webového rozhraní API nebo Office 365, který můžete použít mobilní aplikace po dokončení ověřování.

    ![](register-images/05.-configure.jpg "Také na této obrazovce můžete poskytnout přístup k vaší mobilní aplikaci do služby Active Directory nebo přidat jiná aplikace, například webového rozhraní API nebo Office 365")



## <a name="related-links"></a>Související odkazy

- [Ukázka Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
