---
title: "Krok 2. Konfigurace služby přístupu pro mobilní aplikace"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: a679d404eace121b38463e35350b8b85ec08b1e0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>Krok 2. Konfigurace služby přístupu pro mobilní aplikace

Vždy, když všechny prostředků například webovou aplikaci, webové služby, atd. musí být zabezpečené pomocí Azure Active Directory, musí být zaregistrován. Zabezpečené aplikace nebo služby si můžete prohlédnout v části **aplikace** kartě. Tady můžete vybrat aplikaci, která je přístupná z mobilních aplikací a poskytněte přístup k němu.

1. Na **konfigurace** najděte **oprávnění k ostatním aplikacím** části:

  ![](configure-images/2.1-configure.png "Na kartě Konfigurace vyhledejte oprávnění k další části aplikací")

2.  Klikněte na **přidat aplikaci** tlačítko. Na další obrazovce automaticky otevírané okno zobrazí seznam všech aplikací, které jsou zabezpečené pomocí Azure Active Directory. Vyberte aplikace, které potřebuje k přístupu z mobilních aplikací.

  ![](configure-images/2.2-add-application.png "Vyberte aplikace, které potřebuje k přístupu z mobilních aplikací")

3. Po výběru aplikace, vyberte znovu aplikace nově přidané **oprávnění k ostatním aplikacím** části a udělit příslušná oprávnění.

  ![](configure-images/2.3-permissions.png "Po výběru aplikace, znovu vyberte aplikaci, nově přidané v oprávnění k další části aplikací a udělit příslušná oprávnění")

4. Nakonec **Uložit** konfigurace. Tyto služby by teď měly být dostupné v mobilních aplikacích!



## <a name="related-links"></a>Související odkazy

- [Ukázka Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)