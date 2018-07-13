---
title: Souhrn kapitoly 1. Jak zapadá Xamarin.Forms?
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 1. Jak zapadá Xamarin.Forms?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 58a8976b054ac7fad5c4e24f0561d1b4e468c1b2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995128"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Souhrn kapitoly 1. Jak zapadá Xamarin.Forms?

Jeden z nejčastěji nepříjemným úlohy při programování je portování kódu z jedné platformy na jiný, zejména v případě, že tuto platformu zahrnuje různé programovací jazyk. Při přenášení kód Refaktorovat a je pokušení, ale pokud obě platformy musí být zachována paralelně, pak rozdíly mezi dvěma kódech bude ztížit budoucí údržby.

## <a name="cross-platform-mobile-development"></a>Vývoj mobilních řešení napříč platformami

Tento problém je běžný při cílení na mobilní platformy. V současné době existují dva hlavní mobilní platformy, řada Apple Iphony a Ipady s operačního systému iOS a Android operační systém, který běží na různých druhů telefonů a tabletů. Další významné platforma je společnosti Microsoft Universal Windows Platform (UPW), umožňující jedné programu k cílení na Windows 10 a Windows 10 Mobile.

Dodavatele softwaru, který chce tyto tři platformu musí čelit paradigmat různých uživatelského rozhraní, tří různých vývojových prostředích, tři různé programovací rozhraní, a&mdash;možná největší nepohodlně&mdash; tří různých programovacích jazycích: Objective-C pro iPhone a iPad, Java pro Android a C# pro Windows.

## <a name="the-c-and-net-solution"></a>Řešení C# a .NET

I když Objective-C, Java a C# jsou odvozeny z programovací jazyk C, že se vyvinula velmi odlišnými cestami. C# je nejnovější z těchto jazyků a bylo zrání způsoby velmi užitečné. Kromě toho C# je velmi úzce spojené s celou programovací infrastrukturu, která volá .NET, která poskytuje podporu pro matematické, ladění, reflexe, kolekce, globalizace, vstupu a výstupu souborů, sítě, zabezpečení, práce s vlákny, webových služeb, zpracování dat a XML a JSON pro čtení a zápis.

Xamarin aktuálně poskytuje nástroje, které cílí nativní Mac, iOS a Android API s využitím C# a .NET. Tyto nástroje jsou volány Xamarin.Mac, Xamarin.iOS a Xamarin.Android, souhrnně označované jako platformy Xamarin. Toto jsou knihovny a vazby, které express nativních rozhraní API z těchto platforem s idiomy .NET.

Vývojáři mohou pomocí platformy Xamarin pro psaní aplikací v jazyce C#, že cílové Mac, iOS nebo Android. Ale při cílení na více než jedné platformě, je velmi výhodné sdílet část z kódu mezi platformami cílového. To zahrnuje rozdělení program do kódu závislého na platformě (obecně zahrnující uživatelského rozhraní) a kód nezávislý na platformě, což obvykle vyžaduje pouze základní rozhraní .NET framework. Tento kód nezávislý na platformě se může nacházet v přenosnou knihovnou tříd (PCL) nebo sdíleného projektu, často označované jako projekt sdílených prostředků nebo SAP.

## <a name="introducing-xamarinforms"></a>Úvod do Xamarin.Forms

Při cílení na různé mobilní platformy, Xamarin.Forms umožňuje ještě větší sdílení kódu. Jeden program napsané pro Xamarin.Forms může cílit na pět různých platforem:

- iOS pro programy, které běží na iPhone, iPad a iPod touch
- Android pro programy, které běží na Androidu telefonech a tabletech
- Univerzální platforma Windows na cíl Windows 10 a Windows 10 Mobile
- modul Runtime Windows rozhraní API systému Windows 8.1
- modul Runtime Windows rozhraní API nástroje Windows Phone 8.1

Aktuální šablony řešení Xamarin.Forms nezahrnují šablony projektů pro platformy Windows 8.1 a Windows Phone 8.1.

Hromadné Xamarin.Forms program existuje v PCL nebo SAP. Každou z platforem se skládá z zástupnou proceduru malou aplikaci, která volá PCL. Rozhraní API Xamarin.Forms mapování na nativní ovládací prvky na jednotlivých platformách tak, aby každá platforma udržuje jeho charakteristické vzhled a chování:

[![Trojitá snímek obrazovky s vizuály platforma pro sdílení obsahu](images/ch01fg03-small.png "Xamarin.Forms ovládacích prvků na každé platformě")](images/ch01fg03-large.png#lightbox "Xamarin.Forms ovládacích prvků na každé platformě")

Snímky obrazovky zleva doprava zobrazit Iphonu, telefonu s Androidem a Windows 10 Mobile phone. Na jednotlivých obrazovkách stránka obsahuje Xamarin.Forms [ `Label` ](xref:Xamarin.Forms.Label) pro zobrazení textu, [ `Button` ](xref:Xamarin.Forms.Button) za inicializaci akce, [ `Switch` ](xref:Xamarin.Forms.Switch) pro výběr na hodnotu Zapnuto/vypnuto a [ `Slider` ](xref:Xamarin.Forms.Slider) pro zadání hodnoty průběžné rozsahu. Všechny čtyři tato zobrazení jsou podřízené [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

Také připojeny k stránky je Xamarin.Forms nástrojů, který se skládá z několika [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) objekty. Toto jsou viditelné jako ikony nahoře v iOS a Android obrazovek a v dolní části obrazovky Windows 10 Mobile.

Také podporuje XAML Xamarin.Forms, Extensible Application Markup Language vyvinuté v Microsoftu pro několik aplikačních platformách. Všechny vizuály programu uvedené výše jsou definovány v XAML, jak je ukázáno v [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) vzorku.

Xamarin.Forms programu můžete určit, jestli běží na platformě a provést různý kód odpovídajícím způsobem. Vývojáři jednodušší, můžete psát vlastní kód pro různé platformy a spustit tento kód z aplikace Xamarin.Forms způsobem nezávislá na platformě. Vývojáři mohou také vytvořit další ovládací prvky napsáním renderery pro každou platformu.

Xamarin.Forms je dobrým řešením pro-obchodní aplikace nebo pro vytváření prototypů nebo zajištěním, rychlou ukázku testování konceptu, je méně ideální pro aplikace, které vyžadují vektorovou grafiku nebo komplexní dotykovou interakci.

## <a name="your-development-environment"></a>Vývojové prostředí

Vývojové prostředí závisí na platformách, kterou chcete cílit a co počítače, které chcete použít.

Pokud chcete do cílového systému iOS, budete potřebovat počítač Mac s Xcode a nainstalovanou platformu Xamarin. Také podporuje Android vyžaduje instalaci Javy a požadované sady SDK. Můžete směrovat se systémy iOS a Android pomocí sady Visual Studio pro Mac.

Instalace sady Visual Studio na počítači vám umožní cílit na všechny platformy Windows, iOS a Android. Ale cílících na iOS ze sady Visual Studio nadále vyžaduje počítač Mac s Xcode a nainstalovanou platformu Xamarin.

Můžete otestovat programy na buď skutečný zařízení připojeném pomocí USB k počítači nebo na simulátoru.

## <a name="installation"></a>Instalace

Před vytvořením a sestavování aplikací Xamarin.Forms, doporučujeme vytvořit a sestavit samostatně aplikace pro iOS, aplikaci pro Android a aplikaci pro UPW, v závislosti na platformách, které mají cíl a vývojového prostředí.

Webové servery Xamarin a Microsoft obsahují informace o tom, jak to udělat:

- [Začínáme s Iosem](~/ios/get-started/index.md)
- [Začínáme s Androidem](~/android/get-started/index.md)
- [Windows Dev Center](http://dev.windows.com)

Můžete vytvořit jednou a spouštění projektů pro tyto jednotlivé platformy, neměla mít problém vytváření a spouštění aplikací Xamarin.Forms.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 1 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Ukázka kapitoly 1](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
