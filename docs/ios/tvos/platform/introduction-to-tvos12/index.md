---
title: Úvod do Tvosu 12
description: Tento dokument obsahuje podrobný přehled o nových a aktualizovaných funkcích tvOS 12 které Xamarin ve verzi preview v současné době poskytuje vazby C#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38847558"
---
# <a name="introduction-to-tvos-12"></a>Úvod do Tvosu 12

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> 12 podporu Xamarinu pro tvOS je aktuálně ve verzi preview, což znamená, že může obsahovat chyb, které není plně funkční, a může změnit. Použijte pouze experimentování.

> [!NOTE]
> - Zkontrolujte [Začínáme](~/ios/platform/introduction-to-ios12/get-started.md) příručka pokyny o tom, abyste mohli začít vytvářet aplikace pro tvOS 12 s využitím kódu Xamarin pro iOS 12 a.
> - Další informace najdete v článku [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) pro Xamarin ve verzi preview verzi.

Tento dokument poskytuje základní přehled o nových a aktualizovaných tvOS 12 funkce verze pro Xamarin, které ve verzi preview v současné době poskytuje vazby C#.

## <a name="password-autofill"></a>Automatické vyplňování hesel

S 12 pro tvOS můžete použít uživatelé svá zařízení s Iosem k přihlášení do aplikace pro tvOS s jediným klepnutím. To je povolená díky kombinaci `UITextContentType` pole informací o využití a zadejte uživatelské jméno a heslo, přidružené domény k vytvoření vztahu mezi aplikace pro iOS a aplikace pro tvOS a upřednostňovaný fokus prostředí a vyberte položku, která má být vybrán poté, co uživatel poskytuje uživatelské jméno a heslo.

## <a name="focus-engine-enhancements"></a>Vylepšení modulu fokus

tvOS 12 umožňuje všechny aplikace, bez ohledu na to, jak jejich vykreslením, pracovat s modulem fokus. Prostřednictvím interakce uživatele s Siri Remote modul fokus je možné s libovolnou aplikací vyberte položku, pomocného parametru na změní možné fokus a přirozeně aktualizovat fokus. Tato možnost je povolena ve vlastních aplikacích prostřednictvím vaší UIKit `IUIFocusItemContainer` rozhraní, `UIFocusMovementHint` třídy, `IUIFocusItemScrollableContainer` rozhraní a související třídy a metody.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – pro vývojáře Apple (Apple)](https://developer.apple.com/tvos/)
- [Co je nového v tvOS 12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin ve verzi preview [zpráva k vydání verze](https://releases.xamarin.com/preview-release-xcode-10-beta/)
