---
title: Některé konkrétní chyb při správě licencí
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 8592fe5381c974e999477d0ef6ca6ebdd8b38cc4
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2018
---
# <a name="some-specific-licensing-errors"></a>Některé konkrétní chyb při správě licencí

> [!IMPORTANT]
> Níže uvedené informace nevztahuje uživatelům MSDN, protože jsou problémy, které jsou specifické pro starší verze Xamarin licence. Pokud uživatelé MSDN a zobrazí chyby podobné těm, níže, zkuste to prosím [aktualizaci na nejnovější verzi Xamarin](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) & pokus [licencování možnosti Průvodce](~/cross-platform/get-started/requirements.md).



## <a name="invalid-license"></a>"Neplatný licence"

> Neplatná licence. Prosím znovu aktivujte Xamarin.Android (XA9999) se nepodařilo určit edici. (XA9010)

Tyto zprávy můžete zobrazit v několika různých okolností.

-   Aktuální licencí na disk může být se na synchronizaci s informace o aktuálním uživateli v počítači. [Obnovení souborů s licencí](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) vyřešit tento problém v mnoha případech.

-   Počítač může mít 2 konfliktní duplicitní aktivací. Tento typ licence se už používá pomocí Xamarin. Pokud máte problémy, které se zobrazují mají vztahovat k této chybě prosím [e-mailová podpora](https://www.xamarin.com/support).

-   Účet Xamarin nemusí být ještě pozvánku, abyste licenci Xamarin. Viz také: [tým Správa licencí](~/cross-platform/troubleshooting/legacy-licenses/team-management.md).

## <a name="failed-to-load-android-entitlements"></a>"Nepodařilo se načíst Android oprávnění"

> Mono.VisualStudio.Shell.ShellPackage Chyba: 0: Nepodařilo se načíst Android oprávnění: Neplatná licence. Prosím znovu aktivovat Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage Chyba: 0: licence aktualizace se nezdařilo: Neplatná licence. Prosím znovu aktivovat Xamarin.Android (XA9999)

Toto je starší verze "Neplatná licence" problému z výše. Použít stejný postup.

## <a name="this-version-was-released-after-your-subscription-expired"></a>"Tato verze byla vydána po vypršení platnosti předplatného"

> Chyba XA9000: Tato verze byla vydána po vypršení platnosti předplatného (11/11/2014 5:23:41: 00). (XA9000) (Mobile.Android.Utilities) XA9010 Chyba: Nelze určit edici. (XA9010) (Mobile.Android.Utilities)

Tyto zprávy se obvykle zobrazují ve dvou situacích:

-   Licence na disku jsou zastaralé. [Obnovení souborů s licencí](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) vyřešit tento problém v mnoha případech.

-   Xamarin předplatné už je aktivní a aktuální nainstalovaná verze Xamarin je pozdější než datum vypršení platnosti předplatného. V takovém případě je správná oprava [downgradovat](http://kb.xamarin.com/customer/portal/articles/1699777) na verzi nástroje Xamarin, která byla vydána, než vyprší platnost předplatného. Nebojte se e-mailovou zprávu na [ hello@xamarin.com ](mailto:hello@xamarin.com) požádat o platnou verzi pro vaše předplatné. Pokud stále zobrazí tato chyba po přechod na starší verzi, ujistěte se, že jste zkuste aktualizovat příliš souborů s licencí.

## <a name="additional-references"></a>Další odkazy

-   [Jak aktualizovat licence](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [Jak na starší verzi Xamarin](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Seznam kódů chyb Xamarin.Android](~/android/troubleshooting/errors.md)
-   [Seznam kódů chyb MonoTouch (Xamarin.iOS)](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>Další kroky
O další pomoc, kontaktujte nás, nebo pokud tento problém zůstane i po použití výše uvedené informace najdete v tématu [jaké možnosti podpory jsou dostupná pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace o možnostech kontaktní, návrhy, a také jak soubor nové chyby v případě potřeby.
