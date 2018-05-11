---
title: Správa účtů Apple
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 2a37f6644c66ebeb3b10a9fa0467115a21f69e75
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="apple-account-management"></a>Správa účtů Apple

Rozhraní pro správu účtu Apple poskytuje způsob, jak zobrazit všechny vývojové týmy, které jsou přidružené k zadání Apple ID. Také umožňuje zobrazit další podrobnosti o každém team se seznamem _podepisování identity_ a _profily zřizování_ které se instalují na váš počítač.

Ověřování Apple ID se provádí na příkazovém řádku s [fastlane](https://fastlane.tools/). fastlane musí být nainstalován na počítači, abyste mohli úspěšně ověřit. Další informace o fastlane a k jeho instalaci je podrobně popsaná v [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) příručky.

Dialogové okno účet Apple umožňuje postupujte takto:

* **Vytváření a správě certifikátů** 
* **Vytvářet a spravovat profily zřizování** 

Informace o tom, jak to udělat najdete v této příručce.

Můžete taky automatické zřizování nástroje pro iOS automaticky vytvořit a spravovat identity přihlašování, ID aplikace a profily zřizování.

Další informace o používání těchto funkcí najdete v části [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) průvodce.
️
## <a name="requirements"></a>Požadavky

Správa Apple účtu je k dispozici v sadě Visual Studio pro Mac a Visual Studio 2017 (verze 15.7 a vyšší)

Musí mít účet Apple Developer, který chcete tuto funkci používat. Další informace o Apple developer účty je k dispozici v [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

- Zkontrolujte, zda že jste připojeni k Internetu. To je proto fastlane komunikuje přímo s portál pro vývojáře Apple.
- Zajistěte, abyste měli [nainstalované nástroje fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Zajistěte, abyste měli nejnovější nástroje fastlane z [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Než začnete, ujistěte se, přijměte licenční smlouvy všechny uživatele v [portál pro vývojáře](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Přidání účtu vývojáře Apple

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Chcete-li otevřít dialogové okno správy účtu přejděte na **Visual Studio > Předvolby > účet Apple Developer**:

    ![Možnosti účtu vývojáře Apple](apple-account-management-images/image1.png)

2. Stiskněte **+** tlačítko zobrazíte přihlášení v dialogovém okně, jak je znázorněno níže: 

    ![Dialogové okno fastlane.](apple-account-management-images/image2.png)

4. Zadejte svoje Apple ID a heslo a klikněte na **přihlásit** tlačítko. Uloží zabezpečeného řetězce klíčů přihlašovacích údajů na tomto počítači. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) slouží ke zpracování přihlašovacích údajů, bezpečně a předat je na portál pro vývojáře Apple.
 
5. Vyberte **vždy povolit** na dialogového okna výstrah umožňující sady Visual Studio použijte svoje přihlašovací údaje:

    ![Vždy povolit dialogového okna výstrah](apple-account-management-images/image4.png)

6. Jakmile je váš účet byl úspěšně přidán, uvidíte vaše Apple ID a všechny týmy, které vaše Apple ID je součástí.

    ![Dialogové okno účtu vývojáře Apple s účty přidat](apple-account-management-images/image5.png)

7. Vyberte všechny týmu a stiskněte klávesu **zobrazit podrobnosti...** tlačítko. To se zobrazí seznam všech podepisování identit a profilů zřizování, zda jsou nainstalovány v počítači:

    ![Zobrazení podrobností obrazovky znázorňující podepisování identit a profily na počítači pro zřizování](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Než začnete přidávat Apple ID do Visual Studio 2017, ujistěte se, že vývojového prostředí je [spárováno s hostiteli sestavení Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Chcete-li otevřít okno správy účtu, přejděte na **nástroje > Možnosti > Xamarin > účty Apple**:

    ![Účty Apple možnosti obrazovky](apple-account-management-images/prov1.png)

1. Vyberte **přidat** tlačítko a zadejte svoje Apple ID a heslo:

    ![Dialogové okno uživatelského jména a hesla](apple-account-management-images/prov1a.png)

1. Jakmile je váš účet byl úspěšně přidán, uvidíte vaše Apple ID a všechny týmy, které vaše Apple ID je součástí.
 
1. Vyberte všechny týmu a stiskněte klávesu **zobrazit podrobnosti...** tlačítko. To se zobrazí seznam všech podepisování identit a profilů zřizování, zda jsou nainstalovány v počítači:

    ![Dialogové okno uživatelského jména a hesla](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>Správa identit pro podepisování a profily zřizování

Dialogové okno Podrobnosti o team zobrazí seznam podepisování identit uspořádané podle typu. **Stav** sloupec objeví upozornění, pokud je certifikát: 

* **Platný** – podpisové identity (certifikát a privátní klíč) je nainstalovaný na počítači a nevypršela platnost.

* **Není v řetězci klíčů** – je platný podpisové identity na serveru společnosti Apple. K instalaci tohoto na počítači, musí být exportován z jiného počítače. Podpisové identity nelze stáhnout z portálu pro vývojáře Apple, jak nebude obsahovat privátní klíč.

* **Chybí privátní klíč** – certifikát bez soukromého klíče je nainstalován v řetězci klíčů.

* **Platnost** – vypršela platnost certifikátu. To byste měli odebrat z vaší řetězce klíčů.

  ![informace o dialogovém okně Podrobnosti o Team](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Vytvoření podepsání identit

Chcete-li vytvořit nové podpisové identity, vyberte **vytvořit certifikát** tlačítko rozevíracího seznamu a vyberte typ, který požadujete. Pokud máte správná oprávnění nové podepsání identity se zobrazí za několik sekund.

Pokud možnost v rozevíracím seznamu šedá a zrušit, znamená to, že nemáte oprávnění správný tým pro vytvoření tohoto typu certifikátu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Vytvoření certifikátu možnosti](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Vytvoření certifikátu možnosti](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Stáhnout zřizovacích profilů

Dialogové okno Podrobnosti o team taky zobrazí seznam všech profilů zřizování připojený ke svému účtu vývojáře. Stisknutím kombinace kláves si můžete stáhnout všechny zřizovacích profilů do místního počítače **stáhnout všechny profily** tlačítko

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Stažením profily zřizování](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Stažením profily zřizování](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS podepisování sady

Informace o nasazení aplikace na zařízení, najdete v části [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

## <a name="troubleshooting"></a>Poradce při potížích

### <a name="view-details-dialog-is-empty"></a>Dialogové okno Podrobnosti zobrazení je prázdný

Toto je momentálně o známý problém týkající se chyby [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Ujistěte se, že používáte nejnovější stabilní verze sady Visual Studio pro Mac

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Pokud dochází k problémům protokolování ve vašem účtu, postupujte následujícím způsobem:

* Otevřete aplikaci řetězce klíčů a v části Kategorie vyberte *hesla*. Vyhledejte `deliver.`a odstranit všechny položky.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Chyba při přidávání účtu. Přihlaste se heslo specifické pro aplikace"

Je to proto, že povolíte 2 Multi-Factor authentication na vašem účtu. Ujistěte se, že používáte nejnovější stabilní verze sady Visual Studio pro Mac

### <a name="failed-to-create-new-certificate"></a>Nepodařilo se vytvořit nový certifikát
"Jste dosáhli limitu pro certifikáty tohoto typu"

![Dialogové okno omezení certifikátu](apple-account-management-images/image10.png)

Maximální počet certifikátů povolené byly vytvořeny. Chcete-li odstranit tento problém, procházejte k [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) a jedním z provozní certifikáty odvolat.

## <a name="known-issues"></a>Známé problémy

* Distribuční zřizovací profily jsou standardně zaměřené na App Store. Místní nebo jednorázové profily se musí vytvořit ručně.
