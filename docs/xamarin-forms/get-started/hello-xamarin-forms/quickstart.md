---
title: Rychlý start Xamarin.Forms
description: Tento článek vysvětluje, jak vytvořit aplikaci, která znamená, že alfanumerické telefonní číslo, zadané uživatelem do číselné telefonní číslo a číslo, který volá.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 3a1da095559b4030332f4b7cc74b4f8f94fb1c61
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268872"
---
# <a name="xamarinforms-quickstart"></a>Rychlý start Xamarin.Forms

Tento návod ukazuje, jak vytvořit aplikaci, která znamená, že alfanumerické telefonní číslo, zadané uživatelem do číselné telefonní číslo a číslo, který volá. Konečné aplikace je zobrazena níže:

[![](quickstart-images/intro-app-examples-sml.png "Aplikace Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikace")

Vytvoření aplikace Phoneword následujícím způsobem:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **spustit** obrazovky, spusťte sadu Visual Studio. Otevře se stránka start:

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. V sadě Visual Studio, klikněte na tlačítko **vytvořit nový projekt...**  k vytvoření nového projektu:

    ![](quickstart-images/vs/new-solution.png "Nový projekt")

3. V **nový projekt** dialogové okno, klikněte na tlačítko **napříč platformami**, vyberte **mobilní aplikace (Xamarin.Forms)** šablony, nastavte název název a řešení na `Phoneword`, vyberte vhodné umístění projektu a klikněte na **OK** tlačítko:

    ![](quickstart-images/vs/new-project.w157.png "Šablony projektů a platformy")

4. V **novou aplikaci křížové platformy** dialogové okno, klikněte na tlačítko **prázdnou aplikaci**, vyberte **.NET Standard** strategie sdílení kódu a klikněte na **OK** tlačítko:

    ![](quickstart-images/vs/new-app.png "Novou aplikaci pro různé platformy")

5. V **Průzkumníku řešení**v **Phoneword** projektu, klikněte dvakrát na **MainPage.xaml** a otevře se:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

6. V **MainPage.xaml**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje deklarativně uživatelské rozhraní pro stránky:

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

    Uložit změny do **MainPage.xaml** stisknutím **CTRL + S**a zavřete soubor.

7. V **Průzkumníku řešení**, rozbalte položku **MainPage.xaml** a dvakrát klikněte na **MainPage.xaml.cs** a otevře se:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Otevřete MainPage.xaml.cs")

8. V **MainPage.xaml.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. `OnTranslate` a `OnCall` metody bude proveden v reakci **přeložit** a **volání** tlačítka klepnutí v uživatelském rozhraní, v uvedeném pořadí:

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
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
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
    > Při pokusu o vytvoření aplikace v tomto okamžiku bude mít za následek chyby, které budou později opravit.

    Uložit změny do **MainPage.xaml.cs** stisknutím **CTRL + S**a zavřete soubor.

9. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > novou položku...** :

    ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

10. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > kódu > třída**, název nový soubor **PhoneTranslator**a klikněte na tlačítko **přidat** tlačítko :

    ![](quickstart-images/vs/add-translator-class.w157.png "Přidejte novou třídu")

11. V **PhoneTranslator.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód se přeloží slovo phone na telefonní číslo:

    ```csharp
    using System.Text;

    namespace Core
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

    Uložit změny do **PhoneTranslator.cs** stisknutím **CTRL + S**a zavřete soubor.

12. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > novou položku...** :

    ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

13. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > kódu > rozhraní**, pojmenujte nový soubor **IDialer**a klikněte na tlačítko **přidat** tlačítko:

    ![](quickstart-images/vs/add-idialer-interface.w157.png "Přidejte nové rozhraní")

14. V **IDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje `Dial` metodu, která musí být implementována na jednotlivých platformách k vytočení přeložený telefonní číslo:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    Uložit změny do **IDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

    > [!NOTE]
    > Společný kód aplikace je nyní dokončen. Kód programu Telefon specifické pro platformu phone se teď implementuje jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.iOS** projektu a vyberte **Přidat > novou položku...** :

    ![](quickstart-images/vs/add-new-item-ios.png "Přidat novou položku")

16. V **přidat novou položku** dialogovém okně, vyberte **Apple > kódu > třída**, název nový soubor **telefon.dokument**a klikněte na tlačítko **přidat** tlačítko:

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "Přidejte novou třídu")

17. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří <code>Dial</code> metoda, která se použije na platformě iOS k vytočení přeložený telefonní číslo:

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

    Uložit změny do **PhoneDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

18. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.Android** projektu a vyberte **Přidat > novou položku...** :

    ![](quickstart-images/vs/add-new-item-android.png "Přidat novou položku")

19. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > Android > třída**, název nový soubor **telefon.dokument**a klikněte na tlačítko **přidat** tlačítko:

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "Přidejte novou třídu")

20. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda, která se použije na platformě Android k vytočení přeložený telefonní číslo:

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

    Uložit změny do **PhoneDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

21. V **Průzkumníku řešení**v **Phoneword.Android** projektu, klikněte dvakrát na **MainActivity.cs** otevřete ho odebrat všechny kód šablony a nahraďte ho Následující kód:

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

    Uložit změny do **MainActivity.cs** stisknutím **CTRL + S**a zavřete soubor.

22. V **Průzkumníku řešení**v **Phoneword.Android** projektu, klikněte dvakrát na **vlastnosti** a vyberte **Android Manifest** karty:

    ![](quickstart-images/vs/android-manifest.png "Otevřete manifestu systému Android.")

23. V **požadovaná oprávnění** povolte **CALL_PHONE** oprávnění. To dává oprávnění aplikace umístit telefonní hovor:

    ![](quickstart-images/vs/android-manifest-changed.png "Povolit CallPhone oprávnění")

    Uložit změny do manifestu stisknutím **CTRL + S**a zavřete soubor.

24. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.UWP** projektu a vyberte **Přidat > novou položku...** :

    ![](quickstart-images/vs/add-new-item-uwp.png "Přidat novou položku")

25. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > kódu > třída**, název nový soubor **telefon.dokument**a klikněte na tlačítko **přidat** tlačítko:

    ![](quickstart-images/vs/new-phone-dialer-uwp.w157.png "Přidejte novou třídu")

26. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda a pomocné metody, které se použijí k vytočení přeložený telefonní číslo na univerzální platformu Windows:

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

    Uložit změny do **PhoneDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

27. V **Průzkumníku řešení**v **Phoneword.UWP** projektu, klikněte pravým tlačítkem na **odkazy**a vyberte **přidat odkaz na...** :

    ![](quickstart-images/vs/uwp-add-reference.png "Přidání odkazu")

28. V **správce odkazů** dialogovém okně, vyberte **Universal Windows > Rozšíření > rozšíření Windows Mobile pro UPW**a klikněte na tlačítko **OK** tlačítko:

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "Přidání mobilní rozšíření Windows pro UPW")

29. V **Průzkumníku řešení**v **Phoneword.UWP** projektu, klikněte dvakrát na **Package.appxmanifest**:

    ![](quickstart-images/vs/uwp-manifest.png "Otevřete UWP Manifest")

30. V **možnosti** povolte **telefonní hovor** schopností. To dává oprávnění aplikace umístit telefonní hovor:

    ![](quickstart-images/vs/uwp-manifest-changed.png "Povolení schopností telefonního hovoru")

    Uložit změny do manifestu stisknutím **CTRL + S**a zavřete soubor.

31. V sadě Visual Studio, vyberte **sestavení > Sestavit řešení** položku nabídky (nebo stiskněte klávesu **CTRL + SHIFT + B**). Aplikace bude sestavení a na stavovém řádku v sadě Visual Studio se zobrazí zpráva o úspěšném provedení:

    ![](quickstart-images/vs/build-successful.png "Sestavení úspěšné")

    Pokud nejsou chyby, opakujte předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.

32. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.UWP** projektu a vyberte **nastavit jako spouštěný projekt**:

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "Nastavit jako spouštěný projekt")

33. Na panelu nástrojů Visual Studio, stiskněte **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) ke spuštění aplikace:

    ![](quickstart-images/vs/start.png "Panel nástrojů Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikace UWP")

34. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.Android** projektu a vyberte **nastavit jako spouštěný projekt**.
35. Na panelu nástrojů Visual Studio, stiskněte **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v emulátoru Androidu.
36. Pokud máte zařízení s iOS a splňovat požadavky na systém Mac pro vývoj s Xamarin.Forms, použijte k nasazení aplikace na zařízení s iOS podobným způsobem. Můžete taky nasadit aplikaci, která [vzdáleného simulátoru iOS](~/tools/ios-simulator.md).

    Poznámka: telefonní hovory nejsou podporovány na všechny simulátorů.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte Visual Studio pro Mac a na úvodní stránce klikněte na tlačítko **nový projekt...**  k vytvoření nového projektu:

    ![](quickstart-images/xs/new-solution.png "Nové řešení")

2. V **vyberte šablonu pro nový projekt** dialogové okno, klikněte na tlačítko **Multiplatform > aplikace**, vyberte **prázdnou aplikaci Forms** šablonu a klikněte na tlačítko **Další** tlačítko:

    ![](quickstart-images/xs/choose-template.png "Výběr šablony")

3. V **konfigurace aplikace prázdné formuláře** dialogové okno, název nové aplikace `Phoneword`, ujistěte se, že **Standard .NET použijte** přepínač je vybrána a klikněte na **Další** tlačítko:

    ![](quickstart-images/xs/configure-app.png "Konfigurace aplikace formulářů")

4. V **konfigurace nové prázdné formuláře aplikace** dialogové okno, ponechte název řešení a projektu nastavena na `Phoneword`, vyberte vhodný umístění projektu a klikněte **vytvořit** tlačítko vytvořte projektu:

    ![](quickstart-images/xs/configure-project.png "Konfigurace projektu formulářů")

5. V **řešení Pad**, dvakrát klikněte na **MainPage.xaml** a otevře se:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

6. V **MainPage.xaml**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje deklarativně uživatelské rozhraní pro stránky:

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

    Uložit změny do **MainPage.xaml** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

7. V **řešení Pad**, dvakrát klikněte na **MainPage.xaml.cs** a otevře se:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Otevřete MainPage.xaml.cs")

8. V **MainPage.xaml.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. `OnTranslate` a `OnCall` metody bude proveden v reakci **přeložit** a **volání** tlačítka klepnutí na uživatelské rozhraní v uvedeném pořadí:

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
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
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
    > Při pokusu o vytvoření aplikace v tomto okamžiku bude mít za následek chyby, které budou později opravit.

    Uložit změny do **MainPage.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

9. V **řešení Pad**, vyberte **Phoneword** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-translator-file.png "Přidat nový soubor")

10. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **PhoneTranslator**a klikněte na tlačítko **nový** tlačítko:

    ![](quickstart-images/xs/add-translator-class.png "Přidejte novou třídu")

11. V **PhoneTranslator.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód se přeloží slovo phone na telefonní číslo:

    ```csharp
    using System.Text;

    namespace Core
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

    Uložit změny do **PhoneTranslator.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

12. V **řešení Pad**, vyberte **Phoneword** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-interface.png "Přidat nový soubor")

13. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdného rozhraní**, pojmenujte nový soubor **IDialer**a klikněte na tlačítko **nový** tlačítko:

    ![](quickstart-images/xs/add-idialer-interface.png "Přidejte nové rozhraní")

14. V **IDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje `Dial` metodu, která musí být implementována na jednotlivých platformách k vytočení přeložený telefonní číslo:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    Uložit změny do **IDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

    > [!NOTE]
    > Společný kód aplikace je nyní dokončen. Kód programu Telefon specifické pro platformu phone se teď implementuje jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. V **řešení Pad**, vyberte **Phoneword.iOS** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-file-ios.png "Přidat nový soubor")

16. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **telefon.dokument**a klikněte na tlačítko **nový** tlačítko:

    ![](quickstart-images/xs/new-phonedialer-ios.png "Přidejte novou třídu")

17. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda, která se použije na platformě iOS k vytočení přeložený telefonní číslo:

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

    Uložit změny do **PhoneDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

18. V **řešení Pad**, vyberte **Phoneword.Droid** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-file-android.png "Přidat nový soubor")

19. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **telefon.dokument**a klikněte na tlačítko **nový** tlačítko:

    ![](quickstart-images/xs/new-phonedialer-android.png "Přidejte novou třídu")

20. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda, která se použije na platformě Android k vytočení přeložený telefonní číslo:

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

    Uložit změny do **PhoneDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

21. V **řešení Pad**v **Phoneword.Droid** projektu, klikněte dvakrát na **MainActivity.cs** otevřete ho odebrat všechny kód šablony a nahraďte ji následujícím kódem :

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

    Uložit změny do **MainActivity.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

22. V **řešení Pad**, rozbalte **vlastnosti** složku a dvojím kliknutím **AndroidManifest.xml** souboru:

    ![](quickstart-images/xs/android-manifest.png "Otevřete manifestu systému Android.")

23. V **požadovaná oprávnění** povolte **CallPhone** oprávnění. To dává oprávnění aplikace umístit telefonní hovor:

    ![](quickstart-images/xs/android-manifest-changed.png "Povolit CallPhone oprávnění")

    Uložit změny do **AndroidManifest.xml** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

24. V sadě Visual Studio pro Mac, vyberte **sestavení > sestavení všechny** položku nabídky (nebo stiskněte klávesu  **&#8984; + B**). Aplikace bude sestavení a zobrazí se zpráva o úspěšném provedení v sadě Visual Studio pro Mac panelu nástrojů.

    ![](quickstart-images/xs/build-successful.png "Sestavení úspěšné")

25. Pokud nejsou chyby, opakujte předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.
26. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v simulátoru iOS:

    ![](quickstart-images/xs/start.png "Visual Studio pro Mac nástrojů")
    ![](quickstart-images/xs/phoneword-result-ios.png "simulátoru iOS")

    Poznámka: telefonní hovory nejsou podporovány v simulátoru iOS.

27. V **řešení Pad**, vyberte **Phoneword.Droid** projektu klikněte pravým tlačítkem a vyberte **nastavit jako spouštěný projekt**:

    ![](quickstart-images/xs/set-startup-project.png "Nastavit jako spouštěný projekt")

28. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v emulátoru Androidu:

    ![](quickstart-images/xs/phoneword-result-android.png "Emulátoru systému Android")

    Poznámka: telefonní hovory nejsou podporovány v systému Android emulátorů.

-----

Blahopřejeme k dokončení Xamarin.Forms aplikace. [Dalšího tématu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) v této příručce zkontroluje kroky, které byly provedeny v tomto názorném postupu získáte informace o základní informace o vývoj aplikací pomocí Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Přístup k nativní funkce prostřednictvím DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
