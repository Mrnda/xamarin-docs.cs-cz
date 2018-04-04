---
title: Data a cloudové služby
description: Ustálení a příruček nasazení
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 81aebe5fd7431e578b75c5b61e1d2c92ce546909
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="data-and-cloud-services"></a>Data a cloudové služby


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Přístup k datům](~/ios/data-cloud/data/index.md)

Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi. iOS má databázový stroj Sqlite "součástí" a přístup k datům se zjednodušilo díky Xamarin na platformě, která se dodává s zprostředkovatele dat SQLite.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Úložiště iCloud rozhraní API v iOS 5 umožňuje aplikacím ukládat dokumenty uživatele a specifická data do centrálního umístění a přístup k těmto položkám ze zařízení, všechny uživatele.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Rozhraní framework CloudKit zjednodušuje vývoj aplikací, že přístup k serveru služby iCloud. To zahrnuje načtení data aplikací a asset práva a také schopnost bezpečně uložit informace o aplikaci. Tato sada poskytuje uživatelům vrstvu anonymity tím, že přístup k aplikacím s jejich Icloudu ID bez sdílení osobní údaje.