---
title: Sjednotit Google Play Services komponenty a NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 3f5c5f75ae1c7a44537afa59ff4a15d54b1df50b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351480"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Sjednotit Google Play Services komponenty a NuGet

## <a name="history"></a>Historie

Existuje používá několik Google Play Services součásti a balíčky NuGet:

-   Aplikace služby Google Play (Froyo)
-   Aplikace služby Google Play (Perník)
-   Služby Google Play (ICS)
-   Aplikace služby Google Play (JellyBean)
-   Aplikace služby Google Play (KitKat)

Ve skutečnosti pouze .jar dodává dvě Google soubory služby Google Play:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Nesrovnalosti existoval, protože naše nástroje nebyl správně určit `aapt.exe` prostředků maximální úroveň rozhraní API byla pro danou aplikaci. To znamená, pokud jsme se pokusili pomocí vazby aplikace služby Google Play (KitKat) na nižší úroveň rozhraní API, jako je Perník jsme obdrželi chyby kompilace.

## <a name="unifying-google-play-services"></a>Sjednocení služby Google Play

V novějších verzích Xamarin.Android jsme teď řekněte `aapt.exe` jaká verze maximální prostředků chcete použít, abyste tento problém zmizet, pro nás.

To znamená, že neexistuje žádný skutečný důvod mít samostatné balíčky pro Perník/sdílení připojení k Internetu nebo JellyBean/KitKat (ale stále potřebujeme samostatnou vazbu pro Froyo vzhledem k tomu, že je soubor různých .jar úplně).

Usnadnili pro vývojáře, jsme jste nyní sloučený naše komponenty a NuGet na dva balíčky:

-   Služby Google Play (Froyo) (váže `google-play-services-froyo.jar`)
-   Služby Google Play (váže `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Které z nich je vhodné používat?

V téměř všech případech by měla sloužit služby Google Play. Jediným důvodem pro použití balíčku (Froyo) je, pokud aktivně cílíte Froyo. Je pouze z důvodu, že tento soubor .jar samostatné existuje z Googlu, protože Froyo se na malé procento zařízení, sami rozhodli jste se zastavit podporu, takže tento soubor .jar zmrazeno, nepodporované snímek služby Google Play.

### <a name="note-about-gingerbread"></a>Poznámka o Perník

Perník nemá Fragment ve výchozím nastavení podporovat a z tohoto důvodu některé třídy ve vazbě nebude možné použít v aplikaci za běhu na Perník zařízení. Třídy jako `MapFragment` nebudou fungovat ve Perník a jejich podpora variant by místo toho používat `SupportMapFragment`. Záleží jen na vývojářské vědět, která se použije. K této nekompatibilitě je uvedeno ve Google v jejich dokumentaci služby Google Play.

### <a name="what-happens-to-the-old-componentsnugets"></a>Co se stane staré komponenty/NuGet?

Protože už nejsou potřeba, máme zakázáno/Delisted následující součásti a balíčky Nuget:

-   Aplikace služby Google Play (Perník)
-   Aplikace služby Google Play (JellyBean)
-   Aplikace služby Google Play (KitKat)

Existující _služby Google Play (ICS)_ Nuget komponenty/byl přejmenován na _služby Google Play_ a bude se udržovat aktuální do budoucna. Všechny projekty odkazování na některý z balíčků zakázáno/Delisted má být aktualizován na použití tohoto objektu.

Zakázané komponenty stále existovat a by měl být obnovitelné pro projekty, které se na ně stále odkazuje, aby se zabránilo přerušení je. Podobně delisted balíčky NuGet stále existovat a dají se obnovit. Neaktualizují do budoucna.
