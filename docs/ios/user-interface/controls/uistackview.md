---
title: Zobrazení zásobníku v Xamarin.iOS
description: Tento článek se týká použití nové aplikace Xamarin.iOS ovládacího prvku UIStackView ke správě sadu dílčích zobrazení buď vodorovně nebo svisle uspořádaných zásobníku.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a894efebe4089adefeb02007bd394c13fc77974c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790097"
---
# <a name="stack-views-in-xamarinios"></a>Zobrazení zásobníku v Xamarin.iOS

_Tento článek se týká použití nové aplikace Xamarin.iOS ovládacího prvku UIStackView ke správě sadu dílčích zobrazení buď vodorovně nebo svisle uspořádaných zásobníku._

> [!IMPORTANT]
> Upozorňujeme, že když StackView je podporované v iOS Designer, můžete setkat s použitelnost chyby při použití stabilní kanál. Přepnutí Beta nebo Alpha kanálů by se tím tento problém. Rozhodli jsme k dispozici Tento názorný postup pomocí Xcode, dokud opravy, vyžaduje se implementují ve stabilní kanál.

Ovládací prvek zobrazení zásobníku (`UIStackView`) využívá sílu automatického rozložení a velikost třídy, které slouží ke správě více dílčích zobrazení, vodorovně nebo svisle, která dynamicky reaguje na velikost orientaci a obrazovky zařízení s iOS.

Rozložení všechny dílčích zobrazení připojené k zobrazení protokolů jsou spravovány podle vývojáře definované vlastnosti například osy, distribuci, zarovnání a mezery:

[![](uistackview-images/stacked01.png "Diagram rozložení zobrazení zásobníku")](uistackview-images/stacked01.png#lightbox)

Při použití `UIStackView` v aplikaci Xamarin.iOS, můžete definovat vývojář buď dílčích zobrazení buď uvnitř Storyboard v iOS Designer nebo přidávání a odebírání dílčích zobrazení v kódu jazyka C#.

Tento dokument se skládá ze dvou částí: rychlý start pomůže implementace zobrazit vaše první zásobník a potom některé další technické podrobnosti o tom, jak funguje.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView, pomocí [univerzity Xamarin](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>UIStackView rychlý start

Jako rychlý úvod do `UIStackView` řízení, jsme se chystáte vytvořit jednoduché rozhraní, které umožní, aby uživatel zadal hodnocení od 1 do 5. Budeme používat dvě zobrazení zásobník: jeden uspořádat rozhraní svisle na zařízení obrazovky a jeden uspořádat ikony hodnocení 1-5 vodorovně po obrazovce.

### <a name="define-the-ui"></a>Definování uživatelského rozhraní

Spuštění nového projektu Xamarin.iOS a upravit **Main.storyboard** souboru v Xcode na rozhraní tvůrce. Nejdřív přetáhněte jeden **svislé zobrazení zásobníku** na **View Controller**:

[![](uistackview-images/quick01.png "Přetáhněte jednoho svislé zásobníku zobrazení na řadiče zobrazení")](uistackview-images/quick01.png#lightbox)

V **atribut Inspector**, nastavte následující možnosti:

[![](uistackview-images/quick02.png "Nastavit možnosti zobrazení zásobníku")](uistackview-images/quick02.png#lightbox)

Kde:

- **Osy** – Určuje, pokud zobrazení zásobníku uspořádá dílčích zobrazení buď **vodorovně** nebo **svisle**.
- **Zarovnání** – ovládací prvky způsob zarovnání dílčích zobrazení v rámci zásobníku zobrazení.
- **Distribuce** – Určuje, jak jsou v rámci zobrazení zásobníku dimenzované dílčích zobrazení.
- **Mezer** – ovládací prvky minimální mezera mezi každou dílčí zobrazení v zásobníku zobrazení.
- **Základní relativní** – Pokud je zaškrtnuto, bude svislé mezery v každé dílčí zobrazení odvozena z jeho směrného plánu.
- **Rozložení okraje relativní** – umístí dílčích zobrazení okrajů standardní rozložení.

Při práci se zobrazením zásobníku, si můžete představit **zarovnání** jako **X** a **Y** umístění dílčí zobrazení a **distribuční** jako **Výška** a **šířka**.

> [!IMPORTANT]
> `UIStackView` slouží jako kontejner bez vykreslování zobrazení a jako takový není vykreslován na plátno, jako ostatní měly podtřídy `UIView`. Nastavení vlastností, jako například `BackgroundColor` nebo přepsání `DrawRect` nebude mít žádný vliv visual.

Dál rozložení aplikace rozhraní přidáním štítek, ImageView, dvě tlačítka a vodorovné zásobníku zobrazení tak, aby vypadá přibližně takto:

[![](uistackview-images/quick03.png "Rozložení uživatelského rozhraní zobrazení zásobníku")](uistackview-images/quick03.png#lightbox)

Vodorovné zobrazení zásobníku nakonfigurujte následující možnosti:

[![](uistackview-images/quick04.png "Konfigurovat možnosti vodorovné zobrazení zásobníku")](uistackview-images/quick04.png#lightbox)

Protože Neradi bychom ikonu, která představuje každý "místo" v hodnocení na dojít k roztažení když je přidán do vodorovné zásobníku zobrazení, jsme nastavili **zarovnání** k **Center** a  **Distribuce** k **vyplnění stejně**.

Nakonec propojit si následující **výstupy** a **akce**:

[![](uistackview-images/quick05.png "Výstupy zobrazení zásobníku a akcí")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Naplnění UIStackView z kódu

Vrátí k sadě Visual Studio pro Mac a upravit **ViewController.cs** souboru a přidejte následující kód:

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

### <a name="testing-the-ui"></a>Testování uživatelského rozhraní

S všechny požadované prvky uživatelského rozhraní a kódu v místě můžete nyní spustit a otestovat rozhraní. Když se zobrazí uživatelské rozhraní, všechny elementy ve svislém zobrazení zásobníku se být rovnoměrně rozmístěny shora dolů.

Když uživatel klepnutím **zvýšit hodnocení** tlačítko jiné "hvězdičky" se přidá na obrazovku (až do maximálního počtu 5):

[![](uistackview-images/intro01.png "Ukázková aplikace spustit")](uistackview-images/intro01.png#lightbox)

"Hvězdiček" bude automaticky zarovnaný na střed a rovnoměrně rozložena mezi vodorovné zobrazení zásobníku. Když uživatel klepnutím **snížit hodnocení** tlačítko "hvězdu" se odebere (dokud žádné nezbývají).

## <a name="stack-view-details"></a>Zobrazení podrobností zásobníku

Teď, když máme obecnou představu o co `UIStackView` ovládacího prvku a jak funguje, podívejme hlubší pohled na některé funkce a podrobnosti.

### <a name="auto-layout-and-size-classes"></a>Automatické rozložení a velikost třídy

Jak jsme viděli výše, pokud dílčí zobrazení je přidán do zásobníku zobrazení jeho rozložení úplně řídí tento zásobník zobrazení pomocí automatického rozložení a velikost třídy k umístění a velikost uspořádaných zobrazení.

Zobrazení zásobníku se _pin_ první a poslední dílčí zobrazení v jeho kolekci **horní** a **dolní** okrajů pro vertikální zobrazení zásobníku nebo **zbývajících**a **vpravo** okrajů pro vodorovné zásobníku zobrazení. Pokud nastavíte `LayoutMarginsRelativeArrangement` vlastnost `true`, pak zobrazení PIN kódy dílčích zobrazení pro relevantní okraje místo okraj.

Zobrazení zásobníku používá dílčí zobrazení `IntrinsicContentSize` při výpočtu velikosti dílčích zobrazení podél definovaný `Axis` (s výjimkou `FillEqually Distribution`). `FillEqually Distribution` Změní velikost všech dílčích zobrazení, aby se stejnou velikostí, proto naplnění zobrazení zásobníku společně `Axis`.

S výjimkou produktů `Fill Alignment`, zobrazení zásobníku používá dílčí zobrazení `IntrinsicContentSize` vlastnost pro výpočet velikosti zobrazení kolmé na danou `Axis`. Pro `Fill Alignment`, všechny dílčích zobrazení mají velikost tak, aby kolmé na zásobníku zobrazení danou `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Umístění a velikost zásobníku zobrazení

Při zobrazení zásobník má plně pod kontrolou nad rozložení žádné dílčí zobrazení (na základě vlastností, jako `Axis` a `Distribution`), stále je třeba umístit zobrazení zásobníku (`UIStackView`) v jeho nadřazeném zobrazení pomocí automatického rozložení a velikost třídy.

Obecně platí to znamená Připnutí alespoň dva okraje zobrazení zásobníku a rozbalte smlouvy, proto definování jeho umístění. Bez libovolná další omezení zobrazení zásobníku se automaticky se přizpůsobí všechny jeho dílčích zobrazení následujícím způsobem:

 - Velikost podél jeho `Axis` bude součet všech velikostí dílčí zobrazení a veškeré místo, která byla definována mezi každou dílčí zobrazení.
 - Pokud `LayoutMarginsRelativeArrangement` vlastnost je `true`, velikost zásobníku zobrazení bude obsahovat také prostor pro pravého okraje.
 - Velikost kolmé na `Axis` bude nastavena pro největší dílčí zobrazení v kolekci.

Kromě toho můžete zadat omezení pro zobrazení zásobníku **výška** a **šířka**. V takovém případě dílčích zobrazení budou rozloženy (velikostí) při vyplňování místa určeného zobrazení zásobníku určeného `Distribution` a `Alignment` vlastnosti.

Pokud `BaselineRelativeArrangement` vlastnost je `true`, dílčích zobrazení jsou navrženy podle standardních hodnot první nebo poslední dílčí zobrazení je místo použití **horní**, **dolní** nebo **Center* -  **Y** pozici. Tyto jsou vypočítávány následujícím způsobem v zásobníku zobrazení obsahu:

 - Svislé zobrazení zásobníku vrátí první dílčí zobrazení směrného plánu první a poslední pro poslední. Pokud některý z těchto dílčích zobrazení jsou sami zásobníku zobrazení, se jejich účaří první nebo poslední používat.
 - Vodorovné zobrazení zásobníku použije pro první a poslední základní jeho nejvyšší dílčí zobrazení. Pokud je nejvyšší zobrazení také zobrazení zásobníku, použije jeho nejvyšší dílčí zobrazení jako směrného plánu.

> [!IMPORTANT]
> Zarovnání účaří nefunguje velikostí roztažené nebo komprimovaných dílčí zobrazení, jako směrného plánu budou vypočítány na nesprávné umístění. Pro zarovnání směrného plánu, zkontrolujte, zda dílčí zobrazení **výška** odpovídá vnitřní zobrazení obsahu **výška**.

### <a name="common-stack-view-uses"></a>Běžná použití zobrazení zásobníku

Existuje několik typů rozložení, které funkce fungují dobře u ovládací prvky zásobníku zobrazení. Podle společnosti Apple následuje několik příkladů běžných použití:

- **Zadejte velikost společně osy** – ve Připnutí i okraje podél zobrazení zásobníku `Axis` a jedno z přiléhající okrajů nastavíte umístění, zásobník zobrazení se zvýší podél osy podle místa definované jeho dílčích zobrazení.
 -  **Definování pozice dílčí zobrazení** – tím, že připnete přiléhající hrany zásobníku zobrazení nadřazeného zobrazení, bude zobrazení zásobníku růst v obou dimenzí podle jeho obsahující dílčích zobrazení.
- **Zadejte velikost a umístění zásobníku** – tím, že připnete všechny čtyři strany zásobníku zobrazení pro nadřazené zobrazení, zobrazení zásobníku uspořádá dílčích zobrazení podle definovaného v rámci zásobníku zobrazení prostoru.
 -  **Zadejte velikost kolmé na ose** – ve Připnutí obě zobrazení zásobníku v kolmém směru okraje `Axis` a jeden z okrajů podél osy nastavíte umístění, zásobník zobrazení se zvýší kolmé na ose podle místa definované jeho dílčích zobrazení.

### <a name="managing-the-appearance"></a>Správa vzhledu

`UIStackView` Slouží jako kontejner bez vykreslování zobrazení a jako takový není vykreslován na plátno, jako ostatní měly podtřídy `UIView`. Nastavení vlastností, jako `BackgroundColor` nebo přepsání `DrawRect` nebude mít žádný vliv visual.

Existuje několik vlastností, které řídí, jak se zobrazení zásobníku uspořádá svou kolekci dílčích zobrazení:

- **Osy** – Určuje, pokud zobrazení zásobníku uspořádá dílčích zobrazení buď **vodorovně** nebo **svisle**.
- **Zarovnání** – ovládací prvky způsob zarovnání dílčích zobrazení v rámci zásobníku zobrazení.
- **Distribuce** – Určuje, jak jsou v rámci zobrazení zásobníku dimenzované dílčích zobrazení.
- **Mezer** – ovládací prvky minimální mezera mezi každou dílčí zobrazení v zásobníku zobrazení.
- **Základní relativní** – Pokud `true`, svislé mezery v každé dílčí zobrazení bude odvozena z jeho směrného plánu.
- **Rozložení okraje relativní** – umístí dílčích zobrazení okrajů standardní rozložení.

Obvykle použijete zobrazení zásobníku uspořádat malý počet dílčích zobrazení. Složitější uživatelského rozhraní lze vytvořit pomocí vnoření jeden nebo více zobrazení zásobníku v sobě navzájem (podle kroků v [rychlý start UIStackView](#UIStackView-Quickstart) výše).

Uživatelská rozhraní vzhled můžete vyladit tak, že přidáte další omezení na dílčích zobrazení (například k řízení výšky a šířky). Ale potřeba dát pozor nechcete zahrnout konfliktní omezení k úpravě zavedené zobrazení zásobníku sám sebe.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Zachování uspořádané zobrazení a konzistence dílčí zobrazení

Zobrazení zásobníku zajistí, že jeho `ArrangedSubviews` vlastnost je vždy podmnožinou své `Subviews` vlastnost pomocí následujících pravidel:

 - Pokud dílčí zobrazení je přidán do `ArrangedSubviews` kolekce, bude automaticky přidán do `Subviews` kolekci (Pokud je již součástí této kolekce).
 - Pokud dílčí zobrazení je odebrán z `Subviews` kolekce (odebral z zobrazení), se odebere i z `ArrangedSubviews` kolekce.
 - Odebrání dílčí zobrazení z `ArrangedSubviews` kolekce nedojde k jeho odebrání z `Subviews` kolekce. Proto budou rozloženy už v zásobníku zobrazení, ale stále bude zobrazovat na obrazovce.

`ArrangedSubviews` Kolekce je vždy podmnožinou `Subview` kolekce, ale pořadí jednotlivých dílčích zobrazení v každé kolekci je samostatný a pod kontrolou následující:

 - Pořadí dílčích zobrazení v rámci `ArrangedSubviews` kolekce určit pořadí jejich zobrazení v rámci zásobníku.
 - Pořadí dílčích zobrazení v rámci `Subview` kolekce určuje jejich pořadí Z-Order (nebo rozvrstvení) v rámci zobrazení zpět na popředí.

### <a name="dynamically-changing-content"></a>Dynamická změna obsahu

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

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých nové `UIStackView` řízení (pro iOS 9) ke správě sadu dílčích zobrazení v buď vodorovně nebo svisle uspořádaných zásobníku v aplikaci pro Xamarin.iOS.
Začal s jednoduchý příklad vytvoření uživatelského rozhraní pomocí zobrazení zásobníku a bylo dokončeno s podrobnější pohled na zobrazení zásobníku a jejich vlastnosti a funkce.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [Co je nového v iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Odkaz na UIStackView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Představení UIStackView (video)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
