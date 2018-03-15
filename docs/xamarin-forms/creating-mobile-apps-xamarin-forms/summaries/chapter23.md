---
title: "Shrnutí kapitoly 23. Aktivační události a chování"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ddb76e00cfe1c19a9d31dc3e53b80a2be0697dbc
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Shrnutí kapitoly 23. Aktivační události a chování

Aktivační události a chování jsou podobné, v tom, že obě jsou určena pro použití v souborů XAML pro zjednodušení interakce element nad rámec použití vazby dat a rozšířit funkce elementů XAML. Aktivační události a chování jsou téměř vždy použít s objekty visual uživatelského rozhraní.

Pro podporu aktivační události a chování, jak `VisualElement` a `Style` podporovat dvě vlastnosti v kolekci:

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) a [ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/) typu `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) a [ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/) typu `IList<Behavior>`

## <a name="triggers"></a>Aktivační procedury

Aktivační událost je stav (změnu vlastnosti nebo pálení události), který nastane v odpovědi (jiné změnu vlastnosti nebo spuštění některých kódu). `Triggers` Vlastnost `VisualElement` a `Style` je typu `IList<TriggersBase>`. [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) je abstraktní třída, ze které jsou odvozeny čtyři zapečetěné třídy:

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) pro odpovědi na základě změn vlastností
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) pro odpovědi, které jsou založené na události firings
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) pro odpovědi na základě dat vazby
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) pro odpovědi, které jsou založené na více aktivačních událostí

Aktivační události je vždycky nastavený na element, jehož vlastnost došlo ke změně má aktivační procedura.

### <a name="the-simplest-trigger"></a>Nejjednodušší aktivační události

[ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) Třída zkontroluje změnu hodnotu vlastnosti a odpovídá nastavením jinou vlastnost stejného elementu.

`Trigger` definuje tři vlastnosti:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) typu `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) typu `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) typu `IList<SetterBase>`, vlastnost obsahu `Trigger`

Kromě toho `Trigger` vyžaduje, aby následující vlastnost zděděno z `TriggerBase` nastavit:

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) k označení typ elementu, na kterém `Trigger` je připojen

`Property` a `Value` tvoří podmínku a `Setters` kolekce je odpovědi. Když uvedené `Property` má hodnotu indikován `Value`, pak se `Setter` objekty v `Setters` kolekce se používají. Když `Property` má jinou hodnotu, jsou odebrány setter. `Setter` definuje dvě vlastnosti, které jsou stejné jako první dvě vlastnosti `Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) typu `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) typu `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) příklad znázorňuje jak `Trigger` použít `Entry` můžete zvětšit velikost `Entry` prostřednictvím `Scale` vlastnost při `IsFocused`vlastnost `Entry` je `true`.

I když není běžné, `Trigger` lze nastavit v kódu, jako [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) příklad znázorňuje.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) příklad znázorňuje jak `Trigger` lze nastavit v `Style` chcete použít pro více `Entry` elementy.

### <a name="trigger-actions-and-animations"></a>Akce aktivační události a animací

Je také možné spustit ještě kód v závislosti na triggeru. Tento kód může být animace zacílený vlastnost. Jeden běžným způsobem je použít [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/), která definuje dvě vlastnosti:

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) typu `string`, název události
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) typu `IList<TriggerAction>`, seznam akce pro spuštění v odpovědi.

Využít, potřebujete napsat třídu odvozenou z [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/), obecně `TriggerAction<VisualElement>`. V této třídě můžete definovat vlastnosti. Toto jsou prostý vlastnosti CLR nikoli vazbu vlastnosti, protože `TriggerAction` není odvozena od `BindableObject`. Je nutné přepsat [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/) metoda, která je volána, když je vyvolána akce. Argument je cílový element.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna je příklad. Zavolá `ScaleTo` vlastností pro animaci `Scale` vlastnost elementu. Protože jedna z vlastností typu `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) třída umožňuje použít standardní `Easing` statických polí v jazyce XAML.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) ukázka ukazuje, jak má být vyvolán `ScaleAction` z `EventTrigger` objekty tohoto monitoru `Focused` a `Unfocused` události.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) příklad ukazuje, jak definovat vlastní nejvýraznější funkcí pro `ScaleAction` v souboru kódu na pozadí.

Můžete také vyvolat akce s použitím `Trigger` (jako distinguished z `EventTrigger`). To vyžaduje, že víte, `TriggerBase` definuje dvě kolekce:

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) typu `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) typu `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) příklad ukazuje způsob použití těchto kolekcí.

### <a name="more-event-triggers"></a>Více aktivačních událostí

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) volání knihovny `ScaleTo` dvakrát se škálovat nahoru a dolů. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) ukázce se používá tato v stylem `EventTrigger` k poskytnutí zpětné vazby visual při `Button` stisknutí. Tato dvojité animace je také možné pomocí dvě akce v kolekci typu [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovny definuje přizpůsobitelné shiver akce. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) příklad znázorňuje ho.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovna je omezen na `Entry` elementy a nastaví `TextColor` vlastnost red, když `Text` vlastnost není `double`. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) příklad znázorňuje ho.

### <a name="data-triggers"></a>Aktivační události dat

[ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) Je podobná `Trigger` s tím rozdílem, že místo monitorování vlastnost na hodnotu změny, sleduje datová vazba. Tato vlastnost umožňuje v jeden element ovlivnit vlastnost v elementu jiného.

`DataTrigger` definuje tři vlastnosti:

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) typu `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) typu `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) typu `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) ukázka vyžaduje [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) knihovny a nastaví barvy názvy studentů modrá nebo na základě růžová `Sex` vlastnost:

[![Trojitá snímek obrazovky pohlaví barvy](images/ch23fg04-small.png "pohlaví barvy")](images/ch23fg04-large.png#lightbox "pohlaví barvy")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) ukázkové sady `IsEnabled` vlastnost `Entry` k `False` Pokud `Length` vlastnost `Text` vlastnost `Entry`rovná 0. Všimněte si, že `Text` vlastnost inicializovala na prázdný řetězec, ve výchozím nastavení je `null`a `DataTrigger` nebude fungovat správně.

### <a name="combining-conditions-in-the-multitrigger"></a>Kombinování podmínky v MultiTrigger

[ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) Je kolekce podmínky. Když jsou všechny `true`, pak se použijí setter. Třída definuje dvě vlastnosti:

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) typu `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) typu `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) je abstraktní třída a má dva podřízené třídy:

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/), který má [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/) a [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/) vlastnosti, například `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/), který má [ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/) a [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/) vlastnosti, například `DataTrigger`

V [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) ukázce `BoxView` je pouze barevný při čtyři `Switch` elementy se zapnou při.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) příklad ukazuje, jak provést `BoxView` barvu, která při *žádné* čtyři `Switch` elementy jsou zapnuté. To vyžaduje De Nováková práva a Prohodit veškerou logiku aplikace.

Kombinování a a nebo logiku není tak snadno a obvykle vyžaduje neviditelná `Switch` prvky pro mezilehlých výsledků. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) příklad znázorňuje jak `Button` dají povolit, pokud buď dva `Entry` prvky mít některé text zadaný v, ale není v případě, že oba mají nějaký text zadali.

## <a name="behaviors"></a>Chování

Nic můžete provést s aktivační událost, můžete provést také s chování, ale chování vždy vyžadují třídu odvozenou z [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) a přepíše následující dvě metody:

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

Argument je elementu, který chování je připojen k. Obecně platí `OnAttachedTo` metoda připojí některé obslužné rutiny událostí a `OnDetachingFrom` je odpojí. Protože tyto třídy obvykle uloží některé stav, se obecně nelze sdílet v `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) ukázka je podobná **TriggerEntryValidation** s tím rozdílem, že používá chování &mdash; [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

### <a name="behaviors-with-properties"></a>Chování vlastnostmi

`Behavior<T>` odvozená z `Behavior`, která je odvozena z `BindableObject`, takže lze definovat vazbu vlastnosti na chování. Tyto vlastnosti můžou být aktivní v datové vazby.

Tento postup je znázorněn v [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) použít program, který [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) třídy v [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. `ValidEmailBehavior` má vlastnost vázat jen pro čtení a slouží jako zdroj v datové vazby.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) ukázce se používá tato stejné chování zobrazíte jiný typ ukazatele signál, že je platné e-mailovou adresu.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) ukázka je variace v předchozím příkladu. Používá ButtonGlide `DataTrigger` v kombinaci s daná chování.

### <a name="toggles-and-check-boxes"></a>Přepínačů a zaškrtávací políčka

Je možné, jako zapouzdření chování přepínací tlačítka v třídě [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny a pak zadejte všechny vizuální prvky pro přepínání zcela v jazyce XAML.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) ukázkové používá `ToggleBehavior` s `DataTrigger` sloužící `Label` s dva textové řetězce pro přepínač.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) ukázka rozšiřuje tento koncept o přepínání mezi dvěma `FormattedString` objekty.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovna je odvozena z `ContentView`, definuje `IsToggled` vlastnost a zahrnuje `ToggleBehavior` pro přepínač logika. To usnadňuje definovat přepínací tlačítko v jazyce XAML, jak je předvedeno pomocí [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) ukázka.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) zahrnuje [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) třídu odvozenou od `ToggleBase` a používá [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)třída vytvořit přepínací tlačítko, která se podobá platformě Xamarin.Forms `Switch`.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) v **Xamarin.FormsBook.Toolkit** poskytuje animace používají k vytvoření animovaný úroveň v [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)ukázka.

### <a name="responding-to-taps"></a>Neodpovídá na požadavky odposlouchávání

Nevýhodou `EventTrigger` je, že nemůžete připojit jej do `TapGestureRecognizer` reagovat na odposlouchávání. Získání chcete tento problém vyřešit, je účelem [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) v [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) ukázkové používá `TapBehavior` použít dříve `ShiverAction` pro stisknuté `BoxView` elementy.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) příklad ukazuje, jak omezit kód zapouzdřením `ShiverView` třídy.

### <a name="radio-buttons"></a>Přepínací tlačítka

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny má také [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) třídu pro provedení přepínače, které jsou seskupené podle `string` název skupiny.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) program používá textové řetězce pro jeho přepínač. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) ukázkové používá `Style` pro rozdíl vzhled mezi tlačítky zaškrtnuto a nezaškrtnuto. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) Ukázka používá pevně určené bitové kopie pro jeho přepínače:

[![Trojitá snímek obrazovky přepínač image](images/ch23fg17-small.png "obrázky přepínacích tlačítek")](images/ch23fg17-large.png#lightbox "obrázky přepínacích tlačítek")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) ukázka nevykresluje tradiční vypadající přepínačů s tečkou uvnitř kruh.

### <a name="fades-and-orientation"></a>Stmívačkami a orientace

Konečný vzorek [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) přepínat mezi tři zobrazení jiný výběr barev pomocí přepínače. Tři zobrazení objevovat a odhlašování pomocí [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

Program také reaguje na změny v orientaci mezi na výšku a šířku pomocí [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) v **Xamarin.FormsBook.Toolkit** knihovny.



## <a name="related-links"></a>Související odkazy

- [Úplný text 23 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Ukázky kapitoly 23](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Práce s aktivační události](~/xamarin-forms/app-fundamentals/triggers.md)
- [Chování Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
