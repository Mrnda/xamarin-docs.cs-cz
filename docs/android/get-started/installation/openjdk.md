---
title: Náhled distribuce mobilních OpenJDK od Microsoftu
description: Podrobný průvodce konfigurací rozdělení OpenJDK společnosti Microsoft pro vývoj mobilních aplikací.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 2022337ebd65997c7b2492137193586278f2dffd
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573591"
---
# <a name="microsofts-mobile-openjdk-distribution-preview"></a>Náhled distribuce mobilních OpenJDK od Microsoftu

_Tento průvodce popisuje kroky pro přepnutí na verzi preview od Microsoftu distribuce OpenJDK. Toto rozdělení je určená pro vývoj pro mobilní zařízení._

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="overview"></a>Přehled

Od verze Visual Studio 15.9 a sady Visual Studio pro Mac 7.7, Visual Studio Tools for Xamarin přejde od společnosti Oracle JDK k **odlehčenou verzi OpenJDK, který je určený výhradně pro vývoj pro Android**:

![Nový pracovní postup nabízí náhled webové OpenJDK VS 15.8 Preview 5](openjdk-images/openjdk.png)

Tento přesun výhody:

- Budete mít vždy OpenJDK verze, která se dá použít pro vývoj pro Android.

- Stažení sady JDK 9 nebo 10 neovlivní. vývojové prostředí.

- Výrazně sníženou stahování velikost a nároky na místo.

- Žádné další problémy se 3. stran servery a instalační programy.

Pokud chcete přesunout na vylepšené prostředí dříve, sestavení distribuce OpenJDK mobilní aplikace Microsoft jsou k dispozici k otestování na Windows i Mac. Instalační program proces je popsán níže a můžete kdykoli vrátit zpět k Oracle JDK.

## <a name="download"></a>Stáhnout

Abyste mohli začít, stáhněte si správné sestavení pro váš systém:

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

## <a name="configure"></a>Konfigurace

Rozbalte do správného umístění:

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

> [!IMPORTANT]
> Tento příklad používá sestavení 1.8.0.9; však může být novější verze, kterou stáhnete.

Přejděte na novou sadu JDK integrovaném vývojovém prostředí:

- **Mac** &ndash; klikněte na tlačítko **nástroje > správce sady SDK > umístění** a změnit **Java SDK (JDK) umístění** úplnou cestu instalace OpenJDK. V následujícím příkladu je nastavena tato cesta **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**.

![Nastavení JDK cestu k distribuční OpenJDK mobilní aplikace Microsoft na počítači Mac](openjdk-images/vsm.png)

- **Windows** &ndash; klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu** a změnit **umístění sady Java Development Kit** úplnou cestu instalace OpenJDK. V následujícím příkladu je nastavena tato cesta **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**:

![Nastavení JDK cestu k distribuční OpenJDK mobilní aplikace Microsoft Windows](openjdk-images/vs.png)

## <a name="revert"></a>Vrátit zpět

Vrátit sadu JDK Oracle, změnit umístění sady Java SDK do cesty dříve používali Oracle JDK a znovu sestavte řešení. Na počítači Mac, můžete se vrátit k cestě Oracle JDK kliknutím **resetovat na výchozí hodnoty**.

Pokud máte potíže s distribuce OpenJDK mobilní aplikace Microsoft, oznamte prostřednictvím nástroje pro zpětnou vazbu v prostředí IDE, takže je možné sledovat a rychle opravit problémy.

## <a name="known-issues--planned-fix-dates"></a>Známé problémy a plánované oprava data

`JAVA_HOME` Proměnnou prostředí nemusí správně exportovány do sady SDK a Správce zařízení. Jako alternativní řešení můžete nastavit tato proměnná prostředí do umístění OpenJDK ve vašem počítači. Tento problém je vyřešený v 15.9 verze Preview.

## <a name="summary"></a>Souhrn

V tomto článku jste zjistili, jak nakonfigurovat svého integrovaného vývojového prostředí pomocí verze preview distribuce OpenJDK mobilní zařízení od Microsoftu, což je plánované v průběhu roku 2018 stabilní verzi.
