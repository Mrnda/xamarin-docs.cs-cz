---
title: Rychlý start s více obrazovkami Xamarin.Forms
description: Tento článek vysvětluje, jak rozšířit Phoneword aplikaci tak, že přidáte druhou obrazovku ke sledování historie volání pro aplikaci.
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: a4e27f1810a16b5d13838d2e2c1067950586fab3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996178"
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Rychlý start s více obrazovkami Xamarin.Forms

Tento rychlý start ukazuje, jak rozšířit Phoneword aplikaci tak, že přidáte druhou obrazovku ke sledování historie volání pro aplikaci. Konečná aplikace je zobrazena níže:

[![](quickstart-images/intro-app-examples-sml.png "Aplikace Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikace")

Rozšiřuje možnosti aplikace Phoneword následujícím způsobem:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spusťte sadu Visual Studio. Na úvodní stránce klikněte na tlačítko **otevřít projekt...** a **otevřít projekt** dialogové okno Vybrat soubor řešení pro projekt Phoneword:

    ![](quickstart-images/vs/open-solution.png "Otevřít projekt")

2. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > Nová položka...** :

    ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

3. V **přidat novou položku** dialogového okna, vyberte **položky Visual C# > Xamarin.Forms > stránka obsahu**, pojmenujte novou položku **CallHistoryPage**a klikněte na tlačítko **přidat**  tlačítko. Tím přidáte na stránku s názvem **CallHistoryPage** do projektu:

    ![](quickstart-images/vs/add-callhistorypage-class.png "Šablony projektu Xamarin.Forms")

4. V **CallHistoryPage.xaml**, odeberte všechny šablony kód a nahraďte následujícím kódem. Tento kód definuje deklarativně uživatelského rozhraní pro stránky:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>
    ```

    Uložit změny do **CallHistoryPage.xaml** stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

5. V **Průzkumníka řešení**, dvakrát klikněte **App.xaml.cs** soubor ve sdílené **Phoneword** projekt a otevře se:

    ![](quickstart-images/vs/open-app-class.png "Otevřete App.xaml.cs.")

6. V **App.xaml.cs**, import `System.Collections.Generic` obor názvů, přidat deklaraci `PhoneNumbers` vlastnost, inicializaci vlastnosti `App` konstruktor a inicializační [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) vlastnost jako [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). `PhoneNumbers` Kolekce se použije k uložení seznamu jednotlivých přeložené telefonní číslo volané aplikace:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Uložit změny do **App.xaml.cs** stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

7. V **Průzkumníka řešení**, dvakrát klikněte **MainPage.xaml** soubor ve sdílené **Phoneword** projekt a otevře se:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

8. V **MainPage.xaml**, přidejte [ `Button` ](xref:Xamarin.Forms.Button) ovládacího prvku na konci [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ovládacího prvku. Tlačítko se použije na stránku historie volání:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Uložit změny do **MainPage.xaml** stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

9. V **Průzkumníka řešení**, dvakrát klikněte na panel **MainPage.xaml.cs** otevřete:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

10. V **MainPage.xaml.cs**, přidejte `OnCallHistory` metoda obslužné rutiny události a upravit `OnCall` přidejte přeložené telefonní číslo, které má metodu obslužné rutiny události `App.PhoneNumbers` kolekce a povolit `callHistoryButton`, za předpokladu, že `dialer` proměnná není `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Uložit změny do **MainPage.xaml.cs** stisknutím kombinace kláves **CTRL + S**a zavřete soubor.

11. V sadě Visual Studio, vyberte **sestavení > Sestavit řešení** položky nabídky (nebo stiskněte klávesu **CTRL + SHIFT + B**). Aplikace se sestaví a zprávu o úspěšném dokončení se zobrazí ve stavovém řádku sady Visual Studio:

    ![](quickstart-images/vs/build-successful.png "Sestavení bylo úspěšné")

    Pokud chyby existují, opakujte předchozí kroky a opravte všechny chyby, dokud aplikace sestavena úspěšně.

12. Na panelu nástrojů sady Visual Studio stiskněte klávesu **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) ke spuštění aplikace:

    ![](quickstart-images/vs/start.png "Panel nástrojů sady Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikace UPW")

13. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Phoneword.Droid** projektu a vyberte **nastavit jako spouštěný projekt**.
14. Na panelu nástrojů sady Visual Studio stiskněte klávesu **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) ke spuštění aplikace v emulátoru Androidu.
15. Pokud máte zařízení s Iosem a splňovat požadavky na systém Mac pro vývoj s Xamarin.Forms, použijte podobné techniky nasazení aplikace do zařízení s Iosem. Můžete také nasadit aplikaci [vzdálený simulátor iOS](~/tools/ios-simulator.md).

    Poznámka: telefonní hovory nejsou podporovány ve všech simulátorů.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte sadu Visual Studio pro Mac. Na úvodní stránce klikněte na tlačítko **otevřít...** a v dialogovém okně vyberte soubor řešení pro projekt Phoneword:

    ![](quickstart-images/xs/open-solution.png "Otevřít řešení")

2. V **oblasti řešení**, vyberte **Phoneword** projektu, klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-file.png "Přidejte nový soubor")

3. V **nový soubor** dialogového okna, vyberte **Forms > Forms ContentPage Xaml**, pojmenujte nový soubor **CallHistoryPage**a klikněte na tlačítko **nový** tlačítko. Tím přidáte na stránku s názvem **CallHistoryPage** do projektu:

    ![](quickstart-images/xs/add-callhistorypage-class.png "Přidat Forms ContentPage")

4. V **oblasti řešení**, dvakrát klikněte na panel **CallHistoryPage.xaml** otevřete:

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "Otevřete CallHistoryPage.xaml")

5. V **CallHistoryPage.xaml**, odeberte všechny šablony kód a nahraďte následujícím kódem. Tento kód definuje deklarativně uživatelského rozhraní pro stránky:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>      
    ```

    Uložit změny do **CallHistoryPage.xaml** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

6. V **oblasti řešení**, dvakrát klikněte na panel **App.xaml.cs** otevřete:

    ![](quickstart-images/xs/open-app-class.png "Otevřete App.xaml.cs.")

7. V **App.xaml.cs**, import `System.Collections.Generic` obor názvů, přidat deklaraci `PhoneNumbers` vlastnost, inicializaci vlastnosti `App` konstruktor a inicializační [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) vlastnost jako [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). `PhoneNumbers` Kolekce se použije k uložení seznamu jednotlivých přeložené telefonní číslo volané aplikace:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Uložit změny do **App.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

8. V **oblasti řešení**, dvakrát klikněte na panel **MainPage.xaml** otevřete:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

9. V **MainPage.xaml**, přidejte [ `Button` ](xref:Xamarin.Forms.Button) ovládacího prvku na konci [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ovládacího prvku. Tlačítko se použije na stránku historie volání:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Uložit změny do **MainPage.xaml** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

10. V **oblasti řešení**, dvakrát klikněte na panel **MainPage.xaml.cs** otevřete:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

11. V **MainPage.xaml.cs**, přidejte `OnCallHistory` metoda obslužné rutiny události a upravit `OnCall` přidejte přeložené telefonní číslo, které má metodu obslužné rutiny události `App.PhoneNumbers` kolekce a povolit `callHistoryButton`, za předpokladu, že `dialer` proměnná není `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Uložit změny do **MainPage.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím klávesy  **&#8984; + S**) a zavřete soubor.

12. V sadě Visual Studio pro Mac, vyberte **sestavení > sestavení všechny** položky nabídky (nebo stiskněte klávesu  **&#8984; + B**). Aplikace se sestaví a zprávu o úspěšném dokončení se zobrazí v sadě Visual Studio pro Mac nástrojů:

    ![](quickstart-images/xs/build-successful.png "Sestavení bylo úspěšné")

    Pokud chyby existují, opakujte předchozí kroky a opravte všechny chyby, dokud aplikace sestavena úspěšně.

13. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) Chcete-li spustit aplikaci v simulátoru iOS:

    ![](quickstart-images/xs/start.png "Visual Studio pro Mac nástrojů")
    ![](quickstart-images/xs/phone-result-ios.png "simulátor iOS")

    Poznámka: telefonní hovory nejsou podporovány v simulátoru iOS.

14. V **oblasti řešení**, vyberte **Phoneword.Droid** projektu, klikněte pravým tlačítkem a vyberte **nastavit jako spouštěný projekt**:

    ![](quickstart-images/xs/set-startup-project.png "Nastavit jako spouštěný projekt")

15. V aplikaci Visual Studio pro Mac nástrojů, stiskněte **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) ke spuštění aplikace v emulátoru Androidu:

    ![](quickstart-images/xs/phone-result-android.png "Emulátor androidu")

    Poznámka: telefonní hovory nejsou podporovány v emulátory Androidu.

-----

Blahopřejeme k dokončení aplikace Xamarin.Forms s více obrazovkami. [Dalším tématu](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) v této příručce kontroly kroky, které byly provedeny v tomto názorném průvodci k vývoji znalosti o navigaci stránkami a datové vazby pomocí Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Phoneword (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
