---
title: Jak se můžu podívat na které knihovny se podporují v PCL?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.date: 07/27/2018
ms.openlocfilehash: 7e1017baf7daed68b5e55319a9ce13a4a2df5f2e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351467"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Jak se můžu podívat na které knihovny se podporují v PCL?

- Přehled různých funkcí podporovaných různým PCL cílových platforem v části můžete najít *podporované funkce* části této stránky: [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- Další možností je použít [.NET Portability Analyzeru](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) k vyhodnocení, zda vaše stávající knihovny lze převést na profilem PCL.

- Třetí možností je procházet obsah skutečné profil, který můžete použít. Například pomocí profil 78, můžete přejít sem: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` a zobrazit všechna sestavení v rámci něj.

Podle toho, která metoda jste zvolili, prosím Všimněte si, že některé funkce má ke stažení přes NuGet a knihovnu Microsoft BCL.
