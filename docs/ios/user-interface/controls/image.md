---
title: "Zobrazení obrázků"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 71f3774c12add26e818b0859cf90c17ab6358538
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="displaying-images"></a>Zobrazení obrázků

Přidání bitové kopie do vaší aplikace vyžaduje dva kroky: nejprve přidejte bitové kopie do projektu; pak přidejte ovládacími prvky a kódem jejich zobrazení na obrazovce. Odkazovat [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) článek podrobnější pokrytí bitové kopie v Xamarin.iOS.

## <a name="adding-images-to-your-app"></a>Přidání bitové kopie do vaší aplikace

Bitové kopie mohou být přidány do libovolné složky ve vaší sadě Visual Studio pro Mac řešení a pokud **akce sestavení** je nastaven na **obsahu** soubor bude součástí vaší aplikace a je možné zobrazit.

Visual Studio pro Mac také podporuje speciální adresář s názvem prostředky, které může také obsahovat soubory obrázků. Soubory ve složce prostředky musí mít **akce sestavení** nastavena na **BundleResource**.

Tento snímek obrazovky ukazuje **akce sestavení** možnosti, které se zobrazí při otevření souboru je klepli pravým tlačítkem myši:

 [ ![](image-images/image30a.png "Nabídka Akce sestavení")](image-images/image30a.png)

Visual Studio pro Mac obvykle vyberte správný **akce sestavení** automaticky, ale byste měli vědět z těchto nastavení, zejména v případě, že můžete přesouvat soubory v projektu.

### <a name="adding-an-image-file"></a>Přidání souboru obrázku

Chcete-li přidat do projektu soubor obrázku, nejprve klikněte pravým tlačítkem na projekt a zvolte **přidat soubory...**

 [ ![](image-images/image31a.png "Přidání souborů... nabídky")](image-images/image31a.png)

Vyberte bitovou kopii (nebo bitové kopie) chcete zahrnout v dialogovém okně standardní soubor. Výchozí akce sestavení pro bitové kopie bude **BundleResource** – nepotlačí tuto hodnotu, pokud nemáte konkrétní důvod, proč.

 [ ![](image-images/image32a.png "Přidat soubory – dialogové okno")](image-images/image32a.png)

Obrázek se přidá do projektu a k dispozici pro načtení a zobrazí v kódu. Tento snímek obrazovky ukazuje image přidat do projektu aplikace iOS:

 [ ![](image-images/image33a.png "Bitové kopie v projektu")](image-images/image33a.png)

### <a name="what-is-the-resources-directory"></a>Co je adresář prostředky?

Soubory umístěné v adresáři prostředky jsou zpracovávat odděleně od regulární soubory – obsah složky prostředky se zkopírují do kořenového adresáře aplikace a můžete na něj odkazovat z ní ve vašem kódu. To může být užitečné z mnoha důvodů:

-  Ukládání bitových kopií ve vlastnostech aplikace, jako je například výchozí spouštěcí Image a ikony aplikace.
-  Ukládání jiné bitové kopie a soubory nezávisle na kód, proto jsou snazší správa (podadresáře se zachová, i když se zkopírují obsah adresáře prostředky).


Adresář prostředků je obzvláště užitečná v projektu knihovny vzhledem k tomu, že kód můžete předpokládat, že tyto bitové kopie se zkopírují do kořenové spotřebitelskou aplikací, což usnadňuje zápisu knihovny sdíleného kódu, které vyžadují image, zvuk, video, XML nebo jiné soubory.



Adresář prostředků musí mít název, a všechny soubory musí mít akce sestavení nastavena na **BundleResource**

## <a name="displaying-the-image"></a>Zobrazení bitovou kopii

Zobrazíte bitovou kopii pomocí návrháře zobrazení obrazu slouží jako kontejner a zobrazit jedinou bitovou kopii nebo animace bitových kopií. **Image zobrazení** ikonu z panelu nástrojů je zobrazena níže:

 [ ![](image-images/image35a.png "ImageView v sadě nástrojů")](image-images/image35.png)

Přetáhněte **bitové kopie zobrazení** z **Toobox** do řadiče zobrazení. Potom v části ** Image zobrazení > Image ** rozevíracího seznamu vám poskytne seznam všech souborů bitové kopie k dispozici ve vašem projektu. Vyberte některé z těchto přidat do bitové kopie zobrazení.

 [ ![](image-images/image36a.png "ImageView v sadě nástrojů")](image-images/image36.png)

### <a name="displaying-the-image-programmatically"></a>Zobrazení bitovou kopii prostřednictvím kódu programu

Protože blocks.jpg je umístěný v kořenovém adresáři prostředky budou dostupné v době běhu kořenové sadu aplikací. Chcete-li zobrazit tuto bitovou kopii v ImageView ovládací prvek použít následující kód:

```csharp
imageview1.Image = UIImage.FromBundle ("SF Monkey.png");
```

Pokud jsme měl umístit bitovou kopii v `/Resources/Pics/blocks.jpg` pak kód by mělo zahrnovat obrazy složky v cestě:

```csharp
imageview1.Image = UIImage.FromBundle ("Pics/SF Monkey.png");
```

Zdrojový soubor odkazuje nikdy potřeba zahrnovat `Resources` složky.


## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
