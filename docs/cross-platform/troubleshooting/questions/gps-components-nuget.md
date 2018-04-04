---
title: Sjednotit Google Play Services součásti a NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: e0ba5ee9417917b834ab060a94f72d1f071b4912
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Sjednotit Google Play Services součásti a NuGet

### <a name="history"></a>Historie

Existuje používá několik Google Play Services součásti a balíčky NuGet:

-   Služby Google Play (Froyo)
-   Služby Google Play (Perník)
-   Google Play Services (ICS)
-   Služby Google Play (JellyBean)
-   Služby Google Play (KitKat)

Google ve skutečnosti jenom .jar dodává dva soubory služby Google Play:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Nesrovnalost existovalo, protože naše nástrojů nebyla správně určit `aapt.exe` prostředků maximální úroveň rozhraní API byla pro danou aplikaci. Vynutila, dostali jsme chyby při kompilaci, pokud jsme se pokusili pomocí vazby Google Play Services (KitKat) na nižší úrovni rozhraní API jako Perník.

### <a name="unifying-google-play-services"></a>Sjednotit služby Google Play

V novějších verzích Xamarin.Android jsme teď říct `aapt.exe` jaké maximální prostředků verze se má použít, takže tento problém vyčkat, abychom.

To znamená, neexistuje žádný důvod skutečné mít samostatné balíčky Perník/sdílení připojení k Internetu nebo JellyBean/KitKat (ale stále potřebujeme samostatnou vazbu pro Froyo vzhledem k tomu, že je soubor různých .jar zcela).

Aby usnadnily pro vývojáře, jsme jste nyní unified naše součásti a NuGet balíčky do dvou:

-   Google Play Services (Froyo) (váže `google-play-services-froyo.jar`)
-   Služby Google Play (váže `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Která má být použita?

Téměř vždy třeba použít služby Google Play. Použití balíčku (Froyo) je pouze pokud aktivně cílíte Froyo. Pouze z důvodu, že tento soubor samostatné .jar existuje z Google je totiž Froyo na malé procento zařízení, jsou samy jste se rozhodli zastavit, podpora, takže tento soubor .jar je ukotvené, nepodporovaný snímek služby Google Play.

### <a name="note-about-gingerbread"></a>Poznámka o Perník

Perník nemá Fragment ve výchozím nastavení podporovat a z toho důvodu některé třídy vazba nebude možné použít v aplikaci za běhu na Perník zařízení. Třídy jako `MapFragment` nebude fungovat na Perník a jejich podpora variant by místo toho použít `SupportMapFragment`. Je to na vývojáři vědět, který se použije. Tato nekompatibilita je poznamenat Google v jejich dokumentaci služby Google Play.

### <a name="what-happens-to-the-old-componentsnugets"></a>Co se stane v původním součásti nebo NuGet?

Vzhledem k tomu, že už nejsou potřeba, máme zakázána nebo Delisted následující součásti nebo NuGets:

-   Služby Google Play (Perník)
-   Služby Google Play (JellyBean)
-   Služby Google Play (KitKat)

Existující _služby Google Play (ICS)_ součásti nebo Nuget byl přejmenován na _služby Google Play_ a se zachová aktuální do budoucna. Všechny projekty odkazující na jeden z balíčků zakázána nebo Delisted by měl aktualizovat, a použít tento jeden.

Zakázané komponenty stále existovat a musí být obnovitelné pro projekty, které se stále odkazují, abyste předešli porušení je. Podobně delisted balíčky NuGet stále existovat a může být obnovena. Se nebude aktualizovat do budoucna.
