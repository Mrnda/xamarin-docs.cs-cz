---
title: Pokyny ke konfiguraci brány Xamarin Firewall
description: Seznam hostitelů, které je potřeba na seznam povolených adres v bráně firewall povolit platformu pro Xamarin pro práci pro vaši společnost.
ms.topic: article
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 5c6e850594e23d650dbe67126143ce7d58fcaa82
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/29/2018
---
# <a name="xamarin-firewall-configuration-instructions"></a>Pokyny ke konfiguraci brány Xamarin Firewall

_Seznam hostitelů, které je potřeba na seznam povolených adres v bráně firewall povolit platformu pro Xamarin pro práci pro vaši společnost._

Aby produkty Xamarin k instalaci a správně fungovat musí být přístupné a stahovat požadované nástroje a aktualizace softwaru pro určité koncové body. Pokud jste vy nebo vaše společnost nastavení striktní brány firewall, může zaznamenat problémy s instalací, licencování, komponenty a další. Tento dokument popisuje některé známé koncových bodů, které musí být seznam povolených adres v bráně firewall v pořadí pro Xamarin pro práci. Tento seznam neobsahuje koncových bodů vyžaduje se pro všechny nástroje jiných výrobců, které jsou součástí staženého souboru. Pokud po projít tento seznam se stále setkáváte problémy, podívejte se na Apple nebo Android instalace příručky pro řešení problémů.

## <a name="endpoints-to-whitelist"></a>Koncové body k seznamu povolených IP adres

### <a name="xamarin-installer"></a>Xamarin Installer

Následující známé adresy bude muset být přidán v pořadí pro software, který chcete nainstalovat správně, pokud používáte nejnovější verzi Instalační služby Xamarin:

-  xamarin.com (Instalační program manifesty)
-  DL.xamarin.com (umístění stahování balíčku)
-  DL.Google.com (ke stažení sady SDK pro Android)
-  download.oracle.com (JDK)
-  VisualStudio.com (instalační balíčky stáhnout umístění)
-  go.microsoft.com (překlad adresy URL nastavení)
-  aka.MS (překlad adresy URL nastavení)

Pokud používáte Mac a jsou zjištění problémy instalace Xamarin.Android, ujistěte se, že tento systému macOS je možné stáhnout Java.


### <a name="components-store-and-nuget-including-xamarinforms"></a>Součásti úložiště a NuGet (včetně Xamarin.Forms)

Následující adresy bude třeba přidat pro přístup k úložišti součástí Xamarin nebo NuGet (Xamarin.Forms zabalený jako NuGet):

-  Components.xamarin.com (pro použití úložiště součástí Xamarin)
-  xampubdl.BLOB.Core.Windows.NET (součásti úložiště hostitelů soubory ke stažení)
-  www.nuget.org (pro přístup k NuGet)
-  az320820.vo.msecnd.net (NuGet downloads)
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