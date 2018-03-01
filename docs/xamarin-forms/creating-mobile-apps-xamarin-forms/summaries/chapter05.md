---
title: "Souhrn kapitoly 5. Práci s velikostí"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 4454150b4caad86eb063ab7fcf8a721cbab9b5ec
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Souhrn kapitoly 5. Práci s velikostí

Několik velikosti na platformě Xamarin.Forms došlo, pokud:

- Výška iOS stavový řádek je 20
- `BoxView` Má výchozí šířku a výšku 40
- Výchozí hodnota `Padding` v `Frame` je 20
- Výchozí hodnota `Spacing` na `StackLayout` 6
- `Device.GetNamedSize` Metoda vrátí velikost číselné písma

Tyto velikosti nejsou pixelů. Místo toho jsou nezávislé na zařízení jednotky nezávisle rozpoznáno každou platformu.

## <a name="pixels-points-dps-dips-and-dius"></a>Pixelů, body, distribučních bodů, vyhrazené IP adresy a DIUs

Časná v historií Apple Mac a Microsoft Windows programátory fungovala jednotky pixelů. Nástupem vyšší řešení zobrazí však vyžaduje více virtualizované a abstraktní přístupu k souřadnice obrazovky. Na světě Mac programátory fungovala jednotky *body*, tradičně 1/72 palce při Windows vývojáři použít *jednotky nezávislé na zařízení* na 1/96 palce na základě (DIUs).

Mobilní zařízení, ale jsou obecně uložené mnohem blíže tučné a vyšší rozlišení než plochu obrazovky, které znamená, že může tolerovat vyšší hustotu pixelů.

Cílení na zařízení Apple iPhone a iPad programátory pokračovat v práci v jednotkách *body*, ale existují 160 těchto bodů palec. V závislosti na zařízení může být 1, 2 nebo 3 pixelů do bodu.

Android jsou podobné. Programátory pracovních jednotek *nezávislé na hustotě pixelů* (distribučních bodů), a je na 160 distribučních bodů na palec na základě vztah mezi distribučních bodů a pixelů.

Prostředí Windows Runtime také navázal škálování faktorů, které implikují něco blízko 160 jednotky nezávislé na zařízení, které mají palec.

V souhrnu Xamarin.Forms programátorem cílení na telefonů a tabletů Předpokládejme, aby všechny jednotky měření jsou založené na následující kritéria:

- 160 jednotky pro palec, ekvivalentní
- 64 jednotky pro centimetr

Jen pro čtení [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) a [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) vlastnosti definované `VisualElement` mít výchozí "model" hodnoty & #x 2013; 1. Jenom v případě, že element má velikost a shromáždit v rozložení se tyto vlastnosti projeví skutečná velikost element v jednotky nezávislé na zařízení. Tato velikost obsahuje jakoukoliv `Padding` nastavené u elementu, ale ne `Margin`.

Aktivuje se vizuální prvek [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/) události při jeho `Width` nebo `Height` došlo ke změně. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) ukázce se používá tato událost Pokud chcete zobrazit velikost obrazovky programu.

## <a name="metrical-sizes"></a>Metrical velikosti

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) používá [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) a [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) k zobrazení `BoxView` jeden palec talovat a jeden centimetr široké.

## <a name="estimated-font-sizes"></a>Velikosti odhadované písem

[ **FontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) ukázka ukazuje, jak použít 160 jednotky na the palců pravidlo k určení velikosti písem v jednotkách bodů. Visual konzistence mezi touto technikou platformy je lepší, než `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Přizpůsobení textu do dostupné velikosti

Je možné, aby vyhovovaly blok textu v obdélníku konkrétní pomocí výpočtu `FontSize` z `Label` pomocí následujících kritérií:

- Mezery mezi řádky je 120 % velikosti písma (130 % na platformách systému Windows).
- Šířka znaku průměrná je 50 % velikosti písma.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) příklad znázorňuje tento postup. Tento program byla zapsána před [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) byla vlastnost k dispozici, takže používá [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) s [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) nastavení pro simulaci okraj.

[![Trojitá snímek obrazovky velikost písma odhadované](images/ch05fg07-small.png "Text nevejde do dostupné velikosti")](images/ch05fg07-large.png "Text nevejde do dostupné velikosti")

## <a name="a-fit-to-size-clock"></a>Přizpůsobit velikost hodiny

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) příklad ukazuje použití [ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/) spustit časovač, který pravidelně upozorní aplikace, když je na čase aktualizovat hodiny. Aby zobrazení co největší velikost písma nastavena na-šestinu šířce stránky.

## <a name="accessibility-issues"></a>Problémy s

**EstimatedFontSize** program a **FitToSizeClock** program obě obsahují jemně chybu: Pokud uživatel změní nastavení usnadnění telefonu na Android nebo Windows 10 Mobile, už programu můžete odhadnout, jak velký vykreslením text na základě velikosti písma. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) příklad znázorňuje tento problém.

## <a name="empirically-fitting-text"></a>Empirically přizpůsobování textu

Dalším způsobem podle textu do obdélníku je empirically vypočítat velikost vykresleného textu a upravit ho nahoru nebo dolů. Program volání kniha [ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/) na vizuální prvek získat požadovaná velikost elementu. Že metoda je zastaralá a programy by měly volat místo [`Measure`] (nebo api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/).

Pro `Label`prvního argumentu musí být Šířka kontejneru (aby bylo možné zabalení), zatímco druhý argument by měl být nastaven na `Double.PositiveInfinity` aby neomezeného výšku. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) příklad znázorňuje tento postup.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 5 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Ukázky kapitoly 5](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Kapitola 5 F # – ukázky](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
