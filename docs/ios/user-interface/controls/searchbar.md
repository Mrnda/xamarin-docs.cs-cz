---
title: Panely vyhledávání v Xamarin.iosu
description: Tento dokument popisuje způsob použití panely vyhledávání v Xamarin.iOS. Popisuje, jak vytvořit panely hledání prostřednictvím kódu programu a ve scénáři.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: fdd9fe647f1a2f63b2a86a64ad92d1e71d6fcd2e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242076"
---
# <a name="search-bars-in-xamarinios"></a>Panely vyhledávání v Xamarin.iosu

UISearchBar se používá k hledání v seznamu hodnot. 

Obsahuje tři hlavní komponenty: 

- Pole se používá k zadání textu. Uživatelé mohou využít tuto hodnotu na zadání jejich hledaného termínu.
- Vymazat tlačítka, odeberte text z vyhledávacího pole.
- Tlačítko Storno, ukončete funkce vyhledávání.

![Panel hledání](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implementace panelu hledání

K implementaci úvodní panel vyhledávání po vytvoření instance nového:

```csharp
searchBar = new UISearchBar();
```

A umístěte ji. Následující příklad ukazuje, jak umístěte ho na navigačním panelu nebo v HeaderView tabulku:

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Nastavení vlastnosti na panelu hledání:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Vlastnosti panelu hledání](searchbar-images/image6.png)

Vyvolat `SearchButtonClicked` událost, když se stiskne tlačítko Hledat. To bude volat logiky hledání:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Informace o správě prezentaci panel hledání a výsledky hledání [vyhledávání Kontroleru ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) předpisu.

## <a name="using-the-search-bar-in-the-designer"></a>Pomocí panelu hledání v Návrháři

Návrhář nabízí dvě možnosti pro implementaci panelu hledání v Návrháři

- Panel hledání
- Panel hledání s Kontroleru zobrazení vyhledávání (zastaralé)

![Hledat ovládací prvky stavového řádku v Návrháři](searchbar-images/image2.png)

Pomocí panelu Vlastnosti můžete nastavit vlastnosti na panelu hledání

![Návrhář vlastnosti panel hledání](searchbar-images/image3.png)

Tyto vlastnosti jsou vysvětleny níže:

- **Text zástupného symbolu, řádku** – tyto vlastnosti umožňují navrhovat a dát pokyn, jak uživatelé by měli použít panel vyhledávání. Například pokud vaše aplikace zobrazí seznam úložišť můžete použít vlastnost výzvy informovat, že uživatelé můžou "zadat město, název článku nebo PSČ"
- **Hledat styl** – můžete nastavit na panelu hledání buď jako **Prominent** nebo **minimální**. Použití viditelného bude odstínu všechno ostatní na obrazovce, s výjimkou hledání řádku fokus vykreslit do panelu hledání. Na panelu hledání minimální styl bude splynou s jeho okolí.
- **Možnosti** – povolení tyto vlastnosti se zobrazí pouze prvek uživatelského rozhraní. Funkce je nutné implementovat pro tyto vyvoláním správné události podle popisu v [dokumentace API pro vyhledávání na panelu](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Zobrazí výsledky hledání / tlačítku záložek – zobrazuje výsledky hledání nebo záložky ikonu na panelu hledání
    - : Zobrazí tlačítko Storno – umožňuje uživatelům ukončit funkci vyhledávání. Doporučuje se, že je vybrána tato možnost.
    - Zobrazuje rozsah – uživatelé si můžou omezit obor vyhledávání. Například při vyhledávání v aplikace music uživatel může vybrat, zda chtějí prohledávat Apple Music nebo jejich knihovny pro určité skladby nebo interpreta. Chcete-li zobrazit různé možnosti, přidejte pole názvy, které **ScopeBarTitles** vlastnost.
    ![Hledání panel oboru názvů](searchbar-images/image4.png)

- **Chování textu** – tyto možnosti slouží k formátování vstupu uživatele při zadávání adres. Nastaví začátek každé slovo nebo věty, malá a velká písmena nebo každý znak jako velká písmena. Opravy a kontroly pravopisu s výzva s navrhované pravopis slova jako jejich typu.
- **Klávesnice** – ovládací prvky stylu klávesnice zobrazuje pro vstup, a proto jaké klíče jsou k dispozici na klávesnici. To zahrnuje číslo oblasti, panel telefonu, e-mailu, adresa URL společně s další možnosti.
- **Vzhled** – Určuje styl vzhledu klávesnice a bude buď tmavý nebo světle s motivem.
- **Vrátí klíč** – změňte popisek na klávesu Return lépe podle, jaká akce se provedou. Mezi podporované hodnoty patří, Go, spojení, další, směrování, dokončení a hledání.
- **Zabezpečení** – označuje, zda maskované vstup (například pro vstup hesla).

## <a name="related-links"></a>Související odkazy

- [Vyhledávání Kontroleru](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
