---
title: Kde najdu soubor .dSYM symbolicate protokoly havárií iOS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ce60c19ab0b680e00338f517e5a3f17f725ed329
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Kde najdu soubor .dSYM symbolicate protokoly havárií iOS?

Při sestavování aplikací pro iOS ze sady visual studio, končí .dSYM soubor, který slouží k symbolicate hlášení selhání na hostiteli sestavení v cestě:
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

Všimněte si, že `~/Library` je skrytá složka ve výchozím nastavení v hledání, takže když třeba má použít na vyhledávací **přejděte > přejděte do složky** nabídky a zadejte: `~/Library/Caches/Xamarin/mtbs/builds/` otevření složky.  

Případně můžete zobrazit skryté `~/Library` složky pomocí **zobrazit možnosti zobrazení** panel pro domovskou složku. Pokud vyberete domovskou složku na bočním panelu v hledání a použijte nabídku vyhledávací **zobrazení > Zobrazit možnosti zobrazení** (nebo cmd j), pak se zobrazí zaškrtávací políčko pro **zobrazit složku knihovny**.


### <a name="see-also"></a>Viz také
- Rozšířené kroky pro symbolicating iOS havárií sestavy: [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying iOS havárií protokoly aplikací](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
