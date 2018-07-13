---
title: Nastavení hodnot přístupnosti pro prvky uživatelského rozhraní
description: Tento článek vysvětluje, jak použít třídu AutomationProperties tak, aby se čtečkou obrazovky mohou mluvit o elementy na stránce.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 576fe34b0536c83825aa39bdd7111143d9474225
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998907"
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Nastavení hodnot přístupnosti pro prvky uživatelského rozhraní

_Xamarin.Forms umožňuje hodnot přístupnosti nastavení na prvky uživatelského rozhraní pomocí připojené vlastnosti ze třídy vlastnosti automatizace, které následně nastavte nativní usnadnění hodnoty. Tento článek vysvětluje, jak použít třídu AutomationProperties tak, aby se čtečkou obrazovky mohou mluvit o elementy na stránce._

## <a name="overview"></a>Přehled

Xamarin.Forms umožňuje hodnot přístupnosti nastavení na prvky uživatelského rozhraní pomocí následující připojené vlastnosti:

- `AutomationProperties.IsInAccessibleTree` – Určuje, zda elementu je k dispozici pro aplikace přístupné. Další informace najdete v tématu [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – Stručný popis prvku, který slouží jako speakable identifikátor elementu. Další informace najdete v tématu [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – delší popis prvku, který si lze představit jako text popisu tlačítka, které jsou spojené s tímto prvkem. Další informace najdete v tématu [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – Umožňuje definovat informace o usnadnění pro aktuální prvek jiný element. Další informace najdete v tématu [AutomationProperties.LabeledBy](#labeledby).

Tyto připojené vlastnosti nastavte nativní usnadnění hodnoty tak, aby se čtečkou obrazovky mohou mluvit o elementu. Další informace o přidružené vlastnosti najdete v tématu [připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Použití `AutomationProperties` připojené vlastnosti může mít vliv na provádění testů uživatelského rozhraní v systému Android. [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` a `AutomationProperties.HelpText` vlastnosti budou nastaveny nativní `ContentDescription` vlastnost, s `AutomationProperties.Name` a `AutomationProperties.HelpText` hodnoty vlastností přednost `AutomationId`hodnotu (pokud mají oba `AutomationProperties.Name` a `AutomationProperties.HelpText` je nastaveno, bude zřetězení hodnoty). To znamená, že všechny testy, které hledáte `AutomationId` selže, pokud `AutomationProperties.Name` nebo `AutomationProperties.HelpText` jsou také nastaven pro element. V tomto scénáři, mělo by být změněno testy uživatelského rozhraní k vyhledání hodnoty `AutomationProperties.Name` nebo `AutomationProperties.HelpText`, nebo zřetězení obou.

Každá platforma má jiné obrazovky k vyprávění příběhu hodnot přístupnosti:

- iOS je VoiceOver. Další informace najdete v tématu [Test přístupnosti na vaše zařízení s VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) na developer.apple.com.
- Android má možnost TalkBack. Další informace najdete v tématu [testování vaší aplikace usnadnění](https://developer.android.com/training/accessibility/testing.html#talkback) na developer.android.com.
- Windows má Předčítání. Další informace najdete v tématu [ověřit scénáře hlavní aplikace pomocí programu Předčítání](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Přesné chování čtečky obrazovky, ale závisí na software a na konfiguraci uživatele je. Například většina čtečky obrazovky číst text související s ovládacím prvkem, když získá fokus, povolíte uživatelům orientaci při přechodu mezi ovládacími prvky na stránce. Některé čtečky obrazovky si také přečíst celé aplikace uživatelského rozhraní při, zobrazí se stránka, která umožňuje přijímat všechny stránky k dispozici informativní obsah před pokusem ho můžete procházet.

Čtečky obrazovky také číst hodnoty různou přístupností. V ukázkové aplikaci:

- Přečte voiceOver `Placeholder` hodnotu `Entry`následovaný pokyny, jak pomocí ovládacího prvku.
- Přečte talkBack `Placeholder` hodnotu `Entry`následovaný `AutomationProperties.HelpText` hodnotu, za nímž následuje pokyny, jak pomocí ovládacího prvku.
- Program Předčítání bude číst `AutomationProperties.LabeledBy` hodnotu `Entry`následovaný pokyny, pomocí ovládacího prvku.

Kromě toho bude program Předčítání preferovat `AutomationProperties.Name`, `AutomationProperties.LabeledBy`a potom `AutomationProperties.HelpText`. V systému Android, může kombinovat TalkBack `AutomationProperties.Name` a `AutomationProperties.HelpText` hodnoty. Doporučujeme proto, že usnadnění důkladné testování provádí na jednotlivých platformách zajistit optimální výkon.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` Je připojená vlastnost `boolean` , která určuje, zda elementu je dostupná a proto viditelné pro čtečky obrazovky. Musí být nastaveno na `true` používat další usnadnění připojené vlastnosti. To lze provést v XAML následujícím způsobem:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Případně ho lze nastavit v jazyce C# následujícím způsobem:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody lze také nastavit `AutomationProperties.IsInAccessibleTree` přidružená vlastnost – `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` Hodnotu přidružené vlastnosti by měla být krátký a popisný textový řetězec, který používá čtečky obrazovky oznamujeme elementu. Tuto vlastnost měli nastavit pro prvky, které mají význam, které jsou důležité pro pochopení obsahu nebo interakci s uživatelským rozhraním. To lze provést v XAML následujícím způsobem:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Případně ho lze nastavit v jazyce C# následujícím způsobem:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody lze také nastavit `AutomationProperties.Name` přidružená vlastnost – `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` Připojená vlastnost by měla být nastavena na text, který popisuje prvku uživatelského rozhraní a můžete být myšlenky o jako text popisku spojené s tímto prvkem. To lze provést v XAML následujícím způsobem:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Případně ho lze nastavit v jazyce C# následujícím způsobem:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody lze také nastavit `AutomationProperties.HelpText` přidružená vlastnost – `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Na některých platformách pro úpravy ovládací prvky jako například [ `Entry` ](xref:Xamarin.Forms.Entry), `HelpText` vlastnost někdy můžete vynechat a nahradit zástupný text. Například "zadejte sem patří vaše jméno", je vhodným kandidátem pro [ `Entry.Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) vlastnost, která umístí text v ovládacím prvku před skutečné vstup uživatele.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` Připojená vlastnost umožňuje jiný element k definování informací o usnadnění pro aktuální element. Například [ `Label` ](xref:Xamarin.Forms.Label) vedle [ `Entry` ](xref:Xamarin.Forms.Entry) slouží k popište, co `Entry` představuje. To lze provést v XAML následujícím způsobem:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Případně ho lze nastavit v jazyce C# následujícím způsobem:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Všimněte si, že [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody lze také nastavit `AutomationProperties.IsInAccessibleTree` přidružená vlastnost – `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak nastavení hodnot přístupnosti pro uživatele prvky rozhraní aplikace Xamarin.Forms s použitím připojené vlastnosti z `AutomationProperties` třídy. Tyto připojené vlastnosti nastavte nativní usnadnění hodnoty tak, aby se čtečkou obrazovky mohou mluvit o elementy na stránce.


## <a name="related-links"></a>Související odkazy

- [Připojené vlastnosti](~/xamarin-forms/xaml/attached-properties.md)
- [Pro usnadnění (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
