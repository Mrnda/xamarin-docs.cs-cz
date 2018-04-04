---
title: Vytváření řez sledování
description: Tato příručka vysvětluje, jak implementovat vlastní sledování vzhled služby pro Android nosit. Podrobné pokyny jsou uvedené pro vytváření odřapíkovaného dolů služby vzhled digitální sledovat, následuje další kód k vytvoření analogovým stylu sledovat řez.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c02755cc3ff5b46a5a97b6c14185794d8ad538d8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-watch-face"></a>Vytváření řez sledování

_Tato příručka vysvětluje, jak implementovat vlastní sledování vzhled služby pro Android nosit. Podrobné pokyny jsou uvedené pro vytváření odřapíkovaného dolů služby vzhled digitální sledovat, následuje další kód k vytvoření analogovým stylu sledovat řez._

## <a name="overview"></a>Přehled 

V tomto návodu se vytvoří základní sledování vzhled služby pro ilustraci essentials vytváření vlastní Android nosit sledovat řez. Služba počáteční sledovat řez zobrazí jednoduché digitální sledování, které se zobrazí aktuální čas v hodinách a minutách: 

[![Vzhled digitální sledovat](creating-a-watchface-images/01-initial-face.png "snímku obrazovky s příkladem písmo, počáteční digitální sledování")](creating-a-watchface-images/01-initial-face.png#lightbox)

Tento digitální sledovat řez je vyvinutých, testování, je přidán další kód ho upgradovat další pokročilé analogovým sledovat setkávají s tři rukou: 

[![Vzhled analogovým sledovat](creating-a-watchface-images/02-example-watchface.png "snímku obrazovky s příkladem písmo, poslední analogovým sledování")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Podívejte se na vzhled služby jsou seskupeny a nainstalován jako součást aplikace a opotřebením motoru. V následujících příkladech `MainActivity` obsahuje nic jiného než kód z šablony aplikace opotřebení tak, aby službu sledování řez se dají zabalené a nasadit do inteligentní sledování v rámci aplikace. V důsledku toho tato aplikace bude sloužit výhradně jako vehicle pro získání službu sledování vzhled načten do paměti zařízení (nebo emulátoru) pro ladění a testování. 

## <a name="requirements"></a>Požadavky

K implementaci služby vzhled sledování, je nutné provést následující:

-   Android 5.0 (API úrovně 21) nebo vyšší na emulátoru nebo zařízení a opotřebením motoru.

-   [Xamarin Android nosit knihovny podporu](https://www.nuget.org/packages/Xamarin.Android.Wear) musí být přidán do projektu Xamarin.Android. 

I když Android 5.0 je minimální rozhraní API úrovně pro implementaci sledování služby vzhled Android 5.1 a později se doporučuje. Android nosit zařízení se systémem Android 5.1 (22 rozhraní API) nebo vyšší povolit opotřebení aplikacím řídit, co se zobrazí na obrazovce, když se zařízení právě úsporného režimu *vedlejším* režimu. Když zařízení opustí úsporného režimu *vedlejším* režimu, je v *interaktivní* režimu. Další informace o těchto režimech najdete v tématu [zachování vaše aplikace viditelné](https://developer.android.com/training/wearables/apps/always-on.html). 


## <a name="start-an-app-project"></a>Spusťte projekt aplikace

Vytvořit nový projekt Android nosit s názvem **WatchFace** (Další informace o vytváření nových projektů Xamarin.Android najdete v tématu [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dialogové okno Nový projekt](creating-a-watchface-images/03-wear-project-vs-sml.png "vyberte nosit aplikace v dialogovém okně Nový projekt")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dialogové okno Nový projekt](creating-a-watchface-images/03-wear-project-xs-sml.png "vyberte nosit aplikace v dialogovém okně Nový projekt")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


Nastavte název balíčku na `com.xamarin.watchface`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Balíček nastavení název](creating-a-watchface-images/04-package-name-vs.png "nastavte název balíčku na com.xamarin.watchface")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Balíček nastavení název](creating-a-watchface-images/04-package-name-xs.png "nastavte název balíčku na com.xamarin.watchface")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kromě toho, posuňte se dolů a povolte **INTERNET** a **WAKE_LOCK** oprávnění: 

[![Požadovaná oprávnění](creating-a-watchface-images/05-required-permissions-vs.png "oprávnění povolit INTERNET a WAKE_LOCK")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nastavte minimální Android verzi **Android 5.1 (API úrovně 22)**. Také povolit **Internet** a **WakeLock** oprávnění:

[![Požadovaná oprávnění](creating-a-watchface-images/05-required-permissions-xs.png "oprávnění povolit Internet a WakeLock")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

V dalším kroku Stáhnout [preview.png](creating-a-watchface-images/preview.png) &ndash; se zařadí do **drawables** složku dále v tomto návodu.


## <a name="add-the-xamarin-android-wear-package"></a>Přidejte balíček opotřebení Xamarin Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Spusťte Správce balíčků NuGet (v sadě Visual Studio, klikněte pravým tlačítkem na **odkazy** v **Průzkumníku řešení** a vyberte **spravovat balíčky NuGet...** ). Aktualizovat na nejnovější stabilní verze projektu **Xamarin.Android.Wear**: 

[![Přidat Správce balíčků NuGet](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "přidejte balíček Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Dále pokud **Xamarin.Android.Support.v13** je nainstalovaná, odinstalujte ji:

[![Odebrat Správce balíčků NuGet](creating-a-watchface-images/07-uninstall-v13-sml.png "odebrat Xamarin.Support.v13")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Spusťte Správce balíčků NuGet (v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **balíčky** v **řešení podokně** a vyberte **přidat balíčky...** ). Aktualizovat na nejnovější stabilní verze projektu **Xamarin.Android.Wear**: 

[![Přidat Správce balíčků NuGet](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "přidejte balíček Xamarin.Android.Wear")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


Sestavení a spusťte aplikaci v emulátoru nebo zařízení a opotřebením motoru (Další informace o tom, jak to udělat najdete v tématu [Začínáme](~/android/wear/get-started/index.md) průvodce). Na zařízení a opotřebením motoru, byste měli vidět obrazovce následující aplikace:

[![Snímek obrazovky aplikace](creating-a-watchface-images/08-app-screen.png "obrazovky aplikace v paměti zařízení")](creating-a-watchface-images/08-app-screen.png#lightbox)

Základní aplikaci opotřebení v tomto okamžiku nemá sledovat vzhled funkci, protože ještě neposkytuje implementace služby sledovat řez. Tato služba se přidají další. 

 
## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Podívejte se na Android implementuje opotřebení řezy prostřednictvím `CanvasWatchFaceService` třídy. `CanvasWatchFaceService` je odvozený od `WatchFaceService`, které je odvozený od `WallpaperService` jak je znázorněno v následujícím diagramu: 

[![Diagram dědičnosti](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService dědičnosti diagram")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` obsahuje vnořený `CanvasWatchFaceService.Engine`; vytvoření instance `CanvasWatchFaceService.Engine` objekt, který neodpovídá skutečné práci při kreslení sledovat písmo. `CanvasWatchFaceService.Engine` je odvozený od `WallpaperService.Engine` jak je vidět v diagramu. 

To není znázorněné v tomto diagramu je `Canvas` , `CanvasWatchFaceService` používá pro kreslení tučné sledovat &ndash; to `Canvas` je předaná prostřednictvím `OnDraw` metoda, jak je popsáno níže. 

V následujících částech se vytvoří služba vzhled vlastní sledování pomocí následujících kroků: 

1.  Zadejte třídu s názvem `MyWatchFaceService` je odvozena z `CanvasWatchFaceService`. 

2.  V rámci `MyWatchFaceService`, vytvořit vnořené třídy s názvem `MyWatchFaceEngine` je odvozena z `CanvasWatchFaceService.Engine`. 

3.  V `MyWatchFaceService`, implementovat `CreateEngine` metoda, která vytvoří instanci `MyWatchFaceEngine` a vrátí ji.

4.  V `MyWatchFaceEngine`, implementovat `OnCreate` metodu pro vytvoření styl plochy sledovat a provádět další úlohy inicializace. 

5.  Implementace `OnDraw` metodu `MyWatchFaceEngine`. Tato metoda je volána, když tučné sledování musí být překreslen (tj. *zrušena*). `OnDraw` je metoda, která nevykresluje (nebo ho překreslí) sledovat vzhled prvky, jako jsou například hodinu, minutu a druhý rukou. 

6.  Implementace `OnTimeTick` metodu `MyWatchFaceEngine`. 
    `OnTimeTick` je volána alespoň jednou za minutu (v režimech vedlejším a interaktivní) nebo pokud se změnila data a času. 

Další informace o `CanvasWatchFaceService`, najdete v části pro Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) dokumentaci k rozhraní API.
Podobně [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) vysvětluje skutečné implementace písmo, sledovat.


### <a name="add-the-canvaswatchfaceservice"></a>Přidat CanvasWatchFaceService

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Přidat nový soubor s názvem **MyWatchFaceService.cs** (v sadě Visual Studio, klikněte pravým tlačítkem na **WatchFace** v **Průzkumníku řešení**, klikněte na tlačítko **Přidat > novou položku...** a vyberte **třída**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Přidat nový soubor s názvem **MyWatchFaceService.cs** (v sadě Visual Studio pro Mac, klikněte pravým tlačítkem myši **WatchFace** projektu, klikněte na tlačítko **Přidat > Nový soubor...** a vyberte **prázdné třídy**). 

-----

Obsah tohoto souboru nahraďte následujícím kódem: 

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService` (odvozený z `CanvasWatchFaceService`) je "hlavní aplikace" písmo, sledovat. `MyWatchFaceService` implementuje pouze jednu metodu `OnCreateEngine`, která vytvoří a vrátí `MyWatchFaceEngine` objektu (`MyWatchFaceEngine` je odvozený od `CanvasWatchFaceService.Engine`). Vytvořenou instanci `MyWatchFaceEngine` objekt musí být vrácen jako `WallpaperService.Engine`. Zapouzdřením `MyWatchFaceService` je objekt předaný do konstruktoru. 

`MyWatchFaceEngine` je implementace vzhled skutečné sledovat &ndash; obsahuje kód, který nevykresluje sledovat písmo. Také obstará systémové události, jako jsou například změny obrazovky (vedlejším, interaktivní režimy obrazovky vypnutí, atd.). 

 
### <a name="implement-the-engine-oncreate-method"></a>Implementujte metodu OnCreate modul

`OnCreate` Metoda inicializuje sledovat písmo. Přidejte následující pole, které chcete `MyWatchFaceEngine`: 

```csharp
Paint hoursPaint;
```

To `Paint` objektu se použije k vykreslení aktuální čas na tučné sledovat. Dál přidejte následující metodu do `MyWatchFaceEngine`: 

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate` je volána krátce po `MyWatchFaceEngine` je spuštěná. Nastavuje `WatchFaceStyle` (který určuje způsob opotřebení ze zařízení interakce s uživatelem) a vytvoří `Paint` objekt, který se použije k zobrazení času. 

Volání `SetWatchFaceStyle` provede následující akce:

1.  Nastaví *funkce Náhled režimu* k `PeekModeShort`, což způsobí, že oznámení zobrazí jako karty malé funkce "náhled" v zobrazení. 

2.  Nastaví viditelnost pozadí `Interruptive`, což způsobí, že na pozadí funkce Náhled karty, která se má zobrazit pouze stručně, představuje interruptive oznámení.

3.  Zakáže výchozí doba uživatelského rozhraní systému z přitahuje na tučné sledovat tak, aby vlastní sledování tučné zobrazení času místo.

Další informace o těchto a dalších možnostech styl vzhled sledovat, najdete v části pro Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) dokumentaci k rozhraní API.

Po `SetWatchFaceStyle` dokončení `OnCreate` vytvoří `Paint` objektu (`hoursPaint`) a nastaví její barvu prázdný a jeho velikost textu na 48 pixelů ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) musí být zadán v pixelech). 


### <a name="implement-the-engine-ondraw-method"></a>Implementace modulu OnDraw – metoda

`OnDraw` Metoda je možná nejdůležitější `CanvasWatchFaceService.Engine` metoda &ndash; je metoda, ve skutečnosti nakreslí sledovat vzhled prvky, jako jsou například číslic a taktovací vzhled rukou. V následujícím příkladu nevykresluje řetězec času na tučné sledovat.
Přidejte následující metodu do `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str, 
        (float)(frame.Left + 70), 
        (float)(frame.Top  + 80), hoursPaint);
}
```

Při volání Android `OnDraw`, předává v `Canvas` instance a rozsah, ve kterých lze rozlišovat písmo. Ve výše uvedeném příkladu kódu `DateTime` se používá k výpočtu aktuální čas v hodinách a minutách (ve formátu 12 hodin). Výsledný řetězec času vykreslením na plátně pomocí `Canvas.DrawText` metoda. Řetězec se zobrazí 70 pixelů přes z levý okraj a 80 pixelů dolů z horní okraj. 

Další informace o `OnDraw` metodu, najdete v části pro Android [OnDraw –](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html#onDraw(android.graphics.Canvas, android.graphics.Rect)) dokumentaci k rozhraní API.


### <a name="implement-the-engine-ontimetick-method"></a>Implementujte metodu OnTimeTick modul

Android pravidelně volá `OnTimeTick` metoda aktualizace zobrazí tučné sledování času. Je volána v minimálně jednou za minutu (v vedlejším i interaktivní režim), nebo pokud došlo ke změně data a času nebo časové pásmo. Přidejte následující metodu do `MyWatchFaceEngine`: 

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Tato implementace `OnTimeTick` jednoduše volá `Invalidate`. `Invalidate` Metoda plány `OnDraw` k ho překreslit sledovat písmo. 

Další informace o `OnTimeTick` metodu, najdete v části pro Android [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) dokumentaci k rozhraní API.

 
## <a name="register-the-canvaswatchfaceservice"></a>Registrace CanvasWatchFaceService

`MyWatchFaceService` musí být zaregistrovaný ve **AndroidManifest.xml** přidružené aplikace a opotřebením motoru. K tomu, přidejte následující XML tak, aby `<application>` části: 

```xml
<service 
    android:name="watchface.MyWatchFaceService" 
    android:label="Xamarin Sample" 
    android:allowEmbedded="true" 
    android:taskAffinity="" 
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data 
        android:name="android.service.wallpaper" 
        android:resource="@xml/watch_face" />
    <meta-data 
        android:name="com.google.android.wearable.watchface.preview" 
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

Tato konfigurace XML provede následující akce:

1.  Nastaví `android.permission.BIND_WALLPAPER` oprávnění. Toto oprávnění poskytuje sledování vzhled služby oprávnění ke změně tapetu systému v zařízení. Všimněte si, že toto oprávnění musí být nastavena v `<service>` části spíše než v vnější `<application>` části. 

2.  Definuje `watch_face` prostředků. Tento prostředek je krátký soubor XML, který deklaruje `wallpaper` prostředků (Tento soubor bude vytvořen v další části). 

3.  Deklaruje bitovou kopii drawable názvem `preview` které se bude zobrazovat obrazovku pro výběr výběr sledovat. 

4.  Zahrnuje `intent-filter` umožníte Android vědět, že `MyWatchFaceSevice` zobrazovat sledovat řez. 

Která se dokončí kód pro základní `WatchFace` příklad. Dalším krokem je přidání potřebné prostředky.

 
## <a name="add-resource-files"></a>Přidat soubory prostředků

Než můžete spustit službu sledování, je nutné přidat **watch_face** prostředků a náhled obrázku. Nejprve vytvořte nový soubor XML na **Resources/xml/watch_face.xml** a nahraďte jeho obsah následující kód XML: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Nastavit tento soubor sestavení akce na **AndroidResource**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Akce sestavení](creating-a-watchface-images/10-android-resource-vs.png "akci k AndroidResource sestavení sady")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Akce sestavení](creating-a-watchface-images/10-android-resource-xs.png "akci k AndroidResource sestavení sady")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Tento soubor prostředků definuje jednoduchou `wallpaper` element, který se použije pro sledování písmo. 

Pokud jste tak ještě neučinili, stáhněte si [preview.png](creating-a-watchface-images/preview.png). Nainstalujte ji na **Resources/drawable/preview.png**. Nezapomeňte přidat tento soubor do `WatchFace` projektu. Tento náhled obrázku se zobrazí se uživateli na výběr vzhled sledovat na opotřebení ze zařízení. Vytvoření bitové kopie preview pro vlastní vzhled sledování, může trvat snímek písmo, sledovat je spuštěna. (Další informace o získávání snímky obrazovky z opotřebení ze zařízení najdete v tématu [trvá snímky obrazovky](~/android/wear/deploy-test/debug-on-device.md#screenshots)). 


## <a name="try-it"></a>Můžete je vyzkoušejte.

Vytvoření a nasazení aplikace na zařízení a opotřebením motoru. Měli byste vidět obrazovce opotřebení aplikace se zobrazí jako předtím. Následujícím postupem povolit nový vzhled sledovat: 

1.  Prstem doprava, dokud neuvidíte na pozadí na obrazovce sledovat. 

2.  Touch a podržením kdekoli na pozadí obrazovky na 2 sekundy.

3.  Prstem zleva doprava procházet různé řezy sledovat. 

4.  Vyberte **Xamarin ukázka** sledovat vzhled (zobrazené na pravé straně): 

    [![Výběr Watchface](creating-a-watchface-images/11-watchface-picker.png "prstem najít vzhled sledovat ukázka Xamarin")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  Klepněte na **Xamarin ukázka** sledovat vzhled ji vyberte. 

Tato operace změní písmo sledovat opotřebení ze zařízení používat službu vzhled vlastní sledování implementována, pokud: 

[![Vzhled digitální sledovat](creating-a-watchface-images/12-digital-watchface.png "vlastní digitální sledovat spuštěné na opotřebení ze zařízení")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Je to řez relativně hrubých sledovat, proto je tak minimální implementace aplikace (například neobsahuje čelní strany pozadí sledovat a není volání `Paint` vyhlazení metody zlepšit vzhled). Ale ho implementovat holou funkce, které jsou potřeba k vytvoření vlastních sledovat řez. 

V další části tohoto sledování vzhled upgraduje sofistikovanější implementaci. 

 
## <a name="upgrading-the-watch-face"></a>Upgrade tučné sledování

Ve zbývající části v tomto návodu `MyWatchFaceService` upgradu zobrazíte analogovým stylu sledovat vzhled a je rozšířené k podpoře více funkcí. K vytvoření tučné upgradovaný sledování budou přidány následující možnosti: 

1.  Označuje čas s analogovým hodinu, minutu a druhý rukou.

2.  Reaguje na změny v viditelnosti.

3.  Reaguje na změny mezi vedlejším režimu a interaktivním režimu. 

4.  Načte vlastnosti základní opotřebení ze zařízení.

5.  Je čas při změně časové pásmo probíhá automaticky aktualizuje. 

Před implementací změny kódu níže, stáhněte si [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), rozbalte ho a přesunout soubory rozbalené .png **prostředky/drawable** (přepsat předchozí **preview.png**). Přidat nové soubory PNG `WatchFace` projektu.


### <a name="update-engine-features"></a>Aktualizace modulu funkce

Dalším krokem je upgrade **MyWatchFaceService.cs** na implementaci nevykresluje řez analogovým sledovat, která podporuje nové funkce. Nahraďte obsah **MyWatchFaceService.cs** analogovým verzí kód vzhled sledovat [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (můžete vyjmout a vložit tento zdroj do existujících  **MyWatchFaceService.cs**). 

Tato verze **MyWatchFaceService.cs** přidá další kód existující metody a obsahuje další přepsané metody pro přidání dalších funkcí. Následující části obsahují seznámení zdrojového kódu.

#### <a name="oncreate"></a>OnCreate

Aktualizovaný **OnCreate** metoda nakonfiguruje styl sledovat plochy jako předtím, ale zahrnuje některé další kroky: 

1.  Nastaví obrázek pozadí **xamarin_background** prostředek, který se nachází v **Resources/drawable-hdpi/xamarin_background.png**. 

2.  Inicializuje `Paint` objektů pro kreslení ruční hodinu, minutu ruční a druhé straně.

3.  Inicializuje `Paint` objekt pro vykreslení rysky hodinu kolem okraje sledovat písmo. 

4.  Vytvoří časovač tohoto volání `Invalidate` – metoda (překreslování) tak, aby druhé straně bude překreslen každou sekundu. Všimněte si, že tento časovač je nutné protože `OnTimeTick` volání `Invalidate` jenom jednou na každou minutu.

Tento příklad obsahuje pouze jeden **xamarin_background.png** obraz; však můžete chtít vytvořit různé pozadí bitovou kopii pro každý hustotě, která bude podporovat vaše vlastní sledování vzhled. 

#### <a name="ondraw"></a>OnDraw

Aktualizovaný **OnDraw –** metoda nevykresluje řez analogovým stylu sledovat pomocí následujících kroků: 

1.  Získá aktuální čas, který je nyní udržován v `time` objektu. 

2.  Určuje rozsah kreslicí plochy a jeho center.

3.  Kreslení na pozadí, když pozadí se vykresluje přizpůsobí zařízení se.

4.  Číslo dvanáct nevykresluje *rysky* kolem tučné hodin (odpovídající hodin na hodin). 

5.  Vypočítá úhel, otáčení a délka pro každé straně sledovat.

6.  Na ploše sledovat nevykresluje každé straně. Všimněte si, že pokud sledovat je v režimu vedlejším není vykreslován druhé straně. 

 
#### <a name="onpropertieschanged"></a>OnPropertiesChanged

Tato metoda je volána k informování `MyWatchFaceEngine` o vlastnostech opotřebení ze zařízení (například vedlejším režimu nízká bitů a zápis na disk v ochrany). V `MyWatchFaceEngine`, tato metoda zkontroluje pouze pro nízké bit vedlejším režimu (v režimu nízkou verzi vedlejším obrazovky podporuje méně bits pro každou barvu). 

Další informace o této metodě naleznete v tématu Android [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) dokumentaci k rozhraní API.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Tato metoda je volána, když zařízení opotřebení zadá nebo ji opustí vedlejším režimu. V `MyWatchFaceEngine` implementace, sledovat tučné zakáže vyhlazení při v vedlejším režimu. 

Další informace o této metodě naleznete v tématu Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) dokumentaci k rozhraní API.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

Tato metoda je volána vždy, když se stane hodinek viditelný nebo skrytý. V `MyWatchFaceEngine`, tato metoda registruje/zrušení registrace příjemce časové pásmo (popsaný níže) podle stav viditelnosti. 

Další informace o této metodě naleznete v tématu Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) dokumentaci k rozhraní API.

 
### <a name="time-zone-feature"></a>Funkce časového pásma

Nové **MyWatchFaceService.cs** také zahrnuje funkci aktualizovat aktuální čas vždy, když čas zónu změny (například při cestě mezi různými časovými pásmy). Téměř na konci **MyWatchFaceService.cs**, čas změnit zóny `BroadcastReceiver` je definován, která zpracovává změnit časové pásmo záměrné objekty: 

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

`RegisterTimezoneReceiver` a `UnregisterTimezoneReceiver` metody jsou volány `OnVisibilityChanged` metoda. 
`UnregisterTimezoneReceiver` je volána, když se změní stav viditelnosti písmo, sledovat na skrytá. Při sledování čelí, je viditelná znovu `RegisterTimezoneReceiver` nazývá (najdete v článku `OnVisibilityChanged` metoda). 

Modul `RegisterTimezoneReceiver` metoda deklaruje obslužnou rutinu pro toto časové pásmo příjemce `Receive` událostí; Tato obslužná rutina aktualizace `time` objekt s nový čas vždy, když je překročí časové pásmo: 

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Filtr záměrné je vytvořená a zaregistrovaná pro příjemce časové pásmo:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver` Metoda zruší registraci příjemce časové pásmo:

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Spustit řez vylepšené sledování

Vytvoření a nasazení aplikace na zařízení a opotřebením motoru znovu. Vyberte tučné sledovat vzhled výběru sledovat jako před. Ve verzi preview v dialogu pro výběr sledování se zobrazí na levé straně a nový vzhled sledování se zobrazí na pravé straně:

[![Vzhled analogovým sledovat](creating-a-watchface-images/13-analog-watchface.png "vylepšené analogovým řez v výběr a na zařízení")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

Na tomto snímku obrazovky je ruční sekundu přesunutí jednou za sekundu. Při spuštění tohoto kódu na zařízení a opotřebením motoru, druhé straně zmizí při hodinek zadá vedlejším režimu.

 
## <a name="summary"></a>Souhrn

V tomto návodu se vlastní Android nosit watchface implementována a otestovat. `CanvasWatchFaceService` a `CanvasWatchFaceService.Engine` tříd zavedených a základní metody třídy modul byly implementovány vytvořit jednoduché digitální sledovat řez. Tato implementace byla aktualizována s další funkce k vytvoření analogovým sledovat vzhled a další metody, které byly implementovány zpracovat změny v viditelnosti, vedlejším režimu a rozdíly ve vlastnosti zařízení. Nakonec časové pásmo všesměrového vysílání příjemce byl implementován tak, aby hodinek automaticky aktualizuje čas, kdy je překročí časové pásmo. 


## <a name="related-links"></a>Související odkazy

- [Vytváření řezy sledování](https://developer.android.com/training/wearables/watch-faces/index.html)
- [Ukázka WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
