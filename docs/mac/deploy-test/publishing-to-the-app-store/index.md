---
title: "Publikování do obchodu s aplikacemi"
description: "Tento průvodce vás provede nasazení aplikace na Xamarin.Mac pomocí sady Visual Studio for Mac. Vysvětluje, jak nastavit účet pro vývojáře Mac, vás provede procesem vytvoření certifikáty pro podepisování kódu a ukazuje, jak je používat k vytvoření aplikace pro Mac, které mohou být distribuovány přímo nebo prostřednictvím Mac App Storu."
ms.topic: article
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0846dc8bdb3ac722838faa4173f5d30233912ecc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-to-the-app-store"></a>Publikování do obchodu s aplikacemi

_Tento průvodce vás provede nasazení aplikace na Xamarin.Mac pomocí sady Visual Studio for Mac. Vysvětluje, jak nastavit účet pro vývojáře Mac, vás provede procesem vytvoření certifikáty pro podepisování kódu a ukazuje, jak je používat k vytvoření aplikace pro Mac, které mohou být distribuovány přímo nebo prostřednictvím Mac App Storu._

## <a name="overview"></a>Přehled

Xamarin.Mac aplikace lze distribuovat dvěma různými způsoby:

- **ID vývojáře** – aplikace podepsané vývojáře ID může být distribuované mimo obchodu s aplikacemi, ale jsou rozpoznáno těchto pravidel a dovoleno instalovat.
- **Mac App Storu** – musí mít instalační balíček aplikace a aplikace a instalační program musí být podepsané, pro odeslání Mac App Storu.

Tento dokument vysvětluje, jak používat Visual Studio pro Mac a Xcode k nastavení účtu pro vývojáře Apple a konfigurace projektu Xamarin.Mac pro každý typ nasazení.


## <a name="mac-developer-program"></a>Programu pro vývojáře Mac

Po připojení [programu pro vývojáře Mac](https://developer.apple.com/devcenter/mac/) vývojář budou se nabízet zvolit možnost připojení jako jednotlivec nebo společnosti, jak ukazuje následující snímek obrazovky:

[![Portál pro vývojáře Apple](images/image1.png "portál pro vývojáře Apple")](images/image1-large.png)

Vyberte typ správné registrace pro vaši situaci.

> [!NOTE]
> **Poznámka:**: volby provedené v tomto poli ovlivní způsob, jak zobrazit některé obrazovky při konfiguraci vývojářského účtu. Popisy a snímky obrazovky v tomto dokumentu se provádějí z perspektivy **jednotlivých** vývojářský účet. V **společnosti**, jenom některé možnosti, bude k dispozici pro **Team správce** uživatele.


### <a name="certificates-and-identifiersmacdeploy-testpublishing-to-the-app-storecertificates-identifiersmd"></a>[Certifikáty a identifikátory](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

Tento průvodce vás provede vytvořením potřebné certifikáty a identifikátory, které bude vyžadovat, aby publikování aplikace Xamarin.Mac.


### <a name="create-provisioning-profilemacdeploy-testpublishing-to-the-app-storeprofilesmd"></a>[Vytvořit profil pro zřizování](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

Tento průvodce vás provede vytvořením nezbytné profily zřizování, který bude vyžadovat, aby publikování Xamarin.Mac aplikace.


### <a name="mac-app-configurationmacdeploy-testpublishing-to-the-app-storeapp-configurationmd"></a>[Konfigurace aplikace pro Mac](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

Tento průvodce vás provede konfiguraci Xamarin.Mac aplikace pro publikace.


### <a name="sign-with-developer-idmacdeploy-testpublishing-to-the-app-storesigningmd"></a>[Přihlášení pomocí ID vývojáře](~/mac/deploy-test/publishing-to-the-app-store/signing.md)

Tento průvodce vás provede podepisování aplikace Xamarin.Mac s ID vývojáře pro publikaci.


### <a name="bundle-for-mac-app-storemacdeploy-testpublishing-to-the-app-storebundlingmd"></a>[Balíček pro Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/bundling.md)

Tento průvodce vás provede sdružování Xamarin.Mac aplikace pro publikaci Mac App Storu.


### <a name="upload-to-mac-app-storemacdeploy-testpublishing-to-the-app-storeuploadingmd"></a>[Nahrát do obchodu Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/uploading.md)

Tento průvodce vás provede Xamarin.Mac aplikace pro publikaci se nahrávají na Mac App Storu.


## <a name="related-links"></a>Související odkazy

- [Instalace](/visualstudio/mac/installation/)
- [Hello, ukázka Mac](~/mac/get-started/hello-mac.md)
- [ID vývojáře a těchto pravidel](https://developer.apple.com/resources/developer-id/)
