---
title: Xamarin.Android a Java Development Kit 9
description: Tento článek vysvětluje, jak vyřešit chyby Java Development Kit (JDK) 9 v Xamarin.Android.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 529062f820cd682dc6a9c22f706dbceecef1c836
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269670"
---
# <a name="xamarinandroid-and-java-development-kit-9"></a>Xamarin.Android a Java Development Kit 9

_Tento článek vysvětluje, jak vyřešit chyby Java Development Kit (JDK) 9 v Xamarin.Android._


## <a name="overview"></a>Přehled

Xamarin.Android používá Java Development Kit (JDK) pro integraci sady Android SDK pro vytváření aplikací pro Android a systémem Android návrháře. Nejnovější verze sady SDK pro Android (API 24 a vyšší) vyžadují JDK 8 (1.8). **Protože k dispozici z Google Android SDK nástroje nejsou kompatibilní s JDK 9, Xamarin.Android nefunguje s JDK 9.**

## <a name="jdk-9-errors"></a>JDK 9 chyby

Pokud se pokusíte vytvoření projektu Xamarin.Android pomocí JDK 9 nainstalovaná, zobrazí se explicitní chyba oznamující, že není podporovaný JDK 9.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Chcete-li vyřešit tyto chyby, musíte nainstalovat JDK 8 (1.8), jak je popsáno v [jak aktualizovat verzi jazyka Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md).


## <a name="checking-the-jdk-version"></a>Kontrola verze JDK

Můžete zkontrolovat, zjistěte, kterou verzi jazyka Java instalaci tak, že zadáte následující příkaz (sadu JDK `bin` adresář se musí nacházet ve vašem `PATH`):

```shell
java -version
```

Pokud JDK 9 nainstalovaná, zobrazí se zpráva podobná této:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Pokud je nainstalovaná JDK 9, je nutné nainstalovat Java JDK 8 (1.8). Informace o tom, jak nainstalovat JDK 8 najdete v tématu [jak aktualizovat verzi jazyka Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

Všimněte si, že nemáte odinstalovat JDK 9; Nicméně je nutné zajistit, že Xamarin používá JDK 8, místo JDK 9. V sadě Visual Studio, klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu**. Pokud **Java Development Kit umístění** není nastavený na JDK 8 umístění (například **C:\\Program Files\\Java\\jdk1.8.0_111**), klikněte na tlačítko **změn**  a nastavte ji na umístění, kde je nainstalován JDK 8. V sadě Visual Studio pro Mac, přejděte na **Předvolby > Projekty > SDK umístění > Android > Java SDK (JDK)** a klikněte na tlačítko **Procházet** k aktualizaci této cestě.

## <a name="known-issues-with-jdk-9"></a>Známé problémy s JDK 9

### <a name="apksigner"></a>apksigner

Je známý problém s apksigner a JDK 9, ve kterém `apksigner.bat` vyvolá souboru `apksigner.jar` s `-Djava.ext.dirs` místo `-classpath` která očekává JDK 9. Doporučujeme používat JDK 8 (1.8). Informace o tom, jak nainstalovat JDK 8 najdete v tématu [jak aktualizovat verzi jazyka Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

Pokud jste nainstalovali JDK 9, ujistěte se, zda následující cesta není nastaven na vaše `PATH` proměnné prostředí, jak se budou i nadále odkazovat JDK 9: `C:\ProgramData\Oracle\Java\javapath`. Po odebrání, `java-version` v příkazovém řádku by měl zobrazit JDK 8.
