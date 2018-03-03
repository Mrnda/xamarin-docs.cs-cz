---
title: "Vzory podnikové aplikace pomocí elektronická kniha Xamarin.Forms"
description: "Architektury pokyny pro vývoj aplikací enterprise Xamarin.Forms přizpůsobitelné, udržovatelného a možností intenzivního testování"
ms.topic: article
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 7ed546ac975ce1956d94d509486e4cfb25d28100
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Vzory podnikové aplikace pomocí elektronická kniha Xamarin.Forms

_Architektury pokyny pro vývoj aplikací enterprise Xamarin.Forms přizpůsobitelné, udržovatelného a možností intenzivního testování_

![](images/cover-sml.png "Vzory podnikové aplikace pomocí elektronická kniha Xamarin.Forms")

Tato elektronická kniha obsahuje pokyny o tom, jak implementovat Model-View-ViewModel (modelem MVVM) vzor, vkládání závislostí, navigace, ověření a správa konfigurace, při zachování volné párování. Kromě toho je také pokyny na provádění ověřování a autorizace s IdentityServer, přístup k datům z kontejnerizované mikroslužeb a testování částí.

## <a name="prefaceprefacemd"></a>[Předmluva](preface.md)

Tato kapitola vysvětluje účel a rozsah v průvodci a který je zaměřen na.

## <a name="introductionintroductionmd"></a>[Úvod](introduction.md)

Vývojáři aplikací enterprise čelí několik výzvy, které můžete změnit architektuře aplikace během vývoje. Proto je důležité k sestavení aplikace, abyste mohli upravit nebo rozšířené v čase. Návrh pro takové přizpůsobivost může být složité, ale obvykle zahrnuje dělení aplikace do diskrétní, volně párované součásti, které lze snadno integrovat společně do aplikace.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Vzor Model-View-ViewModel (modelem MVVM) pomáhá řádně jednotlivé obchodní a prezentace logiku aplikace z jeho uživatelské rozhraní (UI). Zachování čistou oddělení mezi aplikační logiku a uživatelské rozhraní pomáhá řešit potíže se množství vývoj a můžete usnadnit aplikace testování, údržbu a momentální. To může také výrazně zlepšit příležitosti opětovné použití kódu a umožňuje vývojářům a Návrháři UI více snadno spolupracovat při vývoji jejich odpovídajících částí aplikace.

## <a name="dependency-injectiondependency-injectionmd"></a>[Injektáž závislostí](dependency-injection.md)

Vkládání závislostí umožňuje oddělení konkrétní typy z kód, který závisí na těchto typů. Obvykle používá kontejner, který obsahuje seznam registrace a mapování mezi rozhraní a abstraktní typy a konkrétní typy, které implementují nebo rozšířit tyto typy.

Kontejnery vkládání závislostí snížit párování mezi objekty tím, že poskytuje budovy doložit instancí tříd a spravovat své životnosti, v závislosti na konfiguraci kontejneru. Při vytváření objektů kontejneru vloží všechny závislosti, které vyžaduje objekt do ní. Pokud tyto závislosti ještě nevytvořili, kontejner vytvoří a přeloží nejprve závislé.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Komunikace mezi volně vázanými komponentami](communicating-between-loosely-coupled-components.md)

Platformě Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třída implementuje publikování-přihlášení k odběru vzor, umožňuje na základě zpráv komunikace mezi součástmi, které jsou praktické, že lze propojit pomocí objektu a typu odkazy. Tento mechanismus umožňuje vydavatele a odběratele komunikovat bez nutnosti odkaz na sebe, pomáhá snížit závislosti mezi součástmi, a zároveň součástí nezávisle vývoji a testování.

## <a name="navigationnavigationmd"></a>[Navigace](navigation.md)

Xamarin.Forms zahrnuje podporu pro navigaci na stránce, které obvykle výsledky z interakce uživatele s uživatelským rozhraním, nebo z aplikace, v důsledku vnitřní stav řízené logiku změny. Navigace však může být složité implementace v aplikacích, které používají rozhraní MVVM vzorec.

Tato kapitola uvede `NavigationService` třídy, která se používá k provádění zobrazení modelu první navigační z modelů zobrazení. Umístění logiku navigace v zobrazení třídy modelu znamená, že logiku může uplatnit prostřednictvím automatizovaných testů. Kromě toho model zobrazení poté můžete implementovat logiku pro ovládací prvek navigace k zajištění, že některé obchodní pravidla jsou vyžadována.

## <a name="validationvalidationmd"></a>[Ověřování](validation.md)

Jakékoli aplikaci, která přijímá vstup od uživatele zkontrolujte, že vstup je neplatný. Bez ověřování může uživatel zadat data, která způsobila selhání aplikace. Ověření vynucuje obchodní pravidla a zabraňuje útočníkovi vložení škodlivá data.

V kontextu systému Model ViewModel Model (modelem MVVM) vzor, zobrazení model nebo model bude často nutné provést ověření dat a signál všechny chyby ověření do zobrazení, takže uživatel může opravte je.

## <a name="configuration-managementconfiguration-managementmd"></a>[Správa konfigurace](configuration-management.md)

Nastavení umožňují oddělení dat, které konfiguruje chování nástroje aplikace z kódu, což chování změnit bez opětovného aplikace. Nastavení aplikace jsou data, která aplikace vytváří a spravuje a nastavení uživatele jsou přizpůsobitelné nastavení aplikace, která ovlivňují chování aplikace a nevyžadují časté opakované úpravu.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Kontejnerizované mikroslužby](containerized-microservices.md)

Mikroslužeb nabízí přístup k vývoji aplikací a nasazení, který je vhodný flexibility, měřítko a spolehlivost požadavkům moderní cloudové aplikace. Jedním z hlavní výhody mikroslužeb je, aby mohly být upraveným nezávisle, což znamená, že konkrétní funkční oblast je možné rozšířit vyžadující další zpracování napájení nebo síťové šířky pásma pro podporu poptávky, bez zbytečně škálování oblastí aplikace, která k těmto problémům zvýšené poptávky.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Ověřování a autorizace](authentication-and-authorization.md)

Existuje mnoho přístupů k integraci ověřování a autorizace do aplikace na platformě Xamarin.Forms, která komunikuje s webovou aplikaci ASP.NET MVC. Zde ověřování a autorizace se provádí s mikroslužbu kontejnerizované identity, která používá IdentityServer 4. IdentityServer je otevřeným zdrojem OpenID Connect a OAuth 2.0 framework pro ASP.NET Core, který se integruje s ASP.NET Identity Core k provedení ověřování tokenu nosiče.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Přístup ke vzdáleným datům](accessing-remote-data.md)

Ujistěte se, mnohá moderní řešení založené na webu pomocí webových služeb, hostované webové servery, nabízí funkce pro vzdálené klientské aplikace. Operace, které zpřístupní webové služby tvoří webového rozhraní API a klientské aplikace by měla umět využívají webové rozhraní API bez znalosti, jak jsou implementované dat nebo operace, které zpřístupňuje rozhraní API.

## <a name="unit-testingunit-testingmd"></a>[Testování částí](unit-testing.md)

Testování modely a Zobrazit modely z rozhraní MVVM aplikací je stejný jako při testování jiné třídy a můžete použít stejné nástroje a techniky. Existují však některé vzorů, které jsou typické modelu a třídy modelu zobrazení, které můžete využít techniky testování konkrétní jednotka.

## <a name="feedback"></a>Zpětná vazba

Tento projekt má web komunity, ve kterém můžete Ptejte se a poskytnout zpětnou vazbu. Komunitní web umístěn na [Githubu](https://github.com/dotnet-architecture/eShopOnContainers). Alternativně může být názor elektronická kniha vám poslali na [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
