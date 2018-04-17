---
title: Instalační program platformy WPF
description: Xamarin.Forms má nyní preview podporu pro platformu WPF
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 51aad1643709a96c56ccad8187a53f47a65a9dac
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="wpf-platform-setup"></a>Instalační program platformy WPF

![Náhled](~/media/shared/preview.png)

Xamarin.Forms má nyní preview podporu pro Windows Presentation Foundation (WPF). Tento článek ukazuje, jak přidat projekt WPF řešení Xamarin.Forms.

Před začátkem, vytvořte nové řešení Xamarin.Forms ve Visual Studio 2017 nebo použít existující řešení Xamarin.Forms, například [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Aplikace WPF můžete přidat pouze k řešení Xamarin.Forms v systému Windows.

## <a name="adding-a-wpf-app"></a>Přidání aplikace WPF

Postupujte podle těchto pokynů můžete přidat aplikaci WPF, který se spustí na stolní počítače Windows 7, 8 a 10:

1. V Visual Studio 2017, klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a zvolte **Přidat > Nový projekt...** .

2. V **nový projekt** okně na levém vyberte **Visual C#** a **Windows Classic Desktop**. V seznamu typy projektů, vyberte **aplikace WPF (rozhraní .NET Framework)**. 

3. Zadejte název projektu s **WPF** rozšíření, například **BoxViewClock.WPF**. Klikněte **Procházet** tlačítko, vyberte **BoxViewClock** složky a stiskněte klávesu **vyberte složku**. Projekt WPF to bude umístit do stejného adresáře jako ostatní projekty v řešení.

    ![Přidat nový projekt WPF](wpf-images/add-new-project.png "přidat nový projekt WPF")

    Kliknutím na tlačítko OK vytvořte projekt.

4. V **Průzkumníku řešení**, vpravo klikněte na nový **BoxViewClock.WPF** projektu a vyberte **spravovat balíčky NuGet**. Vyberte **Procházet** , klikněte na **zahrnout předběžné verze** zaškrtávací políčko a vyhledejte **Xamarin.Forms**.

    ![Vyberte balíček NuGet](wpf-images/select-nuget-package.png "vyberte balíček NuGet")

    Vyberte, že balíček a klikněte na **nainstalovat** tlačítko.

5. Nyní vyhledejte **Xamarin.Forms.Platform.WPF** balíčku a také jakou jeden nainstalovat. Ujistěte se, že balíček je od společnosti Microsoft!

6. Klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a vyberte **spravovat balíčky NuGet pro řešení**. Vyberte **aktualizace** kartě a **Xamarin.Forms** balíčku. Vybrat všechny projekty a provede jejich aktualizaci na stejnou verzi Xamarin.Forms:

    ![Aktualizovat balíček NuGet](wpf-images/update-nuget-package.png "aktualizovat balíček NuGet") 

7. V projektu WPF, klikněte pravým tlačítkem na **odkazy**. V **správce odkazů** dialogovém okně, vyberte **projekty** na levé straně a zaškrtněte políčka u **BoxViewClock** projektu:

    ![Referenční sdílený projekt](wpf-images/reference-shared-project.png "odkazovat sdílený projekt")

8. Upravit **MainWindow.xaml** souboru projektu WPF. V `Window` značky, přidejte deklarace oboru názvů XML pro **Xamarin.Forms.Platform.WPF** sestavení a oboru názvů:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Nyní změnit `Window` značky k `wpf:FormsApplcationPage`. Změna `Title` nastavení na název vaší aplikace, například **BoxViewClock**. Dokončené souboru XAML by měl vypadat takto:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>
        
        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. Upravit **MainWindow.xaml.cs** souboru projektu WPF. Přidejte dva nové `using` direktivy:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Změňte základní třídu `MainWindow` z `Window` k `FormsApplicationPage`. Následující `InitializeComponent` volat funkci, přidejte následující dva příkazy:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    S výjimkou komentáře a nepoužívané `using` direktivy, kompletní **MainWindows.xaml.cs** soubor by měl vypadat takto:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

10. Klikněte pravým tlačítkem na projekt WPF **Průzkumníku řešení** a vyberte **nastavit jako spouštěný projekt**. Stisknutím klávesy F5 spusťte program pomocí ladicího programu sady Visual Studio na ploše systému Windows:

    ![WPF BoxView hodiny](wpf-images/wpf-boxviewclock.png "hodiny BoxView WPF" )

## <a name="next-steps"></a>Další kroky

### <a name="platform-specifics"></a>Specifika platformy

Můžete určit, jaké platformě Xamarin.Forms aplikace běží na z kódu nebo XAML. To umožňuje změnit vlastnosti program, když je spuštěn na WPF. V kódu porovnat hodnotu `Device.RuntimePlatform` s `Device.WPF` konstanta, (který se rovná řetězec "WPF"). Pokud je nalezena shoda, aplikace běží na WPF.

V jazyce XAML, můžete použít `OnPlatform` značky a vyberte hodnotu vlastnosti specifické pro platformu:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="window-size"></a>Velikost okna

Můžete upravit počáteční velikost okna v WPF **MainWindow.xaml** souboru:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Problémy

Toto je náhled, takže byste měli očekávat, že není vše produkční připraven. Ne všechny balíčky NuGet pro Xamarin.Forms jsou připravené pro grafický subsystém WPF a některé funkce nemusí být plně práce.

