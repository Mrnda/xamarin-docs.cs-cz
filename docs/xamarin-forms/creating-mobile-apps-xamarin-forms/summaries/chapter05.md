---
title: Souhrn kapitola 5. Řešení velikostí
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 5. Řešení velikostí'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: c82e222fd47f3a3f13043c076c488b4769659352
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156493"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Souhrn kapitola 5. Řešení velikostí

> [!NOTE] 
> Poznámky na této stránce označit oblasti, kde se Xamarin.Forms se rozcházela z materiály uvedené v seznamu.

Více velikostí v Xamarin.Forms zatím byly zjištěny:

- Výška stavového řádku iOS je 20
- `BoxView` Má výchozí šířku a výšku 40
- Výchozí hodnota `Padding` v `Frame` je 20
- Výchozí hodnota `Spacing` na `StackLayout` 6
- `Device.GetNamedSize` Metoda vrátí číselné písmo

Tyto velikosti nejsou pixelů. Místo toho jsou jednotkách nezávislých na zařízení, nezávisle na sobě rozpoznávaných jednotlivé platformy.

## <a name="pixels-points-dps-dips-and-dius"></a>Obrazové body, body, distribučních bodů, vyhrazené IP adresy a DIUs

V rané fázi historie Apple Mac a Microsoft Windows nastavení spolupráci programátorům v jednotkách, které pixelů. Nástupu vyšším rozlišením však vyžaduje virtualizované a abstraktní přístup na souřadnice obrazovky. Ve světě Mac programátoři pracovali v jednotkách, které *body*, tradičně 1/72 palce při Windows vývojáři použít *jednotkách nezávislých na zařízení* (DIUs) založené na 1/96 palce.

Mobilní zařízení, ale obvykle ukládají brzo plochu a vyšší rozlišení než plochy obrazovky, což znamená, že může tolerovat vyšší hustota pixelů.

Pokračovat v práci v jednotkách, které programátoři cílení na zařízení iPhone a iPad Apple *body*, ale existují 160 těchto bodů palec. V závislosti na zařízení může být 1, 2 nebo 3 pixelů do bodu.

Android je podobná. Programátoři pracovat v jednotkách, které *pixelech nezávislých na hustotě* (dps), a vztah mezi distribučních bodů a je založen na 160 dps palec.

Windows Phone a mobilních zařízení také zavedli škálování faktory, které znamenají něco blízko 160 jednotky nezávislé na zařízení, aby palec.

> [!NOTE]
> Xamarin.Forms už nepodporuje všechny založené na Windows phone nebo mobilním zařízení.

Stručně řečeno programátor Xamarin.Forms cílení na telefonech a tabletech můžete předpokládat, že všechny jednotky měření vycházejí z následujících podmínek:

- 160 jednotky na palec, odpovídá
- 64 jednotek, aby centimetr

Jen pro čtení [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) a [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) vlastnosti určené `VisualElement` mají výchozí hodnoty "napodobení" &ndash;1. Pouze v případě, že element má velikost a shromáždit v rozložení těchto vlastností projeví se skutečná velikost prvku v jednotkách nezávislých na zařízení. Tato velikost obsahuje jakoukoliv `Padding` nastaven pro element, ale ne `Margin`.

Aktivuje vizuální prvek [ `SizeChanged` ](xref:Xamarin.Forms.VisualElement.SizeChanged) událostí při jeho `Width` nebo `Height` došlo ke změně. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) Ukázka používá tuto událost zobrazit velikosti obrazovky programu.

## <a name="metrical-sizes"></a>Metrical velikosti

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) používá [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) a [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) zobrazíte `BoxView` 2,5 talovat a jeden centimetr široké.

## <a name="estimated-font-sizes"></a>Písmo odhadované velikosti

[ **FontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) příklad ukazuje, jak použít 160 jednotky na the palců pravidlo k určení velikosti písem v jednotkách, které body. Vizuální konzistence mezi platformami pomocí této techniky je obecně lepší než `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Přizpůsobení text, který má k dispozici velikost

Je možné přizpůsobit blok textu v rámci konkrétní obdélník výpočtem `FontSize` z `Label` pomocí následujících kritérií:

- Řádkování je 120 % velikost písma (% 130 na platformách Windows).
- Šířka znaku průměrné je 50 % velikosti písma.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) ukázka demonstruje tento postup. Tento program vytvořené před [ `Margin` ](xref:Xamarin.Forms.View.Margin) vlastnost byla k dispozici, aby používalo [ `ContentView` ](xref:Xamarin.Forms.ContentView) s [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) nastavení pro simulaci okraj.

[![Trojitá snímek odhadované písmo](images/ch05fg07-small.png "Text přizpůsobení dostupné velikosti")](images/ch05fg07-large.png#lightbox "Text přizpůsobení dostupné velikosti")

## <a name="a-fit-to-size-clock"></a>Přizpůsobit velikost hodiny

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) ukázka znázorňuje použití [ `Device.StartTimer` ](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) spustit časovač, který pravidelně upozorní aplikaci, když je čas aktualizovat hodiny. Velikost písma je nastavený na čtvrtinu, šestinu šířky stránky, abyste měli zobrazení co největší.

## <a name="accessibility-issues"></a>Usnadnění

**EstimatedFontSize** programu a **FitToSizeClock** program oba obsahují drobným chybu: Pokud uživatel změní nastavení usnadnění v telefonu na Android nebo Windows 10 Mobile, aplikace už odhadnout velikost textu je vykreslen na základě velikosti písma. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) ukázka demonstruje tento problém.

## <a name="empirically-fitting-text"></a>Empirických přizpůsobování textu

Dalším způsobem podle textu do obdélníku je empirických vypočítat velikost vykresleného textu a upravte ho nahoru nebo dolů. Program ve voláních knihy [ `GetSizeRequest` ](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) na vizuální prvek získat požadovaná velikost prvku. Že metoda je zastaralá a programy by měly místo toho volat [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)).

Pro `Label`, první argument by měl být Šířka kontejneru (povolit zalamování), zatímco druhý argument by měl být nastaven na `Double.PositiveInfinity` na výšku vstupy bez omezení. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) ukázka demonstruje tento postup.



## <a name="related-links"></a>Související odkazy

- [Kapitola 5 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Ukázky kapitola 5](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Ukázky kapitola 5 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
