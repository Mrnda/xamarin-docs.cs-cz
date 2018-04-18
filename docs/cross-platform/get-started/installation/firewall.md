---
title: Pokyny ke konfiguraci brány Xamarin Firewall
description: Seznam hostitelů, které je potřeba na seznam povolených adres v bráně firewall povolit platformu pro Xamarin pro práci pro vaši společnost.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 212a190b56465a8401b17b7a379a1f083d8f8d87
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="xamarin-firewall-configuration-instructions"></a>Pokyny ke konfiguraci brány Xamarin Firewall

_Seznam hostitelů, které je potřeba na seznam povolených adres v bráně firewall povolit platformu pro Xamarin pro práci pro vaši společnost._

Aby produkty Xamarin k instalaci a správně fungovat musí být přístupné a stahovat požadované nástroje a aktualizace softwaru pro určité koncové body. Pokud jste vy nebo vaše společnost nastavení striktní brány firewall, může zaznamenat problémy s instalací, licencování, komponenty a další. Tento dokument popisuje některé známé koncových bodů, které musí být seznam povolených adres v bráně firewall v pořadí pro Xamarin pro práci. Tento seznam neobsahuje koncových bodů vyžaduje se pro všechny nástroje jiných výrobců, které jsou součástí staženého souboru. Pokud po projít tento seznam se stále setkáváte problémy, podívejte se na Apple nebo Android instalace příručky pro řešení problémů.

## <a name="endpoints-to-whitelist"></a>Koncové body k seznamu povolených IP adres

### <a name="xamarin-installer"></a>Instalační program Xamarin

Následující známé adresy bude muset být přidán v pořadí pro software, který chcete nainstalovat správně, pokud používáte nejnovější verzi Instalační služby Xamarin:

-  xamarin.com (Instalační program manifesty)
-  DL.xamarin.com (umístění stahování balíčku)
-  DL.Google.com (ke stažení sady SDK pro Android)
-  download.Oracle.com (JDK)
-  VisualStudio.com (instalační balíčky stáhnout umístění)
-  go.microsoft.com (překlad adresy URL nastavení)
-  aka.MS (překlad adresy URL nastavení)

Pokud používáte Mac a jsou zjištění problémy instalace Xamarin.Android, ujistěte se, že tento systému macOS je možné stáhnout Java.


### <a name="components-store-and-nuget-including-xamarinforms"></a>Součásti úložiště a NuGet (včetně Xamarin.Forms)

Následující adresy bude třeba přidat pro přístup k úložišti součástí Xamarin nebo NuGet (Xamarin.Forms zabalený jako NuGet):

-  Components.xamarin.com (pro použití úložiště součástí Xamarin)
-  xampubdl.BLOB.Core.Windows.NET (součásti úložiště hostitelů soubory ke stažení)
-  Webová\.nuget.org (pro přístup k NuGet)
-  az320820.vo.msecnd.NET (NuGet soubory ke stažení)
-  DL-ssl.google.com (Google komponenty)


### <a name="software-updates"></a>Aktualizace softwaru

Následující adresy bude třeba přidat k zajištění, že aktualizace softwaru můžete stáhnout správně:

-  software.xamarin.com (aktualizační službu)
-  download.visualstudio.microsoft.com
-  dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac Agent

Pro připojení k sestavení Mac Visual Studio vyžaduje hostitele pomocí Xamarin Mac Agent třeba otevřít port SSH. Ve výchozím nastavení to je **Port 22**.

## <a name="summary"></a>Souhrn

Tato příručka zahrnutých koncových bodů do seznamu povolených IP adres umožňuje Xamarin produkty pro instalaci a aktualizaci správně na váš počítač.
