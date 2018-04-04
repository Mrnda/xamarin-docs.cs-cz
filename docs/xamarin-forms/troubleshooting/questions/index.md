---
title: Nejčastější dotazy
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: c0c8a6f4736bdcbb028425296f2e05dd500294d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="frequently-asked-questions"></a>Nejčastější dotazy


## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Můžu aktualizovat výchozí šablonu Xamarin.Forms na novější balíček NuGet?](update-forms-template.md)
Tato příručka používá jako příklad šablony Xamarin.Forms PCL, ale stejnou metodu Obecné bude fungovat i pro šablonu Xamarin.Forms sdílených projektů. 

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Proč Návrhář XAML v sadě Visual Studio nefunguje pro soubory XAML Xamarin.Forms?](forms-xaml-designer.md)
Xamarin.Forms nepodporuje aktuálně vizuální nástroje pro soubory XAML.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Chyba sestavení Androidu: Úloha LinkAssemblies neočekávaně selhala.](android-linkassemblies-error.md)
Mohou se zobrazit chybová zpráva `The "LinkAssemblies" task failed unexpectedly` při sestavení projektu Xamarin.Android, která používá formuláře. To se stane, když je aktivní linkeru (obvykle na *verze* sestavení ke snížení velikosti balíčku aplikace); a k ní dojde, protože cíle pro Android nejsou aktualizovány na nejnovější rozhraní. 


## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["Proč můj projektu Xamarin.Forms.Maps Android skončit COMPILETODALVIK: došlo k NEOČEKÁVANÉ CHYBĚ nejvyšší úrovně?"](maps-compiletodalvik-error.md)
Tato chyba může se zobrazí v panelu pro chybu sady Visual Studio pro Mac, nebo v okně výstupu sestavení sady Visual Studio; v systému Android projektů pomocí Xamarin.Forms.Maps. Nejčastěji se nevyřeší zvětšením velikosti haldy Java pro projekt Xamarin.Android.

