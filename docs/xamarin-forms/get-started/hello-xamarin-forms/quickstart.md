---
title: Xamarin.Forms Quickstart
ms.topic: article
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 7ce674ea38bc847bc9064a5a61113900a45b991d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms Quickstart

Tento návod ukazuje, jak vytvořit aplikaci, která znamená, že alfanumerické telefonní číslo, zadané uživatelem do číselné telefonní číslo a číslo, který volá. Konečné aplikace je zobrazena níže:

[![](quickstart-images/intro-app-examples-sml.png "Aplikace Phoneword")](quickstart-images/intro-app-examples.png "Phoneword aplikace")

Vytvoření aplikace Phoneword následujícím způsobem:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **spustit** obrazovky, spusťte sadu Visual Studio. Otevře se stránka start:

  ![](quickstart-images/vs/start-page.png "Visual Studio")

1. V sadě Visual Studio, klikněte na tlačítko **vytvořit nový projekt...**  k vytvoření nového projektu:

  ![](quickstart-images/vs/new-solution.png "Nový projekt")

1. V **nový projekt** dialogové okno, klikněte na tlačítko **napříč platformami**, vyberte **křížové platformy aplikace (Xamarin.Forms)** šablony, nastavte název název a řešení na `Phoneword`, Vyberte vhodný umístění projektu a klikněte na **OK** tlačítko:

  ![](quickstart-images/vs/new-project.png "Šablony projektů a platformy")

1. V **novou aplikaci křížové platformy** dialogové okno, klikněte na tlačítko **prázdnou aplikaci**, vyberte **Xamarin.Forms** technologií uživatelského rozhraní, vyberte **.NET Standard** jako Strategie sdílení kódu a klikněte na **OK** tlačítko:

  ![](quickstart-images/vs/new-app.png "Novou aplikaci pro různé platformy")

1. V **Průzkumníku řešení**v **Phoneword** projektu, klikněte dvakrát na **MainPage.xaml** a otevře se:

  ![](quickstart-images/vs/open-mainpage-xaml.png "Open MainPage.xaml")

1. V **MainPage.xaml**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje deklarativně uživatelské rozhraní pro stránky:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.MainPage">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <Label Text="Enter a Phoneword:" />
              <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
              <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
              <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
            </StackLayout>
        </ContentPage>

  Uložit změny do **MainPage.xaml** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**, rozbalte položku **MainPage.xaml** a dvakrát klikněte na **MainPage.xaml.cs** a otevře se:

  ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

1. V **MainPage.xaml.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. `OnTranslate` a `OnCall` metody bude proveden v reakci **přeložit** a **volání** tlačítka klepnutí v uživatelském rozhraní, v uvedeném pořadí:

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

  > [!NOTE]
> **Poznámka:**: Probíhá pokus o sestavení aplikace v tomto okamžiku bude mít za následek chyby, které budou později opravit.

  Uložit změny do **MainPage.xaml.cs** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**, rozbalte položku **App.xaml** a dvakrát klikněte na **App.xaml.cs** a otevře se:

  ![](quickstart-images/vs/open-app-class.png "Open App.xaml.cs")

1. V **App.xaml.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. `App` Konstruktor nastaví `MainPage` třídu, jak je stránka, ke které se zobrazí při spuštění aplikace:

        using Xamarin.Forms;
        using Xamarin.Forms.Xaml;

        [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
        namespace Phoneword
        {
            public partial class App : Application
            {
                public App()
                {
                    InitializeComponent();
                    MainPage = new MainPage();
                }

                protected override void OnStart()
                {
                    // Handle when your app starts
                }

                protected override void OnSleep()
                {
                    // Handle when your app sleeps
                }

                protected override void OnResume()
                {
                    // Handle when your app resumes
                }
            }
        }

  Uložit změny do **App.xaml.cs** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > novou položku...** :

  ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

1. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > kódu > třída**, název nový soubor **PhoneTranslator**a klikněte na tlačítko **přidat** tlačítko :

  ![](quickstart-images/vs/add-translator-class.png "Přidejte novou třídu")

1. V **PhoneTranslator.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód se přeloží slovo phone na telefonní číslo:

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

  Uložit změny do **PhoneTranslator.cs** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > novou položku...** :

  ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

1. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > kódu > rozhraní**, pojmenujte nový soubor **IDialer**a klikněte na tlačítko **přidat** tlačítko:

  ![](quickstart-images/vs/add-idialer-interface.png "Přidejte nové rozhraní")

1. V **IDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje `Dial` metodu, která musí být implementována na jednotlivých platformách k vytočení přeložený telefonní číslo:

        namespace Phoneword
        {
            public interface IDialer
            {
                bool Dial(string number);
            }
        }

  Uložit změny do **IDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

  > [!NOTE]
> Společný kód aplikace je nyní dokončen. Kód programu Telefon specifické pro platformu phone se teď implementuje jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.iOS** projektu a vyberte **Přidat > novou položku...** :

  ![](quickstart-images/vs/add-new-item-ios.png "Přidat novou položku")

1. V **přidat novou položku** dialogovém okně, vyberte **Apple > kódu > třída**, název nový soubor **telefon.dokument**a klikněte na tlačítko **přidat** tlačítko:

  ![](quickstart-images/vs/new-phone-dialer-ios.png "Přidejte novou třídu")

1. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří <code>Dial</code> metoda, která se použije na platformě iOS k vytočení přeložený telefonní číslo:

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

  Uložit změny do **PhoneDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.Android** projektu a vyberte **Přidat > novou položku...** :

  ![](quickstart-images/vs/add-new-item-android.png "Přidat novou položku")

1. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > Android > třída**, název nový soubor **telefon.dokument**a klikněte na tlačítko **přidat** tlačítko:

  ![](quickstart-images/vs/new-phone-dialer-android.png "Přidejte novou třídu")

1. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda, která se použije na platformě Android k vytočení přeložený telefonní číslo:

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

                    var intent = new Intent (Intent.ActionCall);
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

  Uložit změny do **PhoneDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**v **Phoneword.Android** projektu, klikněte dvakrát na **MainActivity.cs** otevřete ho odebrat všechny kód šablony a nahraďte ho Následující kód:

        using Android.App;
        using Android.Content.PM;
        using Android.OS;

        namespace Phoneword.Droid
        {
            [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

  Uložit změny do **MainActivity.cs** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**v **Phoneword.Android** projektu, klikněte dvakrát na **vlastnosti** a vyberte **Android Manifest** karty:

  ![](quickstart-images/vs/android-manifest.png "Otevřete manifestu systému Android.")

1. V **požadovaná oprávnění** povolte **CALL_PHONE** oprávnění. To dává oprávnění aplikace umístit telefonní hovor:

  ![](quickstart-images/vs/android-manifest-changed.png "Povolit CallPhone oprávnění")

  Uložit změny do manifestu stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.UWP** projektu a vyberte **Přidat > novou položku...** :

  ![](quickstart-images/vs/add-new-item-uwp.png "Přidat novou položku")

1. V **přidat novou položku** dialogovém okně, vyberte **Visual C# > kódu > třída**, název nový soubor **telefon.dokument**a klikněte na tlačítko **přidat** tlačítko:

  ![](quickstart-images/vs/new-phone-dialer-uwp.png "Přidejte novou třídu")

1. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda a pomocné metody, které se použijí k vytočení přeložený telefonní číslo na univerzální platformu Windows:

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

  Uložit změny do **PhoneDialer.cs** stisknutím **CTRL + S**a zavřete soubor.

1. V **Průzkumníku řešení**v **Phoneword.UWP** projektu, klikněte pravým tlačítkem na **odkazy**a vyberte **přidat odkaz na...** :

  ![](quickstart-images/vs/uwp-add-reference.png "Přidání odkazu")

1. V **správce odkazů** dialogovém okně, vyberte **Universal Windows > Rozšíření > rozšíření Windows Mobile pro UPW**a klikněte na tlačítko **OK** tlačítko:

  ![](quickstart-images/vs/uwp-add-reference-extensions.png "Přidání mobilní rozšíření Windows pro UPW")

1. V **Průzkumníku řešení**v **Phoneword.UWP** projektu, klikněte dvakrát na **Package.appxmanifest**:

  ![](quickstart-images/vs/uwp-manifest.png "Otevřete UWP Manifest")

1. V **možnosti** povolte **telefonní hovor** schopností. To dává oprávnění aplikace umístit telefonní hovor:

  ![](quickstart-images/vs/uwp-manifest-changed.png "Povolení schopností telefonního hovoru")

  Uložit změny do manifestu stisknutím **CTRL + S**a zavřete soubor.

1. V sadě Visual Studio, vyberte **sestavení > Sestavit řešení** položku nabídky (nebo stiskněte klávesu **CTRL + SHIFT + B**). Aplikace bude sestavení a na stavovém řádku v sadě Visual Studio se zobrazí zpráva o úspěšném provedení:

  ![](quickstart-images/vs/build-successful.png "Sestavení úspěšné")

  Pokud nejsou chyby, opakujte předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.UWP** projektu a vyberte **nastavit jako spouštěný projekt**:

  ![](quickstart-images/vs/uwp-set-as-startup-project.png "Nastavit jako spouštěný projekt")

1. Na panelu nástrojů Visual Studio, stiskněte **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) ke spuštění aplikace:

  ![](quickstart-images/vs/start.png "Panel nástrojů Visual Studio")
  ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikace UWP")

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.Android** projektu a vyberte **nastavit jako spouštěný projekt**.
1. Na panelu nástrojů Visual Studio, stiskněte **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v emulátoru Androidu.
1. Pokud máte zařízení s iOS a splňovat požadavky na systém Mac pro vývoj s Xamarin.Forms, použijte k nasazení aplikace na zařízení s iOS podobným způsobem. Můžete taky nasadit aplikaci, která [vzdáleného simulátoru iOS](~/tools/ios-simulator.md).

  Poznámka: telefonní hovory nejsou podporovány na všechny simulátorů.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte Visual Studio pro Mac a na úvodní stránce klikněte na tlačítko **nový projekt...**  k vytvoření nového projektu:

  ![](quickstart-images/xs/new-solution.png "Nové řešení")

1. V **vyberte šablonu pro nový projekt** dialogové okno, klikněte na tlačítko **Multiplatform > aplikace**, vyberte **prázdnou aplikaci Forms** šablonu a klikněte na tlačítko **Další** tlačítko:

  ![](quickstart-images/xs/choose-template.png "Výběr šablony")

1. V **konfigurace prázdné formuláře aplikace** dialogové okno, název nové aplikace `Phoneword`, ujistěte se, že **pomocí přenosné knihovny tříd** přepínače, ujistěte se, že **použití XAML pro uživatele rozhraní soubory** zaškrtávací políčko je zaškrtnuto a klikněte na **Další** tlačítko:

  ![](quickstart-images/xs/configure-app.png "Konfigurace aplikace formulářů")

1. V **konfigurace nové prázdné formuláře aplikace** dialogové okno, ponechte název řešení a projektu nastavena na `Phoneword`, vyberte vhodný umístění projektu a klikněte **vytvořit** tlačítko vytvořte projektu:

  ![](quickstart-images/xs/configure-project.png "Konfigurace projektu formulářů")

1. V **řešení Pad**, vyberte **Phoneword** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

  ![](quickstart-images/xs/add-new-file.png "Přidat nový soubor")

1. V **nový soubor** dialogovém okně, vyberte **Forms > Forms ContentPage Xaml**, pojmenujte nový soubor **MainPage**a klikněte na tlačítko **nový** tlačítko. Bude přidáno stránku s názvem **MainPage** do projektu:

  ![](quickstart-images/xs/add-mainpage-class.png "Přidat nové ContentPage")

1. V **řešení Pad**, dvakrát klikněte na **MainPage.xaml** a otevře se:

  ![](quickstart-images/xs/open-mainpage-xaml.png "Open MainPage.xaml")

1. V **MainPage.xaml**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje deklarativně uživatelské rozhraní pro stránky:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.MainPage">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <Label Text="Enter a Phoneword:" />
              <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
              <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
              <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
            </StackLayout>
        </ContentPage>

  Uložit změny do **MainPage.xaml** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**, dvakrát klikněte na **MainPage.xaml.cs** a otevře se:

  ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

1. V **MainPage.xaml.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. `OnTranslate` a `OnCall` metody bude proveden v reakci **přeložit** a **volání** tlačítka klepnutí na uživatelské rozhraní v uvedeném pořadí:

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

  > [!NOTE]
> **Poznámka:**: Probíhá pokus o sestavení aplikace v tomto okamžiku bude mít za následek chyby, které budou později opravit.

  Uložit změny do **MainPage.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**, dvakrát klikněte na **App.xaml.cs** a otevře se:

  ![](quickstart-images/xs/open-app-class.png "Open App.xaml.cs")

1. V **App.xaml.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. `App` Konstruktor nastaví `MainPage` třídu, jak je stránka, ke které se zobrazí při spuštění aplikace:

        using Xamarin.Forms;
        using Xamarin.Forms.Xaml;

        [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
        namespace Phoneword
        {
            public partial class App : Application
            {
                public App()
                {
                    InitializeComponent();
                    MainPage = new MainPage();
                }

                protected override void OnStart()
                {
                    // Handle when your app starts
                }

                protected override void OnSleep()
                {
                    // Handle when your app sleeps
                }

                protected override void OnResume()
                {
                    // Handle when your app resumes
                }
            }
        }

  Uložit změny do **Phoneword.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**, vyberte **Phoneword** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

  ![](quickstart-images/xs/add-new-translator-file.png "Přidat nový soubor")

1. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **PhoneTranslator**a klikněte na tlačítko **nový** tlačítko:

  ![](quickstart-images/xs/add-translator-class.png "Přidejte novou třídu")

1. V **PhoneTranslator.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód se přeloží slovo phone na telefonní číslo:

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

  Uložit změny do **PhoneTranslator.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**, vyberte **Phoneword** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

  ![](quickstart-images/xs/add-new-interface.png "Přidat nový soubor")

1. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdného rozhraní**, pojmenujte nový soubor **IDialer**a klikněte na tlačítko **nový** tlačítko:

  ![](quickstart-images/xs/add-idialer-interface.png "Přidejte nové rozhraní")

1. V **IDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje `Dial` metodu, která musí být implementována na jednotlivých platformách k vytočení přeložený telefonní číslo:

        namespace Phoneword
        {
            public interface IDialer
            {
                bool Dial(string number);
            }
        }

  Uložit změny do **IDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

  > [!NOTE]
> Společný kód aplikace je nyní dokončen. Kód programu Telefon specifické pro platformu phone se teď implementuje jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

1. V **řešení Pad**, vyberte **Phoneword.iOS** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

  ![](quickstart-images/xs/add-new-file-ios.png "Přidat nový soubor")

1. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **telefon.dokument**a klikněte na tlačítko **nový** tlačítko:

  ![](quickstart-images/xs/new-phonedialer-ios.png "Přidejte novou třídu")

1. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda, která se použije na platformě iOS k vytočení přeložený telefonní číslo:

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

  Uložit změny do **PhoneDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**, vyberte **Phoneword.Droid** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

  ![](quickstart-images/xs/add-new-file-android.png "Přidat nový soubor")

1. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída**, pojmenujte nový soubor **telefon.dokument**a klikněte na tlačítko **nový** tlačítko:

  ![](quickstart-images/xs/new-phonedialer-android.png "Přidejte novou třídu")

1. V **PhoneDialer.cs**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód vytvoří `Dial` metoda, která se použije na platformě Android k vytočení přeložený telefonní číslo:

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

                    var intent = new Intent (Intent.ActionCall);
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

  Uložit změny do **PhoneDialer.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**v **Phoneword.Droid** projektu, klikněte dvakrát na **MainActivity.cs** otevřete ho odebrat všechny kód šablony a nahraďte ji následujícím kódem kód:

        using Android.App;
        using Android.Content.PM;
        using Android.OS;

        namespace Phoneword.Droid
        {
            [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

  Uložit změny do **MainActivity.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**, rozbalte **vlastnosti** složku a dvojím kliknutím **AndroidManifest.xml** souboru:

  ![](quickstart-images/xs/android-manifest.png "Otevřete manifestu systému Android.")

1. V **požadovaná oprávnění** povolte **CallPhone** oprávnění. To dává oprávnění aplikace umístit telefonní hovor:

  ![](quickstart-images/xs/android-manifest-changed.png "Povolit CallPhone oprávnění")

  Uložit změny do **AndroidManifest.xml** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves **&#8984; + S**) a zavřete soubor.

1. V **řešení Pad**, odeberte **PhonewordPage** třídy z **Phoneword** projektu. Tato stránka se automaticky přidáno, jakmile projekt byl vytvořen a už nepotřebujete.
1. V sadě Visual Studio pro Mac, vyberte **sestavení > sestavení všechny** položku nabídky (nebo stiskněte klávesu **&#8984; + B**). Aplikace bude sestavení a zobrazí se zpráva o úspěšném provedení v sadě Visual Studio pro Mac panelu nástrojů.

  ![](quickstart-images/xs/build-successful.png "Sestavení úspěšné")

1. Pokud nejsou chyby, opakujte předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.
1. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v simulátoru iOS:

  ![](quickstart-images/xs/start.png "Visual Studio pro Mac nástrojů")
  ![](quickstart-images/xs/phoneword-result-ios.png "simulátoru iOS")

  Poznámka: telefonní hovory nejsou podporovány v simulátoru iOS.

1. V **řešení Pad**, vyberte **Phoneword.Droid** projektu klikněte pravým tlačítkem a vyberte **nastavit jako spouštěný projekt**:

  ![](quickstart-images/xs/set-startup-project.png "Nastavit jako spouštěný projekt")

1. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v emulátoru Androidu:

  ![](quickstart-images/xs/phoneword-result-android.png "Emulátoru systému Android")

  Poznámka: telefonní hovory nejsou podporovány v systému Android emulátorů.

-----

Blahopřejeme k dokončení Xamarin.Forms aplikace. [Dalšího tématu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) v této příručce zkontroluje kroky, které byly provedeny v tomto názorném postupu získáte informace o základní informace o vývoj aplikací pomocí Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Přístup k nativní funkce prostřednictvím DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)