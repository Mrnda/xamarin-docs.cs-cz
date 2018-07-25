---
title: Společnosti Microsoft OpenJDK distribuce ve verzi Preview
description: Podrobný průvodce Konfigurace distribuce OpenJDK od Microsoftu.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 6c1346918ca6881e551f6c5b89ab16ad13d3f804
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242545"
---
# <a name="microsofts-openjdk-distribution-preview"></a>Společnosti Microsoft OpenJDK distribuce ve verzi Preview

_Tento průvodce popisuje kroky pro přepnutí na verzi preview od Microsoftu distribuce OpenJDK._

![Funkce ve verzi Preview](~/media/shared/preview.png)

## <a name="overview"></a>Přehled

Od verze Visual Studio 15.9 a sady Visual Studio pro Mac 7.7, Visual Studio Tools for Xamarin přejde od společnosti Oracle JDK na zjednodušené verzi OpenJDK, který je určený výhradně pro vývoj pro Android:

![Nový pracovní postup nabízí náhled webové OpenJDK VS 15.8 Preview 5](openjdk-images/openjdk.png)

Tento přesun výhody:

- Budete mít vždy OpenJDK verze, která se dá použít pro vývoj pro Android.

- Stažení sady JDK 9 nebo 10 neovlivní. vývojové prostředí.

- Výrazně sníženou stahování velikost a nároky na místo.

- Žádné další problémy se 3. stran servery a instalační programy.

Pokud chcete přesunout na vylepšené prostředí dříve, sestavení distribuce Microsoft OpenJDK jsou k dispozici k otestování na Windows i Mac. Instalační program proces je popsán níže a můžete kdykoli vrátit zpět k Oracle JDK.

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

![Nastavení cesty JDK distribuce Microsoft OpenJDK na počítači Mac](openjdk-images/vsm.png)

- **Windows** &ndash; klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu** a změnit **umístění sady Java Development Kit** úplnou cestu instalace OpenJDK. V následujícím příkladu je nastavena tato cesta **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**:

![Nastavení JDK cestu k distribuční Microsoft OpenJDK Windows](openjdk-images/vs.png)

## <a name="revert"></a>Vrátit zpět

Vrátit sadu JDK Oracle, změnit umístění sady Java SDK do cesty dříve používali Oracle JDK a znovu sestavte řešení. Na počítači Mac, můžete se vrátit k cestě Oracle JDK kliknutím **resetovat na výchozí hodnoty**.

Pokud máte potíže s distribuce Microsoft OpenJDK, oznamte prostřednictvím nástroje pro zpětnou vazbu v prostředí IDE, takže je možné sledovat a rychle opravit problémy.

## <a name="known-issues--planned-fix-dates"></a>Známé problémy a plánované oprava data

`JAVA_HOME` Proměnnou prostředí nemusí správně exportovány do sady SDK a Správce zařízení. Jako alternativní řešení můžete nastavit tato proměnná prostředí do umístění OpenJDK ve vašem počítači. Tento problém je vyřešený v 15.9 verze Preview.

## <a name="summary"></a>Souhrn

V tomto článku jste zjistili, jak nakonfigurovat svého integrovaného vývojového prostředí pomocí verze preview OpenJDK distribuce společnosti Microsoft, který je plánované v průběhu roku 2018 stabilní verzi.
