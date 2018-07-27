---
title: Položka Xamarin.Forms
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms položky tak, aby přijímal jedním řádkem textu nebo zadávání hesla v aplikaci.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: 5ccd2a653e5190df11a58477905e868b25878e44
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270109"
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

### <a name="customizing-the-keyboard"></a>Přizpůsobení klávesnice

Klávesnice, který se zobrazí, když uživatelé možnost zasahovat [ `Entry` ](xref:Xamarin.Forms.Entry) lze programově nastavit prostřednictvím [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) vlastnost na jednu z následujících vlastností z [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) třídy:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – pro odesílání textových zpráv a místa, kde je užitečné emoji.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – výchozí klávesnice.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – Při zadávání e-mailové adresy.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – Při zadávání čísel.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – Při zadávání textu, bez jakékoli [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags) zadané.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – Při zadávání telefonních čísel.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – Při zadávání textu.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – používá se pro zadání cesty k souborům & webové adresy.

To lze provést v XAML následujícím způsobem:

```xaml
<Entry Keyboard="Chat" />
```

Ekvivalentní kód jazyka C# je:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

Příklady každý klávesnice můžete najít v našich [recepty](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) úložiště.

[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Třída má také [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) metoda factory, která slouží k přizpůsobení klávesnice zadáním chování malá a velká písmena, kontrola pravopisu a návrh. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) hodnoty výčtu jsou zadané jako argumenty metody s přizpůsobeným `Keyboard` se vrací. `KeyboardFlags` Výčet obsahuje následující hodnoty:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – žádné funkce byly přidány na klávesnici.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) – Označuje, že první písmeno prvního slova větě zadané budou automaticky velká.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) – Označuje, že kontrola pravopisu se provede na zadaný text.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) – označuje toto slovo dokončování budou nabízet v zadaným textem.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – Označuje, že první písmeno každého slova budou automaticky velká.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – Označuje, že každému znaku budou automaticky velká.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – Označuje, že dojde k žádné automatické malá a velká písmena.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) – Označuje, že kontrola pravopisu, dokončování slov a větu malá a velká písmena dojde u zadaným textem.

Následující příklad kódu XAML ukazuje, jak přizpůsobit výchozí [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) nabízí dokončování slov, a využijte každý zadaný znak:

```xaml
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

Ekvivalentní kód jazyka C# je:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Přizpůsobení klávesu Return

Vzhled klávesu return na softwarová klávesnice, což je zobrazí, když [ `Entry` ](xref:Xamarin.Forms.Entry) má fokus, můžete přizpůsobit tak, že nastavíte [ `ReturnType` ](xref:Xamarin.Forms.Entry.ReturnType) vlastnost na hodnotu [ `ReturnType` ](xref:Xamarin.Forms.ReturnType) výčtu:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) – Označuje, že žádné konkrétní návratový klíč je požadován a, bude použita výchozí platforma.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) – označuje "Hotovo" klávesu return.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) – označuje klávesu return "Go".
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – Označuje, že "Následující" návratový klíč.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – Označuje, že vrácená klíč "Vyhledat".
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – označuje klávesu return "Odeslat".

Následující příklad XAML ukazuje, jak nastavit klávesu return:

```xaml
<Entry ReturnType="Send" />
```

Ekvivalentní kód jazyka C# je:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Přesný vzhled klávesu return je závislá na platformu. V systémech iOS klávesu return je založený na textu tlačítka. Na Android a univerzální platformy Windows, klávesu return ale založené na ikonu tlačítka.

Při stisknutí klávesy return [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) dojde k aktivaci události a veškeré `ICommand` určená [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) vlastnost provádí. Kromě toho všechny `object` určená [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) budou předána vlastnost `ICommand` jako parametr. Další informace o příkazech najdete v tématu [příkaz rozhraní](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

### <a name="enabling-and-disabling-spell-checking"></a>Povolení a zakázání kontroly pravopisu

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Ovládací prvky vlastnost určuje, zda kontrola pravopisu je povolená. Ve výchozím nastavení je vlastnost nastavena na `true`. Jako uživatel zadá text, jsou uvedeny pravopisné chyby.

Však pro některé scénáře vstupního textu, jako je zadání uživatelského jména, kontrolu pravopisu poskytuje negativní zkušenosti a by mělo být zakázáno nastavením [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) vlastnost `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Když [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) je nastavena na `false`a se nepoužívá vlastní klávesnice, nástroj pro kontrolu pravopisu nativní se deaktivuje. Ale pokud [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) má byla sada, která zakáže pravopisu kontrolu, například [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` vlastnost se ignoruje. Proto nelze vlastnost použít k povolení kontroly pravopisu pro `Keyboard` zakazující explicitně.

### <a name="enabling-and-disabling-text-prediction"></a>Povolení a zakázání prediktivní vkládání textu

[ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) Vlastností ovládacích prvků, zda prediktivní vkládání textu a automaticky je povolena oprava textu. Ve výchozím nastavení je vlastnost nastavena na `true`. Jako uživatel zadá text, jsou uvedeny slovo předpovědi.

Však pro některé scénáře vstupního textu, jako je zadání uživatelského jména, prediktivní vkládání textu a automatický text opravy poskytuje negativní zkušenosti a by mělo být zakázáno nastavením [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) vlastnost `false`:

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Při [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) je nastavena na `false`, a vlastní klávesnice není právě používá, prediktivní vkládání textu a automaticky oprava textu je zakázaná. Ale pokud [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) nastavil tento prediktivní vkládání textu zakáže `IsTextPredictionEnabled` vlastnost se ignoruje. Proto nelze použít vlastnost umožňující prediktivní vkládání textu `Keyboard` zakazující explicitně.

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

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; Vyvolá se při změně textu v položce. Obsahuje text před a po provedení změny.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; vyvoláno, když uživatel skončila vstup stisknutím klávesy return na klávesnici.

### <a name="completed"></a>Byla dokončena

`Completed` Událost se používá k reagovat na dokončení interakci s položkou. `Completed` je vyvolána, když uživatel ukončí vstup s polem stisknutím klávesy return na klávesnici. Obslužná rutina události je obslužná rutina události obecného, trvá odesílatele a `EventArgs`:

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

Po [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) událost aktivuje všechny `ICommand` určená [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) vlastnost provádí, s `object` určená [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) vlastnost předána `ICommand`.

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
