---
title: Nastavení hodnot usnadnění na elementy uživatelského rozhraní
description: Xamarin.Forms umožňuje usnadnění hodnoty nastavení na elementy uživatelského rozhraní pomocí přidružené vlastnosti ze třídy AutomationProperties, které se v změní hodnoty nativní usnadnění přístupu sady. Tento článek vysvětluje, jak používat třídu AutomationProperties tak, aby čtečky obrazovky můžete prodiskutovat elementy na stránce.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: cf9071684061b584e1cb75cfd50b33212f42bf79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Nastavení hodnot usnadnění na elementy uživatelského rozhraní

_Xamarin.Forms umožňuje usnadnění hodnoty nastavení na elementy uživatelského rozhraní pomocí přidružené vlastnosti ze třídy AutomationProperties, které se v změní hodnoty nativní usnadnění přístupu sady. Tento článek vysvětluje, jak používat třídu AutomationProperties tak, aby čtečky obrazovky můžete prodiskutovat elementy na stránce._

## <a name="overview"></a>Přehled

Xamarin.Forms umožňuje usnadnění hodnoty nastavení na elementy uživatelského rozhraní pomocí následující přidružené vlastnosti:

- `AutomationProperties.IsInAccessibleTree` – Určuje, zda je k dispozici pro aplikaci přístupné elementu. Další informace najdete v tématu [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – Stručný popis elementu, který slouží jako speakable identifikátor elementu. Další informace najdete v tématu [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – delší popis element, který lze chápat jako text popisu tlačítka, které jsou přidružené k tomuto prvku. Další informace najdete v tématu [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – Umožňuje nastavit jiný element definovat informace o usnadnění pro aktuální element. Další informace najdete v tématu [AutomationProperties.LabeledBy](#labeledby).

Tyto připojené vlastnosti nastavené nativní usnadnění hodnoty tak, aby čtečky obrazovky můžete prodiskutovat elementu. Další informace o přidružené vlastnosti najdete v tématu [připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Pomocí `AutomationProperties` přidružené vlastnosti může mít vliv na spuštění testu uživatelského rozhraní v systému Android. [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` a `AutomationProperties.HelpText` vlastnosti se nastavit nativního `ContentDescription` vlastnost s `AutomationProperties.Name` a `AutomationProperties.HelpText` hodnoty vlastností přednost před `AutomationId`hodnota (pokud obě `AutomationProperties.Name` a `AutomationProperties.HelpText` jsou nastavené hodnoty budou být zřetězen). To znamená, že všechny testy hledá `AutomationId` se nezdaří, pokud `AutomationProperties.Name` nebo `AutomationProperties.HelpText` jsou nastaveny také v elementu. V tomto scénáři, mělo by být změněno testů uživatelského rozhraní a hledat v hodnotě `AutomationProperties.Name` nebo `AutomationProperties.HelpText`, nebo zřetězení těchto dvou možností.

Každá platforma má čtečka různých obrazovek komentovat hodnoty usnadnění přístupu:

- iOS má VoiceOver. Další informace najdete v tématu [usnadnění Test na vaše zařízení s VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) na developer.apple.com.
- Android má TalkBack. Další informace najdete v tématu [testování aplikace pro usnadnění](https://developer.android.com/training/accessibility/testing.html#talkback) na developer.android.com.
- Windows has Narrator. Další informace najdete v tématu [ověřit scénáře hlavní aplikace pomocí Předčítání](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Přesné chování čtečky obrazovky však závisí na software a konfigurace uživatele je. Například většina čtečky obrazovky přečtěte si text související s ovládacím prvkem, pokud obdrží fokus, tím, že uživatelům orientaci přechází mezi ovládacími prvky na stránce. Uživatelské rozhraní celá aplikace si také přečíst některé čtečky obrazovky při, zobrazí se stránka, která umožňuje uživatelům přijímat všechny stránky k dispozici informační obsahu před pokusem o jeho přejděte.

Čtečky obrazovky také načíst hodnoty různých usnadnění. V ukázkové aplikaci:

- Budou číst voiceOver `Placeholder` hodnotu `Entry`, za nímž následují pokyny pro použití ovládacího prvku.
- Budou číst talkBack `Placeholder` hodnotu `Entry`, za nímž následují `AutomationProperties.HelpText` hodnotu následovanou pokyny pro použití ovládacího prvku.
- Program Předčítání, bude číst `AutomationProperties.LabeledBy` hodnotu `Entry`, za nímž následují pokyny týkající se použití ovládacího prvku.

Kromě toho bude preferovat Předčítání `AutomationProperties.Name`, `AutomationProperties.LabeledBy`a potom `AutomationProperties.HelpText`. V systému Android se může kombinovat TalkBack `AutomationProperties.Name` a `AutomationProperties.HelpText` hodnoty. Proto se doporučuje, aby důkladné usnadnění testování se provede na každou platformu zajistit optimální zkušenosti.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` Je přidružená vlastnost `boolean` který určuje, pokud se element nachází přístupné a proto viditelné pro čtečky obrazovky. Musí být nastavena na `true` používat další usnadnění přidružené vlastnosti. Můžete to provést v jazyce XAML následujícím způsobem:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Případně se může být nastavena v C# následujícím způsobem:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody lze také nastavit `AutomationProperties.IsInAccessibleTree` přidružená vlastnost – `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` Přidružená vlastnost hodnota by měla být krátký a popisný textový řetězec, který používá čtečky obrazovky oznamujeme elementu. Tato vlastnost musí být nastavená u elementů, které mají význam, který je důležité pro pochopení obsahu nebo interakci s uživatelským rozhraním. Můžete to provést v jazyce XAML následujícím způsobem:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Případně se může být nastavena v C# následujícím způsobem:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody lze také nastavit `AutomationProperties.Name` přidružená vlastnost – `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` Připojené, musí být vlastnost nastavena na hodnotu text, který popisuje element uživatelského rozhraní a může být myšlenku z jako text popisku přidružené elementu. Můžete to provést v jazyce XAML následujícím způsobem:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Případně se může být nastavena v C# následujícím způsobem:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody lze také nastavit `AutomationProperties.HelpText` přidružená vlastnost – `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Na některých platformách pro úpravy ovládací prvky jako například [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), `HelpText` vlastnost někdy může tento parametr vynechán a nahradí zástupný text. Například "zadejte zde název" je vhodným kandidátem [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) vlastnost, která umístí text v ovládacím prvku před skutečným vstup uživatele.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` Přidružená vlastnost umožňuje jiný element můžete definovat informace o usnadnění pro aktuální element. Například [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) vedle [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) slouží k popište, co `Entry` představuje. Můžete to provést v jazyce XAML následujícím způsobem:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Případně se může být nastavena v C# následujícím způsobem:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody lze také nastavit `AutomationProperties.IsInAccessibleTree` přidružená vlastnost – `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Souhrn

Jak nastavit usnadnění hodnoty na uživatelské rozhraní elementy v aplikaci Xamarin.Forms pomocí přidružené vlastnosti z popsané v tomto článku `AutomationProperties` třídy. Tyto připojené vlastnosti nastavené nativní usnadnění hodnoty tak, aby čtečky obrazovky můžete prodiskutovat elementy na stránce.


## <a name="related-links"></a>Související odkazy

- [Připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md)
- [Usnadnění přístupu (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
