---
title: Ikony obchodu s aplikacemi v Xamarin.iOS
description: Tento dokument popisuje, jak používat katalog asset ke správě ikonu App Store pro aplikace pro Xamarin.iOS. Ikony obchod byly dříve, spravované s iTunes připojit.
ms.prod: xamarin
ms.assetid: BFB5665A-F557-46E1-B35E-870CC2026AD9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/26/2017
ms.openlocfilehash: 749dbf01af382a54fe24652706f6a605ac7b20b4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783607"
---
# <a name="app-store-icons-in-xamarinios"></a>Ikony obchodu s aplikacemi v Xamarin.iOS

Před Xcode 9 byly přidány všechny ikony obchodu s aplikacemi prostřednictvím iTunes připojit. Je to ale už tento případ. Ikony obchod teď musí být zahrnut jako součást vaší projektu sady a přidat v rámci katalog asset. Aplikace, které neobsahují ikonu App Store budou odmítnuty společností Apple.

Ikona úložiště aplikace je písmo vaší aplikace pro uživatele, takže musí být snadno zapamatovatelný a zobrazení dobře na malou velikost. Čistá, jednoduchý a okamžitě rozpoznatelném jsou snadno zapamatovatelný ikony.

Apple navrhuje následující pokyny při navrhování vaší ikona aplikace:

- Ujistěte se, ikonu vhodné pro vaši aplikaci.
- Vytvoření jednoduchého ikonu, která je konzistentní s návrhem vaší aplikace.
- Nepoužívejte slova ve vašem ikonu.
- Vezměte v úvahu globálně: ikonou jedna aplikace se používá v všechny oblasti úložiště.

Obrázku 1 024 x 1 024 pixelů je vyžadována pro ikonu aplikace, která se zobrazí v úložišti aplikací.  Apple uvedli, že ikonu úložiště pro aplikace v katalogu asset nemůže být průhledná ani obsahovat kanálu alfa.

Další informace najdete v tématu společnosti Apple [iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/image-size-and-resolution/).

## <a name="adding-an-app-store-icon"></a>Přidání ikonu obchodu s aplikacemi

Ikony aplikace úložiště by měl nyní doručil katalog asset. 

Chcete-li přidat ikonu obchod takto:

1. Vyhledejte **AppIcon** image nastavit **Assets.xcassets** souboru projektu. 
    - Musí mít všechny nové projekty **Assets.xcassets** soubor, který obsahuje sadu AppIcon bitové kopie.
    - Chcete-li přidat nový katalog asset, klikněte pravým tlačítkem na projekt a vyberte **Přidat > Nový soubor > Katalog Asset**.
    - Chcete-li přidat nový, sady image ikon aplikace, klikněte pravým tlačítkem v oblasti sady ikonu a vyberte **ikony aplikace & spuštění bitové kopie > novou ikonu aplikace**:
    
    ![Přidat novou bitovou kopii sady možnost](app-store-icon-images/image1.png)

2. Přejděte k položce **obchod** ikonu v seznamu:

    ![Ikona obchodu s aplikacemi](app-store-icon-images/image2.png)

3. Klikněte na ikonu a vyhledejte image 1 024 x 1 024 pixelů. Uložte katalogu Asset Catalog.




## <a name="related-links"></a>Související odkazy

- [Správa ikony s Asset katalogů](~/ios/app-fundamentals/images-icons/app-icons.md#managing)
