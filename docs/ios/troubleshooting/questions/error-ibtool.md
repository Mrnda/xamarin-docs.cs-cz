---
title: 'Chyba IBTool: Operaci nelze dokončit.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4647227ad208bfa968f8282a966220a09ab7f4a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Chyba IBTool: Operaci nelze dokončit.

## <a name="fixed-in-xcode-611"></a>V Xcode 6.1.1 opraveno

Apple [pevné](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) to `ibtool` je nejjednodušší oprava chyb v Xcode 6.1.1, takže upgrade na Xcode 6.1.1 nebo vyšší.

* * *

## <a name="description-of-the-problem"></a>Popis problému

`ibtool` Příkaz v Xcode 6.0 měl chyby v OS X 10.10 Yosemite. Xamarin.iOS používá na Xcode `ibtool` zkompilovat scénářů a `XIB` soubory.

Další informace o chybě ve vztahu k Xcode naleznete na následující Stack Overflow post: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Chybová zpráva

> V dokumentu "MainStoryboard.storyboard" nelze otevřít. Operaci nelze dokončit. (Chyba com.apple.InterfaceBuilder -1.)

## <a name="workarounds-for-xcode-60"></a>Alternativní řešení (Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Možnost 1: Spravovat všechny `UIImageView.Image` vlastností v kódu

Místo nastavení `Image` vlastnost `UIImageView` ve scénáři, nebo `.xib` souboru, můžete nastavit vlastnost v jednom zobrazení přepsání metody životní cyklus v kontroleru zobrazení (například v `ViewDidLoad()`). Viz také [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) tipy pro používání `UIImage.FromBundle()` oproti `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Možnost 2: Všechny bitové kopie prostředky přesunout do nejvyšší úrovně `Resources` složky

Po přesunutí bitové kopie na nejvyšší úroveň `Resources` složky, budete muset aktualizovat scénáři a `.xib` soubory použít nové cesty bitové kopie.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Možnost 3: Nastavte `LogicalName` pro všechny prostředky problematické bitové kopie, budou zkopírovány do nejvyšší úrovní`.app` sady

Předpokládejme například, že vaše původní `.csproj` soubor obsahuje následující položky:

`<BundleResource Include="Resources\Images\image.png" />`

Můžete změnit tento prvek a přidat `LogicalName` tak, aby bitovou kopii se místo toho zkopírují na nejvyšší úrovni `.app `sady:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

V sadě Visual Studio pro Mac `LogicalName` lze také nastavit pomocí `Resource ID` pole pro obrázek v části **zobrazení > dotyková zařízení > vlastnosti**. (Viz také: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Po této změně, budete muset aktualizovat scénáři a `.xib` soubory použít nové cesty nejvyšší úrovně bitové kopie. Visual Studio pro Mac se automaticky aktualizuje seznam autocompletions pro `Image` vlastnost v iOS Designer. V sadě Visual Studio budete muset ručně upravit cestu. IOS Designer se pak zobrazit tuto aplikaci jako chybějící bitové kopie, ale bude projekt sestavit a spustit správně.

### <a name="next-steps"></a>Další kroky

O další pomoc, kontaktujte nás, nebo pokud tento problém zůstane i po použití výše uvedené informace najdete v tématu [jaké možnosti podpory jsou dostupná pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace o možnostech kontaktní, návrhy, a také jak soubor nové chyby v případě potřeby. 

