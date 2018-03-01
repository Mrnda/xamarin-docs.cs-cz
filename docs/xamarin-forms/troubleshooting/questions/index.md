---
title: "Nejčastější dotazy"
ms.topic: article
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 95728bf6bf1009db1cc834bf1d9d0be6a8fc5ef5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="frequently-asked-questions"></a>Nejčastější dotazy


## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Můžete výchozí šablonu Xamarin.Forms aktualizovat na novější balíček NuGet](update-forms-template.md)
Tato příručka používá jako příklad šablony Xamarin.Forms PCL, ale stejnou metodu Obecné bude fungovat i pro šablonu Xamarin.Forms sdílených projektů. 

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Proč nefunguje návrháře Visual Studio XAML pro soubory Xamarin.Forms XAML?](forms-xaml-designer.md)
Xamarin.Forms nepodporuje aktuálně vizuální nástroje pro soubory XAML.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Chyby sestavení Android: neočekávaně selhaly "LinkAssemblies" úlohy](android-linkassemblies-error.md)
Mohou se zobrazit chybová zpráva `The "LinkAssemblies" task failed unexpectedly` při sestavení projektu Xamarin.Android, která používá formuláře. To se stane, když je aktivní linkeru (obvykle na *verze* sestavení ke snížení velikosti balíčku aplikace); a k ní dojde, protože cíle pro Android nejsou aktualizovány na nejnovější rozhraní. 


## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["Proč můj projektu Xamarin.Forms.Maps Android skončit COMPILETODALVIK: došlo k NEOČEKÁVANÉ CHYBĚ nejvyšší úrovně?"](maps-compiletodalvik-error.md)
Tato chyba může se zobrazí v panelu pro chybu sady Visual Studio pro Mac, nebo v okně výstupu sestavení sady Visual Studio; v systému Android projektů pomocí Xamarin.Forms.Maps. Nejčastěji se nevyřeší zvětšením velikosti haldy Java pro projekt Xamarin.Android.

