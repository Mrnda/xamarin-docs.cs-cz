---
title: Hello, tvOS úvodní příručce
description: Tento průvodce vás provede vytvoření první aplikace Xamarin.tvOS a jeho nástrojů pro vývoj. Také zavádí návrháře Xamarin, která zveřejňuje ovládacích prvků uživatelského rozhraní na kód a ukazuje, jak vytvářet, spustit a otestovat Xamarin.tvOS aplikace.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/02/2018
ms.openlocfilehash: 0adf6e326dd29db15b6bd90626f424b803dc0bc9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, tvOS úvodní příručce

_Tento průvodce vás provede vytvoření první aplikace Xamarin.tvOS a jeho nástrojů pro vývoj. Také zavádí návrháře Xamarin, která zveřejňuje ovládacích prvků uživatelského rozhraní na kód a ukazuje, jak vytvářet, spustit a otestovat Xamarin.tvOS aplikace._

Apple vydala 5. generování Apple TV, Apple TV 4 kB, který se spouští tvOS 11.

Platforma Apple TV je otevřená pro vývojáře, což jim umožní vytvářet bohaté, dokonalé aplikace a uvolněte je pomocí předdefinovaných televizoru Apple App Storu.

Pokud jste obeznámeni s vývojem pro Xamarin.iOS, by měl zjistit přechod tvOS docela jednoduché. Většina rozhraní API a funkce jsou stejné, ale nejsou k dispozici (například WebKit) mnoho běžných rozhraní API. Kromě toho práce pomocí vzdálené Siri představuje některé běžné problémy, které se nenacházejí v zařízení s iOS dotykovou obrazovku na základě návrhu.

Tento průvodce vám poskytne úvod do práce s tvOS v aplikaci Xamarin. Další informace o tvOS, najdete v tématu společnosti Apple [připravit Apple TV 4K](https://developer.apple.com/tvos/) dokumentaci.

## <a name="overview"></a>Přehled

Xamarin.tvOS umožňuje vyvíjet plně nativní Apple TV aplikace v C# a .NET pomocí stejné knihovny OS X a ovládací prvky rozhraní, které se používají při vývoji v *Swift* (nebo *jazyka Objective-C*) a *Xcode*.

Navíc vzhledem k tomu, že aplikace Xamarin.tvOS jsou napsané v C# a rozhraní .NET, běžné, back-end kód je možné sdílet s aplikace Xamarin.iOS, Xamarin.Android a Xamarin.Mac; všechny při doručování nativním prostředím na každou platformu.

Tento článek vás seznámí s klíčové koncepty jsou potřeba k vytvoření aplikace pro Apple TV pomocí návodem proces vytváření základní Xamarin.tvOS a Visual Studio **Hello, tvOS** aplikaci, která udává počet, kolikrát má tlačítka klepnutí na:

[![](hello-tvos-images/run05.png "Příklad aplikace spustit")](hello-tvos-images/run05.png#lightbox)

Zaměříme jsme následující koncepty:

-  **Visual Studio pro Mac** – Úvod do sady Visual Studio pro Mac a vytváření aplikací Xamarin.tvOS s ním.
-  **Anatomie aplikace Xamarin.tvOS** – co Xamarin.tvOS aplikace skládá z.
-  **Vytvoření uživatelského rozhraní** – postupy pomocí návrháře Xamarin pro iOS na vytvoření uživatelského rozhraní.
-  **Nasazení a testování** – spuštění a testování aplikace v simulátoru tvOS a na skutečné tvOS hardwaru.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Spuštění nové aplikace Xamarin.tvOS v sadě Visual Studio pro Mac

Jak jsme uvedli výše, vytvoříme Apple TV aplikace volá `Hello-tvOS` doplňuje jednoho tlačítka a popisek na hlavní obrazovku. Při kliknutí na tlačítko se zobrazí popisek počet časy, kdy bylo stisknuto.

Abyste mohli začít, budeme takto:

1. Spuštění sady Visual Studio pro Mac:

    [![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png#lightbox)
2. Klikněte na **nové řešení...**  odkaz v levém horním rohu obrazovky, otevřete **nový projekt** dialogové okno.
3. Vyberte **tvOS** > **aplikace** > **jediné zobrazení aplikace** a klikněte na **Další** tlačítko:

    [![](hello-tvos-images/setup02.png "Vyberte aplikaci, jednoho zobrazení")](hello-tvos-images/setup02.png#lightbox)
4. Zadejte `Hello, tvOS` pro **název aplikace**, zadejte vaše **identifikátor organizace** a klikněte na tlačítko **Další** tlačítko:

    [![](hello-tvos-images/setup04.png "Zadejte text Hello, tvOS")](hello-tvos-images/setup04.png#lightbox)
5. Zadejte `Hello_tvOS` pro **název projektu** a klikněte na **vytvořit** tlačítko:

    [![](hello-tvos-images/setup03.png "Enter HellotvOS")](hello-tvos-images/setup03.png#lightbox)

Visual Studio pro Mac se vytvořit novou aplikaci Xamarin.tvOS a zobrazit výchozí soubory, které se přidají do vaší aplikace řešení:

 [![](hello-tvos-images/project01.png "Výchozí zobrazení souborů")](hello-tvos-images/project01.png#lightbox)

Visual Studio pro Mac používá **řešení** a **projekty**, přesný stejným způsobem, který Visual Studio. Řešení je kontejner, který může obsahovat jeden nebo více projektů; projekty může zahrnovat aplikací, podpora knihovny, testovací aplikace atd. V takovém případě Visual Studio pro Mac řešení a projekt aplikace pro vás vytvořil.

Pokud chcete, můžete vytvořit jeden nebo více kód projektů knihovny, které obsahovat kód, který běžné, sdílené. Tyto knihovny projekty můžou spotřebovávají projekt aplikace nebo sdíleny s jinými projekty Xamarin.tvOS aplikace (nebo Xamarin.iOS, Xamarin.Android a Xamarin.Mac na základě typu kódu), stejně jako při byly vytváření standardní aplikace .NET.

## <a name="anatomy-of-a-xamarintvos-app"></a>Anatomie Xamarin.tvOS aplikace

Pokud jste obeznámeni s iOS programování, můžete si všimnout spoustu podobnosti sem. Ve skutečnosti tvOS 9 je podmnožinu iOS 9, takže spoustu koncepty bude překřížila sem.

Podívejme se na soubory v projektu:

-   `Main.cs` – Tato položka obsahuje hlavní vstupní bod aplikace. Když je aplikace spuštěná, tato položka obsahuje velmi první třídy a metody, která se spustí.
-   `AppDelegate.cs` – Tento soubor obsahuje třídu hlavní aplikace, která je zodpovědná za naslouchá událostem z operačního systému.
-   `Info.plist` – Tento soubor obsahuje vlastnosti aplikace, jako je například název aplikace, ikony, atd.
-   `ViewController.cs` – Toto je třída, která představuje hlavní okno a určuje životní cyklus ho.
-   `ViewController.designer.cs` – Tento soubor obsahuje vložení kód, který umožňuje integraci se službou hlavní obrazovky uživatelského rozhraní.
-  `Main.storyboard` – Uživatelské rozhraní pro hlavní okno. Tento soubor můžete vytvářené a udržované pomocí návrháře Xamarin pro iOS.

V následujících částech provedeme rychle zkontrolovat pomocí některé z těchto souborů. Jsme budete prozkoumat je podrobněji později, ale je vhodné pochopit jejich základy teď.

### <a name="maincs"></a>Main.cs

`Main.cs` Soubor obsahuje statický `Main` metodu, která vytvoří novou instanci aplikace Xamarin.tvOS a předá název třídy, která bude zpracovávat události operačního systému, v našem případě je `AppDelegate` třídy:

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Soubor obsahuje naše `AppDelegate` třídy, která je odpovědná za vytváření naše okno a naslouchá událostem operačního systému:

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

Tento kód je pravděpodobně obeznámeni, pokud jste vytvořili aplikaci pro systém iOS před, ale je poměrně jednoduché. Podívejme se na důležité řádky.

Nejprve Podívejme se na deklarace proměnné na úrovni třídy:

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window` Vlastnost poskytuje přístup k hlavní okno. tvOS používá, která se označuje jako *Model View Controller* vzor (MVC). Obecně platí pro každý okno, které vytvoříte (a pro mnoho dalších položek v rámci systému windows), není kontroler, který je zodpovědný za okna životní cyklus, například zobrazení, přidání nové zobrazení (ovládací prvky) Chcete-li ji, atd.

Potom máme `FinishedLaunching` metoda. Tato metoda se spouští po po vytvoření instance aplikace a je odpovědná za ve skutečnosti vytvoření okna aplikace a od proces zobrazení zobrazení v ní. Protože naše aplikace používá k definování jeho uživatelském rozhraní scénáře, není žádný další kód potřeba sem.

Existuje mnoho dalších metod, které jsou uvedeny v šabloně, jako `DidEnterBackground` a `WillEnterForeground`. Ty lze bezpečně odebrat Pokud události aplikace nejsou používány ve vaší aplikaci.

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController` Třída je naše hlavní okno řadiče. To znamená, že je zodpovědná za životního cyklu hlavního okna. Vytvoříme prozkoumat to později podrobně, teď právě Podívejme se rychlé na to:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Návrháře soubor pro třídu hlavní okno je teď prázdný, ale jej bude automaticky vyplňovat Visual Studio pro Mac jako vytvoříme naše uživatelské rozhraní s iOS Designer:

```csharp
using Foundation;

namespace HellotvOS
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Jsme nejsou obvykle problémem s návrháře souborů, protože jste právě automaticky spravovány Visual Studio pro Mac a stačí zadat kód požadavků vložení, který umožňuje přístup k ovládacím prvkům nemůžeme přidat do libovolného okna nebo zobrazit v naší aplikaci.

Teď, když jsme vytvořili vaší aplikace Xamarin.tvOS a budeme mít základní znalosti o jeho komponenty, podíváme se na vytvoření uživatelského rozhraní.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>Vytvoření uživatelského rozhraní

Nemáte k vytvoření uživatelského rozhraní pro vaši aplikaci Xamarin.tvOS pomocí Xamarin Designer pro iOS, uživatelského rozhraní lze vytvořit přímo z kódu jazyka C#, ale který je nad rámec tohoto článku. Z důvodu zjednodušení budeme vytvářet naše uživatelské rozhraní ve zbývající části tohoto kurzu používat iOS Designer.

Pokud chcete začít vytvářet uživatelské rozhraní, můžeme dvakrát klikněte na `Main.storyboard` v soubor **Průzkumníku řešení** otevřete pro úpravy v iOS Designer:

[![](hello-tvos-images/designer01.png "Main.storyboard soubor v Průzkumníku řešení")](hello-tvos-images/designer01.png#lightbox)

To by měl spusťte návrháře a vypadat následovně:

[![](hello-tvos-images/designer02.png "Návrháře")](hello-tvos-images/designer02.png#lightbox)

Další informace o iOS Designer a jak to funguje, najdete v části [Úvod do návrháře Xamarin pro iOS](~/ios/user-interface/designer/introduction.md) průvodce.

Nyní jsme můžete spustit, přidání ovládacích prvků na návrhovou plochu naše Xamarin.tvOS aplikace.

Postupujte takto:

1. Vyhledejte **sada nástrojů**, což by mělo být napravo od návrhové plochy:

    [![](hello-tvos-images/designer03.png "V panelu nástrojů")](hello-tvos-images/designer03.png#lightbox)

    Pokud zde ji nelze najít, přejděte do **zobrazení > dotyková zařízení > Sada nástrojů** k jeho zobrazení.
2. Přetáhněte **popisek** z **sada nástrojů** na návrhovou plochu:

    [![](hello-tvos-images/designer04.png "Přetáhněte štítek z panelu nástrojů")](hello-tvos-images/designer04.png#lightbox)
3. Klikněte na **název** vlastnost v **vlastnost pad** a změňte název na tlačítko pro `Hello, tvOS` a nastavte **velikost písma** do 128:

    [![](hello-tvos-images/designer05.png "Nastavený nadpis můžete Hello, tvOS a velikost písma na 128")](hello-tvos-images/designer05.png#lightbox)
4. Změnit velikost popisku tak, aby všechna slova jsou viditelné a umístěte ji na střed v horní části okna:

    [![](hello-tvos-images/designer06.png "Změní velikost a center popisek")](hello-tvos-images/designer06.png#lightbox)
5. Popisek teď muset být omezené na jeho pracovní pozici, tak, aby se tak, jak má. bez ohledu na velikost obrazovky. K tomu, klikněte na štítek, dokud *ve tvaru T popisovač* se zobrazí:

    [![](hello-tvos-images/designer07.png "Popisovač ve tvaru T")](hello-tvos-images/designer07.png#lightbox)
6. Chcete-li omezit popisek vodorovně, vyberte druhou mocninu center a přetáhněte jej do svisle přerušovanou čáru:

    [![](hello-tvos-images/designer08.png "Vyberte druhou mocninu center")](hello-tvos-images/designer08zoom.png#lightbox)

     Popisek doporučujeme zapnout oranžová.
7. Vyberte popisovač T v horní části popisek a přetáhněte ji na horní okraj okna:

    [![](hello-tvos-images/designer09.png "Přetažením úchytu na horní okraj okna")](hello-tvos-images/designer09.png#lightbox)
8. Klikněte na tlačítko šířku a potom výška *kost popisovač* jak je uvedeno dále:

    [![](hello-tvos-images/designer10.png "Šířka a výška úplně dělit obslužných rutin")](hello-tvos-images/designer10.png#lightbox)

     Při každém *kost popisovač* je klikli, vyberte šířku i výšku v uvedeném pořadí nastavit pevné dimenze.
9. Po dokončení vaše omezení by měl vypadat podobné těm, na kartě Rozložení panelu Vlastnosti pro:

    [![](hello-tvos-images/designer11.png "Příklad omezení")](hello-tvos-images/designer11.png#lightbox)
8. Přetáhněte **tlačítko** z **sada nástrojů** a umístěte jej do popisku.
9. Klikněte na **název** vlastnost **vlastnost pad** a změňte název na tlačítko pro `Click Me`:

    [![](hello-tvos-images/designer12.png "Změnit text tlačítka klikněte na tlačítko vám.")](hello-tvos-images/designer12.png#lightbox)
10. Opakujte kroky 5 až 8 omezit tlačítka v okně tvOS výše. Však několik způsobů T-popisovač do horní části okna (jako kroku #7), přetáhněte jej do dolní části štítku:

    [![](hello-tvos-images/designer14.png "Omezit na tlačítko")](hello-tvos-images/designer14.png#lightbox)
11. Přetáhněte jiný popisek pod tlačítko, velikost, že bude stejnou délku jako první popisek a sadu jeho **zarovnání** k **Center**:

    [![](hello-tvos-images/designer15.png "Přetáhněte jiný popisek pod tlačítko, velikost ho mít stejnou šířku jako první popisek a nastavit jeho zarovnání na Center")](hello-tvos-images/designer15.png#lightbox)
12. Jako první popisek a tlačítko nastavte tento popisek center a připnout do umístění a velikost:

    [![](hello-tvos-images/designer16.png "PIN kód popisek do umístění a velikost")](hello-tvos-images/designer16.png#lightbox)
13. Uloží změny do uživatelského rozhraní.

Jako byly změny velikosti a přesouvání ovládacích prvků kolem, měli jste si všimli, že návrháře poskytuje užitečné snap pomocné parametry, které jsou založeny na [Apple TV Human Interface Guidelines](https://developer.apple.com/tvos/human-interface-guidelines/). Tyto pokyny vám pomůže vytvořit vysoce kvalitního aplikace, které budou mít známých vzhled a chování pro Apple TV uživatele.

Pokud se podíváte **Osnova dokumentu** část, Všimněte si, jak jsou uvedeny rozložení a hierarchii elementů, které tvoří naše uživatelské rozhraní:

[![](hello-tvos-images/designer17.png "V části Osnova dokumentu")](hello-tvos-images/designer17.png#lightbox)

Tady můžete vybrat položky, které chcete upravit nebo přetáhněte v případě potřeby změňte pořadí prvky uživatelského rozhraní. Například pokud prvku uživatelského rozhraní se vztahuje jiný element, vám může přetáhněte jej do dolní části seznamu, chcete-li nejvyšší položku v okně.

Teď, když máme naše uživatelské rozhraní vytvořen, musíme vystavit položky uživatelského rozhraní, aby mohli Xamarin.tvOS přístup a s nimi pracovat v kódu jazyka C#.

### <a name="accessing-the-controls-in-the-code-behind"></a>Přístup k ovládacím prvkům v kódu

Pro přístup k ovládacích prvků, které jste přidali v Návrháři iOS z kódu dvěma způsoby:

* Vytvoření obslužné rutiny události v ovládacím prvku.
* Pojmenujete ovládacího prvku, tak, aby jsme ho novější odkazovat.

Pokud některý z těchto přidají, třídu v rámci `ViewController.designer.cs` budou aktualizovány tak, aby odrážely změny. To vám umožní pak přístup k ovládacím prvkům v Kontroleru zobrazení.

### <a name="creating-an-event-handler"></a>Vytvoření obslužné rutiny událostí

V této ukázkové aplikaci, po kliknutí na tlačítko chceme _něco_ provést, takže obslužné rutiny události musí být přidán do konkrétní události na tlačítko. Pokud chcete nastavit tuto možnost, postupujte takto:

1. V Xamarin iOS Designer vyberte tlačítko řadiče zobrazení.
2. V panelu pro vlastnosti, vyberte **události** karty:

    [![](hello-tvos-images/event1.png "Na kartě události")](hello-tvos-images/event1.png#lightbox)
3. Vyhledejte události TouchUpInside a pojmenujte ho obslužné rutiny události s názvem `Clicked`:

    [![](hello-tvos-images/event2.png "Událost TouchUpInside")](hello-tvos-images/event2.png#lightbox)
4. Po stisknutí klávesy **Enter**, **ViewController**.cs soubor se otevře, které naznačují, umístění pro vaší obslužné rutiny událostí v kódu. Chcete-li nastavit umístění, použijte klávesy se šipkami na klávesnici:

    [![](hello-tvos-images/event3.png "Nastavení umístění")](hello-tvos-images/event3.png#lightbox)
5. Tím se vytvoří částečné metodu, jak je uvedeno níže:

    [![](hello-tvos-images/event4.png "Částečné metody")](hello-tvos-images/event4.png#lightbox)

Nyní připraveni začít přidávat kód, který povolí tlačítko pro funkce.

### <a name="naming-a-control"></a>Názvy ovládacího prvku

Při kliknutí na tlačítko popisek by měl aktualizovat na základě počtu kliknutí. K tomuto účelu jsme se potřebujete přístup k popisku v kódu. K tomu je potřeba ho pojmenujete. Postupujte takto:

1. Otevřete scénáři a vyberte štítek, který v dolní části řadiče zobrazení.
2. V panelu pro vlastnosti, vyberte **pomůcky** karty:

    [![](hello-tvos-images/name1.png "Vyberte kartu pomůcky")](hello-tvos-images/name1.png#lightbox)
3. V části **Identity > název**, přidejte `ClickedLabel`:

    [![](hello-tvos-images/name2.png "Nastavit ClickedLabel")](hello-tvos-images/name2.png#lightbox)

Nyní připraveni spusťte aktualizaci popisku!

### <a name="how-controls-are-accessed"></a>Jak získat přístup z ovládacích prvků

Pokud jste vybrali `ViewController.designer.cs` v **Průzkumníku řešení** budete moci zobrazit jak `ClickedLabel` popisek a `Clicked` namapované na obslužné rutiny události **výstupu** a  **Akce** v jazyce C#:

[![](hello-tvos-images/accesscontrol.png "Výstupy a akcí")](hello-tvos-images/accesscontrol.png#lightbox)

Může také zjistíte, že `ViewController.designer.cs` je konkrétní třídu, takže nemá Visual Studio pro Mac k úpravě `ViewController.cs` který by přepsala veškeré změny, které jsme provedli pro třídu.

Vystavení prvky uživatelského rozhraní tímto způsobem můžete přistupovat k nim v Kontroleru zobrazení.

Obvykle se nikdy musíte otevřít `ViewController.designer.cs` sami, ho se netýkají pro vzdělávací účely.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>Psaní kódu

Naše uživatelské rozhraní, vytvořit a jeho prvky uživatelského rozhraní, které jsou zpřístupněny kódu prostřednictvím **výstupy** a **akce**, jsme připraveni nakonec napsat kód umožnit funkce programu.

V naší aplikaci pokaždé, když se po kliknutí na první tlačítko, vytvoříme naše štítek, který chcete zobrazit kolikrát kliknutí na tlačítko Aktualizovat. K tomu je potřeba otevřít `ViewController.cs` soubor pro úpravy poklepáním v **řešení Pad**:

[![](hello-tvos-images/code01.png "Odsazení řešení")](hello-tvos-images/code01.png#lightbox)

Nejprve musíme vytvořit proměnnou úrovni třídy v našem `ViewController` třída sledovat počet kliknutí, které došlo. Upravit definici třídy a nastavit jej vypadat následovně:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Další ve stejné třídě (`ViewController`), musíme přepsat **ViewDidLoad** metoda a přidejte kód, který nastavit počáteční zprávu pro našeho štítku:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Je potřeba použít `ViewDidLoad `, místo jiné metody, jako `Initialize`, protože `ViewDidLoad ` nazývá *po* má načíst a vytvoření instance uživatelské rozhraní z operačního systému `.storyboard` souboru. Pokud jsme se pokusili získat přístup ovládací prvek popisek před `.storyboard` soubor má plně načíst a vytvořit instance, by se nám získat `NullReferenceException` chyba protože ovládací prvek popisek by dosud vytvořena.

Dále je potřeba přidat kód reagovat na uživatel klepnutím na tlačítko. Přidejte následující částečné třídy k jsme vytvořili:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Tento kód bude volán vždy, když uživatel klikne na našem tlačítko.

S vše na místě můžeme nyní připraveni k sestavení a otestování naší aplikace Xamarin.tvOS.

## <a name="testing-the-application"></a>Testování aplikace

Je čas pro sestavení a spuštění aplikace a ujistěte se, že pracuje podle očekávání. Jsme můžete sestavit a spustit v jediném kroku, nebo jsme sestavení bez jeho spuštění.

Vždy, když jsme sestavit aplikaci, jsme můžete zvolit, jaký druh sestavení chceme:

-   **Ladění** – zkompilován sestavení ladicí verze "soubor (aplikace) s další metadata, která umožňuje ladění, co se děje, když aplikace běží.
-   **Verze** – sestavení pro vydání vytvoří také '' souboru, ale neobsahuje informace o ladění, takže je menší a rychleji spustí.  

Můžete vybrat typ sestavení z **konfigurace selektor** v levém horním rohu sady Visual Studio pro Mac obrazovky:

[![](hello-tvos-images/run01.png "Vyberte typ sestavení")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>Sestavení aplikace

V našem případě jsme právě má sestavení ladicí verze, takže umožňuje Ujistěte se, že **ladění** je vybrána. Vytvořme naší aplikaci nejprve stisknutím **⌘B**, nebo z **sestavení** nabídce zvolte **sestavení všechny**.

Pokud nebyly žádné chyby, zobrazí se **sestavení úspěšné** zprávu v sadě Visual Studio pro Mac na stavovém řádku. Pokud došlo k chybám, projděte si projekt a ujistěte se, že jste udělali kroky správně. Spustit tak, že zkontrolujete, jestli váš kód (i v prostředí Xcode a v sadě Visual Studio pro Mac) odpovídá kód v tomto kurzu.

### <a name="running-the-application"></a>Spuštění aplikace

Ke spuštění aplikace, máme tři možnosti:

-  Stiskněte klávesu **⌘ + Enter**.
-  Z **spustit** nabídce zvolte **ladění**.
-  Klikněte na tlačítko **přehrání** tlačítko v sadě Visual Studio pro Mac panelu nástrojů (právě vyšší **Průzkumníku řešení**).

Aplikace bude sestavení (pokud nebyl již vytvořen), spustí spuštění v režimu ladění, tvOS simulátoru a bude spustit a zobrazí rozhraní hlavní okno aplikace:

[![Domovskou obrazovku ukázkové aplikace](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

Z **hardwaru** nabídky vyberte možnost **zobrazit Apple TV na vzdálené** , můžete řídit simulátoru.

[![](hello-tvos-images/run04.png "Vyberte zobrazení Apple TV vzdálené")](hello-tvos-images/run04.png#lightbox)

Pokud kliknete na tlačítko několikrát popisek pomocí vzdáleného simulátoru, je třeba aktualizovat počet:

[![](hello-tvos-images/run05.png "Popisek aktualizovaný počet")](hello-tvos-images/run05.png#lightbox)

Blahopřejeme! Jsme zahrnutých spoustu základů zde, ale pokud jste provedli v tomto kurzu od začátku do konce, teď byste měli mít dobré porozumění součásti aplikace Xamarin.tvOS, jakož i nástroje používané k jejich vytvoření.

## <a name="where-to-next"></a>Kde další?

Vývoj Apple TV aplikace uvede několik vyzve kvůli odpojení mezi uživatelem a rozhraní (je v místnosti není v dolním uživatele) a omezení tvOS umístí na velikost aplikace a úložiště.

V důsledku toho důrazně doporučujeme, aby vaše čtení následující dokumenty před přechod do návrhu Xamarin.tvOS aplikace:

- [Úvod do tvOS 9](~/ios/tvos/platform/tvos9.md) – Tento článek představuje všechny nové a změněné rozhraní API a funkce dostupné v tvOS 9 pro vývojáře Xamarin.tvOS.
- [Práce s navigační a výběr](~/ios/tvos/app-fundamentals/navigation-focus.md) – uživatele vaší aplikace Xamarin.tvOS nebude interakci s jeho rozhraní přímo jako s iOS kde jejich klepněte na bitové kopie na displeji zařízení, ale nepřímo z napříč místnosti pomocí vzdáleného Siri. Tento článek se zabývá koncept fokus a jak se používají pro zpracování navigace v uživatelském rozhraní aplikace Xamarin.tvOS.
- [Vzdálené Siri a Bluetooth řadiče](~/ios/tvos/platform/remote-bluetooth.md) – hlavní způsobu, jakým uživatelé budou interakci s Apple TV a Xamarin.tvOS aplikací, je prostřednictvím zahrnuté vzdálené Siri. Pokud je vaše aplikace hry, můžete volitelně vytvořit v podpora pro 3. stran provedené pro iOS (MFI s Připojením) Bluetooth herní zařízení ve vaší aplikaci také. Tento článek se zabývá podpora nové řadiče herní Siri Remote a Bluetooth v Xamarin.tvOS aplikací.
- [Prostředky a úložiště dat](~/ios/tvos/app-fundamentals/resources-data-storage.md) – na rozdíl od zařízení iOS, nové Apple TV neposkytuje trvalé, místní úložiště pro tvOS aplikace. Výsledkem je Pokud vaše aplikace Xamarin.tvOS musí zachovat informace (například uživatelské předvolby) musí uložení a načtení dat ze serveru služby iCloud. Tento článek se zabývá práci s prostředky a úložiště dat v aplikaci Xamarin.tvOS.
- [Práce s ikony a bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md) – vytváření poutavé ikony a obrazů jsou důležitou součástí vývoj dokonalé uživatelské prostředí pro aplikace pro Apple TV. Tato příručka popisuje kroky potřebné k vytvoření a zahrnutí nezbytné grafické prostředky pro vaše aplikace Xamarin.tvOS.
- [Uživatelské rozhraní](~/ios/tvos/user-interface/index.md) – pokrytí obecné uživatelské rozhraní (UX), včetně ovládacích prvků uživatelského rozhraní (UI), použijte rozhraní tvůrce na Xcode a zásady designu UX při práci s Xamarin.tvOS.
- [Nasazení a testování](~/ios/tvos/deploy-test/index.md) – Tato část obsahuje témata týkající se používá k testování aplikace a její distribuce. Témata v tomto poli mezi patří například nástroje používané pro ladění, nasazení a testery, jak publikovat aplikace pro Apple TV App Store.

Pokud narazíte na potíže s práce s Xamarin.tvOS, najdete v tématu naše [Poradce při potížích s](~/ios/tvos/troubleshooting.md) dokumentace pro seznam na vědět, problémy a jejich řešení.

## <a name="summary"></a>Souhrn

Tento článek poskytuje rychlý start pro vývoj aplikací pro tvOS pomocí sady Visual Studio pro Mac vytvořením jednoduchého Hello, tvOS aplikace. Základní informace o zařízení tvOS zřizování, vytváření rozhraní, pro tvOS kódování a testování v simulátoru tvOS o něm zmínka.


## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Vytváření aplikací pro tvOS s Xamarinem (video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
