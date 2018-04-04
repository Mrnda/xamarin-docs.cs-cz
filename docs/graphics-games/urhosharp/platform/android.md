---
title: Podpora UrhoSharp Android
description: Android – konkrétní nastavení a funkcí pro UrhoSharp.
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 2d0eb183817a84b982149736fa24f9ec56c1753e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="urhosharp-android-support"></a>Podpora UrhoSharp Android

_Android – konkrétní nastavení a funkcí_

Při Urho je knihovny přenosných tříd a umožňuje stejné rozhraní API pro použití na celém různé platformy pro herní logiku, je stále nutné inicializovat Urho v ovladači konkrétní platformu a v některých případech, můžete využít výhod funkcí konkrétní platformu .

Na stránkách předpokládat, že `MyGame` je podtřídou třídy `Application` třídy.

## <a name="architectures"></a>Architektury

**Podporované architektury**: x86, armeabi, armeabi v7a

## <a name="create-a-project"></a>Vytvoření projektu

Vytvoření projektu Android a přidejte balíček UrhoSharp NuGet.

Přidat Data obsahující vaše prostředky se mají **prostředky** adresář a zkontrolujte, zda mají všechny soubory **AndroidAsset** jako **akce sestavení**.

![Projekt instalace](android-images/image-3.png "přidat Data obsahující prostředky, k adresáři prostředky")

## <a name="configure-and-launching-urho"></a>Konfigurace a spuštění Urho

Přidání direktivy using pro příkazy `Urho` a `Urho.Android` obory názvů a poté přidejte tento kód pro inicializaci Urho, jakož i spuštění vaší aplikace.

Nejjednodušší způsob, jak spustit hru, jak je implementována ve třídě MyGame se má volat

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

Tím otevřete aktivitu celá obrazovka s příslušnou hru jako obsahu.

## <a name="custom-embedding-of-urho"></a>Vlastní vložení Urho

Vám může případně na situaci, kdy Urho převzít obrazovce celá aplikace, a pokud chcete použít jako součást vaší aplikace, můžete vytvořit `SurfaceView` prostřednictvím:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

Budete také muset pár události od vás aktivitu předat UrhoSharp, např:

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

Musí Totéž proveďte pro: `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` a `OnWindowFocusChanged`.

Ukazuje to typický aktivity, která spustí hra:

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

