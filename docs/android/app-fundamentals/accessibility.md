---
title: Usnadnění přístupu v systému Android
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 2a49d15651b8c6ab7417a69d934af5d20bfc13d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763901"
---
# <a name="accessibility-on-android"></a>Usnadnění přístupu v systému Android

Tato stránka popisuje, jak pomocí rozhraní Android API usnadnění můžete vytvářet aplikace podle požadavků [kontrolní seznam usnadnění](~/cross-platform/app-fundamentals/accessibility.md).
Odkazovat [iOS usnadnění](~/ios/app-fundamentals/accessibility.md) a [usnadnění OS X](~/mac/app-fundamentals/accessibility.md) stránky pro jinou platformu rozhraní API.


## <a name="describing-ui-elements"></a>Popisuje prvky uživatelského rozhraní

Poskytuje Android `ContentDescription` vlastnost, která se používá k zajištění k přístupné popis účelu ovládacího prvku obrazovky čtení rozhraní API.

Popis obsahu lze nastavit v buď C# nebo v souboru AXML rozložení.

**C#**

Popis můžete nastavit v kódu libovolného řetězce (nebo prostředek řetězec):

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML rozložení**

V XML rozložení pomocí `android:contentDescription` atribut:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>Použijte pomocný parametr pro TextView

Pro `EditText` a `TextView` ovládací prvky pro vstup dat, použijte `Hint` vlastnost zadejte popis je očekáván jaké vstup (místo `ContentDescription`).
Po zadání nějaký text, pro vlastní text bude "číst" místo v pomocném parametru.

**C#**

Nastavte `Hint` vlastností v kódu:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML rozložení**

V XML soubory rozložení pomocí `android:hint` atribut:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>Odkazy LabelFor vstupní pole s popisky

Chcete-li přidružit štítek s ovládacím prvkem vstupní data, použijte `LabelFor` vlastnosti

**C#**

V jazyce C#, nastavte `LabelFor` popisuje vlastnost, která má ID prostředku tomto tento obsah ovládacího prvku (obvykle tato vlastnost nastavena na štítek a odkazuje na některé vstupního ovládacího prvku):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML rozložení**

V rozložení pomocí XML `android:labelFor` vlastnost, která má odkazovat na další ovládací prvek identifikátor:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>Oznamujeme pro usnadnění přístupu

Použití `AnnounceForAccessibility` metoda na všech zobrazení vzájemnou komunikaci o změnu stavu nebo události pro uživatele, pokud je povoleno usnadnění. Tato metoda není povinné pro většinu operací, kde předdefinované komentář poskytují dostatečná zpětnou vazbu, ale dá se použít kde mohou být užitečné pro uživatele Další informace.

Následující kód ukazuje jednoduchý příklad volání `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>Změna nastavení fokusu

Přístupné navigační spoléhá na ovládací prvky s zaměřuje na podporu uživatele porozumět tomu, jaké operace jsou k dispozici. Poskytuje Android `Focusable` vlastnost, která můžete označit ovládací prvky jako konkrétně možné aktivovat během navigace.

**C#**

Abyste zabránili ovládacího prvku v získání fokus pomocí C#, nastavte `Focusable` vlastnost `false`:

```csharp
label.Focusable = false;
```

**AXML rozložení**

V rozložení soubory XML sady `android:focusable` atribut:

```xml
<android:focusable="false" />
```

Můžete také ovládat pořadí zaměřit se `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` atributy, které se obvykle nastavuje v rozložení AXML. Ujistěte se, že uživatel můžete snadno procházet ovládacích prvků na obrazovce pomocí těchto atributů.


## <a name="accessibility-and-localization"></a>Usnadnění přístupu a lokalizace

Ve výše uvedených příkladech jsou popis pomocný parametr a obsah nastavit přímo na k nalezení hodnoty zobrazení. Je vhodnější použít hodnoty **Strings.xml** souboru, jako je tato:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

Pomocí text ze souboru řetězce jsou uvedeny níže v C# a AXML rozložení soubory:

**C#**

Místo použití textové literály v kódu, vyhledejte přeložený hodnoty z řetězce souborů s `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

V rozložení XML usnadnění atributy, například `hint` a `contentDescription` může být nastaven na identifikátor řetězce:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

Výhodou ukládání textu do samostatného souboru je že ve vaší aplikaci lze zadat více překlady jazyka souboru. Najdete v článku [Android Lokalizace průvodce](~/android/app-fundamentals/localization.md) další soubory lokalizovaný řetězec jak přidat do projektu aplikace.


## <a name="testing-accessibility"></a>Testování usnadnění přístupu

Postupujte podle [tyto kroky](http://developer.android.com/training/accessibility/testing.html#how-to) umožnit TalkBack a prozkoumat modelem Touch k testování usnadnění na zařízeních s Androidem.

Budete muset nainstalovat [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) na webu Google Play, pokud se nezobrazí v **Nastavení > usnadnění**.


## <a name="related-links"></a>Související odkazy

- [Usnadnění přístupu a platformy](~/cross-platform/app-fundamentals/accessibility.md)
- [Rozhraní API systému Android usnadnění přístupu](http://developer.android.com/guide/topics/ui/accessibility/index.html)
