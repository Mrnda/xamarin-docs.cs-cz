---
title: Nasazení zaškrtávací políčka zakázaná v nástroji Configuration Manager
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: c0f116565a2741c62a00ed2a255cfde8c57b8569
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Nasazení zaškrtávací políčka zakázaná v nástroji Configuration Manager

Od verze Xamarin 3.5, Xamarin.iOS projekty nasazení automaticky vždy, když stisknete **spustit** tlačítka panelu nástrojů nebo vybrat **ladění > Spustit ladění** položku nabídky. Stále je nutné nastavit požadovanou projekt aplikace Xamarin.iOS jako **spouštěný projekt** před dřív, než spustíte buď těchto příkazů.

Z toho důvodu **nasadit** zaškrtávací políčka záměrně jsou zakázány ve Visual Studio Správci konfigurace systému pro Xamarin.iOS projekty:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager zobrazující políčka 'Nasadit' zakázána pro Xamarin.iOS projekt Xamarin 3.5")

Tato změna eliminuje chybu, která by mohla při projekt aplikace Xamarin.iOS nebyl nastaven na nasazení zobrazují ve starších verzích Xamarin (verze 3.3 a starší):

![](deploy-checkboxes-images/error.png "Dialogové okno chyby: iPhoneApp1 projektu musí být nasazené, než může být spuštěn. Ověřte, zda že je vybrán projekt nasazení v nástroji Configuration Manager řešení.")
