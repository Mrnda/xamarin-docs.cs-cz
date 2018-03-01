---
title: "Chyby sestavení Android – LinkAssemblies úlohy se nezdařilo neočekávaně"
ms.topic: article
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 667381e7f27f08f8b1d8b6c1979a7d0b26775177
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Chyby sestavení Android – LinkAssemblies úlohy se nezdařilo neočekávaně

Mohou se zobrazit chybová zpráva `The "LinkAssemblies" task failed unexpectedly` při sestavení projektu Xamarin.Android, která používá formuláře. To se stane, když je aktivní linkeru (obvykle na *verze* sestavení ke snížení velikosti balíčku aplikace); a k ní dojde, protože cíle pro Android nejsou aktualizovány na nejnovější rozhraní. (Další informace: [Xamarin.Forms Android požadavky](~/xamarin-forms/get-started/installation.md#android))

Řešení tohoto problému je ujistit se, že máte nejnovější podporované verze sady SDK pro Android a nastavte **cílové rozhraní** k **použijte nejnovější nainstalované platformy**. Dále je doporučeno, abyste nastavili **cílová verze Android** k **použití Target Framework verze** a **minimální verze Android** API 15 nebo vyšší. Je to na podporovanou konfiguraci.

## <a name="setting-in-visual-studio-for-mac"></a>Nastavení v sadě Visual Studio pro Mac

1.  Klikněte pravým tlačítkem na projekt pro Android.
2.  Přejděte na **sestavení > Obecné > cíl Frameworku**.
3.  Nastavte **cílové rozhraní: použijte nejnovější nainstalované platformy**.
4.  Stále v možnosti projektu, přejděte do **sestavení > aplikace pro Android**.
5.  Nastavte **minimální verzi systému Android** do rozhraní API úrovně 15 nebo vyšší & **cílovou verzi systému Android** k **automatické - použití target framework verze**.

## <a name="setting-in-visual-studio"></a>Nastavení v sadě Visual Studio

1.  Klikněte pravým tlačítkem na projekt pro Android.
2.  Přejděte na **aplikace** v možnosti projektu.
3.  Nastavte **zkompilovat pomocí verzi systému Android** & **cílovou verzi systému Android** nastavení **použijte nejnovější platformu** / **použití Kompilace pomocí sady SDK verze**.
4.  Nastavte **minimální Android k cíli** nastavení na rozhraní API 15 nebo vyšší.

Jakmile aktualizujete tato nastavení, odstranit a znovu sestavte projekt a zda že jsou zachyceny změny.
