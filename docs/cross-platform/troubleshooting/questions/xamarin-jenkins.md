---
title: Proč se pomocí Xamarin nepodporuje Jenkins?
description: Tento dokument popisuje na vysoké úrovni Xamarinu pro interakci se systémem Jenkins CI. Také popisuje několik běžných problémů, ke kterým při práci s Jenkinsem.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: 54947d04d6241120df4b3a0f60947ed5cc2b7ffd
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351156"
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Proč se pomocí Xamarin nepodporuje Jenkins?

## <a name="jenkins-support-explanation"></a>Vysvětlení podporu Jenkinse

Jenkins je open source sada CI; z důvodu tolik problémů, které jsou přímo způsobeny Jenkinse *samotného* bude muset být zaznamenávané jako problémy, proti které jste získali kód; například [hlavním úložišti Jenkins](https://github.com/jenkinsci/jenkins), nebo úložiště pro [ Jenkins.App](https://github.com/stisti/jenkins-app).

Výjimkou je na problémy, které můžou být izolované na konkrétní chyby v Xamarinu pro nástroje. Pokud se domníváte, že to v případě můžete zkontrolovat vaše [možnosti podpory](~/cross-platform/troubleshooting/support-options.md), když znovu, problém může být něco mimo podporu Xamarinu týmu můžete *přímo* usnadní.

## <a name="setup-jenkins-with-xamarin"></a>Nastavení Jenkinse s využitím kódu Xamarin

Při výše uvedené problémy Jenkins nejsou podporovány přímo náš tým; [použití Jenkinse s využitím kódu Xamarin](~/tools/ci/jenkins-walkthrough.md) příručka slouží k nastavení serveru Jenkins CI je integrovaná se sadou Xamarin. 

## <a name="fixes-for-common-issues"></a>Řeší běžné problémy

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins nelze nalézt sadu Android SDK

Chybová zpráva pro tento problém je přibližně takto:

> Chyba XA5205: The Android adresář sady SDK se nenašel. Nastavte prosím přes /p:AndroidSdkDirectory

Možnosti pro nastavení umístění sady SDK se mohou lišit v závislosti na přesné Jenkins Android modul plug-in, které používáte; Dobrým vyhledejte nastavení to je v Průvodci modulu plug-in. Například; [modul plug-in Android Emulator](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) automaticky vyhledá sady SDK, ale pokud ho nelze najít; umístění lze také nastavit na stránce konfigurace systému Jenkins pro tento modul plug-in. 


## <a name="deprecated-errors"></a>Nepoužívané chyby

> [!IMPORTANT]
> Tento problém byl vyřešen v nejnovějších verzích Xamarin. Ale pokud k problému dochází na nejnovější verzi softwaru, požádejte prosím [nová chyba](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s plnou verzí informace a plná výstup protokolu sestavení.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins sestavy neplatná licence Xamarinu
Chybové zprávy pro tento problém jsou obvykle něco jako

> Chyba XA9008: sestavování z příkazového řádku musí mít licenci firmy

or

> Chyba: Počáteční verzi Xamarin.iOS nepodporuje sestavování mimo Xamarin Studio 

Nejběžnější příčina tohoto scénáře je použití Jenkinse po přihlášení pomocí uživatelského účtu není přidružené k vaší licence Xamarinu. Nejjednodušším způsobem řešení, je instalace Jenkinse aplikace přímo prostřednictvím uživatelského účtu. Tento proces a některé další aspekty jsou popsány zde: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
