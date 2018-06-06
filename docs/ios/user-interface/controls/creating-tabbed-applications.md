---
title: Karta řádky a posuvníku řadičů v Xamarin.iOS
description: Tento dokument popisuje iOS karta panelu řadiče a postup jejich používání s Xamarin.iOS. Ukazuje, jak nastavit UITabBarController, práce s obrázky, nastavení oznámení "BADGE" hodnoty, práce s událostmi a další.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: d8b096774e60ec0e0b69e109fa5da53c25e66d25
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789755"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Karta řádky a posuvníku řadičů v Xamarin.iOS

Na kartách aplikace se používají v iOS pro podporu uživatelského rozhraní, kde může získat přístup více obrazovek v žádné konkrétní pořadí. Prostřednictvím `UITabBarController` třídy aplikace můžete snadno zahrnují podporu pro tyto scénáře s více obrazovky. `UITabBarController` má na starosti správu více obrazovky, umožňuje vývojáři aplikace umožňuje zaměřit se na podrobnosti o každém obrazovky.

Obvykle jsou záložkách aplikace vytvořené s nástroji `UITabBarController` probíhá `RootViewController` hlavního okna. Nicméně s bit další kód, záložkách aplikace lze také po sobě na některé další úvodní obrazovka, jako je například scénář, kde aplikace nejprve uvede přihlašovací obrazovku, za nímž následuje rozhraní s kartami.

Podíváme pomocí karty zde projdete návod jednoduchou aplikaci. Potom podíváme jak pracovat s kartami v jinou hodnotu než `RootViewController` scénář.

## <a name="introducing-uitabbarcontroller"></a>Představení UITabBarController

`UITabBarController` Podporuje – vývoj aplikací záložkami podle následující:

-  Umožňuje několika řadičů, který se má přidat k němu.
-  Poskytnutí záložkách uživatelské rozhraní, pomocí `UITabBar` třída, chcete, aby uživatel přepínat mezi řadiče a jejich zobrazení. 


Řadiče jsou přidány do `UITabBarController` přes jeho `ViewControllers` vlastnost, která je `UIViewController` pole. `UITabBarController` Samotné zpracovává načítání správné řadiče a zobrazíte jeho zobrazení na základě vybraná karta.

Karty jsou instancemi třídy `UITabBarItem` třídy, která jsou součástí `UITabBar` instance. Každý `UITabBar` instance je přístupná prostřednictvím `TabBarItem` vlastnost řadiče v každé kartě.

Chcete-li získat představu o tom, jak pracovat `UITabBarController`, projděme vytváření jednoduchou aplikaci, která používá jednu.

## <a name="tabbed-application-walkthrough"></a>Návod na kartách aplikace

V tomto návodu vytvoříme vytvořit následující aplikace:

[![](creating-tabbed-applications-images/00-app.png "Ukázkové aplikace s kartami")](creating-tabbed-applications-images/00-app.png#lightbox)

I když už není k dispozici v sadě Visual Studio pro Mac, v tomto příkladu šablonu záložkách aplikace, vytvoříme z prázdného projektu k získání lepší pochopení, jak je vytvořený aplikace fungovat.

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>Vytvoření aplikace

Začněme tím, že vytvoříte novou aplikaci.

Vyberte **soubor > Nový > řešení** položky nabídky v sadě Visual Studio pro Mac a vyberte **iOS > aplikace > prázdný projekt** šablony, název projektu `TabbedApplication`, jak je uvedeno níže:

[![](creating-tabbed-applications-images/newsolution1.png "Vyberte šablonu, prázdný projekt")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Název projektu TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)



### <a name="adding-the-uitabbarcontroller"></a>Přidávání UITabBarController

Dál přidejte třídu prázdný výběrem **soubor > Nový soubor** a výběr **Obecné: prázdné třídy** šablony. Název souboru `TabController` jak je uvedeno níže:

[![](creating-tabbed-applications-images/02-newclass.png "Přidání třídy TabController")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController` Třída bude obsahovat implementace `UITabBarController` který budou spravovat pole `UIViewControllers`. Když uživatel vybere na kartě `UITabBarController` se postará o prezentací zobrazení pro řadič odpovídající zobrazení.

K implementaci `UITabBarController` budeme muset udělat následující:

1.  Nastavit základní třídu `TabController` k `UITabBarController` . 
1.  Vytvoření `UIViewController` instance, které chcete přidat do `TabController` . 
1.  Přidat `UIViewController` instance do pole přiřazené `ViewControllers` vlastnost `TabController` . 


Přidejte následující kód, který `TabController` třída k dosažení těchto kroků:

```csharp
using System;
using UIKit;

namespace TabbedApplication {
        public class TabController : UITabBarController {

                UIViewController tab1, tab2, tab3;

                public TabController ()
                {
                        tab1 = new UIViewController();
                        tab1.Title = "Green";
                        tab1.View.BackgroundColor = UIColor.Green;

                        tab2 = new UIViewController();
                        tab2.Title = "Orange";
                        tab2.View.BackgroundColor = UIColor.Orange;

                        tab3 = new UIViewController();
                        tab3.Title = "Red";
                        tab3.View.BackgroundColor = UIColor.Red;

                        var tabs = new UIViewController[] {
                                tab1, tab2, tab3
                        };

                        ViewControllers = tabs;
                }
        }
}
```

Všimněte si, že se pro každou `UIViewController` instance, nastaví `Title` vlastnost `UIViewController`. Když jsou přidány na řadiče do `UITabBarController`, `UITabBarController` , bude číst `Title` pro každý kontroler a zobrazit na kartě přidružené popisek, jak je uvedeno níže:

[![](creating-tabbed-applications-images/00-app.png "Ukázková aplikace spustit")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Nastavení TabController jako RootViewController

Pořadí, aby řadiče jsou umístěny v karty odpovídá pořadí, budou přidány do `ViewControllers` pole.

Chcete-li získat `UITabController` načíst jako první obrazovce, potřebujeme, aby okna `RootViewController`, jak je znázorněno v následujícím kódu pro `AppDelegate`:

```csharp
[Register ("AppDelegate")]
        public partial class AppDelegate : UIApplicationDelegate
        {
                UIWindow window;
                TabController tabController;

                public override bool FinishedLaunching (UIApplication app, NSDictionary options)
                {
                        window = new UIWindow (UIScreen.MainScreen.Bounds);

                        var tabController = new TabController ();
                        window.RootViewController = tabController;

                        window.MakeKeyAndVisible ();
            
                        return true;
                }
        }
```

Pokud jsme aplikaci teď spustit `UITabBarController` načte první kartě ve výchozím nastavení zaškrtnuto. Výběrem libovolné ostatní karty má za následek přidruženého kontroleru zobrazení se předložený `UITabBarController,` jak je uvedeno níže, kde má koncový uživatel vybrané kartě druhý:

[![](creating-tabbed-applications-images/03-secondtab.png "Druhý karta ukazuje")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>Úprava TabBarItems

Teď, když máme spuštěný kartě aplikace, Pojďme upravit `TabBarItem` změnit bitové kopie a text, který se zobrazí, jakož i oznámení "BADGE" přidat do jedné z karet.

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>Nastavení systému položky

Nejprve umožňuje nastavit na první kartě použít položku systému. V konstruktoru `TabController`, odeberte řádek, který nastaví kontroleru `Title` pro `tab1` instance a nahraďte ji následujícím kódem nastavení kontroleru `TabBarItem` vlastnost:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

Při vytváření `UITabBarItem` pomocí `UITabBarSystemItem`, název a image jsou poskytovány automaticky iOS, jak je vidět na snímku obrazovky níže zobrazuje **oblíbených položek** ikonu a title na první kartě:

 ![](creating-tabbed-applications-images/04a-tabimage.png "Na první kartě s ikonou s hvězdičkou")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>Nastavení nadpisu a bitové kopie

Kromě pomocí položky systém, název a bitovou kopii `UITabBarItem` může být nastaven na vlastní hodnoty. Můžete například změnit kód, který nastaví `TabBarItem` vlastnost s názvem řadiče `tab2` následujícím způsobem:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

Ve výše uvedeném kódu předpokládá image s názvem `second.png` byla přidána do kořenového adresáře projektu v sadě Visual Studio for Mac. Jsme přidali ve skutečnosti tři bitové kopie do našich projektu, tak, aby pokrývalo řešení všech zařízení, jak je uvedeno níže:

 [![](creating-tabbed-applications-images/tabbedimages7new.png "Bitové kopie, přidány do projektu")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

Obrázek karty musí být 30 × 30 png s průhlednost pro normální překlad IP adres 60 x 60 pro s vysokým rozlišením a 90 x 90 pro iPhone 6 Plus řešení. V našem kódu, musíme pouze načíst soubor s názvem `second.png` a iOS se automaticky načte vysokým rozlišením, jeden v zařízeních s sítnice zobrazení. Další informace najdete v [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) příručky. Ve výchozím nastavení jsou položky panelu karta šedá s blue odstín při výběru.

**Poznámka:**

Výše uvedené Image také lze přidat do **prostředky** adresáři, který je speciální adresář, jejichž obsah se automaticky zkopírují do kořenového adresáře aplikace sady:

[![](creating-tabbed-applications-images/tabbedapplication8.png "Bitové kopie jako prostředky")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

Kromě toho když nastaví `Title` vlastnost přímo na `TabBarItem`, ho by se mělo přepsat žádnou hodnotu pro nastavit `Title` na samotný řadič.

Při spuštění aplikace nyní jsme druhý karta ukazuje naše vlastní název a bitové kopie jak je uvedeno níže:

[![](creating-tabbed-applications-images/05-customtab.png "Na druhé kartu s čtvercovou ikonu")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>Nastavení hodnoty oznámení "BADGE"

Na kartě lze také zobrazit oznámení "BADGE". Například přidejte následující řádek kódu na kartě třetí nastavit oznámení "BADGE":

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Spuštění to výsledkem red štítek s řetězec "Hi" v levém horním rohu na kartě, jak je uvedeno níže:

[![](creating-tabbed-applications-images/06-badge.png "Na druhé kartu s HIS použití oznámení")](creating-tabbed-applications-images/06-badge.png#lightbox)

Oznámení "BADGE" se často používá k zobrazení se číslo označením nepřečtená, nové položky. Chcete-li odebrat oznámení, nastavte `BadgeValue` na hodnotu null, jak je uvedeno níže:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Karty v jiných RootViewController scénáře

V předchozím příkladu jsme vám ukázal, jak pracovat `UITabBarController` když se nachází `RootViewController` okna. V tomto příkladu vyzkoušíme použití `UITabBarController` když není `RootViewController` a zobrazit, jak je vytvořena pomocí scénářů.

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>Příklad úvodní obrazovka

V tomto scénáři úvodní obrazovka načte z kontroler, který není `UITabBarController`. Když uživatel pracuje s obrazovky klepnutím na tlačítko, do budou načteny stejného řadiče zobrazení `UITabBarController`, který se předloží uživateli. Následující snímek obrazovky zobrazuje tok aplikace:

[![](creating-tabbed-applications-images/inital-screen-application.png "Tento snímek obrazovky ukazuje toku aplikace")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Začněme nové aplikace v tomto příkladu. Znovu, použijeme **iPhone > aplikace > prázdný projekt (C#)** šablony, tentokrát pojmenování projektu `InitialScreenDemo`.


V tomto příkladu budeme potřebovat scénáře pro uložení naše řadiče zobrazení. Chcete-li přidat scénáře:

- Klikněte pravým tlačítkem na název projektu a vyberte **Přidat > Nový soubor**.

- Když se zobrazí dialogové okno Nový soubor, přejděte na **iOS > prázdná iPhone Storyboard**.

Umožňuje volání této nové scénáře **MainStoryboard** , jak je uvedeno dále: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Přidejte soubor MainStoryboard do projektu")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Existuje několik důležitých kroků, které při přidávání scénáře do souboru dříve scénáře, které jsou popsané v pamatujte [Úvod do scénářů](~/ios/user-interface/storyboards/index.md) průvodce. Jsou to:

 
1. Přidání názvu Storyboard k **hlavní rozhraní** části `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Nastavte hlavní rozhraní na MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. Ve vašem `App Delegate`, přepsat metodu okno s následujícím kódem:

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

Budeme potřebovat tři řadiče zobrazení v tomto příkladu. Jeden, s názvem `ViewController1`, budou použity jako počáteční Kontroleru zobrazení a na první kartě. Další dvě, s názvem `ViewController2` a `ViewController3`, který se použije na kartách druhý a třetí v uvedeném pořadí.

Otevřete návrháře dvojím kliknutím na soubor MainStoryboard.storyboard a přetáhněte tři řadiče zobrazení na návrhovou plochu. Každý z těchto zobrazení řadičů do mají své vlastní třídu odpovídající se jménem uvedeným výše, chceme Ano, v části **Identity > třída**, zadejte jeho název, jak ukazuje následující snímek obrazovky:

[![](creating-tabbed-applications-images/class-name.png "Nastavit třídy na ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio pro Mac automaticky vygeneruje třídy a návrháře soubory potřebné, to se zobrazí v panelu pro řešení, jak je uvedeno dále:

[![](creating-tabbed-applications-images/solution-pad2.png "Automaticky generované soubory v projektu")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>Vytvoření uživatelského rozhraní

V dalším kroku vytvoříme jednoduché uživatelské rozhraní pro každou z ViewController zobrazení, pomocí Xamarin iOS Designer.

Chceme přetáhněte `Label` a `Button` do ViewController1 z **sada nástrojů** na pravé straně. Další použijeme Pad vlastnosti upravit název a text ovládacích prvků takto:

-  **Popisek** : `Text`  =  **jeden**
-  **Tlačítko** : `Title`  =  **uživatel provede některé počáteční akci**


Jsme řízení viditelnost v našem tlačítko `TouchUpInside` událostí a budeme muset na ni odkazuje v kódu. Umožňuje určit její **název** `aButton` v panelu pro vlastnosti, jak je znázorněno na následujícím snímku obrazovky:

[![](creating-tabbed-applications-images/abutton-properties.png "Nastavte název na aButton v panelu pro vlastnosti")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Návrhové ploše by teď měl vypadat podobně jako tento snímek obrazovky:

[![](creating-tabbed-applications-images/design-surface1.png "Návrhové ploše by teď měl vypadat podobně jako tento snímek obrazovky")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Přidejme trochu podrobněji k `ViewController2` a `ViewController3`, přidáním štítek pro každé a změna text na 'Dva' a 'Tři' v uvedeném pořadí. To ukazuje uživateli kartě nebo zobrazení, které jsme prohlížení.

#### <a name="wiring-up-the-button"></a>Připojení tlačítko

Vytvoříme načíst `ViewController1` při prvním spuštění aplikace. Když uživatel klepnutím na tlačítko, jsme budete skrýt tlačítko a načíst `UITabBarController` s `ViewController1` instance na první kartě.

Když uživatel uvolní `aButton`, chceme TouchUpInside událostí až se spustí. Umožňuje vybrat tlačítko a v **kartu události** plocha vlastnosti deklarovat obslužná rutina události – `InitialActionCompleted` – tak může být uvedené v kódu. To je znázorněno v následující snímek obrazovky:

[![](creating-tabbed-applications-images/event-handler.png "Když uživatel uvolní aButton, aktivuje událost TouchUpInside")](creating-tabbed-applications-images/event-handler.png#lightbox)

Nyní potřebujeme říct řadiče zobrazení skrýt tlačítko při aktivuje událost `InitialActionCompleted`. V `ViewController1`, přidejte následující metodu částečné:

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

Uložte soubor a spusťte aplikaci. Bychom měli vidět obrazovky zobrazit některá a tlačítko zmizí při Touch nahoru.

#### <a name="adding-the-tab-bar-controller"></a>Přidávání na posuvníku řadiče

Nyní je k dispozici naše počáteční zobrazení funguje podle očekávání. V dalším kroku chceme ho přidat do `UITabBarController`, společně s zobrazení 2 a 3. Umožňuje otevřít scénáři v návrháři.

V **sada nástrojů**, vyhledejte **kartě panelu řadiče** pod řadiče & objekty a přetáhněte to na návrhovou plochu. Jak vidíte na tomto snímku obrazovky, je na kartě panelu řadič bez uživatelského rozhraní a proto přináší dva řadiče zobrazení s ním ve výchozím nastavení:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Přidávání řadiče panelu karta k rozložení")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Odstraňte tyto nové řadiče zobrazení výběrem černým panelu v dolní a stisknutím klávesy odstranit.

V našem scénáři používáme k Segues pro zpracování přechodů mezi TabBarController a naše řadiče zobrazení. Po dokončení práce se počáteční zobrazení, chceme načíst do TabBarController uživateli. Umožňuje nastavit tuto možnost v návrháři.

**CTRL + kliknutí** a **přetáhněte** z tlačítka TabBarController. Na myši nahoru zobrazí se kontextová nabídka. Chceme používat modální segue. 
 
Nastavit všechny naše karty **Ctrl + kliknutí** z TabBarController ke každému z našich řadiče zobrazení v pořadí od jedné do tří a vyberte vztah **kartě** z kontextové nabídky, jak je uvedeno dále:

[![](creating-tabbed-applications-images/context-menu.png "Vyberte kartu vztah")](creating-tabbed-applications-images/context-menu.png#lightbox)

Vaše scénáře by měl vypadat na následující snímek obrazovky:

[![](creating-tabbed-applications-images/segue-layout.png "Scénáři by měl vypadat podobně jako tento snímek obrazovky")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Pokud jsme klikněte na jedné z položek panelu kartě a prozkoumejte panelu Vlastnosti, můžete zjistit počet různé možnosti, jak je uvedeno dále:

[![](creating-tabbed-applications-images/properties-panel.png "Nastavení na kartě Možnosti v Průzkumníku vlastnosti")](creating-tabbed-applications-images/properties-panel.png#lightbox)

To jsme můžete použít k úpravě některých atributů, jako jsou třeba oznámení "BADGE", název a iOS [identifikátor](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), mimo jiné

Pokud jsme uložit a spustit nyní aplikaci, jsme tu, na tlačítko se znovu zobrazí při ViewController1 instance je načten do TabBarController. Umožňuje opravit to kontrolou, zda má aktuální zobrazení nadřazené řadič zobrazení. Pokud ano, víme je uvnitř TabBarController, a proto by být skrytý tlačítko. K třídě ViewController1 přidejme kód níže:

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

Při spouštět aplikace a uživatel klepnutím na tlačítko na první obrazovce UITabBarController je načtena, k zobrazení na první obrazovce umístěn na první kartě, jak je uvedeno níže:

[![](creating-tabbed-applications-images/first-view.png "Ukázkový výstup aplikace")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>Souhrn

Jak používat zahrnuté v tomto článku `UITabBarController` v aplikaci. Jsme projít jak načíst řadiče do každé kartě a také jak nastavit vlastnosti na kartách takový název, image a oznámení "BADGE". Jsme pak zkontrolují pomocí scénářů, jak načíst `UITabBarController` za běhu, když není `RootViewController` okna.


## <a name="related-links"></a>Související odkazy

- [Vytváření aplikací pro – záložkami (ukázka)](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Odkaz na UITabBarController – třída](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
