---
title: Možnosti linkeru Xamarin.Mac
description: Propojení je výkonný optimalizace nástroj, který snižuje velikost aplikace odebráním nepoužívané kódu.
ms.prod: xamarin
ms.assetid: F03176C3-F8D4-4DE8-870C-7F27D8CE525A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f98953574f33612395500787a09351d2ba451802
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-linker-options"></a>Možnosti linkeru Xamarin.Mac

_Propojení je výkonný optimalizace nástroj, který snižuje velikost aplikace odebráním nepoužívané kódu._

## <a name="overview"></a>Přehled

Na základě [cílové rozhraní](~/mac/platform/target-framework.md) používá projektu, může být k dispozici možnosti linkeru omezený. Toto je vzhledem k tomu, že propojení vyžaduje vytvoření grafu objektu každých typu používané aplikace, a to není možné v úplné (nebo Unsupported) z důvodu System.Configuration.

K dispozici jsou čtyři možnosti:

- **Žádný** – zakázat všechna propojení. Výchozí hodnota v konfiguraci ladění v moderních a všechny konfigurace v plném znění.
- **SDK** – odkazy všechny sestavení sady SDK, s výjimkou sestavení uživatele. Výchozí hodnota je v rámci konfigurace verze v moderních. K dispozici na úplné.
- **Úplné** – odkaz ve všech sestaveních. To vyžaduje bezpečné, viz linkeru uživatelského kódu [poznámky](~/ios/deploy-test/linker.md) Další informace. K dispozici na úplné.
- **Platforma** – odkaz právě Xamarin.Mac.dll. Níže naleznete podrobnosti.

## <a name="platform-linking"></a>Platforma propojení

Propojení aplikací s použitím kompletní [cílové rozhraní](~/mac/platform/target-framework.md) je obecně nebezpečné, ale existuje několik scénářů, kdy je potřeba propojení velmi omezené.

Například aplikace, které jsou odeslána do systému macOS obchod nesmí odkazovat na počet nepoužívané rozhraní API (například QTKit), z nichž některé Xamarin.Mac obsahuje vazeb. I v případě, že aplikace nevyvolá tyto vazby, volání bude existovat v sadě SDK a odmítnuty.

Propojování platformy předpokládá aplikaci a BCL linkeru unsafe a právě Odeberte nepoužívané kód z Xamarin.Mac.dll. 

Všechny aplikace na typech Xamarin.Mac.dll neodrážejí zobrazí zlepšení menší spuštění z odebrání nepotřebných typy.

Platforma propojení je obecně pouze užitečné pro aplikace pomocí úplné cílové rozhraní, i moderních aplikací můžete použít možnost výkonnější SDK.

## <a name="setting-the-linker-configuration"></a>Nastavení konfigurace linkeru

Chcete-li změnit konfiguraci linkeru pro projekt Xamarin.Mac, postupujte takto:

1. Otevřete projekt Xamarin.Mac v sadě Visual Studio for Mac.
2. V **Průzkumníku řešení**, poklikejte na soubor projektu otevřete **možnosti projektu** dialogové okno.
3. Z **sestavení Mac** , vyberte typ **Linkeru chování** která vyhovuje potřebám vaší aplikace:

  ![Vyberte, jaké chování linkeru používat](linker-images/link-behavior.png "zvolte jaké chování linkeru používat")

4. Platforma propojení pro úplné cílové rozhraní se nezobrazí v prostředí IDE dokud budoucí aktualizaci. Do té doby se přidat `--linkplatform` k **mmp další argumenty** místo.
5. Klikněte **OK** tlačítko uložte provedené změny.


## <a name="related-links"></a>Související odkazy

- [Propojení v systému iOS](~/ios/deploy-test/linker.md)
