---
title: Souhrn kapitoly 23. Triggery a chování
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 23. Triggery a chování'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 83a445555f9f184f735c105370de20665ad704a3
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156753"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Souhrn kapitoly 23. Triggery a chování

Triggery a chování jsou podobné, v tom, že oba jsou určena pro použití v souborech XAML pro zjednodušení interakce element nad rámec použití datových vazeb a rozšířit funkce elementů XAML. Triggery a chování jsou téměř vždy používá s objekty visual uživatelského rozhraní.

Pro podporu triggery a chování, obě `VisualElement` a `Style` podporují dvě vlastnosti kolekce:

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) a [ `Style.Triggers` ](xref:Xamarin.Forms.Style.Triggers) typu `IList<TriggerBase>`
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) a [ `Style.Behaviors` ](xref:Xamarin.Forms.Style.Behaviors) typu `IList<Behavior>`

## <a name="triggers"></a>Aktivační procedury

Aktivační události je podmínka (Změna vlastnosti nebo jeho spuštění události), která vede na odpověď (jiné vlastnosti změnit nebo nainstalováno nějaké). `Triggers` Vlastnost `VisualElement` a `Style` je typu `IList<TriggersBase>`. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) je abstraktní třída, ze které jsou odvozeny čtyři zapečetěné třídy:

- [`Trigger`](xref:Xamarin.Forms.Trigger) pro odpovědi na základě změn vlastností
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) pro odpovědi, které jsou založené na události firings
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) pro odpovědi, které jsou založené na datové vazby
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) pro odpovědi podle více aktivačních událostí

Aktivační události je vždycky nastavený na element, jehož vlastnost mění aktivační událost.

### <a name="the-simplest-trigger"></a>Nejjednodušší aktivační událost

[ `Trigger` ](xref:Xamarin.Forms.Trigger) Třídy kontroluje změny hodnoty vlastnosti a reaguje tak, že nastavíte jiné vlastnosti stejného elementu.

`Trigger` definuje tři vlastnosti:

- [`Property`](xref:Xamarin.Forms.Trigger.Property) typu `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) typu `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) typ `IList<SetterBase>`, vlastnost obsahu `Trigger`

Kromě toho `Trigger` vyžaduje, že následující vlastnost dědí z `TriggerBase` nastavit:

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType) označuje typ prvku, na kterém `Trigger` je připojen

`Property` a `Value` tvoří podmínka a `Setters` kolekce je tato odpověď. Když na zadaný `Property` má hodnotu `Value`, pak bude `Setter` objekty v `Setters` kolekce jsou použity. Když `Property` má jinou hodnotu, odeberou se funkce setter. `Setter` definuje dvě vlastnosti, které jsou stejné jako první dvě vlastnosti `Trigger`:

- [`Property`](xref:Xamarin.Forms.Setter.Property) typu `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) typu `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) Ukázka předvádí, jak `Trigger` u `Entry` můžete zvětšit velikost `Entry` prostřednictvím `Scale` vlastnost při `IsFocused`vlastnost `Entry` je `true`.

I když není běžné, `Trigger` lze nastavit v kódu, jako [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) ukázce.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) Ukázka předvádí, jak `Trigger` je možné nastavit v `Style` má použít pro více `Entry` elementy.

### <a name="trigger-actions-and-animations"></a>Aktivační událost akce a animace

Je také možné spouštět malé množství kódu v závislosti na triggeru. Tento kód může být animace, který se zaměřuje vlastnost. Jedním z běžných způsobů je použít [ `EventTrigger` ](xref:Xamarin.Forms.EventTrigger), která definuje dvě vlastnosti:

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) typ `string`, název události
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) typ `IList<TriggerAction>`, seznam akcí pro spuštění v odpovědi.

K tomu budete muset napsat třídu, která je odvozena z [ `TriggerAction<T>` ](xref:Xamarin.Forms.TriggerAction`1), obecně `TriggerAction<VisualElement>`. Definujte vlastnosti této třídy. Tyto jsou jednoduché vlastnosti CLR, nikoli vlastnosti umožňující vazbu, protože `TriggerAction` není odvozen od `BindableObject`. Je nutné přepsat [ `Invoke` ](xref:Xamarin.Forms.TriggerAction`1.Invoke*) metodu, která je volána při vyvolání akce. Argument je cílového prvku.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna je příklad. Volá `ScaleTo` vlastností pro animaci `Scale` vlastnost elementu. Protože jedna z jeho vlastností je typu `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) třída umožňuje použít standardní `Easing` statická pole v XAML.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) Ukázka předvádí, jak vyvolat `ScaleAction` z `EventTrigger` objekty tohoto monitorování `Focused` a `Unfocused` události.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) příklad ukazuje, jak definovat vlastní funkce usnadnění pro `ScaleAction` v souboru kódu na pozadí.

Můžete také vyvolat akce, `Trigger` (jako distinguished z `EventTrigger`). To vyžaduje, že jste si vědomi, který `TriggerBase` definuje dvě kolekce:

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) typu `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) typu `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) Ukázka předvádí, jak pomocí těchto kolekcí.

### <a name="more-event-triggers"></a>Více aktivačních událostí

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) volání knihovny `ScaleTo` dvakrát pro horizontální navýšení nebo snížení kapacity. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) Tato ukázka používá v Stylizovaný `EventTrigger` poskytnout vizuální zpětnou vazbu při `Button` stisknutí. Tato dvojitá animace je také možné pomocí dvou akcí v kolekci typu [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovna definuje přizpůsobitelné shiver akce. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) ukázce ho.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovny je omezen na `Entry` elementy a nastaví `TextColor` vlastnost červenou, když `Text` vlastnost není `double`. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) ukázce ho.

### <a name="data-triggers"></a>Aktivační události dat

[ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) Je podobný `Trigger` s tím rozdílem, že místo monitorování vlastnost pro změny hodnot, monitoruje datové vazby. To umožňuje vlastnost v jeden element ovlivňovat vlastnost v jiném elementu.

`DataTrigger` definuje tři vlastnosti:

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) typu `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) typu `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) typu `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) ukázka vyžaduje [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) knihoven a sad barvy jména studentů na modrou nebo na základě růžová `Sex` vlastnost:

[![Trojitá snímek pohlaví barvy](images/ch23fg04-small.png "pohlaví barvy")](images/ch23fg04-large.png#lightbox "pohlaví barvy")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) ukázkové sady `IsEnabled` vlastnost `Entry` k `False` Pokud `Length` vlastnost `Text` vlastnost `Entry`rovná 0. Všimněte si, že `Text` vlastnost je inicializována na prázdný řetězec, ve výchozím nastavení je `null`a `DataTrigger` nebude fungovat správně.

### <a name="combining-conditions-in-the-multitrigger"></a>Kombinace podmínek MultiTrigger

[ `MultiTrigger` ](xref:Xamarin.Forms.MultiTrigger) Je kolekce podmínek. Když jsou všechny `true`, pak se použijí metody setter. Třída definuje dvě vlastnosti:

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) typu `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) typu `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) je abstraktní třída a má dvě podřízené třídy:

- [`PropertyCondition`](xref:Xamarin.Forms.Condition), který má [ `Property` ](xref:Xamarin.Forms.PropertyCondition.Property) a [ `Value` ](xref:Xamarin.Forms.PropertyCondition.Value) vlastnosti, jako je `Trigger`
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition), který má [ `Binding` ](xref:Xamarin.Forms.BindingCondition.Binding) a [ `Value` ](xref:Xamarin.Forms.BindingCondition.Value) vlastnosti, jako je `DataTrigger`

V [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) ukázce `BoxView` jsou zobrazeny pouze při čtyři `Switch` prvky jsou všechny zapnuty.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) Ukázka předvádí, jak můžete `BoxView` barvu při *jakékoli* ze čtyř `Switch` prvky jsou zapnuté. To vyžaduje, aby aplikace De Morgan práva a vrácení veškerou logiku.

Kombinování a a nebo logiky není tak snadno a obvykle vyžaduje neviditelné `Switch` prvky pro průběžné výsledky. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) Ukázka předvádí, jak `Button` lze povolit, pokud dva `Entry` elementy mají nějaký text zadali, ale ne v případě, že obě mají nějaký text zadaný v.

## <a name="behaviors"></a>Chování

Všechno můžete dělat s triggerem, můžete také provést pomocí chování, ale chování vždy vyžadují třídu, která je odvozena z [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) a přepíše těchto dvou metod:

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

Argument je element připojený k chování. Obecně platí `OnAttachedTo` metoda připojí některé obslužné rutiny událostí a `OnDetachingFrom` odpojí je. Protože takové třídy obvykle ukládá některé stav, obecně nemůže být sdílen v `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) vzorek je podobný **TriggerEntryValidation** s tím rozdílem, že používá chování &mdash; [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

### <a name="behaviors-with-properties"></a>Chování s vlastnostmi

`Behavior<T>` je odvozen od `Behavior`, která je odvozena z `BindableObject`, takže může být definované s možností vazby vlastnosti chování. Tyto vlastnosti mohou být aktivní v datové vazby.

To je patrné [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) využívání program, který [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) třídy v [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. `ValidEmailBehavior` má vlastnost podporující vazby jen pro čtení a slouží jako zdroj v datové vazby.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) Ukázka používá toto chování stejné zobrazíte jiný typ ukazatele, který signalizuje, že platnost e-mailovou adresu.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) vzorek je podobný předchozí ukázce. Používá ButtonGlide `DataTrigger` v kombinaci se tohoto chování.

### <a name="toggles-and-check-boxes"></a>Přepínačů a zaškrtávacích políček

Je možné k zapouzdření chování přepínací tlačítko ve třídě, jako [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny a pak definovat všechny vizuály pro přepínací tlačítko zcela v XAML.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) ukázkový používá `ToggleBehavior` s `DataTrigger` používat `Label` dva textové řetězce pro tento přepínač.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) příklad rozšiřuje tento koncept díky přepínání mezi dvěma `FormattedString` objekty.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovny je odvozena z `ContentView`, definuje `IsToggled` vlastnost a zahrnuje `ToggleBehavior` pro přepínač logika. To usnadňuje definovat přepínací tlačítko v XAML, jak je uvedeno ve [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) vzorku.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) zahrnuje [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) třídu odvozenou od `ToggleBase` a používá [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)třídy k sestavení kompletních přepínací tlačítko, která se podobá Xamarin.Forms `Switch`.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) v **Xamarin.FormsBook.Toolkit** poskytuje animaci, aby animovaný úroveň v [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)vzorku.

### <a name="responding-to-taps"></a>Reakce na odposlouchávání

Nevýhodou `EventTrigger` je, že nemůžete připojit ho k `TapGestureRecognizer` reagovat na odposlouchávání. Získání kolem tohoto problému je účelem [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) v [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) ukázkový používá `TapBehavior` používat dřívější `ShiverAction` pro klepnutí `BoxView` elementy.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) příklad ukazuje, jak omezit značky zapouzdřením `ShiverView` třídy.

### <a name="radio-buttons"></a>Přepínací tlačítka

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny má také [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) třídy pro vytváření přepínacích tlačítek, které jsou seskupeny podle `string` název skupiny.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) program používá řetězce pro jeho přepínač. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) ukázkový používá `Style` pro rozdíl mezi tlačítky zaškrtnuto a nezaškrtnuto vzhled. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) Ukázka používá zabalené obrázky pro jeho přepínače:

[![Trojitá snímek obrazovky s Imagí přepínač](images/ch23fg17-small.png "obrázky tlačítek přepínače")](images/ch23fg17-large.png#lightbox "obrázky tlačítek přepínače")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) ukázka nakreslí tradiční zobrazení přepínačů s tečkou v kruhu.

### <a name="fades-and-orientation"></a>Pomalu a orientace

Poslední vzorek [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) umožňuje přepínat mezi zobrazeními jiný výběr barvy pomocí přepínače. Tři zobrazení fade a odhlašování pomocí [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

Program také reaguje na změny v orientaci mezi na výšku a šířku [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) v **Xamarin.FormsBook.Toolkit** knihovny.



## <a name="related-links"></a>Související odkazy

- [Kapitola 23 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Ukázky kapitoly 23](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Práce s aktivačními procedurami](~/xamarin-forms/app-fundamentals/triggers.md)
- [Chování Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
