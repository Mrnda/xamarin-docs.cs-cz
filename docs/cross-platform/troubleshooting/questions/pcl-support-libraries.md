---
title: "Jak lze zobrazit v PCL jsou podporované jaké knihovny?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: e05b15f0a9af7666d635ad375b6c401e95a44525
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Jak lze zobrazit v PCL jsou podporované jaké knihovny?

- Přehled různých funkcí podporovaných různé cílové platformy PCL pod můžete najít *podporované funkce* části této stránky: [http://msdn.microsoft.com/en-us/library/gg597391.aspx](https://msdn.microsoft.com/en-us/library/gg597391.aspx)

- Další možností je použít [.NET přenositelnost analyzátor](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) k vyhodnocení, zda existující knihovnu lze převést na PCL profilu.

- Třetí možnost je procházet obsah skutečné profil, který můžete použít. Například pomocí profilu 78, můžete přejít zde: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` a zobrazit všechna sestavení v něm.

Podle toho, která metoda jste zvolili, prosím Všimněte si, že některé funkce musí být staženy přes NuGet a knihovna Microsoft BCL.
