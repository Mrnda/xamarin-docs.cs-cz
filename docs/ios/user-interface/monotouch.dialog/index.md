---
title: "Úvod do MonoTouch.Dialog"
description: "MonoTouch.Dialog (strojový překladů. D) Sada nástrojů je představuje nepostradatelné rozhraní pro aplikaci rychlý vývoj uživatelského rozhraní v Xamarin.iOS. STROJOVÝ PŘEKLADŮ. D umožňuje rychle a snadno definovat komplexní aplikace uživatelského rozhraní pomocí deklarativní způsob místo nebylo nutné pracně navigační řadiče, tabulek atd. Kromě toho strojový překladů. D má flexibilní sadu rozhraní API, která poskytne vývojářům úplnou kontrolu nebo rukou vypnout přístup a také další funkce, například obrázek pozadí-aktualizace obsahu, načtení, vyhledání podporu a dynamické generování uživatelského rozhraní pomocí JSON data. Tento průvodce uvádí různé způsoby, jak pracovat s strojový překladů. D a pak dives o pokročilé využití."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b279f3e643e008e88b8ad086c400d992427c6df4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-monotouchdialog"></a>Úvod do MonoTouch.Dialog

_MonoTouch.Dialog (strojový překladů. D) Sada nástrojů je představuje nepostradatelné rozhraní pro aplikaci rychlý vývoj uživatelského rozhraní v Xamarin.iOS. STROJOVÝ PŘEKLADŮ. D umožňuje rychle a snadno definovat komplexní aplikace uživatelského rozhraní pomocí deklarativní způsob místo nebylo nutné pracně navigační řadiče, tabulek atd. Kromě toho strojový překladů. D má flexibilní sadu rozhraní API, která poskytne vývojářům úplnou kontrolu nebo rukou vypnout přístup a také další funkce, například obrázek pozadí-aktualizace obsahu, načtení, vyhledání podporu a dynamické generování uživatelského rozhraní pomocí JSON data. Tento průvodce uvádí různé způsoby, jak pracovat s strojový překladů. D a pak dives o pokročilé využití._


MonoTouch.Dialog, označuje jako strojový překladů. D pro zkrácení, je rychlé toolkit vývoj uživatelského rozhraní, která umožňuje vývojářům vytvářet aplikace obrazovky a navigační pomocí informace místo nebylo nutné pracně vytváření řadiče zobrazení, tabulek atd. Jako takový nabízí významné zjednodušení snížení vývoj a kód uživatelského rozhraní. Zvažte například následující snímek obrazovky:

 [ ![](images/image1.png "Představte si třeba tento snímek obrazovky")](images/image1.png)

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

Při práci s tabulkami v iOS, je často tuny automatizujete kódu.
Například pokaždé, když je potřeba tabulku, zdroj dat je potřeba k vyplnění této tabulky. Každý obrazovky v aplikaci, která má dva obrazovky založené na tabulky, které jsou připojené přes řadič navigační, sdílí spoustu stejný kód.

STROJOVÝ PŘEKLADŮ. D, zjednodušuje tím, kdy zapouzdřuje všechny tento kód do obecné rozhraní API pro vytvoření tabulky. Pak poskytuje abstrakci nad toto rozhraní API umožňující pro objekt deklarativní syntaxi, která zjednodušuje i vazby. Jako takový existují dvě rozhraní API dostupná v strojový překladů. D:

-   **Nízké úrovně rozhraní API elementy** – *API elementy* je založena na vytváření hierarchická stromu prvky, které představují obrazovky a jejich součástí. Prvky rozhraní API poskytuje vývojářům nejvíce flexibilitu a řízení při vytváření uživatelská rozhraní. Rozhraní API elementy navíc obsahuje rozšířené podpory pro deklarativní definice prostřednictvím formátu JSON, která umožňuje velmi rychlé deklarace, jak dynamické generování uživatelského rozhraní ze serveru. 
-   **Nejdůležitější rozhraní API reflexe** – taky známé jako *vazby**rozhraní API* , ve které třídy jsou opatřeny poznámkami pomocí uživatelského rozhraní pomocné parametry a potom strojový překladů. D automaticky vytvoří obrazovky založené na objektech a poskytuje vazbu mezi co se zobrazí (a volitelně upravovat) na obrazovce a základní objekt zálohování.   Výše uvedený příklad ukazuje použití rozhraní API reflexe. Toto rozhraní API neposkytuje podrobné ovládací prvek, který nemá elementy rozhraní API, ale snižuje složitost i další podle budovy automaticky se element hierarchii na základě třídy atributů. 


STROJOVÝ PŘEKLADŮ. D dodává sbalené s velké sady součástí prvky uživatelského rozhraní pro vytvoření obrazovky, ale také rozpozná potřeba vlastní elementy a pokročilé obrazovky rozložení. Rozšiřitelnost jako takový je, že že první třídy vybrané pečené do rozhraní API. Vývojáři můžou rozšířit stávající elementy nebo vytvořit nové a pak bezproblémově integrovat.

Kromě toho strojový překladů. D má celou řadu funkcí UX běžné iOS, které jsou součástí, jako je "vyžádání aktualizace" podporovat asynchronní image načítání a vyhledejte podpory.

V tomto článku bude trvat komplexní pohled na práci s strojový překladů. D, včetně:

-   **STROJOVÝ PŘEKLADŮ. Součásti D** – to bude soustředit na pochopení třídy, které tvoří strojový překladů. Chcete-li povolit, získávání rychle se zorientujte D. 
-   **Referenční dokumentace elementů** – kompletní seznam předdefinovaných elementů strojový překladů. D. 
-   **Rozšířené použití** – to zahrnuje pokročilé funkce jako aktualizace obsahu, hledání, načítání obrázku pozadí, použití LINQ k sestavení se element hierarchií a vytvoření vlastní elementy buněk, a řadiče pro použití s strojový překladů. D. 

## <a name="understanding-the-pieces-of-mtd"></a>Principy kusy strojový překladů. D

I když pomocí reflexe rozhraní API, strojový překladů. D vytvoří Element hierarchie pod pokličkou, stejně, jako kdyby se byly vytvořené prostřednictvím rozhraní API elementy přímo. Také podporu JSON uvedených v předchozí části vytvoří také elementy. Z tohoto důvodu je důležité mít základní znalosti o základní údaje strojový překladů. D.

STROJOVÝ PŘEKLADŮ. D sestavení obrazovky pomocí následující čtyři části:

-  **DialogViewController**
-  **RootElement**
-  **Část**
-  **Element**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, nebo *DVC* pro zkrácení, dědí z `UITableViewController` a proto představuje obrazovky s tabulkou. DVCs možné převést na navigační řadič stejně jako regulární UITableViewController.

### <a name="rootelement"></a>RootElement

A *RootElement* je kontejner pro položky, které přejděte do DVC nejvyšší úrovně. Obsahuje oddíly, které pak může obsahovat elementy. RootElements se nevykreslí; Místo toho jsou jednoduše kontejnery pro co ve skutečnosti získá vykreslen. RootElement je přiřazena k DVC a pak DVC vykreslí podřízené.

### <a name="section"></a>Část

Oddíl je skupina buněk v tabulce. Jako s normální tabulky oddílu, můžete volitelně může mít záhlaví a zápatí stránky, která buď může být text nebo dokonce vlastní zobrazení, stejně jako na následujícím snímku obrazovky:

 [ ![](images/image2.png "Jako s normální tabulky oddílu, můžete volitelně může mít záhlaví a zápatí stránky, která buď může být text nebo dokonce vlastní zobrazení, stejně jako tento snímek obrazovky")](images/image2.png)

### <a name="element"></a>Prvek

Element představuje skutečný buňku v tabulce. STROJOVÝ PŘEKLADŮ. D dodává sbalené s širokou škálu prvky, které představují různými datovými typy nebo jiné vstupy. Například následující snímky obrazovky ilustruje několik k dispozici následující prvky:

 [ ![](images/image3.png "Například tato snímky obrazovky ilustruje několik dostupných prvků")](images/image3.png)

## <a name="more-on-sections-and-rootelements"></a>Další na oddíly a RootElements

Nyní probereme RootElements a oddíly podrobněji.

### <a name="rootelements"></a>RootElements

Alespoň jeden RootElement je nezbytné ke spuštění procesu MonoTouch.Dialog.

Pokud je RootElement inicializovaný s hodnotou prvek oddílu nebo tato hodnota slouží k vyhledání podřízený Element, který obsahuje všechny konfigurace, které je vykresleno na pravé straně zobrazení. Například následující snímek obrazovky ukazuje tabulky na levé straně s buňku obsahující název obrazovky podrobností na pravé straně, "Cukrářské", společně s hodnotu vybrané poušť.

 [ ![](images/image4.png "Tento snímek obrazovky ukazuje tabulky na levé straně s buňku obsahující název obrazovky podrobností na pravé straně, cukrářské, společně s hodnotu vybrané poušť") ](images/image4.png) [ ![ ] (images/image5.png "to Následující snímek obrazovky ukazuje tabulky na levé straně s buňku obsahující název obrazovky podrobností na pravé straně, cukrářské, společně s hodnotu vybrané poušť")](images/image5.png)

Kořenové elementy lze také v části pro aktivaci načítání novou stránku vnořené konfigurace, jako v příkladu nahoře. Při použití v tomto režimu titulek zadaný se používá při vykreslit v rámci oddílu a také slouží jako nadpis na podstránku. Příklad:

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

V předchozím příkladu když uživatel klepnutím na "Cukrářské", MonoTouch.Dialog se vytvořit novou stránku a přejděte k němu s kořenovým je "Cukrářské" a nutnosti skupinu přepínač pomocí tří hodnot.

V této ukázce konkrétní skupině přepínacích vybere "Čokoládový dort" v části "Cukrářské", protože jsme předat hodnotu "2" RadioGroup. To znamená, vyberte 3. položka v seznamu (nula – index).

Volání metody přidat nebo pomocí syntaxe inicializátoru 4 C# přidá oddíly.
K dispozici jsou metody Insert vkládat oddíly s animace.

Pokud vytvoříte RootElement s instance skupiny (namísto RadioGroup) bude souhrnnou hodnotu RootElement, kdy se zobrazí v části Kumulativní počet všech BooleanElements a CheckboxElements, které mají stejný klíč jako Group.Key hodnotu.

### <a name="sections"></a>Oddíly

Oddíly slouží k seskupení prvků na obrazovce a jsou jedinými platnými přímé podřízené objekty RootElement. Části může obsahovat jakýkoli standardní elementy, včetně nové RootElements.

RootElements vložených v části se používají k přejděte do nové hlubší úrovni.

Části může mít jako řetězce nebo jako UIViews záhlaví a zápatí stránky.
Obvykle použijete pouze řetězce, ale chcete-li vytvořit vlastní uživatelská rozhraní můžete použít všechny UIView jako záhlaví nebo zápatí. Řetězec můžete použít buď při jejich vytváření takto:

```csharp
var section = new Section ("Header", "Footer")
```

Chcete-li použít zobrazení, stačí předejte zobrazení konstruktoru:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Získávání upozornění

#### <a name="handling-nsaction"></a>Zpracování NSAction

STROJOVÝ PŘEKLADŮ. D povrchy `NSAction` jako delegáta pro zpracování zpětných volání.
Řekněme například, že chcete zpracovat událost touch buňky tabulky vytvořené strojový překladů. D. Při vytváření element s strojový překladů. D, jednoduše zadejte a zpětného volání funkce, jak je uvedeno níže:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Načítání hodnota elementu

V kombinaci s `Element.Value` vlastnost zpětné volání můžete načíst hodnotu nastavenou v dalších prvků. Zvažte například následující kód:

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

Tento kód vytvoří uživatelského rozhraní, jak je uvedeno níže. Kompletní a podrobný postup tohoto příkladu, najdete v článku [prvky rozhraní API návod](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) kurzu.

 [ ![](images/image6.png "V kombinaci s vlastností Element.Value, zpětné volání můžete načíst hodnotu nastavenou v další prvky")](images/image6.png)

Když uživatel stiskne dolní buňky tabulky, kód v anonymní funkce provede, zápis hodnotu z `element` instance k **výstupu aplikace** odsazení v sadě Visual Studio for Mac.

## <a name="built-in-elements"></a>Předdefinované elementy

STROJOVÝ PŘEKLADŮ. D obsahuje mnoho položek buňky předdefinované tabulce označuje jako elementy.
Tyto prvky se používají k zobrazení v buňkách tabulky například řetězce, obtékaných objektů, kalendářních dat a i obrázky, název několika celou řadu různých typů. Každý prvek postará zobrazení datovým typem odpovídajícím způsobem. Logická hodnota elementu například zobrazí přepínač k přepnutí jeho hodnotu. Float element, se zobrazí jezdce ke změně hodnoty float.

Existuje i složitější elementů pro podporu bohatší datové typy, jako jsou bitové kopie a html. Element html, který se otevře UIWebView načíst webovou stránku, pokud vybraná, například zobrazí popisek v buňce tabulky.

### <a name="working-with-element-values"></a>Práce s hodnotami – Element

Prvky, které se používají k zachycení uživatelského vstupu vystavit veřejné `Value` vlastnost, která obsahuje aktuální hodnotu elementu kdykoli. Se automaticky aktualizuje, protože uživatel používá aplikaci.

Jedná se o chování pro všechny elementy, které jsou součástí MonoTouch.Dialog, ale není nutné u elementů vytvořené uživatelem.

### <a name="string-element"></a>Řetězec – Element

A `StringElement` zobrazí popisek na levé straně hodnotu řetězce na pravé straně buňky a buňku tabulky.

 [ ![](images/image7.png "StringElement zobrazí popisek na levé straně hodnotu řetězce na pravé straně buňky a buňku tabulky")](images/image7.png)

Použít `StringElement` jako tlačítko, zadejte delegáta.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [ ![](images/image8.png "Pokud chcete použít jako tlačítka StringElement, poskytovat delegáta")](images/image8.png)

### <a name="styled-string-element"></a>Element stylem řetězce

A `StyledStringElement` umožňuje řetězců, které mají zobrazovat pomocí buď styly buňky předdefinované tabulky nebo s vlastní formátování.

 [ ![](images/image9.png "StyledStringElement umožňuje řetězců, které mají zobrazovat pomocí buď styly buňky předdefinované tabulky nebo s vlastní formátování")](images/image9.png)

`StyledStringElement` Třída odvozená z `StringElement`, ale umožňuje vývojářům přizpůsobit několik vlastností, jako je písmo, barvy, barva pozadí buněk, režimu ukončování řádků, počet řádků, které chcete zobrazit, a zda má být zobrazena určité příslušenství.

### <a name="multiline-element"></a>Multiline – Element

 [ ![](images/image10.png "Multiline – Element")](images/image10.png)

### <a name="entry-element"></a>Vstupní Element

`EntryElement`, Jako název znamená, že se používá k získání vstupu uživatele. Podporuje regulárních řetězců nebo hesla, kde jsou skryté znaky.

 [ ![](images/image11.png "EntryElement slouží k získání vstupu uživatele")](images/image11.png)

Není inicializována pomocí tří hodnot:

-  Popisek pro položku, která se zobrazí uživateli.
-  Zástupný text (Toto je aktivní out text, který poskytuje nápovědu pro uživatele). 
-  Hodnota textu.


Zástupný text a hodnoty může mít hodnotu null. Popisek je však vyžaduje.

V libovolném bodě přístupu k jeho hodnotu vlastnosti můžete načíst hodnotu `EntryElement`.

Kromě toho `KeyboardType` vlastnost lze nastavit v okamžiku vytvoření na styl typu klávesnice požadovaných pro zadávání dat. Tímto lze nakonfigurovat pomocí hodnot klávesnice `UIKeyboardType` jak je uvedeno dále:

-  číselné
-  Telefon
-  Adresa URL
-  E-mailu


### <a name="boolean-element"></a>Boolean – Element

 [ ![](images/image12.png "Boolean – Element")](images/image12.png)

### <a name="checkbox-element"></a>Zaškrtávací políčko elementu

 [ ![](images/image13.png "Zaškrtávací políčko elementu")](images/image13.png)

### <a name="radio-element"></a>Element přepínače

A `RadioElement` vyžaduje `RadioGroup` zadat v `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [ ![](images/image14.png "RadioElement vyžaduje RadioGroup zadat v RootElement")](images/image14.png)

 `RootElements` také se používají ke koordinaci přepínač elementy. `RadioElement` Členy může mít rozsah více oddílů (například k implementaci něco podobného prstenec styl podání selektor a samostatné vlastní vyzvánění z tónům systému). Souhrnné zobrazení se zobrazí přepínač elementu, který je momentálně zvolen. Pokud chcete použít, vytvořte `RootElement` s konstruktorem skupiny, například takto:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Název skupiny v `RadioGroup` se používá k zobrazení vybrané hodnoty na stránce obsahující (pokud existuje) a hodnotu, kterou v tomto případě je nulová, je index první vybrané položky.

### <a name="badge-element"></a>Element oznámení "BADGE"

 [ ![](images/image15.png "Element oznámení "BADGE"")](images/image15.png)

### <a name="float-element"></a>Float – Element

 [ ![](images/image16.png "Float – Element")](images/image16.png)

### <a name="activity-element"></a>Element aktivity

 [ ![](images/image17.png "Element aktivity")](images/image17.png)

### <a name="date-element"></a>Element datum

 ![](images/image18.png "Element datum")

Pokud je vybraná buňky odpovídající DateElement, výběr data se zobrazí, jak je uvedeno níže:

 [ ![](images/image19.png "Pokud je vybraná buňky odpovídající DateElement, výběr data se zobrazí, jak je znázorněno")](images/image19.png)

### <a name="time-element"></a>Time Element

 [ ![](images/image20.png "Time Element")](images/image20.png)

Pokud je vybraná buňky odpovídající TimeElement, výběr času se zobrazí, jak je uvedeno níže:

 [ ![](images/image21.png "Pokud je vybraná buňky odpovídající TimeElement, výběr času se zobrazí, jak je znázorněno")](images/image21.png)

### <a name="datetime-element"></a>Element data a času

 [ ![](images/image22.png "Element data a času")](images/image22.png)

Pokud je vybraná buňky odpovídající DateTimeElement, se zobrazí pro výběr data a času, jak je uvedeno níže:

 [ ![](images/image23.png "Pokud je vybraná buňky odpovídající DateTimeElement, jak je znázorněno se zobrazí pro výběr data a času")](images/image23.png)

### <a name="html-element"></a>HTML Element

 [ ![](images/image24.png "HTML Element")](images/image24.png)

`HTMLElement` Zobrazuje hodnotu jeho `Caption` vlastnost v buňce tabulky. Vybraná, kde `Url` přiřazen k elementu je načteno v `UIWebView` řídit, jak je uvedeno níže:

 [ ![](images/image25.png "Kde vybraná, adresu Url přiřazen k elementu je načtena v ovládacím prvku UIWebView, jak je uvedeno níže")](images/image25.png)

### <a name="message-element"></a>Element zprávy

 [ ![](images/image26.png "Element zprávy")](images/image26.png)

### <a name="load-more-element"></a>Načíst další – Element

Povolit uživatelům načíst více položek v seznamu, použijte tento element. Můžete přizpůsobit normální a načítání titulků, jakož i barvu písma a text.
`UIActivity` Indikátor spustí animace a titulek načítání se zobrazí, když uživatel klepnutím na buňku a potom `NSAction` předaný do konstruktoru se spustí. Jednou kódu v `NSAction` po dokončení, `UIActivity` indikátor zastaví animace a titulek normální, zobrazí se znovu.

### <a name="uiview-element"></a>UIView Element

Kromě toho vlastní `UIView` lze zobrazit pomocí `UIViewElement`.

### <a name="owner-drawn-element"></a>Element vykreslované uživatelem

Tento element musí být rozčlenění, protože je abstraktní třída. By měly přepsat `Height(RectangleF bounds)` metoda, ve kterém by měla vrátit Výška elementu, a také `Draw(RectangleF bounds, CGContext context, UIView view)` v které měli byste udělat všechny přizpůsobené kreslení v rámci dané hranice pomocí parametrů kontextu a zobrazení. Tento element nemá lifting těžký o vytváření podtříd `UIView`a umístit ho do buňky, který se má vrátit, ponechat pouze bylo třeba implementovat dvě jednoduché přepsání. Můžete zobrazit lepší implementace ukázka v ukázkové aplikace v `DemoOwnerDrawnElement.cs` souboru.

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

### <a name="json-element"></a>JSON – Element

`JsonElement` Je podtřídou třídy `RootElement` který rozšiřuje `RootElement` moct načíst obsah vnořené podřízených z místní nebo Vzdálená adresa url.

`JsonElement` Je `RootElement` , může být vytvořena ve dvou formách. Vytvoří jednu verzi `RootElement` , načte obsah na vyžádání. Tyto jsou vytvořeny pomocí `JsonElement` konstruktory, které nepřijímají nadbytečný argument na konci načíst obsah z adresy url:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Jiná forma vytvoří data z místního souboru nebo existující `System.Json.JsonObject` který už mají analyzovat:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Další informace o používání JSON s strojový překladů. D, najdete v článku [JSON Element návod](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) kurzu.

## <a name="other-features"></a>Další funkce

### <a name="pull-to-refresh-support"></a>Podpora aktualizace obsahu

 *Pro vyžádání obsahu-na-* *aktualizovat* vizuální efekt původně nachází v *Tweetie2* aplikaci, která se aktivovala oblíbených efekt mezi mnoho aplikací.

Přidání podpory automatické aktualizace obsahu do vaší dialogová okna, stačí udělat dvě věci: spojit Obslužné rutiny události chcete být upozorněni, když si uživatel vyžádá data a upozorňovat `DialogViewController` po načtení dat se vrátíte do výchozího stavu.

Zapojování oznámení je jednoduché. připojit pouze k `RefreshRequested` událostí na `DialogViewController`, podobné výjimky:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Pak na metodu `OnUserRequestedRefresh`, by fronty některé načítání dat, některé data žádosti z sítě nebo číselníku vlákno k výpočtu data. Jakmile se má načíst data, musíte upozornit `DialogViewController` nová data v, a pokud chcete obnovit do výchozího stavu zobrazení, to se provádí volání `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Podpora hledání

Chcete-li podporovat hledání, nastavte `EnableSearch` vlastnost na vaše `DialogViewController`. Můžete také nastavit `SearchPlaceholder` vlastnost, která má použít jako vodoznakového textu v panelu vyhledávání.

Hledání se změní obsah zobrazení jako typy uživatelů. To vyhledá viditelná pole a ukazuje, aby se uživatel. `DialogViewController` Poskytuje tři metody prostřednictvím kódu programu zahájení, ukončení nebo aktivovat novou operace filtru na výsledky. Tyto metody jsou uvedeny níže:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


Systém je rozšiřitelný, takže toto nastavení můžete změnit, pokud chcete.

### <a name="background-image-loading"></a>Načítání obrázku pozadí

Zahrnuje MonoTouch.Dialog [TweetStation](https://github.com/migueldeicaza/TweetStation) aplikace pro načítání obrázků. Tento nástroj pro načítání obrázků slouží k načtení obrázky na pozadí, podporuje ukládání do mezipaměti a váš kód může upozornit, když se načetl bitovou kopii.

Také omezí počet odchozích síťových připojení.

Načítání obrázků je implementována ve `ImageLoader` volání je třída, všechny musíte udělat `DefaultRequestImage` metoda, budete muset zadat identifikátor Uri pro bitové kopie, které chcete zatížení, jakož i instanci `IImageUpdated` rozhraní, která bude vyvolána v případě bitovou kopii ha s bylo načteno.

Například následující kód načte z adresy Url do obrázku `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

Třída ImageLoader zpřístupní vyprázdnění metodu, kterou můžete volat, když chcete uvolnit všechny bitové kopie, které jsou aktuálně uložené do mezipaměti v paměti. Aktuální kód má mezipaměti pro 50 bitové kopie. Pokud chcete použít jiný mezipaměti velikost (například pokud očekáváte obrázky, které chcete být příliš velký, že 50 bitové kopie budou příliš mnoho), můžete pouze vytvářet instance ImageLoader a předat počet bitových kopií, které chcete zachovat v mezipaměti.

## <a name="using-linq-to-create-element-hierarchy"></a>Chcete-li vytvořit Element hierarchii pomocí LINQ

Prostřednictvím inteligentní použití syntaxe inicializace LINQ a C# pro LINQ slouží k vytvoření hierarchie elementu. Například následující kód vytvoří obrazovky z některých polí řetězec a zpracovává buňka výběr přes anonymní funkce, která je předána do každé `StringElement`:

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

To může snadno kombinovat s XML úložiště dat nebo dat z databáze k vytvoření složitých aplikací téměř úplně z dat.

## <a name="extending-mtd"></a>Rozšíření strojový překladů. D

### <a name="creating-custom-elements"></a>Vytváření vlastní elementy

Můžete vytvořit vlastní element dědění ze buď existující Element nebo odvozené od třídy kořenový Element.

Pokud chcete vytvořit vlastní Element, můžete přepsat tyto metody:

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

Pokud vaše element může obsahovat proměnné velikosti, potřebujete implementovat `IElementSizing` rozhraní, který obsahuje jedno z těchto metod:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Pokud plánujete implementace vaše `GetCell` metoda voláním `base.GetCell(tv)` a přizpůsobení vrácený buňky, musíte také přepsat `CellKey` vlastnost vrátit klíč, který bude jedinečný pro Element, jako jsou to:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Tento postup funguje pro většinu prvků, ale ne pro `StringElement` a `StyledStringElement` jako ty, použijte vlastní sadu klíčů pro různé scénáře vykreslování. Museli byste se replikovat kód v těchto tříd.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Odraz a rozhraní API elementy používat stejné `DialogViewController`. Někdy budete chtít přizpůsobit vzhled zobrazení nebo můžete chtít použít některé funkce `UITableViewController` která překročila základní vytváření uživatelská rozhraní.

`DialogViewController` Je jenom podtřídou třídy `UITableViewController` a upravit ji stejným způsobem, který by přizpůsobit `UITableViewController`.

Například, pokud jste chtěli změnit styl seznamu být buď `Grouped` nebo `Plain`, tuto hodnotu můžete nastavit změnou vlastnosti při vytvoření řadiče, například takto:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Pro pokročilejší vlastní nastavení `DialogViewController`, jako je třeba nastavení jeho pozadí, byste podtřídami a přepsání správné metod, jak je znázorněno v následujícím příkladu:

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

Dalším kritériem při přizpůsobení je následující virtuální metody v `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Tato metoda by měla vrátit podtřídou třídy `DialogViewController.Source` pro případy, kdy jsou vaše buněk rovnoměrně dimenzované nebo podtřídou třídy `DialogViewController.SizingSource` Pokud nerovnoměrné vaší buněk.

Toto přepsání můžete zaznamenat některé z `UITableViewSource` metody. Například [TweetStation](https://github.com/migueldeicaza/TweetStation) pomocí této funkce sledovat, kdy uživatel má přechod na horní a počet nepřečtená tweetů se aktualizují odpovídajícím způsobem.

## <a name="validation"></a>Ověřování

Elementy neposkytují ověření sami jako modely, které se nehodí pro webové stránky a aplikací klasické pracovní plochy se nemapují přímo do modelu interakce iPhone.

Pokud chcete provést ověření dat, měli byste to provést, když uživatel spustí akci s zadaná data. Například <span class="ui">provádí</span> nebo <span class="ui">Další</span> tlačítko na horním panelu nástrojů nebo některé `StringElement` používá jako tlačítko Přejít do další fáze.

Toto je, kde můžete provést základní ověření vstupu, a případně více komplikovanější, ověřování, jako je kontrola platnosti kombinace uživatele a hesla se serverem.

Jak upozornit uživatele chyba je specifické pro aplikaci. Může překryvné `UIAlertView` nebo zobrazit nápovědu.

## <a name="summary"></a>Souhrn

Tento článek zahrnutých velké množství informací o MonoTouch.Dialog. Je popsané základní informace o způsobu, jakým strojový překladů. D funguje a zahrnutých různé součásti, které tvoří strojový překladů. D. Je také vám ukázal, širokou škálu elementy a přizpůsobení Tabulka nepodporuje strojový překladů. D a jak strojový překladů. D lze rozšířit pomocí vlastní elementy. Kromě toho je vysvětlené podporu JSON v strojový překladů. D, která umožňuje dynamicky vytváření prvků z formátu JSON.


## <a name="related-links"></a>Související odkazy

- [Záznam dění na monitoru - Miguel de Icaza vytvoří obrazovka pro přihlášení iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru - snadno vytvářet iOS uživatelského rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Návod: Vytvoření aplikace pomocí rozhraní Elements API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Návod: Vytvoření aplikace pomocí rozhraní Reflection API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Návod: Vytvoření uživatelského rozhraní pomocí elementu JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON Markup](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Odkaz na UITableViewController – třída](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odkaz na UINavigationController – třída](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
