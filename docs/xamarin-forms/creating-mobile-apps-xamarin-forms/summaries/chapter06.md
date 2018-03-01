---
title: "Souhrn kapitoly 6. Kliknutí na tlačítko"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 44dbb4d2cdc573e721bb772fdd05d392c90b746a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>Souhrn kapitoly 6. Kliknutí na tlačítko

[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Zobrazení, které umožňuje uživatelům spustit příkaz. A `Button` je identifikována text (a volitelně bitovou kopii jak je předvedeno v [kapitoly 13, rastrové obrázky](chapter13.md)). V důsledku toho `Button` definuje řadu stejných vlastností, jako `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` také definuje tři vlastnosti, řídí vzhled jeho ohraničení, ale podporu těchto vlastností a jejich vzájemné nezávislost je specifický pro platformu:

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) typu `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) typu `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) typu `Double`

`Button` rovněž dědí všechny vlastnosti `VisualElement` a `View`, včetně `BackgroundColor`, `HorizontalOptions`, a `VerticalOptions`.

## <a name="processing-the-click"></a>Zpracování a klikněte na

`Button` Třída definuje [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) událost, která je aktivována, když uživatel klepnutím `Button`. `Click` Obslužná rutina je typu `EventHandler`. První argument je `Button` objektu generování události; druhý argument je `EventArgs` objekt, který poskytuje žádné další informace.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) příklad znázorňuje jednoduché `Clicked` zpracování.

## <a name="sharing-button-clicks"></a>Kliknutím na tlačítko pro sdílení

Více `Button` zobrazení můžete sdílet stejný `Clicked` obslužná rutina, ale obslužná rutina obecně musí určit, které `Button` zodpovídá za určité události. Jeden z přístupů je pro uložení různých `Button` objekty pole a kontrolu, která se aktivuje událost v obslužné rutině.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) příklad znázorňuje tento postup. Program také ukazuje, jak nastavit [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) vlastnost `Button` k `false` při stisknutí kláves `Button` již není platný. A zakázané `Button` negeneruje `Clicked` událostí.

## <a name="anonymous-event-handlers"></a>Obslužné rutiny anonymní událostí

Je možné definovat `Clicked` obslužné rutiny jako anonymní lambda funkce, jako [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) příklad znázorňuje. Anonymní obslužné rutiny však nelze sdílet bez komplikované reflexe kód.

## <a name="distinguishing-views-with-ids"></a>Rozlišovací zobrazení s ID

Více `Button` objektů můžete také rozlišovat podle nastavení [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) vlastnost nebo [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) vlastnosti `string`. Tato vlastnost je definována `Element` však není použit v rámci Xamarin.Forms. Je určena k používány pouze ve programy aplikací.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) Ukázka používá stejné obslužné rutiny události pro všechny 10 čísel klíče na numerické klávesnici a slouží k rozlišení mezi nimi pomocí `StyleId` vlastnost:

[![Trojitá snímek obrazovky nejjednodušší klávesnici](images/ch06fg04-small.png "kalkulačky")](images/ch06fg04-large.png "kalkulačky")

## <a name="saving-transient-data"></a>Ukládání dat přechodné

Mnoho aplikací potřebovat k uložení dat při ukončení programu a znovu načíst data při spuštění programu znovu. [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) Třída definuje několik členů, které pomáhají váš program uložení a obnovení dat o přechodný:

- [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) Vlastnost je slovník s `string` klíče a `object` položky. Obsah slovníku jsou automaticky uloží do místního úložiště aplikací před ukončení programu a znovu načíst při spuštění programu.
- `Application` Třída definuje tři chráněné virtuální metody, že je standardní program `App` třídy přepsání: [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), a [ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). Tyto odkazovat na *životního cyklu aplikace* události.
- [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) Metoda uloží obsah slovníku.

Není nutné volat `SavePropertiesAsync`. Obsah slovníku jsou automaticky před ukončení programu uloží a načtené před spuštění programu. Je užitečná během testování program k uložení dat, pokud dojde k chybě programu.

Užitečné je také:

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/), pomocí statické vlastnosti, který vrací aktuální `Application` objekt, který pak můžete získat `Properties` slovníku.

Prvním krokem je identifikace všech proměnných na stránku, která chcete zachovat při ukončení programu. Pokud znáte všechna místa, kde tyto proměnné změnit, můžete jednoduše přidat je do `Properties` slovníku v daném okamžiku. V konstruktoru stránky, můžete nastavit proměnné z `Properties` slovníku existuje-li klíč.

Větší programu bude pravděpodobně muset řešit události životního cyklu aplikace. Nejdůležitější je `OnSleep` metoda. Volání této metody označuje, že program opustil popředí. Možná má uživatel stisknutí **Domů** tlačítko na zařízení, zobrazí všechny aplikace nebo se vypíná telefonu. Volání `OnSleep` pouze oznámení, že program přijme dříve, než je ukončen. Tento program by měl využít této příležitosti a ujistěte se, že `Properties` slovník je aktuální.

Volání `OnResume` označuje, že program není ukončit po posledním volání `OnSleep` , ale je teď spuštěná v popředí, se znovu. Program může použít tuto možnost aktualizovat připojení k Internetu (třeba).

Volání `OnStart` dojde při spuštění programu. Není nutné čekat, až volání této metody pro přístup k `Properties` slovníku protože již bylo obsah obnovit, když `App` volání konstruktoru.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) ukázka je velmi podobné **SimplestKeypad** s tím rozdílem, že program používá `OnSleep` přepsání uložit aktuální položky klávesnici a stránka konstruktor data obnovit.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 6 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Ukázky kapitoly 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Kapitola 6 F # – ukázky](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
