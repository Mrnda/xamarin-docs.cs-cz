---
title: Položka
description: Jednořádkové textové nebo heslo, zadejte
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: a45a4edb93920cfe1d0289da44ee664e41c25cf1
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847845"
---
# <a name="entry"></a>Položka

_Jednořádkové textové nebo heslo, zadejte_

Platformě Xamarin.Forms `Entry` se používá pro zadávání textu jeden řádek. `Entry`, Například `Editor` zobrazení, podporuje více typů klávesnice. Kromě toho `Entry` lze použít jako pole pro heslo.

## <a name="display-customization"></a>Přizpůsobení zobrazení

### <a name="setting-and-reading-text"></a>Nastavení a čtení textu

`Entry`, Jako ostatních zobrazení textu prezentací zpřístupní `Text` vlastnost. Tuto vlastnost lze použít k nastavení a přečtěte si text uvedený na `Entry`. Následující příklad ukazuje nastavení `Text` vlastnost v jazyce XAML:

```xaml
<Entry Text="I am an Entry" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Čtení textu, přístup `Text` vlastnost v jazyce C#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Šířka `Entry` můžete definovat nastavením jeho `WidthRequest` vlastnost. Nezávisí na šířku `Entry` definovaný na základě hodnoty z jeho `Text` vlastnost.

### <a name="limiting-input-length"></a>Omezení vstupní délka

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Vlastnosti lze omezit vstupní délku, která je povolené [ `Entry` ](xref:Xamarin.Forms.Entry). Tato vlastnost by měla být nastavená na kladné celé číslo:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) vlastnost hodnota 0 znamená, že bude možné žádný vstup a hodnota `int.MaxValue`, což je výchozí hodnota pro [ `Entry` ](xref:Xamarin.Forms.Entry), určuje, že neexistuje žádná efektivní limit počtu znaků, které mohou být zapsána.

### <a name="keyboards"></a>Klávesnice

Klávesnice, který se zobrazí, když uživatelé komunikovat s `Entry` lze nastavit prostřednictvím kódu programu prostřednictvím `Keyboard` vlastnost.

Možnosti pro typ klávesnice jsou:

- **Výchozí** &ndash; výchozí klávesnice
- **Chat** &ndash; používá pro odesílání textových zpráv & místech kde jsou užitečné emoji
- **E-mailu** &ndash; použít při zadávání e-mailové adresy
- **Číselné** &ndash; použít při zadávání čísel
- **Telefonní** &ndash; použít při zadávání telefonních čísel
- **Adresa URL** &ndash; používá pro zadání cesty k souborům a webové adresy

Došlo [příklad každý klávesnice](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) v části našich recepty.

### <a name="enabling-and-disabling-spell-checking"></a>Povolení a zakázání kontrola pravopisu

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Ovládací prvky vlastnost jestli pravopisu kontrola je povolené. Ve výchozím nastavení, je vlastnost nastavena `true`. Jako uživatel zadá text, jsou vypsány pravopisné chyby.

Ale v některých případech vstupní text, například zadáním uživatelského jména, kontrolu pravopisu poskytuje záporné prostředí a to by mělo být zakázáno nastavením [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) vlastnost `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Když [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) je nastavena na `false`a není používán vlastní klávesnice, kontrola pravopisu nativní bude zakázán. Ale pokud [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) má byla sada, která zakáže pravopisu kontrola, jako například [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` vlastnost je ignorována. Proto vlastnost nelze použít k povolení pro kontrolu pravopisu `Keyboard` zakazující explicitně.

### <a name="placeholders"></a>Zástupné symboly

`Entry` může být nastaven na Zobrazit zástupný text při jeho není ukládání vstup uživatele. V praxi to je často vidět ve formulářích o vysvětlení, obsah, který je vhodný pro dané pole. Barva textu zástupný symbol nelze přizpůsobit a stejný bez ohledu na to `TextColor` nastavení. Pokud váš návrh žádá barvu, která vlastní zástupného symbolu, bude nutné vrátit zpět [vlastní zobrazovací jednotky](). Vytvoří následující `Entry` "UserName" jako zástupný symbol v jazyce XAML:

```xaml
<Entry Placeholder="Username" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Příklad položky zástupný symbol")

### <a name="password-fields"></a>Pole hesla

`Entry` poskytuje `IsPassword` vlastnost. Když `IsPassword` je `true`, obsah pole zobrazí jako černá kroužky:

V jazyce XAML:

```xaml
<Entry IsPassword="true" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Příklad IsPassword položka")

Zástupné symboly lze použít s instancí `Entry` které jsou nakonfigurovány jako pole heslo:

V jazyce XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

V jazyce C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Položka IsPassword a příklad zástupný symbol")


### <a name="colors"></a>Barvy

Položka může být nastaven na použití vlastní pozadí a barvy textu prostřednictvím následující vazbu vlastnosti:

- **TextColor** &ndash; nastaví barvu textu.
- **BackgroundColor** &ndash; nastaví barvu zobrazí pozadí textu.

Zvláštní pozornost je třeba zajistit, že barvy bude možné použít na každou platformu. Protože každá platforma má jiné výchozí hodnoty pro barvu textu a pozadí, často musíte nastavit obě, pokud jste nastavili jednu.

Chcete-li nastavit barvu textu položky, použijte následující kód:

V jazyce XAML:

```xaml
<Entry TextColor="Green" />
```

V jazyce C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "Příklad TextColor položka")

Všimněte si, že zástupného textu není ovlivněné zadaný `TextColor`.

Nastavení barvy pozadí v jazyce XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

V jazyce C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Příklad BackgroundColor položka")

Dávejte pozor, abyste měli jistotu, že barvy pozadí a text, který zvolíte lze použít na každou platformu a nemáte skrývat žádné zástupný text.

## <a name="events-and-interactivity"></a>Události a interakce

Položka zpřístupní dvě události:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; vyvolá, když se text změní v položce. Poskytuje text před a po provedení změny.
- [Dokončit](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; vyvolá, když uživatel skončila vstup po stisknutí klávesy návratový na klávesnici.

### <a name="completed"></a>Byla dokončena

`Completed` Se používá k reagovat na dokončení interakci s položkou. `Completed` se vyvolá, když uživatel končí vstup s polem tak, že zadáte návratový klíč na klávesnici. Obslužná rutina události je obslužná rutina události Obecné, trvá odesílatele a `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Dokončení události může přihlásit k odběru v jazyce XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

a C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Se používá k reagovat na změny v obsahu pole.

`TextChanged` se vyvolá, když `Text` z `Entry` změny. Obslužná rutina události přijímá instanci `TextChangedEventArgs`. `TextChangedEventArgs` poskytuje přístup k staré a nové hodnoty `Entry` `Text` prostřednictvím `OldTextValue` a `NewTextValue` vlastnosti:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Událostí můžete přihlásit k odběru v jazyce XAML:

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
- [Položka rozhraní API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
