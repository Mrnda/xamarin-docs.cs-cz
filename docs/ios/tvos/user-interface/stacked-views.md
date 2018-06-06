---
title: Práce se zobrazeními skládaný tvOS v Xamarinu
description: Tento dokument popisuje, jak pracovat s tvOS skládaný zobrazení v aplikaci vytvořené s nástroji Xamarin. Poskytuje přehled skládaný zobrazení a popisuje automatické rozložení, umístění a velikost skládaný zobrazení, běžné používá, integrace s scénářů a další.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7e718e525c23e78fbf846209602a07bf0f3f386e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789369"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Práce se zobrazeními skládaný tvOS v Xamarinu

Ovládací prvek zobrazení zásobníku (`UIStackView`) využívá sílu automatického rozložení a velikost třídy, které slouží ke správě více dílčích zobrazení, vodorovně nebo svisle, která dynamicky reaguje na změny obsahu a velikost obrazovky zařízení Apple TV.

Rozložení všechny dílčích zobrazení připojené k zobrazení protokolů jsou spravovány podle vývojáře definované vlastnosti například osy, distribuci, zarovnání a mezery:

[![](stacked-views-images/stacked01.png "Diagram rozložení dílčí zobrazení")](stacked-views-images/stacked01.png#lightbox)

Při použití `UIStackView` v Xamarin.tvOS aplikaci, můžete definovat vývojář buď dílčích zobrazení buď uvnitř Storyboard v iOS Designer nebo přidávání a odebírání dílčích zobrazení v kódu jazyka C#.

## <a name="about-stacked-view-controls"></a>O ovládacích prvcích skládaný zobrazení 

`UIStackView` Slouží jako kontejner bez vykreslování zobrazení a jako takový není vykreslován na plátno, jako ostatní měly podtřídy `UIView`. Nastavení vlastností, jako `BackgroundColor` nebo přepsání `DrawRect` nebude mít žádný vliv visual.

Existuje několik vlastností, které řídí, jak se zobrazení zásobníku uspořádá svou kolekci dílčích zobrazení:

- **Osy** – Určuje, pokud zobrazení zásobníku uspořádá dílčích zobrazení buď **vodorovně** nebo **svisle**.
- **Zarovnání** – ovládací prvky způsob zarovnání dílčích zobrazení v rámci zásobníku zobrazení.
- **Distribuce** – Určuje, jak jsou v rámci zobrazení zásobníku dimenzované dílčích zobrazení.
- **Mezer** – ovládací prvky minimální mezera mezi každou dílčí zobrazení v zásobníku zobrazení.
- **Základní relativní** – Pokud `true`, svislé mezery v každé dílčí zobrazení bude odvozena z jeho směrného plánu.
- **Rozložení okraje relativní** – umístí dílčích zobrazení okrajů standardní rozložení.

Obvykle použijete zobrazení zásobníku uspořádat malý počet dílčích zobrazení. Složitější uživatelského rozhraní lze vytvořit pomocí vnoření jedno nebo více zobrazení zásobníku uvnitř sebe navzájem.

Uživatelská rozhraní vzhled můžete vyladit tak, že přidáte další omezení na dílčích zobrazení (například k řízení výšky a šířky). Ale potřeba dát pozor nechcete zahrnout konfliktní omezení k úpravě zavedené zobrazení zásobníku sám sebe.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Automatické rozložení a velikost třídy

Dílčí zobrazení se přidá do zobrazení zásobníku jeho rozložení je zcela řízeno této zásobníku zobrazení pomocí automatického rozložení a velikost třídy k umístění a velikost uspořádaných zobrazení.

Zobrazení zásobníku se _pin_ první a poslední dílčí zobrazení v jeho kolekci **horní** a **dolní** okrajů pro vertikální zobrazení zásobníku nebo **zbývajících**a **vpravo** okrajů pro vodorovné zásobníku zobrazení. Pokud nastavíte `LayoutMarginsRelativeArrangement` vlastnost `true`, pak zobrazení PIN kódy dílčích zobrazení pro relevantní okraje místo okraj.

Zobrazení zásobníku používá dílčí zobrazení `IntrinsicContentSize` při výpočtu velikosti dílčích zobrazení podél definovaný `Axis` (s výjimkou `FillEqually Distribution`). `FillEqually Distribution` Změní velikost všech dílčích zobrazení, aby se stejnou velikostí, proto naplnění zobrazení zásobníku společně `Axis`.

S výjimkou produktů `Fill Alignment`, zobrazení zásobníku používá dílčí zobrazení `IntrinsicContentSize` vlastnost pro výpočet velikosti zobrazení kolmé na danou `Axis`. Pro `Fill Alignment`, všechny dílčích zobrazení mají velikost tak, aby kolmé na zásobníku zobrazení danou `Axis`.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Umístění a velikost zásobníku zobrazení

Při zobrazení zásobník má plně pod kontrolou nad rozložení žádné dílčí zobrazení (na základě vlastností, jako `Axis` a `Distribution`), stále je třeba umístit zobrazení zásobníku (`UIStackView`) v jeho nadřazeném zobrazení pomocí automatického rozložení a velikost třídy.

Obecně platí to znamená Připnutí alespoň dva okraje zobrazení zásobníku a rozbalte smlouvy, proto definování jeho umístění. Bez libovolná další omezení zobrazení zásobníku se automaticky se přizpůsobí všechny jeho dílčích zobrazení následujícím způsobem:

* Velikost podél jeho `Axis` bude součet všech velikostí dílčí zobrazení a veškeré místo, která byla definována mezi každou dílčí zobrazení.
* Pokud `LayoutMarginsRelativeArrangement` vlastnost je `true`, velikost zásobníku zobrazení bude obsahovat také prostor pro pravého okraje.
* Velikost kolmé na `Axis` bude nastavena pro největší dílčí zobrazení v kolekci.

Kromě toho můžete zadat omezení pro zobrazení zásobníku **výška** a **šířka**. V takovém případě dílčích zobrazení budou rozloženy (velikostí) při vyplňování místa určeného zobrazení zásobníku určeného `Distribution` a `Alignment` vlastnosti.

Pokud `BaselineRelativeArrangement` vlastnost je `true`, dílčích zobrazení jsou navrženy podle standardních hodnot první nebo poslední dílčí zobrazení je místo použití **horní**, **dolní** nebo **Center* -  **Y** pozici. Tyto jsou vypočítávány následujícím způsobem v zásobníku zobrazení obsahu:

* Svislé zobrazení zásobníku vrátí první dílčí zobrazení směrného plánu první a poslední pro poslední. Pokud některý z těchto dílčích zobrazení jsou sami zásobníku zobrazení, se jejich účaří první nebo poslední používat.
* Vodorovné zobrazení zásobníku použije pro první a poslední základní jeho nejvyšší dílčí zobrazení. Pokud je nejvyšší zobrazení také zobrazení zásobníku, použije jeho nejvyšší dílčí zobrazení jako směrného plánu.

> [!IMPORTANT]
> Zarovnání účaří nefunguje velikostí roztažené nebo komprimovaných dílčí zobrazení, jako směrného plánu budou vypočítány na nesprávné umístění. Pro zarovnání směrného plánu, zkontrolujte, zda dílčí zobrazení **výška** odpovídá vnitřní zobrazení obsahu **výška**.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Běžná použití zobrazení zásobníku

Existuje několik typů rozložení, které funkce fungují dobře u ovládací prvky zásobníku zobrazení. Podle společnosti Apple následuje několik příkladů běžných použití:

- **Zadejte velikost společně osy** – ve Připnutí i okraje podél zobrazení zásobníku `Axis` a jedno z přiléhající okrajů nastavíte umístění, zásobník zobrazení se zvýší podél osy podle místa definované jeho dílčích zobrazení.
*  **Definování pozice dílčí zobrazení** – tím, že připnete přiléhající hrany zásobníku zobrazení nadřazeného zobrazení, bude zobrazení zásobníku růst v obou dimenzí podle jeho obsahující dílčích zobrazení.
- **Zadejte velikost a umístění zásobníku** – tím, že připnete všechny čtyři strany zásobníku zobrazení pro nadřazené zobrazení, zobrazení zásobníku uspořádá dílčích zobrazení podle definovaného v rámci zásobníku zobrazení prostoru.
*  **Zadejte velikost kolmé na ose** – ve Připnutí obě zobrazení zásobníku v kolmém směru okraje `Axis` a jeden z okrajů podél osy nastavíte umístění, zásobník zobrazení se zvýší kolmé na ose podle místa definované jeho dílčích zobrazení.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Zobrazení zásobníku a scénářů

Nejjednodušší způsob, jak pracovat se zobrazeními zásobníku v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V **řešení Pad**, dvakrát kliknete na panel `Main.storyboard` souborů a otevřete pro úpravy.
1. Návrh rozložení vaše jednotlivé elementy, které se chystáte přidat do zásobníku zobrazení: 

    [![](stacked-views-images/layout01.png "Příklad rozložení – element")](stacked-views-images/layout01.png#lightbox)
1. Přidejte všechny požadované omezení na elementy a zda že správně škálovat. Tento krok je důležitý, jakmile bude prvek přidán do zásobníku zobrazení.
1. Ujistěte se, požadovaný počet kopií, (čtyři v tomto případě): 

    [![](stacked-views-images/layout02.png "Požadovaný počet kopií")](stacked-views-images/layout02.png#lightbox)
1. Přetáhněte **zásobníku zobrazení** z **sada nástrojů** na zobrazení: 

    [![](stacked-views-images/layout03.png "Zobrazení zásobníku")](stacked-views-images/layout03.png#lightbox)
1. Vyberte zobrazení zásobníku v **pomůcky karta** z **Pad vlastnosti** vyberte **vyplnění** pro **zarovnání**, **vyplnění Stejně** pro **distribuční** a zadejte `25` pro **mezery**: 

    [![](stacked-views-images/layout04.png "Na kartě pomůcky")](stacked-views-images/layout04.png#lightbox)
1. Pozice zobrazení zásobníku na obrazovce, kam chcete ho a přidat omezení udržovat ho v požadovaném umístění.
1. Vyberte jednotlivé elementy a je přetáhněte do zobrazení zásobníku: 

    [![](stacked-views-images/layout05.png "Jednotlivé prvky v zobrazení zásobníku")](stacked-views-images/layout05.png#lightbox)
1. Je nutné upravit rozložení a elementy se uspořádány v zásobníku zobrazení na základě atributů, které nastavíte výše.
1. Přiřadit **názvy** v **pomůcky karta** z **Explorer vlastnosti** pro práci s ovládacími prvky uživatelského rozhraní v kódu jazyka C#.
1. Uložte provedené změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **Průzkumníku řešení**, dvakrát kliknete na panel `Main.storyboard` souborů a otevřete pro úpravy.
1. Návrh rozložení vaše jednotlivé elementy, které se chystáte přidat do zásobníku zobrazení: 

    [![](stacked-views-images/layout01.png "Příklad element rozložení")](stacked-views-images/layout01.png#lightbox)
1. Přidejte všechny požadované omezení na elementy a zda že správně škálovat. Tento krok je důležitý, jakmile bude prvek přidán do zásobníku zobrazení.
1. Ujistěte se, požadovaný počet kopií, (čtyři v tomto případě): 

    [![](stacked-views-images/layout02.png "Požadovaný počet kopií")](stacked-views-images/layout02.png#lightbox)
1. Přetáhněte **zásobníku zobrazení** z **sada nástrojů** na zobrazení: 

    [![](stacked-views-images/layout03-vs.png "Zobrazení zásobníku")](stacked-views-images/layout03-vs.png#lightbox)
1. Zásobník zobrazení, vyberte v **pomůcky karta** z **Explorer vlastnosti** vyberte **vyplnění** pro **zarovnání**, **vyplnění Stejně** pro **distribuční** a zadejte `25` pro **mezery**: 

    [![](stacked-views-images/layout04-vs.png "Na kartě pomůcky")](stacked-views-images/layout04-vs.png#lightbox)
1. Pozice zobrazení zásobníku na obrazovce, kam chcete ho a přidat omezení udržovat ho v požadovaném umístění.
1. Vyberte jednotlivé elementy a je přetáhněte do zobrazení zásobníku: 

    [![](stacked-views-images/layout05-vs.png "Jednotlivé prvky v zobrazení zásobníku")](stacked-views-images/layout05-vs.png#lightbox)
1. Je nutné upravit rozložení a elementy se uspořádány v zásobníku zobrazení na základě atributů, které nastavíte výše.
1. Přiřadit **názvy** v **pomůcky karta** z **Explorer vlastnosti** pro práci s ovládacími prvky uživatelského rozhraní v kódu jazyka C#.
1. Uložte provedené změny.

-----

> [!IMPORTANT]
> I když je možné přiřadit akce, jako `TouchUpInside` k prvku uživatelského rozhraní (například `UIButton`) v iOS Návrhář při vytváření obslužné rutiny události ho nebude nikdy volat protože Apple TV nemá touch obrazovky nebo podporují touch události. Je třeba použít výchozí `Action Type` při vytváření akcí pro tvOS prvky uživatelského rozhraní.

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md).

V případě našem příkladu jsme jste přístup výstupu a akce pro ovládací prvek segmentu a výstupu pro každou "player kartu". V kódu jsme skrýt a zobrazit player podle aktuální segment. Příklad:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take Action based on the segment
    switch(PlayerCount.SelectedSegment) {
    case 0:
        Player1.Hidden = false;
        Player2.Hidden = true;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 1:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 2:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = true;
        break;
    case 3:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = false;
        break;
    }
}
```

Při spuštění aplikace čtyři elementy stejně distribuované v našem zásobníku zobrazení:

[![](stacked-views-images/layout06.png "Při spuštění aplikace čtyři elementy budou rovnoměrně distribuována v našem zobrazení zásobníku")](stacked-views-images/layout06.png#lightbox)

Pokud je snížen počet přehrávačů, jsou skryté nepoužívané zobrazení a zobrazení zásobníku upravit rozložení podle:

[![](stacked-views-images/layout07.png "Pokud je snížen počet přehrávačů, jsou skryté nepoužívané zobrazení a zobrazení zásobníku upravit rozložení podle")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Naplnění zobrazení zásobníku z kódu

Kromě zcela definování obsahu a rozložení zobrazení zásobníku v iOS Designer, můžete vytvořit a dynamicky odebrat z kódu jazyka C#.

Proveďte následující příklad, který používá zásobník zobrazení pro zpracování "hvězdičky" v kontrolu (1 až 5):

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

Podívejme se na několik částí tohoto kódu podrobně. Nejprve používáme `if` příkazy, které chcete zkontrolovat, že není k dispozici více než pět "hvězdiček" nebo menší než nula.

Chcete-li přidat nový "hvězdu" se nám načíst jeho image a nastavte jeho **obsahu režimu** k **škálování aspekt podle**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Díky tomu na ikonu "hvězdičky" z poškozený, když je přidán do zásobníku zobrazení.

Potom přidáme na novou ikonu "hvězdičky" zobrazení zásobníku kolekci dílčích zobrazení:

```csharp
RatingView.AddArrangedSubview(icon);
```

Můžete si všimnout, že jsme přidali `UIImageView` k `UIStackView`na `ArrangedSubviews` vlastnost a nikoli k `SubView`. Všechna zobrazení, že chcete, aby zobrazení zásobníku k řízení jeho rozložení musí být přidán do `ArrangedSubviews` vlastnost.

Pokud chcete odebrat dílčí zobrazení ze zobrazení zásobníku, nejprve se nám získat dílčí zobrazení odebrat:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Pak je potřeba ho odebrat z obou `ArrangedSubviews` kolekce a Super zobrazení:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Odebrání dílčí zobrazení z právě `ArrangedSubviews` kolekce trvá mimo ovládací prvek zobrazení zásobníku, ale neodebere z obrazovky.

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>Dynamická změna obsahu

Zobrazení zásobníku automaticky upraví rozložení dílčích zobrazení vždy, když je dílčí zobrazení přidat, odebrat nebo skrytý. Rozložení se také upraví, pokud se upraví nějaká vlastnost zobrazení zásobníku (například jeho `Axis`).

Změny rozložení můžete animovaný tím, že je třeba v rámci bloku animace:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Řadu zásobníku zobrazit vlastnosti lze zadat pomocí velikost tříd v rámci scénáře. Tyto vlastnosti budou automaticky animovaný je odpověď na změny velikosti nebo orientace.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s skládaný zobrazení uvnitř Xamarin.tvOS aplikace.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
