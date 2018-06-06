---
title: Aktualizace aplikace pro iOS 11
description: Tento dokument obsahuje odkazy na různé příručky, které popisují nové funkce, které jsou k dispozici pro Xamarin.iOS vývojářům verzi iOS 11. Například aktualizace visual návrhu, App Store změny, a aktualizace ikona aplikace.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: ea57acbd6165f7b1abd8b9bd69873670c179f411
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787569"
---
# <a name="updating-your-app-to-ios-11"></a>Aktualizace aplikace pro iOS 11

V iOS 11 má společnost Apple vydala architektura aktualizací, nové visual změny a o proces připojení aktualizované iTunes. Tato příručka prozkoumá, každý z těchto změn, což pomáhá vaší aplikace Xamarin.iOS aktualizovat pro iOS 11.

## <a name="architecture-changesarchitecture-changesmd"></a>[Změny architektury](architecture-changes.md)

Jednou z největších změn, které byste měli vědět o s iOS 11 je 32-bit podpory pro aplikace, podle popisu v [společnosti Apple](https://developer.apple.com/news/?id=06282017b) stiskněte verze.

Tento průvodce vás provede aktualizace pro 64bitovou verzi aplikace.

## <a name="visual-design-updatesvisual-designmd"></a>[Aktualizace vizuálního návrhu](visual-design.md)

Společnost Apple vydala v iOS 11, nastavení nové visual změny, včetně aktualizací na navigačním panelu, panelu vyhledávání a zobrazení tabulek. Kromě toho vylepšily k povolení pro větší flexibilitu v okraje a obsah celé obrazovky. Tyto změny jsou popsané v této příručce.

## <a name="app-store-changesapp-store-changesmd"></a>[Změny obchodu App Store](app-store-changes.md)

IOS App Store zaznamenala dokončení změna, která pouze umožňuje uživatelům efektivně přejděte úložiště, ale můžete taky, jako vývojář, zvýšení úrovně vaší aplikaci pro uživatele. Tyto úrovně celého čísla zahrnují aktualizace nákupy v aplikaci a aktualizace stránky produktu. iOS 11 také přidá aktualizace týkající se ke komunikaci s uživateli, jak přidat ikonu vaší aplikace a postup verze vaší aplikace na veřejnost.

## <a name="app-icon-updates"></a>Ikona aplikace aktualizace

> [!NOTE]
> Ikony aplikace by měl nyní přijímá _katalog Asset_. 

Informace o používání asset katalogů najdete v části [App Store ikonu](~/ios/app-fundamentals/images-icons/app-store-icon.md) průvodce. Nápovědu k migraci najdete v části ikon z Info.plist do katalog Asset [migrace z Info.plist Asset katalogů](~/ios/app-fundamentals/images-icons/app-icons.md) průvodce.

Název požadované ikony v katalogu Asset **obchod** a musí být 1 024 x 1 024 velikost. Apple uvedli, že ikonu úložiště pro aplikace v katalogu asset nemůže být průhledná ani obsahovat kanálu alfa.

![Aplikace ukládá umístění ikony v katalogu asset catalog.](images/image1.png)

## <a name="related-links"></a>Související odkazy

- [Co je nového v iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Stránka produktu aktualizované obchodu s aplikacemi (Apple)](https://developer.apple.com/app-store/product-page/)
- [Aktualizace aplikace pro iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
