---
title: "Automatizaci Xamarin.Forms testování s Xamarin.UITest a Center aplikace"
description: "Součást Xamarin UITest lze s Xamarin.Forms zápis testů uživatelského rozhraní pro spouštění v cloudu na stovky zařízení."
ms.topic: article
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/31/2016
ms.openlocfilehash: 78788524c1afdda127762049018ca769926f729e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="automate-xamarinforms-testing-with-xamarinuitest-and-app-center"></a>Automatizaci Xamarin.Forms testování s Xamarin.UITest a Center aplikace

_Součást Xamarin UITest lze s Xamarin.Forms zápis testů uživatelského rozhraní pro spouštění v cloudu na stovky zařízení._

## <a name="overview"></a>Přehled

**[Testovací aplikace Center](/appcenter/test-cloud/)**  umožňuje vývojářům psát testy automatizované uživatelské rozhraní pro systémy iOS a Android. Pomocí některé dílčí vylepšení lze otestovat aplikace Xamarin.Forms pomocí Xamarin.UITest, včetně sdílení stejné testovacího kódu. Tento článek představuje konkrétní tipy, jak Xamarin.UITest práce s Xamarin.Forms.

Tato příručka předpokládá, že znalost Xamarin.UITest. Obeznámení se s Xamarin.UITest doporučujeme následující příručky:

- [Úvod do aplikace Center testu](/appcenter/test-cloud/)
- [Úvod do UITest](/appcenter/test-cloud/preparing-for-upload/uitest/)

Jakmile se projekt UITest byl přidán do řešení Xamarin.Forms, kroky pro zápis a a spouštění testů pro aplikaci Xamarin.Forms jsou stejné jako aplikace Xamarin.Android nebo Xamarin.iOS.

## <a name="requirements"></a>Požadavky

Odkazovat [Xamarin.UITest](/appcenter/test-cloud/uitest/) potvrďte projektu je připraven pro automatizované testování uživatelského rozhraní.

## <a name="adding-uitest-support-to-xamarinforms-apps"></a>Přidání podpory UITest na platformě Xamarin.Forms aplikace

UITest automatizuje uživatelské rozhraní aktivace ovládacích prvků na obrazovce a provedením kdekoli vstup uživatele by za normálních okolností pracovat s aplikací. Chcete-li povolit testy, které můžete *klikněte tlačítko* nebo *zadejte text v poli* testovacího kódu budou potřebovat způsob, jak identifikovat ovládacích prvků na obrazovce.

Pokud chcete povolit UITest kód, který odkazuje ovládací prvky, musí každý ovládací prvek jedinečný identifikátor. V Xamarin.Forms, doporučeným způsobem, jak nastavit tento identifikátor je pomocí `AutomationId` vlastnost, jak je uvedeno níže:

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

`AutomationId` Vlastnost lze nastavit i v XAML:

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

Jedinečný `AutomationId` musí být přidaní do všech ovládacích prvků, které jsou požadovány pro testování (včetně tlačítek, text položky a popisky, jehož hodnota možná muset zadat dotaz).

> [!NOTE]
> Všimněte si, že `InvalidOperationException` bude vyvolána, pokud je proveden pokus o nastavení `AutomationId` vlastnost `Element` více než jednou.

### <a name="ios-application-project"></a>Projekt aplikace iOS

Ke spuštění testů v systému iOS, [balíček Xamarin Test Cloud agenta NuGet](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/) musí být přidaný do projektu. Jakmile byl přidán, zkopírujte následující kód do `AppDelegate.FinishedLaunching` metoda:

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

Sestavení Calabash díky použití neveřejný Apple rozhraní API což způsobí, že aplikace budou odmítnuti v obchodu App Store. Ale linkeru Xamarin.iOS odebere Calabash sestavení z poslední soubor IPA Pokud není výslovně odkazována z kódu.

> [!NOTE]
>  Verze sestavení nemají `ENABLE_TEST_CLOUD` proměnnou kompilátoru, což způsobí Calabash sestavení, které má být odebrán ze sady prostředků aplikace. Sestavení pro ladění ale mají kompilátoru definovaný direktivou, brání linkeru odebrání sestavení.

Následující snímek obrazovky ukazuje `ENABLE_TEST_CLOUD` kompilátoru proměnná nastavená pro sestavení pro ladění:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "Možnosti sestavení")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "Možnosti kompilátoru")

-----

### <a name="android-application-project"></a>Projekt aplikace pro Android

Na rozdíl od iOS Android projekty není nutné žádné speciální spuštění kódu.

## <a name="writing-uitests"></a>Zápis UITests

Informace o vytváření UITests najdete v tématu [UITest dokumentaci](/appcenter/test-cloud/preparing-for-upload/uitest/). Následující kroky jsou určeny souhrn, konkrétně popisující, jak [Xamarin.Forms ukázku **UsingUITest** ](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/) je sestaven.

### <a name="use-automationid-in-the-xamarinforms-ui"></a>Použití AutomationId v uživatelském rozhraní Xamarin.Forms

Před žádné UITests lze zapisovat, uživatelské rozhraní aplikace Xamarin.Forms musí být možné používat skripty. Zajistěte, aby měly všechny ovládací prvky v uživatelském rozhraní `AutomationId` tak, aby se může být odkazováno v testovací kód.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="adding-a-uitest-project-to-a-new-solution"></a>Přidání UITest projektu do nové řešení

Při vytváření nového projektu Xamarin.Forms pomocí sady Visual Studio pro Mac, můžete nový projekt UITest přidán do řešení výběrem **Xamarin Test Cloud: přidejte automatizované projekt testu uživatelského rozhraní,**:

![](uitest-and-test-cloud-images/01-new-solution-xs.png "Konfigurovat nový projekt")

Nové řešení bude automaticky nakonfigurována pro spuštění Xamarin.UITests vzhledem k aplikaci Xamarin.Forms.

-----

#### <a name="referring-to-the-automationid-in-uitests"></a>Odkazy na AutomationId v UITests

Při zápisu UITests, `AutomationId` hodnota je vystaven jinak na jednotlivých platformách:

- **iOS** používá `id` pole.
- **Android** používá `label` pole.

Zápis UITests a platformy, které najdete `AutomationId` na iOS a Android, použijte `Marked` test dotazu:

```csharp
app.Query(c=>c.Marked("MyButton"))
```

Kratší formu `app.Query("MyButton")` funguje taky.

### <a name="adding-a-uitest-project-to-an-existing-solution"></a>Přidání projektu UITest do existujícího řešení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio má šablony můžete přidat do existujícího řešení Xamarin.Forms Xamarin.UITest projektu:

1. Klikněte pravým tlačítkem na řešení a vyberte **soubor > Nový projekt**.
1. Z **Visual C#** šablony, vyberte **Test** kategorie. Vyberte **uživatelského rozhraní testování aplikace > napříč platformami** šablony:

    ![](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "Přidat nový projekt")

    To bude přidat nový projekt pomocí **NUnit**, **Xamarin.UITest**, a **NUnitTestAdapter** balíčky NuGet pro řešení:

    ![](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "Správce balíčků NuGet")

    **NUnitTestAdapter** je test runner třetích stran, který umožňuje sadě Visual Studio spustit testy NUnit ze sady Visual Studio.

    Nový projekt také obsahuje dvě třídy. **AppInitializer** obsahuje kód, abyste inicializovat a nastavit testy. Jiná třída **testy**, obsahuje kód, často používaný ke spuštění UITests.

1. Přidáte odkaz na projekt z projektu UITest do projektu Xamarin.Android:

    ![](uitest-and-test-cloud-images/10-test-apps-vs.png "Správce odkazů projektu")

    To vám umožní **NUnitTestAdapter** ke spuštění UITests pro aplikace pro Android ze sady Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Je možné ručně přidat nový projekt Xamarin.UITest do existujícího řešení:

1. Začněte přidáním nového projektu výběrem řešení a kliknutím na **soubor > Přidat nový projekt**. V **nový projekt** dialogovém okně, vyberte **napříč platformami > testy > Xamarin Test Cloud > testování aplikace uživatelského rozhraní**:

    ![](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "Výběr šablony")

    To bude přidat nový projekt, který už má **NUnit** a **Xamarin.UITest** balíčků NuGet v řešení:

    ![](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "Balíčky NuGet Xamarin UITest")

    Nový projekt také obsahuje dvě třídy. **AppInitializer** obsahuje kód, abyste inicializovat a nastavit testy. Jiná třída **testy**, obsahuje kód, často používaný ke spuštění UITests.

1. Vyberte **zobrazení > dotyková zařízení > testování částí** zobrazíte panelu pro testování částí. Rozbalte položku **UsingUITest > UsingUITest.UITests > testování aplikace**:

    ![](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "Odsazení testů jednotek")

1. Klikněte pravým tlačítkem na **testovací aplikace**, klikněte na **přidejte projekt aplikace**a vyberte iOS a Android projekty v dialogovém okně se zobrazí:

    ![](uitest-and-test-cloud-images/05-add-test-apps-xs.png "Test aplikace – dialogové okno")

    **Testování částí** pad by měl nyní obsahují odkaz na iOS a Android projekty. To vám umožní sady Visual Studio pro Mac pro spuštění testu provést UITests místně proti dva projekty Xamarin.Forms.

#### <a name="adding-uitest-to-the-ios-app"></a>Přidání UITest do aplikace pro iOS

Existují některé další změny, které musí být provedeny pro aplikace systému iOS, než bude pracovat Xamarin.UITest:

1. Přidat **Xamarin Test Cloud Agent** balíček NuGet. Klikněte pravým tlačítkem na **balíčky**, vyberte **přidat balíčky**, NuGet pro vyhledávání **Xamarin Test Cloud Agent** a přidejte ji do projektu Xamarin.iOS:

    ![](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "Přidání balíčků NuGet")

1. Upravit `FinishedLaunching` metodu **AppDelegate** tříd k chybě při inicializaci Xamarin Test Cloud Agent při spuštění aplikace iOS a nastavit `AutomationId` vlastnost zobrazení. `FinishedLaunching` Metoda by měla vypadat podobně jako následující příklad kódu:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
        #if ENABLE_TEST_CLOUD
        Xamarin.Calabash.Start();
        #endif

        global::Xamarin.Forms.Forms.Init();

        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
}
```

-----

Po přidání Xamarin.UITest do řešení Xamarin.Forms, je možné vytvořit UITests spouštět místně a odesílat je na Xamarin Test Cloud.

## <a name="summary"></a>Souhrn

Xamarin.Forms aplikací lze snadno testovat pomocí **Xamarin.UITest** pomocí jednoduchého mechanismus ke zveřejnění `AutomationId` jako zobrazení jedinečný identifikátor pro automatizaci test. Jakmile se projekt UITest byl přidán do řešení Xamarin.Forms, kroky pro zápis a a spouštění testů pro aplikaci Xamarin.Forms jsou stejné jako aplikace Xamarin.Android nebo Xamarin.iOS.

Informace o tom, jak odeslat testů pro Test Center aplikace najdete v tématu [odesílání UITests](/appcenter/test-cloud/preparing-for-upload/uitest/). Další informace o UITest najdete v tématu [Test Center aplikace dokumentaci](/appcenter/test-cloud/).


## <a name="related-links"></a>Související odkazy

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Ukázky Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Testovací aplikace Center](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
