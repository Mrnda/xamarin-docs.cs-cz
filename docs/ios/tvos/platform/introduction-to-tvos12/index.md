---
title: Úvod do tvOS 12
description: Tento dokument poskytuje vysokou úroveň přehled nové a aktualizované funkce v tvOS 12 pro verzi preview Xamarin, který aktuálně poskytuje vazby C#.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067160"
---
# <a name="introduction-to-tvos-12"></a>Úvod do tvOS 12

![Náhled](~/media/shared/preview.png)

> [!WARNING]
> 12 podpora pro Xamarin pro tvOS je aktuálně ve verzi preview, což znamená, že farma může obsahovat chyb, není funkce dokončení, a může se změnit. Použijte pouze experimenty.

> [!NOTE]
> - Zkontrolujte [Začínáme](~/ios/platform/introduction-to-ios12/get-started.md) příručka pokyny o tom, jak začít vytváření iOS 12 a tvOS 12 aplikace s funkcí Xamarin.
> - Další informace najdete v tématu [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/) pro Xamarin náhled verze.

Tento dokument obsahuje souhrnné informace o nových a aktualizovaných tvOS 12 funkce, pro které Xamarin preview verze aktuálně poskytuje vazby C#.

## <a name="password-autofill"></a>Heslo automatické vyplňování

TvOS 12 uživatele pomocí jejich zařízení s iOS k přihlášení do aplikace pro tvOS jedním klepnutím. Tato možnost je povolena pomocí kombinace `UITextContentType` polí využití, a zadejte uživatelské jméno a heslo domény přidružené k vytvoření vztahu mezi aplikaci pro iOS a tvOS aplikace a upřednostňované fokus prostředí a vyberte položku, kterou chcete získat fokus poté, co uživatel poskytuje uživatelské jméno a heslo.

## <a name="focus-engine-enhancements"></a>Vylepšení fokusu modulu

tvOS 12 umožňuje všechny aplikace, bez ohledu na to, jak budou zpracovány, pracovat s modulem fokus. Prostřednictvím uživatelské interakce se vzdálenými Siri modul fokus použít s libovolnou aplikací vyberte položku, pomocného parametru v změní možné fokus a přirozeně aktualizovat fokus. Tato možnost je povolena ve vlastních aplikacích prostřednictvím na UIKit `IUIFocusItemContainer` rozhraní, `UIFocusMovementHint` třídy, `IUIFocusItemScrollableContainer` rozhraní a další související třídy a metody.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – vývojáře Apple (Apple)](https://developer.apple.com/tvos/)
- [Co je nového v tvOS 12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin preview [poznámky k verzi](https://releases.xamarin.com/preview-release-xcode-10-beta/)
