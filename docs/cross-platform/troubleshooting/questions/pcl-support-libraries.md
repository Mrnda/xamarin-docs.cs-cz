---
title: Jak lze zobrazit v PCL jsou podporované jaké knihovny?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 7875fc47b1caac025488b8b71bdbd909844e7823
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Jak lze zobrazit v PCL jsou podporované jaké knihovny?

- Přehled různých funkcí podporovaných různé cílové platformy PCL pod můžete najít *podporované funkce* části této stránky: [http://msdn.microsoft.com/library/gg597391.aspx](https://msdn.microsoft.com/library/gg597391.aspx)

- Další možností je použít [.NET přenositelnost analyzátor](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) k vyhodnocení, zda existující knihovnu lze převést na PCL profilu.

- Třetí možnost je procházet obsah skutečné profil, který můžete použít. Například pomocí profilu 78, můžete přejít zde: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` a zobrazit všechna sestavení v něm.

Podle toho, která metoda jste zvolili, prosím Všimněte si, že některé funkce musí být staženy přes NuGet a knihovna Microsoft BCL.
