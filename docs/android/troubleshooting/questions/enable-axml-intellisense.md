---
title: "Povolení technologie Intellisense v souborech Android .axml"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: c68ec5a6d369e8673c8764e01943e513489775b0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Povolení technologie Intellisense v souborech Android .axml

Android Intellisense v sadě Visual Studio vyžaduje dva soubory schématu XML fungovat správně. Chcete-li najít tyto soubory, přejděte do této složky:

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (Dříve: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

Uvnitř zjistíte dva soubory XSD:

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio udržuje všechny schémat XML do příslušné složky:

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

Můžete přesunout tyto dva soubory XSD na výše uvedené umístění nebo jednoduše stačí přidat tato schémata v sadě Visual Studio. Poté můžete "Přidat" Tato schémata v sadě Visual Studio prostřednictvím **XML > schémata > Přidat** dialogové okno.






