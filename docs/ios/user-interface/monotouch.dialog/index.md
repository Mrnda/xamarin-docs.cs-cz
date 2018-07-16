---
title: Úvod do MonoTouch.Dialog pro Xamarin.iOS
description: Tento dokument popisuje MonoTouch.Dialog (MT.) D), rozhraní pro rychlé, deklarativní vývoj uživatelského rozhraní s Xamarin.iOS. Popisuje, jak používat rozhraní API MonoTouch.Dialog vytváření rozhraní v kódu nebo JSON a používání funkcí, jako je o přijetí změn – aktualizace, hledání, načítání obrázků na pozadí a další.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bee4b460552c7273021b16955b52ba3d95d3e07c
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038401"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Úvod do MonoTouch.Dialog pro Xamarin.iOS

MonoTouch.Dialog označovány jako pro MT. D zkráceně, je rychlé toolkit vývoj uživatelského rozhraní, která umožňuje vývojářům vytvářet aplikace obrazovky a navigace pomocí informace spíše než můžete vytvořit kontrolery zobrazení tabulky, atd. V důsledku toho poskytuje výrazné zjednodušení vývoje a kód snížení uživatelského rozhraní. Zvažte například na následujícím snímku obrazovky:

 [![](images/image1.png "Zvažte například tento snímek obrazovky")](images/image1.png#lightbox)

Následující kód se používá k definování této celé obrazovky:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Při práci s tabulkami v iOS, je často spoustu automatizujete kódu.
Například pokaždé, když je potřeba tabulku, je potřeba zdroje dat k vyplnění tabulky. Každou obrazovku v aplikaci, která má dvě obrazovky založené na tabulce, které jsou připojené přes kontroler navigace, sdílí mnoho stejný kód.

PRO MT. D, která zjednodušuje tím, že zapouzdření všech tohoto kódu do obecného rozhraní API pro vytvoření tabulky. Pak poskytuje abstrakce vycházející z tohoto rozhraní API, který umožňuje deklarativní objektu vazby syntaxe, která ještě více zjednodušuje. V důsledku toho dvě rozhraní API ve nejsou k dispozici pro MT. D:

-   **Rozhraní API nízké úrovně prvky** – *Elements API* je založen na vytváření hierarchická stromové struktuře prvků, které představují obrazovek a jejich komponent. Rozhraní Elements API poskytuje vývojářům největší flexibilitu a řízení při vytváření uživatelského rozhraní. Kromě toho rozhraní Elements API má rozšířenou podporu pro deklarativní definice prostřednictvím formátu JSON, která umožňuje jak neuvěřitelně rychlá prohlášení, tak i dynamické generování uživatelského rozhraní ze serveru. 
-   **Rozhraní API vysoké úrovně reflexe** – označované také jako *vazby**rozhraní API* , do které třídy jsou opatřeny poznámkami s uživatelské rozhraní pomocné parametry a potom pro MT. D automaticky vytvoří obrazovky založené na objektech a poskytuje vazby mezi co je zobrazen (a volitelně upravovat) na obrazovce a základního objektu zálohování.   Výše uvedený příklad znázorněný využívání Reflection API. Toto rozhraní API neposkytuje podrobné ovládací prvek, který nemá prvky rozhraní API, ale to snižuje složitost ještě více tak automaticky vytvářela hierarchie elementů na základě atributů třídy. 


PRO MT. D obsahuje balené s rozsáhlou sadou integrovaným prvky uživatelského rozhraní pro vytvoření obrazovky, ale také rozpozná potřeby pro vlastní elementy a rozložení pokročilé obrazovky. V důsledku toho rozšíření je prvotřídní doporučených dokončené do rozhraní API. Vývojáři mohou rozšířit stávající elementy nebo vytvářet nové a bezproblémově integrují.

Kromě toho pro MT. D má celou řadu běžných funkcí uživatelského rozhraní iOS součástí, jako jsou "o přijetí změn – aktualizace" podpora asynchronního načítání, obrázků a podpora vyhledávání.

Tento článek bude trvat podrobný přehled práce se pro MT. D, včetně:

-   **PRO MT. Součásti D** – to se zaměřuje na principy třídy, které tvoří pro MT. Povolit získávání rychle se Naučte D. 
-   **Referenční dokumentace elementů** – úplný seznam předdefinovaných prvků pro MT. D. 
-   **Rozšířené využití** – Toto téma zahrnuje rozšířených funkcí, třeba o přijetí změn – aktualizace, vyhledávání, načítání obrázků na pozadí, pomocí jazyka LINQ k sestavení element hierarchie a vytváření vlastních elementů, buňky, a řadiče pro použití s pro MT. D. 

## <a name="setting-up-mtd"></a>Nastavení pro MT. D

PRO MT. D je distribuován spolu s Xamarin.iOS. Ho Pokud chcete použít, klikněte pravým tlačítkem na **odkazy** uzel Xamarin.iOS projektu v sadě Visual Studio 2017 nebo Visual Studio pro Mac a přidejte odkaz na **MonoTouch.Dialog 1** sestavení. Pak přidejte `using MonoTouch.Dialog` příkazy ve zdrojovém kódu podle potřeby.

## <a name="understanding-the-pieces-of-mtd"></a>Principy, které byly pro MT. D

I když se používá reflexi pro rozhraní API, MT. D vytvoří hierarchii Element pod pokličkou, stejně jako by byl vytvořen pomocí rozhraní Elements API přímo. Také podpora JSON uvedené v předchozí části vytvoří také elementy. Z tohoto důvodu je důležité mít základní znalosti o základních částí pro MT. D.

PRO MT. D sestavení obrazovky pomocí následující čtyři části:

-  **DialogViewController**
-  **Vlastnosti RootElement**
-  **Oddíl**
-  **– Element**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, nebo *DVC* zkráceně, dědí z `UITableViewController` a proto představuje obrazovku s tabulkou. DVCs mohou být vloženy do kontroler navigace stejně jako regulární UITableViewController.

### <a name="rootelement"></a>Vlastnosti RootElement

A *vlastnosti RootElement* je kontejner pro položky, které DVC nejvyšší úrovně. Obsahuje oddíly, které může obsahovat elementy. RootElements nevykreslují; Místo toho jsou jednoduše kontejnery pro co ve skutečnosti získá vykreslen. Vlastnosti RootElement přiřazen DVC a pak DVC vykreslí své podřízené objekty.

### <a name="section"></a>Oddíl

Oddíl je skupina buněk v tabulce. Jako s normální tabulky oddílu, můžete volitelně mít záhlaví a zápatí, které můžou být text nebo dokonce vlastní zobrazení jako na následujícím snímku obrazovky:

 [![](images/image2.png "Jako s normální tabulky oddílu, můžete volitelně obsahovat záhlaví a zápatí, které můžou být text nebo dokonce i ve vlastních zobrazeních, stejně jako v tomto snímku obrazovky")](images/image2.png#lightbox)

### <a name="element"></a>Prvek

Element představuje skutečný buňku v tabulce. PRO MT. D obsahuje balené s celou řadu prvků, které představují různé datové typy nebo různé vstupy. Například na následujících snímcích obrazovky ukazuje několik dostupných prvků:

 [![](images/image3.png "Například tato snímky obrazovky ukazují některé z dostupných prvků")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>Další části a RootElements

Nyní Pojďme RootElements a oddíly podrobněji.

### <a name="rootelements"></a>RootElements

Alespoň jeden vlastnosti RootElement je potřebná ke spuštění procesu MonoTouch.Dialog.

Pokud je vlastnosti RootElement inicializovat pomocí hodnoty elementu/section tato hodnota se používá k vyhledání podřízený Element, který bude poskytovat přehled o konfiguraci, který je vykreslen na pravé straně obrazovky. Například následující snímek obrazovky ukazuje tabulky na levé straně s buňku obsahující název obrazovky podrobností na pravé straně, "Dessert", včetně hodnoty vybrané desert.

 [![](images/image4.png "Tento snímek obrazovky znázorňuje tabulky na levé straně s buňku obsahující název obrazovky podrobností na pravé straně, Dessert, včetně hodnoty vybrané desert") ](images/image4.png#lightbox) [ ![ ] (images/image5.png "to Následující snímek obrazovky ilustruje tabulky na levé straně s buňku obsahující název obrazovky podrobností na pravé straně, Dessert, včetně hodnoty vybrané poušť")](images/image5.png#lightbox)

Kořenových elementů můžete také použít uvnitř oddíly k aktivaci načtení nové stránky vnořené konfigurace, jak je uvedeno výše. Při použití v tomto režimu titulek, zadat se používá při vykreslit v rámci oddílu a slouží také jako název pro podstránku. Příklad:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

V příkladu výše když uživatel klepne na "Dessert", MonoTouch.Dialog se vytvoří novou stránku a přejít k němu se ukládají "Dessert" a skupinu přepínačů pomocí tří hodnot s kořenem.

V této ukázce konkrétní skupině přepínačů vybere "Čokoládový dort" v části "Dessert" vzhledem k tomu, RadioGroup jsme předat hodnotu "2". To znamená, že vyberte 3. položky v seznamu (nula index).

Volání metody Add nebo pomocí syntaxe inicializátoru C# 4 přidá oddíly.
Vložit metody jsou k dispozici pro vložení oddíly se animace.

Pokud vytvoříte pomocí skupiny instance (namísto RadioGroup) vlastnosti RootElement souhrnnou hodnotu vlastnosti RootElement při zobrazení v části bude kumulativní počet všech BooleanElements a CheckboxElements, které mají stejný klíč jako hodnotu Group.Key.

### <a name="sections"></a>Oddíly

Oddíly slouží k seskupení prvků na obrazovce a jsou platné pouze přímé podřízené objekty vlastnosti RootElement. Oddíly mohou obsahovat žádný z standardních prvků, včetně nového RootElements.

RootElements vložené v části se používají pro navigaci na zcela novou úroveň hlouběji.

Oddíly může mít buď jako řetězce, nebo jako UIViews záhlaví a zápatí.
Obvykle použijete pouze řetězce, ale chcete-li vytvořit vlastní uživatelská rozhraní můžete použít libovolný UIView jako záhlaví nebo zápatí. Chcete-li vytvořit tímto způsobem můžete použít buď řetězec:

```csharp
var section = new Section ("Header", "Footer")
```

Použití zobrazení, stačí pouze předejte zobrazení konstruktoru:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Získávání oznámení

#### <a name="handling-nsaction"></a>Zpracování NSAction

PRO MT. Zařízení Surface D `NSAction` jako delegáta pro zpracování zpětných volání.
Řekněme například, že chcete zpracovat událost dotykového ovládání pro buňky tabulky vytvořené pro MT. D. Při vytváření elementu se pro MT. D, jednoduše zadejte zpětné volání funkce, jak je znázorněno níže:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Načtení hodnoty prvku

V kombinaci s `Element.Value` vlastnost zpětného volání můžete načíst hodnotu nastavenou v dalších prvků. Zvažte například následující kód:

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

Tento kód vytvoří uživatelské rozhraní, jak je znázorněno níže. Kompletní postup v tomto příkladu najdete v článku [prvky rozhraní API návod](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) kurzu.

 [![](images/image6.png "V kombinaci s vlastností Element.Value, zpětné volání můžete načíst hodnotu nastavenou v dalších prvků")](images/image6.png#lightbox)

Když uživatel stiskne dolní buňky tabulky, kód ve funkci anonymní spustí, zápisu hodnoty z `element` instance na **výstup aplikace** panel v sadě Visual Studio pro Mac.

## <a name="built-in-elements"></a>Integrované elementy

PRO MT. D zahrnuje celou řadu položek buňky předdefinované tabulce označuje jako prvky.
Tyto prvky se používají k zobrazení širokou škálu různých typů v například řetězce, float, data a dokonce i obrázky, pojmenujte několika buňkami v tabulce. Každý prvek postará zobrazení datovým typem odpovídajícím způsobem. Například prvek logické hodnoty zobrazí přepínače přepnete jeho hodnotu. Prvek typu float, se zobrazí posuvník, chcete-li změnit hodnoty typu float.

Existují i složitější prvky pro podporu bohatší datové typy, jako jsou obrázky a html. Element html, který se otevře UIWebView načíst webovou stránku při výběru, například zobrazí titulku v buňce tabulky.

### <a name="working-with-element-values"></a>Práce s hodnotami – Element

Prvky, které se používají k zachycení uživatelského vstupu vystavit veřejnou `Value` vlastnost, která obsahuje aktuální hodnotu prvku v každém okamžiku. Se automaticky aktualizuje, protože uživatel používá aplikaci.

Toto chování se použije pro všechny prvky, které jsou součástí MonoTouch.Dialog, ale není nutné pro uživatelem vytvořené elementy.

### <a name="string-element"></a>Element řetězce

A `StringElement` zobrazí popisek na levé straně buňky tabulky a řetězcovou hodnotu na pravé straně buňky.

 [![](images/image7.png "StringElement zobrazí popisek na levé straně buňky tabulky a řetězcovou hodnotu na pravé straně buňky")](images/image7.png#lightbox)

Použití `StringElement` jako tlačítko, poskytují delegáta.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "Pokud chcete používat StringElement jako tlačítko, zadejte delegáta")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Element upravený řetězce

A `StyledStringElement` umožňuje řetězců, které mají zobrazovat pomocí buď předdefinované styly buňky nebo vlastního formátování.

 [![](images/image9.png "StyledStringElement umožňuje řetězců, které mají zobrazovat pomocí buď předdefinované styly buňky nebo vlastní formátování")](images/image9.png#lightbox)

`StyledStringElement` Třída odvozena z `StringElement`, ale umožňuje vývojářům přizpůsobit několika vlastnosti, jako je písmo, barva textu, barva pozadí buňky, režimu ukončování řádků, kolik řádků chcete zobrazit, a určuje, zda má být zobrazena určité příslušenství.

### <a name="multiline-element"></a>Multiline – Element

 [![](images/image10.png "Multiline – Element")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry Element

`EntryElement`, Jako název znamená, slouží k získání vstupu uživatele. Podporuje běžné řetězce nebo hesla, ve kterém jsou skryté znaků.

 [![](images/image11.png "EntryElement slouží k získání vstupu uživatele")](images/image11.png#lightbox)

Je inicializován pomocí tří hodnot:

-  Popisek pro položku, která se zobrazí uživateli.
-  Zástupný text (jedná se šedě text, který poskytuje nápovědu pro uživatele). 
-  Hodnota textu.


Zástupný text a hodnoty může mít hodnotu null. Popisek je však vyžaduje.

V kterékoli fázi můžete přístup k jeho hodnota vlastnosti načtení hodnoty `EntryElement`.

Kromě toho `KeyboardType` vlastnost lze nastavit v okamžiku vytvoření požadovaného pro zadávání dat stylu typ klávesnice. To je možné nakonfigurovat pomocí hodnot z klávesnice `UIKeyboardType` jak je uvedeno níže:

-  Číselné
-  Telefon
-  Adresa URL
-  E-mailu


### <a name="boolean-element"></a>Boolean – Element

 [![](images/image12.png "Boolean – Element")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Zaškrtávací políčko elementu

 [![](images/image13.png "Zaškrtávací políčko elementu")](images/image13.png#lightbox)

### <a name="radio-element"></a>Element přepínače

A `RadioElement` vyžaduje `RadioGroup` zadat v `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "Vyžaduje RadioGroup zadat v vlastnosti RootElement RadioElement")](images/image14.png#lightbox)

 `RootElements` slouží také ke koordinaci prvků přepínačů. `RadioElement` Členové můžou pokrývat více oddílů (třeba k implementaci podobný výběr tón aktualizačního kanálu a samostatné vlastní vyzvánění z vyzváněcím tónům systému). Souhrnné zobrazení se zobrazí prvku přepínač, který aktuálně není vybrán. Pokud chcete použít, vytvořte `RootElement` pomocí konstruktoru skupiny takto:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Název skupiny v `RadioGroup` se používá k zobrazení vybrané hodnoty obsahující stránce (pokud existuje) a hodnotu, která v tomto případě je nula, je index první vybrané položky.

### <a name="badge-element"></a>Oznámení "BADGE" – Element

 [![](images/image15.png "Oznámení \"BADGE\" – Element")](images/image15.png#lightbox)

### <a name="float-element"></a>Float – Element

 [![](images/image16.png "Float – Element")](images/image16.png#lightbox)

### <a name="activity-element"></a>Element aktivity

 [![](images/image17.png "Element aktivity")](images/image17.png#lightbox)

### <a name="date-element"></a>Data elementu

 ![](images/image18.png "Data elementu")

Při výběru buňky odpovídající DateElement ovládacího prvku pro výběr data se zobrazí, jak je znázorněno níže:

 [![](images/image19.png "Pokud je vybrána odpovídající DateElement buňky, výběr data se zobrazí, jak je znázorněno")](images/image19.png#lightbox)

### <a name="time-element"></a>Time Element

 [![](images/image20.png "Time Element")](images/image20.png#lightbox)

Pokud je vybrána odpovídající TimeElement buňky, ovládacího prvku pro výběr času se zobrazí, jak je znázorněno níže:

 [![](images/image21.png "Pokud je vybrána odpovídající TimeElement buňky, jak je znázorněno se zobrazí ovládacího prvku pro výběr času")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime – Element

 [![](images/image22.png "DateTime – Element")](images/image22.png#lightbox)

Pokud je vybrána odpovídající DateTimeElement buňky, ovládacího prvku pro výběr data a času se zobrazí, jak je znázorněno níže:

 [![](images/image23.png "Pokud je vybrána odpovídající DateTimeElement buňky, ovládacího prvku pro výběr data a času se zobrazí, jak je znázorněno")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML Element

 [![](images/image24.png "HTML Element")](images/image24.png#lightbox)

`HTMLElement` Zobrazuje hodnotu jeho `Caption` vlastnost v buňce tabulky. Vybraná, kde `Url` přiřadit k elementu je načtena do `UIWebView` řídit, jak je znázorněno níže:

 [![](images/image25.png "Kde vybraná, adresu Url přiřadit k elementu je načtena v ovládacím prvku UIWebView, jak je znázorněno níže")](images/image25.png#lightbox)

### <a name="message-element"></a>Element zprávy

 [![](images/image26.png "Element zprávy")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Načíst další prvek

Tento element slouží k povolení uživatelům načíst další položky v seznamu. Můžete přizpůsobit normální a načítání popisků, jakož i barvu písma a text.
`UIActivity` Indikátor spuštění animace a načítání popisek se zobrazí, když uživatel klepne na buňku a potom `NSAction` předaná do konstruktoru provádí. Jednou kódu v `NSAction` dokončení `UIActivity` indikátor zastaví, animace a normální popisek se zobrazí znovu.

### <a name="uiview-element"></a>UIView Element

Kromě toho všechny vlastní `UIView` lze zobrazit pomocí `UIViewElement`.

### <a name="owner-drawn-element"></a>Element vykreslovaných vlastníkem

Protože je abstraktní třída rozčlenit do podtříd tohoto elementu. By měl přepsat `Height(RectangleF bounds)` metody, ve kterém by měl vrátit výšku prvku, stejně jako `Draw(RectangleF bounds, CGContext context, UIView view)` ve kterém byste měli udělat všechny vlastní kreslení v rámci dané hranice pomocí parametrů kontextu a zobrazení. Tento element nemá rutinní vytváření podtříd `UIView`a jejich umístěním do buňky, která má být vrácena, poskytne vám pouze by bylo potřeba implementovat dva jednoduché přepisy. Můžete zobrazit lepší ukázková implementace v ukázkové aplikaci v `DemoOwnerDrawnElement.cs` souboru.

Tady je velmi jednoduchý příklad implementace třídy:

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
 }
```

### <a name="json-element"></a>JSON Element

`JsonElement` Je podtřídou třídy `RootElement` , který rozšiřuje `RootElement` být schopni načíst obsah vnořené podřízené z místní nebo vzdálené adresy url.

`JsonElement` Je `RootElement` , který může být vytvořena ve dvou formách. Vytvoří jednu verzi `RootElement` , který načte obsah na vyžádání. Ty se vytvářejí pomocí `JsonElement` konstruktorů, které přijímají nadbytečný argument na konci adresy url načíst obsah ze:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Další způsob vytvoří data z místního souboru nebo z existujícího `System.Json.JsonObject` , kterou jste již analyzovat:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Další informace o použití JSON se pro MT. D, najdete v článku [návod elementu JSON](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) kurzu.

## <a name="other-features"></a>Další funkce

### <a name="pull-to-refresh-support"></a>Podpora o přijetí změn – aktualizace

 *O přijetí změn-na-* *aktualizovat* vizuální efekt původně najdete v *Tweetie2* aplikace, kde byl program oblíbených efekt mezi několika aplikacemi.

Přidává automatické o přijetí změn – aktualizace na vaše dialogová okna, stačí provést dva kroky: připojení obslužnou rutinu události, která vás upozorní, když si uživatel vyžádá data a upozorňovat `DialogViewController` při načtení dat se vrátíte do výchozího stavu.

Zapojování oznámení je jednoduché. Stačí se připojit k `RefreshRequested` událostí na `DialogViewController`, tímto způsobem:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Pak na vaše metoda `OnUserRequestedRefresh`, by některé načítání dat do fronty, některá data žádosti z net nebo aktivovat vlákna pro data. Po načtení dat musíte upozornit `DialogViewController` jsou nová data v, který Pokud chcete obnovit do výchozího stavu zobrazení, to provedete voláním `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Hledat podporu

Hledání, nastavit `EnableSearch` vlastnost na vaše `DialogViewController`. Můžete také nastavit `SearchPlaceholder` vlastnost použít jako vodoznakového textu v panelu vyhledávání.

Hledání se změní obsah zobrazení zadaného uživatelem. Vyhledá pole viditelné a ukazuje, aby uživatel. `DialogViewController` Poskytuje tři metody prostřednictvím kódu programu zahájení, ukončení nebo aktivovat novou operaci filtr na výsledky. Tyto metody jsou uvedeny níže:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


Systém je rozšiřitelný, takže je možné toto chování změnit, pokud chcete.

### <a name="background-image-loading"></a>Načítání obrázků na pozadí

Zahrnuje MonoTouch.Dialog [TweetStation](https://github.com/migueldeicaza/TweetStation) načítání obrázků aplikace. Tento obrázek zavaděč slouží k načtení obrázků na pozadí, podporuje ukládání do mezipaměti a váš kód může upozornit, když se obrázek načetl.

Také omezí počet odchozí síťová připojení.

Zavaděč obrázků je implementována v `ImageLoader` třídy, všechno, co potřebujete udělat, je volání `DefaultRequestImage` metodu, budete muset poskytnout identifikátor Uri pro image, kterou chcete nahrát, stejně jako instance `IImageUpdated` rozhraní, která bude vyvolána v případě bitovou kopii ha s bylo načteno.

Například následující kód načte bitovou kopii z adresy Url do `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader třída zveřejňuje vyprázdnění metodu, která můžete volat, pokud chcete uvolnit všechny Image, které jsou aktuálně uložených v mezipaměti v paměti. Aktuální kód má mezipaměť pro 50 bitové kopie. Pokud chcete použít jiný velikost (například pokud očekáváte imagí příliš velká, že 50 Image by byly příliš mnoho), stačí vytvořit instance ImageLoader a předejte počet imagí, které chcete zachovat v mezipaměti.

## <a name="using-linq-to-create-element-hierarchy"></a>Vytvoření hierarchie elementů pomocí LINQ

Prostřednictvím inteligentní využití syntaxe inicializace LINQ a C# je možné vytvořit hierarchii element LINQ. Například následující kód vytvoří obrazovky z některých polí řetězce a popisovače buňky výběr přes anonymní funkci, která je předána do každé `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

To může snadno kombinovat s XML úložiště dat nebo dat z databáze k vytvoření komplexní aplikace z dat téměř zcela.

## <a name="extending-mtd"></a>Rozšíření pro MT. D

### <a name="creating-custom-elements"></a>Vytváří se vlastní elementy

Můžete vytvořit vlastní element dědí buď z existujícího prvku nebo odvozený od třídy kořenový Element.

Vytvoření vlastního elementu, kterou chcete přepsat následující metody:

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

Pokud vaše element může obsahovat proměnné velikosti, budete muset implementovat `IElementSizing` rozhraní, která obsahuje jednu metodu:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Plánujete na implementaci vašeho `GetCell` metoda voláním `base.GetCell(tv)` a přizpůsobení vrácený buňky, je třeba také přepsat `CellKey` vlastnost vrátit klíč, který bude jedinečný pro Element, tímto způsobem:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Tento postup funguje pro většinu prvků, ale ne pro `StringElement` a `StyledStringElement` jako ty, použijte vlastní sadu klíčů pro různé scénáře vykreslování. Je třeba replikaci kód v těchto tříd.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (systém DVCs)

Odraz i rozhraní Elements API používají stejné `DialogViewController`. Někdy budete chtít přizpůsobit vzhled zobrazení nebo můžete chtít použít některé funkce `UITableViewController` , které přesahují základní vytváření uživatelských rozhraní.

`DialogViewController` Je pouze podtřídou třídy `UITableViewController` a přizpůsobit ho stejným způsobem, který by upravíte `UITableViewController`.

Například Kdybyste chtěli změnit styl seznamu, který má být buď `Grouped` nebo `Plain`, tuto hodnotu můžete nastavit tak, že změníte vlastnost při vytváření kontroler, například takto:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Pro pokročilejší úpravy `DialogViewController`, jako je nastavení jeho pozadí, byste podtřídy ho a správné přepsání metody, jak je znázorněno v následujícím příkladu:

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

Další přizpůsobení bod je následující virtuální metody v `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Tato metoda by měla vrátit podtřída `DialogViewController.Source` pro případy, kde jsou vaše buňky měly rovnoměrnou velikost, nebo jejich podtřída `DialogViewController.SizingSource` Pokud nestejné vaše buňky.

Toto přepsání můžete použít k zachycení všech `UITableViewSource` metody. Například [TweetStation](https://github.com/migueldeicaza/TweetStation) používá toto příslušně aktualizovat počet nepřečtených tweety a sledovat, kdy se uživatel posunul do horní části.

## <a name="validation"></a>Ověřování

Prvky neposkytují ověření samotných jako modely, které jsou vhodné pro webové stránky a aplikací klasické pracovní plochy nemapovaly přímo do modelu interakce iPhone.

Pokud chcete provést ověření dat, by měl to provést, když uživatel spustí akce s daty zadali. Například <span class="ui">provádí</span> nebo <span class="ui">Další</span> tlačítko na horním panelu nástrojů nebo některé `StringElement` slouží jako tlačítko pro přechod do další fáze.

Je to můžete provést základní ověření vstupu, kde možná více složité ověřování, jako je kontrola platnosti kombinaci uživatele a hesla se serverem.

Jak oznámit uživateli chybu je specifické pro aplikaci. Může automaticky otevře `UIAlertView` nebo zobrazit nápovědu.

## <a name="summary"></a>Souhrn

Tento článek popisuje velké množství informací o MonoTouch.Dialog. Popsané základní informace o způsobu, jakým pro MT. D funguje a které pokrývá různých komponent, které tvoří pro MT. D. To jsme si také ukázali širokou škálu prvky a přizpůsobení tabulky podporované pro MT. D a jak popsaných pro MT. D je možné rozšířit pomocí vlastní elementy. Kromě vysvětlení podporu JSON pro MT. D, která umožňuje dynamicky vytváření prvků z formátu JSON.


## <a name="related-links"></a>Související odkazy

- [Záznam dění na monitoru - Miguela de Icaza vytvoří přihlašovací obrazovka aplikace iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru – snadno vytvářet iOS uživatelské rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Návod: Vytvoření aplikace pomocí rozhraní Elements API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Návod: Vytvoření aplikace pomocí rozhraní Reflection API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Návod: Vytvoření uživatelského rozhraní pomocí elementu JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Značka MonoTouch.Dialog JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Referenční třída UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Referenční třída UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
