---
title: Úvod do Tvosu 12
description: Tento dokument obsahuje podrobný přehled o nových a aktualizovaných funkcích tvOS 12 které Xamarin ve verzi preview v současné době poskytuje vazby C#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 5cbec23aa81a4637a18f83d9955a78183dadaa21
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615196"
---
# <a name="introduction-to-tvos-12"></a>Úvod do Tvosu 12

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> 12 podporu Xamarinu pro tvOS je aktuálně ve verzi preview, což znamená, že může obsahovat chyb, které není plně funkční, a může změnit. Použijte pouze experimentování.

Tento dokument poskytuje základní přehled o nových a aktualizovaných tvOS 12 funkce verze pro Xamarin, které ve verzi preview v současné době poskytuje vazby C#.

Abyste mohli začít vytvářet aplikace pro tvOS 12 s využitím kódu Xamarin, podívejte se na:

- [– Příručka Začínáme](~/ios/platform/introduction-to-ios12/get-started.md)
- Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)

## <a name="tvuikit"></a>TVUIKit

tvOS 12 zahrnuje TVUIKit sadu rozhraní API, která umožňují vývojářům používat běžné ovládací prvky pro tvOS například plakát zobrazení, popisek tlačítka, zobrazení karet a zobrazení monogram tvOS. tvOS 12 také zavádí vlastnost, která umožňuje popisky posouvat text, který je příliš dlouhý a nemůže být zcela viditelné.

## <a name="password-autofill"></a>Automatické vyplňování hesel

S 12 pro tvOS můžete použít uživatelé svá zařízení s Iosem k přihlášení do aplikace pro tvOS s jediným klepnutím. To je povolená díky kombinaci `UITextContentType` pole informací o využití a zadejte uživatelské jméno a heslo, přidružené domény k vytvoření vztahu mezi aplikace pro iOS a aplikace pro tvOS a upřednostňovaný fokus prostředí a vyberte položku, která má být vybrán poté, co uživatel poskytuje uživatelské jméno a heslo.

## <a name="focus-engine-enhancements"></a>Vylepšení modulu fokus

tvOS 12 umožňuje všechny aplikace, bez ohledu na to, jak jejich vykreslením, pracovat s modulem fokus. Prostřednictvím interakce uživatele s Siri Remote modul fokus je možné s libovolnou aplikací vyberte položku, pomocného parametru na změní možné fokus a přirozeně aktualizovat fokus. Tato možnost je povolena ve vlastních aplikacích prostřednictvím vaší UIKit `IUIFocusItemContainer` rozhraní, `UIFocusMovementHint` třídy, `IUIFocusItemScrollableContainer` rozhraní a související třídy a metody.

## <a name="vision-framework"></a>Rozhraní pro zpracování obrazu

Rozhraní pro zpracování obrazu zahrnuje vylepšené face detectoru, mohou v různých orientace rozpoznávání tváří. Žádosti o revize také, je nyní možné vybrat konkrétní revizi algoritmus framework pro zpracování obrazu.

## <a name="natural-language-framework"></a>Přirozený jazyk rozhraní

Přirozený jazyk rozhraní umožňuje aplikacím provádět různé druhy analýzy jazyka. Například jej lze použít k identifikaci částí řeči a určit jazyk reprezentována blok textu.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – pro vývojáře Apple (Apple)](https://developer.apple.com/tvos/)
- [Co je nového v tvOS 12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin ve verzi preview [release blogový příspěvek](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
