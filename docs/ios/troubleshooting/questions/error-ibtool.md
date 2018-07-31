---
title: 'Chyba IBTool: Operaci nelze dokončit.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 593706de23d370d6bff03c97bb50e064d77a52ef
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350729"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Chyba IBTool: Operaci nelze dokončit.

## <a name="fixed-in-xcode-611"></a>Opraveno v Xcode 6.1.1

Apple [oprava](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) to `ibtool` je nejjednodušší oprava chyby v Xcode 6.1.1, takže upgrade na Xcode 6.1.1 nebo vyšší.

* * *

## <a name="description-of-the-problem"></a>Popis problému

`ibtool` Došlo k příkazu v Xcode 6.0 na OS X 10.10 Yosemite chybu. Xcode používá Xamarin.iOS `ibtool` ke kompilaci scénářů a `XIB` soubory.

Další informace o chybě ve vztahu k Xcode najdete na následující příspěvek Stack Overflow: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Chybová zpráva

> Dokument "MainStoryboard.storyboard" nebylo možné otevřít. Operaci nešlo dokončit. (Chyba com.apple.InterfaceBuilder -1.)

## <a name="workarounds-for-xcode-60"></a>Alternativní řešení (pro Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Možnost 1: Spravovat všechny `UIImageView.Image` vlastností v kódu

Místo nastavení `Image` vlastnost `UIImageView` ve scénáři nebo `.xib` soubor, můžete nastavit vlastnosti v jednom zobrazení přepsání metody životní cyklus v kontroleru zobrazení (například v `ViewDidLoad()`). Viz také [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) tipy pro používání `UIImage.FromBundle()` vs. `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Možnost 2: Přesuňte všechny prostředky obrázků na nejvyšší úrovni `Resources` složky

Po přesunutí bitové kopie na nejvyšší úrovni `Resources` složky, budete muset aktualizovat scénář a `.xib` soubory použít nové cesty obrazu.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Možnost 3: Nastavte `LogicalName` pro všechny prostředky obrázků problematické tak, aby se zkopírovat na nejvyšší úrovni`.app` sady prostředků

Předpokládejme například, že váš původním `.csproj` soubor obsahuje následující položky:

`<BundleResource Include="Resources\Images\image.png" />`

Můžete změnit tento prvek a přidat `LogicalName` tak, aby image místo toho budou zkopírovány na nejvyšší úrovni `.app `sady prostředků:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

V sadě Visual Studio for Mac `LogicalName` lze také nastavit pomocí `Resource ID` pole pro obrázek v rámci **zobrazení > panely > vlastnosti**. (Viz také: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Po této změně, budete muset aktualizovat scénář a `.xib` soubory použít nové cesty obrazu nejvyšší úrovně. Visual Studio pro Mac bude automaticky aktualizovat seznam autocompletions pro `Image` vlastnost v iOS designeru. V sadě Visual Studio budete muset ručně upravit cestu. IOS Designer se potom zobrazit tuto aplikaci jako chybějící image, ale bude projekt sestavit a spustit správně.

### <a name="next-steps"></a>Další kroky

Potřebujete další pomoc, kontaktujte nás, nebo pokud tento problém i po použití výše uvedené informace, najdete [jaké možnosti podpory jsou k dispozici pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace týkající se možností kontaktu, návrhy, jak se dá soubor s novou chybou, v případě potřeby. 

