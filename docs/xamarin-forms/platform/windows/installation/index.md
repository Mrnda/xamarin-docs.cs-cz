---
title: Projekty instalace systému Windows
description: Přidání nové projekty Windows do existujícího řešení Xamarin.Forms
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: b6ea988aa8c058fe5a92a17e9b72f81e0ccb12db
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="setup-windows-projects"></a>Projekty instalace systému Windows

_Přidání nové projekty Windows do existujícího řešení Xamarin.Forms_

Starší řešení Xamarin.Forms (nebo nebyla vytvořena v systému macOS) nebudou mít projekty aplikací pro univerzální platformu Windows (UWP). Proto musíte ručně přidat do projektu UPW sestavení Windows 10 (UWP) aplikace.

<a name="pcl" />

## <a name="update-the-pcl-profile"></a>Aktualizovat profil PCL

Pokud vaše stávající aplikace Xamarin.Forms používá šablony přenosných třída knihovny PCL (), je třeba aktualizovat jeho profil.

1. **Klikněte pravým tlačítkem na > vlastnosti** (stávající nastavení se můžou lišit)

  ![](images/targets.png "PCL cíle")

2. Klikněte na **změnu...** tlačítko

3. Ujistěte se, **Windows 8** a **Windows Phone 8.1** jsou vybrané možnosti (a **Windows Phone Silveright** je *zrušte vybrané*):

  ![](images/pcl.png "PCL Target – možnosti")

4. Stiskněte klávesu **OK** a uložte změny.

Tato hodnota rovná **profil 111** při konfigurování vašeho PCL v sadě Visual Studio pro Mac pomocí rozevíracího seznamu.

  ![](images/pcl-xs.png "Profil PCL 111")

## <a name="add-a-universal-windows-platform-app"></a>Přidat Universal Windows Platform aplikace

Zda je spuštěna **Visual Studio 2017** na **Windows 10** pro vývoj aplikací UWP. Další informace o univerzální platformu Windows najdete v tématu [Úvod do univerzální platformy Windows](/windows/uwp/get-started/universal-application-platform-guide/).

UWP je k dispozici v Xamarin.Forms 2.1 a novějším a Xamarin.Forms.Maps je podporována v Xamarin.Forms 2.2 nebo novější.

Zkontrolujte <a href="#troubleshooting">řešení potíží s</a> části užitečné tipy pro.

Postupujte podle těchto pokynů můžete přidat aplikace pro UPW, který se spustí na Windows 10 telefony, tablety a stolní počítače:

 1 . Klikněte pravým tlačítkem na řešení a vyberte **Přidat > Nový projekt...**  a přidejte **prázdná aplikace (univerzální pro Windows)** projektu:

  ![](universal-images/add-wu.png "Přidat dialogové okno Nový projekt")

 2 . V **nový projekt univerzální platformu Windows** dialogovém okně, vyberte minimální a cílové verze systému Windows 10, které aplikace poběží na:

  ![](universal-images/target-version.png "Universal Windows Platform dialogové okno Nový projekt")

 3 . Klikněte pravým tlačítkem na projekt UWP a vyberte **spravovat balíčky NuGet...**  a přidejte **Xamarin.Forms** balíčku. Zajistěte, aby že ostatní projekty v řešení jsou aktualizované taky na stejnou verzi balíčku Xamarin.Forms.

 4 . Ujistěte se, že nový projekt UWP budou vytvořeny **sestavení > nástroje Configuration Manager** okno (to pravděpodobně nebude došlo ve výchozím nastavení). Značky **sestavení** a **nasadit** polí pro univerzální projekt:

  [![](universal-images/configuration-sml.png "Okno nástroje Configuration Manager")](universal-images/configuration.png#lightbox "okno nástroje Configuration Manager")

 5 . Klikněte pravým tlačítkem na projekt a vyberte **Přidat > odkaz** a vytvořit odkaz na projekt aplikace Xamarin.Forms (PCL, .NET Standard nebo sdílený projekt).

  ![](universal-images/addref-sml.png "Dialogové okno Správce odkazů")

 6 . V projektu UPW upravit **App.xaml.cs** zahrnout `Init` volání metody uvnitř `OnLaunched` metoda kolem řádku 52:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . V projektu UPW upravit **MainPage.xaml** odebráním `Grid` obsažené v `Page` elementu.

 8 . V **MainPage.xaml**, přidejte nový `xmlns` záznam pro `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . V **MainPage.xaml**, změňte kořenu `<Page` element `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 . V projektu UPW upravit **MainPage.xaml.cs** odebrat `: Page` dědičnosti specifikátor pro název třídy (vzhledem k tomu, že bude nyní dědí `WindowsPage` z důvodu změny provedené v předchozím kroku):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . V **MainPage.xaml.cs**, přidejte `LoadApplication` volání v `MainPage` konstruktor a spusťte aplikaci Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12 . Přidejte libovolné místní prostředky (např. soubory obrázků) z existující projekty platformy, které jsou požadovány.

## <a name="troubleshooting"></a>Poradce při potížích

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"Cíle vyvolání výjimky" při použití "Kompilovat s .NET nativní nástroj řetězec"

Pokud vaše aplikace pro UPW odkazuje na více sestavení (například knihovny ovládacího prvku třetí strany nebo vaše aplikace je rozdělená do více knihoven), může být Xamarin.Forms nelze načíst objekty z těchto sestavení (například vlastní nástroji pro vykreslování).

Tato situace může nastat při použití **kompilovat s .NET Native řetězu nástroj** tedy možnost pro aplikace UWP v **vlastnosti > sestavení > Obecné** okna pro projekt.

Toto můžete opravit pomocí na přetížení specifické pro UPW `Forms.Init` volání v **App.xaml.cs** jak je znázorněno v následujícím kódu (má nahradit `ClassInOtherAssembly` ke skutečné třídě váš kód odkazuje):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Přidáte odkaz na každé sestavení, které se odkazuje na aplikaci.

#### <a name="dependency-services-and-net-native-compilation"></a>Závislosti služeb a rozhraní .NET nativní kompilace

Verze sestavení pomocí .NET Native kompilace může selhat k rozpoznávání služeb závislosti, které jsou definovány mimo hlavní aplikace spustitelný soubor (například v samostatných projektu nebo knihovny).

Použití `DependencyService.Register<T>()` metoda ručně zaregistrovat třídy závislosti služeb. Podle toho v příkladu výše, přidejte metodu registrace takto:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
