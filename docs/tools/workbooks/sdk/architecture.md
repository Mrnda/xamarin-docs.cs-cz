---
title: Přehled architektury
description: Tento dokument popisuje architekturu sešity Xamarin, prozkoumání, jak spolupracují interaktivní agenta a interaktivní klienta.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 3cd0d3e8cc47dd7fa43dfa0b910c9f09740fcc9a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794004"
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
