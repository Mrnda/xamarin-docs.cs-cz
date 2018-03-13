---
title: "Volné zřizování"
description: "Verze společnosti Apple Xcode 7 pocházejí důležité změně pro všechny iOS a Mac vývojáři – volné zřizování."
ms.topic: article
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4e93696f8eef44030ffacbdbaa8ebcd860a402f6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="free-provisioning"></a>Volné zřizování

_Verze společnosti Apple Xcode 7 pocházejí důležité změně pro všechny iOS a Mac vývojáři – volné zřizování._

Volné zřizování umožňuje vývojářům k nasazení své aplikace pro Xamarin.iOS na svém zařízení s iOS **bez** stal součástí žádné **programu pro vývojáře Apple**. To je velmi výhodné pro vývojáře, jako testování na zařízení testování v simulátoru, včetně, ale bez omezení na paměti, úložiště, připojení k síti, mimo jiné umožňuje řadu výhod.

Zřizování bez vývojáře Apple účet se musí provádět prostřednictvím Xcode, která vytvoří *identitu podepisování* (obsahující privátní klíč a certifikátu vývojáře) a *profil zřizování* () obsahující explicitní ID aplikace a UDID vašeho zařízení připojených iOS).

## <a name="requirements"></a>Požadavky

Pokud chcete využít výhod nasazení vaší Xamarin.iOS musí aplikace na zařízení s bezplatnou zřizování pomocí Xcode 7 nebo novější.

**Apple ID, které používá nesmí být připojen k žádné programu pro vývojáře Apple.**

Číslo ID sady použité v aplikaci musí být jedinečný a nemůže byly použity v jiné aplikaci dříve. Žádné ID sady použité s bezplatnou zřizování můžete není znovu použít znovu. Pokud již jste distribuovali aplikace, nejde zřídit aplikaci s bezplatnou zřizování. 

Odkazovat [provede distribuci aplikací](~/ios/deploy-test/app-distribution/index.md) Další informace.

Pokud vaše aplikace používá aplikační služby, pak budete muset vytvořit profil pro zřizování podle popisu v [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md#appservices) průvodce. Se zobrazí další omezení [odpovídající část](#limitations) níže.


## <a name="a-namelaunching--launching-your-app"></a><a name="launching" /> Spuštění aplikace

Při použití volné zřizování pro nasazení aplikace do zařízení se pomocí Xcode vytvořte podpisovou identitu a zřizovacích profilů a pak bude používat Visual Studio pro Mac nebo Visual Studio vyberte správný profil k podepsání vaší aplikace s. Postupujte podle podrobný návod k tomu následující:

1. Pokud nemáte Apple ID, vytvořit na [appleid.apple.com](https://appleid.apple.com/account).
2. Otevřete Xcode a přejděte do **Xcode > Předvolby**.
3. V části **účty**, použijte  **+**  tlačítko Přidat existující ID Apple. By měl vypadat podobně jako tento snímek obrazovky:

  [![](free-provisioning-images/launchapp1.png "Xcode předvolby účty")](free-provisioning-images/launchapp1.png#lightbox)

4. Připojte zařízení iOS, které chcete nasadit do a vytvořte nový projekt iOS prázdné zobrazení jedním v Xcode. Nastavte **Team** rozevírací seznam pro Apple ID, který jste právě přidali. Musí být v podobném formátu `your name (Personal Team - your Apple ID)`:

  [![](free-provisioning-images/launchapp2.png "Vytvořit identitu podepisování")](free-provisioning-images/launchapp2.png#lightbox)

5. V části **Obecné > Identity** Ujistěte se, že identifikátor balíčku shoduje _přesně_ identifikátor balíku aplikace Xamarin.iOS a ujistěte se, cíl nasazení odpovídá nebo je nižší než zařízení s iOS připojené. Tento krok je velmi důležité, protože Xcode pouze vytvoří profil pro zřizování s explicitní ID aplikace:

  [![](free-provisioning-images/launchapp5.png "Vytvořit profil pro zřizování s explicitní ID aplikace")](free-provisioning-images/launchapp5.png#lightbox)

6. V části podpisování vyberte **automaticky spravovat podepisování** a z rozevíracího seznamu vyberte váš tým:

  [![](free-provisioning-images/launchapp6.png "Vyberte automaticky spravovat podepisování a z rozevíracího seznamu vyberte váš tým")](free-provisioning-images/launchapp6.png#lightbox)

7. V předchozím kroku automaticky generovat zřizovací profil a podpisové identity pro vás. To můžete zobrazit kliknutím na ikonu informace vedle profilu pro zřizování:

  [![](free-provisioning-images/launchapp7.png "Zobrazení profilu pro zřizování")](free-provisioning-images/launchapp7.png#lightbox)

8. K testování v Xcode, nasazení prázdné aplikace na zařízení kliknutím na tlačítko spustit.

9. Vraťte se do integrovaného vývojového prostředí, se stejným zařízením napájen ze sítě a klikněte pravým tlačítkem na název projektu Xamarin.iOS otevřete **možnosti projektu** dialogové okno. Přejděte do části podepisování sady iOS a explicitně nastavit podepisování identity a profil pro zřizování:

  [![](free-provisioning-images/launchapp8.png "Nastavte podepisování identity a profil pro zřizování")](free-provisioning-images/launchapp8.png#lightbox)

Pokud nevidíte podpisové identity nebo správný profil zřizování ve vašem IDE, musíte jej restartovat.


## <a name="a-namelimitations-limitations"></a><a name="limitations" />Omezení

Apple uložila počet omezení kdy a jak můžete použít bezplatné zřizování spusťte aplikaci na zařízení s iOS, zajistíte, že lze nasadit pouze do *vaše* zařízení. Tyto podmínky jsou uvedeny v této části.

Přístup k iTunes Connect je omezená a proto službami, jako je publikování do obchodu s aplikacemi a TestFlight nejsou k dispozici pro vývojáře volně zřizování svých aplikací. Je potřeba distribuovat přes Ad Hoc a interní znamená účet Apple Developer (Enterprise nebo osobní).

Profily vytvořené v tomto případě zřizování skončí za jeden týden, podepisování identity po jednom roce. Kromě toho profily zřizování pouze se vytvoří pomocí explicitní ID aplikace a proto je třeba postupovat podle pokynů [výše](#launching) pro každou aplikaci, kterou chcete nainstalovat.

Zřizování pro většinu aplikační služby není také možné pomocí volné zřizování. Sem patří:

- Platím Apple
- Herní Centrum
- iCloud
- V aplikaci nákupu
- Nabízená oznámení
- Peněženka (byl Passbook.)

Úplný seznam je poskytovaných společností Apple v jejich [podporované možnosti](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html#//apple_ref/doc/uid/TP40012582-CH38-SW1) průvodce. Zřídit aplikace pro použití s aplikačními službami, najdete [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) příručky.


## <a name="summary"></a>Souhrn

Tato příručka obsahuje prozkoumali výhody a omezení použití volné zřizování k instalaci aplikací na zařízení s iOS. Také byl spojen, krok za krokem, instalace aplikace Xamarin.iOS pomocí volné zřizování.

## <a name="related-links"></a>Související odkazy

- [Zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md)
- [Zřizování pro aplikační služby](~/ios/get-started/installation/device-provisioning/index.md#appservices)
