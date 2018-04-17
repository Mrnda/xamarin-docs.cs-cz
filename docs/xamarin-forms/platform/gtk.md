---
title: 'Instalační program platformy GTK #'
description: 'Xamarin.Forms má nyní preview podporu pro platformu GTK #'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: a601e74cc274fd57bb2be9af3562b3a7290d7047
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="gtk-platform-setup"></a>Instalační program platformy GTK #

![Náhled](~/media/shared/preview.png)

Xamarin.Forms má nyní preview podporuje GTK # aplikace. GTK # je sada nástrojů grafické uživatelské rozhraní pro propojující toolkit GTK + a různých knihoven GNOME umožňuje vývoj plně nativní aplikace grafiky GNONE pomocí Mono a rozhraní .NET. Tento článek ukazuje, jak přidat projekt GTK # do řešení Xamarin.Forms.

Před začátkem, vytvořte nové řešení Xamarin.Forms nebo použít existující řešení Xamarin.Forms, například [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Když tento článek se zaměřuje na přidání aplikace GTK # do řešení Xamarin.Forms v VS2017 a Visual Studio pro Mac, je lze také provést v [MonoDevelop](http://www.monodevelop.com/) pro Linux.

## <a name="adding-a-gtk-app"></a>Přidání aplikace GTK #

GTK # pro systému macOS a Linux je nainstalován jako součást [Mono](http://www.mono-project.com/download/stable/). GTK # pro .NET je možné nainstalovat na systém Windows pomocí [GTK # instalační program](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Postupujte podle těchto pokynů můžete přidat GTK # aplikaci, která se spustí na ploše systému Windows:

1. V Visual Studio 2017, klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a zvolte **Přidat > Nový projekt...** .

2. V **nový projekt** okně na levém vyberte **Visual C#** a **Windows Classic Desktop**. V seznamu typy projektů, vyberte **knihovny tříd (rozhraní .NET Framework)**a ujistěte se, že **Framework** rozevíracího seznamu je nastaven na minimálně rozhraní .NET Framework 4.7.

3. Zadejte název projektu s **GTK** příponu, třeba **GameOfLife.GTK**. Klikněte **Procházet** tlačítko, vyberte složku obsahující jiné platformy projekty a stiskněte klávesu **vyberte složku**. Tím bude přidán GTK projektu ve stejném adresáři jako ostatní projekty v řešení.

    ![Přidat nový projekt GTK](gtk-images/win/add-new-project.png "přidat nový projekt GTK")

    Stiskněte **OK** tlačítko pro vytvoření projektu.

4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na nový projekt GTK a vyberte **spravovat balíčky NuGet**. Vyberte **Procházet** , klikněte na **zahrnout předběžné verze** zaškrtávací políčko a vyhledejte **Xamarin.Forms** 3.0 nebo novější.

    ![Vyberte balíček Xamarin.Forms NuGet](gtk-images/win/select-forms-nuget-package.png "vyberte balíček Xamarin.Forms NuGet")

    Vyberte balíček a klikněte na **nainstalovat** tlačítko.

5. Nyní Hledat **Xamarin.Forms.Platform.GTK** balíček 3.0 nebo vyšší.

    ![Vyberte balíček Xamarin.Forms.Platform.GTK NuGet](gtk-images/win/select-forms-platform-nuget-package.png "vyberte balíček Xamarin.Forms.Platform.GTK NuGet")

    Vyberte balíček a klikněte na **nainstalovat** tlačítko.

6. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název řešení a vyberte **spravovat balíčky NuGet pro řešení**. Vyberte **aktualizace** kartě a **Xamarin.Forms** balíčku. Vybrat všechny projekty a provede jejich aktualizaci na stejnou verzi jako použité v projektu GTK Xamarin.Forms.

7. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **odkazy** v GTK projektu. V **správce odkazů** dialogovém okně, vyberte **projekty** na levé straně a zaškrtněte políčka u projektu .NET Standard, PCL nebo sdílené:

    ![Referenční sdílený projekt](gtk-images/win/reference-shared-project.png "odkazovat sdílený projekt")

8. V **správce odkazů** dialogové okno, stiskněte **Procházet** tlačítko a přejděte do **C:\Program Files (x86)\GtkSharp\2.12\lib** složky a vyberte  **ATK sharp.dll**, **gdk sharp.dll**, **glade sharp.dll**, **glib sharp.dll**, **gtk-dotnet.dll**, **gtk sharp.dll** soubory.

    ![Reference knihovny GTK #](gtk-images/win/reference-gtk-libraries.png "Reference knihovny GTK #")

    Stiskněte **OK** tlačítko přidáte odkazy.

9. V projektu GTK přejmenovat **Class1.cs** k **Program.cs**.

10. V projektu GTK upravit **Program.cs** tak, aby je podobná následující kód:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Tento kód inicializuje GTK # a Xamarin.Forms, vytvoří okna aplikace a spustí aplikace.

11. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt GTK a vyberte **vlastnosti**.

12. V **vlastnosti** vyberte **aplikace** kartě a změňte **výstupní typ** rozevírací seznam pro **aplikace Windows**.

    ![Změnit typ výstupu projektu](gtk-images/win/change-project-output-type.png "změnit typ výstupu projektu")

13. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt WPF a vyberte **nastavit jako spouštěný projekt**. Stisknutím klávesy F5 spusťte program pomocí ladicího programu sady Visual Studio na ploše systému Windows:

    ![Herní GTK # života](gtk-images/win/gtk-gameoflife.png "GTK # herní životnosti")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Postupujte podle těchto pokynů můžete přidat aplikaci GTK # které poběží na ploše Mac:

1. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na řešení Xamarin.Forms a zvolte **Přidat > Přidat nový projekt...** .

2. V **nový projekt** okno zvolte **jiné > .NET > projektu 2.0 Gtk #** a stiskněte klávesu **Další**.

3. Zadejte název projektu s **GTK** příponu, třeba **GameOfLife.GTK**a stiskněte klávesu **vytvořit**.

4. V **řešení Pad**, klikněte pravým tlačítkem na **balíčků > přidat balíčky...**  pro GTK projekt a přidejte balíček NuGet předběžné verze Xamarin.Forms 3.0 nebo vyšší.

    ![Vyberte balíček Xamarin.Forms NuGet](gtk-images/mac/select-forms-nuget-package.png "vyberte balíček Xamarin.Forms NuGet")

5. V **řešení Pad**, klikněte pravým tlačítkem na **balíčků > přidat balíčky...**  pro GTK projekt a přidejte balíček NuGet předběžné verze Xamarin.Forms.Platform.GTK 3.0 nebo vyšší.

    ![Vyberte balíček Xamarin.Forms.Platform.GTK NuGet](gtk-images/mac/select-forms-platform-nuget-package.png "vyberte balíček Xamarin.Forms.Platform.GTK NuGet")

6. Aktualizujte ostatní platformy projekty, aby používaly stejnou verzi Xamarin.Forms jako použité GTK projektu.

7. V **řešení Pad**, klikněte pravým tlačítkem na **odkazy > Upravit odkazy...**  pro GTK projekt a přidejte odkaz na projekt Xamarin.Forms (.NET Standard, PCL nebo sdílený projekt).

    ![Referenční sdílený projekt](gtk-images/mac/reference-shared-project.png "odkazovat sdílený projekt")

8. Upravit **Program.cs** GTK projektu, které se podobá následující kód:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Tento kód inicializuje GTK # a Xamarin.Forms, vytvoří okna aplikace a spustí aplikace.

9. V **řešení Pad**, klikněte pravým tlačítkem na projekt GTK a vyberte **nastavit jako spouštěný projekt**.

10. V sadě Visual Studio pro Mac nástrojů stisknutím klávesy **spustit** (tlačítko trojúhelníkovou podobná tlačítko Přehrát akci) spusťte aplikaci.

    ![Herní GTK # života](gtk-images/mac/gtk-gameoflife.png "GTK # herní životnosti")

-----

## <a name="next-steps"></a>Další kroky

### <a name="platform-specifics"></a>Specifika platformy

Můžete určit, jaké platformě Xamarin.Forms aplikace běží na z XAML nebo kódu. To umožňuje změnit vlastnosti program, když je spuštěn na GTK #. V kódu porovnat hodnotu `Device.RuntimePlatform` s `Device.GTK` konstanta, (který se rovná řetězec "GTK"). Pokud je nalezena shoda, aplikace běží na GTK #.

V jazyce XAML, můžete použít `OnPlatform` značky a vyberte hodnotu vlastnosti specifické pro platformu:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>Ikona aplikace

Ikona aplikace můžete nastavit při spuštění:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Motivy

Nejsou k dispozici pro GTK # širokou škálu motivy a použít z aplikace na platformě Xamarin.Forms:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Nativní formulářů

Nativní Forms umožňuje Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, které se spotřebovávají nativní projektech, včetně GTK # projekty. To můžete udělat tak, že vytvoříte instanci [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky a převod na nativní GTK # typ pomocí `CreateContainer` metoda rozšíření:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Další informace o nativní Forms najdete v tématu [nativní Forms](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Problémy

Toto je náhled, takže byste měli očekávat, že není vše produkční připraven. Aktuální stav implementaci, najdete v části [stav](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)a aktuální známé problémy najdete v tématu [čekající & známé problémy](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
