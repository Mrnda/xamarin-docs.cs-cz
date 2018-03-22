---
title: "Automatické zřizování"
description: "Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka prozkoumá pomocí automatické přihlašování v sadě Visual Studio pro Mac požadavků vývoj certifikátů a profilů."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 11/17/2017
ms.openlocfilehash: 271d9e3f7ae04f03a132ae2fd0ebf531fe52578c
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="automatic-provisioning"></a>Automatické zřizování

_Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka prozkoumá pomocí automatické přihlašování v sadě Visual Studio pro Mac požadavků vývoj certifikátů a profilů._

## <a name="requirements"></a>Požadavky

- Visual Studio pro Mac 7.3 nebo větší
- Xcode 9 nebo vyšší

> [!IMPORTANT]
> Tato příručka ukazuje, jak používat Visual Studio pro Mac k nastavení zařízení se systémem Apple pro nasazení a nasazení aplikace. Pro ruční postup k tomu nebo chcete-li to provést pomocí sady Visual Studio v systému Windows, je doporučeno, postupujte podle podrobných pokynů v [ručního zřizování](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) průvodce.

## <a name="enabling-automatic-signing"></a>Povolení automatické přihlašování

Před zahájením procesu automatického podepisování, měli ujistit, že máte Apple ID přidat v sadě Visual Studio pro Mac, jak je popsáno v [správy účtů Apple](~/cross-platform/macios/apple-account-management.md) průvodce. Po přidání Apple ID, můžete použít všechny přidružené _Team_. To umožňuje certifikátů, profily a další ID má být provedeno s týmem. ID se také používá k vytvoření týmu předponu pro ID aplikace, které budou zahrnuty v profilu zřizování. To umožní Apple, kdo Řekněme, že jste jsou.

Automaticky podepsání vaší aplikace pro nasazení na zařízení s iOS, postupujte takto:

1. Otevřete projekt pro iOS v sadě Visual Studio for Mac.

2. Otevřete **Info.plist** souboru.

3. V **podpisování** část, vyberte **automatické zřizování**:

    ![Rozevírací seznam pro výběr Team](automatic-provisioning-images/image2.png)

4. Vyberte váš tým z **Team** rozevíracího seznamu.

6. Za několik sekund se vytvoří podpisový certifikát a zřizování profilu:

    ![úspěšně vytvořit certifikát a profil](automatic-provisioning-images/image5.png)

    Pokud automatické podepisování selže **automatické podpisový pad** zobrazí z důvodu chyby.

## <a name="triggering-automatic-provisioning"></a>Aktivuje automatické zřizování

Pokud je povolena automatická podepisování, Visual Studio pro Mac zaktualizuje těchto artefaktů v případě potřeby při žádnou z následujících akcí dojít:

* Zařízení s iOS je připojeno do počítače mac
    - Tato kontrola ověřuje automaticky zobrazíte, když je zařízení zaregistrované na portál pro vývojáře Apple. Pokud tomu tak není, bude ho přidat a vygenerovat nový profil pro zřizování, který jej obsahuje.
* Je-li změnit ID sady prostředků aplikace
    - Tím se aktualizuje ID aplikace. Nový profil pro zřizování obsahující ID této aplikace se vytvoří.
* Podporované funkce je v souboru Entitlements.plist povoleno.
    - Tato funkce se zařadí do ID aplikace a nový profil pro zřizování s aktualizovaná aplikace, které se generuje ID.
    - Ne všechny funkce jsou aktuálně podporovány. Další informace o ty, které jsou podporovány, podívejte se [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce.


## <a name="related-links"></a>Související odkazy

- [Bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Distribuce aplikace](~/ios/deploy-test/app-distribution/index.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
- [Apple – průvodci distribucí aplikace](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
