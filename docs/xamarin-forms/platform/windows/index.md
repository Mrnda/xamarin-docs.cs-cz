---
title: "Funkce platformy systému Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2017
ms.openlocfilehash: 4385534a6e2ecfc9c908648fa267a543c2313ce0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="windows-platform-features"></a>Funkce platformy systému Windows

Vývoj aplikací Xamarin.Forms na platformách systému Windows vyžaduje Visual Studio. [Požadavky stránky](~/xamarin-forms/get-started/installation.md) obsahuje další informace o požadavky.

![](images/allhanselman.png "Xamarin.Forms aplikace běžící v systému Windows")

## <a name="platform-support"></a>Podpora platformy

K dispozici v sadě Visual Studio Xamarin.Forms šablony obsahovat jeden projektu pro Windows ve výchozím nastavení:

* **Universal Windows Platform Apps** -Xamarin.Forms aplikace lze také optimalizovat pro Windows 10. Univerzální aplikace (UWP) můžete spustit na telefon, tablet a stolních zařízení.

Pokud jste nainstalovali možnosti správné vývoj v sadě Visual Studio, je také možné [přidat](installation/index.md) tyto typy pro podporu starší verze systému Windows projektů:

* **Windows 8.1** – můžete nasadit aplikace Xamarin.Forms na tablet a plochy velikostem jako aplikace Windows 8.1 projektu pomocí ovládacích prvků WinRT.
* **Windows Phone 8.1** -Xamarin.Forms má plnou podporu pro platformu Windows Phone 8.1 pomocí WinRT. Vzhled a chování aplikací pomocí podporu Windows Phone 8.1 se můžou lišit pro vaše starší aplikace Xamarin.Forms Windows Phone, které jsou založené na Silverlight.


> [!NOTE]
> **Poznámka:** podpora 1.x a 2.x Xamarin.Forms _Windows Phone 8 Silverlight_ vývoje aplikací, ale tento typ projektu je zastaralá.


## <a name="getting-started"></a>Začínáme

Přejděte na **soubor > Nový > projekt** v sadě Visual Studio a zvolte jednu z **napříč platformami > prázdná aplikace (Xamarin.Forms)** šablony začít pracovat.

Starší řešení Xamarin.Forms, nebo nebyla vytvořena v systému macOS, nebude mít všechny projekty Windows uvedené výše (ale je třeba ručně přidat).
Pokud chcete zacílit platforma Windows již není ve vašem řešení, navštivte [pokyny](installation/index.md) přidat požadované Windows projektu typu/s.


## <a name="samples"></a>Ukázky kódu

[Všechny ukázky](https://github.com/xamarin/xamarin-forms-book-preview-2) Charlese Petzold knihy [ *vytváření mobilních aplikací s Xamarin.Forms* ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) zahrnují Windows Phone 8.1, Windows 8.1 a projekty univerzální platformu Windows (pro Windows 10).

["Scott Hanselman" ukázku aplikace](https://github.com/jamesmontemagno/Hanselman.Forms) je k dispozici samostatně a také zahrnuje projekty Apple Watch a Android nosit (pomocí Xamarin.iOS a Xamarin.Android, Xamarin.Forms nejde spustit na těchto platformách).


## <a name="related-links"></a>Související odkazy

- [Projekty instalace systému Windows](~/xamarin-forms/platform/windows/installation/index.md)
