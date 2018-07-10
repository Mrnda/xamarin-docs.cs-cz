---
title: Používání a vytváření modulů plug-in Xamarin.Forms
description: Tento článek vysvětluje, jak využívat a vytvářet moduly plug-in Xamarin.Forms. Moduly plug-in se obvykle používají k jednoduchým způsobem můžete zveřejnit funkce nativní platformy.
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
ms.openlocfilehash: 4d121c2dfcca380e1735da1a4ca47c42d1957b8a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854737"
---
# <a name="consuming-and-creating-xamarinforms-plugins"></a>Používání a vytváření modulů plug-in Xamarin.Forms

Existuje mnoho funkcí nativní platformy, které existují na všech platformách, ale mají mírně odlišné rozhraní API. Jedním ze způsobů pro vývojáře k používání těchto funkcí je vytvoření abstraktní multiplatformního rozhraní a pak implementace rozhraní v různých platformách. Aplikace Xamarin.Forms pak přistupuje k implementace pro tyto platformy pomocí [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

Vývojáři tuto práci můžete sdílet zápisem _modulu plug-in_ a publikujete ji na NuGet.

> [!NOTE]
> Řadu multiplatformních funkcí byly dřív dostupné jenom prostřednictvím modulů plug-in jsou teď součástí open-source **[Xamarin.Essentials](~/essentials/index.md)** knihovny. Tyto funkce patří: stav baterie, compass, senzory pohybu, informace o zeměpisné poloze, převod textu na řeč a mnoho dalších. V budoucnu **Xamarin.Essentials** bude sloužit jako primární zdroj funkce multiplatformní aplikace Xamarin.Forms. I když se vývojáři můžete pořád vytvářet a publikovat moduly plug-in, vezměte v úvahu přispívající k prodloužení **Xamarin.Essentials**.

## <a name="finding-and-adding-plugins"></a>Hledání a přidávání modulů plug-in

Společenství Xamarin vytvořila mnoho multiplatformních modulů plug-in kompatibilní s Xamarin.Forms. Rozsáhlá kolekce najdete na následujících stránkách:

[**Xamarin Plugins**](https://github.com/xamarin/XamarinComponents)

Tento průvodce do vašeho projektu se přidávají balíčky NuGet, najdete v našem návodu na [zahrnutí balíčku NuGet ve vašem projektu](/visualstudio/mac/nuget-walkthrough/).

## <a name="creating-plugins"></a>Vytváření modulů plug-in

Je také možné k vytvoření a publikování vlastní moduly plug-in jako balíčky Nuget (a Xamarin Components). Mnoho existujících moduly plug-in jsou open source, takže můžete zkontrolovat jejich kód, abyste pochopili, jak byly writtern.

Například seznam modulů plug-in níže jsou všechny open source a odpovídají na některé ukázky v [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) části:

- **Převod textu na řeč** podle James Montemagno &ndash; [Githubu](https://github.com/jamesmontemagno/TextToSpeechPlugin) a [NuGet  ](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **Stav baterie** podle James Montemagno &ndash; [Githubu](https://github.com/jamesmontemagno/BatteryPlugin) a [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

Tyto projekty Githubu můžete zadat dobrým výchozím bodem pro vytvoření vlastní moduly plug-in pro různé platformy, stejně jako tyto pokyny pro [vytváření modul plug-in pro Xamarin](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin).

### <a name="structuring-cross-platform-plugin-projects"></a>Strukturování projektů modulu plug-in pro různé platformy

I když neexistují žádné zvláštní požadavky na návrh balíčku NuGet, existují některé pokyny pro vytvoření balíčku pro aplikace pro různé platformy.

V minulosti se modul plug-in multiplatformní obecně skládal z následujících součástí:

- PCL s, který představuje rozhraní API pro modul plug-in, rozhraní
- iOS, Android a univerzální platformu Windows (UPW) třídy knihovny se zmírněními hrozeb implementace rozhraní.

Čtení James Montemagno [blogový příspěvek](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) popisující proces vytváření modulů plug-in pro Xamarin.

Nedávno moduly plug-in lze možné vytvořit pomocí jedné více cílové platformy. Tento přístup je podrobněji popsána James Montemagno [blogový příspěvek](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/). Tento přístup se používá v moduly plug-in James Montemagno propojené výše, a také ve formátu v **Xamarin.Essentials**.

Je vhodnější Chcete-li zabránit odkazování Xamarin.Forms přímo z modulu plug-in.
To můžete vytvořit konflikt verzí problémy, když ostatní vývojáři se pokoušejí pro používání modulu plug-in. Místo toho zkuste k návrhu rozhraní API tak, aby ho můžete použít ve všech aplikacích Xamarin nebo .NET.

### <a name="publishing-nuget-packages"></a>Publikování balíčků NuGet

Mají balíčky NuGet **nuspec** soubor, který se nachází soubor xml, který definuje, jaké části vašeho projektu se publikují v balíčku. **Nuspec** soubor zahrnuje taky informace o balíčku, jako je například id, název a autoři.

Zobrazit [dokumentace pro NuGet](/nuget/create-packages/creating-a-package.md) Další informace o vytváření a publikování balíčků NuGet.

## <a name="related-links"></a>Související odkazy

- [Vytváření opakovaně použitelných moduly plug-in pro Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Pomocí & vývoj modulů plug-in pro Xamarin (video)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
