---
title: Úvod do scénářů v Xamarin.iosu
description: Tento dokument obsahuje úvod do scénáře v Xamarin.iOS. Popisuje, jak se ve scénáři používá k definování uživatelského rozhraní, přechody a chcete-li upravit soubory scénáře použití v iOS designeru.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: bd8fee1b8f1941203bb0e6f00e261cbfbbccc9a7
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242339"
---
# <a name="introduction-to-storyboards-in-xamarinios"></a>Úvod do scénářů v Xamarin.iosu

V této příručce vysvětlíme, jaké scénáře je a prozkoumat některé z klíčových komponent – například přechody. Podíváme na tom, jak můžete vytvořit a použít, scénáře a jaké výhody mají pro vývojáře.

Před formát souboru scénáře byla zavedena společností Apple jako vizuální znázornění uživatelského rozhraní aplikace pro iOS, vývojáři vytvořené soubory XIB pro každý kontroler zobrazení a navigaci mezi každé zobrazení naprogramovat ručně.  Pomoci scénáře umožňuje vývojářům definovat kontrolery zobrazení a navigaci mezi nimi na návrhové ploše a nabízí WYSIWYG úpravy uživatelského rozhraní pro aplikace.

Ve scénáři může být vytvořeno, otevřít a upravit článek přeložený překladatelem Xamarin pro iOS Designer. Tato příručka se také návod jak používat návrháře k vytvoření vašich scénářů při použití jazyka C# programovat navigaci.


## <a name="requirements"></a>Požadavky

Scénáře je možné v IOS designeru v sadě Visual Studio pro Mac nebo Visual Studio 2015 a 2017 s úlohami Xamarin nainstalovat.

## <a name="what-is-a-storyboard"></a>Co je scénáře?

Scénář je vizuální znázornění všech obrazovek v aplikaci. Obsahuje řadu scény, pomocí nichž každá představuje scény *kontroler zobrazení* a jeho *zobrazení*. Tato zobrazení může obsahovat objekty a [ovládací prvky](~/ios/user-interface/controls/index.md) , který vám umožní uživatelů k interakci s vaší aplikací. Tuto sadu zobrazení a ovládacích prvků (nebo *dílčích zobrazení*) se označuje jako *obsahu zobrazit hierarchii*. Scén připojeni pomocí přechod na to objekty, které představují přechod mezi kontrolery zobrazení. To se obvykle dosahuje vytváření segue mezi objektu na počáteční zobrazení a zobrazení připojení. Relace na návrhové ploše jsou znázorněné na následujícím obrázku:

 [![](images/storyboardsview.png "Na tomto obrázku jsou znázorněny relací na návrhové ploše")](images/storyboardsview.png#lightbox)

Jak je znázorněno, scénář bude každý z vašich scén rozložení s obsahem již vykreslen a znázorňuje připojení mezi nimi.  Je vhodné poznamenat, že když mluvíme o scén na Iphonu, se dá předpokládat, že jedna v tomto okamžiku *scény* ve scénáři je rovna jednomu *obrazovky* obsahu na zařízení. Ale vzhledem k Ipadu, který je možné mít více scén se zobrazí najednou – například používá Popover kontroler zobrazení.

Existuje mnoho výhod použití scénářů pro vytvoření uživatelského rozhraní aplikace, zejména při používání Xamarin. Za prvé, je vizuální znázornění uživatelského rozhraní, jako všech objektů, včetně [vlastní ovládací prvky](~/ios/user-interface/designer/ios-designable-controls-overview.md) – jsou generovány v době návrhu. To znamená, že před sestavení nebo nasazení aplikace můžete vizualizovat její vzhled a flow. Vezměme si jako příklad obrázku výše. Mohli bychom to zjistit z rychlý přehled návrhu jsou surface kolik existuje scény, rozložení jednotlivých zobrazení a jak všechno, co souvisí. To dělá scénáře takový dopad.

Události jsou snáze spravovatelné s použitím scénářů, zejména při použití v iOS designeru. Většina ovládacích prvků uživatelského rozhraní bude mít seznam možných událostí v oblasti vlastnosti. Obslužná rutina události lze přidávat tady a dokončit v částečné metody ve třídě Kontrolery zobrazení...

Obsah ve scénáři se ukládá jako soubor XML. Čas, sestavení na jakékoli `.storyboard` soubory jsou kompilovány do binárních souborů, které jsou známé jako drť. Za běhu jsou tyto drť inicializován a vytvořit instanci k vytvoření nového zobrazení.

## <a name="segues"></a>Přechody

A *Segue*, nebo *přechod na to objekt*, se používá při vývoji pro iOS k reprezentaci přechod mezi scény. Chcete-li vytvořit segue, podržte **Ctrl** klíč a klikněte na tlačítko přetažením z jednoho scény do jiného. Přetahování jsme naše myši, zobrazí se modrý konektor určující, kde segue povede, jak je ukázáno na následujícím obrázku:

 [![](images/createsegue.png "Zobrazí se modrý konektor, určující, kde segue povede, jak je ukázáno v tomto obrázku")](images/createsegue.png#lightbox)

Na myši nahoru se zobrazí nabídka dovolte nám vybrat akci pro naše segue. Může vypadat podobně jako následující obrázky: 

**Předběžné iOS 8 a velikost třídy**:

[![](images/segue1.png "Rozevírací seznam přechod na to akce bez třídy velikostí")](images/segue1.png#lightbox)

**Při použití třídy velikostí a adaptivní přechody**:

[![](images/16new.png "Přechod na to akce rozevírací seznam s třídy velikostí")](images/16new.png#lightbox)

> [!IMPORTANT]
> Pokud používáte VMWare pro virtuální počítač Windows, jako je mapován klávesu Ctrl a klikněte _klikněte pravým tlačítkem na_ tlačítko myši ve výchozím nastavení. Vytvoření Segue upravit předvolby klávesnice prostřednictvím **Předvolby** > **klávesnice a myši** > **zkratky myši** a přemapování vaší **Sekundární tlačítko** jak je znázorněno níže:
> 
> [![](images/image22.png "Nastavení předvoleb klávesnici a myš")](images/image22.png#lightbox)
> 
> Teď by měl být schopen přidat segue mezi vaše řadiče zobrazení jako za normálních okolností.

Existují různé druhy přechody, každý poskytne kontrolu nad jak se uživateli zobrazí nový kontroler zobrazení a interakci se ostatní řadiče zobrazení ve scénáři. Tyto informace jsou vysvětleny níže. Je také možné podtřídy objekt segue implementovat vlastní přechodu:

-  **Zobrazit / Push** – přechod na to push přidá kontroler zobrazení do navigačního zásobníku. Předpokládá se, že kontroler zobrazení pocházející nasdílení změn je součástí stejného kontroler navigace jako kontroler zobrazení, který je přidáván do zásobníku. Dělá to samé jako `pushViewController` a se obecně používají při některých relace mezi daty na obrazovkách. Pomocí nasdílení změn přechod na to poskytuje možnost navrhnout s navigační panel s tlačítko Zpět a nadpis přidán do jednotlivých zobrazení v zásobníku, umožňující navigaci zobrazit hierarchii k podrobnostem.
-  **Modální** – modální segue vytvořit relaci mezi žádné kontrolery dvě zobrazení ve vašem projektu, s možností na animovaný přechod zobrazeno. Kontroler zobrazení podřízených zcela skryje kontroler zobrazení nadřazené při zařazení do zobrazení. Na rozdíl od push přechod na to, který přidá tlačítko Zpět pro nás; Při použití modální přechod na to `DismissViewController` musí být použita, pokud chcete vrátit na předchozí kontroler zobrazení.
-  **Vlastní** – všechny vlastní segue lze vytvořit jako podtřída ` UIStoryboardSegue`.
-  **Unwind** – což je unwind přechod na to je možné přejít zpět prostřednictvím nabízených oznámení nebo modální okno přechod na to – například zrušením kontroler modálně uvedené zobrazení. Kromě toho můžete vrátit zpět prostřednictvím pouze jeden, ale řadu nabízených oznámení a modální okno přechody a vraťte se, že více kroků ve vaší hierarchii navigace pomocí jediného unwind akce. Chcete-li pochopit, jak používat unwind přechod na to v iOS, přečtěte si [vytváření Unwind přechody](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue) předpisu.
-  **Sourceless** – sourceless segue označuje scény obsahující kontroler počáteční zobrazení, a proto kterého zobrazení uživateli zobrazí jako první. Je reprezentován segue vidíte níže:  

    [![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Adaptivní přechod na to typy

 iOS 8 zavedené [třídy velikostí](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) umožňující soubor scénáře pro iOS pro práci se všemi velikostmi obrazovky k dispozici, umožňuje vývojářům vytvářet jednoho uživatelského rozhraní pro všechna zařízení s Iosem. Ve výchozím nastavení budou všechny nové aplikace Xamarin.iOS pomocí třídy velikostí. Pokud chcete použít třídy velikostí z starší projektu, přečtěte si [Úvod do scénáře sjednocené](~/ios/user-interface/storyboards/unified-storyboards.md) průvodce. 
 
Všechny aplikace využívajícího třídy velikosti také použít novou [ *adaptivní přechody*](~/ios/user-interface/storyboards/unified-storyboards.md). Při použití třídy velikostí, mějte na paměti, že jsme se zadáním přímo zda používáme iPhone nebo iPad. Jinými slovy vytváříme jedné uživatelské rozhraní, které budou vždy vypadat stejně, bez ohledu na to, kolik nemovitosti musí pracovat. Adaptivní přechody práce podle obsahovým prostředí a určení, jak nejlépe k předkládání obsahu. Adaptivní přechody jsou uvedeny níže: 

[![](images/adaptivesegue.png "Rozevírací seznam adaptivní přechody")](images/adaptivesegue.png#lightbox)

|Přechod na to|Popis|
|--- |--- |
|Show|To je velmi podobný Push přechod na to, ale zohledňuje obsah na obrazovce.|
|Zobrazit podrobnosti|Pokud aplikace zobrazí zobrazení seznamu a podrobností (například v kontroleru rozdělené zobrazení na Ipadu), nahradí obsah podrobné zobrazení. Pokud aplikace zobrazí pouze hlavní nebo podrobnosti, nahradí obsah nahoru zásobníkem kontroleru zobrazení.|
|Prezentace|To se podobá modální segue a umožňuje výběr prezentace a přechod stylů.|
|Popover prezentace|To představuje obsah jako popover|

### <a name="transferring-data-with-segues"></a>Přenos dat s přechody

Výhody segue neukončovat s přechody. Můžete také používají ke správě přenosu dat mezi kontrolery zobrazení. Toho můžete dosáhnout přepsáním `PrepareForSegue` metodu na kontroler počáteční zobrazení a zpracování dat, chceme. Když se aktivuje segue – například s stisknutí tlačítka – aplikace bude volat tuto metodu, poskytující příležitosti k přípravě nový kontroler zobrazení *před* dojde k jakékoli navigaci. Níže uvedený kód z [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) ukázky, ukazuje to: 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
}
```

V tomto příkladu `PrepareForSegue` metoda bude volána, když uživatel aktivuje segue. Nejprve musíme vytvořit instanci "přijímající" kontroler zobrazení a nastavíte jako cíl segue kontroler zobrazení. To se provádí na řádek kódu níže:

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Metoda má teď možnost můžete nastavit vlastnosti `DestinationViewController`. V tomto příkladu jsme využili to předáním seznam s názvem `PhoneNumbers` k `CallHistoryController` a její přiřazení k objektu se stejným názvem:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Po dokončení přechodu, uživateli se zobrazí `CallHistoryController` s naplněný seznam.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Přidání scénáře do projektu mimo scénář

V některých případech budete muset přidat do souboru dříve scénáře scénáře. Jednou to udělat v sadě Visual Studio pro Mac lze zefektivnit pomocí následujících kroků:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Vytvořte nový soubor scénář tak, že přejdete do **soubor > Nový soubor > iOS > scénáře**, jak je znázorněno níže: 
    
    [![](images/new-storyboard-xs.png "Dialogové okno Nový soubor")](images/new-storyboard-xs.png#lightbox)

2. Přidat název scénáře k **hlavní rozhraní** část **Info.plist**, jak je znázorněno níže:
    
    [![](images/infoplist.png "V editoru Info.plist")](images/infoplist.png#lightbox)
    
    Dělá to ekvivalent vytvoření instance počáteční kontroler zobrazení v `FinishedLaunching` metody v rámci delegáta aplikace. Nastavenou tuto možnost, aplikace vytvoří instanci okna (viz níže), načte hlavní storyboard a přiřadí instance Kontroleru zobrazení počáteční do scénáře (jeden vedle sourceless Segue) jako `RootViewController` vlastnost okno a potom provede okno, které jsou viditelné na obrazovce.

3. V `AppDelegate`, přepsat výchozí `Window` metodu s následující kód do implementace okna vlastnosti:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Vytvořte nový soubor scénáře kliknutím pravým tlačítkem na projekt tak, aby **Přidat > Nový soubor > iOS > prázdný scénář**, jak je znázorněno níže: 
    
    [![](images/new-storyboard-vs.png "Dialogové okno nové položky")](images/new-storyboard-vs.png#lightbox)

2. Přidat název scénáře k **hlavní rozhraní** části iOS aplikace, jak je znázorněno níže:
    
    [![](images/ios-app.png "V editoru Info.plist")](images/ios-app.png#lightbox)
    
    Dělá to ekvivalent vytvoření instance počáteční kontroler zobrazení v `FinishedLaunching` metody v rámci delegáta aplikace. Nastavenou tuto možnost, aplikace vytvoří instanci okna (viz níže), načte hlavní storyboard a přiřadí instance Kontroleru zobrazení počáteční do scénáře (jeden vedle sourceless Segue) jako `RootViewController` vlastnost okno a potom provede okno, které jsou viditelné na obrazovce.

3. V `AppDelegate`, přepsat výchozí `Window` metodu s následující kód do implementace okna vlastnosti:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Vytvoření scénáře v IOS designeru

Scénář je možné vytvořit pomocí návrháře Xamarin pro iOS, která je integrována bez problémů pomocí sady Visual Studio pro Mac a Visual Studio.

Pokud chcete začít používat v iOS designeru vytvoření scénářů, postupujte [Hello, iOS s více obrazovkami](~/ios/get-started/hello-ios-multiscreen/index.md) průvodce. V tomto názorném postupu zkoumáte navigace mezi Kontrolery zobrazení pomocí přechody a tom, jak zpracovávat události pro ovládací prvky.

## <a name="instantiate-storyboards-manually"></a>Ručně vytvořit instanci scénáře

Scénáře zcela nahradit jednotlivé soubory XIB ve vašem projektu, ale kontrolery zobrazení jednotlivých ve scénáři můžete stále vytvořit instanci pomocí `Storyboard.InstantiateViewController`.

Někdy aplikace mají speciální požadavky, které nelze zpracovat pomocí předdefinovaných scénáře přechody poskytované návrháře. Například pokud bychom měli vytvořit aplikaci, která spouští různé obrazovky pomocí stejného tlačítka, v závislosti na aktuální stav aplikace, může chceme ručně vytvořit instanci kontrolery zobrazení a program přechod sami.

Následující snímek obrazovky ukazuje dva řadiče zobrazení na naše návrhové ploše bez přechod na to mezi nimi. V další části provede jak tohoto přechodu lze upravit v kódu.

 [![](images/viewcontrollerspink.png "Tento snímek obrazovky ukazuje dva řadiče zobrazení na návrhové ploše bez přechod na to mezi nimi")](images/viewcontrollerspink.png#lightbox)

1. Přidat _prázdný scénář pro iPhone_ do existujícího projektu projektu:
    
    [![](images/add-storyboard1.png "Přidávání scénářů")](images/add-storyboard1.png#lightbox)

2. Dvakrát klikněte na nově vytvořený scénář tak, aby ho otevřete a přidat nový **kontroler navigace** na návrhovou plochu. Protože kontroler navigace bez uživatelského rozhraní, ve výchozím nastavení se dodávají s kontroler zobrazení kořenové, jak je znázorněno níže:

    [![](images/uinavigationcontroller.png "Přechody Kontrolerů zobrazení s")](images/uinavigationcontroller.png#lightbox)

3. Vyberte _kontroler zobrazení_ po kliknutí na černý pruh v dolní části. V okně Návrhář **vlastnost Pad**v části **Identity** můžeme určit vlastní třídu, stejně jako jedinečný Identifikátor pro kontroler zobrazení. Nastavte **název třídy** a **ID scénáře** k `MainViewController`.

    [![](images/identitypanelnew.png "Zadejte vlastní třídy")](images/identitypanelnew.png#lightbox)

4. Později, budeme muset vytvořit instanci naše kontrolery zobrazení scénáře a použije k odkazování v našem kódu ID scénáře. Nastavení ID obnovení tak, aby odpovídaly ID scénáři zajišťuje, že kontroler zobrazení získá znovu vytvořit správně Pokud státu potřeba obnovit.

5. Aktuálně máme pouze jeden kontroler zobrazení. Přetáhněte jiný kontroler zobrazení na návrhové ploše. V **vlastnost Pad**, identitou, nastavte třídu a ID scénáře `PinkViewController`, jak je znázorněno níže:

    [![](images/pinkvcnew.png "Vlastnost Pad")](images/pinkvcnew.png#lightbox)
    
    Rozhraní IDE vytvoří tyto vlastní třídy pro kontrolery zobrazení. Ty lze zobrazit v **oblasti řešení**, jak je znázorněno v následujícím snímku obrazovky:
    
    [![](images/solution-pad.png "Oblasti řešení")](images/solution-pad.png#lightbox)

6. V `PinkViewController`, vyberte zobrazení kliknutím směrem do středu rámce kontroleru. V oblasti vlastnosti v rámci zobrazení změnit **pozadí** fialově:
    
    [![](images/pinkcontroller.png "Nastavení barvy pozadí")](images/pinkcontroller.png#lightbox)

7. A konečně, přetáhněte tlačítko z **nástrojů** na `MainViewController`. V oblasti vlastnosti jí název `PinkButton` a GoToPink Title, jak je znázorněno níže:

    [![](images/pinkbutton.png "Nastavte název tlačítka")](images/pinkbutton.png#lightbox)

Scénář je úplný, ale pokud jsme teď nasadit projekt, jsme se zobrazí prázdnou obrazovku. Důvodem je, musíme říct rozhraní IDE k používání našich scénáře a nastavení řadiče zobrazení kořenové sloužit jako první zobrazení. Obvykle to můžete udělat přes naše možnosti projektu, jak je znázorněno výše. Ale v tomto příkladu jsme se dosáhne stejného výsledku v kódu, přidáním následujícího **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

To je spousta kódu, ale stačí několik řádků neznámé. Nejprve jsme zaregistrujte náš scénář s **AppDelegate** předáním názvu do scénáře **MainStoryboard**. Dále jsme informace aplikace pro vytvoření instance řadič počáteční zobrazení scénáře voláním `InstantiateInitialViewController` v našem scénáři a nastavíme kontroleru zobrazení jako kontroler zobrazení kořenové naši aplikaci. Tato metoda určuje první obrazovka, která se uživateli zobrazí, a vytvoří novou instanci třídy Kontroleru zobrazení.

Všimněte si, že v podokně řešení, která si vytvořila integrovaného vývojového prostředí `MainViewcontroller.cs` třídy a jeho `corresponding designer.cs` když jsme přidali název třídy do oblasti vlastnosti v kroku 4. Můžeme vidět tuto třídu vytvoří speciální konstruktor, který obsahuje základní třídu:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Při vytváření scénáře použití návrháře, rozhraní IDE automaticky přidá [[zaregistrovat]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atribut v horní části `designer.cs` třídy a předejte mu identifikátor řetězce, který se shoduje s ID scénáře podle předchozí krok. To bude propojovat jazyka C# relevantní scény ve scénáři.

Někdy můžete chtít přidat existující třídu, která byla **není** vytvořené v návrháři. V takovém případě by zaregistrujete tuto třídu jako za normálních okolností:

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

Další informace o registraci třídy a metody, naleznete [typ registrátora](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) dokumentaci.

Posledním krokem v této třídě je nastavit na tlačítko a přecházet na kontroler růžový zobrazení. Doporučujeme vám vytvořit instanci `PinkViewController` ze scénáře; poté bude programujeme push přechod na to s `PushViewController`, jak je znázorněno podle níže uvedeného ukázkového kódu:

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

Spuštění aplikace vytváří aplikace 2 obrazovky:

![](images/finishedstoryboard.png "Běh obrazovky ukázkové aplikace")

## <a name="conditional-segues"></a>Podmíněné přechody

Přesune z jedné kontroleru zobrazení k další je často závisí na určitou podmínku. Například, pokud jsme provedli jednoduchý přihlašovací obrazovka by pouze chceme přesunout na další obrazovce *Pokud* kdyby byly ověřeny uživatelské jméno a heslo.

V následujícím příkladu přidáme heslo pole příkladu výše. Pouze uživatel bude mít přístup *PinkViewController* po zadání správné heslo, jinak chyba se zobrazí.

Než začneme, použijte kroky 1 – 8 výše. V těchto kroků můžeme vytvořit náš scénář, začít vytvářet naší uživatelské rozhraní a řekněte naše delegáta aplikace řadiče zobrazení jako je RootViewController.

1. Nyní Pojďme vytvořit naše uživatelské rozhraní a přidat další zobrazení uvedené `MainViewController` aby vypadala jako v následujícím snímku obrazovky:

    - UITextField
        - Název: PasswordTextField
        - Zástupný symbol: Zadejte tajný klíč do
    - UILabel
        - Text: "Chyba: nesprávné heslo. Nebudou předávat! "
        - Barva: červená
        - Zarovnání: System Center
        - Řádky: 2
        - Zaškrtnuté políčko 'skryté. 
        
    [![](images/passwordvc.png "System Center řádky")](images/passwordvc.png#lightbox)
    
2. Vytvoření Segue mezi tlačítko Přejít na růžovou a kontroler zobrazení přetažením Ctrl-z *PinkButton* k *PinkViewController*a výběrem **Push** na myši nahoru . 

3. Klikněte na Segue a přiřaďte mu *identifikátor* `SegueToPink`:

    [![](images/namesegue.png "Klikněte na Segue a přidělte mu identifikátor SegueToPink")](images/namesegue.png#lightbox)  
    

4. Nakonec přidejte následující metodu do ShouldPerformSegue `MainViewController` třídy:

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

V tomto kódu jste spojili jsme segueIdentifier do našich `SegueToPink` přechod na to, aby potom jsme mohli otestovat podmínku; v tomto případě platné heslo. Pokud naše podmínka vrátí `true`, Segue provede a nabídne `PinkViewController`. Pokud `false`, nebudou se vám nový kontroler zobrazení.

Tento přístup jsme můžete použít libovolný Segue v tomto kontroleru zobrazení tak, že zkontrolujete segueIdentifier argument k metodě ShouldPerformSegue. V tomto případě máme pouze jeden identifikátor Segue – `SegueToPink`.

Odkazovat na Storyboards.Conditional řešení v [ukázkové scénáře ruční](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) pro funkční příklad.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Použití odkazů scénáře

Odkaz scénáře vám umožní provést velké a komplexní scénáře návrhu a rozdělit na menší scénáře, které získáte na něj odkazovat z původní, tedy odebírání odebrání složitost a provádění výsledný jednotlivých scénářů snadněji návrhu a Udržujte.

Kromě toho můžete zadat odkaz scénáře _ukotvení_ do jiného scény v rámci stejné scénáře nebo konkrétní scény na jiný.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Odkazování na externí scénáře

Chcete-li přidat odkaz na externí scénáře, postupujte takto:

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na název projektu a vyberte **přidat** > **nový soubor...**   >  **iOS** > **scénáře**. Zadejte **název** pro nový scénář a klikněte na tlačítko **nový** tlačítka:
    
    [![](images/ref01.png "Dialogové okno Nový soubor")](images/ref01.png#lightbox)
    
2. Návrh rozložení scén nový scénář obvykle by a uložte změny: 
    
    [![](images/ref02.png "Rozložení novou scénu")](images/ref02.png#lightbox)
    
3. Otevřete scénář, který budete přidávat odkaz na v iOS designeru.

4. Přetáhněte **scénáře odkaz** z **nástrojů** na návrhovou plochu: 
    
    [![](images/ref03.png "Scénář odkazu")](images/ref03.png#lightbox)
    
5. V **Widget** karty **Průzkumník vlastností**, vyberte název **scénáře** , kterou jste vytvořili výše: 

    [![](images/ref04.png "Na kartě widgetu")](images/ref04.png#lightbox)
    
6. Kliknutí s klávesou Control na uživatelského rozhraní widgetu (jako tlačítko) na stávajících scény a vytvořit nové Segue k **scénáře odkaz** , který jste právě vytvořili: 

    [![](images/ref05.png "Vytváření segue")](images/ref05.png#lightbox) 
    
7. V místní nabídce vyberte **zobrazit** dokončit Segue: 

    [![](images/ref06.png "Výběrem možnosti zobrazit na dokončení Segue")](images/ref06.png#lightbox) 
    
8. Uložte provedené změny do scénáře.

Zobrazí se při spuštění aplikace a uživatel klikne na prvek uživatelského rozhraní, které jste vytvořili Segue z počáteční kontroler zobrazení z externí scénáře zadaný v referenci Storyboard.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Odkazování na konkrétní scény v externí scénáře

Chcete-li přidat odkaz na konkrétní scény externí scénáře (a ne počáteční kontroler zobrazení), postupujte takto:

1. V **Průzkumníka řešení**, dvakrát klikněte na externí scénář tak, aby ji otevřete pro úpravy.

2. Přidat novou scénu a navrhněte rozložení běžným způsobem: 

    [![](images/ref07.png "Nové rozložení scény")](images/ref07.png#lightbox)
    
3. V **Widget** kartě **Průzkumník vlastností**, zadejte **ID scénáře** pro novou scénu kontroler zobrazení: 

    [![](images/ref08.png "Zadejte ID scénáře pro nový kontroler zobrazení scén")](images/ref08.png#lightbox)
    
3. Otevřete scénář, který budete přidávat odkaz na v iOS designeru.

4. Přetáhněte **scénáře odkaz** z **nástrojů** na návrhovou plochu: 

    [![](images/ref03.png "Scénář odkazu")](images/ref03.png#lightbox)
    
5. V **Widget** karty **Průzkumník vlastností**, vyberte název **scénáře** a **referenční ID** (ID scénáře) z Scény, který jste vytvořili výše: 

    [![](images/ref09.png "Na kartě widgetu ")](images/ref09.png#lightbox)
    
6. Kliknutí s klávesou Control na uživatelského rozhraní widgetu (jako tlačítko) na stávajících scény a vytvořit nové Segue k **scénáře odkaz** , který jste právě vytvořili: 

    [![](images/ref10.png "Vytváření segue")](images/ref10.png#lightbox) 
    
7. V místní nabídce vyberte **zobrazit** dokončit Segue: 

    [![](images/ref06.png "Výběrem možnosti zobrazit na dokončení Segue")](images/ref06.png#lightbox) 
    
8. Uložte provedené změny do scénáře.

Pokud je aplikace spustit a uživatel klikne na prvek uživatelského rozhraní, který jste vytvořili Segue od scény s daný **ID scénáře** z externí scénáře podle scénáře odkazu se zobrazí.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Odkazování na konkrétní scény ve stejné scénáře

Chcete-li přidat odkaz na konkrétní scény stejné scénáře, postupujte takto:

1. V **Průzkumníka řešení**, dvakrát klikněte na scénář tak, aby ji otevřete pro úpravy.

2. Přidat novou scénu a navrhněte rozložení běžným způsobem: 

    [![](images/ref11.png "Nové rozložení scény")](images/ref11.png#lightbox)

3. V **Widget** kartě **Průzkumník vlastností**, zadejte **ID scénáře** pro novou scénu kontroler zobrazení: 

    [![](images/ref12.png "Na kartě widgetu")](images/ref12.png#lightbox)
    
3. Přetáhněte **scénáře odkaz** z **nástrojů** na návrhovou plochu: 

    [![](images/ref03.png "Scénář odkazu")](images/ref03.png#lightbox)
    
5. V **Widget** karty **Průzkumník vlastností**vyberte **referenční ID** (ID scénáře) scény, který jste vytvořili výše: 

    [![](images/ref13.png "Na kartě widgetu")](images/ref13.png#lightbox)
    
6. Kliknutí s klávesou Control na uživatelského rozhraní widgetu (jako tlačítko) na stávajících scény a vytvořit nové Segue k **scénáře odkaz** , který jste právě vytvořili: 

    [![](images/ref14.png "Vytváření segue")](images/ref14.png#lightbox) 
    
7. V místní nabídce vyberte **zobrazit** dokončit Segue: 

    [![](images/ref06.png "Výběrem možnosti zobrazit na dokončení Segue")](images/ref06.png#lightbox) 
    
8. Uložte provedené změny do scénáře.

Pokud je aplikace spustit a uživatel klikne na prvek uživatelského rozhraní, který jste vytvořili Segue od scény s daný **ID scénáře** ve stejné scénáře zadaný v referenci Storyboard zobrazí.

## <a name="summary"></a>Souhrn

Tento článek představuje pojem scénářů a jak může být užitečné při vývoji aplikací pro iOS. Popisuje scény, kontrolery zobrazení, zobrazení a zobrazení hierarchie a jak jsou scén propojené s různé druhy přechody.  Je také popisuje konkretizujete kontrolery zobrazení ručně z scénáře a vytváření podmíněné přechody.



## <a name="related-links"></a>Související odkazy

- [Ruční scénáře (ukázka)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [Úvod do Iosu návrháře](~/ios/user-interface/designer/introduction.md)
- [Převod se scénáři](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [Referenční třída UIStoryboard](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
