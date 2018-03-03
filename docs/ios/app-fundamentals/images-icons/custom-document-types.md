---
title: "Ikony vlastní šablony dokumentů"
description: "Tento článek obsahuje včetně a správu prostředek bitové kopie v aplikaci Xamarin.iOS má být použit jako ikona vlastní typ dokumentu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: 582fcbacbf1959e05773babb1219817ba319a937
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="custom-document-icons"></a>Ikony vlastní šablony dokumentů

_Tento článek obsahuje včetně a správu prostředek bitové kopie v aplikaci Xamarin.iOS má být použit jako ikona vlastní typ dokumentu._

Pokud aplikace Xamarin.iOS podporuje načítání typu určitému dokumentu, vývojář může poskytnout ikony, které systém použije při výskytu tohoto typu dokumentu, například když uživatel obsahuje dolů přílohy v *aplikaci Mail* jako Zde se zobrazují:

 [ ![](custom-document-types-images/17.png "Příklad ikony typu dokumentu")](custom-document-types-images/17.png)

Vývojář může přidat informace o typu dokumentu pro formátu souboru aplikace je schopen otevírání zahrnutím položky slovníku pro `CFBundleTypeName` řetězec a `LSItemContentTypes` pole v dané aplikaci `Info.plist`. Ikony typu dokumentu v přejděte `CFBundleTypeIconFiles` pole. Pokud není k dispozici ikona dokumentu, odvodí z části ikonu pro aplikace iOS.
Ikony můžete zadat několik velikostí, optimalizovaný pro rozlišení různých zařízení. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

K přiřazení tyto hodnoty v sadě Visual Studio pro Mac, použijte **typů dokumentů** oddílu pod **Upřesnit** na kartě `Info.plist` editoru přidejte typ dokumentu a přiřadit ikony bitové kopie. Například zde je snímek obrazovky zobrazující registrace pro podporu PDF:

 [ ![](custom-document-types-images/18.png "V části typů dokumentů na kartě Upřesnit v editoru 'Info.plist.")](custom-document-types-images/18.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li přiřadit tyto hodnoty v sadě Visual Studio, použijte **typů dokumentů** tématu v části **Upřesnit** na kartě `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Otevřete část typů dokumentů na kartě Upřesnit")

Klikněte **přidat typ dokumentu** tlačítko a vyplňte požadovaná pole:

![](custom-document-types-images/doc02w.png "Formuláře přidat typ dokumentu")

-----


Další informace o typech dokumentů obsahuje, najdete v článku společnosti Apple [Uniform odkaz na typ identifikátory](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) a [dokumentu interakce programování témata pro iOS](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>Související odkazy

- [Práce s obrázky (ukázka)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Vlastní ikonou a pokyny pro vytvoření bitové kopie](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
