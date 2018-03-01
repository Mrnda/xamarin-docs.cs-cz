---
title: "Data a cloudové služby"
description: "Ustálení a příruček nasazení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 5814936289164f14a07b33c219ad1fd01b00473b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="data-and-cloud-services"></a>Data a cloudové služby


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Přístup k datům](~/ios/data-cloud/data/index.md)

Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi. iOS má databázový stroj Sqlite "součástí" a přístup k datům se zjednodušilo díky Xamarin na platformě, která se dodává s zprostředkovatele dat SQLite.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Úložiště iCloud rozhraní API v iOS 5 umožňuje aplikacím ukládat dokumenty uživatele a specifická data do centrálního umístění a přístup k těmto položkám ze zařízení, všechny uživatele.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Rozhraní framework CloudKit zjednodušuje vývoj aplikací, že přístup k serveru služby iCloud. To zahrnuje načtení data aplikací a asset práva a také schopnost bezpečně uložit informace o aplikaci. Tato sada poskytuje uživatelům vrstvu anonymity tím, že přístup k aplikacím s jejich Icloudu ID bez sdílení osobní údaje.