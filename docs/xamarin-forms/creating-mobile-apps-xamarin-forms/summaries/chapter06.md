---
title: Souhrn kapitola 6. Kliknutí na tlačítko
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 6. Kliknutí na tlačítko'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: 464fbdb043ac35eba7a4cc2d9ec76b78cc91ac5b
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156509"
---
# <a name="summary-of-chapter-6-button-clicks"></a>Souhrn kapitola 6. Kliknutí na tlačítko

[ `Button` ](xref:Xamarin.Forms.Button) Je zobrazení, které umožňuje uživateli spustit příkaz. A `Button` je identifikován podle textu (a volitelně obrázku jak je ukázáno v [Kapitola 13, bitmap](chapter13.md)). V důsledku toho `Button` definuje řadu stejné vlastnosti jako `Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` Definuje také tři vlastnosti, které řídí vzhled ohraničení, ale podporu těchto vlastností a jejich vzájemné nezávislost je specifický pro platformu:

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) typu `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) typu `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) typu `Double`

`Button` také dědí všechny vlastnosti `VisualElement` a `View`, včetně `BackgroundColor`, `HorizontalOptions`, a `VerticalOptions`.

## <a name="processing-the-click"></a>Kliknutím na zpracování

`Button` Definuje třídu [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) událost, která se aktivuje, když uživatel klepne `Button`. `Click` Obslužná rutina je typu `EventHandler`. První argument je `Button` objektu generování události; druhý argument je `EventArgs` objekt, který poskytuje žádné další informace.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) příklad ukazuje jednoduchý `Clicked` zpracování.

## <a name="sharing-button-clicks"></a>Sdílení tlačítka klikne

Více `Button` zobrazení můžou sdílet stejný `Clicked` obslužné rutiny, ale obslužnou rutinu obecně potřebuje k určení, které `Button` zodpovídá za konkrétní události. Jedním z přístupů je pro uložení různých `Button` objekty jako pole a kontroly, který se spouští v obslužné rutině události.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) ukázka demonstruje tento postup. Program také ukazuje, jak nastavit [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) vlastnost `Button` k `false` při stisknutí klávesy `Button` již není platný. A je zakázaná. `Button` negeneruje `Clicked` událostí.

## <a name="anonymous-event-handlers"></a>Obslužné rutiny událostí anonymní

Je možné definovat `Clicked` obslužnými rutinami jako anonymní lambda funkce, jako [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) ukázce. Anonymní obslužné rutiny však nelze sdílet bez kódu neuspořádaných reflexe.

## <a name="distinguishing-views-with-ids"></a>Rozlišování zobrazení s ID

Více `Button` objekty také dají rozlišovat podle nastavení [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) vlastnost nebo [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) vlastnost `string`. Tato vlastnost je definován `Element` , ale není použit v rámci Xamarin.Forms. Jeho účelem je používat výhradně aplikacemi.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) Ukázka používá stejný ovladač událostí pro všechny 10 čísel klíče na numerické klávesnici a rozlišuje mezi nimi pomocí `StyleId` vlastnost:

[![Trojitá snímek nejjednodušší klávesnici](images/ch06fg04-small.png "Kalkulačka")](images/ch06fg04-large.png#lightbox "Kalkulačka")

## <a name="saving-transient-data"></a>Ukládání přechodných dat

Mnoho aplikací se musí k ukládání dat, když program se ukončí a znovu načíst data při spuštění programu znovu. [ `Application` ](xref:Xamarin.Forms.Application) Třída definuje několik členů, které pomáhají program uložení a obnovení přechodných dat:

- [ `Properties` ](xref:Xamarin.Forms.Application.Properties) Vlastnost je slovník s `string` klíče a `object` položky. Obsah slovníku se automaticky uloží do místního úložiště aplikací před ukončením programu a znovu načíst při spuštění programu.
- `Application` Třída definuje tři chráněné virtuální metody, že je standardní program `App` třídy přepsání: [ `OnStart` ](xref:Xamarin.Forms.Application.OnStart), [ `OnSleep` ](xref:Xamarin.Forms.Application.OnSleep), a [ `OnResume` ](xref:Xamarin.Forms.Application.OnResume). Jde o *životního cyklu aplikací* události.
- [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) Metoda uloží obsah slovníku.

Není nutné volat `SavePropertiesAsync`. Obsah slovníku jsou automaticky uložen před ukončením programu a načíst před spuštěním programu. Je užitečná během testování aplikace k ukládání dat, pokud dojde k chybě programu.

Užitečné je také:

- [`Application.Current`](xref:Xamarin.Forms.Application.Current), statickou vlastnost, která vrátí aktuální `Application` objekt, který potom slouží k získání `Properties` slovníku.

Prvním krokem je identifikace všechny proměnné na stránce, které chcete zachovat, když program skončí. Pokud znáte všechna místa, kde tyto proměnné změnit, můžete jednoduše přidat je `Properties` slovníku v tomto okamžiku. V konstruktoru na stránce můžete nastavit proměnné z `Properties` slovníku existuje-li klíč.

Rozsáhlejšího programu bude pravděpodobně muset zabývat události životního cyklu aplikace. Nejdůležitější je `OnSleep` metody. Volání této metody Určuje, že program opustil popředí. Například uživatel stiskne **Domů** tlačítko na zařízení, zobrazí všechny aplikace nebo Probíhá ukončení telefonu. Volání `OnSleep` je jenom oznámení, že program přijímá předtím, než se ukončí. Program by měl využijte této příležitosti a ujistěte se, `Properties` slovník je aktuální.

Volání `OnResume` označuje, že program nebyl ukončen následující po posledním volání do `OnSleep` , ale je teď spuštěná v popředí, se znovu. Program může využít tuto příležitost k aktualizaci připojení k Internetu (třeba).

Volání `OnStart` dochází při spuštění programu. Není nutné čekat na volání této metody pro přístup k `Properties` slovníku vzhledem k tomu, že již byly obsah obnovit, kdy `App` volání konstruktoru.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) ukázka je velmi podobný **SimplestKeypad** s tím rozdílem, že program použije `OnSleep` přepsání uložit s aktuální položkou klávesnici a Konstruktor stránky pro tato data obnovit.

> [!NOTE]
> Dalším přístupem k ukládání nastavení programu je poskytován Xamarin.Essentials [Předvolby](~/essentials/preferences.md) třídy.

## <a name="related-links"></a>Související odkazy

- [Kapitola 6 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Ukázky kapitola 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Ukázky kapitola 6 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Tlačítko Xamarin.Forms](~/xamarin-forms/user-interface/button.md)
