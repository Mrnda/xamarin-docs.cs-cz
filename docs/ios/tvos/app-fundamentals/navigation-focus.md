---
title: Práce s tvOS, navigace a zaměřit v Xamarinu
description: Tento článek se zabývá koncept fokus a jak se používá k zobrazení a zpracování navigace v rámci Xamarin.tvOS aplikace.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2af3306b605ee802b72b159d5f2759d71d292a64
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788729"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>Práce s tvOS, navigace a zaměřit v Xamarinu

_Tento článek se zabývá koncept fokus a jak se používá k zobrazení a zpracování navigace v rámci Xamarin.tvOS aplikace._


Tento článek se zabývá koncept [fokus](#Focus-and-Selection) a jak se používají pro zpracování [navigační](#Navigation) v uživatelském rozhraní aplikace Xamarin.tvOS. Podíváme se, jak použít předdefinované tvOS ovládací prvky pro navigaci fokus, zvýraznění a výběr zajistit aplikace Xamarin.tvOS uživatelské rozhraní navigace.

[![](navigation-focus-images/intro01.png "aplikace tvOS navigační uživatelské rozhraní")](navigation-focus-images/intro01.png#lightbox)

V dalším kroku provedeme podívejte se na použití fokus s [paralaxy](#Focus-and-Parallax) a *na základě bitové kopie* zajistit visual různá vodítka pro aktuální stav navigace pro koncového uživatele.

Nakonec se podíváme práce s [fokus](#Working-with-Focus), [fokus aktualizace](#Working-with-Focus-Updates), [fokus příručky](#Working-with-Focus-Guides), [fokusu v kolekcích](#Working-with-Focus-in-Collections) a [ Povolení paralaxy](#Enabling-Parallax) na zobrazení bitové kopie ve svých aplikacích Xamarin.tvOS.

<a name="Navigation" />

## <a name="navigation"></a>Navigace

Uživatele vaší aplikace Xamarin.tvOS nebude interakci s jeho rozhraní přímo jako s iOS kde jejich klepněte na Image na displeji zařízení, ale nepřímo z napříč místnosti pomocí [Siri vzdálené](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Musíte se to mít na paměti, při navrhování vaší aplikace uživatelského rozhraní tak, aby přirozeně toky, ale ponechá ponořené Apple TV prostředí uživatele.

Úspěšné tvOS aplikace implementuje navigační způsobem, který bez problémů podporuje účel aplikace a struktura dat, která představuje bez volání pozornost navigační sám sebe. Návrh vašeho navigační tak, aby se domnívá, přirozené a známým bez dominantní uživatelského rozhraní nebo kreslení fokus od obsah a činnost koncového uživatele aplikace.

[![](navigation-focus-images/nav01.png "V aplikaci nastavení tvOS")](navigation-focus-images/nav01.png#lightbox)

Při použití Apple TV, uživatel obvykle přejde na skládaný sadu obrazovky, každý prezentací danou sadu obsahu. Pak může vést každé nové obrazovky na jeden nebo více dílčích obrazovky obsahu pomocí standardní ovládací prvky uživatelského rozhraní, jako [tlačítka](~/ios/tvos/user-interface/buttons.md), [kartě řádky](~/ios/tvos/user-interface/tab-bars.md), tabulky, [zobrazení kolekce](~/ios/tvos/user-interface/collection-views.md) nebo [ Rozdělení zobrazení](~/ios/tvos/user-interface/split-views.md).

Každé nové obrazovce dat uživatel přejde hlubší do tohoto balíčku obrazovky. Pomocí **nabídky** tlačítko na vzdáleném Siri, můžete procházet zpětné zásobníku vrátit na předchozí obrazovce nebo hlavní nabídky.

Apple naznačuje, přičemž následující mějte při navrhování navigace pro tvOS aplikace:

- **Rozložení vytvořit navigační zkontrolujte hledání obsahu rychle a snadno** -chcete uživatele pro přístup k obsahu v rámci vaší aplikace v nejmenšího počtu odposlouchávání, klikne a swipes míře. Vytvořit navigační zjednodušit a uspořádání obsahu minimální množství obrazovky.
- **Vytvoření kapaliny rozhraní pomocí Touch** – Ujistěte se, že uživatel můžete přesunout mezi _může získat fokus položky_ s minimálním třecí pomocí nejmenšího počtu gesta možné.
- **Návrh s fokusem pamatovat** -vzhledem k tomu, že uživatel komunikuje s obsahem v místnosti, je nutné přesunout fokus na položku uživatelské rozhraní před interakci s pomocí vzdáleného Siri. Uživatelé budou dostávat frustrovaní s vaší aplikací, pokud vyžaduje příliš mnoho gesta mohly dosáhli svých cílů.
- **Poskytování zpětné navigační prostřednictvím tlačítko nabídky** – Pokud chcete vytvořit snadnou prostředí, umožnit uživatelům procházení zpětné pomocí vzdáleného Siri **nabídky** tlačítko. Stisknutím **nabídky** tlačítko vždy by měla vrátit na předchozí obrazovku nebo vrátit do hlavní nabídky aplikace. V aplikaci na nejvyšší úrovni, stisknete **nabídky** tlačítko by měla vrátit k obrazovce Apple TV Domů.
- **Obvykle nepoužívejte zobrazení tlačítka Zpět** – protože stisknutím **nabídky** tlačítko na vzdáleném Siri přejde zpětné na obrazovce zásobníku, vyhněte se zobrazení další ovládací prvek, který duplikuje toto chování. Výjimkou z tohoto pravidla je k nákupu obrazovky nebo obrazovky s destruktivní akcí (například odstranění obsahu) kde **zrušit** tlačítko by měl být také zobrazen.
- **Zobrazit rozsáhlých kolekcí na jedné obrazovce, namísto mnoho** -vzdálené Siri byla navržena tak, aby procházení velkých skupin obsah rychlé a snadné pomocí gesta. Pokud vaše aplikace funguje s velké kolekce položek může získat fokus, vezměte v úvahu je uložíte na jedné obrazovce místo je rozdělení do mnoha obrazovky, které vyžadují další navigace v rámci daného uživatele.
- **Používejte standardní ovládací prvky pro navigaci** -znovu, pokud chcete vytvořit snadnou uživatelské prostředí, kde je to možné, používat integrované `UIKit` ovládací prvky, jako je například ovládací prvky stránky, karta řádky, Segmentovaným ovládacích prvků, zobrazení tabulek, zobrazení kolekcí a rozdělení Zobrazení pro navigaci vaší aplikace. Vzhledem k tomu, že uživatel obeznámeni s těmito prvky, budou intuitivně moci přejděte vaší aplikace.
- **Upřednostnit vodorovný obsahu navigační** -charakter Apple TV, k načtení zleva doprava na vzdáleném Siri je přirozenější než nahoru či dolů. Zvažte tuto možnost, při navrhování obsahu rozložení pro vaši aplikaci.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>Fokus a výběr

Na Apple TV, bitovou kopii, tlačítko nebo jiného uživatelského rozhraní elementu se považuje za _nezvýrazní_ Pokud je cílem aktuální navigace.

[![](navigation-focus-images/focus01.png "Příklad fokus a výběr")](navigation-focus-images/focus01.png#lightbox)

Na rozdíl od zařízení iOS, kde je uživatel přímo interakci s elementů na dotykovou obrazovku zařízení uživatelé komunikovat s tvOS elementy z napříč místnosti pomocí vzdáleného Siri. K dispozici a zpracovávají interakci s uživatelem, Apple TV používá _fokus_ na základě modelu.

Pomocí gesta a tlačítko stiskem tlačítka [Siri vzdálené](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), uživatel přesune fokus z prvky uživatelského rozhraní na jiný. Jakmile fokus posunula na požadovaný element, uživatel kliknutím na Vybrat obsah nebo aktivovat akce reprezentována daný element.

Jako změní fokus jemně animace a efekty (například paralaxy vliv na obrázky) se používají pro jednoznačnou identifikaci položek uživatelské rozhraní, který má právě fokus.

Společnost Apple má následující návrhy pro práci s fokus a výběr:

- **Použít integrované ovládací prvky uživatelského rozhraní pro efekty pohybu** – pomocí `UIKit` a rozhraní API fokusu v uživatelském rozhraní, modelu fokus automaticky použije výchozí pohybu a vizuální efekty k vaší elementům uživatelského rozhraní. To usnadňuje aplikace myslíte, že nativní a pro uživatele na platformě Apple TV a umožňuje plynulá práce a intuitivní přesun mezi položkami může získat fokus.
- **Přesun zaměření v očekává směrech** -na Apple TV, téměř každý element používá nepřímých manipulaci. Například uživatel používá vzdálené Siri přesunout fokus a systém automaticky posune rozhraní, které chcete zachovat aktuálně nastavený fokus položky viditelné. Pokud aplikace implementuje tento typ interakce, ujistěte se, že se aktivuje ve směru gesto uživatele. Pokud uživatel potažením prstem přejděte přímo na vzdálené Siri fokus přesuňte vpravo (to by mohlo způsobit přejděte na levé straně obrazovky). Jedinou výjimkou tohoto pravidla je celá obrazovka položky, které používají přímá manipulace (kde se k načtení přesune element nahoru).
- **Zkontrolujte, zda je položka zaměřuje Obvious** -vzhledem k tomu, že uživatelé jsou interakci s prvky uživatelského rozhraní od afar, je důležité, aby aktuálně nastavený fokus položka vystupoval. Obvykle to bude zpracovávat automaticky integrovanou `UIKit` elementy. Pro vlastní ovládací prvky znázornit funkce, jako je velikost položek nebo stínové fokus.
- **Použít paralaxy k zkontrolujte zaměřuje položky přizpůsobivý** – malé cyklické gesta na vzdáleném Siri mít za následek postupně, v reálném čase přesun cílených položky. To [paralaxy vliv](#Focus-and-Parallax) je integrovaná do `UIKit` _na základě bitové kopie_ poskytuje představu o připojení k položce cílených uživateli.
- **Vytvoření může získat fokus položek odpovídající velikost** -velké položky s dostatečným mezery se snadněji a vyberte přejděte než menší položky.
- **Návrh uživatelského rozhraní Element vypadat dobrý buď zaměřuje nebo Unfocused** – obvykle Apple TV představuje položku zaměřuje zvýšením jeho velikost. Zajistěte, aby vaše aplikace prvky uživatelského rozhraní vypadají skvěle na libovolnou velikost prezentace a v případě potřeby zadejte prostředků pro větší velikostí prvky.
- **Fokus změny plynule představují** -plynule objevovat mezi položkami pomocí animace **Focused** a **Unfocused** stavu zachovat jarring přechodů ze vrácení.
- **Nezobrazí kurzoru** -uživatelé očekávají, že se orientovat uživatelského rozhraní vaší aplikace pomocí fokus a není přesunutím kurzoru po obrazovce. Uživatelské rozhraní měli vždycky používat Model fokus k dispozici konzistentní podmínky koncového uživatele.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Práce s fokusem

Může být pokusů, které chcete vytvořit vlastní ovládací prvek, který se může stát položku může získat fokus. Pokud tak přepsat `CanBecomeFocused` vlastností a vraťte se `true`, jinak vrátí `false`. Příklad:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

Kdykoli můžete použít `Focused` vlastnost `UIKit` řízení zkontrolujte, jestli je aktuální položky. Pokud `true` položku uživatelského rozhraní má právě fokus, jinak je nepoužívá. Příklad:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Pokud nemůžete přesunout přímo fokus jiný element uživatelského rozhraní pomocí kódu, můžete určit, které elementy uživatelského rozhraní nejdřív získá fokus při načtení obrazovky nastavením jeho `PreferredFocusedView` vlastnost `true`. Příklad:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Práce s fokus aktualizace 

Když způsobí, že uživatel zaměření přesune na jiné (například v reakci na gesto na vzdáleném Siri) od jednoho prvku uživatelského rozhraní _událost aktualizace aktivace_ odešlou do položky došlo ke ztrátě fokus a položka získat fokus.

Pro vlastní elementy, které dědí od `UIView` nebo `UIViewController`, můžete přepsat několik metod pro práci s událost aktualizace aktivace:

- **DidUpdateFocus** -tato metoda bude volána kdykoli zobrazení získá nebo ztratí fokus.
- **ShouldUpdateFocus** – pomocí této metody můžete definovat, kde je povoleno přesunout fokus.

Žádosti, že modul fokus přesune fokus zpět do `PreferredFocusedView` elementu uživatelského rozhraní, volání `SetNeedsUpdateFocus` metoda řadiče zobrazení.

> [!IMPORTANT]
> Volání metody `SetNeedsUpdateFocus` má vliv pouze pokud je volána před řadiče zobrazení obsahuje zobrazení, který má právě fokus.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Práce s fokus příručky

Modul fokus součástí tvOS je skvělým na zpracování pohybů mezi položkami, které spadají na vodorovné nebo svislé mřížky. Obvykle je potřeba udělat nic více než přidat že elementy uživatelského rozhraní pro návrhu rozhraní a modul fokus automaticky zpracuje přesun mezi těmito elementy bez zásahu vývojáře.

Však může nastat situace, z důvodu životní potřeby vašeho návrhu uživatelského rozhraní při prvky uživatelského rozhraní není přejít na vodorovné nebo svislé mřížky a může být nedostupná, protože jsou diagonálních navzájem. K tomu dochází, protože modul fokus byla navržena pro zpracování jednoduché nahoru, dolů, doleva a doprava přesun mezi pouze položky uživatelského rozhraní.

Proveďte následující rozložení uživatelského rozhraní pro příklad:

 [![](navigation-focus-images/guide01.png "Práce s příklad fokus příručky")](navigation-focus-images/guide01.png#lightbox)
 
Protože **Další informace o** tlačítko nespadá na vodorovného a svislého mřížka s **koupit** tlačítko je dostupná pro uživatele. Však tento problém můžete snadno odstranit pomocí _fokus průvodce_ zajistit pohyb Nápověda k modulu fokus. 

Průvodce fokus (`UIFocusGuide`) zpřístupní neviditelné oblasti zobrazení jako Focusable k modulu fokus, což umožňuje zaměřit přesměrovat do jiného zobrazení.

Ano, v příkladu výše uvedených vyřešíte následující kód nebylo možné přidat řadiče zobrazení vytvořit průvodce fokus mezi **Další informace o** a **koupit** tlačítka:

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

První, nový `UIFocusGuide` je vytvořen a přidán do zobrazení rozložení průvodce kolekce pomocí `AddLayoutGuide` metoda.

V dalším kroku upravena průvodci fokus Top, vlevo, šířka a výška kotvy vzhledem k **Další informace o** a **koupit** tlačítka na pozici mezi nimi. Další informace:

[![](navigation-focus-images/guide02.png "Příklad fokus Průvodce")](navigation-focus-images/guide02.png#lightbox)

Je také důležité si uvědomit, že nové omezení jsou aktivované jako je vytvořen nastavením své `Active` vlastnost `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Pomocí nového Průvodce fokus vytvořit a přidat do zobrazení, řadiče zobrazení `DidUpdateFocus` lze metodu přepsat a přidat následující kód pro přesun mezi **Další informace o** a **koupit** tlačítka:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

První, tento kód get `NextFocusedView` z `UIFocusUpdateContext` který byl předán v (`context`). Pokud je toto zobrazení `null`, je nutné žádné zpracování a metodu byl ukončen.

Dále `nextFocusableItem` vyhodnotí. Pokud odpovídá buď **Další informace o** nebo **koupit** tlačítka, fokus posílá tlačítko opačné pomocí Průvodce fokus `PreferredFocusedView` vlastnost. Příklad:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

V případě, že ani tlačítko je zdroj k posunu fokus `PreferredFocusedView` není zaškrtnuta vlastnost:

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Práce s fokusem v kolekcích

Při rozhodování, zda lze jednotlivé položky může získat fokus v `UICollectionView` nebo `UITableView`, budete přepsání metody `UICollectionViewDelegate` nebo `UITableViewDelegate` v uvedeném pořadí. Příklad:

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

`CanFocusItem` Metoda vrátí `true` , zda aktuální položka může být aktivní, jinak vrátí hodnotu `false`.

Pokud chcete `UICollectionView` nebo `UITableView` mějte na paměti a obnovit fokus na poslední položky při ztratí a znovu získal fokus, nastavte `RemembersLastFocusedIndexPath` vlastnost `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Fokus a paralaxy

Jak jsme uvedli výše, uživatelské rozhraní elementu se považuje za _nezvýrazní_ když je aktuální položky během události navigace. Obvykle dodává položku do fokus se mírně zvýší jeho velikost a vizuálně zvýšenými pomocí stínu. 

Pokud uživatel zadá gesto pomalé, cyklické na vzdáleném Siri, bude sway v reálném čase v reakci na tento přesun zaměřuje položky. Jak boční výkyvy dojde, osvětlených Šín se použijí na jeho image provedení prostor pravděpodobně Vylepšete. Po danou dobu nečinnosti veškerý obsah na více systémů fokus času pro ztlumení a Focused položky se zvýší i větší. 

Tyto důsledky společně poskytují duševní připojení mezi obsah na obrazovce TV a interakci s Apple TV z pohovce uživatele.

Na Apple TV je tento efekt paralaxy používán v celém systému k předávání představu o 3D hloubkou a dynamics zaměřit položky. tvOS používá řadu transparentní, [na základě bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) který přesune a dynamicky škáluje k vytvoření této vliv.

Vrstvený Image jsou vyžadovány pro aplikaci tvOS ikonu a je podporována pro dynamický obsah police Top. Není požadováno Apple důrazně doporučuje, implementace na základě bitové kopie v ostatních může získat fokus položkách v uživatelském rozhraní vaší aplikace.

### <a name="enabling-parallax"></a>Povolení paralaxy

`UIImageView` Ovládací prvek (nebo libovolný ovládací prvek, který dědí z `UIImageView`) automaticky podporuje paralaxy účinek. Ve výchozím nastavení je tato podpora je zakázané, chcete-li ji povolit, použijte následující kód:

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

S Tato vlastnost nastavena na `true`, zobrazení bitové kopie automaticky získají paralaxy účinek, pokud je vybrána, uživatel a zaměřit.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých koncept fokus a jak se používají pro zpracování navigace v uživatelském rozhraní aplikace Xamarin.tvOS. Ho zkontrolujte, jak použít předdefinované tvOS ovládací prvky pro navigaci fokus, zvýraznění a výběr zajistit navigace. V dalším kroku hledá toho, jak můžete využít fokus s paralaxy a na základě bitových kopií zajistit visual různá vodítka pro aktuální stav navigace pro koncového uživatele. Nakonec ověřuje práce se zaměřit, fokus aktualizace fokusu v kolekcích a povolení paralaxy.




## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
