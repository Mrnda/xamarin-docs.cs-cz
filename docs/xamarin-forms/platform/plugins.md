---
title: Moduly plug-in
description: Nativní funkce snadno přidat na platformě Xamarin.Forms aplikace
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: 5770d13c46998872752820b7a0cbb222a04c3ff8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="plugins"></a>Moduly plug-in

Existuje mnoho funkcí nativní platformy, které existují pro všechny platformy, ale mají mírně odlišné rozhraní API. Vývojáři psát modulů plug-in pro vytvoření abstraktní rozhraní a platformy pro tyto funkce, které můžou taky sdílet s ostatními.

Tyto funkce patří: stav baterie, kompas, pohybu senzorů, informace o zeměpisné poloze, převod textu na řeč a mnoho dalších. Moduly plug-in povolit tyto funkce nelze snadno přistupovat pomocí Xamarin.Forms aplikací.

## <a name="finding-and-adding-plugins"></a>Hledání a přidání modulů plug-in

Komunita Xamarin vytvořil mnoha modulů plug-in napříč platformami kompatibilní s Xamarin.Forms - velké kolekce nachází zde:

[**Xamarin Plugins**](https://github.com/xamarin/plugins)

Průvodce přidání balíčků NuGet do projektu, naleznete v našem návodu na [včetně balíček NuGet do projektu](/visualstudio/mac/nuget-walkthrough/).


## <a name="creating-plugins"></a>Vytváření modulů plug-in

Je také možné vytvoření a publikování vlastní moduly plug-in jako balíčky Nuget (a Xamarin součásti). Mnoho existující modulů plug-in jsou open source, takže můžete zkontrolovat svůj kód, abyste pochopili, jak nejsou writtern.

Například seznam modulů plug-in níže jsou všechny s otevřeným zdrojem, a odpovídají ukázky [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) části:

- **Převod textu na řeč** podle James Montemagno &ndash; [Githubu](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) a [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **Stav baterie** podle James Montemagno &ndash; [Githubu](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) a [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Tyto projekty Githubu poskytne to dobrý výchozí bod pro vytvoření vlastní moduly plug-in a platformy, stejně jako tyto pokyny pro [vytváření modulu plug-in pro Xamarin](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Strukturování projekty modulů plug-in a platformy

I když neexistují žádné zvláštní požadavky pro návrh balíčku NuGet, jsou uvedeny pokyny pro vytváření balíčku pro aplikací pro různé platformy.

Modul plug-in napříč platformami obecně by měla obsahovat následující součásti:

- PCL s rozhraní, které představuje rozhraní API pro modul plug-in,
- iOS, Android a Windows třídy knihovny s implementaci rozhraní.

Čtení James Montemagno [příspěvku na blogu](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) popisující proces vytváření modulů plug-in pro Xamarin.

Je vhodnější předejdete odkazující na platformě Xamarin.Forms přímo z modulu plug-in.
To můžete vytvořit konfliktu verze problémy při jinými vývojáři pokus o použití modulu plug-in. Místo toho zkuste návrh rozhraní API, aby se může použít jakékoli aplikace Xamarin nebo .NET.

### <a name="publishing-nuget-packages"></a>Publikování balíčků NuGet

Balíčky NuGet **nuspec** souboru, který je soubor xml, který definuje, které části projektu jsou publikovány v balíčku. **Nuspec** soubor zahrnuje taky informace o balíčku, jako je například id, názvu a autoři.

V tématu [NuGet dokumentace](http://docs.nuget.org/create/creating-and-publishing-a-package) Další informace o vytváření a publikování balíčků NuGet.


## <a name="related-links"></a>Související odkazy

- [Vytváření opakovaně použitelných modulů plug-in pro Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Pomocí & vývoj modulů plug-in pro Xamarin (video)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
