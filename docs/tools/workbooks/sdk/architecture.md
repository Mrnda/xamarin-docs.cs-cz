---
title: Přehled architektury
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 04ed545955b10ed95a15ac0820e4a99b697e0c3e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="architecture-overview"></a>Přehled architektury

Sešity Xamarin funkce dvě hlavní součásti, které musí fungování ve spojení se vzájemně: _agenta_ a _klienta_.

## <a name="interactive-agent"></a>Interaktivní agenta

Součást agenta je malý sestavení specifických pro platformy, které běží v kontextu aplikace .NET.

Sešity Xamarin poskytuje předem sestavených aplikací "prázdný" pro několik platforem, jako je iOS, Android, Mac a WPF. Tyto aplikace explicitně Hostitel agenta.

Během kontroly za provozu (Xamarin Inspector) je agent vložit pomocí ladicího programu IDE do existující aplikace jako součást regulární vývoj & ladění pracovního postupu.

## <a name="interactive-client"></a>Interaktivní klienta

Klient je nativní prostředí (kakao v systému Mac, grafického subsystému WPF v systému Windows), který je hostitelem povrch webového prohlížeče pro prezentování rozhraní sešitu nebo REPL. Z hlediska služby SDK jsou implementované všechny integrace klienta v JavaScript a CSS.

Klient zodpovídá (prostřednictvím Roslyn) pro kompilování zdrojového kódu do malých sestavení a jejich odesláním prostřednictvím připojeného agenta pro provedení. Výsledky provádění se odesílají zpět do klienta pro vykreslování. Jednotlivých buněk sešitu vypočítá jeden sestavení, které odkazuje na sestavení předchozí buňku.

Vzhledem k tomu, že agent může být spuštěná na jakýkoli typ platformy .NET a má přístup k ničemu v běžící aplikaci, musí dbát na to, k serializaci výsledky způsobem, bez ohledu na platformu.