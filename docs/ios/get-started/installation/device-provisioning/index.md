---
title: Device Provisioning Service pro Xamarin.iOS
description: Tento dokument popisuje, jak zřídit zařízení tak, aby ho můžete použít k testování aplikace. Také popisuje, jak nakonfigurovat aplikaci tak, aby ho můžete použít možnosti, jako jsou nabízená oznámení.
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: f0d6d2343350455a101033aced7cec0c31695503
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353227"
---
# <a name="device-provisioning-for-xamarinios"></a>Device provisioning Service pro Xamarin.iOS

Při vývoji aplikace pro Xamarin.iOS je nutné ji otestovat nasazení aplikace do fyzického zařízení, dále na simulátoru. Při spuštění na zařízení, z důvodu omezení hardwaru, jako je například paměť nebo připojením k síti, můžete probíhají jen pro zařízení chyby a problémy s výkonem. K testování na fyzickém zařízení, musí být zařízení *zřízené*, a Apple, musí být informováni, že zařízení se použije pro účely testování.

Zvýrazněné sekce na následujícím obrázku ukazují kroky potřebné k jejich nastavení pro zřizování iOS:

[![](images/provisioningdiagram.png "Zvýrazněné sekce na tomto obrázku zobrazit kroky potřebné k jejich nastavení pro zřizování iOS")](images/provisioningdiagram.png#lightbox)

Po této dalším krokem je distribuci aplikací. Další informace o nasazení, najdete [distribuci aplikací](~/ios/deploy-test/app-distribution/index.md) vodítka.

Před nasazením aplikace do zařízení, musíte mít aktivní předplatné do společnosti Apple Developer Program *nebo* použít [bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md). Apple nabízí dvě možnosti programu:

- **Apple Developer Program** – bez ohledu na tom, jestli jste jednotlivec nebo představují organizaci Apple Developer Program umožňuje vývoj, testování a distribuce aplikací.
- **Apple Developer Enterprise Program** – podniku programu je nejvhodnější pro organizace, které chtějí vyvinout a distribuovat pouze interní aplikace. Členové programu Enterprise nebudou mít přístup do služby iTunes Connect a aplikací vytvořených nemůže být publikována do App Store.

Zaregistrujte se na některý z těchto programů, najdete v tématu [portálu Apple Developer](https://developer.apple.com/programs/enroll/) k registraci. Všimněte si, že pokud chcete zaregistrovat jako vývojář Apple, je potřeba mít [Apple ID, které](https://appleid.apple.com/). Tato příručka se vytvořil za předpokladu, kterou jste **jsou** členem programu Apple Developer Program.

Alternativně Apple představila [bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md) v Xcode 7, což umožní jedné aplikace spustit na jednom zařízení *bez* se členy programu pro vývojáře společnosti Apple. Existuje několik omezení při zřizování tímto způsobem, jak je uvedeno [tady](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations).

Každou aplikaci, která běží na zařízení musí obsahovat sadu metadata (nebo *kryptografický otisk*), který obsahuje informace o aplikaci a vývojáře. Apple používá tímto kryptografickým otiskem, abyste měli jistotu, že aplikace bez dozoru při nasazování do služby, nebo běžící na zařízení uživatele. Toho dosáhnete pomocí vývojáři aplikace k registraci jejich Apple ID, které jako vývojář a nastavit ID aplikace, které vyžadují požádat o certifikát a zaregistrovat zařízení, na kterém bude aplikace nasazena.

Při nasazení aplikace do zařízení, zřizovací profil se nainstaluje také na zařízení s Iosem. Zřizovací profil existuje potvrzení informací, že aplikace byla podepsána pomocí v okamžiku sestavení a je kryptograficky podepsaný společností Apple. Kontroly zřizovací profil a "kryptografický otisk" společně určují, pokud je aplikace nasadit do zařízení tak, že zkontrolujete:

- **Kdo** (certifikáty – byl podepsán aplikace s privátním klíčem, který má odpovídající veřejný klíč ve zřizovacím profilu? Tento certifikát také přidruží vývojář vývojový tým)
- **Co** (jednotlivých ID aplikace – nepodporuje identifikátor sady prostředků nastavení v souboru Info.plist shoda ve zřizovacím profilu zadejte ID aplikace)?
- **Kde** (zařízení – je zařízení součástí zřizovacího profilu?)

Tyto kroky Ujistěte se, že vše, co je vytvořená nebo používaná během procesu vývoje, včetně aplikací a zařízení, můžete zpětně vysledovat účet pro vývojáře Apple.

## <a name="provisioning-your-device"></a>Zřizování zařízení

Existují dva způsoby, jak zřídit zařízení s Iosem:

* **Automaticky (doporučeno)** – vyberte, **automatické zřizování** schéma ve vašem projektu chcete, aby Visual Studio automaticky vytvářet a spravovat identity podepisování, ID aplikace a zřizovací profily. Informace o tom, jak automaticky spravovat zřizování najdete v tématu [automatické zřizování](automatic-provisioning.md) průvodce. Toto je doporučený postup zřizování zařízení s Iosem.

* **Ručně** – podpisové identity, ID aplikace a zřizovací profily můžete vytvářet a spravovat prostřednictvím portálu pro vývojáře Apple, jak je popsáno v [ruční zřizování](manual-provisioning.md) průvodce. Tyto artefakty pak jde spravovat jak je popsáno v [Správa účtů Apple](~/cross-platform/macios/apple-account-management.md) průvodce.

## <a name="provisioning-for-application-services"></a>Zřizování pro aplikační služby

Apple nabízí celou řadu zvláštní aplikační služby, také nazývané možnosti, které můžete aktivovat pro aplikace pro Xamarin.iOS. Tyto aplikace služby musí být nakonfigurovaná na obou iOS Provisioning Portal při **ID aplikace** se vytvoří a **do souboru Entitlements.plist** souboru, který je součástí projektu aplikace pro Xamarin.iOS. Informace o přidání aplikačních služeb do vaší aplikace [Úvod k možnostem](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce a [práce s nároky](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

* Vytvoření ID aplikace se službami požadovanou aplikaci.
* Vytvořte nový [zřizovací profil](#provisioning-your-device) , který obsahuje ID této aplikace.
* Nastavit oprávnění v projektu Xamarin.iOS

## <a name="related-links"></a>Související odkazy

- [Bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Distribuce aplikace](~/ios/deploy-test/app-distribution/index.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
- [Apple – průvodci distribucí aplikace](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
