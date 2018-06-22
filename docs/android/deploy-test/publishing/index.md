---
title: Publikování aplikace
ms.prod: xamarin
ms.assetid: 51E19000-040A-2B74-C462-EC57C617085C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 99efc1b6cbef4e9004da564d433e891f36d0df49
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764033"
---
# <a name="publishing-an-application"></a>Publikování aplikace

Po vytvoření kvalitních aplikací, uživatelé chtějí ho použít. Tato část obsahuje kroky s distribuci veřejného aplikace vytvořené s Xamarin.Android prostřednictvím kanálů například e-mailu, privátní webového serveru, webu Google Play nebo obchodu s aplikacemi Amazon pro Android.


## <a name="overview"></a>Přehled

Posledním krokem při vývoji aplikace pro Xamarin.Android se k publikování aplikace. Publikování je kompilaci aplikace pro Xamarin.Android, aby je připraven pro uživatele k instalaci na svoje zařízení, a zahrnuje dvě základní úlohy:

-   **Příprava pro publikaci** &ndash; verzi aplikace je vytvořen, který se dá nasadit na zařízení používá technologii Android (najdete v části [Příprava aplikace pro verzi](~/android/deploy-test/release-prep/index.md) Další informace o uvolnění přípravy).

-   **Distribuce** &ndash; verzi aplikace je k dispozici prostřednictvím jednoho nebo více různých distribučních kanálů.

Následující diagram znázorňuje kroky s publikováním aplikace pro Xamarin.Android:

[![Vytváření a nasazování vývojový diagram](images/build-and-deploy-steps.png)](images/build-and-deploy-steps.png#lightbox)

Jak můžete vidět diagramu výše, přípravy je stejný bez ohledu na metodu distribuce, který se používá. Existuje několik způsobů, že aplikace pro Android se může uvolnit uživatelům:

-   **Prostřednictvím webu** &ndash; aplikace pro A Xamarin.Android může být k dispozici ke stažení na webu, ze kterého uživatelů může potom nainstalovat aplikaci kliknutím na odkaz.
-   **Pomocí e-mailu** &ndash; je možné pro uživatele k instalaci aplikace pro Xamarin.Android z e-mailové zprávy. Aplikace bude nainstalována v případě otevření přílohy s zařízení se systémem Android zapnuté.
-   **Prostřednictvím na trhu** &ndash; existuje několik tržišť aplikace, které existují pro distribuci, jako například [Google Play](http://play.google.com/) nebo [Amazon App Store pro systém Android](http://www.amazon.com/mobile-apps/b?ie=UTF8&node=2350149011) .


Pomocí zavedených marketplace je nejběžnější způsob, jak publikovat aplikaci, protože poskytuje nejširší reach trhu a větší kontrolu nad distribuční. Publikování aplikace prostřednictvím na trhu, ale vyžaduje další úsilí.

Více typů komunikačních kanálů můžete distribuovat aplikace pro Xamarin.Android současně. Například aplikace lze publikovat na webu Google Play, Amazon obchodu s aplikacemi pro Android a také stáhnout z webového serveru.

Tyto dvě metody rozšíření (stahování nebo e-mailu) jsou nejvhodnější pro řízené podmnožině uživatelů, jako jsou prostředí rozlehlé sítě nebo aplikace, která je určená jenom pro malé dobře určenou, nebo sadu uživatelů.
Server a distribuce e-mailů jsou také jednodušší publikování modely menší přípravy publikovat aplikaci, které vyžadují.

Program distribuce Amazon mobilní aplikace umožňuje vývojářům mobilní aplikace k distribuci a prodeje svých aplikací na Amazon. Uživatelé zjistit a nakupovat pro aplikace na svých zařízeních používá technologii Android pomocí aplikace Amazon App Store. Snímek obrazovky obchodu s aplikacemi Amazon spuštěné v zařízení se systémem Android se zobrazí níže:

Komplexní a oblíbených marketplace pro aplikace pro Android je pravděpodobně Google Play. Google Play umožňuje uživatelům zjistit, stáhnout, míry a kliknutím na jednom ikonu na svém zařízení nebo na svém počítači platit pro aplikace. Také poskytuje nástroje, které pomáhá s analýzou prodeje a trendů trhu a k řízení zařízení, která Google Play a uživatelé mohou stáhnout aplikaci. Snímek obrazovky Google Play spuštěné v zařízení se systémem Android se zobrazí níže:

[![Snímek obrazovky Google Play](images/google-play-app.png)](images/google-play-app.png#lightbox)

V této části ukazuje, jak nahrát aplikaci do úložiště jako je Google Play, společně s příslušnou propagační materiály. APK rozšíření soubory jsou vysvětleny umožní koncepční přehled o co jsou a jak pracují. Google licencování služby jsou také popsány. Nakonec se zavedly alternativní způsob distribuce, včetně použití webového serveru se službou protokolu HTTP, distribuce jednoduchého e-mailu a obchodu s aplikacemi Amazon pro Android.


## <a name="related-links"></a>Související odkazy

- [HelloWorldPublishing (ukázka)](https://developer.xamarin.com/samples/monodroid/HelloWorldPublishing/)
- [Proces sestavení](~/android/deploy-test/building-apps/build-process.md)
- [Propojování](~/android/deploy-test/linker.md)
- [Získání služby Google mapuje klíč rozhraní API](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Podepisování aplikací](https://source.android.com/security/apksigning/)
- [Publikování na webu Google Play](http://developer.android.com/distribute/googleplay/publish/index.html)
- [Licencování aplikace Google](http://developer.android.com/guide/google/play/licensing/index.html)
- [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)
- [Portál distribuční mobilní aplikace](https://developer.amazon.com/welcome.html)
- [Distribuce mobilních aplikací Amazon – nejčastější dotazy](https://developer.amazon.com/help/faq.html)
