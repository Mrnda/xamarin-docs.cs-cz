---
title: "Proč moje sestavení iOS dojde k chybě: nalezen žádný platný iPhone kód podpisových klíčů v řetězci klíčů?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6a853bc7186647dbdae1d5d12f3b185d302a8088
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Proč moje sestavení iOS dojde k chybě: nalezen žádný platný iPhone kód podpisových klíčů v řetězci klíčů?

## <a name="cause-of-the-error"></a>Příčinu chyby
Tato chybová zpráva nastane, když v projektu hledá platné přihlašovací údaje podepisování kódu, ale nemůže nalézt je. Podepisování kódu se vyžaduje pro účely testování a nasazení na zařízení s iOS fyzické; stejně jako aplikace pro Ad-hoc a ukládání sestavení. 


### <a name="provisioning-devices"></a>Zřizování zařízení
Pokud zařízení s iOS před nebyly zřízené, následující průvodce vás provede úplné podrobný postup: [Průvodce zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Chyba při použití simulátoru iOS

> [!NOTE]
> Tento problém byl vyřešen v posledních verzích Xamarin pro Visual Studio. Ale pokud k danému problému dojde na nejnovější verzi softwaru, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.


Chyby se v 3.11 Xamarin.Visual Studio, který chybu způsobil projekt pro iOS v šabloně Xamarin.Forms Chcete-li přidat že codesign Entitlements.plist k simulátoru sestavení; blokování efektivně testování pomocí simulátoru.

### <a name="how-to-fix"></a>Informace o vyřešení
Alternativní řešení problému můžete odebráním `<CodesignEntitlements>` příznak z ladění sestavení v souboru .csproj. To provedete následovně:

*Upozornění: Chyb v souborech .csproj může dojít k narušení projektu, takže je vhodné zálohovat soubory před pokusem o to.*

1. Klikněte pravým tlačítkem na projekt pro iOS v podokně řešení a vyberte **uvolnit projekt**
2. Klikněte pravým tlačítkem na projekt znovu a vyberte **upravit .csproj [ProjectName]**
3. Najít PropertyGroups ladění, by měla začínat znakem příznaky, které vypadají takto:
   - Ladění: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Verze: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. V každé sestavení, které používají simulátoru odstranit nebo komentář následující vlastnost: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Umožňuje znovu načíst projekt a nyní byste měli mít k nasazení do simulátoru.

### <a name="next-steps"></a>Další kroky
O další pomoc, kontaktujte nás, nebo pokud tento problém zůstane i po použití výše uvedené informace najdete v tématu [jaké možnosti podpory jsou dostupná pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace o možnostech kontaktní, návrhy, a také jak soubor nové chyby v případě potřeby. 
