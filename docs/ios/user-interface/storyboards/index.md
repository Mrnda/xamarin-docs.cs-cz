---
title: Úvod do scénářů
description: Scénář je vizuální reprezentace vzhled a tok vaší aplikace. Xamarin má zavedl Designer umožňuje aplikace Xamarin.iOS využívat výhod scénářů, abyste mohli vizuální návrh obrazovky aplikace a přístup k zobrazení, řadiče a segues pomocí C# pro další ovládací prvek.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 647bd7d339dc56978752f7ab29de30cf8acb7e07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-storyboards"></a>Úvod do scénářů

_Scénář je vizuální reprezentace vzhled a tok vaší aplikace. Xamarin má zavedl Designer umožňuje aplikace Xamarin.iOS využívat výhod scénářů, abyste mohli vizuální návrh obrazovky aplikace a přístup k zobrazení, řadiče a segues pomocí C# pro další ovládací prvek._

V této příručce vysvětlíme, jaké scénáře je a prozkoumat některé z klíčových součástí – například Segues. Podíváme jak vytvořit a použít, scénářů a jaké výhody mají pro vývojáře.

Před formát souboru Storyboard společností Apple jako vizuální znázornění uživatelského rozhraní aplikace pro iOS, vývojáři vytvoří soubory XIB u každého řadiče zobrazení a naprogramovat přecházení mezi jednotlivých zobrazení ručně.  Pomocí scénář umožňuje vývojáři definovat řadiče zobrazení a navigace mezi nimi na návrhovou plochu a nabízí WYSIWYG úpravy uživatelského rozhraní pro aplikace.

Scénář můžete vytvořit, otevřít a upravit s Xamarin iOS Designer. Tato příručka se také návod jak používat návrháře k vytvoření vašeho scénářů při použití jazyka C# do programu navigaci.


## <a name="requirements"></a>Požadavky

Scénářů lze iOS Návrhář v sadě Visual Studio pro Mac nebo s Visual Studio 2015 a 2017 s úlohami Xamarin nainstalována.

## <a name="what-is-a-storyboard"></a>Co je scénáře?

Scénář je vizuální reprezentace všechny obrazovky v aplikaci. Obsahuje posloupnost scény s každou scény představující *View Controller* a jeho *zobrazení*. Tato zobrazení může obsahovat objekty a [ovládací prvky](~/ios/user-interface/controls/index.md) který vám umožní vaše uživatelům interakci s vaší aplikací. Tato kolekce zobrazení a ovládací prvky (nebo *dílčích zobrazení*) se označuje jako *obsahu zobrazení hierarchie*. Scény připojeni pomocí segue objekty, které představují přechod mezi řadiče zobrazení. Dosahuje se obvykle vytvořením segue mezi objektu na počáteční zobrazení a připojování zobrazení. Vztahy na návrhovou plochu, která jsou zobrazené na obrázku níže:

 [![](images/storyboardsview.png "Na tomto obrázku jsou znázorněny vztahy na návrhovou plochu")](images/storyboardsview.png#lightbox)

Jak je znázorněno, každý z vaší scény bude Rozvrhněte s obsahem již vykreslen scénáři a znázorňuje připojení mezi nimi.  Je vhodné poznamenat nyní, když v souvislosti se děje v zařízení iPhone, je bezpečné předpokládají, že jeden *scény* ve scénáři je jednu *obrazovky* obsahu na zařízení. Nicméně s iPad, které je možné, že více scény se zobrazí najednou – například pomocí Popover zobrazení řadiče.

Existuje mnoho výhod pomocí scénářů pro vytvoření uživatelského rozhraní aplikace, zejména v případě, že pomocí Xamarin. Za prvé, je vizuální znázornění uživatelského rozhraní, jako všechny objekty – včetně [vlastní ovládací prvky](~/ios/user-interface/designer/ios-designable-controls-overview.md) – jsou vykreslovány v době návrhu. To znamená, že před sestavení nebo nasazení aplikace můžete vizualizovat její vzhled a toku. Výše uvedený obrázek, například trvat. Jsme poznáte z rychlý přehled návrhu jsou prostor kolik scény existuje, rozložení jednotlivých zobrazení a jak všechno, co souvisí. To je, takže efektivní díky scénářů.

Události jsou lepší správu bitlockeru s scénářů, zejména při použití iOS Designer. Většina ovládacích prvků uživatelského rozhraní bude mít seznam možných událostí v panelu pro vlastnosti. Obslužné rutiny události lze přidat sem a dokončit v částečné metody ve třídě zobrazení řadičů...

Obsah scénáře je uložený jako soubor XML. Čas, sestavení na všechny `.storyboard` soubory jsou zkompilovány do binární soubory, které jsou známé jako drť. Za běhu jsou tyto drť inicializován a instanci pro vytvoření nového zobrazení.

## <a name="segues"></a>Segues

A *Segue*, nebo *Segue objekt*, se používá v vývoj pro iOS k reprezentování přechod mezi scény. Chcete-li vytvořit segue, podržte klávesu **Ctrl** klíč a klikněte na tlačítko přetažení z jediné scény do jiného. Tažení jsme naše myš, se zobrazí modrý konektor, která určuje, kde segue povede, jak ukazuje následující obrázek:

 [![](images/createsegue.png "Se zobrazí modré konektor, která určuje, kde segue povede, jak je předvedeno v této bitové kopie")](images/createsegue.png#lightbox)

Na myši nahoru nabídky se zobrazí upozornění vyberte akci pro naše segue. Může vypadat podobně jako obrázky níže: 

**Předběžné iOS 8 a velikost třídy**:

[![](images/segue1.png "Rozevírací seznam Segue akce bez velikost třídy")](images/segue1.png#lightbox)

**Při použití třídy velikost a adaptivní Segues**:

[![](images/16new.png "Akce Segue rozevírací seznam s třídami velikost")](images/16new.png#lightbox)

> [!IMPORTANT]
> Pokud používáte VMWare pro virtuální počítač se systémem Windows, jako je namapovaný Ctrl + kliknutí _klikněte pravým tlačítkem na_ tlačítko myši ve výchozím nastavení. Pokud chcete vytvořit Segue, upravit upřednostňovaných klávesnice prostřednictvím **Předvolby** > **klávesnice a myš** > **zkratky myši** a přemapovat vaší **Sekundární tlačítko** jak je uvedeno dále:
> 
> [![](images/image22.png "Nastavení předvoleb klávesnici a myš")](images/image22.png#lightbox)
> 
> Teď by měla být možné přidat segue mezi své řadiče zobrazení jako normální.

Existují různé typy přechody, každý uvádějící kontrolu nad jak je nový řadič zobrazení zobrazovat uživateli, a jak komunikuje s jinými řadiči zobrazení ve scénáři. Tyto mechanismy jsou popsány níže. Je také možné podtřídami objekt segue implementovat vlastní přechod:

-  **Zobrazit / Push** – push segue přidá řadiče zobrazení v navigačním zásobníku. Předpokládá se, že řadiče zobrazení pocházející nabízeného oznámení je součástí stejného řadiče navigační jako řadič zobrazení, který je přidáván do zásobníku. To provede stejnou akci jako `pushViewController` a se běžně používají, pokud je některé vztah mezi daty na obrazovkách. Pomocí nabízeného oznámení segue vám dává možnost s navigační panel s tlačítko Zpět a title přidány do jednotlivých zobrazení v zásobníku, což rozbalení navigační prostřednictvím zobrazení hierarchie.
-  **Modální** – modální segue vytvořit relaci mezi jakékoli dvě zobrazení řadičů v projektu, s možností animovaný přechodu se zobrazí. Řadiče zobrazení podřízených bude zcela skrývat nadřazené řadič zobrazení po začlenění do zobrazení. Na rozdíl od push segue, který přidá tlačítko Zpět pro nás; Při použití modální segue `DismissViewController` cílem vrátit do předchozího řadiče zobrazení se musí použít.
-  **Vlastní** – všechny vlastní segue se dá vytvořit jako podtřídou třídy ` UIStoryboardSegue`.
-  **Unwind** – na unwind segue slouží k přejděte zpátky pomocí nabízených nebo modální segue – například zrušením řadiče modálně uvedené zobrazení. Kromě toho můžete unwind prostřednictvím ne jenom jednoho, ale řadu nabízení a modální segues a vraťte se, že několik kroků ve vaší hierarchii navigační s jedním unwind akce. Abyste pochopili, jak používat unwind segue v iOS, přečtěte si [vytváření Unwind Segues](https://developer.xamarin.com/recipes/ios/general/storyboard/unwind_segue/) recepturách.
-  **Sourceless** – sourceless segue označuje scény obsahující řadič počáteční zobrazení a které zobrazit uživatele se proto zobrazí první. Je zobrazena ve segue vidíte níže:  

    [![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Adaptivní Segue typy

 iOS 8 zavedená [velikost třídy](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) umožňující soubor storyboard iOS pro práci s všech velikostí dostupné obrazovky umožňuje vývojářům vytvářet jednoho uživatelského rozhraní pro všechna zařízení s iOS. Ve výchozím nastavení použije všechny nové aplikace Xamarin.iOS velikost třídy. Použití tříd velikost ze starší projektu, najdete v tématu [Úvod do Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) průvodce. 
 
Všechny aplikace pomocí třídy velikost také použít novou [ *adaptivní Segues*](~/ios/user-interface/storyboards/unified-storyboards.md). Při použití třídy velikosti, nezapomeňte, že jsme nejsou zadání přímo wether používáme iPhone nebo iPad. Jinými slovy vytváříme jeden uživatelské rozhraní, které bude vždy vypadají stejně, bez ohledu na to, kolik nemovitosti musí pracovat. Adaptivní Segues pracovní posuzování prostředí a určení, jak nejlépe prezentovat obsah. Níže jsou uvedeny adaptivní Segues: 

[![](images/adaptivesegue.png "Adaptivní Segues rozevíracího seznamu")](images/adaptivesegue.png#lightbox)

|Segue|Popis|
|--- |--- |
|Zobrazit|To je velmi podobné Push segue, ale obsah obrazovky bere v úvahu.|
|Zobrazit podrobnosti|Pokud aplikace zobrazí zobrazení seznamu a podrobností (například v kontroleru zobrazení rozdělení na Ipadu), nahradí obsah podrobné zobrazení. Pokud aplikace zobrazí, jenom na hlavní server nebo podrobností, nahradí obsah horní části zásobníku řadiče zobrazení.|
|Prezentace|To je podobná modální segue a umožňuje výběr prezentace a přechod stylů.|
|Popover prezentace|To představuje obsah jako popover|

### <a name="transferring-data-with-segues"></a>Přenos dat pomocí Segues

Výhody segue nemáte končit přechody. Můžete také používají ke správě přenosu dat mezi řadiče zobrazení. Toho se dosáhne přepsání `PrepareForSegue` metodu řadiče počáteční zobrazení a zpracování dat sebe. Když se aktivuje segue – například s stisknutí tlačítka – aplikace bude volat tuto metodu, což poskytuje příležitost k přípravě nového řadiče zobrazení *před* žádné navigaci. Kód uvedený níže, z [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) ukázkové, představuje to: 


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

V tomto příkladu `PrepareForSegue` metoda bude volána, když se aktivuje segue uživatelem. Nejprve musíme vytvořit instanci 'přijímající' řadiče zobrazení a nastavte jako cíl segue řadiče zobrazení. K tomu je potřeba na řádek kódu níže:

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Metoda má teď možnost můžete nastavit vlastnosti `DestinationViewController`. V tomto příkladu jsme provedli výhod to předáním seznam s názvem `PhoneNumbers` k `CallHistoryController` a jeho přiřazení k objektu se stejným názvem:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Po dokončení přechodu, uživatel uvidí `CallHistoryController` s vyplněná seznamu.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Přidávání scénáře do jiných Storyboard projektu

Potřebujete v některých případech přidat do souboru dříve scénáře scénáře. Pomocí následujícího postupu můžete zjednodušený jednou díky tomuto v sadě Visual Studio pro Mac:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Vytvoření nového souboru Storyboard procházením **soubor > Nový soubor > iOS > Storyboard**, jak je uvedeno dále: 
    
    [![](images/new-storyboard-xs.png "Dialogové okno Nový soubor")](images/new-storyboard-xs.png#lightbox)

2. Přidání názvu Storyboard k **hlavní rozhraní** části **Info.plist**, jak je uvedeno níže:
    
    [![](images/infoplist.png "Info.plist editor")](images/infoplist.png#lightbox)
    
    To se o ekvivalent vytváření instancí počáteční řadiče zobrazení v `FinishedLaunching` metoda v rámci delegáta aplikace. S touto sadou možnost aplikace vytvoří okno (viz níže), načte hlavní storyboard a přiřadí instance do scénáře počáteční View Controller (jeden vedle sourceless Segue) jako `RootViewController` vlastnost okna a potom díky okno viditelný na obrazovce.

3. V `AppDelegate`, potlačit výchozí `Window` metoda s následující kód do implementace vlastnost okno:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Vytvoření nového souboru Storyboard kliknutím pravým tlačítkem na projekt, který má **Přidat > Nový soubor > iOS > prázdná Storyboard**, jak je uvedeno dále: 
    
    [![](images/new-storyboard-vs.png "Dialogové okno Nový položky")](images/new-storyboard-vs.png#lightbox)

2. Přidání názvu Storyboard k **hlavní rozhraní** části IOS aplikací, jak je uvedeno níže:
    
    [![](images/ios-app.png "Info.plist editor")](images/ios-app.png#lightbox)
    
    To se o ekvivalent vytváření instancí počáteční řadiče zobrazení v `FinishedLaunching` metoda v rámci delegáta aplikace. S touto sadou možnost aplikace vytvoří okno (viz níže), načte hlavní storyboard a přiřadí instance do scénáře počáteční View Controller (jeden vedle sourceless Segue) jako `RootViewController` vlastnost okna a potom díky okno viditelný na obrazovce.

3. V `AppDelegate`, potlačit výchozí `Window` metoda s následující kód do implementace vlastnost okno:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Vytváření scénáře s iOS návrháře

Scénář lze vytvořit pomocí návrháře Xamarin pro iOS, které bylo integrováno bezproblémově Visual Studio pro Mac a Visual Studio.

Chcete-li začít používat iOS Designer pro vytvoření scénářů, postupujte [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) průvodce. V tomto návodu zaměříte navigace mezi řadiče zobrazení pomocí Segues a zpracování událostí na vaše ovládací prvky.

## <a name="instantiate-storyboards-manually"></a>Ručně vytvořit instanci scénářů

Scénářů úplně nahradit jednotlivé XIB soubory v projektu, ale řadiče jednotlivých zobrazení v scénáře můžete přesto vytvořit instanci pomocí `Storyboard.InstantiateViewController`.

Někdy aplikací mít speciální požadavky, které nelze zpracovat s přechody předdefinované storyboard poskytované návrháře. Například pokud nám chcete-li vytvořit aplikaci, která spouští různé obrazovky ze stejného tlačítka, v závislosti na aktuální stav aplikace, může chceme ručně vytvořit instanci řadiče zobrazení a program přechodu sebe.

Následující snímek obrazovky ukazuje, že dva řadiče zobrazení na našem návrhové ploše bez segue mezi nimi. V další části provede jak můžete tento přechod nastavit v kódu.

 [![](images/viewcontrollerspink.png "Tento snímek obrazovky ukazuje, že dva řadiče zobrazení na návrhovou plochu bez segue mezi nimi")](images/viewcontrollerspink.png#lightbox)

1. Přidat _prázdný iPhone Storyboard_ do existujícího projektu projektu:
    
    [![](images/add-storyboard1.png "Přidání scénáře")](images/add-storyboard1.png#lightbox)

2. Dvakrát klikněte na nově vytvořený storyboard jej otevřete a přidejte nový **navigační řadiče** na plochu návrháře. Jak je navigace řadič bez uživatelského rozhraní, ve výchozím nastavení se dodává s řadič zobrazení kořenové, jak je uvedeno dále:

    [![](images/uinavigationcontroller.png "Zobrazení, Kontrolery Segues")](images/uinavigationcontroller.png#lightbox)

3. Vyberte _View Controller_ kliknutím na černé panelu v dolní části. V nástroji Designer **vlastnost Pad**v části **Identity** lze zadat vlastní třídu, jakož i jedinečné ID pro řadiče zobrazení. Nastavte **název třídy** a **Storyboard ID** k `MainViewController`.

    [![](images/identitypanelnew.png "Zadejte vlastní třídy")](images/identitypanelnew.png#lightbox)

4. Později, budeme muset vytvořit instanci naše řadiče zobrazení scénáře a bude pomocí Storyboard ID odkazujte na ně v našem kódu. Nastavení obnovení ID se má shodovat s ID Storyboard zajistí, že kontroleru zobrazení získá znovu vytvořit správně Pokud stav je nutné obnovit.

5. Jsme aktuálně mít pouze jeden řadič zobrazení. Přetáhněte jiného řadiče zobrazení na návrhovou plochu. V **vlastnost Pad**, pod identitou, nastavení třídy a Storyboard ID `PinkViewController`, jak je uvedeno dále:

    [![](images/pinkvcnew.png "Vlastnost odsazení")](images/pinkvcnew.png#lightbox)
    
    Prostředí IDE vytvoří tyto vlastní třídy pro řadiče zobrazení. Ty lze zobrazit v **řešení Pad**, jak ukazuje následující snímek obrazovky:
    
    [![](images/solution-pad.png "Odsazení řešení")](images/solution-pad.png#lightbox)

6. V `PinkViewController`, vyberte zobrazení kliknutím směrem k centru rámce kontroleru. V panelu pro vlastnosti v rámci zobrazení změnit **pozadí** fialově:
    
    [![](images/pinkcontroller.png "Nastavení barvy pozadí")](images/pinkcontroller.png#lightbox)

7. Nakonec přetáhněte tlačítko z **sada nástrojů** na `MainViewController`. V panelu pro vlastnosti poskytněte název `PinkButton` a název GoToPink, jak je uvedeno dále:

    [![](images/pinkbutton.png "Nastavte název tlačítka")](images/pinkbutton.png#lightbox)

Scénář je úplný, ale pokud jsme teď nasazení projektu, se nám se získat prázdnou obrazovku. Je to způsobeno musíme sdělení rozhraní IDE používat naše scénáře a nastavit řadič zobrazení kořenové, která bude sloužit jako první zobrazení. Obvykle to lze provést prostřednictvím našich možnosti projektu jak je uvedeno výše. Ale v tomto příkladu jsme se dosáhne stejného výsledku v kódu přidáním následujícího **AppDelegate**:

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

Který je velké množství kódu, ale nejste obeznámeni jenom pár řádků. Nejprve, jsme naše scénáři s zaregistrovat **AppDelegate** předáním v názvu do scénáře **MainStoryboard**. V dalším kroku jsme sdělení aplikace k vytváření instancí řadič počáteční zobrazení scénáře voláním `InstantiateInitialViewController` naše scénáře, a nastaví tohoto řadiče zobrazení jako řadič zobrazení kořenové naší aplikace. Tato metoda určuje první obrazovka, která uživateli se zobrazí, a vytvoří novou instanci třídy Kontroleru zobrazení.

Všimněte si, v podokně řešení vytvořený IDE `MainViewcontroller.cs` třída a jeho `corresponding designer.cs` když jsme přidali název třídy vlastnosti odsazení v kroku 4. Tato třída vytvořit speciální konstruktor, který obsahuje základní třídy uvidíme:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Při vytváření scénáře pomocí návrháře, IDE, automaticky přidá [[zaregistrovat]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atribut v horní části `designer.cs` třídy a předat identifikátor řetězce, který je totožný s ID Storyboard zadané v předchozí krok. Tento odkaz jazyka C# na příslušné scény ve scénáři.

V určitém okamžiku budete chtít přidat existující třídy, která byla **není** vytvořené v návrháři. V takovém případě by zaregistrovat tuto třídu jako za normálních okolností:

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

Další informace o registraci třídy a metody, najdete v části [typ registrátora](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) dokumentaci.

Posledním krokem v této třídě je propojit tlačítko a přechodu na řadič růžový zobrazení. Jsme budete doložit `PinkViewController` scénáře; potom jsme bude program push segue s `PushViewController`, které jsou popsány v následující příklad kódu:

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

Spuštění aplikace vytváří 2 obrazovky aplikace:

![](images/finishedstoryboard.png "Ukázková aplikace spustit obrazovky")

## <a name="conditional-segues"></a>Podmíněné Segues

Často se přesouvat z jednoho řadiče zobrazení na další závislé na určité podmínky. Například pokud jsme byly provedení jednoduchého přihlašovací obrazovku by pouze chceme Přesun na další obrazovce *Pokud* měl ověřena uživatelské jméno a heslo.

V následujícím příkladu přidáme výše uvedené ukázkové pole pro heslo. Pouze uživatel bude mít přístup *PinkViewController* pokud jejich zadejte správné heslo, jinak chyba zobrazí.

Než začneme, proveďte kroky 1 – 8 výše. Tyto kroky jsme vytvořit naše storyboard, začít vytvářet naše uživatelské rozhraní a že naše aplikace delegáta řadič zobrazení, který chcete použít jako je RootViewController.

1. Nyní Pojďme vytvoříte naše uživatelské rozhraní a můžete přidat další zobrazení uvedené `MainViewController` aby vypadal jako je například na tomto snímku obrazovky:

    - UITextField
        - Název: PasswordTextField
        - Zástupný symbol: Zadejte tajné heslo
    - UILabel
        - Text: ' Chyba: nesprávný heslo. Nebude předáte!.
        - Barva: červená
        - Zarovnání: Center
        - Řádky: 2
        - 'Hidden' zaškrtávací políčko, zaškrtnuté 
        
    [![](images/passwordvc.png "Řádky Center")](images/passwordvc.png#lightbox)
    
2. Vytvoření Segue mezi tlačítko přejděte růžový a zobrazení řadiče pomocí Ctrl přetažením z *PinkButton* k *PinkViewController*a výběrem **Push** na myši nahoru . 

3. Klikněte na Segue a pojmenujte ho *identifikátor* `SegueToPink`:

    [![](images/namesegue.png "Klikněte na Segue a dejte mu identifikátor SegueToPink")](images/namesegue.png#lightbox)  
    

4. Nakonec přidejte následující metodu ShouldPerformSegue do `MainViewController` třídy:

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

V tomto kódu mají spojili jsme segueIdentifier na našem `SegueToPink` segue, abychom mohli otestovat pak podmínku; platné heslo v tomto případě. Pokud naše podmínka vrátí `true`, Segue provede a bude k dispozici `PinkViewController`. Pokud `false`, nebude předkládané nového řadiče zobrazení.

Tento postup jsme můžete použít na všechny Segue na tento řadič zobrazení kontrolou segueIdentifier argument pro metodu ShouldPerformSegue. V takovém případě budeme mít pouze jeden identifikátor Segue – `SegueToPink`.

Odkazovat na řešení Storyboards.Conditional v [ukázkových scénářů ruční](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) například práci.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Pomocí odkazů na scénáře

Odkaz Storyboard umožňuje využít scénáře návrh složitých a rozdělit na menší scénářů, které získat na něj odkazovat z původní, proto odebírání odebrání složitost a provádění výsledná jednotlivých scénářů snadnější k návrhu a Udržujte.

Kromě toho můžete zadat odkaz Storyboard _ukotvení_ na jiné scény v rámci stejné Storyboard nebo konkrétní scény na jiný.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Odkazování na externí scénáře

Pokud chcete přidat odkaz na externí Storyboard, postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **přidat** > **nový soubor...**   >  **iOS** > **Storyboard**. Zadejte **název** pro nové scénáře a klikněte na **nový** tlačítko:
    
    [![](images/ref01.png "Dialogové okno Nový soubor")](images/ref01.png#lightbox)
    
2. Návrh rozložení scény nové scénáře, jako za normálních okolností byste a uložte změny: 
    
    [![](images/ref02.png "Rozložení nové scény")](images/ref02.png#lightbox)
    
3. Otevřete scénáře, který budete přidávat odkaz na v iOS Designer.

4. Přetáhněte **scénáře odkaz** z **sada nástrojů** na návrhovou plochu: 
    
    [![](images/ref03.png "Odkaz na scénáře")](images/ref03.png#lightbox)
    
5. V **pomůcky** kartě **Explorer vlastnosti**, vyberte název **Storyboard** kterou jste vytvořili výše: 

    [![](images/ref04.png "Na kartě pomůcky")](images/ref04.png#lightbox)
    
6. Ovládací prvek, klikněte na Widget uživatelského rozhraní (např. tlačítka) na existující scény a vytvořit nové Segue k **Storyboard odkaz** kterou jste právě vytvořili: 

    [![](images/ref05.png "Vytváření segue")](images/ref05.png#lightbox) 
    
7. V místní nabídce vyberte **zobrazit** k dokončení Segue: 

    [![](images/ref06.png "Výběr zobrazení k dokončení Segue")](images/ref06.png#lightbox) 
    
8. Uložte změny do scénáře.

Zobrazí se při spuštění aplikace a uživatel klikne na prvek uživatelského rozhraní, které jste vytvořili Segue z počáteční řadiče zobrazení z externí Storyboard zadaný v odkaz na scénáře.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Odkazování na konkrétní scény v externí scénáře

Chcete-li přidat odkaz na konkrétní scény externí Storyboard (a ne počáteční View Controller), postupujte takto:

1. V **Průzkumníku**, dvakrát klikněte na externí Storyboard otevřete pro úpravy.

2. Přidejte nové scény a návrh jeho rozložení běžným způsobem: 

    [![](images/ref07.png "Nové rozložení scény")](images/ref07.png#lightbox)
    
3. V **pomůcky** kartě **Explorer vlastnosti**, zadejte **Storyboard ID** pro nové scény View Controller: 

    [![](images/ref08.png "Zadejte ID scénáře pro nový řadič zobrazení scény")](images/ref08.png#lightbox)
    
3. Otevřete scénáře, který budete přidávat odkaz na v iOS Designer.

4. Přetáhněte **scénáře odkaz** z **sada nástrojů** na návrhovou plochu: 

    [![](images/ref03.png "Odkaz na scénáře")](images/ref03.png#lightbox)
    
5. V **pomůcky** kartě **Explorer vlastnosti**, vyberte název **Storyboard** a **ID odkazu na** (Storyboard ID) z Scény, kterou jste vytvořili výše: 

    [![](images/ref09.png "Na kartě pomůcky ")](images/ref09.png#lightbox)
    
6. Ovládací prvek, klikněte na Widget uživatelského rozhraní (např. tlačítka) na existující scény a vytvořit nové Segue k **Storyboard odkaz** kterou jste právě vytvořili: 

    [![](images/ref10.png "Vytváření segue")](images/ref10.png#lightbox) 
    
7. V místní nabídce vyberte **zobrazit** k dokončení Segue: 

    [![](images/ref06.png "Výběr zobrazení k dokončení Segue")](images/ref06.png#lightbox) 
    
8. Uložte změny do scénáře.

Pokud je aplikace spustit a uživatel klikne na prvek uživatelského rozhraní, kterou jste vytvořili Segue z scény s danou **Storyboard ID** z externí Storyboard zadaný v Storyboard odkaz se zobrazí.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Odkazování na konkrétní scény ve scénáři, stejné

Pokud chcete přidat odkaz na konkrétní scény stejné Storyboard, postupujte takto:

1. V **Průzkumníku**, dvakrát klikněte na scénáři otevřete pro úpravy.

2. Přidejte nové scény a návrh jeho rozložení běžným způsobem: 

    [![](images/ref11.png "Nové rozložení scény")](images/ref11.png#lightbox)

3. V **pomůcky** kartě **Explorer vlastnosti**, zadejte **Storyboard ID** pro nové scény View Controller: 

    [![](images/ref12.png "Na kartě pomůcky")](images/ref12.png#lightbox)
    
3. Přetáhněte **scénáře odkaz** z **sada nástrojů** na návrhovou plochu: 

    [![](images/ref03.png "Odkaz na scénáře")](images/ref03.png#lightbox)
    
5. V **pomůcky** kartě **Explorer vlastnosti**, vyberte **ID odkazu** (Storyboard ID) scény, kterou jste vytvořili výše: 

    [![](images/ref13.png "Na kartě pomůcky")](images/ref13.png#lightbox)
    
6. Ovládací prvek, klikněte na Widget uživatelského rozhraní (např. tlačítka) na existující scény a vytvořit nové Segue k **Storyboard odkaz** kterou jste právě vytvořili: 

    [![](images/ref14.png "Vytváření segue")](images/ref14.png#lightbox) 
    
7. V místní nabídce vyberte **zobrazit** k dokončení Segue: 

    [![](images/ref06.png "Výběr zobrazení k dokončení Segue")](images/ref06.png#lightbox) 
    
8. Uložte změny do scénáře.

Pokud je aplikace spustit a uživatel klikne na prvek uživatelského rozhraní, kterou jste vytvořili Segue z scény s danou **Storyboard ID** ve stejném scénáři, zadaný v Storyboard odkaz se zobrazí.

## <a name="summary"></a>Souhrn

Tento článek představuje koncept scénářů a jak může být výhodné pro vývoj aplikací pro iOS. Popisuje scény, řadiče zobrazení, zobrazení a zobrazení hierarchie a jak jsou propojeny scény společně s různé typy Segues.  Je také prozkoumá konkretizujete řadiče zobrazení ručně z scénáře a vytváření podmíněného Segues.



## <a name="related-links"></a>Související odkazy

- [Ruční Storyboard (ukázka)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [Úvod do systému iOS návrháře](~/ios/user-interface/designer/introduction.md)
- [Převádění na scénářů](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [Odkaz na UIStoryboard – třída](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
