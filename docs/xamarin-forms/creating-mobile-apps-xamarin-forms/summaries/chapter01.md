---
title: Souhrn kapitoly 1. Jak se uplatní Xamarin.Forms?
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 1. Jak se uplatní Xamarin.Forms?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 2897229b0749b1a6ead805d6ad063603a77f8f0d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240457"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Souhrn kapitoly 1. Jak se uplatní Xamarin.Forms?

Mezi nejvíce nepříjemným úlohy při programování je portování kód základní z jedné platformy na jiný, zvlášť pokud této platformě zahrnuje různé programovací jazyk. Dojde riziko portování kód, který je také Refaktorovat, avšak pokud obě platformy musí být zachovaná paralelně, pak rozdíly mezi základů dvou kódu budou budoucí údržby obtížnější.

## <a name="cross-platform-mobile-development"></a>Vývoj mobilních řešení napříč platformami

Tento problém je běžné, pokud je cílem mobilní platformy. V současné době existují dvě hlavní mobilní platformy, Apple řadu Iphony a Ipady spuštěn operační systém iOS a Android operačního systému, který běží na různých druhů telefonů a tabletů. Jiné významné platformě je Microsoftu pro univerzální platformu Windows (UWP), který umožňuje jednu aplikaci pro Windows 10 a Windows 10 Mobile.

Dodavatele softwaru, který chce tyto tři platformy musí řešit vzorů různých uživatelského rozhraní, tři různé vývojových prostředí, tři různé programovací rozhraní, a&mdash;možná nejvíce nepohodlně&mdash; třech různých programovacích jazycích: jazyka Objective-C pro iPhone a iPad, Java pro Android a C# pro systém Windows.

## <a name="the-c-and-net-solution"></a>Řešení C# a rozhraní .NET

I když jazyka Objective-C, Java a C# jsou odvozeny od programovacího jazyka C, vyvinuly se příliš neliší cestami. C# je poslední z těchto jazyků a má byla splatných v velmi užitečné způsoby. Kromě toho C# je velmi úzce spojené s infrastrukturou celý programovací volat rozhraní .NET, která poskytuje podporu pro matematické, ladění, reflexe, kolekce, globalizace, vstupně-výstupní, sítě, zabezpečení, dělení na vlákna, webové služby, manipulaci s daty a XML a JSON čtení a zápis.

Xamarin aktuálně poskytuje nástroje, jehož cílem je nativní Mac, iOS a Android rozhraní API pomocí jazyka C# a rozhraní .NET. Tyto nástroje se nazývají Xamarin.Mac, Xamarin.iOS a Xamarin.Android, souhrnně říká platformou Xamarin. Jsou to knihovny a vazby, které express nativních rozhraní API z těchto platforem s .NET idioms.

Vývojáři mohou pomocí platformy Xamarin pro psaní aplikací v jazyce C#, že cílové Mac, iOS nebo Android. Ale při cílení na více než jedné platformě, umožňuje spoustu smysl sdílet některé kód mezi cílové platformy. To zahrnuje rozdělit program do kódu platformy závislé (obvykle zahrnující uživatelské rozhraní) a nezávislé na platformě kód, který obvykle vyžaduje pouze základní rozhraní .NET framework. Tento kód nezávislé na platformě se může nacházet v přenosných třída knihovny PCL () nebo sdílený projekt, často označovaný jako sdílený prostředek projekt nebo SAP.

## <a name="introducing-xamarinforms"></a>Představení Xamarin.Forms

Při cílení na více mobilních platforem, Xamarin.Forms umožňuje sdílení i další kód. Jeden program vytvořený pro Xamarin.Forms, můžete vybrat pět různých platforem:

- iOS pro programy, které běží na zařízení iPhone, iPad a iPod touch
- Pro programy, které běží na Android telefony a tablety Android
- Univerzální platformu Windows na cíl Windows 10 a Windows 10 Mobile
- prostředí Windows Runtime rozhraní API systému Windows 8.1
- prostředí Windows Runtime rozhraní API systému Windows Phone 8.1

Aktuální šablony řešení Xamarin.Forms nezahrnují šablony projektů pro platformy Windows 8.1 a Windows Phone 8.1.

Hromadné programu Xamarin.Forms existuje PCL nebo SAP. Každý z platformy se skládá z stub malá aplikace, která volá do PCL. Rozhraní API Xamarin.Forms mapování na nativní ovládací prvky na každou platformu, tak, aby každá platforma udržuje jeho charakteristik vzhled a chování:

[![Trojitá snímek obrazovky platformy vizuály sdílení](images/ch01fg03-small.png "Xamarin.Forms ovládacích prvků na každou platformu")](images/ch01fg03-large.png#lightbox "Xamarin.Forms ovládacích prvků na každou platformu")

Snímky obrazovky zleva doprava zobrazit zařízení typu iPhone, Android phone a Windows 10 Mobile phone. Na každé obrazovce, tato stránka obsahuje platformě Xamarin.Forms [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) pro zobrazení textu, [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) za inicializaci akce, [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) pro Výběr hodnotou zapnout nebo vypnout a [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) pro zadání hodnotu nepřetržitá rozsahu. Všechny čtyři těchto zobrazení jsou podřízené objekty [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Také připojit na stránku je Xamarin.Forms panel nástrojů, který se skládá z několika [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) objekty. Toto jsou viditelné jako ikony horní iOS a Android obrazovky a v dolní části obrazovky Windows 10 Mobile.

Xamarin.Forms také podporuje XAML, Extensible Application Markup Language vyvinuté v Microsoft pro několik platforem aplikace. Vizuální prvky programu uvedené výše jsou definovány v jazyce XAML, jak je předvedeno v [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) ukázka.

Xamarin.Forms program můžete určit, jaké platformy je spuštěn na a odpovídajícím způsobem je provede jiný kód. Vývojáři jednodušší, můžete napsat vlastní kód pro různé platformy a spustit tento kód z aplikace Xamarin.Forms způsobem nezávislé na platformě. Vývojáři mohou také vytvářet další ovládací prvky napsáním nástroji pro vykreslování pro každou platformu.

Zatímco Xamarin.Forms je dobrým řešením pro-obchodní aplikace, nebo při vytváření prototypu nebo vytváření rychlé ukázka testování konceptu, je méně ideální pro aplikace, které vyžadují vektorová grafika nebo komplexní touch interakce.

## <a name="your-development-environment"></a>Vývojové prostředí

Vývojové prostředí závisí na co platformy, které chcete zacílit a co počítače, které chcete použít.

Pokud chcete cílový iOS, budete potřebovat Mac s Xcode a Xamarin platformu nainstalována. Podpora Android také vyžaduje instalaci Java a požadované sady SDK. Pak můžete vybrat iOS a Android pomocí sady Visual Studio for Mac.

Instalace sady Visual Studio umožňuje na počítači PC cíl iOS, Android a všechny platformy systému Windows. Ale cílení na iOS ze sady Visual Studio vyžaduje stále Mac s Xcode a Xamarin platformu nainstalována.

Programy na buď skutečné zařízení s připojením USB k počítači nebo na simulátoru můžete otestovat.

## <a name="installation"></a>Instalace

Před vytvořením a vytvoření aplikace Xamarin.Forms, doporučujeme vytvořte a sestavte samostatně aplikace pro iOS, Android aplikace a aplikaci UWP, v závislosti na platformách, které chcete cíl a vývojové prostředí.

Webové servery Xamarin a Microsoft obsahují informace o tom, jak to udělat:

- [Začínáme s iOS](~/ios/get-started/index.md)
- [Začínáme s Android](~/android/get-started/index.md)
- [Centrum vývojářů pro Windows](http://dev.windows.com)

Jednou můžete vytvořit a spustit projekty pro tyto jednotlivé platformy, měli byste mít žádné problémy, vytváření a spouštění aplikací Xamarin.Forms.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 1 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Ukázka kapitoly 1](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
