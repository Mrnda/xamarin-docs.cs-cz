---
title: Xamarin.Forms Editor
description: Tento článek vysvětluje, jak použít ovládací prvek editoru Xamarin.Forms tak, aby přijímal Víceřádkový textový vstup v aplikaci.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 4879ff88d5bbdab5aa92024bee7f50239a141e3b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995861"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms Editor

_Víceřádkový textový vstup_

`Editor` Ovládací prvek se používá tak, aby přijímal Víceřádkový vstupní. Tento článek se zabývá:

- **[Přizpůsobení](#customization)**  &ndash; možnosti klávesnice a barvu.
- **[Interakce](#interactivity)**  &ndash; události, které mohou být data pro interaktivitu.

## <a name="customization"></a>Vlastní nastavení

### <a name="setting-and-reading-text"></a>Nastavení a čtení textu

`Editor`, Stejně jako ostatní zobrazení nabízí ten samý text zpřístupňuje `Text` vlastnost. Tuto vlastnost lze použít k nastavení a přečtěte si text na předložený `Editor`. Následující příklad ukazuje nastavení `Text` vlastnost v XAML:

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

### <a name="keyboards"></a>Klávesnice

Klávesnice, který se zobrazí, když uživatelé možnost zasahovat `Editor` lze programově nastavit prostřednictvím [ ``Keyboard`` ](xref:Xamarin.Forms.Keyboard) vlastnost.

Možnosti pro typ klávesnice jsou:

- **Výchozí** &ndash; výchozí klávesnice
- **Chat** &ndash; použít pro odesílání textových zpráv a místa kde jsou užitečné emoji
- **E-mailu** &ndash; použít při zadávání e-mailové adresy
- **Číselné** &ndash; při zadání čísel
- **Telefonní** &ndash; použít při zadávání telefonní čísla
- **Adresa URL** &ndash; používá pro zadání cesty k souborům & webové adresy

Je [příklad každý klávesnice](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) v oddílu recepty.

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
