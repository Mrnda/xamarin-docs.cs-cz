---
title: Xamarin.Forms Editor
description: Tento článek vysvětluje, jak používat ovládací prvek Xamarin.Forms Editor tak, aby přijímal Víceřádkový textový vstup v aplikaci.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 7ad8c8aa77e23c5a8fb7649736ecb271f835d1a7
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245521"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms Editor

_Víceřádkový textový vstup_

`Editor` Řízení se používá k přijetí Víceřádkový vstup. Tento článek se zabývá:

- **[Přizpůsobení](#customization)**  &ndash; klávesnici a barvu možnosti.
- **[Interaktivity](#interactivity)**  &ndash; události, které můžete naslouchali pro zajistit interaktivity.

## <a name="customization"></a>Přizpůsobení

### <a name="setting-and-reading-text"></a>Nastavení a čtení textu

`Editor`, Jako ostatních zobrazení textu prezentací zpřístupní `Text` vlastnost. Tuto vlastnost lze použít k nastavení a přečtěte si text uvedený na `Editor`. Následující příklad ukazuje nastavení `Text` vlastnost v jazyce XAML:

```xaml
<Editor Text="I am an Editor" />
```

V jazyce C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Čtení textu, přístup `Text` vlastnost v jazyce C#:

```csharp
var text = MyEditor.Text;
```

### <a name="limiting-input-length"></a>Omezení vstupní délka

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Vlastnosti lze omezit vstupní délku, která je povolené [ `Editor` ](xref:Xamarin.Forms.Editor). Tato vlastnost by měla být nastavená na kladné celé číslo:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) vlastnost hodnota 0 znamená, že bude možné žádný vstup a hodnota `int.MaxValue`, což je výchozí hodnota pro [ `Editor` ](xref:Xamarin.Forms.Editor), určuje, že neexistuje žádná efektivní limit počtu znaků, které mohou být zapsána.

### <a name="keyboards"></a>Klávesnice

Klávesnice, který se zobrazí, když uživatelé komunikovat s `Editor` lze nastavit prostřednictvím kódu programu prostřednictvím [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/) vlastnost.

Možnosti pro typ klávesnice jsou:

- **Výchozí** &ndash; výchozí klávesnice
- **Chat** &ndash; používá pro odesílání textových zpráv & místech kde jsou užitečné emoji
- **E-mailu** &ndash; použít při zadávání e-mailové adresy
- **Číselné** &ndash; použít při zadávání čísel
- **Telefonní** &ndash; použít při zadávání telefonních čísel
- **Adresa URL** &ndash; používá pro zadání cesty k souborům & webové adresy

Došlo [příklad každý klávesnice](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) v části našich recepty.

### <a name="enabling-and-disabling-spell-checking"></a>Povolení a zakázání kontrola pravopisu

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Ovládací prvky vlastnost jestli pravopisu kontrola je povolené. Ve výchozím nastavení, je vlastnost nastavena `true`. Jako uživatel zadá text, jsou vypsány pravopisné chyby.

Ale v některých případech vstupní text, například zadáním uživatelského jména, kontrolu pravopisu poskytuje záporné prostředí a to by mělo být zakázáno nastavením [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) vlastnost `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Když [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) je nastavena na `false`a není používán vlastní klávesnice, kontrola pravopisu nativní bude zakázán. Ale pokud [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) má byla sada, která zakáže pravopisu kontrola, jako například [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` vlastnost je ignorována. Proto vlastnost nelze použít k povolení pro kontrolu pravopisu `Keyboard` zakazující explicitně.

### <a name="colors"></a>Barvy

`Editor` může být nastaven na použití vlastní barvu pozadí prostřednictvím `BackgroundColor` vlastnost. Zvláštní pozornost je třeba zajistit, že barvy bude možné použít na každou platformu. Protože každá platforma má jiné výchozí hodnoty pro barvu textu, musíte nastavit vlastní barvu pozadí pro každou platformu. V tématu [práce s Tweaks platformy](~/xamarin-forms/platform/device.md) Další informace o optimalizaci uživatelského rozhraní pro každou platformu.

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

V jazyce XAML:

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

![](editor-images/textbackgroundcolor.png "Editor BackgroundColor příklad")

Ujistěte se, že barvy pozadí a text, který zvolíte lze použít na každou platformu a nemáte skrývat žádné zástupný text.

## <a name="interactivity"></a>Interaktivita

`Editor` zpřístupní dvě události:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash; vyvolá, když se text změní v editoru. Poskytuje text před a po provedení změny.
- [Dokončit](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) &ndash; vyvolá, když uživatel skončila vstup po stisknutí klávesy návratový na klávesnici.

### <a name="completed"></a>Byla dokončena

`Completed` Se používá k reagovat na dokončení interakci s `Editor`. `Completed` se vyvolá, když uživatel končí vstup s polem tak, že zadáte návratový klíč na klávesnici. Obslužná rutina události je obslužná rutina události Obecné, trvá odesílatele a `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Dokončení události může přihlásit k odběru v kódu a XAML:

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

V jazyce XAML:

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

### <a name="textchanged"></a>TextChanged

`TextChanged` Se používá k reagovat na změny v obsahu pole.

`TextChanged` se vyvolá, když `Text` z `Editor` změny. Obslužná rutina události přijímá instanci `TextChangedEventArgs`. `TextChangedEventArgs` poskytuje přístup k staré a nové hodnoty `Editor` `Text` prostřednictvím `OldTextValue` a `NewTextValue` vlastnosti:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Dokončení události může přihlásit k odběru v kódu a XAML:

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

V jazyce XAML:

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
- [Editor rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
