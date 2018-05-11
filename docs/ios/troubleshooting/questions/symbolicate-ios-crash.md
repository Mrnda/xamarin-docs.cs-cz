---
title: Kde najdu soubor .dSYM symbolicate protokoly havárií iOS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/09/2018
ms.openlocfilehash: 60d897be8739ff5b78a322bc4ea3f43011785bb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Kde najdu soubor .dSYM symbolicate protokoly havárií iOS?

Při vytváření aplikace pro iOS pomocí sady Visual Studio pro Mac nebo Visual Studio 2017, .dSYM soubor, který je potřeba k symbolicate havárií sestavy budou umístěny ve stejné hierarchii adresáře jako soubor projektu vaší aplikace (.csproj). Přesné umístění závisí na nastavení sestavení projektu:

- Pokud jste povolili sestavení pro konkrétní zařízení .dSYM naleznete v následujícím adresáři:

    **&lt;adresáři projektu&gt;/bin/&lt;platformy&gt;/&lt;konfigurace&gt;/device-builds /&lt;zařízení&gt; - &lt; verze operačního systému&gt;/**

    Příklad:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- Pokud jste nepovolili sestavení pro konkrétní zařízení, .dSYM naleznete v následujícím adresáři:

    **&lt;adresáři projektu&gt;/bin/&lt;platformy&gt;/&lt;konfigurace&gt;/**

    Příklad:

    **TestApp/bin/iPhone/Release /**

> [!NOTE]
> Jako součást procesu sestavení Visual Studio 2017 zkopíruje soubor .dSYM na hostiteli sestavení Mac a Windows. Pokud nevidíte .dSYM souboru v systému Windows, ujistěte se, nakonfigurujete nastavení sestavení vaší aplikace do [vytvořit soubor IPA](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>Viz také

- [Symbolicating iOS havárií soubory (Xamarin.iOS)](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying iOS havárií protokoly aplikací](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

