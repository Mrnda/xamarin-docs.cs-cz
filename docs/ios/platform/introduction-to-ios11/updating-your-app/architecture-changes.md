---
title: Architektura změny v iOS 11
description: Tento dokument popisuje vyřazení 32bitové aplikace v iOS 11. Popisuje, jak se aktualizace aplikace k cílové 64bitové architektury.
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 09d528ceb0654debd7c0ac8818f19c622775eac2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787432"
---
# <a name="architecture-changes-in-ios-11"></a>Architektura změny v iOS 11

Jednou z největších změn, které byste měli vědět o s iOS 11 je 32-bit podpory pro aplikace, podle popisu v [společnosti Apple](https://developer.apple.com/news/?id=06282017b) stiskněte verze. Všech nových aplikací a aktualizací na stávající aplikace musí podporovat 64-bit. 32bitová verze aplikace **nespustí** v iOS 11.

Chcete-li aktualizovat aplikaci v sadě Visual Studio pro Mac, použijte následující kroky:

1. Klikněte pravým tlačítkem na název projektu otevřete **možnosti projektu**.
2. Vyhledejte **iOS sestavení** kartě.
3. Nastavte **podporované architektury** rozevírací seznam pro **x86_64** pro **ladění | iPhoneSimulator** a **verze | iPhoneSimulator**:

    ![Změnit simulátoru architektury rozevíracího seznamu](architecture-changes-images/image1.png)

4. Pro zařízení s iOS, změňte konfiguraci, aby **ladění | iPhone** a nastavte **podporované architektury** rozevírací seznam pro **ARM64**:

    ![Změňte zařízení architektury rozevíracího seznamu](architecture-changes-images/image2.png)

5. Změňte konfiguraci tak, aby **verze | iPhone** a nastavte **podporované architektury** rozevírací seznam pro **ARM64**.

Další informace o 32bitové a 64bitové verze architektury, najdete v článku [32 nebo 64bitový platformy aspekty](~/cross-platform/macios/32-and-64/index.md#ios) průvodce.

## <a name="related-links"></a>Související odkazy

- [Co je nového v iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Stránka produktu aktualizované obchodu s aplikacemi (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizace aplikace pro iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
