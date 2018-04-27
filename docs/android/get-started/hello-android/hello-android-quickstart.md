---
title: 'Hello, Android: rychlý start'
description: V této příručce dvě části bude vytvoření vaší první aplikace Xamarin.Android (pomocí sady Visual Studio nebo Visual Studio pro Mac) a vytvořit si představu základy vývoje aplikace pro Android pomocí Xamarinu. Na této cestě bude nutné zavést nástrojů, koncepty a kroky potřebné k sestavení a nasazení aplikace pro Xamarin.Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 44007FA1-3ABC-4935-BF52-4613AF0553A6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 44c3e4b0f05526560ff4b32808ba476110ce5e8f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="hello-android-quickstart"></a>Hello, Android: rychlý start

_V této příručce dvě části bude vytvoření vaší první aplikace Xamarin.Android (pomocí sady Visual Studio nebo Visual Studio pro Mac) a vytvořit si představu základy vývoje aplikace pro Android pomocí Xamarinu. Na této cestě bude nutné zavést nástrojů, koncepty a kroky potřebné k sestavení a nasazení aplikace pro Xamarin.Android._

## <a name="hello-android-quickstart"></a>Hello, Android rychlý start

V tomto návodu vytvoříte aplikaci, která znamená, že alfanumerické telefonní číslo (zadaná uživatelem) do číselné telefonní číslo a zobrazit číselné telefonní číslo uživatele. Konečné aplikace vypadá takto:

[![Snímek obrazovky aplikace po dokončení](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)


## <a name="requirements"></a>Požadavky

Podle společně s Tento názorný postup, budete potřebovat následující:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 nebo novější.

-   Visual Studio 2015 Professional nebo novější.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Nejnovější verze sady Visual Studio for Mac.

-   OS X Yosemite nebo novější.

-----

Tento návod předpokládá, že je nainstalovaná a spuštěná na vaši platformu volba nejnovější verzi Xamarin.Android. Příručka k instalaci Xamarin.Android, najdete v části [Xamarin.Android instalace](~/android/get-started/installation/index.md) příručky.
Než začnete, stáhněte a rozbalte [ikony aplikace Xamarin & Spustit obrazovky](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) nastavit.

## <a name="configuring-emulators"></a>Konfigurace emulátorů

Pokud používáte emulátor Google Android SDK, doporučujeme nakonfigurovat emulátoru použít hardwarovou akceleraci. Pokyny ke konfiguraci hardwarovou akceleraci jsou k dispozici v [hardwarovou akceleraci](~/android/get-started/installation/android-emulator/hardware-acceleration.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud používáte Android emulátor sady Visual Studio, technologie Hyper-V musí být povolen v počítači. Další informace o konfiguraci Android emulátor sady Visual Studio najdete v tématu [systémové požadavky pro emulátor sady Visual Studio pro Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-----

## <a name="walkthrough"></a>Návod

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Spuštění sady Visual Studio.  Klikněte na tlačítko **soubor > Nový > projekt** k vytvoření nového projektu.

V **nový projekt** dialogové okno, klikněte **prázdná aplikace (Android)** šablony.
Název nového projektu `Phoneword`. Klikněte na tlačítko **OK** k vytvoření nového projektu:

[![Nový projekt je Phoneword](hello-android-quickstart-images/vs/02-new-project-name-sml.png)](hello-android-quickstart-images/vs/02-new-project-name.png#lightbox)

### <a name="creating-the-layout"></a>Vytváření rozložení

Po vytvoření nového projektu, rozbalte **prostředky** složku a potom **rozložení** složky v **Průzkumníku řešení**.
Klikněte dvakrát na **Main.axml** otevřít v Návrháři Android. Toto je soubor rozložení obrazovky aplikace:

[![Otevřete Main.axml](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png#lightbox)

Z **sada nástrojů** (oblast na levé straně), zadejte `text` do pole hledání a přetažení **Text (velká)** pomůcky na návrhovou plochu (oblast v centru):

[![Přidat pomůcku velké textu](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png#lightbox)

S **Text (velká)** ovládací prvek na návrhovou plochu, použijte **vlastnosti** podokně změnit `text` vlastnost **Text (velká)** pomůcka k `Enter a Phoneword:` jak je vidět tady:

[![Nastavení vlastností velké textu](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png#lightbox)

Přetáhněte **prostý Text** widget **sada nástrojů** na návrh surface a umístěte ji pod **Text (velká)** pomůcky:

[![Přidat pomůcku prostý text](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png#lightbox)

S **prostý Text** pomůcky vybrané na návrhovou plochu, použijte **vlastnosti** podokně změnit `id` vlastnost **prostý Text** pomůcky na `@+id/PhoneNumberText`a změňte `text` vlastnost `1-855-XAMARIN`:

[![Nastavení vlastností ve formátu prostého textu](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png#lightbox)

Přetáhněte **tlačítko** z **sada nástrojů** na návrh surface a umístěte ji pod **prostý Text** pomůcky:

[![Přetáhněte převede tlačítko návrhu](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png#lightbox)

S **tlačítko** vybrané na návrhovou plochu, použijte **vlastnosti** podokně změnit `id` vlastnost **tlačítko** k `@+id/TranslateButton` a změňte `text` vlastnost `Translate`:

[![Sada převede vlastnosti tlačítka](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png#lightbox)

Přetáhněte **TextView** z **sada nástrojů** na návrh surface a umístěte ji pod **tlačítko** pomůcky. Nastavte `id` vlastnost **TextView** k `@+id/TranslatedPhoneWord` a změňte `text` na prázdný řetězec:

[![Nastavte vlastnosti v zobrazení textu.](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png#lightbox)    

Uložte si práci, stisknutím klávesy **CTRL + S**.

### <a name="writing-translation-code"></a>Psaní překlad kódu

Dalším krokem je přidání kód, který převede telefonní čísla z alfanumerické na číselnou. Přidat nový soubor na projekt kliknete pravým tlačítkem **Phoneword** v projektu **Průzkumníku řešení** podokně a výběr **Přidat > novou položku...**  jak je uvedeno níže:

[![Přidat novou položku](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png#lightbox)

V **přidat novou položku** dialogovém okně, vyberte **Visual C# > kódu** a název nového souboru kódu **PhoneTranslator.cs**:

[![Přidat PhoneTranslator.cs](hello-android-quickstart-images/vs/14-add-class-sml.png)](hello-android-quickstart-images/vs/14-add-class.png#lightbox)

Tím se vytvoří novou prázdnou C# třídu. Vložte následující kód do tohoto souboru:

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

Uložit změny do **PhoneTranslator.cs** souboru kliknutím **soubor > Uložit** (nebo stisknutím kombinace kláves **CTRL + S**), pak zavřete soubor.

### <a name="wiring-up-the-interface"></a>Připojení rozhraní

Dalším krokem je přidání kódu k propojit se uživatelské rozhraní vložením kódu zálohování do `MainActivity` třídy. Začněte kabeláž až **přeložit** tlačítko. V `MainActivity` třídy, vyhledejte `OnCreate` metoda. Dalším krokem je přidání kód tlačítko uvnitř `OnCreate`, níže `base.OnCreate(bundle)` a `SetContentView
(Resource.Layout.Main)` volání. Nejprve upravit kód šablony tak, aby `OnCreate` metoda vypadá zhruba takto:

```csharp
using Android.App;
using Android.OS;
using Android.Widget;
using Core;

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

Získáte odkaz na ovládací prvky, které byly vytvořeny v souboru rozložení pomocí návrháře Android. Přidejte následující kód do `OnCreate` po zavolání metody `SetContentView`:

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Přidejte kód, který reaguje na stisknutí uživatele z **přeložit** tlačítko.
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

Uložte si práci, výběrem **soubor > Uložit vše** (nebo stisknutím kombinace kláves **CTRL-SHIFT-S**) a sestavte aplikaci tak, že vyberete **sestavení > znovu sestavit řešení** (nebo stisknutím kombinace kláves **CTRL-SHIFT-B**). 

Pokud nejsou chyby, všechny předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace. Pokud dojde k chybě sestavení jako je třeba _prostředek neexistuje v aktuálním kontextu_, ověřte, že název oboru názvů v **MainActivity.cs** odpovídá názvu projektu (`Phoneword`) a potom úplně znovu sestavte řešení. Pokud bude stále docházet k chybám sestavení, ověřte, zda jsou nainstalovány nejnovější aktualizace Xamarin.Android.

### <a name="setting-the-label-and-app-icon"></a>Nastavení popisku a ikona aplikace

Teď byste měli mít funkční aplikaci &ndash; je třeba přidat všechny změny! V **MainActivity.cs**, upravit `Label` pro `MainActivity`. `Label` Je Android zobrazí v horní části obrazovky a upozornit uživatele tam, kde jsou v aplikaci.
V horní části `MainActivity` třídy, změňte `Label` k `Phone Word` jak je vidět tady:

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

Nyní je čas nastavit ikona aplikace. Ve výchozím nastavení Visual Studio poskytne výchozí ikonu pro projekt. Umožňuje odstranit tyto soubory z řešení a nahradíte je vlastní ikonu. Rozbalte položku **prostředky** složku **Pad řešení**. Všimněte si, že jsou pět složky, které mají předponu **mipmap -**, a že každý z těchto složek obsahuje jediný **Icon.png** souboru:

[![složkách Mipmap a Icon.png soubory](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png#lightbox)

Je nutné odstranit všechny tyto soubory ikon z projektu. Klikněte pravým tlačítkem na každý z **Icon.png** soubory a vyberte **odstranit** v místní nabídce:
   
[![Odstranit výchozí Icon.png](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png#lightbox)
   
Klikněte na **odstranit** tlačítka v dialogovém okně.

Další, stáhněte a rozbalte [nastavit ikony aplikace Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Tento soubor zip obsahuje ikony pro aplikaci. Každá ikona je vizuálně identické, ale v různých řešeních vykreslí správně na různých zařízeních s densities – různých obrazovek.  Sadu souborů musí být zkopírován do projektu Xamarin.Android. V sadě Visual Studio v **Průzkumníku řešení**, klikněte pravým tlačítkem myši **mipmap hdpi** složky a vyberte **Přidat > existující položky**:

[![Přidání souborů](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png#lightbox)

Dialogovém okně pro výběr, přejděte do adresáře rozbalené ikony AdApp Xamarin a otevřete **mipmap hdpi** složky. Vyberte **Icon.png** a klikněte na tlačítko **přidat**.

Tento postup opakujte pro každou z **mipmap -** složek, dokud nebude obsah **mipmap -** ikony aplikace Xamarin složky zkopírovaly do jejich protějškem **mipmap -** složek v **Phoneword** projektu.

Po všech ikon se zkopírují do projektu Xamarin.Android, otevřete **možnosti projektu** dialogové okno kliknutím pravým tlačítkem na projekt v **řešení Pad**. Vyberte **sestavení > aplikace pro Android** a vyberte **@mipmap/icon** z **ikona aplikace** – pole se seznamem:

[![Nastavení ikonu projektu](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Spuštění aplikace

Nakonec spuštěním ho na emulátoru nebo zařízení se systémem Android a překladu Phoneword testování aplikace:

[![Snímek obrazovky aplikace po dokončení](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Spusťte sadu Visual Studio pro Mac z **aplikace** složky nebo z **Spotlight**. 

Klikněte na tlačítko **nové řešení...**  k vytvoření nového projektu.

V **vyberte šablonu pro nový projekt** dialogové okno, klikněte na tlačítko **Android > aplikace** a vyberte **aplikace pro Android** šablony. Klikněte na tlačítko **Další**.

[![Výběr šablony aplikace pro Android](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png#lightbox)

V **nakonfigurovat svoji aplikaci pro Android** dialogové okno, název nové aplikace `Phoneword` a klikněte na tlačítko **Další**.

[![Konfigurace aplikace pro Android](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png#lightbox)

V **konfigurace nové aplikace Android** dialogové okno, ponechte název řešení a projektu nastavena na `Phoneword` a klikněte na tlačítko **vytvořit** a vytvořte tak projekt.

### <a name="creating-the-layout"></a>Vytváření rozložení

Po vytvoření nového projektu, rozbalte **prostředky** složku a potom **rozložení** složky v **řešení** odsazení.
Klikněte dvakrát na **Main.axml** otevřít v Návrháři Android. Toto je soubor rozložení pro obrazovky při jeho zobrazení v Návrháři Android:

[![Otevřete Main.axml](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png#lightbox)

Vyberte **Hello World, klikněte na tlačítko Poslat mi!** **Tlačítko** na návrhovou plochu a stiskněte klávesu **odstranit** klíč k jeho odebrání. 

Z **sada nástrojů** (oblast na pravé straně), zadejte `text` do pole hledání a přetažení **Text (velká)** pomůcky na návrhovou plochu (oblast v centru):

[![Přidat pomůcku velké textu](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png#lightbox)

S **Text (velká)** pomůcky vybrané na návrhovou plochu, můžete použít **vlastnosti** pad změnit `Text` vlastnost **Text (velká)** pomůcky na `Enter a Phoneword:` jak je uvedeno níže:

[![Nastavit vlastnosti pomůcky velké text](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png#lightbox)

V dalším kroku přetáhněte **prostý Text** widget **sada nástrojů** na návrh surface a umístěte ji pod **Text (velká)** pomůcky. Všimněte si, že pole hledání můžete použít pro usnadnění vyhledání pomůcky podle názvu:

[![Přidat pomůcku prostý text](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png#lightbox)

S **prostý Text** pomůcky vybrané na návrhovou plochu, můžete použít **vlastnosti** pad změnit `Id` vlastnost **prostý Text** pomůcka k `@+id/PhoneNumberText` a změňte `Text` vlastnost `1-855-XAMARIN`:

[![Nastavit vlastnosti pomůcky prostý text](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png#lightbox)

Přetáhněte **tlačítko** z **sada nástrojů** na návrh surface a umístěte ji pod **prostý Text** pomůcky:

[![Přidání tlačítka](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png#lightbox)

S **tlačítko** vybrané na návrhovou plochu, můžete použít **vlastnosti** pad změnit `Id` vlastnost **tlačítko** k `@+id/TranslateButton` a Změnit `Text` vlastnost `Translate`:

[![Konfigurovat jako přeložit tlačítka](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png#lightbox)

Přetáhněte **TextView** z **sada nástrojů** na návrh surface a umístěte ji pod **tlačítko** pomůcky. S **TextView** vybrána, nastavte `id` vlastnost **TextView** k `@+id/TranslatedPhoneWord` a změňte `text` na prázdný řetězec:

[![Nastavte vlastnosti v zobrazení textu.](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png#lightbox)    

Uložte si práci, stisknutím klávesy  **&#8984; + S**.

### <a name="writing-translation-code"></a>Psaní překlad kódu

Nyní přidáte kód, který převede telefonní čísla z alfanumerické na číselnou. Přidejte do projektu nový soubor kliknutím na ikonu ozubené kolečko vedle možnosti **Phoneword** projektu v **řešení** odsazení a výběr **Přidat > Nový soubor...** :

[![Přidejte do projektu nový soubor](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png#lightbox)

V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **PhoneTranslator**a klikněte na tlačítko **nový**. Tím se vytvoří novou prázdnou C# třídu pro nás.

Odeberte všechny kód šablony v nové třídy a nahraďte ji následujícím kódem:

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

Uložit změny do **PhoneTranslator.cs** souboru tak, že zvolíte **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**), pak zavřete soubor. Ujistěte se, že nejsou žádné chyby kompilace pomocí znovu sestavit řešení.

### <a name="wiring-up-the-interface"></a>Připojení rozhraní

Dalším krokem je přidání kódu k propojit se uživatelské rozhraní tak, že přidáte kód zálohování do `MainActivity` třídy.
Klikněte dvakrát na **MainActivity.cs** v **řešení Pad** ho otevřete.

Začněte tím, že přidání obslužné rutiny události pro **přeložit** tlačítko. V `MainActivity` třídy, vyhledejte `OnCreate` metoda. Přidejte kód tlačítko uvnitř `OnCreate`, níže `base.OnCreate(bundle)` a `SetContentView (Resource.Layout.Main)` volání. Tlačítko šablony kód pro zpracování odebrat tak, aby `OnCreate` metoda vypadá zhruba takto:

```csharp
using Android.App;
using Android.OS;
using Android.Widget;
using Core;

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

Odkaz na další, je potřeba k ovládacích prvků, které byly vytvořeny v souboru rozložení se systémem Android návrháře. Přidejte následující kód do `OnCreate` – metoda (po volání `SetContentView`):

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Přidejte kód, který reaguje na stisknutí uživatele z **přeložit** tlačítko přidáním následující kód, který `OnCreate` – metoda (po řádcích přidali v posledním kroku):

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

Uložte si práci a sestavte aplikaci tak, že vyberete **sestavení > sestavení všechny** (nebo stisknutím kombinace kláves  **&#8984; + B**). Pokud aplikace zkompiluje, zobrazí se zpráva o úspěšném provedení v horní části sady Visual Studio pro Mac:

Pokud nejsou chyby, všechny předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace. Pokud dojde k chybě sestavení jako je třeba _prostředek neexistuje v aktuálním kontextu_, ověřte, že název oboru názvů v **MainActivity.cs** odpovídá názvu projektu (`Phoneword`) a potom úplně znovu sestavte řešení. Pokud bude stále docházet k chybám sestavení, ověřte, že jste nainstalovali nejnovější Xamarin.Android a aktualizace Visual Studio pro Mac.

### <a name="setting-the-label-and-app-icon"></a>Nastavení popisku a ikona aplikace

Teď, když máte funkční aplikaci, je čas přidat všechny změny! Začněte tím, že úpravy `Label` pro `MainActivity`.
`Label` Je Android zobrazí v horní části obrazovky a upozornit uživatele tam, kde jsou v aplikaci. V horní části `MainActivity` třídy, změňte `Label` k `Phone Word` jak je vidět tady:

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

Nyní je čas nastavit ikona aplikace. Ve výchozím nastavení Visual Studio pro Mac poskytne výchozí ikonu pro projekt. Umožňuje odstranit tyto soubory z řešení a nahradíte je vlastní ikonu. Rozbalte položku **prostředky** složku **Pad řešení**. Všimněte si, že jsou pět složky, které mají předponu **mipmap -**, a že každý z těchto složek obsahuje jediný **Icon.png** souboru:

[![složkách Mipmap a Icon.png soubory](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png#lightbox)

Je nutné odstranit všechny tyto soubory ikon z projektu. Klikněte pravým tlačítkem na každý z **Icon.png** soubory a vyberte **odebrat** v místní nabídce:

[![Odstranit výchozí Icon.png](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png#lightbox)

Klikněte na **odstranit** tlačítka v dialogovém okně.

Další, stáhněte a rozbalte [nastavit ikony aplikace Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Tento soubor zip obsahuje ikony pro aplikaci. Každá ikona je vizuálně identické, ale v různých řešeních vykreslí správně na různých zařízeních s densities – různých obrazovek.  Sadu souborů musí být zkopírován do projektu Xamarin.Android. V sadě Visual Studio pro Mac v **řešení Pad**, klikněte pravým tlačítkem myši **mipmap hdpi** složky a vyberte **Přidat > Přidat soubory**:

[![Přidání souborů](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png#lightbox)

Dialogovém okně pro výběr, přejděte do adresáře rozbalené ikony AdApp Xamarin a otevřete **mipmap hdpi** složky. Vyberte **Icon.png** a klikněte na tlačítko **otevřete**.

V **přidat soubor do složky** dialogové okno, vyberte **zkopírujte soubor do adresáře** a klikněte na tlačítko **OK**:

[![Zkopírujte soubor do adresáře dialogového okna](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png#lightbox)

Tento postup opakujte pro každou z **mipmap -** složek, dokud nebude obsah **mipmap -** ikony aplikace Xamarin složky zkopírovaly do jejich protějškem **mipmap -** složek v **Phoneword** projektu.

Po všech ikon se zkopírují do projektu Xamarin.Android, otevřete **možnosti projektu** dialogové okno kliknutím pravým tlačítkem na projekt v **řešení Pad**. Vyberte **sestavení > aplikace pro Android** a vyberte **@mipmap/icon** z **ikona aplikace** – pole se seznamem:

[![Nastavení ikonu projektu](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Spuštění aplikace

Nakonec spuštěním ho na emulátoru nebo zařízení se systémem Android a překladu Phoneword testování aplikace:

[![Snímek obrazovky aplikace po dokončení](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

-----

Blahopřejeme k dokončení vaší první aplikace Xamarin.Android!
Nyní je čas dissect nástroje a dovednosti, kterou jste právě se naučili. Dále je až [Hello, Android podrobné informace](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>Související odkazy

- [Ikony aplikace Xamarin Android (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (ukázka)](https://developer.xamarin.com/samples/monodroid/Phoneword)
