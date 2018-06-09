---
title: Xamarin.Forms DataPages
description: Tento článek představuje Xamarin.Forms DataPages, která poskytují rozhraní API pro rychle a snadno svázat zdroj dat se předdefinovaných zobrazení.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243327"
---
# <a name="xamarinforms-datapages"></a>Xamarin.Forms DataPages

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!IMPORTANT]
> Vyžaduje DataPages [Xamarin.Forms motiv](~/xamarin-forms/user-interface/themes/index.md) odkaz k vykreslení.

Xamarin.Forms DataPages byly oznamují na měnícím 2016 a jsou dostupné jako verze preview pro zákazníky a zkuste svůj názor.

DataPages poskytují rozhraní API snadno a rychle vytvořit vazbu zdroj dat na předdefinovaných zobrazení. Položky seznamu a podrobností stránky automaticky vykreslí data a lze přizpůsobit pomocí motivů.

Pokud chcete zobrazit, jak funguje měnícím //Build ukázka klíčových prvků, podívejte se [Příručka Začínáme](get-started.md).

[![](images/demo-sml.png "DataPages ukázkovou aplikaci")](images/demo.png#lightbox "DataPages ukázkové aplikace")

## <a name="introduction"></a>Úvod

Zdroje dat a přidružená data stránky umožňují vývojářům snadno a rychle využívat na podporovaný zdroj dat a vykreslit ho integrované, které uživatelského rozhraní generování uživatelského rozhraní, které lze přizpůsobit pomocí motivů.

DataPages se přidají k aplikaci Xamarin.Forms zahrnutím **Xamarin.Forms.Pages** balíček Nuget.

### <a name="data-sources"></a>Zdroje dat

Verze Preview má k dispozici pro použití předem datového zdroje:

* **JsonDataSource**
* **AzureDataSource** (jednotlivé Nuget)
* **AzureEasyTableDataSource** (jednotlivé Nuget)

Najdete v článku [Příručka Začínáme](get-started.md) pro příklad použití `JsonDataSource`.


### <a name="pages--controls"></a>Stránky a ovládací prvky

Umožňuje snadno vazby ke zdroji dat zadané zahrnují se tyto stránek a ovládacích prvků:

* **ListDataPage** – najdete v článku [Začínáme příklad](get-started.md).
* **DirectoryPage** – seznam s seskupení povolena.
* **PersonDetailPage** – jednu datovou položku zobrazení přizpůsobit pro konkrétní typy objektů (položku kontaktu).
* **Zobrazení dat** – zobrazení vystavit data ze zdroje obecné způsobem.
* **Zobrazení karty aplikace** – ve zobrazení, který obsahuje bitovou kopii, nadpis a text popisu.
* **HeroImage** – zobrazení o vykreslování obrázků.
* **ListItem** – předem vytvořené zobrazení s rozložením podobné nativní aplikace pro iOS a Android seznam položek.

Najdete v článku [DataPages ovládací prvky odkaz](controls.md) příklady.



### <a name="under-the-hood"></a>Pod pokličkou

Zdroj dat Xamarin.Forms dodržuje `IDataSource` rozhraní.

Infrastruktury Xamarin.Forms komunikuje se zdrojem dat prostřednictvím následujících vlastností:

* `Data` – seznam datových položek, které mohou být zobrazeny jen pro čtení.
* `IsLoading` – logickou hodnotu udávající, zda je data načíst a k dispozici pro vykreslení.
* `[key]` – indexer načíst elementy.

Existují dvě metody `MaskKey` a `UnmaskKey` , můžete použít (zobrazit nebo skrýt) vlastnosti datové položky (tj. zabránit jejich vykreslení).
Klíč odpovídá vlastnost s názvem na datový objekt položky.
