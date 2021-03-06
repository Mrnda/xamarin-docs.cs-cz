---
title: Podpora UrhoSharp Windows
description: Tento dokument popisuje podporu Windows pro UrhoSharp. Popisuje, jak vytvořit projekt, nakonfigurovat a spustit Urho, integrovat WPF a integrovat UWP.
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 094eaf0ebe84ce8c1771bd6481ee897463349856
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783230"
---
# <a name="urhosharp-windows-support"></a>Podpora UrhoSharp Windows

Při Urho je knihovny přenosných tříd a umožňuje stejné rozhraní API pro použití na celém různé platformy pro herní logiku, je stále nutné inicializovat Urho v ovladači konkrétní platformu a v některých případech, můžete využít výhod funkcí konkrétní platformu .

Na stránkách předpokládat, že `MyGame` je podtřídou třídy `Application` třídy.

**Podporované architektury:** pouze 64bitová verze systému Windows.

Vidíte dokončení příklady znázorňující způsob použití v našem [ukázky](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>Samostatné projektu

### <a name="creating-a-project"></a>Vytvoření projektu

Vytvoření projektu konzoly, odkazovat Urho NuGet a pak se ujistěte, že je možné najít prostředky (adresáře, která obsahuje adresář dat).

### <a name="configuring-and-launching-urho"></a>Konfigurace a spuštění Urho

Chcete-li spustit aplikaci, postupujte takto:

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>Příklad

[Úplný příklad](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>Integrované v grafickém subsystému WPF

### <a name="creating-a-project"></a>Vytvoření projektu

Vytvořte projekt WPF, odkazovat Urho NuGet a pak se ujistěte, že je možné najít prostředky (adresáře, která obsahuje adresář dat).

### <a name="configuring-and-launching-urho-from-wpf"></a>Konfigurace a spuštění Urho z grafického subsystému WPF

Vytvoření podtřídou třídy `Window` a nakonfigurovat vaše prostředky takto:

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

### <a name="example"></a>Příklad

[Úplný příklad](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

## <a name="integrated-with-uwp"></a>Integrovat UWP

### <a name="creating-a-project"></a>Vytvoření projektu

Vytvoření projektu UPW, odkazovat Urho NuGet a pak se ujistěte, že je možné najít prostředky (adresáře, která obsahuje adresář dat).

### <a name="configuring-and-launching-urho-from-uwp"></a>Konfigurace a spuštění Urho z UWP

Vytvoření podtřídou třídy `Window` a nakonfigurovat vaše prostředky takto:

```csharp
{
            InitializeComponent();
            GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
                .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
                .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
                .ToArray();
            DataContext = this;
            Loaded += (s, e) => RunGame (new MyGame ());
        }

        public void RunGame(TypeInfo value)
        {
            //at this moment, UWP supports assets only in pak files (see PackageTool)
            currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
        }
    }
```

### <a name="example"></a>Příklad

[Úplný příklad](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

## <a name="integrated-with-windowsforms"></a>Integrovat Windows.Forms

### <a name="creating-a-project"></a>Vytvoření projektu

Vytvoření projektu Windows.Forms, odkazovat Urho NuGet a pak se ujistěte, že je možné najít prostředky (adresáře, která obsahuje adresář dat).

### <a name="configuring-and-launching-urho-from-windowsforms"></a>Konfigurace a spuštění Urho z Windows.Forms

Spustit Urho ze svého formuláře, najdete v části [ucelenou ukázku](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
