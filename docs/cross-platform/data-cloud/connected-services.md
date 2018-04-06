---
title: Připojené služby v sadě Visual Studio pro Mac
description: Přidání úložiště dat Azure, ověřování a nabízených oznámení do mobilních aplikací v sadě Visual Studio pro Mac
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b9aaa197ccf01c59d6e4bbb0476d10295a108f89
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="connected-services-walkthrough"></a>Návod připojených služeb

Pracovní postup připojení služby Azure pracovního postupu portálu přenese do sady Visual Studio pro Mac, takže není nutné opustit projektu na přidání služeb.

Tento návod ukazuje, jak přidat Azure back-end službu, která přináší datové úložiště v cloudu, ověřování a odesílání nabízených oznámení do aplikace Xamarin.Forms přenosných třída knihovny PCL () různých platformách.


1.  Spusťte dvojitým kliknutím na **připojené služby** uzlu v řešení, které se otevře **služby Galerie**.
  Toto je seznam všech služeb, které jsou k dispozici pro tento typ aplikace. Vyberte službu (například **mobilního back-endu službou Azure App Service**) kliknutím na.

  [![](connected-services-images/image001-sml.png "Připojené uzel služeb v sadě Visual Studio pro Mac")](connected-services-images/image001.png#lightbox)

2. Stránce s podrobnostmi o službu má popis služby a závislosti k instalaci.
  Klikněte **přidat** tlačítko přidejte závislosti do aplikace:

  [![](connected-services-images/image002-sml.png "Mobilní back-end v Azure")](connected-services-images/image002.png#lightbox)

3. Závislosti nutné přidat PCL a specifické pro platformu projekty fungovat.
  Vyberte zaškrtávací políčka pro přidání služby pro každý projekt, který bude odkazovat ho (přímo ani nepřímo):

  [![](connected-services-images/image003-sml.png "Zkontrolujte všechny projekty, které by měla odkazovat služby")](connected-services-images/image003.png#lightbox)

4. Zvolte **přijmout** na **přijetí licence** dialogová okna pro balíčky NuGet.
  Mohou existovat dvě dialogová okna tak, aby přijímal, jeden pro MobileClient a závislosti a druhý pro SQLiteStore, což je vyžadováno pro synchronizaci dat offline:

  [![](connected-services-images/image004-sml.png "Přijměte licenční smlouvy")](connected-services-images/image004.png#lightbox)

  ![](connected-services-images/image005.png "Okno přijetí licence")

5. Po přidání závislostí, budete vyzváni k přihlášení pomocí účtu, který chcete použít ke komunikaci s Azure.
  Pokud jste již přihlášení ID služeb Microsoft, Visual Studio pro Mac se pokusí načíst vašich předplatných Azure a všechny aplikace služby s nimi spojených. Pokud nemáte žádné předplatné, můžete přidat jeden registrací k bezplatné zkušební verze nebo zakoupení plán předplatného na portálu Azure.

6. Vyberte aplikační službu ze seznamu. Tato hodnota se vyplní kód šablony pro `MobileServiceClient` objekt se adresa URL odpovídajícího v Azure app service:

  [![](connected-services-images/image006-sml.png "Vyberte ze seznamu služby app service")](connected-services-images/image006.png#lightbox)

  Pokud neexistují uvedené služby, klikněte **nový** tlačítko (viz krok 9.)

7. Zkopírujte kód šablony pro `MobileServiceClient` do PCL. Aplikace umístění souboru v není důležité, dokud je jenom jednu instanci.
  Doporučuje se vytvořit `AzureService` třídu, která zpracovává všechny interakce Azure a používá `MobileServiceClient`:

  ![](connected-services-images/image007.png "Zkopírujte konfigurační kód do aplikace")

8. Postupujte podle dokumentace v **další kroky** přidat data, offline synchronizace, ověřování a nabízená oznámení do vaší aplikace:

  [![](connected-services-images/image008-sml.png "Projít pokyny pro další kroky")](connected-services-images/image008.png#lightbox)

10. Pokud nemáte žádné existující služby, aplikace, můžete vytvořit nové služby z v sadě Visual Studio for Mac.
  Klikněte na tlačítko **nový** tlačítko v levé dolní části seznamu služeb otevřete **nové služby App Service** dialogové okno:

  [![](connected-services-images/image009-sml.png "Vytvoření nové aplikace služby v sadě Visual Studio pro Mac")](connected-services-images/image009.png#lightbox)

Nová služba vyžaduje následující parametry:

-   **Název služby App service** – jedinečný název nebo id plánu
-   **Předplatné** – předplatné, které chcete použít k platbě za službu
-   **Skupina prostředků** – stejným způsobem jako nebo uspořádání všechny prostředky Azure pro projekt. Možnost použít existující nebo vytvořte novou. Pokud je to první službě Azure, vytvořte novou.
-   **Plánování služby** – Určuje umístění a náklady na všechny prostředky, které ho používají. Možnost použít existující nebo vytvořte novou. Pokud je to první službě Azure, použijte výchozí nastavení nebo vytvořte novou v úrovni free (F1).

Přejděte [dokumentaci služby Azure App Service](https://docs.microsoft.com/azure/app-service/) Další informace


## <a name="related-links"></a>Související odkazy

- [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/)
- [Zpráva k vydání verze](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
