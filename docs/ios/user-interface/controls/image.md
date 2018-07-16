---
title: Zobrazení obrázků s Xamarin.iOS
description: Tento dokument popisuje, jak zobrazit obrázky v Xamarin.iOS. Přidání obrázků do aplikace zahrnují programově nebo prostřednictvím v iOS designeru.
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: 9777b4abf6e7f370178bcff2cb40666612888a9f
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038375"
---
# <a name="displaying-images-with-xamarinios"></a>Zobrazení obrázků s Xamarin.iOS

Přidání obrázků do vaší aplikace sestává ze dvou kroků: nejprve přidat Image do svého projektu; pak přidejte ovládací prvky a kódem se zobrazí na obrazovce. Odkazovat [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) najdete podrobnější pokrytí bitové kopie v Xamarin.iOS.

## <a name="adding-images-to-your-app"></a>Přidání obrázků do vaší aplikace

Image můžete přidat do libovolné složky v sadě Visual Studio for Mac řešení a pokud **akce sestavení** je nastavena na **obsahu** soubor bude součástí vaší aplikace a je možné zobrazit.

Visual Studio for Mac podporuje také speciální adresář s názvem **prostředky** , který může také obsahovat soubory bitových kopií. Soubory ve složce Resources musí mít **akce sestavení** nastavena na **BundleResource**.

Tento snímek obrazovky ukazuje **akce sestavení** možnosti, které se zobrazí při otevření souboru je klikli pravým tlačítkem myši:

 [![](image-images/image30a.png "Vytvořit nabídku akcí")](image-images/image30a.png#lightbox)

Visual Studio pro Mac zpravidla zvolit správnou **akce sestavení** automaticky, ale je třeba si uvědomit z těchto nastavení, zejména v případě, že můžete přesouvat soubory v projektu.

### <a name="adding-an-image-file"></a>Přidávání souboru obrázku

Přidání souboru obrázku do projektu, nejprve klikněte pravým tlačítkem na projekt a zvolte **přidat soubory...**

 [![](image-images/image31a.png "Přidat soubory... nabídky")](image-images/image31a.png#lightbox)

Výběr image (nebo Image) chcete zahrnout do souboru standardní dialogové okno. Výchozí akce sestavení pro Image bude **BundleResource** – nepřepíšete tuto hodnotu, pokud nemáte konkrétní důvod.

 [![](image-images/image32a.png "Přidat dialog soubory")](image-images/image32a.png#lightbox)

Na obrázku se přidají do vašeho projektu a k dispozici pro načte a zobrazí v kódu. Tento snímek obrazovky znázorňuje obrázek přidat projekt aplikace pro iOS:

 [![](image-images/image33a.png "Obrázek v projektu")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Co je adresář prostředků?

Soubory umístěné v **prostředky** directory zpracovávat odděleně od běžné soubory – obsah **prostředky** složce se zkopírují do kořenového adresáře aplikace a může být odkazováno odtud v váš kód. To může být užitečné z mnoha důvodů:

-  Ukládání imagí, určená ve vlastnostech aplikace, jako je například výchozí spouštěcí Image a ikony aplikace.
-  Ukládání další Image a soubory samostatně od kódu, takže můžete snadněji spravovat (podadresáře se zachová, i když se zkopírovat obsah adresáře zdrojů).


**Prostředky** adresář je zvláště užitečná v projektu knihovny, protože kód mohou předpokládat, že tyto Image budou zkopírovány do kořenového náročné aplikace, což usnadňuje zápis sdílených knihoven kódu, které vyžadují obrázek, zvuk, video, XML nebo jiné soubory.

**Prostředky** adresář musí být proto s názvem a všechny soubory musí mít akci sestavení nastavena na **BundleResource**.

## <a name="displaying-the-image"></a>Zobrazování obrázku

V iOS designeru, použijte **zobrazení obrázku** zobrazit obrázek nebo animovaný řadu obrázků. **Zobrazení obrázku** ikonu z panelu nástrojů je uveden níže:

 [![](image-images/image35a.png "ImageView v panelu nástrojů")](image-images/image35.png#lightbox)

Přetáhněte **zobrazení obrázku** z **nástrojů** na kontroler zobrazení. Potom v části **zobrazení obrázku > obrázku** rozevíracího seznamu vám poskytne seznam všech souborů k dispozici image ve vašem projektu. Vyberte některé z nich se přidá do zobrazení obrázku.

 [![](image-images/image36a.png "ImageView v panelu nástrojů")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>Zobrazení obrázku prostřednictvím kódu programu

Protože **SF Monkey.jpg** se nachází v kořenovém adresáři **prostředky** adresář bude k dispozici za běhu v kořenovém adresáři sady prostředků aplikace. Tento obrázek v ovládacím prvku zobrazit obrázek zobrazíte pomocí následujícího kódu:

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

Pokud jsme měli umístit image v **prostředky/Pics/SF Monkey.jpg**, pak by obsahovat kód **Pics** složky v cestě:

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

Soubor prostředků odkazuje nikdy potřeba zahrnovat symbol **prostředky** složky.

## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
