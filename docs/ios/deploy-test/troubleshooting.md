---
title: "Poradce při potížích"
description: "Tipy a triky pro vytvoření smooth nasazení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: 174d1cf974c39420b932d494d5b28c62d7fd1eb1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshooting"></a>Poradce při potížích

## <a name="code-signing--provisioning"></a>Podepisování kódu a zřizování

Podepisování kódu & zřizování s iOS může být poměrně nevhodných, a proto je důležité se ujistit, že kód podepisování certifikátů, profily zřizování jsou v pořadí.

* Velkými týmy neměly by pomocí tlačítka "Opravit problém" v Xcode, znázorněno zde:

    [![](troubleshooting-images/fixissue.png "Dialogové okno vyřešit problémy")](troubleshooting-images/fixissue.png#lightbox)

    Tím se vytvoří nový zajišťování profilů a certifikátů. Na nejvyšší tím se vytvoří profil pro zřizování pokaždé, když se člen týmu na něj klikne, způsobuje disorganization pomocí profilů. V nejhorším případě odvolává certifikáty pro všechny ostatní ve společnosti, způsobuje svoje aplikace přestane fungovat.

* Přístup do řetězce klíčů zachovat uspořádány a odstranit certifikáty s vypršenou platností a profily. Certifikáty pro rozlehlé sítě poslední po dobu tří let, zatímco ostatní poslední pouze jeden rok. Certifikáty nelze obnovit, takže bude potřeba k vytvoření nové certifikáty těsně před staré vyprší. Zajistěte, aby odvolání a odstranit staré certifikáty a opětovné podepisování aplikací pomocí nových certifikátů.

* Odeberte starý zřizovacích profilů, protože jsou nainstalovány nové. To znamená, že Visual Studio pro Mac není na pozici, kde má rozhodnout, který profil chcete použít. Toho dosáhnete tak, nejdřív si nezapomeňte odstranit profil v Apple developer center a potom vyhledejte *Předvolby > váš účet > Zobrazit podrobnosti...* . Vyberte profil zřizování a klikněte na **zobrazit ve vyhledávací**. To bude odhalit umístění profilu v souboru systému Mac, kde je pak odstraněním pomocí funkce hledání.

* Ujistěte se, že všechny požadované certifikáty a odpovídající privátní klíče jsou k dispozici. Pro každý tým bude nutné certifikátu vývojáře (pro instalaci aplikace na vlastní zařízení) a certifikát distribuční (k instalaci na jiných zařízeních)

* Spusťte Xcode a Visual Studio pro Mac / Visual Studio při instalaci nový certifikát a profil zřizování.


## <a name="testflight"></a>TestFlight

V některých případech testování není přejděte poměrně jako plynule podle plánu.  Tyto kroky můžou pomoct s vyřešte problémy s TestFlight:

- TestFlight dostupný jen pro aplikace cílený na iOS 8 a vyšší.

- Musí být *profil distribuce obchod* s nárocích beta.

- **Nové iOS aplikace odeslání** okno musí obsahovat přesně stejné informace jako aplikace **Info.plist**, a všechny oddíly musí být vyplněna. Ikony musí být zadány pro aplikace před jejich odesláním do TestFlight.

- Při nahrávání nového sestavení bude někde trvat od 1 do 5 minut, dokud sestavení se zobrazí v iTunes připojit.

- [Testování Beta TestFlight přepínač](~/ios/deploy-test/testflight.md#beta-testing) musí být zapnutý pro každou verzi vaší aplikace.

- Každý člen týmu vývojáře, která je také interní testování služby musí mít **interní testování** přepínač zapnutý.

- Uživatelé, kteří patří do nebo vlastní jiné iTunes připojit účet nemůže být interní testerům, sada. Pouze mohou být přidány jako externí testerům, sada.

- Interní a externí uživatelé jsou přidány, vybrané a pozvat samostatně. Každý seznamu musí být spravované samostatně.

- Apple musí schválit každé sestavení, který bude distribuován na externí testerům, sada. Pokud se změní verze sestavení, je vyžadován kontrolu nové beta společností Apple. Pokud se změní číslo sestavení, kontrola je volitelné.

- Metadata musí být přidaný do sestavení, které jsou distribuovány do externího testerům, sada. To je přístupná kliknutím na číslo sestavení v **Moje aplikace > předběžné verze**.

- Ke kontrole každý den lze odeslat pouze dvě sestavení. Vzhledem k tomu, že změníte verzi vynutí kontrolu, to znamená, že verze, kterou čísla lze změnit pouze dvakrát denně.

<a name="Automatically_copy_app_bundles_back_to_Windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Automaticky zkopírujte .app sady zpět do systému Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
