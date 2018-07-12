---
title: Xamarin.Forms DataPages
description: Tento článek představuje Xamarin.Forms DataPages, které poskytuje rozhraní API pro rychle a snadno svázat zdroj dat se předdefinovaných zobrazení.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815884"
---
# <a name="xamarinforms-datapages"></a>Xamarin.Forms DataPages

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!IMPORTANT]
> Vyžaduje DataPages [Xamarin.Forms motiv](~/xamarin-forms/user-interface/themes/index.md) odkaz k vykreslení.

Xamarin.Forms DataPages byly bylo ohlášená na Evolve 2016 a jsou k dispozici jako verze preview pro zákazníky, kteří si vyzkoušet a poskytnout zpětnou vazbu.

DataPages poskytují rozhraní API rychle a snadno svázat zdroj dat do předdefinovaných zobrazení. Položky seznamu na stránce podrobností se automaticky generují data a je možné přizpůsobit pomocí motivů.

Podívejte se, jak Evolve //Build ukázka funguje, přečtěte si [– Příručka Začínáme](get-started.md).

[![](images/demo-sml.png "DataPages ukázkovou aplikaci")](images/demo.png#lightbox "DataPages ukázkové aplikace")

## <a name="introduction"></a>Úvod

Zdroje dat a na stránkách přidružená data umožňují vývojářům snadno a rychle používat podporovaný zdroj dat a vykreslit ho pomocí uživatelského rozhraní, generování uživatelského rozhraní, které se dají přizpůsobit integrovanou s motivy.

DataPages jsou přidány do aplikace Xamarin.Forms zahrnutím **Xamarin.Forms.Pages** balíček Nuget.

### <a name="data-sources"></a>Zdroje dat

Verze Preview má některé zdroje dat s využitím předem připravených k dispozici pro použití:

* **JsonDataSource**
* **AzureDataSource** (samostatný Nuget)
* **AzureEasyTableDataSource** (samostatný Nuget)

Zobrazit [– Příručka Začínáme](get-started.md) příklad použití `JsonDataSource`.


### <a name="pages--controls"></a>Stránky a ovládací prvky

Následující stránky a ovládací prvky jsou zahrnuty umožňuje snadné vytvoření vazby na zadané datové zdroje:

* **ListDataPage** – najdete v článku [Začínáme příklad](get-started.md).
* **DirectoryPage** – seznam s seskupení povolena.
* **PersonDetailPage** – jednoho datového bodu zobrazení přizpůsobená pro konkrétní typy objektů (položku kontaktu).
* **Zobrazení dat** – zobrazení ke zveřejňování dat ze zdroje obecně.
* **CardView** – styl zobrazení, která obsahuje image, nadpis a popis.
* **HeroImage** – zobrazení o vykreslování obrázků.
* **ListItem** – předem připravené zobrazení s rozložením podobně jako nativní aplikace pro iOS a Android seznam položek.

Zobrazit [DataPages řídí odkaz](controls.md) příklady.



### <a name="under-the-hood"></a>Pohled pod kapotu

Zdroj dat Xamarin.Forms dodržuje `IDataSource` rozhraní.

Infrastruktura Xamarin.Forms komunikuje se zdrojem dat prostřednictvím následující vlastnosti:

* `Data` – seznam datových položek, které lze zobrazit jen pro čtení.
* `IsLoading` – Logická hodnota označující, zda je data načtena a k dispozici pro vykreslení.
* `[key]` – indexer načíst prvky.

Existují dvě metody `MaskKey` a `UnmaskKey` , který slouží k (zobrazení nebo skrytí) (tj vlastnosti položky dat zabránit jejich vykreslení).
Klíč odpovídá pojmenovanou vlastnost na objekt datové položky.
