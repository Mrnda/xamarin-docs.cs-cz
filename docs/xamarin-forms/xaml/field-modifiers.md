---
title: Modifikátory polí XAML v Xamarin.Forms
description: 'X: fieldmodifier – obor názvů atribut určuje úroveň přístupu pro generované pole s názvem elementů XAML.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209490"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Modifikátory polí XAML v Xamarin.Forms

_`x:FieldModifier` Atribut namespace určuje úroveň přístupu pro generované pole s názvem elementů XAML._

## <a name="overview"></a>Přehled

Platné hodnoty atributu, jsou:

- `Public` – Určuje, že je generované pole pro daný element XAML `public`.
- `NotPublic` – Určuje, že je generované pole pro daný element XAML `internal` sestavení.

Pokud není nastavena hodnota atributu, bude generované pole pro daný element `private`.

Musí být splněné následující podmínky `x:FieldModifier` atribut na zpracování:

- Element nejvyšší úrovně XAML musí být platná `x:Class`.
- Má aktuálního elementu XAML `x:Name` zadaný.

Následující XAML uvádí příklady nastavení atributu:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier` Nelze atribut použít k určení úrovně přístupu třídy XAML.
