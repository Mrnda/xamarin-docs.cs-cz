---
title: "Instalace jako aplikaci systému Xamarin.Android"
description: "Tato příručka popisuje rozdíly mezi aplikaci systému a aplikace uživatele a postup instalace aplikace pro Xamarin.Android jako systémová aplikace. Tento průvodce se týká autorům vlastní Android ROM bitových kopií. Ho nebude vysvětlují, jak vytvořit vlastní ROM."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0113143B-7D8D-4C4C-B2F5-B966A2E7CE1F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: ad2080c61c9cc7fb376997bc56668b6db135dbae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="installing-xamarinandroid-as-a-system-app"></a>Instalace jako aplikaci systému Xamarin.Android

_Tato příručka popisuje rozdíly mezi aplikaci systému a aplikace uživatele a postup instalace aplikace pro Xamarin.Android jako systémová aplikace. Tento průvodce se týká autorům vlastní Android ROM bitových kopií. Ho nebude vysvětlují, jak vytvořit vlastní ROM._

## <a name="system-app"></a>Aplikace systému

Autoři vlastních bitových kopií Android ROM nebo výrobci zařízení se systémem Android chtít zahrnout jako aplikace pro Xamarin.Android _aplikaci systému_ při distribuci ROM nebo zařízení. Aplikace systému je aplikace, který je považován za důležité fungování zařízení nebo poskytují funkce, které vlastní ROM Autor vždy chce být k dispozici.

Aplikace systému jsou nainstalované ve složce **/System/aplikace/** (jen pro čtení adresáře v systému souborů) a nelze odstranit ani přesunout uživatelem, pokud tento uživatel má kořenový přístup. Naproti tomu se říká aplikace, která je nainstalovaná uživatel (obvykle na webu Google Play nebo zkušební načtení před prodejem aplikace) _uživatele aplikace_. Uživatel aplikace můžete odstranit uživatelem a v mnoha případech lze přesunout do jiného umístění v zařízení (například nějaký druh externí úložiště).

Aplikace systému chovat úplně stejně jako uživatel aplikace, ale mají následující důležitými výjimkami:

- Aplikace systému jsou lze upgradovat stejně jako normální _uživatele aplikace_. Ale protože kopie aplikace vždy existuje v **/System/aplikace/**, je vždycky možné vrátit zpět na původní verzi aplikace.

- Aplikace systému může být poskytnuto určité jen systému oprávnění, která nejsou k dispozici pro uživatele aplikace. Je například oprávnění jen systému [ `BLUETOOTH_PRIVILEGED` ](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED), která umožňuje aplikacím spárovat s Bluetooth zařízení bez nutnosti zásahu uživatele.

Je možné k distribuci aplikace jako aplikaci systému Xamarin.Android. Kromě poskytování APK vlastní ROM, jsou k dispozici dvě sdílené knihovny, **libmonodroid.so** a **libmonosgen 2.0.so** , je třeba ručně zkopírovat z APK k filesytem ROM bitové kopie. Tato příručka vysvětluje kroky.

## <a name="restrictions"></a>Omezení

Tento průvodce se týká autorům vlastní Android ROM bitových kopií. Ho nebude vysvětlují, jak vytvořit vlastní ROM.

Tato příručka předpokládá znalost [balení APK verze pro Xamarin.Android](~/android/deploy-test/publishing/index.md) a pochopení [architektury procesoru](~/android/app-fundamentals/cpu-architectures.md) pro aplikace pro Android.

## <a name="install-a-xamarinandroid-app-as-a-system-app"></a>Instalace aplikace pro Xamarin.Android jako aplikaci systému

Následující kroky popisují postup instalace aplikace Xamarin.Android jako aplikaci systému.

1. **Balíček verze APK aplikace Xamarin.Android** &ndash; to je podrobněji popsané v pomocí [publikování aplikace](~/android/deploy-test/publishing/index.md) průvodce.

2. **Extrahování sdílené knihovny z APK** &ndash; pomocí libovolného programu nástroj ZIP, otevřete soubor APK a ověřit obsah **/lib/** složky. Tato složka bude mít podadresáři pro každou _binární rozhraní aplikace_ (ABI) je podporováno v aplikaci; obsah této složky bude obsahovat všechny sdílené knihovny, které jsou vyžadované na tuto konkrétní aplikaci ABI:

    ![Snímek obrazovky .so soubory ve složce armeabi v7a taskypro.zip](install-system-app-images/install-system-app-01.png)

   V předchozím snímku obrazovky je jedinou podporovanou ABI (**armeabi v7a**) podržíte dva **.so** soubory, které jsou požadované aplikací. Všimněte si, že je pouze nutné extrahovat ABI soubory, které jsou vhodné pro architekturu cílového zařízení ROM nebo zařízení, tj. nekopírujte **.so** souborů z **x86** složku pro  **armeabi v7a** zařízení nebo ROM.

3. **Zkopírujte soubory .so/systém/lib** &ndash; kopie **.so** soubory, které se extrahují z APK v předchozím kroku do **/System/lib/** složky na vlastní ROM.

4. **Zkopírujte soubor APK/systém/aplikaci** &ndash; posledním krokem je APK soubor zkopírovat **/systém/aplikace** složky na ROM.


## <a name="summary"></a>Souhrn

Tato příručka popsané rozdíl mezi _aplikaci systému_ a _uživatele aplikace_a vysvětlení najdete postup instalace aplikace pro Xamarin.Android jako aplikaci systému.



## <a name="related-links"></a>Související odkazy

- [Publikování aplikace](~/android/deploy-test/publishing/index.md)
- [Architektury procesorů](~/android/app-fundamentals/cpu-architectures.md)
- [BLUETOOTH_PRIVILEGED](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED)
- [Správa ABI](https://developer.android.com/ndk~/abis.html)
