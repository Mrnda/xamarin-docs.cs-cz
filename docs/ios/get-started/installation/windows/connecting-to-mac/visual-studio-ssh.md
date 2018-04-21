---
title: Kde je můj Mac?
description: Pokyny ke konfiguraci počítače Mac pro sadu Visual Studio k vytvoření projektů, Xamarin.iOS
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f3e2988c9700549db24ad69277df9e412c99caed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="wheres-my-mac"></a>Kde je můj Mac?

_Pokyny ke konfiguraci počítače Mac pro sadu Visual Studio k vytvoření projektů, Xamarin.iOS_

Aktuální **stabilní** verzi Xamarin pro Visual Studio komunikuje s počítači Mac jinak s předchozími verzemi[^](#earlier-versions).

Pokud chcete použít jako Xamarin Mac Agent algoritmu Mac, je nutné povolit **vzdálené přihlášení** na vaše Mac.

1. Otevřete *Spotlight* (`Cmd-Space`) a vyhledejte **vzdálené přihlášení** pak a vyberte možnost **sdílení** výsledek. Otevře se **předvolbách systému** na **sdílení** panelu.

  ![](visual-studio-ssh-images/spotlight.png "Pomocí vyhledávání Spotlight pro vzdálené přihlášení")

2. Značky **vzdálené přihlášení** možnost **služby** seznamu na levé straně, aby bylo možné povolit Xamarin pro Visual Studio pro připojení k Mac.

  ![](visual-studio-ssh-images/sharing.png "Značky v seznamu služeb možnost pro vzdálené přihlášení")

3. Ujistěte se, že **vzdálené přihlášení** je nastaveno na úroveň přístupu pro **všichni uživatelé**, nebo že vaše uživatelské jméno Mac nebo skupiny je zahrnuta v seznamu povolené uživatele v seznamu na pravé straně.

Počítače Mac musí být zjistitelný Visual Studio teď Pokud je ve stejné síti.
Když Visual Studio se pokusí připojit k počítači Mac, budete vyzváni k přihlášení ze sady Visual Studio pomocí svých přihlašovacích údajů uživatele Mac.

## <a name="where-can-i-find-more-information"></a>Kde najdu informace?

Existuje několik příručky, které vám pomůžou nastavit a pochopit nového agenta, které jsou uvedeny níže:

- [Připojení k počítači Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Odstraňování potíží](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin univerzity - Xamarin Mac Agent](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>Migrace z předchozích verzí

Pokud jste použili starší verze Xamarin pro Visual Studio je byste se seznámit s **Xamarin.iOS sestavení hostitele** aplikaci, která je potřeba spustit na počítači Mac, a potom *spárované* prostřednictvím číslo kódu PIN s vaší Visual Studio instance.

Sestavení hostitele aplikace se už nevyžaduje s touto verzí Xamarin pro Visual Studio.
