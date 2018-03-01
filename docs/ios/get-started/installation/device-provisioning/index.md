---
title: "Zřizování zařízení"
description: "Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka se prozkoumejte vyžádání vývoj certifikátů a profilů, práce s aplikační služby a nasazení aplikace na zařízení."
ms.topic: article
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/15/2017
ms.openlocfilehash: 1a5120f088fca6997eef3a1bc27126b442380cac
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="device-provisioning"></a>Zřizování zařízení

_Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka se prozkoumejte vyžádání vývoj certifikátů a profilů, práce s aplikační služby a nasazení aplikace na zařízení._

Při vývoji aplikace pro Xamarin.iOS je nezbytné pro testování pomocí nasazeného aplikaci fyzické zařízení, kromě v simulátoru. Pouze zařízení chyb a problémů s výkonem můžete transpire při spuštění v zařízení, z důvodu omezení hardwaru, jako je například paměť nebo připojením k síti. K testování na fyzické zařízení, musí být zařízení *zřízený*, a Apple musí být informováni, že zařízení se použije pro testování.

Zvýrazněná části na obrázku níže zobrazit kroky potřebné k jejich nastavení pro zřizování iOS:

[![](images/provisioningdiagram.png "Zvýrazněná části na tomto obrázku zobrazit kroky potřebné k jejich nastavení pro zřizování iOS")](images/provisioningdiagram.png)

Po této dalším krokem je distribuce aplikace. Další informace o nasazení naleznete [distribuce aplikací](~/ios/deploy-test/app-distribution/index.md) příručky.

Před nasazením aplikace na zařízení, musíte mít aktivní předplatné do programu pro vývojáře společnosti Apple, *nebo* použít [volné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md). Apple nabízí dvě možnosti program:

- **Programu pro vývojáře Apple** – bez ohledu na to, jestli jsou jednotlivec nebo představují organizaci programu pro vývojáře Apple umožňuje vyvíjet, testování a distribuci aplikací.
- **Apple Developer Enterprise Program** – je nejvhodnější pro organizace, které mají vyvinout a distribuovat aplikace, interní pouze Enterprise program. Členové programu Enterprise nemají přístup k iTunes připojit a vytvořit aplikace nemůže být publikována do obchodu s aplikacemi.


Lze zaregistrovat pro některý z těchto programů [portál pro vývojáře Apple](https://developer.apple.com/programs/enroll/) k registraci. Všimněte si, že pokud chcete zaregistrovat jako vývojář Apple, je potřeba mít [Apple ID](https://appleid.apple.com/). Tento průvodce byl vytvořen s předpokladem, které **jsou** členem programu pro vývojáře Apple.

Případně, Společnost Apple vydala [volné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md) v Xcode 7, což umožňuje jednu aplikaci spustit na jednom zařízení *bez* je členem programu pro vývojáře Apple. Existuje několik omezení při zřizování tímto způsobem, jak je podrobně uvedeno [zde](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations).

Jakékoli aplikace, která běží na zařízení musí obsahovat sadu metadata (nebo *kryptografický otisk*), který obsahuje informace o aplikaci a vývojář. Apple používá tímto kryptografickým otiskem, abyste měli jistotu, že aplikace není manipulováno při nasazení, nebo běží na, zařízení uživatele. Toho dosáhnete tím, že vývojáři aplikací jako vývojář zaregistrovat svoje Apple ID a nastavit ID aplikace, žádost o certifikát a zaregistrovat zařízení, na kterém bude aplikace nasazena.

Při nasazení aplikace do zařízení, profil zřizování taky nainstalovat do zařízení s iOS. Profil zřizování existuje pro ověření informací o byl podepsaný v čase vytvoření buildu a je kryptograficky podepsaný společností Apple aplikace. Kontroly profil zřizování a 'kryptografický otisk, společně určují, pokud je aplikace můžete nasadit do zařízení kontrolou:

- **Kdo** (certifikáty – byl podepsán aplikace s privátním klíčem, který má odpovídající veřejný klíč v profilu pro zřizování? Certifikát také přidruží vývojář vývojový tým)
- **Co** (ID aplikace pro jednotlivé – podporuje sadu identifikátor svazku ID aplikace v profilu zřizování se shodují Info.plist?)
- **Kde** (zařízení – je zařízení obsažené v profilu pro zřizování?)

Tyto kroky Ujistěte se, že vše, co se vytvoří nebo použít během procesu vývoje, včetně aplikací a zařízení, můžete jej použít k identifikaci účtu vývojáře Apple.

<a name="Provisioning_Profile" />

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="provisioning-your-device"></a>Zřizování zařízení

Existují dva způsoby, jak zřídit zařízení s iOS pomocí sady Visual Studio pro Mac:

* **Automaticky (doporučeno)** – vyberte **automaticky spravovat podepisování** možnost, v souboru Info.plist Visual Studio pro Mac automaticky vytvořit a spravovat identity přihlašování, ID aplikace a zřizování Profily.  Informace o tom, jak automaticky spravovat zřizování najdete v tématu [automatické zřizování](automatic-provisioning.md) průvodce. Toto je doporučený způsob zřizování zařízení s iOS.

* **Ručně** – podepisování identit, ID aplikace a profily zřizování můžete vytvořit a spravovat prostřednictvím portálu pro vývojáře Apple, jak je popsáno v [ručního zřizování](manual-provisioning.md) průvodce. Tyto artefakty potom je můžete spravovat jak je popsáno v [správy účtů Apple](~/cross-platform/macios/apple-account-management.md) průvodce.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="provisioning-your-device"></a>Zřizování zařízení

Pokyny o tom, jak nastavit zařízení se systémem Apple pro nasazení a nasazení aplikace pomocí sady Visual Studio v systému Windows, je doporučeno, postupujte podle podrobných pokynů v [ručního zřizování](manual-provisioning.md) průvodce.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Zřizování pro aplikační služby

Apple poskytuje výběr speciální aplikační služby, označované taky jako funkce, které může být aktivovaný pro aplikace pro Xamarin.iOS. Tyto aplikace služby je nutné nakonfigurovat na obou iOS Provisioning Portal při **ID aplikace** je vytvořena a v **Entitlements.plist** souboru, který je součástí projektu aplikace pro Xamarin.iOS. Informace o přidání aplikačních služeb do vaší aplikace, najdete v části [Úvod k funkcím](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce a [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

* Vytvoření ID aplikace se službami požadovaná aplikace.
* Vytvořte novou [profil pro zřizování](#Provisioning_Profile) obsahující číslem ID této aplikace.
* Nastavit oprávnění v projektu Xamarin.iOS

> [!NOTE]
> V současné době zřizování profily vytvořené v sadě Visual Studio pro Mac nebude do účtu oprávnění vybrané ve vašich projektů (Entitlements.plist). Tato funkce bude přidána v budoucích verzích rozhraní IDE. Pokud budete muset použít aplikační služby, je vhodné postupujte podle pokynů [ručního zřizování](manual-provisioning.md) průvodce.

## <a name="related-links"></a>Související odkazy

- [Volné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Distribuce aplikací](~/ios/deploy-test/app-distribution/index.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
- [Apple – průvodci distribucí aplikace](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
