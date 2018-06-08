---
title: Rychlý start Multiobrazovka Xamarin.Forms
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 7d6c52bdee29b99b8a00de8f03d263c6fe528c5e
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847612"
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Rychlý start Multiobrazovka Xamarin.Forms

Tento rychlý start ukazuje, jak rozšířit aplikace Phoneword přidat druhý obrazovky ke sledování historie volání pro aplikaci. Konečné aplikace je zobrazena níže:

[![](quickstart-images/intro-app-examples-sml.png "Aplikace Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikace")

Rozšíření aplikace Phoneword následujícím způsobem:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spusťte sadu Visual Studio. Na úvodní stránce klikněte na tlačítko **otevřeného projektu...** a v **otevřeného projektu** dialogové okno Vybrat soubor řešení pro projekt Phoneword:

    ![](quickstart-images/vs/open-solution.png "Otevřít projekt")

2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword** projektu a vyberte **Přidat > novou položku...** :

    ![](quickstart-images/vs/add-new-item.png "Přidat novou položku")

3. V **přidat novou položku** dialogovém okně, vyberte **Visual C# položky > Xamarin.Forms > obsahu stránce**, název Nová položka **CallHistoryPage**a klikněte na tlačítko **přidat**  tlačítko. Bude přidáno stránku s názvem **CallHistoryPage** do projektu:

    ![](quickstart-images/vs/add-callhistorypage-class.png "Šablony projektů Xamarin.Forms")

4. V **CallHistoryPage.xaml**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje deklarativně uživatelské rozhraní pro stránky:

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

    Uložit změny do **CallHistoryPage.xaml** stisknutím **CTRL + S**a zavřete soubor.

5. V **Průzkumníku řešení**, dvakrát klikněte **App.xaml.cs** soubor sdílený **Phoneword** projektu a otevře se:

    ![](quickstart-images/vs/open-app-class.png "Otevření souboru App.xaml.cs")

6. V **App.xaml.cs**, importovat `System.Collections.Generic` obor názvů, přidejte deklaraci `PhoneNumbers` vlastnost inicializaci vlastnosti `App` konstruktor a inicializovat [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) vlastnost, která má být [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). `PhoneNumbers` Kolekce se použije k uložení seznamu každý přeložený telefonního čísla, která volá aplikaci:

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

    Uložit změny do **App.xaml.cs** stisknutím **CTRL + S**a zavřete soubor.

7. V **Průzkumníku řešení**, dvakrát klikněte **MainPage.xaml** soubor sdílený **Phoneword** projektu a otevře se:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

8. V **MainPage.xaml**, přidejte [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ovládacího prvku na konci [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ovládacího prvku. Tlačítko se použije k přejít na stránku historie volání:

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

    Uložit změny do **MainPage.xaml** stisknutím **CTRL + S**a zavřete soubor.

9. V **Průzkumníku řešení**, dvakrát klikněte na **MainPage.xaml.cs** a otevře se:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Otevřete MainPage.xaml.cs")

10. V **MainPage.xaml.cs**, přidejte `OnCallHistory` obslužná rutina události a upravit `OnCall` obslužná rutina události přidat přeložený telefonní číslo, `App.PhoneNumbers` kolekce a povolit `callHistoryButton`, za předpokladu, že `dialer` proměnná není `null`:

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

    Uložit změny do **MainPage.xaml.cs** stisknutím **CTRL + S**a zavřete soubor.

11. V sadě Visual Studio, vyberte **sestavení > Sestavit řešení** položku nabídky (nebo stiskněte klávesu **CTRL + SHIFT + B**). Aplikace bude sestavení a na stavovém řádku v sadě Visual Studio se zobrazí zpráva o úspěšném provedení:

    ![](quickstart-images/vs/build-successful.png "Sestavení úspěšné")

    Pokud nejsou chyby, opakujte předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.

12. Na panelu nástrojů Visual Studio, stiskněte **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) ke spuštění aplikace:

    ![](quickstart-images/vs/start.png "Panel nástrojů Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikace UWP")

13. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Phoneword.Droid** projektu a vyberte **nastavit jako spouštěný projekt**.
14. Na panelu nástrojů Visual Studio, stiskněte **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v emulátoru Androidu.
15. Pokud máte zařízení s iOS a splňovat požadavky na systém Mac pro vývoj s Xamarin.Forms, použijte k nasazení aplikace na zařízení s iOS podobným způsobem. Můžete taky nasadit aplikaci, která [vzdáleného simulátoru iOS](~/tools/ios-simulator.md).

    Poznámka: telefonní hovory nejsou podporovány na všechny simulátorů.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte sadu Visual Studio for Mac. Na úvodní stránce klikněte na tlačítko **otevřete...** a v dialogovém okně vyberte soubor řešení pro projekt Phoneword:

    ![](quickstart-images/xs/open-solution.png "Otevřít řešení")

2. V **řešení Pad**, vyberte **Phoneword** projektu klikněte pravým tlačítkem a vyberte **Přidat > Nový soubor...** :

    ![](quickstart-images/xs/add-new-file.png "Přidat nový soubor")

3. V **nový soubor** dialogovém okně, vyberte **Forms > Forms ContentPage Xaml**, pojmenujte nový soubor **CallHistoryPage**a klikněte na tlačítko **nový** tlačítko. Bude přidáno stránku s názvem **CallHistoryPage** do projektu:

    ![](quickstart-images/xs/add-callhistorypage-class.png "Přidat ContentPage formulářů")

4. V **řešení Pad**, dvakrát klikněte na **CallHistoryPage.xaml** a otevře se:

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "Otevřete CallHistoryPage.xaml")

5. V **CallHistoryPage.xaml**, odeberte všechny kód šablony a nahraďte ji následujícím kódem. Tento kód definuje deklarativně uživatelské rozhraní pro stránky:

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

    Uložit změny do **CallHistoryPage.xaml** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

6. V **řešení Pad**, dvakrát klikněte na **App.xaml.cs** a otevře se:

    ![](quickstart-images/xs/open-app-class.png "Otevření souboru App.xaml.cs")

7. V **App.xaml.cs**, importovat `System.Collections.Generic` obor názvů, přidejte deklaraci `PhoneNumbers` vlastnost inicializaci vlastnosti `App` konstruktor a inicializovat [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) vlastnost, která má být [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). `PhoneNumbers` Kolekce se použije k uložení seznamu každý přeložený telefonního čísla, která volá aplikaci:

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

    Uložit změny do **App.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

8. V **řešení Pad**, dvakrát klikněte na **MainPage.xaml** a otevře se:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Otevřete MainPage.xaml")

9. V **MainPage.xaml**, přidejte [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ovládacího prvku na konci [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ovládacího prvku. Tlačítko se použije k přejít na stránku historie volání:

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

    Uložit změny do **MainPage.xaml** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

10. V **řešení Pad**, dvakrát klikněte na **MainPage.xaml.cs** a otevře se:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Otevřete MainPage.xaml.cs")

11. V **MainPage.xaml.cs**, přidejte `OnCallHistory` obslužná rutina události a upravit `OnCall` obslužná rutina události přidat přeložený telefonní číslo, `App.PhoneNumbers` kolekce a povolit `callHistoryButton`, za předpokladu, že `dialer` proměnná není `null`:

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

    Uložit změny do **MainPage.xaml.cs** výběrem **soubor > Uložit** (nebo stisknutím kombinace kláves  **&#8984; + S**) a zavřete soubor.

12. V sadě Visual Studio pro Mac, vyberte **sestavení > sestavení všechny** položku nabídky (nebo stiskněte klávesu  **&#8984; + B**). Aplikace bude sestavení a zobrazí se zpráva o úspěšném provedení v sadě Visual Studio pro Mac nástrojů:

    ![](quickstart-images/xs/build-successful.png "Sestavení úspěšné")

    Pokud nejsou chyby, opakujte předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.

13. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v simulátoru iOS:

    ![](quickstart-images/xs/start.png "Visual Studio pro Mac nástrojů")
    ![](quickstart-images/xs/phone-result-ios.png "simulátoru iOS")

    Poznámka: telefonní hovory nejsou podporovány v simulátoru iOS.

14. V **řešení Pad**, vyberte **Phoneword.Droid** projektu klikněte pravým tlačítkem a vyberte **nastavit jako spouštěný projekt**:

    ![](quickstart-images/xs/set-startup-project.png "Nastavit jako spouštěný projekt")

15. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spustíte aplikaci v emulátoru Androidu:

    ![](quickstart-images/xs/phone-result-android.png "Emulátoru systému Android")

    Poznámka: telefonní hovory nejsou podporovány v systému Android emulátorů.

-----

Blahopřejeme k dokončení Multiobrazovka aplikaci Xamarin.Forms. [Dalšího tématu](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) v této příručce zkontroluje kroky, které byly provedeny v tomto návodu k vývoji představu o navigaci na stránce a datová vazba pomocí Xamarin.Forms.


## <a name="related-links"></a>Související odkazy

- [Phoneword (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
