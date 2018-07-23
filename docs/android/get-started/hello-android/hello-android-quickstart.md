---
title: 'Hello, Android: rychlý start'
description: V této příručce dvojdílného se svou první aplikaci Xamarin.Android (pomocí sady Visual Studio nebo Visual Studio for Mac) a vytvořit si představu o základy vývoje pro aplikace pro Android pomocí Xamarinu. Na cestě vám představíme k nástrojům, koncepty a kroky potřebné k sestavení a nasazení aplikace systému Xamarin.Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 44007FA1-3ABC-4935-BF52-4613AF0553A6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/20/2018
ms.openlocfilehash: beb90587e0d720de7770056c8b51264099edecdc
ms.sourcegitcommit: fb55eba393e43bcc9e9d1fef9ef1f1310e99f620
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/21/2018
ms.locfileid: "39189018"
---
# <a name="hello-android-quickstart"></a>Hello, Android: rychlý start

_V této příručce dvojdílného se svou první aplikaci Xamarin.Android (pomocí sady Visual Studio nebo Visual Studio for Mac) a vytvořit si představu o základy vývoje pro aplikace pro Android pomocí Xamarinu. Na cestě vám představíme k nástrojům, koncepty a kroky potřebné k sestavení a nasazení aplikace systému Xamarin.Android._

## <a name="hello-android-quickstart"></a>Hello, Android rychlý start

V tomto návodu vytvoříte aplikaci, která se přeloží alfanumerické telefonní číslo (zadané uživatelem) do číselné telefonní číslo a zobrazit číselné telefonní číslo pro uživatele. Konečná aplikace vypadá takto:

[![Snímek obrazovky aplikace po dokončení](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)


## <a name="requirements"></a>Požadavky

Chcete-li postupovat podle tohoto návodu, budete potřebovat následující:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 nebo novější.

-   Visual Studio 2015 Professional nebo novější.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Nejnovější verzi sady Visual Studio pro Mac.

-   OS X Yosemite nebo vyšší.

-----

Tento názorný průvodce předpokládá, že nejnovější verze Xamarin.Android je nainstalovaná a spuštěná na vybranou platformu. Pokyny k instalaci Xamarin.Android, najdete [instalaci Xamarin.Android](~/android/get-started/installation/index.md) vodítka.
Než začnete, stáhněte a rozbalte [ikony aplikace Xamarin & spuštění obrazovky](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) nastavit.

## <a name="configuring-emulators"></a>Konfigurace emulátorů

Pokud používáte emulátor Androidu, doporučujeme, abyste nakonfigurovali použít hardwarovou akceleraci emulátoru. Pokyny ke konfiguraci hardwarovou akceleraci jsou k dispozici v [hardwarovou akceleraci emulátoru výkonu](~/android/get-started/installation/android-emulator/hardware-acceleration.md).


## <a name="walkthrough"></a>Názorný postup

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Spusťte sadu Visual Studio.  Klikněte na tlačítko **soubor > Nový > projekt** k vytvoření nového projektu.

V **nový projekt** dialogového okna, klikněte na tlačítko **aplikace pro Android** šablony.
Název nového projektu `Phoneword`. Klikněte na tlačítko **OK**:

[![Nový projekt je Phoneword](hello-android-quickstart-images/vs/01-new-project-name-w157-sml.png)](hello-android-quickstart-images/vs/01-new-project-name-w157.png#lightbox)

V **novou aplikaci Android** dialogového okna, klikněte na tlačítko **prázdnou aplikaci** a klikněte na tlačítko **OK** k vytvoření nového projektu:

[![Vyberte šablonu prázdné aplikace](hello-android-quickstart-images/vs/02-blank-app-w157-sml.png)](hello-android-quickstart-images/vs/02-blank-app-w157.png#lightbox)

### <a name="creating-the-layout"></a>Vytváření rozložení

Po vytvoření nového projektu, rozbalte **prostředky** složku a potom **rozložení** složky v **Průzkumníka řešení**.
Dvakrát klikněte na panel **activity_main.axml** ho otevřete v návrháři pro Android. Toto je soubor rozložení pro obrazovku aplikace:

[![Otevřete main.axml aktivity](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png#lightbox)

Z **nástrojů** (oblast na levé straně), zadejte `text` do vyhledávacího pole a přetažením **Text (dlouhodobé používání)** widgetů na návrhovou plochu (oblast ve středu):

[![Přidat pomůcku velký text](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png#lightbox)

S **Text (dlouhodobé používání)** ovládací prvek na návrhové ploše, použijte **vlastnosti** podokně změnit `text` vlastnost **Text (dlouhodobé používání)** widget, který se `Enter a Phoneword:` jak je znázorněno zde:

[![Nastavit vlastnosti velký text](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png#lightbox)

Přetáhněte **prostý Text** widget **nástrojů** návrhu plochu a umístěte ji pod **Text (dlouhodobé používání)** widget:

[![Přidat pomůcku prostý text](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png#lightbox)

S **prostý Text** widgetu vybrané na návrhové ploše, použijte **vlastnosti** podokně změnit `id` vlastnost **prostý Text** widget, který se `@+id/PhoneNumberText`a změnit `text` vlastnost `1-855-XAMARIN`:

[![Nastavení vlastností ve formátu prostého textu](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png#lightbox)

Přetáhněte **tlačítko** z **nástrojů** návrhu plochu a umístěte ji pod **prostý Text** widget:

[![Přeložit přetáhněte tlačítko na návrh](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png#lightbox)

S **tlačítko** vybrané na návrhové ploše, použijte **vlastnosti** podokně změnit `id` vlastnost **tlačítko** k `@+id/TranslateButton` a změňte `text` vlastnost `Translate`:

[![Sada přeložit vlastnosti tlačítka](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png#lightbox)

Přetáhněte **TextView** z **nástrojů** návrhu plochu a umístěte ji v části **tlačítko** widgetu. Nastavte `id` vlastnost **TextView** k `@+id/TranslatedPhoneWord` a změnit `text` na prázdný řetězec:

[![Nastavte vlastnosti pro zobrazení textu.](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png#lightbox)    

Uložte si práci stisknutím kombinace kláves **CTRL + S**.

### <a name="writing-translation-code"></a>Psaní překlad kódu

Dalším krokem je přidání kódu pro převod telefonní čísla z alfanumerické na číselný. Přidání nového souboru do projektu kliknutím pravým tlačítkem myši **Phoneword** projekt **Průzkumníku řešení** podokně a zvolíte **Přidat > Nová položka...**  jak je znázorněno níže:

[![Přidat novou položku](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png#lightbox)

V **přidat novou položku** dialogového okna, vyberte **Visual C# > kód > soubor kódu** a pojmenujte nový soubor kódu **PhoneTranslator.cs**:

[![Přidat PhoneTranslator.cs](hello-android-quickstart-images/vs/14-add-class-sml-w157.png)](hello-android-quickstart-images/vs/14-add-class-w157.png#lightbox)

Tím se vytvoří nové prázdné třídy C#. Do tohoto souboru vložte následující kód:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
                }
                // otherwise we've skipped a non-numeric char
            }
            return newNumber.ToString();
        }
        static bool Contains (this string keyString, char c)
        {
            return keyString.IndexOf(c) >= 0;
        }
        static int? TranslateToNumber(char c)
        {
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Uložit změny do **PhoneTranslator.cs** souboru kliknutím **soubor > Uložit** (nebo stisknutím klávesy **CTRL + S**), pak zavřete soubor.

### <a name="wiring-up-the-interface"></a>Připojení rozhraní

Dalším krokem je přidání kódu propojí vložením pomocného kódu do uživatelského rozhraní `MainActivity` třídy. Začátek podle její nahoru **přeložit** tlačítko. V `MainActivity` třídy, vyhledejte `OnCreate` metody. Dalším krokem je přidání kódu tlačítka uvnitř `OnCreate`níže `base.OnCreate(bundle)` a `SetContentView
(Resource.Layout.Main)` volání. Nejdřív, změnit kód šablony tak, aby `OnCreate` metoda má následující podobu:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // New code will go here
        }
    }
}
```

Získáte odkaz na ovládací prvky, které byly vytvořeny v rozložení souboru přes Android Designer. Přidejte následující kód `OnCreate` po volání metody `SetContentView`:

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Přidejte kód, který odpovídá uživatel stiskne z **přeložit** tlačítko.
Přidejte následující kód, který `OnCreate` – metoda (po řádcích přidali v předchozím kroku):

```csharp
// Add code to translate number
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    string translatedNumber = Core.PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Uložte svou práci tak, že vyberete **soubor > Uložit vše** (nebo stisknutím klávesy **CTRL-SHIFT-S**) a sestavte aplikaci tak, že vyberete **sestavení > znovu sestavit řešení** (nebo stisknutím klávesy **CTRL-SHIFT-B**). 

Pokud chyby existují, projděte si v předchozích krocích a opravte všechny chyby, dokud aplikace sestavena úspěšně. Pokud dojde k chybě sestavení, jako _prostředek neexistuje v aktuálním kontextu_, ověřte, že název oboru názvů v **MainActivity.cs** odpovídá názvu projektu (`Phoneword`) a pak zcela znovu sestavte řešení. Pokud stále docházet k chybám sestavení, ověřte, že jste nainstalovali nejnovější aktualizace Xamarin.Android.

### <a name="setting-the-label-and-app-icon"></a>Nastavení popisku a ikony aplikace

Teď byste měli mít k dispozici funkční aplikaci &ndash; je čas přidat doladění! V **MainActivity.cs**, upravit `Label` pro `MainActivity`. `Label` Je Android zobrazí v horní části obrazovky, chcete-li uživatele informovat o tom, kde jsou v aplikaci.
V horní části `MainActivity` tříd, změnit `Label` k `Phone Word` jak je znázorněno zde:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Nyní je čas nastavit ikonu aplikace. Visual Studio ve výchozím nastavení, poskytne výchozí ikonu pro projekt. Umožňuje odstranit tyto soubory z řešení a nahradit jinou ikonu. Rozbalte **prostředky** složky **oblasti řešení**. Všimněte si, že se pět složek, které mají předponu **mipmap -**, a že každý z těchto složek obsahuje jediný **Icon.png** souboru:

[![Mipmap složek a souborů Icon.png](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png#lightbox)

Je potřeba každý z těchto souborů ikonu Odstranit z projektu. Klikněte pravým tlačítkem myši na všech **Icon.png** souborů a vyberte **odstranit** z místní nabídky:
   
[![Odstranit výchozí Icon.png](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png#lightbox)
   
Klikněte na **odstranit** tlačítko v dialogovém okně.

Dále stáhněte a rozbalte [ikony aplikace Xamarin nastavit](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Tento soubor zip obsahuje ikony pro aplikaci. Jednotlivé ikony jsou vizuálně identické, ale v různých řešení se správně vykresluje na různých zařízeních s hustoty jinou obrazovku.  Sada souborů musí být zkopírován do projektu Xamarin.Android. V sadě Visual Studio v **Průzkumníku řešení**, klikněte pravým tlačítkem myši **mipmap hdpi** a pak zvolte položku **Přidat > existující položky**:

[![Přidání souborů](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png#lightbox)

Z tohoto dialogového okna Výběr přejděte na rozzipovaný directory ikony AdApp Xamarin a otevřete **mipmap hdpi** složky. Vyberte **Icon.png** a klikněte na tlačítko **přidat**.

Tento postup opakujte pro každý z **mipmap -** složek, dokud nebude obsah **mipmap -** složky ikony aplikace Xamarin se zkopírují do jejich protějšky **mipmap -** složek v **Phoneword** projektu.

Po všechny ikony se zkopírují do projektu Xamarin.Android, otevřete **možnosti projektu** dialogové okno kliknutím pravým tlačítkem na projekt v **oblasti řešení**. Vyberte **sestavení > aplikace pro Android** a vyberte **@mipmap/icon** z **ikona aplikace** – pole se seznamem:

[![Nastavení projektu ikonu](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Spuštění aplikace

Nakonec spuštěním na emulátoru nebo zařízení s Androidem a překládá Phoneword testování aplikace:

[![Snímek obrazovky aplikace po dokončení](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Spusťte sadu Visual Studio pro Mac z **aplikací** složky nebo z **Spotlight**. 

Klikněte na tlačítko **nové řešení...**  k vytvoření nového projektu.

V **vybrat šablonu pro nový projekt** dialogového okna, klikněte na tlačítko **Android > aplikace** a vyberte **aplikace pro Android** šablony. Klikněte na tlačítko **Další**.

[![Výběr šablony aplikace pro Android](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png#lightbox)

V **konfigurace aplikace pro Android** dialogového okna, název nové aplikace `Phoneword` a klikněte na tlačítko **Další**.

[![Konfigurace aplikace pro Android](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png#lightbox)

V **konfigurace nové aplikace pro Android** dialogové okno, ponechejte tuto položku názvy řešení a projektu nastavena na `Phoneword` a klikněte na tlačítko **vytvořit** pro vytvoření projektu.

### <a name="creating-the-layout"></a>Vytváření rozložení

Po vytvoření nového projektu, rozbalte **prostředky** složky a pak **rozložení** složky **řešení** panel.
Dvakrát klikněte na panel **Main.axml** ho otevřete v návrháři pro Android. Toto je soubor rozložení pro obrazovku při jeho zobrazení v návrháři pro Android:

[![Otevřete Main.axml](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png#lightbox)

Vyberte **Hello World, kliknutí sem!** **Tlačítko** na návrhové ploše a stiskněte klávesu **odstranit** klíč jeho odstranění. 

Z **nástrojů** (oblast na pravé straně), zadejte `text` do vyhledávacího pole a přetažením **Text (dlouhodobé používání)** widgetů na návrhovou plochu (oblast ve středu):

[![Přidat pomůcku velký text](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png#lightbox)

S **Text (dlouhodobé používání)** widgetu vybrané na návrhové ploše, můžete použít **vlastnosti** panel Chcete-li změnit `Text` vlastnost **Text (velké)** widget, který se `Enter a Phoneword:` jak je znázorněno níže:

[![Nastavit vlastnosti widgetu velký text](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png#lightbox)

V dalším kroku přetáhněte **prostý Text** widget **nástrojů** návrhu plochu a umístěte ji pod **Text (dlouhodobé používání)** widgetu. Všimněte si, že můžete použít vyhledávací pole pro usnadnění vyhledání pomůcek podle názvu:

[![Přidat pomůcku prostý text](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png#lightbox)

S **prostý Text** widgetu vybrané na návrhové ploše, můžete použít **vlastnosti** panel Chcete-li změnit `Id` vlastnost **prostý Text** widget, který se `@+id/PhoneNumberText` a změnit `Text` vlastnost `1-855-XAMARIN`:

[![Nastavit vlastnosti widgetu prostý text](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png#lightbox)

Přetáhněte **tlačítko** z **nástrojů** návrhu plochu a umístěte ji pod **prostý Text** widget:

[![Přidání tlačítka](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png#lightbox)

S **tlačítko** vybrané na návrhové ploše, můžete použít **vlastnosti** panel Chcete-li změnit `Id` vlastnost **tlačítko** k `@+id/TranslateButton` a Změnit `Text` vlastnost `Translate`:

[![Konfigurace tlačítka přeložit](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png#lightbox)

Přetáhněte **TextView** z **nástrojů** návrhu plochu a umístěte ji v části **tlačítko** widgetu. S **TextView** vybrali, nastavte `id` vlastnost **TextView** k `@+id/TranslatedPhoneWord` a změnit `text` na prázdný řetězec:

[![Nastavte vlastnosti pro zobrazení textu.](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png#lightbox)    

Uložte si práci stisknutím kombinace kláves  **&#8984; + S**.

### <a name="writing-translation-code"></a>Psaní překlad kódu

Teď přidejte kód pro převod telefonní čísla z alfanumerické na číselný. Přidání nového souboru do projektu kliknutím na ikonu ozubeného kolečka vedle položky **Phoneword** projekt **řešení** panel a zvolíte **Přidat > Nový soubor...** :

[![Přidání nového souboru do projektu](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png#lightbox)

V **nový soubor** dialogového okna, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **PhoneTranslator**a klikněte na tlačítko **nový**. Tím se vytvoří nové prázdné třídy C# pro nás.

Odebere veškerý kód šablony v nové třídy a nahraďte ho následujícím kódem:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
                }
                // otherwise we've skipped a non-numeric char
            }
            return newNumber.ToString();
        }
        static bool Contains (this string keyString, char c)
        {
            return keyString.IndexOf(c) >= 0;
        }
        static int? TranslateToNumber(char c)
        {
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Uložit změny do **PhoneTranslator.cs** soubor výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**), pak zavřete soubor. Ujistěte se, že nejsou žádné chyby kompilace tím, že znovu sestavit řešení.

### <a name="wiring-up-the-interface"></a>Připojení rozhraní

Dalším krokem je přidání kódu propojí přidáním kódu zálohování do uživatelského rozhraní `MainActivity` třídy.
Dvakrát klikněte na panel **MainActivity.cs** v **oblasti řešení** ho otevřete.

Začněte tím, že obslužná rutina události pro přidání **přeložit** tlačítko. V `MainActivity` třídy, vyhledejte `OnCreate` metody. Přidejte tlačítko kód `OnCreate`níže `base.OnCreate(bundle)` a `SetContentView (Resource.Layout.Main)` volání. Odeberte všechny existující tlačítko kód pro zpracování (například kód, který odkazuje `Resource.Id.myButton` a pro ni vytvoří obslužnou rutinu kliknutí) tak, aby `OnCreate` metoda má následující podobu:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Our code will go here
        }
    }
}
```

V dalším kroku je potřeba odkaz na ovládací prvky, které byly vytvořeny v rozložení souboru se Android Designer. Přidejte následující kód `OnCreate` – metoda (po volání `SetContentView`):

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Přidejte kód, který odpovídá uživatel stiskne z **přeložit** tlačítko přidáním následujícího kódu `OnCreate` – metoda (po řádcích přidali v předchozím kroku):

```csharp
// Add code to translate number
string translatedNumber = string.Empty;

translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Uložte si práci a sestavení aplikace tak, že vyberete **sestavení > sestavení všechny** (nebo stisknutím klávesy  **&#8984; + B**). Pokud aplikaci zkompiluje, zobrazí se zpráva o úspěchu v horní části sady Visual Studio pro Mac:

Pokud chyby existují, projděte si v předchozích krocích a opravte všechny chyby, dokud aplikace sestavena úspěšně. Pokud dojde k chybě sestavení, jako _prostředek neexistuje v aktuálním kontextu_, ověřte, že název oboru názvů v **MainActivity.cs** odpovídá názvu projektu (`Phoneword`) a pak zcela znovu sestavte řešení. Pokud stále neobdržíte chyby sestavení, ověřte, že jste si nainstalovali nejnovější Xamarin.Android a aktualizace nástroje Visual Studio pro Mac.

### <a name="setting-the-label-and-app-icon"></a>Nastavení popisku a ikony aplikace

Teď, když máte funkční aplikaci, je čas na přidání doladění! Začněte tím, že úpravy `Label` pro `MainActivity`.
`Label` Je Android zobrazí v horní části obrazovky, chcete-li uživatele informovat o tom, kde jsou v aplikaci. V horní části `MainActivity` tříd, změnit `Label` k `Phone Word` jak je znázorněno zde:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Nyní je čas nastavit ikonu aplikace. Ve výchozím nastavení bude Visual Studio for Mac poskytovat výchozí ikonu pro projekt. Umožňuje odstranit tyto soubory z řešení a nahradit jinou ikonu. Rozbalte **prostředky** složky **oblasti řešení**. Všimněte si, že se pět složek, které mají předponu **mipmap -**, a že každý z těchto složek obsahuje jediný **Icon.png** souboru:

[![Mipmap složek a souborů Icon.png](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png#lightbox)

Je potřeba každý z těchto souborů ikonu Odstranit z projektu. Klikněte pravým tlačítkem myši na všech **Icon.png** souborů a vyberte **odebrat** z místní nabídky:

[![Odstranit výchozí Icon.png](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png#lightbox)

Klikněte na **odstranit** tlačítko v dialogovém okně.

Dále stáhněte a rozbalte [ikony aplikace Xamarin nastavit](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Tento soubor zip obsahuje ikony pro aplikaci. Jednotlivé ikony jsou vizuálně identické, ale v různých řešení se správně vykresluje na různých zařízeních s hustoty jinou obrazovku.  Sada souborů musí být zkopírován do projektu Xamarin.Android. V sadě Visual Studio pro Mac v **oblasti řešení**, klikněte pravým tlačítkem myši **mipmap hdpi** a pak zvolte položku **Přidat > Přidat soubory**:

[![Přidání souborů](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png#lightbox)

Z tohoto dialogového okna Výběr přejděte na rozzipovaný directory ikony AdApp Xamarin a otevřete **mipmap hdpi** složky. Vyberte **Icon.png** a klikněte na tlačítko **otevřít**.

V **přidat soubor do složky** dialogu **zkopírujte soubor do adresáře** a klikněte na tlačítko **OK**:

[![Zkopírujte soubor do adresáře dialogového okna](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png#lightbox)

Tento postup opakujte pro každý z **mipmap -** složek, dokud nebude obsah **mipmap -** složky ikony aplikace Xamarin se zkopírují do jejich protějšky **mipmap -** složek v **Phoneword** projektu.

Po všechny ikony se zkopírují do projektu Xamarin.Android, otevřete **možnosti projektu** dialogové okno kliknutím pravým tlačítkem na projekt v **oblasti řešení**. Vyberte **sestavení > aplikace pro Android** a vyberte **@mipmap/icon** z **ikona aplikace** – pole se seznamem:

[![Nastavení projektu ikonu](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Spuštění aplikace

Nakonec spuštěním na emulátoru nebo zařízení s Androidem a překládá Phoneword testování aplikace:

[![Snímek obrazovky aplikace po dokončení](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

-----

Blahopřejeme k dokončení vaší první aplikace Xamarin.Android!
Nyní je čas dissect nástroje a dovednosti, které jste právě zjistili. Dále je až [Hello, Android podrobné informace o](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>Související odkazy

- [Ikony aplikace Xamarin Android (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (ukázka)](https://developer.xamarin.com/samples/monodroid/Phoneword)
