---
title: Xamarin.Forms Editor
description: Tento článek vysvětluje, jak použít ovládací prvek editoru Xamarin.Forms tak, aby přijímal Víceřádkový textový vstup v aplikaci.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/13/2018
ms.openlocfilehash: 9774dcad14c2e2fc7e1203ef887a19f4b96218ba
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241472"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms Editor

_Víceřádkový textový vstup_

[ `Editor` ](xref:Xamarin.Forms.Editor) Ovládací prvek se používá tak, aby přijímal Víceřádkový vstupní. Tento článek se týká:

- **[Přizpůsobení](#customization)**  &ndash; možnosti klávesnice a barvu.
- **[Interakce](#interactivity)**  &ndash; události, které mohou být data pro interaktivitu.

## <a name="customization"></a>Vlastní nastavení

### <a name="setting-and-reading-text"></a>Nastavení a čtení textu

[ `Editor` ](xref:Xamarin.Forms.Editor), Stejně jako ostatní zobrazení nabízí ten samý text zpřístupňuje `Text` vlastnost. Tuto vlastnost lze použít k nastavení a přečtěte si text na předložený `Editor`. Následující příklad ukazuje nastavení `Text` vlastnost v XAML:

```xaml
<Editor Text="I am an Editor" />
```

V jazyce C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Čtení textu, přístup `Text` vlastností v jazyce C#:

```csharp
var text = MyEditor.Text;
```

### <a name="limiting-input-length"></a>Omezit délku vstupu

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Vlastnost lze použít k omezení, který je povolený pro vstupní délka [ `Editor` ](xref:Xamarin.Forms.Editor). Tuto vlastnost měli nastavit na kladné celé číslo:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) vlastnost hodnota 0 znamená, že žádný vstup bude možné a hodnotu `int.MaxValue`, což je výchozí hodnota pro [ `Editor` ](xref:Xamarin.Forms.Editor), označuje, že je žádný platit omezení počtu znaků, které mohou být zadány.

### <a name="auto-sizing-an-editor"></a>Automatické nastavování editoru

[ `Editor` ](xref:Xamarin.Forms.Editor) Můžete provést na automatickou velikost na jeho obsah tak, že nastavíte [ `Editor.AutoSize` ](xref:Xamarin.Forms.Editor.AutoSize) vlastnost [ `TextChanges` ](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), má hodnotu [ `EditoAutoSizeOption` ](xref:Xamarin.Forms.EditorAutoSizeOption) výčtu. Tento výčet má dvě hodnoty:

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) Označuje, že automatickou změnu velikosti je zakázané a je výchozí hodnota.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) Určuje, zda je povolena automatická změna velikosti.

To lze provést v kódu následujícím způsobem:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Pokud je povolená Automatická změna velikosti, výšku [ `Editor` ](xref:Xamarin.Forms.Editor) zvýší, když uživatel zadal s textem a výšku se sníží, protože uživatel odstraní text.

> [!NOTE]
> [ `Editor` ](xref:Xamarin.Forms.Editor) Bude if není Automatická velikost [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) byla nastavena vlastnost.

### <a name="customizing-the-keyboard"></a>Přizpůsobení klávesnice

Klávesnice, který se zobrazí, když uživatelé možnost zasahovat [ `Editor` ](xref:Xamarin.Forms.Editor) lze programově nastavit prostřednictvím [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) vlastnost na jednu z následujících vlastností z [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) třídy:

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
<Editor Keyboard="Chat" />
```

Ekvivalentní kód jazyka C# je:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

Příklady každý klávesnice můžete najít v našich [recepty](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) úložiště.

[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Třída má také [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) metoda factory, která slouží k přizpůsobení klávesnice zadáním chování malá a velká písmena, kontrola pravopisu a návrh. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) hodnoty výčtu uvedené jako argumenty pro metody s přizpůsobeným `Keyboard` se vrací. `KeyboardFlags` Výčet obsahuje následující hodnoty:

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
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

Ekvivalentní kód jazyka C# je:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>Povolení a zakázání kontroly pravopisu

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Ovládací prvky vlastnost určuje, zda kontrola pravopisu je povolená. Ve výchozím nastavení je vlastnost nastavena na `true`. Jako uživatel zadá text, jsou uvedeny pravopisné chyby.

Však pro některé scénáře vstupního textu, jako je zadání uživatelského jména, kontrolu pravopisu poskytuje negativní zkušenosti a proto by mělo být zakázáno nastavením [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) vlastnost `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Když [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) je nastavena na `false`a se nepoužívá vlastní klávesnice, nástroj pro kontrolu pravopisu nativní se deaktivuje. Ale pokud [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) má byla sada, která zakáže pravopisu kontrolu, například [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` vlastnost se ignoruje. Proto nelze vlastnost použít k povolení kontroly pravopisu pro `Keyboard` zakazující explicitně.

### <a name="colors"></a>Barvy

`Editor` můžete nastavit vlastní barvu pozadí prostřednictvím `BackgroundColor` vlastnost. Zvláštní pozornost je třeba zajistit, že bude možné použít na jednotlivých platformách barvy. Protože každá platforma má různé výchozí hodnoty pro barvu textu, budete muset nastavit vlastní barvu pozadí pro jednotlivé platformy. Zobrazit [práce s úprav platformy](~/xamarin-forms/platform/device.md) Další informace o optimalizaci uživatelského rozhraní pro každou platformu.

V jazyce C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

V XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Editor s BackgroundColor příklad")

Ujistěte se, že barvy textu a pozadí, který zvolíte lze použít na jednotlivých platformách a není skryl všechny zástupný text.

## <a name="interactivity"></a>Interaktivita

`Editor` poskytuje dvě události:

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash; aktivovaná při změně textu v editoru. Obsahuje text před a po provedení změny.
- [Dokončení](xref:Xamarin.Forms.Editor.Completed) &ndash; vyvolá, když uživatel skončila vstup stisknutím klávesy return na klávesnici.

### <a name="completed"></a>Byla dokončena

`Completed` Událost se používá k reagovat na dokončení interakci se `Editor`. `Completed` je vyvolána, když uživatel ukončí vstup s polem tak, že zadáte klávesu return na klávesnici. Obslužná rutina události je obslužná rutina události obecného, trvá odesílatele a `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Událost dokončení může přihlásit k odběru v kódu a XAML:

V jazyce C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

V XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged.

`TextChanged` Událost se používá k reagovat na změny v obsahu pole.

`TextChanged` je vyvolána vždy, když `Text` z `Editor` změny. Obslužná rutina události přijímá instanci `TextChangedEventArgs`. `TextChangedEventArgs` poskytuje přístup k staré a nové hodnoty `Editor` `Text` prostřednictvím `OldTextValue` a `NewTextValue` vlastnosti:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Událost dokončení může přihlásit k odběru v kódu a XAML:

V kódu:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

V XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```


## <a name="related-links"></a>Související odkazy

- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Editor rozhraní API](xref:Xamarin.Forms.Editor)
