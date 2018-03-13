---
title: "Profily zřizování"
description: "Tento průvodce vás provede vytvořením nezbytné profily zřizování, který bude vyžadovat, aby publikování Xamarin.Mac aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: bdff6c32-f7e3-4a97-a093-dbda48be8227
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: dab923f6150bdf005e9468add6d26d4fdb691a93
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="provisioning-profiles"></a>Profily zřizování

Zřizování profily umožňují vývojář do své aplikace Xamarin.Mac zahrnout několik konkrétních funkcí (dříve označované jako Mac OS X) v systému macOS (například Icloudu a nabízená oznámení). Se musí vytvořit, stáhněte a nainstalujte profil zřizování Mac pro každou aplikaci, jejich vývoj, který používá tyto funkce.

[![](profiles-images/certif13.png "Portál zřizování Apple")](profiles-images/certif13.png#lightbox)

<a name="Development_Provisioning_Profile" />

## <a name="development-provisioning-profile"></a>Vývoj profilu pro zřizování

Profil pro zřizování vývoj umožňuje aplikace má být testována v konkrétních počítačích, které byly vytvořeny v profilu na cílové Mac App Storu. To je zvlášť důležité při použití systému macOS funkcí, jako je iCloud a nabízených oznámení.

> [!NOTE]
> Vývojář musí jste již vytvořili vývojový certifikát Mac dřív, než můžete vytvoří profil pro zřizování vývoj. Dokončení podrobnosti, jak je vidět na tomto snímku obrazovky ke generování **profil zřizování vývoj** který lze použít k vytvoření sestavení. Musí být platný certifikát vývoj Mac v dostupné **certifikát** pole a alespoň jeden systém zaregistrovat pro testování.

Postupujte takto:

1. Vyberte typ zřizování profil, a klikněte na **pokračovat** tlačítko: 

     [![](profiles-images/certif14.png "Vyberte typ profilu")](profiles-images/certif14.png#lightbox)
2. Vyberte ID aplikace pro vytvoření profilu a klikněte na **pokračovat** tlačítko: 

     [![](profiles-images/certif15.png "Výběr ID aplikace")](profiles-images/certif15.png#lightbox)
3. Vyberte vývojáře ID používá k podepisování profilu a klikněte na tlačítko **pokračovat**: 

     [![](profiles-images/certif16.png "Výběr ID vývojáře")](profiles-images/certif16.png#lightbox)
4. Vyberte počítače, které lze použít na tento profil a klikněte na tlačítko **pokračovat**: 

     [![](profiles-images/certif17.png "Výběr povolené počítačů")](profiles-images/certif17.png#lightbox)
5. Nyní, zadejte **název profilu** a klikněte na tlačítko **generování** tlačítko: 

     [![](profiles-images/certif18.png "Generování profil")](profiles-images/certif18.png#lightbox)
6. Klikněte **Stáhnout** tlačítko Stáhnout nový profil: 

     [![](profiles-images/certif19.png "Stažení profilu")](profiles-images/certif19.png#lightbox)
7. V podokně předvolby profily počítačů Mac se nainstalovaly vývoj zřizovacích profilů **předvolbách systému** aplikace: 

     [![](profiles-images/certif20.png "Instalaci profilu")](profiles-images/certif20.png#lightbox)
8. V podokně předvolby profil se zobrazí všechny nainstalované profily: 

     [![](profiles-images/image47.png "Zobrazuje nainstalované profily")](profiles-images/image47.png#lightbox)
9. Profil se také zobrazí v **vývojáře certifikát nástroj** v případě, že musí být znovu stažena: 

     [![](profiles-images/image48.png "Nástroj certifikátu vývojáře")](profiles-images/image48.png#lightbox)

Nový profil zřizování vývoj bude nutné vytvořit pro každou novou aplikaci nebo do nového počítače se přidává do na test.

<a name="Production_Provisioning_Profile" />

## <a name="production-provisioning-profile"></a>Profil pro zřizování produkční

Produkční zřizovacích profilů jsou potřebné k vytvoření balíčku pro odeslání Mac App Storu.

Postupujte takto:

1. Vyberte typ profilu a klikněte na **pokračovat** tlačítko: 

    [![](profiles-images/certif21.png "Vyberte typ profilu")](profiles-images/certif21.png#lightbox)
2. Vyberte ID aplikace k vytvoření profilu a klikněte na **pokračovat** tlačítko: 

    [![](profiles-images/certif15.png "Výběr ID aplikace")](profiles-images/certif15.png#lightbox)
3. Vyberte ID společnosti k podepisování profilu a klikněte na **pokračovat** tlačítko: 

    [![](profiles-images/certif23.png "Výběr ID společnosti")](profiles-images/certif23.png#lightbox)
4. Zadejte **název profilu** a klikněte na **generování** tlačítko: 

    [![](profiles-images/certif24.png "Generování profil")](profiles-images/certif24.png#lightbox)
5. Klikněte na tlačítko **Stáhnout** získat soubor profilu zřizování (rozšíření `.provisionprofile`): 

    [![](profiles-images/certif25.png "Stažení profilu")](profiles-images/certif25.png#lightbox)
6. Přetáhněte ji do **Xcode organizátora** nebo dvojím kliknutím na nainstalovat. Profil se potom zobrazí v Xcode médií: 

    [![](profiles-images/image51.png "Instalaci profilu")](profiles-images/image51.png#lightbox)
7. Profil pro zřizování se také zobrazí v seznamu: 

    [![](profiles-images/certif26.png "Zobrazuje nainstalované profily")](profiles-images/certif26.png#lightbox)


Vývojář někdy změní-li funkce používá ID aplikace (např. povolení Icloudu nebo nabízená oznámení) se musí znovu vytvořit profily zřizování pro ID této aplikace.

## <a name="related-links"></a>Související odkazy

- [Instalace](~//mac/get-started/installation.md)
- [Hello, ukázka Mac](~//mac/get-started/hello-mac.md)
- [Distribuce aplikací na Mac App Storu](https://developer.apple.com/devcenter/mac/checklist/)
- [Nástroje Průvodce: Aplikace pro podepisování kódu](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [ID vývojáře a těchto pravidel](https://developer.apple.com/resources/developer-id/)
