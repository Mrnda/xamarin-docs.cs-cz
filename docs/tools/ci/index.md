---
title: Úvod do průběžnou integraci s funkcí Xamarin
description: Průběžnou integraci je software inženýrství postupem, ve kterém automatizované sestavení zkompiluje a volitelně otestuje aplikace při přidání nebo změně vývojáři v úložišti řízení projektu verze kódu. Tento článek popisuje jsou obecné koncepty průběžnou integraci a některé z možností k dispozici pro nepřetržitou integraci s projekty Xamarin.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/04/2017
ms.openlocfilehash: b5bccfa38a9f382789585284765183efa42b6a3d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Úvod do průběžnou integraci s funkcí Xamarin

_Průběžnou integraci je software inženýrství postupem, ve kterém automatizované sestavení zkompiluje a volitelně otestuje aplikace při přidání nebo změně vývojáři v úložišti řízení projektu verze kódu. Tento článek popisuje jsou obecné koncepty průběžnou integraci a některé z možností k dispozici pro nepřetržitou integraci s projekty Xamarin._

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]


##  <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Úvod do průběžnou integraci](~/tools/ci/intro-to-ci.md)

Tato část obsahuje různé součásti, které jsou spojené s průběžnou integraci a jejich vztahů. Popisuje ho průběžnou integraci prostředí, které jsou popsané v následujících částech konkrétní.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="working-with-continuous-integration-environments"></a>Práce s průběžnou integraci prostředí


### <a name="using-app-center-build-with-xamarinappcenterbuildxamarin"></a>[Používání sestavení App Center se Xamarinem](/appcenter/build/xamarin/)

Sestavení řešení pro Xamarin.iOS a Xamarin.Android s centrem aplikaci přímo z Githubu, služby VSTS nebo Bitbucket.

### <a name="using-teamcity-with-xamarintoolsciteamcitymd"></a>[Používání TeamCity se Xamarinem](~/tools/ci/teamcity.md)

Tento průvodce popisuje kroky s použitím TeamCity pro kompilaci mobilních aplikací a odesílat je na Xamarin Test Cloud.

###  <a name="using-jenkins-with-xamarintoolscijenkins-walkthroughmd"></a>[Používání Jenkins se Xamarinem](~/tools/ci/jenkins-walkthrough.md)

Tato příručka ukazuje, jak nastavit volaných jako server průběžnou integraci a automatizaci kompilování mobilní aplikace vytvořené s funkcí Xamarin. Popisuje postup instalace volaných na OS X, konfiguraci a nastavení úlohy kompilace aplikace Xamarin.iOS a Xamarin.Android, když jsou změny potvrzeny do systému správy verzí.

