---
title: "Proč není volaných Xamarin podporuje?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 8129229a821edd2ef4f251679ee46bca7b74c8f9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Proč není volaných Xamarin podporuje?

## <a name="jenkins-support-explanation"></a>Vysvětlení volaných podpory

Volaných je sada CI open-source; z důvodu tohoto počtu problémy, které jsou přímo způsobeny volaných *samotné* bude muset být vytvářené jako problémy, na které jste získali kód; například [hlavní úložišti volaných](https://github.com/jenkinsci/jenkins), nebo úložišti pro [ Jenkins.App](https://github.com/stisti/jenkins-app).

Výjimkou je na problémy, které může být izolované na konkrétní chyby v nástrojích pro Xamarin; Pokud se domníváte, že to tak můžete zkontrolovat vaše [podporují možnosti](~/cross-platform/troubleshooting/support-options.md), když znovu, tento problém může být něco mimo podporu Xamarin tým může *přímo* usnadní.

## <a name="setup-jenkins-with-xamarin"></a>Instalační program volaných xamarinu

Při výše uvedených hodnot volaných problémy nepodporují přímo náš tým; [volaných pomocí xamarinu](~/tools/ci/jenkins-walkthrough.md) příručka slouží k nastavení volaných CI serveru, který je integrován s funkcí Xamarin. 

## <a name="fixes-for-common-issues"></a>Opravy pro běžné problémy
### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Volaných se nepodařilo najít SDK pro Android

Chybová zpráva pro tento problém je přibližně takto:

> Chyba XA5205: nebyl nalezen Android adresáře sady SDK. Nastavte prosím prostřednictvím /p:AndroidSdkDirectory

Možnosti nastavení umístění SDK může lišit v závislosti na přesné modul plug-in Android volaných, kterou používáte; Dobrým místem, kde hledat toto nastavení je v Průvodci modulu plug-in. Například; [modul plug-in Android emulátoru](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) automaticky vyhledá sadu SDK, ale pokud ji nemůže najít; umístění lze také nastavit prostřednictvím volaných systému konfigurační stránku pro tento modul plug-in. 


## <a name="deprecated-errors"></a>Nepoužívané chyby

> [!IMPORTANT]
> Tento problém byl vyřešen v posledních verzích Xamarin. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Volaných sestavy neplatná licence Xamarin
Chybové zprávy pro tento problém se obvykle něco podobného jako

> Chyba XA9008: sestavení z příkazového řádku je nutná licence firmy

or

> Chyba: Starter Edition Xamarin.iOS nepodporuje vytváření mimo Xamarin Studio 

Nejběžnější příčina tohoto scénáře je použití volaných přihlášením pomocí uživatelského účtu nejsou přidružené licence na aplikaci Xamarin. Nejjednodušší způsob, jak řešení, je instalace volaných jako aplikace přímo přes uživatelský účet. Že proces a některé další aspekty jsou popsány zde: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)

Další možností je, že vaše informace o licencích Xamarin je nějakým způsobem poškozený, můžete použít [nové synchronizace licencí Xamarin průvodce](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) tento scénář lze vyřešit.


