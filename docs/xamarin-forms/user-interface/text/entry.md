---
title: Položka Xamarin.Forms
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms položky tak, aby přijímal jedním řádkem textu nebo zadávání hesla v aplikaci.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 95afdfde878759d4a598e200d16fe6fb1fa2005e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998228"
---
# <a name="xamarinforms-entry"></a>Položka Xamarin.Forms

_Jedním řádkem textu nebo zadejte heslo_

Xamarin.Forms `Entry` se používá pro vstup jedním řádkem textu. `Entry`, Třeba `Editor` zobrazení, podporuje více typů klávesnice. Kromě toho `Entry` může sloužit jako pole pro heslo.

## <a name="display-customization"></a>Přizpůsobení zobrazení

### <a name="setting-and-reading-text"></a>Nastavení a čtení textu

`Entry`, Stejně jako ostatní zobrazení nabízí ten samý text zpřístupňuje `Text` vlastnost. Tuto vlastnost lze použít k nastavení a přečtěte si text na předložený `Entry`. Následující příklad ukazuje nastavení `Text` vlastnost v XAML:

```xaml
<Entry Text="I am an Entry" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Čtení textu, přístup `Text` vlastností v jazyce C#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Šířka `Entry` lze definovat tak, že nastavíte její `WidthRequest` vlastnost. Není závislý na šířku `Entry` definuje na základě hodnoty z jeho `Text` vlastnost.

### <a name="limiting-input-length"></a>Omezit délku vstupu

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Vlastnost lze použít k omezení, který je povolený pro vstupní délka [ `Entry` ](xref:Xamarin.Forms.Entry). Tuto vlastnost měli nastavit na kladné celé číslo:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) vlastnost hodnota 0 znamená, že žádný vstup bude možné a hodnotu `int.MaxValue`, což je výchozí hodnota pro [ `Entry` ](xref:Xamarin.Forms.Entry), označuje, že je žádný platit omezení počtu znaků, které mohou být zadány.

### <a name="keyboards"></a>Klávesnice

Klávesnice, který se zobrazí, když uživatelé možnost zasahovat `Entry` lze programově nastavit prostřednictvím `Keyboard` vlastnost.

Možnosti pro typ klávesnice jsou:

- **Výchozí** &ndash; výchozí klávesnice
- **Chat** &ndash; použít pro odesílání textových zpráv a místa kde jsou užitečné emoji
- **E-mailu** &ndash; použít při zadávání e-mailové adresy
- **Číselné** &ndash; při zadání čísel
- **Telefonní** &ndash; použít při zadávání telefonní čísla
- **Adresa URL** &ndash; používá pro zadání cesty k souborům a webové adresy

Je [příklad každý klávesnice](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) v oddílu recepty.

### <a name="enabling-and-disabling-spell-checking"></a>Povolení a zakázání kontroly pravopisu

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Ovládací prvky vlastnost určuje, zda kontrola pravopisu je povolená. Ve výchozím nastavení je vlastnost nastavena na `true`. Jako uživatel zadá text, jsou uvedeny pravopisné chyby.

Však pro některé scénáře vstupního textu, jako je zadání uživatelského jména, kontrolu pravopisu poskytuje negativní zkušenosti a proto by mělo být zakázáno nastavením [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) vlastnost `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Když [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) je nastavena na `false`a se nepoužívá vlastní klávesnice, nástroj pro kontrolu pravopisu nativní se deaktivuje. Ale pokud [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) má byla sada, která zakáže pravopisu kontrolu, například [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` vlastnost se ignoruje. Proto nelze vlastnost použít k povolení kontroly pravopisu pro `Keyboard` zakazující explicitně.

### <a name="placeholders"></a>Zástupné symboly

`Entry` je možné nastavit na Zobrazit zástupný text při neukládá uživatelský vstup. V praxi to je často zobrazena ve formulářích upřesněte svůj obsah, který je vhodný pro dané pole. Barva textu zástupný symbol nejde přizpůsobit a budou stejné bez ohledu na to `TextColor` nastavení. Pokud váš návrh žádá pro vlastní zástupný text color, bude nutné vrátit zpět [vlastního rendereru](). Vytvoří následující `Entry` "UserName" jako zástupný symbol v XAML:

```xaml
<Entry Placeholder="Username" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Příklad zástupný text položky")

### <a name="password-fields"></a>Pole s heslem

`Entry` poskytuje `IsPassword` vlastnost. Když `IsPassword` je `true`, se zobrazí obsah pole jako černá kroužky:

V XAML:

```xaml
<Entry IsPassword="true" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Příklad IsPassword položka")

Zástupné symboly lze použít s instancí `Entry` , které jsou nakonfigurované jako pole s heslem:

V XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Položka IsPassword a příklad zástupného symbolu")


### <a name="colors"></a>Barvy

Položky můžete nastavit pomocí vlastní pozadí a barvy textu prostřednictvím následující vlastnosti umožňující vazbu:

- **TextColor** &ndash; nastaví barvu textu.
- **BackgroundColor** &ndash; nastaví barvu pozadí textu je znázorněno.

Zvláštní pozornost je třeba zajistit, že bude možné použít na jednotlivých platformách barvy. Protože každá platforma má různé výchozí hodnoty pro barvy textu a pozadí, často musíte nastavit obě, pokud nastavíte jednu.

Pomocí následujícího kódu nastavit barvu textu položky:

V XAML:

```xaml
<Entry TextColor="Green" />
```

V jazyce C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "Příklad položky TextColor")

Všimněte si, že zástupný symbol není ovlivněn zadaný `TextColor`.

Chcete-li nastavit barvu pozadí v XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

V jazyce C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Příklad BackgroundColor položka")

Buďte opatrní, abyste měli jistotu, že barvy textu a pozadí, který zvolíte lze použít na jednotlivých platformách a není skryl všechny zástupný text.

## <a name="events-and-interactivity"></a>Události a interaktivita

Položka poskytuje dvě události:

- [TextChanged](xref:Xamarin.Forms.Entry.TextChanged) &ndash; aktivovaná při změně textu v položce. Obsahuje text před a po provedení změny.
- [Dokončení](xref:Xamarin.Forms.Entry.Completed) &ndash; vyvolá, když uživatel skončila vstup stisknutím klávesy return na klávesnici.

### <a name="completed"></a>Byla dokončena

`Completed` Událost se používá k reagovat na dokončení interakci s položkou. `Completed` je vyvolána, když uživatel ukončí vstup s polem tak, že zadáte klávesu return na klávesnici. Obslužná rutina události je obslužná rutina události obecného, trvá odesílatele a `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Událost dokončení může být přihlášena k odběru v XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

a C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged.

`TextChanged` Událost se používá k reagovat na změny v obsahu pole.

`TextChanged` je vyvolána vždy, když `Text` z `Entry` změny. Obslužná rutina události přijímá instanci `TextChangedEventArgs`. `TextChangedEventArgs` poskytuje přístup k staré a nové hodnoty `Entry` `Text` prostřednictvím `OldTextValue` a `NewTextValue` vlastnosti:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Události může být přihlášena k odběru v XAML:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

a C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>Související odkazy

- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Vstupní rozhraní API](xref:Xamarin.Forms.Entry)
