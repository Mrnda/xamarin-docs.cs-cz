---
title: 'Proč moje sestavení iOS selže s: žádný kód platný iPhone podpisových klíčů se nenašel v řetězci klíčů?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 75f9a78fdc7d15df217378491f016478cde369ff
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351096"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Proč moje sestavení iOS selže s: žádný kód platný iPhone podpisových klíčů se nenašel v řetězci klíčů?

## <a name="cause-of-the-error"></a>Příčinu chyby
Tato chybová zpráva nastane na počátku v projektu hledá platné přihlašovací údaje podepisování kódu, ale nedaří najít. Podepisování kódu se vyžaduje k testování a nasazování na fyzická zařízení iOS; stejně jako aplikace pro Ad-hoc a ukládání sestavení. 


### <a name="provisioning-devices"></a>Zřizování zařízení
Pokud jste zařízení se systémem iOS před nezřídili, následující průvodce vás provede úplné podrobný postup: [Průvodce zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Chyby při použití simulátor iOS

> [!NOTE]
> Tento problém byl vyřešen v nejnovějších verzích Xamarin pro Visual Studio. Ale pokud k problému dochází na nejnovější verzi softwaru, požádejte prosím [nová chyba](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s plnou verzí informace a plná výstup protokolu sestavení.


Došlo chybu v 3.11 Xamarin.Visual Studio, která způsobila projektu pro iOS v šabloně Xamarin.Forms přidáte do souboru Entitlements.plist na simulátor podepisování kódu od sestavení; blokování efektivní testování s využitím simulátoru.

### <a name="how-to-fix"></a>K vyřešení
Můžete tento problém obejdete tak, že odeberete `<CodesignEntitlements>` příznak z ladění sestavení v souboru csproj. Provedete to takto:

*Upozornění: Chyby v souborech .csproj může dojít k narušení projektu, takže je vhodné zálohovat soubory před pokusem o provedení této.*

1. Klikněte pravým tlačítkem na projekt pro iOS v podokně řešení a vyberte **uvolnit projekt**
2. Klikněte znovu pravým tlačítkem projekt a vyberte **upravit .csproj [názevprojektu]**
3. Vyhledejte element PropertyGroups ladění, by měla začínat řetězcem příznaky, které vypadají takto:
   - Ladění: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Verze: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. V každém sestavení, které používají simulátor odstranit nebo okomentovat následující vlastnost: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Znovu načíst projekt a byste měli nasadit na simulátor.

### <a name="next-steps"></a>Další kroky
Potřebujete další pomoc, kontaktujte nás, nebo pokud tento problém i po použití výše uvedené informace, najdete [jaké možnosti podpory jsou k dispozici pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace týkající se možností kontaktu, návrhy, jak se dá soubor s novou chybou, v případě potřeby. 
