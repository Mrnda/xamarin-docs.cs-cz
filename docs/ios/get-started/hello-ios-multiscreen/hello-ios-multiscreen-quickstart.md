---
title: Hello, iOS Multiscreen – rychlý start
description: Tento dokument ukazuje, jak rozšířit Phoneword ukázkovou aplikaci přidat druhý obrazovce popisující model-view-controller, navigace iOS a jiné základní koncepty vývoj pro iOS na cestě.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d72e6230-c9ee-4bee-90ec-877d256821aa
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 469032dc7caa46c6a89b350dc37bc9a93366066a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785668"
---
# <a name="hello-ios-multiscreen--quickstart"></a>Hello, iOS Multiscreen – rychlý start

Tato část průvodce přidá druhý obrazovky k Phoneword aplikaci, která se zobrazí historii telefonní čísla, která byla volána s aplikací. Konečné aplikace bude mít druhý obrazovky, který zobrazuje historie volání, které jsou popsány v následující snímek obrazovky:

 [![](hello-ios-multiscreen-quickstart-images/00.png "Konečné aplikace bude mít druhý obrazovky, který zobrazuje historie volání, které jsou popsány v tomto snímku obrazovky")](hello-ios-multiscreen-quickstart-images/00.png#lightbox)

[Doplňujícími podrobné informace](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md), zkontrolujte, zda aplikace, která je sestavení vytvořené a diskutovat o architektuře, navigace a dalších nových konceptů iOS, které jsme na cestě.

 <a name="Requirements" />

## <a name="requirements"></a>Požadavky

Tato příručka obnoví kde Hello, iOS dokumentu vlevo vypnout a vyžaduje dokončení [Hello, iOS rychlý Start](~/ios/get-started/hello-ios/index.md). Dokončené verzi Phoneword aplikace si můžete stáhnout z [Hello, iOS ukázka](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="walkthrough"></a>Návod

Tento postup přidá historie volání obrazovky naše **Phoneword** aplikace.


1. Otevřete **Phoneword** aplikace v sadě Visual Studio for Mac. V případě potřeby hotová aplikace Phoneword z [Hello, iOS návod](~/ios/get-started/hello-ios/index.md) průvodce si můžete stáhnout z [zde](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).


2. Otevřete **Main.storyboard** souboru z **řešení Pad**:

  ![](hello-ios-multiscreen-quickstart-images/02new.png "Main.storyboard v iOS návrháře")


3. Přetáhněte **navigační řadič** z **sada nástrojů** na návrhovou plochu (možná budete muset oddálení podle těchto všechna na návrhovou plochu!):

  ![](hello-ios-multiscreen-quickstart-images/03new.png "Přetáhněte řadič navigační z panelu nástrojů na návrhovou plochu")


4. Přetáhněte **Sourceless Segue** (který je na šedé šipku nalevo od jedné View Controller) k **navigační řadiče** Chcete-li změnit výchozí bod aplikace:

  ![](hello-ios-multiscreen-quickstart-images/04new.png "Přetáhněte Sourceless Segue řadiče navigační změnit výchozí bod aplikace")


5. Vyberte existující **kořenové View Controller** kliknutím na dolním panelu a stiskněte klávesu **odstranit** ho odebrat z návrhové plochy.
Pak přesuňte **Phoneword** scény vedle **navigační řadiče**:

  ![](hello-ios-multiscreen-quickstart-images/05new.png "Přesunout scény Phoneword vedle řadičem navigace")


6. Nastavte **ViewController** jako řadič navigační **kořenového řadiče zobrazení**. Podržte **Ctrl** klíče a klikněte na tlačítko uvnitř **navigační řadiče**. Modré řádku by se zobrazit. Potom stále podržíte stisknutou **Ctrl** klíče, přetáhněte z **navigační řadič** k **Phoneword** scény a verzi. To se označuje jako _klávesou Ctrl_:

 ![](hello-ios-multiscreen-quickstart-images/06.png "Přetáhněte z řadiče navigační Phoneword scény a verze")


7. Z popover, nastavte vztah na **kořenové**:

  ![](hello-ios-multiscreen-quickstart-images/07new.png "Nastavení kořenové relace")

  **ViewController** je teď **navigační řadiče kořenové View Controller:**

  ![](hello-ios-multiscreen-quickstart-images/08.png "ViewController je nyní řadič navigační řadiče kořenové zobrazení")


8. Dvakrát klikněte na **Phoneword** na obrazovce **název** panel a změňte **název** k **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/09.png "Změnit text na \"Phoneword.")


9. Přetáhněte **tlačítko** z **sada nástrojů** a umístěte ji pod **tlačítka volání**. Přetáhněte obslužných rutin, aby se nové **tlačítko** šířku stejné jako **tlačítka volání**:

  ![](hello-ios-multiscreen-quickstart-images/10new.png "Vytvořit nový tlačítka šířku stejné jako tlačítko volání")


10. V **vlastnosti Pad**, změnit **název** tlačítka na **CallHistoryButton** a změňte **název** k **volání Historie**:

  ![](hello-ios-multiscreen-quickstart-images/11new.png "Změňte název tlačítka na CallHistoryButton a změnit text do historie volání")


11. Vytvořte **historie volání** obrazovky. Z **sada nástrojů**, přetáhněte ji **řadiče zobrazení tabulky** na návrhovou plochu:

 ![](hello-ios-multiscreen-quickstart-images/12new.png "Přetáhněte řadič zobrazení tabulky na návrhovou plochu")


12. Potom vyberte **řadiče zobrazení tabulky** kliknutím na černé panelu v dolní části scény. V **vlastnosti Pad**, změnit **řadiče zobrazení tabulky** třídy k `CallHistoryController` a stiskněte klávesu **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/13new.png "Změňte třídu řadiče zobrazení tabulky CallHistoryController")

  IOS návrhář vygeneruje vlastní základní třídu s názvem `CallHistoryController` ke správě této obrazovce je obsahu zobrazení hierarchie.
  **CallHistoryController.cs** soubor se zobrazí v **řešení Pad**:

  ![](hello-ios-multiscreen-quickstart-images/14new.png "Soubor CallHistoryController.cs v řešení pro")


13. Dvakrát klikněte na **CallHistoryController.cs** soubor otevřít a nahraďte jeho obsah následujícím kódem:

  ```csharp
  using System;
  using Foundation;
  using UIKit;
  using System.Collections.Generic;

  namespace Phoneword_iOS
  {
      public partial class CallHistoryController : UITableViewController
      {
          public List<string> PhoneNumbers { get; set; }

          static NSString callHistoryCellId = new NSString ("CallHistoryCell");

          public CallHistoryController (IntPtr handle) : base (handle)
          {
              TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
              TableView.Source = new CallHistoryDataSource (this);
              PhoneNumbers = new List<string> ();
          }

          class CallHistoryDataSource : UITableViewSource
          {
              CallHistoryController controller;

              public CallHistoryDataSource (CallHistoryController controller)
              {
                  this.controller = controller;
              }

              public override nint RowsInSection (UITableView tableView, nint section)
              {
                  return controller.PhoneNumbers.Count;
              }

              public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
              {
                  var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                  int row = indexPath.Row;
                  cell.TextLabel.Text = controller.PhoneNumbers [row];
                  return cell;
              }
          }
      }
  }
  ```

  Uložit aplikaci (**⌘ + s**) a sestavte jej (**⌘ + b**) k zajištění nejsou žádné chyby.


14. Vytvoření _Segue_ (Přechod) mezi **Phoneword** scény a **historie volání** scény.
  V **Phoneword scény**, vyberte **tlačítka historie volání** a Ctrl přetažení z **tlačítko** k **historie volání** scény:

  ![](hello-ios-multiscreen-quickstart-images/15.png "CTRL přetažení z tlačítka scény historie volání")

  Z **akce Segue** popover, vyberte **zobrazit**

  IOS Návrhář přidá Segue mezi dvěma scény:

  ![](hello-ios-multiscreen-quickstart-images/17new.png "Segue mezi dvěma scény")


15. Přidat **název** k **řadiče zobrazení tabulky** výběrem černým panelu v dolní části scény a změna **nadpis řadiče zobrazení** k **volání Historie** v **Pad vlastnosti**:

  ![](hello-ios-multiscreen-quickstart-images/18new.png "Změna názvu řadiče zobrazení do historie volání v panelu pro vlastnosti")

16. Při spuštění aplikace **tlačítka historie volání** se otevře **historie volání** obrazovky, ale tabulka zobrazení budou prázdná, protože neexistuje žádný kód o sledovat a zobrazit telefonní čísla.

  Tato aplikace se uloží telefonní čísla jako seznam řetězců.

  Přidat `using` direktivy pro `System.Collections.Generic` v horní části **ViewController**:

  ```csharp
  using System.Collections.Generic;
  ```



17. Upravit `ViewController` třídy následujícím kódem:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```


Je několik věcí, které jsou zde děje
  * Proměnná `translatedNumber` byl přesunut z `ViewDidLoad` metodu _proměnné na úrovni třídy_.
  * **CallButton** kód byla změněna přidat vytáčená čísla na seznam telefonních čísel voláním `PhoneNumbers.Add(translatedNumber)`
  * `PrepareForSegue` Metoda byla přidána.

  Uložte a sestavte aplikaci a ujistěte se, že nejsou žádné chyby.

20. Stiskněte **spustit** tlačítko spustit aplikaci uvnitř **simulátoru iOS**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "Klikněte na tlačítko Start, spusťte aplikaci v simulátoru iOS")


Blahopřejeme k dokončení svoji první aplikaci Xamarin.iOS více obrazovky!

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="walkthrough"></a>Návod

Tento postup přidá historie volání obrazovky naše **Phoneword** aplikace.


1. Otevřete **Phoneword** aplikace v sadě Visual Studio. V případě potřeby stáhnout [dokončit Phoneword aplikace](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) z [Hello, iOS návod](~/ios/get-started/hello-ios/index.md) Průvodce stažení. Odvolat, že je nutné se připojit k [Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) používat iOS Designer a simulátoru iOS.


2. Spusťte úpravou uživatelské rozhraní. Otevřete **Main.storyboard** souboru z **Průzkumníku řešení**, a ověřte, zda **zobrazení jako** je nastaven na _iPhone 6_:

  ![](hello-ios-multiscreen-quickstart-images/image1.png "Main.storyboard v iOS návrháře")


3. Přetáhněte **navigační řadič** z **sada nástrojů** na návrhovou plochu:

  ![](hello-ios-multiscreen-quickstart-images/image2.png "Přetáhněte řadič navigační z panelu nástrojů na návrhovou plochu")


4. Přetáhněte **Sourceless Segue** (který je na šedé šipku nalevo od **Phoneword** scény) z **Phoneword** scény k **navigační řadič** Chcete-li změnit výchozí bod aplikace:

  ![](hello-ios-multiscreen-quickstart-images/image3.png "Přetáhněte Sourceless Segue řadiče navigační změnit výchozí bod aplikace")


5. Vyberte **kořenové View Controller** kliknutím na černé řádku a stiskněte klávesu **odstranit** ho odebrat z návrhové plochy.
  Pak přesuňte **Phoneword** scény vedle **navigační řadiče**:

  ![](hello-ios-multiscreen-quickstart-images/image4.png "Přesunout scény Phoneword vedle řadičem navigace")


6. Nastavte **ViewController** jako řadič navigační `Root View Controller`. Podržte **Ctrl** klíče a klikněte na tlačítko uvnitř **navigační řadiče**. Modré řádku by se zobrazit. Potom stále podržíte stisknutou **Ctrl** klíče, přetáhněte z **navigační řadič** k **Phoneword** scény a verzi. To se označuje jako _klávesou Ctrl_:

  ![](hello-ios-multiscreen-quickstart-images/image5.png "Přetáhněte z řadiče navigační Phoneword scény a verze")


7. Z popover, nastavte vztah na **kořenové**:

  ![](hello-ios-multiscreen-quickstart-images/image6.png "Nastaví kořen relace")

  **ViewController** je nyní naše **navigační řadiče kořenové View Controller.**


8. Dvakrát klikněte na **Phoneword** na obrazovce **název** panel a změňte **název** k **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/image7.png "Změňte titulek na Phoneword")


9. Přetáhněte **tlačítko** z **sada nástrojů** a umístěte ji pod **tlačítka volání**. Přetáhněte obslužných rutin, aby se nové **tlačítko** šířku stejné jako **tlačítka volání**:

  ![](hello-ios-multiscreen-quickstart-images/image8.png "Vytvořit nový tlačítka šířku stejné jako tlačítko volání")


10. V **Explorer vlastnosti**, změnit **název** z **tlačítko** k `CallHistoryButton` a změňte **název** k  **Historie volání**:

  ![](hello-ios-multiscreen-quickstart-images/image9.png "Změňte název na tlačítko 'CallHistoryButton' a název historie volání")


11. Vytvořte **historie volání** obrazovky. Z **sada nástrojů**, přetáhněte ji **řadiče zobrazení tabulky** na návrhovou plochu:

  ![](hello-ios-multiscreen-quickstart-images/image10.png "Přetáhněte řadič zobrazení tabulky na návrhovou plochu")


12. Vyberte **řadiče zobrazení tabulky** kliknutím na černé panelu v dolní části scény. V **Explorer vlastnosti**, změnit **řadiče zobrazení tabulky** třídy k `CallHistoryController` a stiskněte klávesu **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/image11.png "Změňte třídu řadiče zobrazení tabulky CallHistoryController")

  IOS návrhář vygeneruje vlastní základní třídu s názvem `CallHistoryController` ke správě této obrazovce je obsahu zobrazení hierarchie.
  **CallHistoryController.cs** soubor se zobrazí v **Průzkumníku řešení**:

  ![](hello-ios-multiscreen-quickstart-images/image12.png "CallHistoryController.cs soubor v Průzkumníku řešení")


13. Dvakrát klikněte na **CallHistoryController.cs** soubor otevřít a nahraďte jeho obsah následujícím kódem:

        using System;
        using Foundation;
        using UIKit;
        using System.Collections.Generic;

        namespace Phoneword
        {
            public partial class CallHistoryController : UITableViewController
            {
                public List<String> PhoneNumbers { get; set; }

                static NSString callHistoryCellId = new NSString ("CallHistoryCell");

                public CallHistoryController (IntPtr handle) : base (handle)
                {
                    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
                    TableView.Source = new CallHistoryDataSource (this);
                    PhoneNumbers = new List<string> ();
                }

                class CallHistoryDataSource : UITableViewSource
                {
                    CallHistoryController controller;

                    public CallHistoryDataSource (CallHistoryController controller)
                    {
                        this.controller = controller;
                    }

                    // Returns the number of rows in each section of the table
                    public override nint RowsInSection (UITableView tableView, nint section)
                    {
                        return controller.PhoneNumbers.Count;
                    }

                    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
                    {
                        var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                        int row = indexPath.Row;
                        cell.TextLabel.Text = controller.PhoneNumbers [row];
                        return cell;
                    }
                }
            }
        }

  Uložit aplikaci a sestavte jej k zajištění, že nejsou žádné chyby. Je to v pořádku prozatím ignorovat všechna upozornění sestavení.


14. Vytvoření _Segue_ (Přechod) mezi **Phoneword** scény a **historie volání** scény.
  V **Phoneword scény**, vyberte **tlačítka historie volání** a **stisknutou klávesou Ctrl** z **tlačítko** k **historie volání**  Scény:

  ![](hello-ios-multiscreen-quickstart-images/image13.png "CTRL přetažení z tlačítka scény historie volání")

  Z **akce Segue** popover, vyberte **zobrazit**:

  ![](hello-ios-multiscreen-quickstart-images/image14.png "Vyberte jako typ segue zobrazit")

  IOS Návrhář přidá Segue mezi dvěma scény:

  ![](hello-ios-multiscreen-quickstart-images/image15.png "Segue mezi dvěma scény")


15. Přidat **název** k **řadiče zobrazení tabulky** výběrem černým panelu v dolní části scény a změna **řadiče zobrazení > název** k **volání Historie** v **Explorer vlastnosti**:

  ![](hello-ios-multiscreen-quickstart-images/image16.png "Změna názvu řadiče zobrazení do historie volání")


16. Při spuštění aplikace **tlačítka historie volání** se otevře **historie volání** obrazovky, ale tabulka zobrazení budou prázdná, protože neexistuje žádný kód o sledovat a zobrazit telefonní čísla.

  Tato aplikace se uloží telefonní čísla jako seznam řetězců.

  Přidat `using` direktivy pro `System.Collections.Generic` v horní části **ViewController**:

        using System.Collections.Generic;

17. Upravit `ViewController` třídy následujícím kódem:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

Je několik věcí, které jsou zde děje
  * Proměnná `translatedNumber` byl přesunut z `ViewDidLoad` metodu _proměnné na úrovni třídy_.
  * **CallButton** kód byla změněna přidat vytáčená čísla na seznam telefonních čísel voláním `PhoneNumbers.Add(translatedNumber)`
  * `PrepareForSegue` Metoda byla přidána.

  Uložte a sestavte aplikaci a ujistěte se, že nejsou žádné chyby.

  Uložte a sestavte aplikaci a ujistěte se, že nejsou žádné chyby.


20. Stiskněte **spustit** tlačítko spustit aplikaci uvnitř **simulátoru iOS**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "Na první obrazovce ukázková aplikace")


Blahopřejeme k dokončení svoji první aplikaci Xamarin.iOS více obrazovky!


-----

Aplikace nyní může zpracovávat navigační pomocí obou Storyboard Segues a v kódu. Nyní je čas dissect nástroje a dovednosti jsme právě naučili v [Hello, iOS Multiobrazovka podrobné informace](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md).


## <a name="related-links"></a>Související odkazy

- [Hello, iOS (ukázka)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS Provisioning Portal](https://developer.apple.com/ios/manage/overview/index.action)
