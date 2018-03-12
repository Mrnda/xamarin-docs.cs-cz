---
title: "Práce s velikost obrazovky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 156D6D1C-83CA-4088-BA08-40B22312269C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c64e5821c1067b64caf3afc85c0e290a354e914d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-screen-sizes"></a>Práce s velikost obrazovky

Apple Watch je k dispozici ve dvou velikost obrazovky:

- **38mm**
  - pixelů na logický 136 x 170 (272 x 340 fyzické pixelů)

- **42mm**
  - 156 x 195 logické pixelů (312 x 390 fyzické pixelech).

Velikost obrazovky byste měli vzít v úvahu při navrhování a testování vaší aplikace.

## <a name="watchos-interface-designer"></a>watchOS návrháři rozhraní

Ve výchozím nastavení se zobrazí sady Visual Studio pro Mac Návrhář sledovat rozhraní řadiče v **Any Apple Watch**.

![](screen-sizes-images/screen-any-sml.png "Návrhář zobrazí sledování rozhraní řadiče na jakýkoli Apple Watch")

Pomocí nabídky Velikost můžete upravit a zobrazte náhled vašeho scénáře na buď velikost dostupné obrazovky: **38mm** nebo **42mm**:

![](screen-sizes-images/screen-menu-sml.png "Výběr velikost 38mm nebo 42mm")

Větší velikost obrazovky někdy vykreslí obsah, který bude zkrácen nebo skryté na obrazovce menší.
Ujistěte se, že testovací obou velikostí.


### <a name="interface-design"></a>Rozhraní návrhu

Aplikace by měl zobrazit stejný obsah na obrazovce, bez ohledu na velikost a měli rozbalit nebo sbalit elementy podle potřeby. V sadě Visual Studio pro Mac Návrhář v atributu Inspector, měli byste použít **relativní ke kontejneru** nebo **velikost podle obsahu** přednostně pevné velikosti.

![](screen-sizes-images/sizeattributepanel-sml.png "Použít relativní ke kontejneru nebo velikost podle obsahu přednostně pevné velikosti")

Vzhledem k tomu, že na obrazovce sledovat je obklopená černým lůžkem, poskytuje výplně kolem vaše rozhraní se nedoporučuje. Umožní elementy rest proti okraje obrazovky a nechat lůžkem formuláři přirozené ohraničení kolem aplikace.


## <a name="watchos-simulator"></a>watchOS simulátoru

Při testování v simulátoru můžete snadno přepínat mezi dvěma obrazovky velikostí pomocí **hardwaru > zařízení** nabídky.

![](screen-sizes-images/simulator.png "Simulátor můžete přepínat mezi velikost dvě obrazovky pomocí nabídky hardwarové zařízení.")


## <a name="image-resources"></a>Prostředky obrázků

Pokud není vypadat dobře při různých velikostech jednoho datového zdroje, měli byste použít více prostředků bitové kopie. Katalog asset Image povolit pro samostatné bitmap zadat pro každou velikost:

![](screen-sizes-images/images-xcassets.png "Editor katalog asset obrázků")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Můžete taky použijte kód pro určení velikosti obrazovky a načtení různých obrázků zcela:

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

Další informace o použití [obrázek ovládacího prvku](~/ios/watchos/user-interface/image.md).



## <a name="related-links"></a>Související odkazy

- [Úvod do watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)