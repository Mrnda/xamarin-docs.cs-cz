---
title: Rychlý start Xamarin.Forms
description: Tento článek vysvětluje, jak vytvořit aplikaci, která se přeloží alfanumerické telefonní číslo, zadané uživatelem na číselné telefonní číslo a číslo, který volá.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 5b5f8c80e49d66ed3bd8b008c975d1cfeda93ed4
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38832381"
---
# <a name="xamarinforms-quickstart"></a>Rychlý start Xamarin.Forms

Tento návod ukazuje, jak vytvořit aplikaci, která se přeloží alfanumerické telefonní číslo, zadané uživatelem na číselné telefonní číslo a číslo, který volá. Konečná aplikace je zobrazena níže:

[![](quickstart-images/intro-app-examples-sml.png "Aplikace Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikace")

Vytvoření aplikace Phoneword následujícím způsobem:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **Start** obrazovky, spusťte sadu Visual Studio. Tím se otevře úvodní stránku:

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. V sadě Visual Studio, klikněte na tlačítko **vytvořit nový projekt...**  k vytvoření nového projektu:

    ![](quickstart-images/vs/new-solution.png "Nový projekt")

3. V **nový projekt** dialogového okna, klikněte na tlačítko **Cross-Platform**, vyberte **mobilní aplikace (Xamarin.Forms)** šablony, nastavte název na **Phoneword**, zvolte vhodné umístění pro projekt a klikněte na tlačítko **OK** tlačítka:

    ![](quickstart-images/vs/new-project.w157.png "Šablony projektu pro různé platformy")

    > [!NOTE]
    > Pojmenujte řešení selhání **Phoneword** způsobí mnoho chyb sestavení.

4. V **nové aplikace pro různé platformy** dialogového okna, klikněte na tlačítko **prázdnou aplikaci pro**vyberte **.NET Standard** jako strategii sdílení kódu a klikněte na tlačítko **OK** tlačítko:

    ![](quickstart-images/vs/new-app.png "Nová aplikace pro víc platforem")

5. V **Průzkumníka řešení**v **Phoneword** projektu, klikněte dvakrát na **MainPage.xaml** otevřete:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

6. V **MainPage.xaml**, odeberte všechny šablony kód a nahraďte následujícím kódem. Tento kód definuje deklarativně uživatelského rozhraní pro stránky:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Uložit změny do **MainPage.xaml** stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

7. V **Průzkumníka řešení**, rozbalte **MainPage.xaml** a dvakrát klikněte na panel **MainPage.xaml.cs** otevřete:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

8. V **MainPage.xaml.cs**, odeberte všechny šablony kód a nahraďte následujícím kódem. `OnTranslate` a `OnCall` metody se spustí v reakci **přeložit** a **volání** tlačítka klepnutí v uživatelském rozhraní, v uvedeném pořadí:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Pokus o sestavení aplikace v tomto okamžiku způsobí chyby, které budou opraveny později.

    Uložit změny do **MainPage.xaml.cs** stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

9. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > Nová položka...** :

    ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

10. V **přidat novou položku** dialogového okna, vyberte **Visual C# > kód > třída**, pojmenujte nový soubor **PhoneTranslator**a klikněte na tlačítko **přidat** tlačítko :

    ![](quickstart-images/vs/add-translator-class.w157.png "Přidejte novou třídu")

11. V **PhoneTranslator.cs**, odeberte všechny šablony kód a nahraďte následujícím kódem. Tento kód se přeloží do telefonu slova, aby telefonní číslo:

    ```csharp
    using System.Text;

    namespace Phoneword
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    V **konfigurace prázdná formulářová aplikace** dialogového okna, název nové aplikace **Phoneword**, ujistěte se, že pomocí .NET Standard přepínací tlačítko zaškrtnuto a klikněte na tlačítko Další tlačítka:

12. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > Nová položka...** :

    ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

13. Konfigurace aplikace Forms

    ![](quickstart-images/vs/add-idialer-interface.w157.png "Přidat nové rozhraní")

14. Konfigurace projektu formuláře Selhání název řešení a projektu `Dial`Phoneword způsobí mnoho chyb sestavení.

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    V **oblasti řešení**, dvakrát klikněte na panel **MainPage.xaml** otevřete:

    > [!NOTE]
    > Uložit změny do MainPage.xaml výběrem soubor > Uložit (nebo stisknutím klávesy  &#8984; + S) a zavřete soubor. V [oblasti řešení](~/xamarin-forms/app-fundamentals/dependency-service/index.md), dvakrát klikněte na panel MainPage.xaml.cs otevřete:

15. **a** metody se spustí v reakci **přeložit** a **volání** tlačítka klepnutí na uživatelské rozhraní v uvedeném pořadí:

    ![](quickstart-images/vs/add-new-item-ios.png "Přidat novou položku")

16. Uložit změny do **MainPage.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "Přidejte novou třídu")

17. V **oblasti řešení**, vyberte Phoneword projektu, klikněte pravým tlačítkem a vyberte Přidat > Nový soubor... : Tento kód vytvoří <code>Dial</code> metodu, která bude použita na platformě iOS k vytočení přeloženého telefonního čísla:

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    V **nový soubor** dialogového okna, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor PhoneTranslatora klikněte na tlačítko nový tlačítka:

18. Uložit změny do **PhoneTranslator.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

    ![](quickstart-images/vs/add-new-item-android.png "Přidat novou položku")

19. V **nový soubor** dialogového okna, vyberte **Obecné > prázdné rozhraní**, pojmenujte nový soubor **IDialer**a klikněte na tlačítko **nový** tlačítka:

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "Přidejte novou třídu")

20. V **oblasti řešení**, vyberte Phoneword projektu, klikněte pravým tlačítkem a vyberte Přidat > Nový soubor... : Uložit změny do `Dial`IDialer.cs výběrem soubor > Uložit (nebo stisknutím klávesy  &#8984; + S) a zavřete soubor.

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionDial);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    V **nový soubor** dialogového okna, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor PhoneTranslatora klikněte na tlačítko nový tlačítka:

21. V **oblasti řešení**, vyberte **Phoneword.iOS** projektu, klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);
                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }
    ```

    Uložit změny do **MainActivity.cs** stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

22. V **Průzkumníka řešení**v **Phoneword.Android** projektu, klikněte dvakrát na **vlastnosti** a vyberte **Manifest v Androidu** kartu:

    ![](quickstart-images/vs/android-manifest.png "Otevřít Manifest Android")

23. V **požadovaná oprávnění** povolte **CALL_PHONE** oprávnění. Díky tomu oprávnění k aplikaci umístit telefonního hovoru:

    ![](quickstart-images/vs/android-manifest-changed.png "Povolit CallPhone oprávnění")

    Uložte změny do manifestu stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

24. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Phoneword.UWP** projektu a vyberte **Přidat > Nová položka...** :

    ![](quickstart-images/vs/add-new-item-uwp.png "Přidat novou položku")

25. V **přidat novou položku** dialogového okna, vyberte **Visual C# > kód > třída**, pojmenujte nový soubor **telefon.dokument**a klikněte na tlačítko **přidat** tlačítka:

    ![](quickstart-images/vs/new-phone-dialer-uwp.w157.png "Přidejte novou třídu")

26. V **oblasti řešení**, vyberte Phoneword projektu, klikněte pravým tlačítkem a vyberte Přidat > Nový soubor... : Tento kód vytvoří `Dial` metoda a pomocné metody, které budou použity na univerzální platformu Windows na přeložený telefonní číslo:

    ```csharp
    using Phoneword.UWP;
    using System;
    using System.Threading.Tasks;
    using Windows.ApplicationModel.Calls;
    using Windows.UI.Popups;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.UWP
    {
        public class PhoneDialer : IDialer
        {
            bool dialled = false;

            public bool Dial(string number)
            {
                DialNumber(number);
                return dialled;
            }

            async Task DialNumber(string number)
            {
                var phoneLine = await GetDefaultPhoneLineAsync();
                if (phoneLine != null)
                {
                    phoneLine.Dial(number, number);
                    dialled = true;
                }
                else
                {
                    var dialog = new MessageDialog("No line found to place the call");
                    await dialog.ShowAsync();
                    dialled = false;
                }
            }

            async Task<PhoneLine> GetDefaultPhoneLineAsync()
            {
                var phoneCallStore = await PhoneCallManager.RequestStoreAsync();
                var lineId = await phoneCallStore.GetDefaultLineAsync();
                return await PhoneLine.FromIdAsync(lineId);
            }
        }
    }
    ```

    V **nový soubor** dialogového okna, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor PhoneTranslatora klikněte na tlačítko nový tlačítka:

27. V **Průzkumníka řešení**v **Phoneword.UWP** projektu, klikněte pravým tlačítkem na **odkazy**a vyberte **přidat odkaz...** :

    ![](quickstart-images/vs/uwp-add-reference.png "Přidat odkaz")

28. V **správce odkazů** dialogového okna, vyberte **Universal Windows > Rozšíření > Windows Mobile rozšíření pro UPW**a klikněte na tlačítko **OK** tlačítka:

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "Přidat mobilní rozšíření Windows pro UPW")

29. V **Průzkumníka řešení**v **Phoneword.UWP** projektu, klikněte dvakrát na **Package.appxmanifest**:

    ![](quickstart-images/vs/uwp-manifest.png "Otevření manifestu UPW")

30. V **možnosti** stránce, povolte **telefonního hovoru** funkce. Díky tomu oprávnění k aplikaci umístit telefonního hovoru:

    ![](quickstart-images/vs/uwp-manifest-changed.png "Povolení možnosti telefonního hovoru")

    Uložte změny do manifestu stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

31. V sadě Visual Studio, vyberte **sestavení > Sestavit řešení** položky nabídky (nebo stiskněte klávesu **CTRL + SHIFT + B**). Aplikace se sestaví a zprávu o úspěšném dokončení se zobrazí ve stavovém řádku sady Visual Studio:

    ![](quickstart-images/vs/build-successful.png "Sestavení bylo úspěšné")

    Pokud chyby existují, opakujte předchozí kroky a opravte všechny chyby, dokud aplikace sestavena úspěšně.

32. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Phoneword.UWP** projektu a vyberte **nastavit jako spouštěný projekt**:

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "Nastavit jako spouštěný projekt")

33. Na panelu nástrojů sady Visual Studio stiskněte klávesu **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) ke spuštění aplikace:

    ![](quickstart-images/vs/start.png "Panel nástrojů sady Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikace UPW")

34. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Phoneword.Android** projektu a vyberte **nastavit jako spouštěný projekt**.
35. Na panelu nástrojů sady Visual Studio stiskněte klávesu **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) ke spuštění aplikace v emulátoru Androidu.
36. Pokud máte zařízení s Iosem a splňovat požadavky na systém Mac pro vývoj s Xamarin.Forms, použijte podobné techniky nasazení aplikace do zařízení s Iosem. Můžete také nasadit aplikaci [vzdálený simulátor iOS](~/tools/ios-simulator.md).

    Poznámka: telefonní hovory nejsou podporovány ve všech simulátorů.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte sadu Visual Studio pro Mac a na úvodní stránce klikněte na tlačítko **nový projekt...**  k vytvoření nového projektu:

    ![](quickstart-images/xs/new-solution.png "Nové řešení")

2. V **vybrat šablonu pro nový projekt** dialogového okna, klikněte na tlačítko **Multiplatformní > aplikace**, vyberte **prázdnou aplikaci pro formuláře** šablonu a klikněte na tlačítko **Další** tlačítka:

    ![](quickstart-images/xs/choose-template.png "Výběr šablony")

3. V **konfigurace prázdná formulářová aplikace** dialogového okna, název nové aplikace **Phoneword**, ujistěte se, že **pomocí .NET Standard** přepínací tlačítko zaškrtnuto a klikněte na tlačítko **Další** tlačítka:

    ![](quickstart-images/xs/configure-app.png "Konfigurace aplikace Forms")

4. V **konfigurovat nová prázdná formulářová aplikace** dialogové okno, ponechejte tuto položku názvy řešení a projektu nastavena na **Phoneword**, zvolte vhodné umístění pro projekt a klikněte na tlačítko **vytvořit**tlačítko pro vytvoření projektu:

    ![](quickstart-images/xs/configure-project.png "Konfigurace projektu formuláře")

    > [!NOTE]
    > Selhání název řešení a projektu **Phoneword** způsobí mnoho chyb sestavení.

5. V **oblasti řešení**, dvakrát klikněte na panel **MainPage.xaml** otevřete:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

6. V **MainPage.xaml**, odeberte všechny šablony kód a nahraďte následujícím kódem. Tento kód definuje deklarativně uživatelského rozhraní pro stránky:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Uložit změny do **MainPage.xaml** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

7. V **oblasti řešení**, dvakrát klikněte na panel **MainPage.xaml.cs** otevřete:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

8. V **MainPage.xaml.cs**, odeberte všechny šablony kód a nahraďte následujícím kódem. `OnTranslate` a `OnCall` metody se spustí v reakci **přeložit** a **volání** tlačítka klepnutí na uživatelské rozhraní v uvedeném pořadí:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Pokus o sestavení aplikace v tomto okamžiku způsobí chyby, které budou opraveny později.

    Uložit změny do **MainPage.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

9. V **oblasti řešení**, vyberte **Phoneword** projektu, klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-translator-file.png "Přidejte nový soubor")

10. V **nový soubor** dialogového okna, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **PhoneTranslator**a klikněte na tlačítko **nový** tlačítka:

    ![](quickstart-images/xs/add-translator-class.png "Přidejte novou třídu")

11. V **PhoneTranslator.cs**, odeberte všechny šablony kód a nahraďte následujícím kódem. Tento kód se přeloží do telefonu slova, aby telefonní číslo:

    ```csharp
    using System.Text;

    namespace Phoneword
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    Uložit změny do **PhoneTranslator.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

12. V **oblasti řešení**, vyberte **Phoneword** projektu, klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-interface.png "Přidejte nový soubor")

13. V **nový soubor** dialogového okna, vyberte **Obecné > prázdné rozhraní**, pojmenujte nový soubor **IDialer**a klikněte na tlačítko **nový** tlačítka:

    ![](quickstart-images/xs/add-idialer-interface.png "Přidat nové rozhraní")

14. Konfigurace projektu formuláře Selhání název řešení a projektu `Dial`Phoneword způsobí mnoho chyb sestavení.

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    Uložit změny do **IDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

    > [!NOTE]
    > Uložit změny do MainPage.xaml výběrem soubor > Uložit (nebo stisknutím klávesy  &#8984; + S) a zavřete soubor. V [oblasti řešení](~/xamarin-forms/app-fundamentals/dependency-service/index.md), dvakrát klikněte na panel MainPage.xaml.cs otevřete:

15. V **oblasti řešení**, vyberte **Phoneword.iOS** projektu, klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-file-ios.png "Přidejte nový soubor")

16. V **nový soubor** dialogového okna, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **telefon.dokument**a klikněte na tlačítko **nový** tlačítka:

    ![](quickstart-images/xs/new-phonedialer-ios.png "Přidejte novou třídu")

17. V **oblasti řešení**, vyberte Phoneword projektu, klikněte pravým tlačítkem a vyberte Přidat > Nový soubor... : Tento kód vytvoří `Dial` metodu, která bude použita na platformě iOS k vytočení přeloženého telefonního čísla:

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    Uložit změny do **PhoneDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

18. V **oblasti řešení**, vyberte **Phoneword.Droid** projektu, klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-file-android.png "Přidejte nový soubor")

19. V **nový soubor** dialogového okna, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **telefon.dokument**a klikněte na tlačítko **nový** tlačítka:

    ![](quickstart-images/xs/new-phonedialer-android.png "Přidejte novou třídu")

20. V **oblasti řešení**, vyberte Phoneword projektu, klikněte pravým tlačítkem a vyberte Přidat > Nový soubor... : Uložit změny do `Dial`IDialer.cs výběrem soubor > Uložit (nebo stisknutím klávesy  &#8984; + S) a zavřete soubor.

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionDial);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    Uložit změny do **PhoneDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

21. V **oblasti řešení**v **Phoneword.Droid** projektu, klikněte dvakrát na **MainActivity.cs** Pokud chcete soubor otevřít, odeberte všechny kód šablony a nahraďte ji následujícím kódem :

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);
                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }
    ```

    Uložit změny do **MainActivity.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

22. V **oblasti řešení**, rozbalte **vlastnosti** složky a dvojím kliknutím **AndroidManifest.xml** souboru:

    ![](quickstart-images/xs/android-manifest.png "Otevřít Manifest Android")

23. V **požadovaná oprávnění** povolte **CallPhone** oprávnění. Díky tomu oprávnění k aplikaci umístit telefonního hovoru:

    ![](quickstart-images/xs/android-manifest-changed.png "Povolit CallPhone oprávnění")

    Uložit změny do **AndroidManifest.xml** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

24. V sadě Visual Studio pro Mac, vyberte **sestavení > sestavení všechny** položky nabídky (nebo stiskněte klávesu  **&#8984; + B**). Aplikace se sestaví a zprávu o úspěšném dokončení se zobrazí v sadě Visual Studio pro Mac nástrojů.

    ![](quickstart-images/xs/build-successful.png "Sestavení bylo úspěšné")

25. Pokud chyby existují, opakujte předchozí kroky a opravte všechny chyby, dokud aplikace sestavena úspěšně.
26. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) Chcete-li spustit aplikaci v simulátoru iOS:

    ![](quickstart-images/xs/start.png "Visual Studio pro Mac nástrojů")
    ![](quickstart-images/xs/phoneword-result-ios.png "simulátor iOS")

    Poznámka: telefonní hovory nejsou podporovány v simulátoru iOS.

27. V **oblasti řešení**, vyberte **Phoneword.Droid** projektu, klikněte pravým tlačítkem a vyberte **nastavit jako spouštěný projekt**:

    ![](quickstart-images/xs/set-startup-project.png "Nastavit jako spouštěný projekt")

28. V aplikaci Visual Studio pro Mac nástrojů, stiskněte **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) ke spuštění aplikace v emulátoru Androidu:

    ![](quickstart-images/xs/phoneword-result-android.png "Emulátor androidu")

    Poznámka: telefonní hovory nejsou podporovány v emulátory Androidu.

-----

Blahopřejeme k dokončení aplikace Xamarin.Forms. [Dalším tématu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) v této příručce kontroly kroky, které byly provedeny v tomto názorném postupu získáte informace o základní informace o vývoji aplikací pomocí Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Přístup k nativním funkcím prostřednictvím DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
