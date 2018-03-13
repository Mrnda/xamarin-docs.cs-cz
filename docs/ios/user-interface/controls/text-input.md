---
title: "Zadávání textu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c45ea8cb7c0e3d12e94666d61c6fdf7e5828264e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="text-input"></a>Zadávání textu

Přijetí zadávání textu uživatele se provádí pomocí `UITextField` pro jeden řádek vstupy a UITextView Víceřádkový upravovat text. Můžete buď z těchto prvků přetažením na obrazovce a dvakrát klikněte na nastavit počáteční text.

Na následujících snímcích obrazovky zobrazit ikony pro tyto ovládací prvky, umístěný v panelu nástrojů v sadě Visual Studio pro Mac:

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

Jakmile mají pojmenované výstupu a uložili soubor Storyboard, Visual Studio pro Mac se aktualizuje `.designer.cs` třídu můžete přidat kód C#, který odkazuje na ovládacího prvku do souboru – třída. Každý ovládací prvek má svou vlastní jedinečné vlastnosti a události, které jsou přístupné v kódu jazyka C#.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

`UITextField` Řízení se nejčastěji používá tak, aby přijímal jeden řádek textu vstupu, například uživatelské jméno nebo heslo. Některé z možností pro přizpůsobení ovládacího prvku k dispozici zde se zobrazují:

 [![](text-input-images/image15a.png "Vlastnosti UITextField")](text-input-images/image15a.png#lightbox)

Tyto ovládací prvky jsou vysvětleny níže:

-  **Zástupný symbol** – tato položka je nepovinná. Pokud nastavit, zobrazí se při textové pole je prázdné, obvykle vysvětlují uživateli je očekáván jaké vstup.
-  **Tlačítko Vymazat** – tato volba určuje, kdy standardní nezaškrtnuté políčko (šedé kruhu (x)) se zobrazí v textové pole, jako způsob, jak uživatelům rychle prostý text. Může být trvale skryté, trvale viditelný nebo uvedené, v závislosti na tom, jestli je upravovaný pole.
-  **Minimální velikost písma** a **upravit přizpůsobit** – umožňuje velikost písma upravena automaticky přizpůsobit text delší a zabránit zkrácení, ale omezený na žádné menší než zadaná velikost.
-  **Malá a velká písmena** – jestli se má automaticky velká počáteční slova, věty nebo veškerý vstup.
-  **Oprava** – zda jsou povoleny kontrolu pravopisu a návrhy.
-  **Klávesnice** – ovládací prvky styl klávesnice zobrazuje pro vstup, a proto jaké klíče jsou k dispozici na klávesnici. To zahrnuje číslo Pad, Phone Pad, e-mailu, adresa URL společně s dalšími možnostmi.
-  **Vzhled** – ovládací prvky styl vzhledu klávesnici a bude buď světlý nebo light motivu.
-  **Vrátí klíč** – změňte popisek na návratový klíč tak, aby lépe odrážela jaká akce se provedou. Podporované hodnoty zahrnují přejít, připojení, další, trasy, provést a vyhledávání.
-  **Zabezpečení** – Určuje, jestli je maskovat vstup (například pro zadání hesla).


Pokud UITextField volána `textfield1` byl přidán do obrazovky pomocí návrháře, můžete nastavit nebo změnit jeho vlastnosti v jazyce C# následujícím způsobem:

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS poskytuje výčty, kde je to vhodné usnadňují vyberte požadovaná nastavení, jako `UIKeyboardType` a `UIReturnKeyType` ve výše uvedeném fragmentu kódu.

### <a name="display-text-programmatically"></a>Zobrazit Text prostřednictvím kódu programu

Pokud nechcete návrh obrazovky pomocí návrháře nebo pokud chcete dynamicky přidat text za běhu, můžete vytvořit a zobrazit UITextField prostřednictvím kódu programu v `ViewDidLoad` metoda řadiče zobrazení takto:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

`UITextView` Řízení lze použít k zobrazení jen pro čtení textu nebo tak, aby přijímal zadávání více řádků textu. Má řadu stejné možnosti jako `UITextField` (například velkých písmen, oprava, atd).

 [![](text-input-images/image16a.png "Vlastnosti UITextView")](text-input-images/image16a.png#lightbox)

Určité vlastnosti:

-  **Chování** – jestli text upravovat, nebo jen pro čtení.
-  **Detekce** – vyhledává a převede zadaném data do elementů můžete kliknout, jako je telefonní čísla, které můžete aktivovat volání, adresy, které se odkazuje na Maps, adresy URL, které se otevřou v prohlížeči Safari nebo dat a časů, který se události v kalendáři.


Pokud UITextView přidala obrazovky pomocí návrháře, můžete nastavit nebo změnit jeho vlastnosti takto:

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
