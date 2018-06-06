---
title: Hledání řádky v Xamarin.iOS
description: Tento dokument popisuje, jak používat panely vyhledávání v Xamarin.iOS. Popisuje, jak vytvořit panely hledání prostřednictvím kódu programu a v scénáře.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: cd78c58ecb119c437296a0befe1d319d8837edae
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789922"
---
# <a name="search-bars-in-xamarinios"></a>Hledání řádky v Xamarin.iOS

UISearchBar se používá k hledání v seznamu hodnot. 

Obsahuje tři hlavní součásti: 

- Pole použité k zadání textu. Uživatelé můžou to k zadání jejich hledaný termín.
- Nezaškrtnuté políčko k odebrání žádné textové pole hledání.
- Tlačítko Storno, chcete-li ukončit funkce vyhledávání.

![Panel hledání](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implementace panelu Hledat

K implementaci úvodní panel vyhledávání po vytvoření instance nového:

```csharp
searchBar = new UISearchBar();
```

A pak ji umístit. Následující příklad ukazuje, jak pro jeho umístění na navigačním panelu nebo v HeaderView tabulky:

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Nastavení vlastnosti na panelu Hledat:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Panel hledání vlastnosti](searchbar-images/image6.png)

Vyvolat `SearchButtonClicked` události při stisknutí tlačítka vyhledávání. Tím bude volána logika vyhledávání:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Informace o správě prezentace panelu Hledat a výsledky hledání, najdete v části [vyhledávání řadiče ](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/) recepturách.

## <a name="using-the-search-bar-in-the-designer"></a>Použití panelu Hledat v Návrháři

Návrhář nabízí dvě možnosti pro implementaci panelu Hledat v Návrháři

- Panel hledání
- Panel hledání s řadiče zobrazení vyhledávání (zastaralé)

![Ovládací prvky panelu Hledat v Návrháři](searchbar-images/image2.png)

Použití panelu vlastnost pro nastavení vlastností na panelu Hledat

![Hledání panelu Vlastnosti návrháře](searchbar-images/image3.png)

Tyto vlastnosti jsou vysvětleny níže:

- **Text, zástupného symbolu, řádku** – tyto vlastnosti se používají k navrhnout a pokyn, jak mají uživatelé použít panelu vyhledávání. Například pokud vaše aplikace zobrazí seznam úložiště můžete použít vlastnost výzva k poradit lze "zadávat Město, název článku nebo PSČ"
- **Hledání styl** – můžete nastavit panelu Hledat buď jako **Prominent** nebo **minimální**. Pomocí viditelného bude odstínu všem ostatním na obrazovce, s výjimkou hledání panelu způsobuje fokus, které se mají vykreslovat na panelu vyhledávání. Panel hledání minimální styl bude přizpůsobte pomocí okolí.
- **Možnosti** – povolení tyto vlastnosti se zobrazí jenom element uživatelského rozhraní. Funkce je nutné implementovat pro tak, že vyvolá událost správné podle popisu v [dokumentace rozhraní API panelu vyhledávání](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Zobrazí výsledky hledání / tlačítko záložky – zobrazuje výsledky hledání nebo záložky ikonu na panelu Hledat
    - Zobrazuje tlačítko Zrušit – umožňuje uživatelům ukončit funkce vyhledávání. Doporučuje se, že tato možnost je vybrána.
    - Zobrazuje oboru – to umožňuje uživatelům omezit obor vyhledávání. Například při hledání v aplikaci Hudba uživatele můžete vyberte, zda chtějí hledat Apple Hudba nebo jejich knihovny pro konkrétní skladbu nebo umělcem. Chcete-li zobrazit různé možnosti, přidejte pole tituly **ScopeBarTitles** vlastnost.
    ![Hledání panelu oboru názvů](searchbar-images/image4.png)

- **Text chování** – tyto možnosti se používají k formátování vstupu uživatele při zadávání adres. Nastaví počáteční každého slova nebo věty, malá a velká písmena nebo každý znak jako velká. Oprava a kontrolu pravopisu se zobrazí výzva s návrhů na změnu slov jako jejich typ.
- **Klávesnice** – ovládací prvky styl klávesnice zobrazuje pro vstup, a proto jaké klíče jsou k dispozici na klávesnici. To zahrnuje číslo Pad, Phone Pad, e-mailu, adresa URL společně s dalšími možnostmi.
- **Vzhled** – ovládací prvky styl vzhledu klávesnici a bude buď světlý nebo light motivu.
- **Vrátí klíč** – změňte popisek na návratový klíč tak, aby lépe odrážela jaká akce se provedou. Podporované hodnoty zahrnují přejít, připojení, další, trasy, provést a vyhledávání.
- **Zabezpečení** – Určuje, jestli je maskovat vstup (například pro zadání hesla).

## <a name="related-links"></a>Související odkazy

- [Vyhledávání řadiče](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
