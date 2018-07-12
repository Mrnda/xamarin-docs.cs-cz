---
title: Automatické zřizování pro Xamarin.iOS
description: Po úspěšné instalaci Xamarin.iOS na další krok ve vývoji pro iOS je ke zřízení zařízení s Iosem. Tato příručka se věnuje použití automatické podepisování pro žádost o vývoj certifikátů a profilů.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: a0c3179dc8e349c23d5521230e0957d1be9384ec
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986184"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Automatické zřizování pro Xamarin.iOS

_Po úspěšné instalaci Xamarin.iOS na další krok ve vývoji pro iOS je ke zřízení zařízení s Iosem. Tato příručka se věnuje použití automatické podepisování pro žádost o vývoj certifikátů a profilů._

## <a name="requirements"></a>Požadavky

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- Visual Studio pro Mac 7.3 nebo novější
- Xcode 9 nebo vyšší

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 verze 15.7 (nebo novější)

Které musí být párován také k hostiteli buildu Mac, který obsahuje následující:

- Xcode 9 nebo vyšší

-----

## <a name="enabling-automatic-signing"></a>Povolení automatické podepisování

Před zahájením procesu automatické podepisování, měli byste zajistit, že máte Apple ID. přidáno v sadě Visual Studio, jak je popsáno v [Správa účtů Apple](~/cross-platform/macios/apple-account-management.md) průvodce. Po přidání Apple ID, můžete použít všechny přidružené _týmu_. Díky tomu certifikátů, profily a jiné ID má být provedeno proti týmu. ID se také používá k vytvoření týmu předponu pro ID aplikace, které budou zahrnuty v profilu zřizování. To umožňuje společnosti Apple k ověření, že jste koho se, že máte.

> [!IMPORTANT]
> Než začnete, ujistěte se, že pro přihlášení k buď [iTunes Connect](https://itunesconnect.apple.com/) nebo [appleid.apple.com](https://appleid.apple.com) zkontrolujte, že jste přijali nejnovější zásady poskytování účet Apple. Pokud se zobrazí výzva, proveďte kroky tak, aby přijímal nové smlouvy účtu od společnosti Apple. Pokud nepřijmete smlouvy o ochraně osobních údajů z května 2018, získáte následující výstraha při pokusu o zřizování vašeho zařízení:
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

Pro automatické podepsání aplikace pro nasazení na zařízení s iOS, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otevřete projekt pro iOS v sadě Visual Studio pro Mac.

2. Otevřít **Info.plist** souboru.

3. V **podepisování** části, vyberte **automatické zřizování**:

    ![Rozevírací selektor týmu](automatic-provisioning-images/image2.png)

4. Vyberte před poškození týmem **týmu** rozevíracího seznamu.

6. Po několika sekundách se vytvoří podpisový certifikát a zřizování profilu:

    ![certifikát a profil se úspěšně vytvořil](automatic-provisioning-images/image5.png)

    Pokud se nezdaří automatické podepisování **automatické podepisování panel** zobrazí z důvodu chyby.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spárujte Visual Studio 2017 pro Mac, jak je popsáno v [spárovat s počítačem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) průvodce.

2. V **Průzkumníka řešení**, klikněte pravým tlačítkem na název projektu a vyberte **vlastnosti**. Potom přejděte **podepsání sady prostředků aplikace pro iOS** kartu.

3. Vyberte **automatické zřizování** schéma:

    ![Výběr automatického schéma](automatic-provisioning-images/prov4.png)

4. Vyberte před poškození týmem **týmu** – pole se seznamem zahájíte proces automatické podepisování.

    ![Výběr týmu](automatic-provisioning-images/prov3.png)

4. Spustí se proces automatické podepisování. Visual Studio se pak pokusí vygenerovat ID aplikace, zřizovací profil a podpisovou identitu použít tyto artefakty pro podepisování. Zobrazí se proces vytváření ve výstupu sestavení:

    ![Generování výstupu se zobrazuje artefaktů sestavení](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Aktivace automatické zřizování

Pokud automatické podepisování se povolila, Visual Studio pro Mac bude aktualizovat tyto artefakty v případě potřeby při některý z následujících akcí:

* Zařízení s Iosem se zapojí se do počítače Mac
    - Tato kontrola ověřuje automaticky zobrazíte, pokud je zařízení zaregistrované na portálu pro vývojáře Apple. Pokud tomu tak není, bude ho přidejte a vygenerujte nový zřizovací profil, který jej obsahuje.
* ID sady prostředků aplikace se změnilo.
    - Tím se aktualizuje ID aplikace. Vytvoření nového zřizovacího profilu obsahující ID této aplikace.
* Podporované funkce je povolená v souboru do souboru Entitlements.plist.
    - Tato funkce bude přidána do ID aplikace a nový zřizovací profil s aktualizovanou aplikací, které se vygeneruje ID.
    - Ne všechny funkce jsou aktuálně podporovány. Další informace o těch, které jsou podporovány, podívejte se [práce s funkcemi](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce.


## <a name="related-links"></a>Související odkazy

- [Bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Distribuce aplikace](~/ios/deploy-test/app-distribution/index.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
- [Apple – průvodci distribucí aplikace](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
