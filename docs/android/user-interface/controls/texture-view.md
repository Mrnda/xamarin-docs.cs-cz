---
title: TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: 1cd332894deac1e1fb076f647bb0120bda2306da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763602"
---
# <a name="textureview"></a>TextureView

`TextureView` Třída je zobrazení, které používá accelerated hardwaru 2D vykreslování můžete povolit video nebo OpenGL obsahu datový proud, který se má zobrazit. Například následující snímek obrazovky ukazuje `TextureView` zobrazení za provozu informačního kanálu z fotoaparátu v zařízení:

[![Příklad snímek obrazovky za provozu bitovou kopii z fotoaparátu zařízení](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

Na rozdíl od `SurfaceView` třídy, který můžete použít také k zobrazení OpenGL nebo video obsahu, TextureView není vyjádřena v samostatném okně.
Proto `TextureView` dokážou podporovat transformace zobrazení podobně jako ostatní zobrazení. Například otáčení `TextureView` můžete docílit, jednoduše nastavením jeho `Rotation` vlastnost, jeho průhlednost nastavením jeho `Alpha` vlastnost a tak dále.

Proto se `TextureView` jsme teď můžete provádět akce podobně jako zobrazení živý datový proud z fotoaparátu a transformovat je, jak je znázorněno v následujícím kódu:

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;
       
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;
           
        SetContentView (_textureView);
    }
       
    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }
           
        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

Výše uvedený kód vytvoří `TextureView` instance v rámci aktivity `OnCreate` metoda a nastaví aktivity tak, jak `TextureView`na `SurfaceTextureListener`. Chcete-li být `SurfaceTextureListener`, implementuje aktivity `TextureView.ISurfaceTextureListener` rozhraní. Systém bude volat `OnSurfaceTextAvailable` metoda při `SurfaceTexture` je připravený k použití. Tato metoda, můžeme provést `SurfaceTexture` , je předaná a nastavte ji na texture náhledu fotoaparátu. Potom jsme jsou zadarmo normální operacích na základě zobrazení, jako je například nastavení `Rotation` a `Alpha`, jako v příkladu nahoře. Výsledná aplikace spuštěné na zařízení, je zobrazena níže:

[![Příklad aplikaci spuštěnou na zařízení, zobrazení obrázku](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Chcete-li použít `TextureView`, hardwarovou akceleraci musí být povolena, který bude ve výchozím nastavení od 14 úroveň rozhraní API. Také, protože tento příklad používá fotoaparátu, jak `android.permission.CAMERA` oprávnění a `android.hardware.camera` funkce musí být nastavena v **AndroidManifest.xml**.



## <a name="related-links"></a>Související odkazy

- [TextureViewDemo (sample)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Představení Sandwichovy Zmrzlinová](http://www.android.com/about/ice-cream-sandwich/)
- [Platformu Android 4.0](http://developer.android.com/sdk/android-4.0.html)
