---
title: iOS rozšíření
description: Zavádí v iOS 8, jsou rozšíření pomůcky, které jsou prezentované podle iOS v standardní kontextech, například v centru oznámení, když uživatel požádá vlastní klávesnice, nebo když jsou fotografie úpravy. Všechna rozšíření jsou nainstalovány ve spojení s aplikací kontejneru a aktivují z určitého bodu rozšíření v hostiteli aplikace.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: af52db5f1add040af025f2134f0e9a7b3936f4f2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="ios-extensions"></a>iOS rozšíření

_Zavádí v iOS 8, jsou rozšíření pomůcky, které jsou prezentované podle iOS v standardní kontextech, například v centru oznámení, když uživatel požádá vlastní klávesnice, nebo když jsou fotografie úpravy. Všechna rozšíření jsou nainstalovány ve spojení s aplikací kontejneru a aktivují z určitého bodu rozšíření v hostiteli aplikace._

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Vytváření rozšíření v iOS, pomocí [univerzity Xamarin](https://university.xamarin.com/)**

Rozšíření, jako byla zavedená v iOS 8, se specializují `UIViewControllers` , jsou prezentované podle iOS uvnitř standardní kontexty například v rámci **centru oznámení**, jak se specializuje vlastní klávesové typy, které požadoval uživatel k provedení vstup nebo jiných kontextech, například úpravy fotografií, kde můžete rozšíření poskytnout filtry zvláštní efekt.

Všechna rozšíření jsou nainstalovány ve spojení s aplikací kontejneru (s obou elementů vytvořené pomocí rozhraní API Unified 64-bit) a jsou aktivovány z určitého bodu rozšíření v hostiteli aplikace. A vzhledem k tomu, že se použije jako doplňky pro stávající funkce systému, musí být vysoký výkon, Štíhlá a robustní. 

Tento článek obsahuje následující témata:

- [Rozšíření body](#Extension-Points) -obsahuje seznam typu rozšíření body k dispozici a typ rozšíření, která se dají vytvořit pro každý bod.
- [Omezení](#Limitations) -rozšíření mít několik omezení, z nichž některé jsou univerzální pro všechny typy, zatímco jiné typy rozšíření může mít specifické omezení na jejich využití.
- [Distribuce, instalace a spuštění rozšíření](#Distributing-Installing-and-Running-Extensions) -rozšíření se distribuují z aplikace kontejneru, který naopak je odeslána a distribuovány prostřednictvím App Storu. Přípony distribuované s aplikace jsou nainstalovány v tomto bodě, ale uživatel musí explicitně povolit každé rozšíření. Různé typy rozšíření jsou povolené různými způsoby.
- [Životní cyklus rozšíření](#Extension-Lifecycle) – rozšíření `UIViewController` bude vytvořena instance a začít normální životního cyklu řadiče zobrazení. Ale na rozdíl od normální aplikace, rozšíření jsou načtena, provést a pak ukončena opakovaně.
- [Vytváření rozšíření](#Creating-an-Extension) -při vývoji rozšíření, vaše řešení bude obsahovat minimálně dva projekty: kontejner aplikaci a jeden projektu pro každé rozšíření kontejneru poskytuje.
- [Návod](#Walkthrough) – zahrnuje vytvoření jednoduché **Dnes** pomůcky rozšíření, které poskytuje jeho uživatelské rozhraní pomocí Storyboard nebo v kódu, instalaci rozšíření a testování v simulátoru iOS.
- [Komunikaci s aplikací hostitele](#Communicating-with-the-Host-App) -stručně popisuje komunikaci s aplikací hostitele z rozšíření.
- [Komunikuje s nadřazenou aplikací](#Communicating-with-the-Parent-App) -stručně popisuje komunikaci s nadřazenou aplikací z rozšíření.
- [Aspekty a opatření](#Precautions-and-Considerations) -seznamu některé vědět opatření a důležité informace, které musí vzít v úvahu při navrhování a implementaci rozšíření.
 

<a name="Extension-Points" />

## <a name="extension-points"></a>Rozšíření body

|Typ|Popis|Rozšíření bodu|Hostitele aplikace|
|--- |--- |--- |--- |
|Akce|Specializované editor nebo prohlížeč pro určitý typ média|`com.apple.ui-services`|všechny|
|Zprostředkovatel dokumentu|Umožňuje aplikaci, aby používala dokumentu vzdáleného úložiště|`com.apple.fileprovider-ui`|Aplikace používající [UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|Klávesnice|Alternativní klávesnice|`com.apple.keyboard-service`|všechny|
|Úpravy fotografií|Fotografie manipulaci a úpravy|`com.apple.photo-editing`|Photos.App editor|
|Sdílené složky|Sdílí data se sociálními sítěmi, zasílání zpráv služby atd.|`com.apple.share-services`|všechny|
|Dnes|"Zařízení", které zobrazí na obrazovce Dnes nebo centra oznámení|`com.apple.widget-extensions`|Centrum oznámení a dnešek|

[Další rozšíření body](~/ios/platform/introduction-to-ios10/index.md#app-extensions) byly přidány v iOS 10.

<a name="Limitations" />

## <a name="limitations"></a>Omezení

Rozšíření mít několik omezení, z nichž některé jsou univerzální pro všechny typy (pro instance, žádný typ rozšíření má přístup ke kamery a mikrofon) při jiné typy rozšíření může mít specifické omezení na jejich využití (například vlastní klávesnice nelze použít pro zabezpečená data vstupní pole, jako pro hesla). 

Univerzální omezení jsou:

- [Stavu Kit](~/ios/platform/healthkit.md) a [uživatelského rozhraní sady události](~/ios/platform/eventkit.md) rozhraní nejsou k dispozici
- Rozšíření nelze použít [Rozšířené režimy pozadí](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- Rozšíření přístup k zařízení kamery a mikrofon (i když přistupují může existující mediální soubory)
- Rozšíření nelze přijímat letecké vyřadit data (i když mohou přenášet data přes letecké vyřadit)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) a [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) nejsou k dispozici; musíte použít rozšíření [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Několik členy [UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) nejsou k dispozici: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` a `UIApplication.EndIgnoringInteractionEvents`
- iOS vynucuje omezení využití paměti 16 MB v dnešních rozšíření.
- Ve výchozím nastavení klávesnice rozšíření nemají přístup k síti. Tato akce ovlivní ladění na zařízení (v simulátoru omezení nevynucuje), protože Xamarin.iOS vyžaduje přístup k síti pro ladění pracovat. Je možné žádost o přístup k síti nastavením `Requests Open Access` hodnota v projektu Info.plist k `Yes`. Najdete v tématu společnosti Apple [vlastní klávesnice průvodce](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) pro další informace o omezeních rozšíření klávesnice.

Jednotlivé omezení, najdete v tématu společnosti Apple [Průvodce programováním rozšíření aplikace](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>Distribuce, instalace a spuštění rozšíření

Rozšíření se distribuují z aplikace kontejneru, který naopak je odeslána a distribuovány prostřednictvím App Storu. Přípony distribuované s aplikace jsou nainstalovány v tomto bodě, ale uživatel musí explicitně povolit každé rozšíření. Různé typy rozšíření jsou povolené různými způsoby; několik vyžadovat, aby uživatel přejděte na **nastavení** aplikaci a povolit je z ní. Zatímco jiné jsou povolené v okamžiku použití, například povolení příponu sdílení při odesílání fotografie. 

Aplikace, ve kterém se používá rozšíření (kde uživatel zaznamená bodem rozšíření) se označuje jako **hostitele aplikace**, protože se aplikace, který je hostitelem rozšíření, jakmile je spuštěn. Aplikace, která instalaci rozšíření je **kontejneru aplikace**, protože se aplikace, která obsahovala rozšíření při instalaci.  

Kontejner aplikace obvykle popisuje rozšíření a provede uživatel provede procesem jeho povolení.

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>Životní cyklus rozšíření

Rozšíření může být stejně jednoduché jako jeden [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) nebo složitější rozšíření, která představují několika obrazovkách uživatelského rozhraní. Když uživatel dojde _rozšíření body_ (jako např. kdy sdílení obrázek), budou mít možnost vybrat z rozšíření registrovaná pro tento bod rozšíření. 

Pokud si zvolí jeden z vaší aplikace je rozšíření, jeho `UIViewController` bude vytvořena instance a začít normální životního cyklu řadiče zobrazení. Ale na rozdíl od normální aplikace, které jsou pozastavené, ale obecně ukončen po dokončení uživateli interakci s nimi, rozšíření jsou načtena, provést a pak ukončena opakovaně.

Rozšíření mohou komunikovat s jejich hostitele aplikací prostřednictvím [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) objektu. Některá rozšíření mít operace, které přijímají asynchronní zpětná volání s výsledky. Tato zpětná volání, které se bude provádět na vlákna na pozadí a rozšíření to je třeba brát v úvahu; například pomocí [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) Pokud chtějí aktualizovat uživatelské rozhraní. Najdete v článku [komunikaci s aplikací hostitele](#Communicating-with-the-Host-App) části níže další podrobnosti.

Ve výchozím nastavení rozšíření a jejich kontejneru aplikace nemůže komunikovat, i když jsou společně. V některých případech aplikace kontejneru je v podstatě prázdný "přenosů" kontejner jejichž účelem je obsluhovat po instalaci rozšíření. Pokud však okolnosti, rozšíření a kontejneru aplikace může sdílet prostředky z běžných oblasti. Kromě toho **dnes rozšíření** může požádat o jeho kontejneru aplikace otevřete adresu URL. Toto chování se zobrazí v [momentální pomůcky odpočítávání](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>Vytváření rozšíření

Rozšíření (a jejich kontejneru aplikace) musí být 64bitová verze binárních souborů a vyvíjené Xamarin.iOS [jednotné rozhraní API](http://developer.xamarin.com/guides/cross-platform/macios/unified). Při vývoji rozšíření, vaše řešení bude obsahovat minimálně dva projekty: kontejner aplikaci a jeden projektu pro každé rozšíření kontejneru poskytuje. 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>Požadavky na projekt aplikace kontejneru

Kontejner aplikace použitý k instalaci rozšíření má následující požadavky:

- Odkaz na projekt se musí spravovat.   
- Musí být kompletní aplikace (musí být schopný spustit a úspěšně spustit) i v případě, že ho neprovede se žádná více než poskytnout způsob, jak nainstalovat rozšíření. 
- Musí mít identifikátor svazku, který slouží jako základ pro identifikátor balíku rozšíření projektu (viz další podrobnosti části níže).

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>Požadavky na rozšíření projektu

Kromě toho rozšíření projektu má následující požadavky:

- Musí mít identifikátor svazku, který začíná identifikátor svazku jeho kontejneru aplikace. Například pokud aplikace kontejneru má identifikátor svazku z `com.myCompany.ContainerApp`, může být identifikátor rozšíření `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Je nutné definovat klíč `NSExtensionPointIdentifier`, s odpovídající hodnotou (, jako `com.apple.widget-extension` pro **Dnes** pomůcky centru oznámení) v jeho `Info.plist` souboru.
- Musí definovat také *buď* `NSExtensionMainStoryboard` klíč nebo `NSPrincipalClass` klíče v jeho `Info.plist` soubor s odpovídající hodnotou:
    - Použití `NSExtensionMainStoryboard` klíč k zadání názvu scénáři, která představuje hlavní uživatelské rozhraní pro rozšíření (minus `.storyboard`). Například `Main` pro `Main.storyboard` souboru.
    - Použití `NSPrincipalClass` klíč zadat třídu, která bude inicializována při spuštění rozšíření. Hodnota musí odpovídat **zaregistrovat** hodnotu vaší `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Konkrétní typy rozšíření může mít větší požadavky: Například **Dnes** nebo **centru oznámení** rozšíření hlavní třída musí implementovat [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> Pokud spustíte projekt pomocí jedné rozšíření šablon, které aplikace Visual Studio pro Mac, většina (Pokud se vše) tyto požadavky bude zadaný a splněny pro vás automaticky šablonou.

<a name="Walkthrough" />

## <a name="walkthrough"></a>Návod 

V následujícím návodu vytvoříte příklad **Dnes** pomůcku, která vypočítá den a počet dnů zbývajících v roce:

[![](extensions-images/carpediemscreenshot-sm.png "Pomůcka dnes příklad, která by vypočítala den a počet dnů zbývajících v roce")](extensions-images/carpediemscreenshot.png#lightbox)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>Vytváření řešení

Pokud chcete vytvořit požadované řešení, postupujte takto:

1. Nejprve vytvořte nový iOS, **jediné zobrazení aplikace** projektu a klikněte na tlačítko **Další** tlačítko: 

    [![](extensions-images/today01.png "Nejprve vytvořte nový iOS, projekt jediné zobrazení aplikace a klikněte na tlačítko Další")](extensions-images/today01.png#lightbox)
2. Volání projektu `TodayContainer` a klikněte na **Další** tlačítko: 

    [![](extensions-images/today02.png "Volání TodayContainer projektu a klikněte na tlačítko Další")](extensions-images/today02.png#lightbox)
3. Ověřte **název projektu** a **název řešení SolutionName** a klikněte na tlačítko **vytvořit** tlačítko pro vytvoření řešení: 

    [![](extensions-images/today03.png "Ověřte název projektu a název řešení SolutionName a klikněte na tlačítko Vytvořit k vytvoření řešení")](extensions-images/today03.png#lightbox)
4. Vedle **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení a přidejte nový **iOS rozšíření** projekt **dnes rozšíření** šablony: 

    [![](extensions-images/today04.png "V dalším kroku v Průzkumníku řešení klikněte pravým tlačítkem na řešení a přidat nový projekt iOS rozšíření ze šablony dnes rozšíření")](extensions-images/today04.png#lightbox)
5. Volání projektu `DaysRemaining` a klikněte na **Další** tlačítko: 

    [![](extensions-images/today05.png "Volání DaysRemaining projektu a klikněte na tlačítko Další")](extensions-images/today05.png#lightbox)
6. Zkontrolujte projekt a klikněte na **vytvořit** tlačítko k jeho vytvoření: 

    [![](extensions-images/today06.png "Zkontrolujte projekt a klikněte na tlačítko Vytvořit k jeho vytvoření")](extensions-images/today06.png#lightbox)

Výsledný řešení teď měli mít dva projekty, jak je vidět tady:

[![](extensions-images/today07.png "Výsledný řešení teď by mělo obsahovat dva projekty, jak je vidět tady")](extensions-images/today07.png#lightbox)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>Vytvoření uživatelského rozhraní rozšíření

Potom budete muset navrhnout rozhraní pro vaše **Dnes** pomůcky. To můžete buď provést pomocí Storyboard nebo pomocí vytvoření uživatelského rozhraní v kódu. Obě metody bude mít podrobně níže.

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>Pomocí scénářů

K vytvoření uživatelského rozhraní s scénáře, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte na projekt rozšíření `Main.storyboard` soubor otevřete pro úpravy: 

    [![](extensions-images/today08.png "Poklikejte na soubor rozšíření projekty Main.storyboard otevřete pro úpravy")](extensions-images/today08.png#lightbox)
2. Vyberte štítek, který byl automaticky přidán do rozhraní šablony a pojmenujte ho **název** `TodayMessage` v **pomůcky** kartě **Explorer vlastnosti**: 

    [![](extensions-images/today09.png "Vyberte štítek, který byl automaticky přidán do rozhraní šablony a poskytněte název TodayMessage na kartě pomůcky Průzkumníku vlastnosti")](extensions-images/today09.png#lightbox)
3. Uložte změny do scénáře.

<a name="Using-Code" />

#### <a name="using-code"></a>Pomocí kódu

K vytvoření uživatelského rozhraní v kódu, postupujte takto: 

1. V **Průzkumníku řešení**, vyberte **DaysRemaining** projekt, přidejte novou třídu a pojmenujte ji `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining projekt, přidejte novou třídu a pojmenujte ji CodeBasedViewController")](extensions-images/code01.png#lightbox)
2. Znovu, za **Průzkumníku řešení**, dvakrát klikněte na rozšíření `Info.plist` soubor otevřete pro úpravy: 

    [![](extensions-images/code02.png "Poklikejte na soubor Info.plist rozšíření otevřete pro úpravy")](extensions-images/code02.png#lightbox)
3. Vyberte **zobrazení zdroje** (od dolní části obrazovky) a otevřete `NSExtension` uzlu: 

    [![](extensions-images/code03.png "Vyberte zobrazení zdroje v dolní části obrazovky a otevřete NSExtension uzlu")](extensions-images/code03.png#lightbox)
4. Odeberte `NSExtensionMainStoryboard` klíče a přidejte `NSPrincipalClass` s hodnotou `CodeBasedViewController`: 

    [![](extensions-images/code04.png "Odeberte NSExtensionMainStoryboard klíč a přidejte NSPrincipalClass s hodnotou CodeBasedViewController")](extensions-images/code04.png#lightbox)
5. Uložte provedené změny.

Potom upravte `CodeBasedViewController.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

Všimněte si, že `[Register("CodeBasedViewContoller")]` odpovídá hodnotě, který jste zadali pro `NSPrincipalClass` výše.

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>Kódování rozšíření

S uživatelským rozhraním, vytvořit, otevřete `TodayViewController.cs` nebo `CodeBasedViewController.cs` souboru (na základě metody použité k vytvoření výše uvedené uživatelské rozhraní), změny **ViewDidLoad** metoda vypadá takto:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

Pokud metodu uživatelské rozhraní pomocí kódu na základě, nahraďte `// Insert code to power extension here...` komentář s nový kód z výše. Po volání základní implementaci (a vkládání štítek pro verzi na základě kódu), tento kód nemá jednoduchý výpočet k získání den v roce a zbývající počet dnů. Poté zobrazí zprávu v popisku (`TodayMessage`) vytvořené v návrhu uživatelského rozhraní.

Všimněte si, jak podobně jako tento proces je normální proces vytváření aplikace. Rozšíření `UIViewController` má stejný životní cyklus jako řadiče zobrazení v aplikaci, s výjimkou rozšíření nemají režimy pozadí a nejsou pozastavit až po dokončení uživatele jejich používání. Místo toho jsou rozšíření opakovaně inicializován a zrušte přidělené podle potřeby.

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>Vytváření kontejneru aplikace uživatelského rozhraní

V tomto návodu aplikace kontejneru jednoduše slouží jako metody k odeslání a nainstalovat rozšíření a poskytuje žádné funkce. Upravit TodayContainer `Main.storyboard` souboru a přidejte text definování rozšíření funkce a jak ji nainstalovat:

[![](extensions-images/today10.png "Upravte soubor TodayContainers Main.storyboard a přidejte text definování rozšíření funkce a k jeho instalaci")](extensions-images/today10.png#lightbox)

Uložte změny do scénáře.

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>Testování rozšíření

Chcete-li otestovat rozšíření v simulátoru iOS, spusťte **TodayContainer** aplikace. Zobrazí se hlavní zobrazení kontejneru:

[![](extensions-images/run01.png "Zobrazí se zobrazení hlavní kontejnery")](extensions-images/run01.png#lightbox)

Další, stiskněte tlačítko **Domů** tlačítko v simulátoru prstem směrem dolů z horní části obrazovky, otevřete **centru oznámení**, vyberte **Dnes** a klikněte **Upravit** tlačítko:

[![](extensions-images/run02.png "Stiskněte tlačítko Domů v simulátoru prstem směrem dolů z horní části obrazovky, otevřete Centrum oznámení, vyberte kartu dnes a klikněte na tlačítko Upravit")](extensions-images/run02.png#lightbox)

Přidat **DaysRemaining** rozšíření **Dnes** zobrazení a klikněte na tlačítko **provádí** tlačítko:

[![](extensions-images/run03.png "Přidejte rozšíření DaysRemaining do zobrazení dnes a klikněte na tlačítko Hotovo")](extensions-images/run03.png#lightbox)

Přidá nový widget **Dnes** zobrazení a výsledky se zobrazí:

[![](extensions-images/run04.png "Přidá nový widget zobrazení dnes a zobrazí výsledky")](extensions-images/run04.png#lightbox)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>Komunikaci s aplikací hostitele

Příklad dnes rozšíření, které jste vytvořili výše nekomunikuje s jeho hostitele aplikace ( **Dnes** obrazovky). Pokud tomu bylo, využije [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) vlastnost `TodayViewController` nebo `CodeBasedViewController` třídy. 

Pro rozšíření, která bude přijímat data z jejich hostitele aplikací, data jsou ve formě pole [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) objekty uložené v [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) vlastnost [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) k rozšíření `UIViewController`.

Další rozšíření, jako jsou úpravy fotografií rozšíření, může uživatel dokončení nebo zrušení využití rozlišit. To bude signál zpět na hostitele aplikaci pomocí [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) a [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) metody [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) vlastnost.

Další informace najdete v tématu společnosti Apple [Průvodce programováním rozšíření aplikace](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>Komunikuje s nadřazenou aplikací

Skupinu aplikace umožňuje různé aplikace (nebo aplikace a její rozšíření) pro přístup k umístění úložiště sdílený soubor. Skupiny aplikací lze použít pro data, jako jsou:

- [Nastavení Apple Watch](~/ios/watchos/app-fundamentals/settings.md).
- [Sdílené NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [Sdílené soubory](~/ios/watchos/app-fundamentals/parent-app.md#files).

Další informace najdete v tématu [skupin aplikací](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) části našich **práce s možností** dokumentaci.

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

Při práci s příponami, použijte identifikátor pro Uniform typu (UTI) k vytvoření a pracovat s daty, která se vyměňují mezi aplikace, ostatní aplikace a služby.

`MobileCoreServices.UTType` Statická třída definuje následující vlastnosti pomocné rutiny, které se týkají společnosti Apple `kUTType...` definice:

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

Podívejte se na následující příklad:

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Další informace najdete v tématu [skupin aplikací](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) části našich **práce s možností** dokumentaci.


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>Opatření a důležité informace

Rozšíření mít výrazně méně paměti, které jsou k dispozici pro jejich než se aplikace. Budou se očekává provádět rychle a s minimální narušení na uživatele a aplikace, které jsou hostované v. Rozšíření ale měl by poskytnout rozlišovací, užitečné funkce spotřebitelskou aplikaci s partnerské uživatelské rozhraní, které umožňují uživateli umožňují identifikovat rozšíření vývojáře nebo kontejneru aplikace, ke které patří.

Zadané tyto úzkou požadavek, měli byste pouze nasadit rozšíření, které důkladně otestovat a optimalizované pro výkon a spotřebu paměti. 

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento dokument má zahrnutých rozšíření, co jsou, typ rozšíření body a známá omezení pro rozšíření zařízení s iOS. Je popsané, vytváření, distribuci, instalaci a spuštění rozšíření a životního cyklu rozšíření. Je poskytovala návod, jak vytvořit jednoduchou **Dnes** pomůcky zobrazující dva způsoby vytvoření ovládacího prvku uživatelského rozhraní pomocí scénářů nebo kódu. Je vám ukázal, jak otestovat rozšíření v simulátoru iOS. Nakonec se stručně popsané komunikaci s použitím aplikace hostitele a několik opatření a důležité informace, které je třeba vzít při vývoji rozšíření. 


## <a name="related-links"></a>Související odkazy

- [ContainerApp (ukázka)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Vytváření rozšíření v Xamarin.iOS (video)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
