---
title: Automatické zřizování pro Xamarin.iOS
description: Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka prozkoumá požadavek vývoj certifikátů a profilů pomocí automatické přihlašování.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: 323174b4a37a12828a32acb398fef63cd9b849e3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785814"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Automatické zřizování pro Xamarin.iOS

_Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka prozkoumá požadavek vývoj certifikátů a profilů pomocí automatické přihlašování._

## <a name="requirements"></a>Požadavky

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- Visual Studio pro Mac 7.3 nebo větší
- Xcode 9 nebo vyšší

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 verze 15.7 (nebo novější)

Také musí spárována hostitele sestavení Mac, který má následující:

- Xcode 9 nebo vyšší

-----

## <a name="enabling-automatic-signing"></a>Povolení automatické přihlašování

Před zahájením procesu automatického podepisování, měli ujistit, že máte Apple ID přidat v sadě Visual Studio, jak je popsáno v [správy účtů Apple](~/cross-platform/macios/apple-account-management.md) průvodce. Po přidání Apple ID, můžete použít všechny přidružené _Team_. To umožňuje certifikátů, profily a další ID má být provedeno s týmem. ID se také používá k vytvoření týmu předponu pro ID aplikace, které budou zahrnuty v profilu zřizování. To umožní Apple, kdo Řekněme, že jste jsou.

> [!IMPORTANT]
> Než začnete, ujistěte se přihlásit k buď [iTunes připojit](https://itunesconnect.apple.com/) nebo [appleid.apple.com](https://appleid.apple.com) zkontrolujte, že jste přijali nejnovější zásady účtu Apple. Pokud se zobrazí výzva, proveďte kroky přijímat žádné nové smlouvy účet od společnosti Apple. Pokud nemáte přijmout smlouvy o ochraně osobních údajů z může 2018, získáte následující výstrahu při pokusu o zřízení zařízení:
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

Automaticky podepsání vaší aplikace pro nasazení na zařízení s iOS, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otevřete projekt pro iOS v sadě Visual Studio for Mac.

2. Otevřete **Info.plist** souboru.

3. V **podpisování** část, vyberte **automatické zřizování**:

    ![Rozevírací seznam pro výběr Team](automatic-provisioning-images/image2.png)

4. Vyberte váš tým z **Team** rozevíracího seznamu.

6. Za několik sekund se vytvoří podpisový certifikát a zřizování profilu:

    ![úspěšně vytvořit certifikát a profil](automatic-provisioning-images/image5.png)

    Pokud automatické podepisování selže **automatické podpisový pad** zobrazí z důvodu chyby.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spárujte Visual Studio 2017 na Mac, jak je popsáno v [pár na Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) průvodce.

2. Otevřete tak, že vyberete možnosti zřizování **Projekt > zřizování vlastnosti...**

3. Vyberte **automaticky zřizování** schéma:

    ![Výběr automatického schématu](automatic-provisioning-images/prov4.png)

4. Vyberte váš tým z **Team** – pole se seznamem zahájíte proces automatického podepisování.

    ![Výběr týmu](automatic-provisioning-images/prov3.png)

4. Spustí se proces automatického podepisování. Visual Studio pokusí ke generování ID aplikace, profil pro zřizování a podpisové identity použít tyto artefakty pro podepisování. Zobrazí se proces generace ve výstupu sestavení:

    ![Výstup zobrazuje generování artefaktů sestavení](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Aktivuje automatické zřizování

Pokud je povolena automatická podepisování, Visual Studio pro Mac zaktualizuje těchto artefaktů v případě potřeby při žádnou z následujících akcí dojít:

* Zařízení s iOS je připojeno do počítače Mac
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
