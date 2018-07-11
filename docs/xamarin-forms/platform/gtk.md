---
title: 'Instalační program platformy GTK #'
description: 'Xamarin.Forms teď nabízí podporu verze preview pro platformu GTK #'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 7f68b7c8affc11b50bdb4a2fc9589f8dcbfb45ec
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830477"
---
# <a name="gtk-platform-setup"></a>Instalační program platformy GTK #

![Náhled](~/media/shared/preview.png)

Xamarin.Forms teď nabízí podporu verze preview pro GTK # aplikace. GTK # je sada nástrojů grafické uživatelské rozhraní propojí se sadou nástrojů GTK + a širokou škálu knihovnách GNOME umožňuje vývoj plně nativní aplikace GNONE grafiky s použitím Mono a .NET. Tento článek ukazuje, jak přidat projekt GTK # do řešení Xamarin.Forms.

Před začít, vytvořte nové řešení Xamarin.Forms nebo použít existující řešení Xamarin.Forms, třeba [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Přestože tento článek se zaměřuje na přidání aplikace GTK # do řešení Xamarin.Forms v VS2017 a sady Visual Studio pro Mac, to lze provést také v [MonoDevelop](http://www.monodevelop.com/) pro Linux.

## <a name="adding-a-gtk-app"></a>Přidává se aplikace GTK #

GTK # pro macOS a Linux je nainstalován jako součást [Mono](http://www.mono-project.com/download/stable/). GTK # pro .NET je možné nainstalovat na Windows s [GTK # instalační program](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Postupujte podle těchto pokynů můžete přidat aplikaci GTK #, který se spustí na ploše Windows:

1. V sadě Visual Studio 2017, klikněte pravým tlačítkem na název řešení v **Průzkumníka řešení** a zvolte **Přidat > Nový projekt...** .

2. V **nový projekt** okně na levém vyberte **Visual C#** a **klasická plocha Windows**. V seznamu typů projektů zvolte **knihovna tříd (.NET Framework)** a ujistěte se, že **Framework** rozevíracího seznamu je nastavit na minimálně rozhraní .NET Framework 4.7.

3. Zadejte název projektu s **GTK** příponu, třeba **GameOfLife.GTK**. Klikněte na tlačítko **Procházet** tlačítko, vyberte složku, která obsahuje jiná platforma projekty a stiskněte klávesu **vybrat složku**. To se umístit projekt GTK ve stejném adresáři jako ostatní projekty v řešení.

    ![Přidat nový projekt GTK](gtk-images/win/add-new-project.png "přidat nový projekt GTK")

    Stisknutím klávesy **OK** tlačítko pro vytvoření projektu.

4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na nový projekt GTK a vyberte **spravovat balíčky NuGet**. Vyberte **Procházet** kartu a vyhledejte **Xamarin.Forms** 3.0 nebo novější.

    ![Vyberte balíček Xamarin.Forms NuGet](gtk-images/win/select-forms-nuget-package.png "vyberte balíček Xamarin.Forms. NuGet")

    Vyberte balíček a klikněte na tlačítko **nainstalovat** tlačítko.

5. Nyní vyhledejte **Xamarin.Forms.Platform.GTK** balíček 3.0 nebo vyšší.

    ![Vyberte balíček Xamarin.Forms.Platform.GTK NuGet](gtk-images/win/select-forms-platform-nuget-package.png "vyberte balíček Xamarin.Forms.Platform.GTK NuGet")

    Vyberte balíček a klikněte na tlačítko **nainstalovat** tlačítko.

6. V **Průzkumníka řešení**, klikněte pravým tlačítkem na název řešení a vyberte **spravovat balíčky NuGet pro řešení**. Vyberte **aktualizace** kartu a **Xamarin.Forms** balíčku. Vybrat všechny projekty a aktualizovat na stejnou verzi jako projekt GTK Xamarin.Forms.

7. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **odkazy** v projektu GTK. V **správce odkazů** dialogového okna, vyberte **projekty** na levé straně a zaškrtněte políčko vedle projekt .NET Standard nebo Shared:

    ![Odkazovat na sdílený projekt](gtk-images/win/reference-shared-project.png "odkazovat na sdílený projekt")

8. V **správce odkazů** dialogového okna, stisknutím klávesy **Procházet** tlačítko a přejděte **C:\Program Files (x86)\GtkSharp\2.12\lib** a pak zvolte položku  **ATK sharp.dll**, **gdk sharp.dll**, **glade sharp.dll**, **glib sharp.dll**, **gtk-dotnet.dll**, **gtk-sharp.dll** soubory.

    ![Odkazovat na knihovnách GTK #](gtk-images/win/reference-gtk-libraries.png "odkazovat na knihovnách GTK #")

    Stisknutím klávesy **OK** tlačítko Přidat odkazy.

9. V projektu GTK přejmenovat **Class1.cs** k **Program.cs**.

10. V projektu GTK upravit **Program.cs** souboru tak, aby vypadá podobně jako následující kód:

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

    Tento kód inicializuje GTK # a Xamarin.Forms, vytvoří okno aplikace a spustí aplikaci.

11. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt GTK a vyberte **vlastnosti**.

12. V **vlastnosti** okna, vyberte **aplikace** kartu a změnit **typ výstupu** rozevíracího seznamu **aplikace Windows**.

    ![Změnit typ výstupu projektu](gtk-images/win/change-project-output-type.png "změnit typ výstupu projektu")

13. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt WPF a vyberte **nastavit jako spouštěný projekt**. Stiskněte klávesu F5 ke spuštění programu v ladicím programu sady Visual Studio na ploše Windows:

    ![Hra GTK # životnosti](gtk-images/win/gtk-gameoflife.png "hru GTK # životnosti")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Postupujte podle těchto pokynů můžete přidat aplikaci GTK #, který se spustí na počítači Mac:

1. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na řešení Xamarin.Forms a zvolte **Přidat > Přidat nový projekt...** .

2. V **nový projekt** okna zvolte **jiných > .NET > Projekt Gtk # 2.0** a stiskněte klávesu **Další**.

3. Zadejte název projektu s **GTK** příponu, třeba **GameOfLife.GTK**a stiskněte klávesu **vytvořit**.

4. V **oblasti řešení**, klikněte pravým tlačítkem na **balíčků > přidat balíčky...**  pro GTK na projekt a přidejte balíček NuGet Xamarin.Forms 3.0 předběžné verze nebo novější.

    ![Vyberte balíček Xamarin.Forms NuGet](gtk-images/mac/select-forms-nuget-package.png "vyberte balíček Xamarin.Forms. NuGet")

5. V **oblasti řešení**, klikněte pravým tlačítkem na **balíčků > přidat balíčky...**  pro GTK projekt a přidejte balíček NuGet předběžné verze Xamarin.Forms.Platform.GTK 3.0 nebo vyšší.

    ![Vyberte balíček Xamarin.Forms.Platform.GTK NuGet](gtk-images/mac/select-forms-platform-nuget-package.png "vyberte balíček Xamarin.Forms.Platform.GTK NuGet")

6. Aktualizujte ostatní projekty platformy používat stejnou verzi Xamarin.Forms jako projekt GTK.

7. V **oblasti řešení**, klikněte pravým tlačítkem na **odkazy > Upravit odkazy...**  pro GTK na projekt a přidejte odkaz na projekt Xamarin.Forms (.NET Standard nebo sdíleného projektu).

    ![Odkazovat na sdílený projekt](gtk-images/mac/reference-shared-project.png "odkazovat na sdílený projekt")

8. Upravit **Program.cs** souboru projekt GTK tak, že se podobá následující kód:

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

    Tento kód inicializuje GTK # a Xamarin.Forms, vytvoří okno aplikace a spustí aplikaci.

9. V **oblasti řešení**, klikněte pravým tlačítkem na projekt GTK a vyberte **nastavit jako spouštěný projekt**.

10. V aplikaci Visual Studio pro Mac nástrojů, stiskněte **Start** tlačítka (trojúhelníkové tlačítko, která se podobá tlačítko Přehrát) ke spuštění aplikace.

    ![Hra GTK # životnosti](gtk-images/mac/gtk-gameoflife.png "hru GTK # životnosti")

-----

## <a name="next-steps"></a>Další kroky

### <a name="platform-specifics"></a>Specifika platforem

Můžete určit, jakou platformu aplikace Xamarin.Forms běží na z XAML nebo kódu. To umožňuje změnit vlastnosti program spuštěný v GTK #. V kódu, porovnat hodnotu `Device.RuntimePlatform` s `Device.GTK` – konstanta (který se rovná řetězci "GTK"). Pokud se zjistí shoda, je aplikace spuštěná v GTK #.

V XAML, můžete použít `OnPlatform` značky, vyberte hodnotu vlastnosti specifické pro platformu:

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

Nejsou k dispozici pro GTK # širokou škálu motivy a lze je použít z aplikace Xamarin.Forms:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Nativní formuláře

Xamarin.Forms umožňuje nativní formuláře [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky, které využívat nativní projekty, včetně GTK # projektů. Toho můžete docílit tak, že vytvoříte instanci [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-odvozené stránky a převod na nativních GTK # typ pomocí `CreateContainer` – metoda rozšíření:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Další informace o nativní formuláře, naleznete v tématu [nativní formuláře](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Problémy

Toto je náhled, proto byste měli očekávat, že ne vše, co je připraveno na produkční. Aktuální stav implementaci, najdete v části [stav](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)a aktuální známé problémy najdete v části [čekající na vyřízení a známé problémy](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
