---
title: "Proč se nemůžete přihlásit do Xamarinem v sadě Visual Studio nebo Visual Studio pro Mac?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b50969e4c1d75c0bd79c08223dd959241dcb229a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>Proč se nemůžete přihlásit do Xamarinem v sadě Visual Studio nebo Visual Studio pro Mac?

> [!IMPORTANT]
> Tato příručka nevztahuje na většina uživatelů MSDN protože nemusí vlastníte nebo přihlaste Xamarin účty, pokud pomocí [Xamarin součásti ukládání](https://components.xamarin.com/) nebo [Visual Studio pro Mac (Mac)](~/cross-platform/get-started/requirements.md). To by měla odkazovat držitele licence MSDN [licencování možnosti Průvodce](~/cross-platform/get-started/requirements.md) místo.



## <a name="overview"></a>Přehled
Existuje několik běžných příčin, které mohou bránit přihlášení k účtu Xamarin v prostředí IDE. Známé problémy a chyb jsou popsané níže.

### <a name="finding-the-login-screen"></a>Hledání přihlašovací obrazovku

Pro referenci přihlašovací obrazovky nachází tady:

- Visual Studio for Mac
   - Pravém horním rohu na úvodní obrazovce
   - **Visual Studio pro Mac > účet** (Mac)
   - **Nástroje > účet** (Windows)
- Visual Studio
   - **Nástroje > účet Xamarin**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>Prostředí IDE se připojuje, ale na obrazovce účet není zobrazuje správné přihlašovací údaje

Tento problém obvykle řeší licence ruční synchronizaci.
Postupujte podle pokynů v tomto článku: [jak na to ručně resyncronize Xamarin licence?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>Neplatné informace o účtu

Pokud přejdete na web Xamarin [přihlašovací stránku](https://store.xamarin.com/Login?from=%2faccount%2f), můžete zkusit protokolování se pomocí přihlašovacích údajů aktuálního účtu.
Stránce má také odkaz k resetování hesla a vytvořit nový účet.

## <a name="account-is-valid-but-the-ide-cant-connect"></a>Účet je platný, ale nemůže připojit prostředí IDE

To se obvykle stává, pokud brána firewall nebo jiná nastavení zabezpečení blokovat IDE v přístupu k potřebné koncové body.
Aktivační servery musí přístup následující:

> Activation.xamarin.com store.xamarin.com auth.xamarin.com

Ale několik dalších koncových bodů jsou důležité pro obecné vývoj procesů, jako je aktualizace, získání balíčky NuGet, atd. Z toho důvodu se doporučuje zajistit *všechny* koncových bodů se přidají [příručce konfigurace brány firewall Xamarin](~/cross-platform/get-started/installation/firewall.md).

### <a name="ios-in-xamarin-studio-windows"></a>iOS v Xamarin Studio Windows
vývoj pro iOS nepodporuje Xamarin Studio pro Windows. Při přístupu k obrazovce přihlášení, iOS licence, které se nezobrazí.

Místo toho přihlášení prostřednictvím (Mac) Xamarin Studio nebo Visual Studio s licencí starší verze. Všimněte si, že uživatelé MSDN v systému Windows, není nutné přihlásit.

## <a name="additional-support"></a>Další podporu

Pokud se výše uvedených scénářích není popisují vaší situaci nebo vyřešte problém, naleznete v těchto [podporují možnosti](https://www.xamarin.com/support).
