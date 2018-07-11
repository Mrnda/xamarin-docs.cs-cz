---
title: Instalační program platformě WPF
description: Xamarin.Forms teď nabízí podporu verze preview na platformě WPF
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 416e33f131c6e1ef144608f98964fd8372f454f8
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831316"
---
# <a name="wpf-platform-setup"></a>Instalační program platformě WPF

![Náhled](~/media/shared/preview.png)

Xamarin.Forms teď nabízí podporu verze preview pro Windows Presentation Foundation (WPF). Tento článek ukazuje, jak přidat projekt WPF do řešení Xamarin.Forms.

Před začít, vytvořte nové řešení Xamarin.Forms v sadě Visual Studio 2017 nebo použít existující řešení Xamarin.Forms, třeba [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Aplikace WPF lze přidat pouze do řešení Xamarin.Forms ve Windows.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Přidat do aplikace na platformě Xamarin.Forms pomocí Xamarin.University projekt WPF

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF, podle podporují [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>Přidání aplikace WPF

Postupujte podle těchto pokynů pro přidání aplikace WPF, která se spustí na stolní počítače Windows 7, 8 a 10:

1. V sadě Visual Studio 2017, klikněte pravým tlačítkem na název řešení v **Průzkumníka řešení** a zvolte **Přidat > Nový projekt...** .

2. V **nový projekt** okně na levém vyberte **Visual C#** a **klasická plocha Windows**. V seznamu typů projektů zvolte **aplikace WPF (.NET Framework)**. 

3. Zadejte název projektu s **WPF** příponu, třeba **BoxViewClock.WPF**. Klikněte na tlačítko **Procházet** tlačítka, vyberte **BoxViewClock** složky a stiskněte klávesu **vybrat složku**. To zařadí projekt WPF ve stejném adresáři jako ostatní projekty v řešení.

    ![Přidat nový projekt WPF](wpf-images/add-new-project.png "přidat nový projekt WPF")

    Kliknutím na tlačítko OK vytvořte projekt.

4. V **Průzkumníka řešení**, vpravo klikněte na nový **BoxViewClock.WPF** projektu a vyberte **spravovat balíčky NuGet**. Vyberte **Procházet** klikněte na tlačítko **zahrnout předběžné verze** zaškrtávací políčko a vyhledejte **Xamarin.Forms**.

    ![Vyberte balíček NuGet](wpf-images/select-nuget-package.png "vyberte balíček NuGet")

    Vyberte, že balíček a klikněte na tlačítko **nainstalovat** tlačítko.

5. Nyní vyhledejte **Xamarin.Forms.Platform.WPF** balíček a také, že jedna instalace. Ujistěte se, že balíček je od Microsoftu!

6. Klikněte pravým tlačítkem na název řešení v **Průzkumníka řešení** a vyberte **spravovat balíčky NuGet pro řešení**. Vyberte **aktualizace** kartu a **Xamarin.Forms** balíčku. Vybrat všechny projekty a aktualizujte jejich na stejné verzi Xamarin.Forms:

    ![Aktualizovat balíček NuGet](wpf-images/update-nuget-package.png "aktualizovat balíček NuGet") 

7. V projektu WPF, klikněte pravým tlačítkem na **odkazy**. V **správce odkazů** dialogového okna, vyberte **projekty** na levé straně a zaškrtněte políčko vedle **BoxViewClock** projektu:

    ![Odkazovat na sdílený projekt](wpf-images/reference-shared-project.png "odkazovat na sdílený projekt")

8. Upravit **souboru MainWindow.xaml** soubor projektu WPF. V `Window` značky, přidejte deklarace oboru názvů XML pro **Xamarin.Forms.Platform.WPF** sestavení a oboru názvů:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Teď změňte `Window` značku na `wpf:FormsApplicationPage`. Změnit `Title` nastavení na název vaší aplikace, například **BoxViewClock**. Hotový soubor XAML by měla vypadat takto:

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

9. Upravit **MainWindow.xaml.cs** soubor projektu WPF. Přidejte dva nové `using` direktivy:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Změňte základní třídu `MainWindow` z `Window` k `FormsApplicationPage`. Následující `InitializeComponent` volání, přidejte následující dva příkazy:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    S výjimkou komentáře a nepoužívané `using` direktivy, kompletní **MainWindows.xaml.cs** soubor by měl vypadat nějak takto:

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

10. Klikněte pravým tlačítkem na projekt WPF v **Průzkumníka řešení** a vyberte **nastavit jako spouštěný projekt**. Stiskněte klávesu F5 ke spuštění programu v ladicím programu sady Visual Studio na ploše Windows:

    ![Hodin WPF BoxView](wpf-images/wpf-boxviewclock.png "BoxView hodin WPF" )

## <a name="next-steps"></a>Další kroky

### <a name="platform-specifics"></a>Specifika platforem

Můžete určit, jakou platformu aplikace Xamarin.Forms běží na z kódu nebo XAML. To umožňuje změnit vlastnosti programu, pokud je spuštěn na WPF. V kódu, porovnat hodnotu `Device.RuntimePlatform` s `Device.WPF` – konstanta (který se rovná řetězci "WPF"). Pokud se zjistí shoda, je aplikace spuštěná na WPF.

V XAML, můžete použít `OnPlatform` značky, vyberte hodnotu vlastnosti specifické pro platformu:

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

Počáteční velikost okna v WPF můžete upravit **souboru MainWindow.xaml** souboru:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Problémy

Toto je náhled, proto byste měli očekávat, že ne vše, co je připraveno na produkční. Ne všechny balíčky NuGet pro Xamarin.Forms, které jsou připravené pro WPF a některé funkce nemusí plně fungovat.

