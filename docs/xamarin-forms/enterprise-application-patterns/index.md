---
title: Modely podnikové aplikací pomocí Xamarin.Forms e-knihy
description: Tato elektronická kniha poskytuje informace o architektuře pro vývoj přizpůsobitelných, udržovatelných a testovatelných podnikových aplikací Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ecfe99f66e16eafabc3117036ff065e3a35259c3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994345"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Modely podnikové aplikací pomocí Xamarin.Forms e-knihy

_Doprovodné materiály k architektuře pro vývoj přizpůsobitelných, udržovatelných a testovatelných podnikových aplikací Xamarin.Forms_

![](images/cover-sml.png "Modely podnikové aplikací pomocí Xamarin.Forms e-knihy")

Tato elektronická kniha poskytuje pokyny o tom, jak implementovat vzor Model-View-ViewModel (MVVM), injektáž závislostí, navigace, ověřování a správě konfigurace, při zachování volné párování. Kromě toho je také pokyny na ověřování a autorizace s IdentityServer, přístup k datům z kontejnerizované mikroslužby a testování částí.

## <a name="prefaceprefacemd"></a>[Předmluva](preface.md)

Tato kapitola popisuje účel a rozsah v průvodci a který je zaměřen na.

## <a name="introductionintroductionmd"></a>[Úvod](introduction.md)

Vývojáři podnikových aplikací pro rozpoznávání tváře několik problémů, které promění architektury aplikace během vývoje. Proto je potřeba vytvořit aplikaci tak, aby ho upravit nebo rozšířit v čase. Navrhování pro tato přizpůsobivost může být obtížné, ale obvykle zahrnuje dělení aplikaci do samostatné, volně komponenty, které je možné snadno integrovat společně do aplikace.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Vzor Model-View-ViewModel (MVVM) pomáhá čistě rozdělte logiku obchodního a prezentační aplikace z uživatelského rozhraní (UI). Udržovat čisté oddělení mezi aplikace logiky a uživatelského rozhraní pomáhá řešit řadu problémy při vývoji a mohou usnadnit aplikaci otestovat, udržovat a vyvíjejí. Můžete také výrazně zlepšit příležitosti opakované použití kódu a umožňuje vývojářům a návrhářům uživatelských rozhraní pro další snadnou spolupráci při vývoji svých příslušné části aplikace.

## <a name="dependency-injectiondependency-injectionmd"></a>[Injektáž závislostí](dependency-injection.md)

Injektáž závislostí umožňuje oddělení konkrétní typy z kódu, který závisí na těchto typů. Obvykle používá, kontejner, který obsahuje seznam registrací a mapování mezi rozhraní a abstraktní typy a konkrétní typy, které implementují nebo rozšířit těchto typů.

Kontejnery injektáž závislostí snížit párování mezi objekty tím, že poskytuje zařízení pro vytvoření instancí třídy a spravovat jejich životního cyklu, v závislosti na konfiguraci kontejneru. Při vytváření objektů kontejneru vkládá všechny závislosti, které vyžaduje objekt do něj. Pokud tyto závislosti ještě nevytvořili, kontejner vytvoří a nejprve řeší jejich závislosti.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Komunikace mezi volně vázanými komponentami](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třída implementuje publikování – vzorec, což založenou na zprávách komunikaci mezi součástmi, které je obtížné propojit pomocí odkazů na objekt a typ odběru. Tento mechanismus umožňuje vydavatelé a odběratelé komunikovat bez nutnosti odkaz k sobě navzájem, což snižuje závislosti mezi komponentami, a zároveň součásti nezávisle na sobě vývoj a testování.

## <a name="navigationnavigationmd"></a>[Navigace](navigation.md)

Xamarin.Forms zahrnuje podporu pro navigaci na stránce, které obvykle výsledky z interakce uživatele s uživatelským rozhraním, nebo z vlastní aplikaci a v důsledku změn ve vnitřní stav řízené logiku. Navigace však může být složité implementace v aplikacích, které používají vzor MVVM.

Tato kapitola uvede `NavigationService` třídu, která se používá k provedení zobrazení první model navigace z modelů zobrazení. Umístění logiky navigace v zobrazení tříd modelu znamená, že logiku lze uplatnit prostřednictvím automatizovaných testů. Navíc model zobrazení pak mohou implementovat logiku pro ovládací prvek navigace k zajištění, že se vynucují určité obchodní pravidla.

## <a name="validationvalidationmd"></a>[Ověřování](validation.md)

Jakékoli aplikaci, která přijímá vstup od uživatelů by měly zajistit, že je vstup platný. Bez ověřování může uživatel zadat data, která způsobí, že aplikace selhala. Ověření vynucuje obchodní pravidla a zabrání útočníkovi ve vkládání škodlivá data.

V rámci Model ViewModel Model (MVVM) vzor, model zobrazení nebo model bude často nutné provést ověření dat a signalizuje, že všechny chyby ověření do zobrazení tak, aby uživatel opravit.

## <a name="configuration-managementconfiguration-managementmd"></a>[Správa konfigurace](configuration-management.md)

Nastavení umožňují oddělení dat, které konfiguruje chování nástroje aplikace od kódu, který umožňuje chování změnit bez opětovné sestavení aplikace. Nastavení aplikace jsou data, která vytvoří a postará se aplikace a nastavení uživatele upravitelných nastavení aplikace, které ovlivňují chování aplikace a nevyžadují časté úpravy znovu.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Kontejnerizované mikroslužby](containerized-microservices.md)

Mikroslužby poskytují přístup k vývoji aplikací a nasazení, která je vhodná k pružnosti, škálovatelnosti a spolehlivosti požadavky na moderní cloudové aplikace. Jedním z hlavních výhod mikroslužeb je, že je možné horizontálním navýšením kapacity nezávisle na sobě, což znamená, že určitou funkční oblast je možné škálovat, který vyžaduje další zpracování napájení nebo sítě šířky pásma pro podporu poptávky, bez zbytečně škálování oblastí aplikace, která nejde zvýšené poptávky.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Ověřování a autorizace](authentication-and-authorization.md)

Existuje celá řada přístupů k integraci ověřování a autorizace v aplikaci Xamarin.Forms, která komunikuje s webovou aplikaci ASP.NET MVC. Tady ověřování a autorizace jsou prováděny s identity kontejnerizované mikroslužby, který používá IdentityServer 4. IdentityServer je open source platforma OpenID Connect a OAuth 2.0 pro ASP.NET Core, která se integruje s ASP.NET Core Identity provádět ověřování tokenu nosiče.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Přístup ke vzdáleným datům](accessing-remote-data.md)

Ujistěte se, mnohá moderní řešení založeného na webu využívání webových služeb hostovaných webových serverů k zajištění funkce pro vzdálené klientské aplikace. Operace, které zveřejňuje webová služba tvoří webového rozhraní API a klientské aplikace by měl být schopen využívají webové rozhraní API bez znalosti, jak se implementují dat nebo operace, které zveřejňuje rozhraní API.

## <a name="unit-testingunit-testingmd"></a>[Testování částí](unit-testing.md)

Testování modely a Zobrazit modely z MVVM aplikací je totožné s testováním jiné třídy a můžou používat stejné nástroje a techniky. Existují však některé vzory, které jsou běžně k modelu a tříd modelu zobrazení, které můžete využít techniky testování konkrétní jednotka.

## <a name="feedback"></a>Zpětná vazba

Tento projekt obsahuje web komunity, ve kterém můžete posílat otázky a poskytovat zpětnou vazbu. Web komunity se nachází na [Githubu](https://github.com/dotnet-architecture/eShopOnContainers). Můžete také zpětnou vazbu o e-kniha může být e-mailem [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
