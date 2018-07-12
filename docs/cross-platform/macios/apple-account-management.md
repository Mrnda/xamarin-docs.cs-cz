---
title: Správa účtů Apple
description: Tento dokument popisuje, jak používat funkce správy účtu Apple v sadě Visual Studio pro Mac a Visual Studio 2017.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 4557d3b055e5c49842b9fdcff1dac9ee996e8bab
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986015"
---
# <a name="apple-account-management"></a>Správa účtů Apple

Rozhraní pro správu účtu Apple poskytuje způsob, jak zobrazit všechny vývojové týmy, které jsou spojené s Apple ID. Také umožňuje zobrazit další podrobnosti o jednotlivých týmu zobrazením seznamu _podepisování identity_ a _zřizovací profily_ , které jsou na vašem počítači nainstalovaný.

Provádí se ověření Apple ID na příkazovém řádku s [fastlane](https://fastlane.tools/). fastlane musí být nainstalován v počítači, abyste mohli úspěšně ověřit. Další informace o fastlane a jak ji nainstalovat, najdete v [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) vodítka.

Dialogové okno účtu Apple umožňuje postupujte takto:

* **Vytvoření a správa certifikátů**
* **Vytvoření a správa zřizovací profily**

Informace o tom, jak to provést, je popsaný v tomto průvodci.

> [!NOTE]
> Xamarinu pro nástroje pro správu účtu Apple zobrazí pouze informace o placené vývojářských účtů Apple. Zjistěte, jak testovat aplikaci na zařízení bez placený účet pro vývojáře Apple, najdete v tématu [bezplatné zřizování pro aplikace Xamarin.iOS](~/ios/get-started/installation/device-provisioning/free-provisioning.md) průvodce.

Nástroje pro automatické zřizování pro iOS můžete také použít k automatickému vytvoření a správě identit podepisování, ID aplikace a zřizovací profily. Další informace o použití těchto funkcí naleznete [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

## <a name="requirements"></a>Požadavky

Správa účtů Apple je k dispozici v sadě Visual Studio pro Mac a Visual Studio 2017 (verze 15.7 nebo novější)

Musíte mít účet Apple Developer tak, aby tuto funkci používat. Další informace o vývojářských účtů Apple je k dispozici v [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

- Ujistěte se, že jste připojeni k Internetu. Je to proto fastlane komunikuje přímo s portálem pro vývojáře Apple.
- Zkontrolujte, že máte [nainstalované nástroje fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Ujistěte se, máte všechny nejnovější nástroje z fastlane [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Než začnete, ujistěte se, že tak, aby přijímal libovolný uživatel licenčních smluv v [portál pro vývojáře](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Přidání účtu Apple developer

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Chcete-li otevřít dialogové okno pro správu účtu přejděte na **sady Visual Studio > Předvolby > vývojářského účtu Apple**:

    ![Možnosti účtu Apple Developer](apple-account-management-images/image1.png)

2. Stisknutím klávesy **+** tlačítko a zobrazte přihlašovací v dialogovém okně, jak je znázorněno níže: 

    ![Dialogové okno fastlane.](apple-account-management-images/image2.png)

4. Zadejte svoje Apple ID a heslo a klikněte na tlačítko **Sign In** tlačítko. Uloží zabezpečené přístupové údaje na tomto počítači. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) se používá ke zpracování pověření bezpečně a předat je do portálu pro vývojáře Apple.
 
5. Vyberte **vždy povolit** na dialogového okna výstrah, které sadě Visual Studio použijte svoje přihlašovací údaje:

    ![Vždy povolit dialogového okna výstrah](apple-account-management-images/image4.png)

6. Jakmile se váš účet byl úspěšně přidán, zobrazí se vám Apple ID a všechny týmy, které Apple ID je součástí.

    ![Dialogové okno účtu Apple developer s účty přidané](apple-account-management-images/image5.png)

7. Vyberte tým a stisknutím klávesy **zobrazit podrobnosti...** tlačítko. Tím se zobrazí seznam všech podepisování identity a zřizovací profily, které jsou nainstalovány na vašem počítači:

    ![Zobrazit podrobnosti obrazovky zobrazující podpisové identity a zřizovací profily v počítači](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Než začnete přidávat svoje Apple ID na Visual Studio 2017, ujistěte se, že je vývojové prostředí [spárované k hostiteli buildu Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Chcete-li otevřít okno správy účtu, přejděte na **nástroje > Možnosti > Xamarin > účty Apple**:

    ![Obrazovka s možností účty Apple](apple-account-management-images/prov1.png)

1. Vyberte **přidat** tlačítko a zadejte svoje Apple ID a heslo:

    ![Dialogové okno uživatelského jména a hesla](apple-account-management-images/prov1a.png)

1. Jakmile se váš účet byl úspěšně přidán, zobrazí se vám Apple ID a všechny týmy, které Apple ID je součástí.
 
1. Vyberte tým a stisknutím klávesy **zobrazit podrobnosti...** tlačítko. Tím se zobrazí seznam všech podepisování identity a zřizovací profily, které jsou nainstalovány na vašem počítači:

    ![Dialogové okno uživatelského jména a hesla](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>Správa identit pro podepisování a zřizovací profily

Dialogové okno Podrobnosti o týmu zobrazí seznam identit podepisování uspořádaný podle typu. **Stav** sloupec výzva, jestli je certifikát: 

* **Platný** – na vašem počítači je nainstalovaná podpisovou identitu (certifikát a privátní klíč) a nevypršela jeho platnost.

* **Není v klíčence** – platná identita podepisování je na serveru společnosti Apple. Jak nainstalovat na svém počítači, musí být exportován z jiného počítače. Podpisová identita nemůže stáhnout z portálu pro vývojáře Apple ho nebude obsahovat privátní klíč.

* **Chybí privátní klíč** – certifikát bez soukromého klíče je nainstalován v řetězci klíčů.

* **Vypršela platnost** – platnost certifikátu vypršela. To byste měli odebrat z klíčenky.

  ![informace o dialogovém okně Podrobnosti o týmu](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Vytvoření podepsání identit

Chcete-li vytvořit novou identitu podepisování, vyberte **vytvořit certifikát** tlačítko rozevíracího seznamu a vyberte typ, který potřebujete. Pokud máte správná oprávnění nový podpisový identity se zobrazí po pár sekundách.

Pokud možnost v rozevíracím seznamu se šedě a zrušení výběru, znamená to, že nemáte oprávnění správnému týmu k vytvoření tohoto typu certifikátu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![vytvořit certifikát možnosti](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![vytvořit certifikát možnosti](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Stáhnout zřizovací profily

Dialogové okno Podrobnosti o týmu také zobrazuje seznam všech zřizovací profily, které jsou připojené ke svému vývojářskému účtu. Stisknutím klávesy si můžete stáhnout všechny zřizovacích profilů do místního počítače **stáhnout všechny profily** tlačítko

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Stáhněte si části zřizovací profily](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Stáhněte si části zřizovací profily](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>Podepsání sady prostředků aplikace pro iOS

Informace o nasazení aplikace na zařízení, najdete [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

## <a name="troubleshooting"></a>Poradce při potížích

### <a name="view-details-dialog-is-empty"></a>Podrobnosti zobrazení dialogového okna je prázdný

Toto je momentálně o známý problém týkající se chyb [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Ujistěte se, že používáte nejnovější stabilní verze sady Visual Studio pro Mac

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Pokud dochází k problémům protokolování ve vašem účtu, postupujte následujícím způsobem:

* Otevřete aplikaci řetězce klíčů a v části Kategorie vyberte *hesla*. Vyhledejte `deliver.`a odstranit všechny položky.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Chyba přidání účtu. Přihlaste se prosím pomocí hesla specifické pro aplikace"

Je to proto, že je na vašem účtu povoleno 2 dvoufaktorové ověřování. Ujistěte se, že používáte nejnovější stabilní verze sady Visual Studio pro Mac

### <a name="failed-to-create-new-certificate"></a>Nepovedlo se vytvořit nový certifikát
"Jste dosáhli limitu pro certifikáty tohoto typu"

![Dialogové okno certifikát limit](apple-account-management-images/image10.png)

Maximální počet povolený certifikáty byly vytvořeny. To pokud chcete napravit, přejděte [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) dokumentů a odvolání přístupu jeden z certifikátů produkčního prostředí.

## <a name="known-issues"></a>Známé problémy

* Distribuční zřizovací profily jsou standardně zaměřené na App Store. Místní nebo jednorázové profily se musí vytvořit ručně.
