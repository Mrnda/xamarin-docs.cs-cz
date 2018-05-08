---
title: Práce s možností
description: Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje potřebné pro všechny možnosti instalace.
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: e6fc3d38fef7c7c3204d1413911ddfa9a486c67c
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="working-with-capabilities"></a>Práce s možností

_Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje potřebné pro všechny možnosti instalace._

Apple poskytuje vývojářům _možnosti_, často označovaný jako _aplikační služby_, jak je způsob rozšířit funkce a rozšíření oboru jaké aplikace pro iOS můžete provést. Umožňují vývojářům přidejte hlubší integrace funkcí platformy pro aplikace, například: schopnost peněžní transakce inicializována z aplikace, služby další zařízení, jako je Siri a další.
Tyto funkce lze použít s projekty Xamarin.iOS. Úplný seznam služeb je popsán dále:

* Skupiny aplikací
* Přidružené domén
* Ochrana dat
* Herní Centrum
* HealthKit
* HomeKit
* Konfigurace bezdrátového příslušenství
* Icloudu
* Nákupy v aplikaci
* Zvuk mezi aplikacemi
* Platím Apple
* Peněženka
* Nabízená oznámení
* Osobní VPN
* Siri
* Mapy
* Režimy pozadí
* Sdílení řetězce klíčů
* Rozšíření sítě
* Konfigurace hotspotů
* Více cest
* Značky NFC čtení


Možnosti lze povolit buď pomocí sady Visual Studio pro Mac a Visual Studio 2017 nebo ručně v portálu pro vývojáře Apple. Některé funkce, jako je například Peněženka, dotykový identifikátor a na Icloudu vyžadují další konfiguraci ID aplikace.

Tato příručka vysvětluje, jak můžete povolit každou z těchto App Services ve vaší aplikaci v sadě Visual Studio automaticky a ručně prostřednictvím centru pro vývojáře, včetně jakékoliv další nastavení, které mohou být potřebné. 

## <a name="adding-app-services"></a>Přidání aplikační služby

Pokud chcete používat funkce, aplikace musí mít platný profil pro zřizování, který obsahuje ID aplikace se na správné služby povolena. Vytvořením této profil pro zřizování můžete buď provést automaticky v sadě Visual Studio pro Mac a Visual Studio 2017, nebo ručně v Centru pro vývojáře Apple.

Tato část vysvětluje, jak používat ke povolit většinu funkcí sady Visual Studio automatické zřizování nebo středisku pro vývojáře. Existují některé funkce, například Peněženka, Icloudu, dotykový identifikátor a skupin aplikací, které vyžadují další nastavení. Tyto jsou podrobně vysvětleny v sousedících příručky.

> [!IMPORTANT]
> Ne všechny funkce můžete přidat a spravovat pomocí automatické zřizování. Následující seznam obsahuje podporované funkce:
>
>* HealthKit 
>* HomeKit 
>* Osobní VPN 
>* Konfigurace bezdrátového příslušenství 
>* Zvuk mezi aplikacemi 
>* SiriKit 
>* Hotspot 
>* Rozšíření sítě 
>* Značky NFC čtení
>* Více cest 
>
>Nabízená oznámení, herní centrum, nákupy v aplikaci, map, sdílení řetězce klíčů, přidružené domén a funkce ochrany dat nejsou aktuálně podporovány. Pokud chcete přidat tyto možnosti, pomocí ručního zřizování a postupujte podle kroků v [středisku pro vývojáře](#devcenter) části.

## <a name="using-the-ide"></a>Používání prostředí IDE

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Možnosti jsou přidány do **Entitlements.plist** v sadě Visual Studio for Mac. Pro přidání možností, použijte následující postup:

1. Otevřete **Info.plist** souboru aplikace systému iOS a vyberte **automaticky zřizování** schéma a **Team** z pole se seznamem. Postupujte podle kroků v [automatické zřizování](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) průvodce, pokud potřebujete pomoc:

    ![Automaticky spravovat podpisový možnost](images/manage-signing.png)

2. Otevřete **Entitlements.plist** soubor a vyberte funkci, která chcete přidat:

    ![Přidání funkce do souboru entitlements.plist](images/image17.png)

    Výběr funkce provádí dvě věci:
    * Tato funkce přidá do vašeho ID aplikace
    * Přidá dvojici klíč/hodnota oprávnění k souboru Entitlements.plist.

    Visual Studio pro Mac vás upozorní, pokud byly provedeny tyto úlohy zobrazením následující zprávu o úspěchu:

    ![Přidání funkce do souboru entitlements.plist](images/image18.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Možnosti jsou přidány do **Entitlements.plist**. Pro přidání možností v Visual Studio 2017, použijte následující postup:

1. Spárujte Visual Studio 2017 na Mac, jak je popsáno v [pár na Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) průvodce.

2. Otevřete tak, že vyberete možnosti zřizování **Projekt > zřizování vlastnosti...**

3. Vyberte **automaticky zřizování** schéma a **Team** z pole se seznamem. Postupujte podle kroků v [automatické zřizování](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) průvodce, pokud potřebujete pomoc:

    ![Automaticky spravovat podpisový možnost](images/manage-signing-vs.png)

4. Otevřete **Entitlements.plist** soubor a vyberte funkci, která chcete přidat. Uložte soubor.

    Ukládání **Entitlement.plist** provádí dvě věci:

    * Tato funkce přidá do vašeho ID aplikace
    * Přidá dvojici klíč/hodnota oprávnění k souboru Entitlements.plist.

-----


<a name="devcenter" />

## <a name="using-the-developer-center"></a>Pomocí centra pro vývojáře

Pomocí centra pro vývojáře je proces dvě krok, který vyžaduje vytvoření ID aplikace a pak pomocí tohoto ID aplikace vytvořte profil pro zřizování. Tyto kroky jsou podrobně popsány níže.

### <a name="creating-an-app-id-with-an-app-service"></a>Vytvoření ID aplikace službou app service

1.  Vyhledejte [Apple Developer Center](https://developer.apple.com/account) na Mac (sestavení hostitele mac při použití počítače s windows) a přihlaste se.
2.  Vyberte **identifikátory, certifikátů a profilů**:

    ![Středisko pro vývojáře Apple](images/image5.png)

3.  V části **identifikátory**, vyberte **ID aplikace**:

    ![Výběr ID aplikaci ve středisku pro vývojáře](images/image6.png)

4.  Stiskněte **+** tlačítko v pravém horním rohu k vytvoření nového ID aplikace.
5.  Zadejte popis ID aplikace, vyberte explicitní ID aplikace a zadejte ID sady ve formátu `com.domain.appname`. Toto ID sady prostředků shodovat s ID sady prostředků ve vašem projektu:

    ![Přidání podrobností ID aplikace](images/image7.png)

6.  V části **App Services** vybrat služby nebo služby, které jsou nutné ve vaší aplikaci:

    ![Stránka Výběr aplikace služby](images/image8.png)

7.  Stiskněte klávesu **pokračovat**.
8.  Potvrďte ID vaší aplikace. Každá služba bude mít jeden z následujících stavů: **povoleno**, **zakázané**, nebo **Konfigurovatelný**, jak je uvedeno dále. Pokud je **povoleno,** je připravený k použití v profilu zřizování. Pokud je **Konfigurovatelný**, další nastavení je nutné pro tuto funkci. Tyto další kroky jsou podrobně popsány v další v pozdějších částech.

    ![ID potvrzení aplikace](images/image9.png)

9.  Klikněte na tlačítko **zaregistrovat** a potom **provádí**. Nově vytvořené ID aplikace by měla zobrazit v seznamu ID aplikace iOS.


<a name="provisioningprofile" />

### <a name="creating-a-provisioning-profile"></a>Vytvoření profilu pro zřizování

Teď vytvořte profil pro zřizování, který obsahuje číslem ID této aplikace. Postupujte podle následujících kroků:

1.  V Apple Developer Center, přejděte do **profily zřizování > všechny**:

    ![Zřizování profilu](images/image10.png)

2.  Stiskněte **+** tlačítko v pravém horním rohu vytvořit nový profil pro zřizování.
3.  Vyberte typ profilu pro zřizování, který potřebujete a klikněte na tlačítko **pokračovat**:

    ![Vybraný profil zřizování](images/image11.png)

4.  Z rozevíracího seznamu vyberte ID aplikace, který byl vytvořen v předchozích krocích a stiskněte klávesu **pokračovat**:

    ![Výběr ID aplikace](images/image12.png)

5.  Vyberte certifikáty použité k podepsání aplikace a stiskněte klávesu **pokračovat**:

    ![Výběr certifikátu](images/image13.png)

6.  Vyberte zařízení, které mají být zahrnuty do tohoto profilu a stiskněte klávesu **pokračovat**:

    ![Vyberte zařízení, pro profil zřizování](images/image14.png)

7.  Zadejte název profilu tak, aby lze identifikovat a stiskněte klávesu **pokračovat** ke generování profil:

    ![Název profilu pro zřizování](images/image15.png)

8.  Stiskněte **Stáhnout** tlačítko se stáhne a poklikejte na soubor v hledání k instalaci profilu pro zřizování.

9. Pokud používáte Visual Studio Ujistěte se, že **ručního zřizování** je vybraná možnost.

10. V sadě Visual Studio pro Mac / Visual Studio, přejděte na **možnosti projektu > podepisování sady** a nastavení profilu pro zřizování jeden, kterou jste právě vytvořili:

    ![Visual Studio pro Mac možnosti projektu](images/image16.png)

> [!IMPORTANT]
> Můžete také nastavit nárocích klíčích v souboru Entitlement.plist a ochrany osobních údajů v souboru Info.plist. Další informace o těchto oprávnění je součástí [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

<a name="nextsteps" />

## <a name="next-steps"></a>Další kroky

Jakmile funkce bylo povoleno na straně serveru, je stále práci, kterou je potřeba zajistit, aby mohla vaše aplikace využívat funkce. Následující seznam popisuje další kroky, které může být nutné věnovat:

*   Používání oboru názvů framework ve vaší aplikaci.
*   Přidejte požadované oprávnění do vaší aplikace. Informace o požadované oprávnění a postup přidání je podrobně [Úvod do oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>Možnosti pro odstraňování potíží

Podrobnosti o níže uvedeného seznamu, některé z nejběžnějších problémů, které můžete vytvořit roadblocks při vývoji aplikace s aplikační službu povoleno.

-   Ujistěte se, že byl správné ID správně vytvořen a zaregistrován v **certifikáty, identifikátory a profily** části portálu pro vývojáře Apple.
-   Ujistěte se, že služby jsou přidané do ID aplikace (nebo rozšíření) a že služba je nakonfigurovaná pro použití aplikace skupiny nebo obchodní ID nebo kontejneru vytvořili výše v **certifikáty, identifikátory a profily** z portálu pro vývojáře Apple.
-   Ujistěte se, zda byly nainstalovány profily zřizování a ID aplikace a aplikace **Info.plist** (v projektu Xamarin) používá jednu z výše nakonfigurovaných ID aplikace.
-   Ujistěte se, že aplikace **Entitlements.plist** soubor (v projektu Xamarin) má správné služby povolena.
-   Ujistěte se, že odpovídající ochrany osobních údajů klíče se nastavují v info.plist
-   V dané aplikaci **iOS podepisování sady**, ujistěte se, že **vlastní oprávnění** je nastaven na **Entitlements.plist**. Toto je _není_ sestavení výchozí nastavení pro ladění a simulátoru iOS.

<a name="summary" />

## <a name="summary"></a>Souhrn

Tato příručka popsané možnosti, nebo _aplikační služby_a popisuje, jak se dá nastavit v sadě Visual Studio a v Centru pro vývojáře Apple. Podrobné také jak nastavit složitější služby jako Peněženka, Icloudu, dotykový identifikátor a skupiny aplikací. Nakonec zahrnutých další kroky k získání nastavení a možnosti jednoduché řešení potíží.