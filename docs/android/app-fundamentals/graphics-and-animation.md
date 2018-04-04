---
title: Grafika a animace
description: Android poskytuje velmi bohatý a různých rozhraní pro podporu 2D grafiky a animace. Toto téma uvádí tyto architektury a popisuje, jak vytvořit vlastní grafika a animací pro použití v aplikaci Xamarin.Android.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 85a7cd2ac5094a4760c5465bd099ce2e3beeae79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="graphics-and-animation"></a>Grafika a animace

_Android poskytuje velmi bohatý a různých rozhraní pro podporu 2D grafiky a animace. Toto téma uvádí tyto architektury a popisuje, jak vytvořit vlastní grafika a animací pro použití v aplikaci Xamarin.Android._


## <a name="overview"></a>Přehled

Navzdory běžící na zařízení, které jsou tradičně omezené napájení, nejvyšší hodnocení mobilní aplikace mají často a prostředí sofistikované uživatele (UX), s vysoce kvalitního grafiky a animací, které poskytují intuitivní, reakce, dynamické chování. Jako mobilní aplikace get více pokročilé, uživatelé jste začali se očekává více z aplikací.

Naštěstí pro nás, moderní mobilní platformy mít velmi výkonné rozhraní pro vytváření sofistikované animace a vlastní grafiky při zachování snadné použití. To umožňuje vývojářům přidat bohaté možnosti interakce s velmi malé úsilí.

Rozhraní API uživatelského rozhraní v Android můžete zhruba rozdělit do dvou kategorií: Grafika a animace.

Grafika jsou dále rozdělit na různé přístupy k provádění 2D a 3D grafiky. 3D grafiky jsou dostupné prostřednictvím několika součástí architektury, jako je například OpenGL ES (mobilní určitou verzi sady OpenGL) a rozhraní třetích stran, jako je například MonoGame (toolkit kompatibilní s XNA toolkit křížové platformy). I když 3D grafický nejsou v rámci oboru v tomto článku, vyzkoušíme předdefinované 2D kreslení techniky.

Android poskytuje dvě různé rozhraní API pro vytváření 2D grafiky. Jedna je nejvyšší úrovni deklarativní přístup a dalších programový nízké úrovně rozhraní API:

-   **Drawable prostředky** &ndash; tyto slouží k vytváření vlastních grafiky buď programově, nebo (více obvykle) vložením kreslení pokyny v souborů XML. Drawable prostředky jsou obvykle definovány jako soubory XML, které obsahují pokyny, nebo akce pro Android k vykreslení 2D obrázek. 

-   **Plátno** &ndash; Toto je nízké úrovni rozhraní API, které zahrnuje kreslení přímo na základní rastrového obrázku. Poskytuje velmi jemně odstupňovanou kontrolu nad co se zobrazí. 

Kromě těchto postupů grafiky 2D Android také poskytuje několik různých způsobů vytvoření animací:

-   **Drawable animací** &ndash; Android podporuje i po jednotlivých-animací známé jako *Drawable animace*. Toto je nejjednodušší animace rozhraní API. Android postupně načte a zobrazí Drawable prostředky v pořadí (podobně jako komiks).

-   **Zobrazení animací** &ndash; *animace zobrazení* jsou původní animace rozhraní API v Android a jsou dostupné ve všech verzích systému Android. Toto rozhraní API je omezená, ji budou fungovat jenom s objekty zobrazení a může provádět pouze jednoduché transformace na těchto zobrazení.
    Animace zobrazení jsou obvykle definovány v soubory XML `/Resources/anim` složky.

-   **Vlastnost animací** &ndash; Android 3.0 zavedly novou sadu animace rozhraní API je známý jako *vlastnost animací*. Tyto nové rozhraní API se zavedl systémem rozšiřitelný a flexibilní, který slouží k animace vlastnosti všech objektů, ne jenom zobrazit objekty. Tato možnost umožňuje animací zapouzdřit do různých tříd, které budou snadnější sdílení kódu.


Animace zobrazení jsou nejvhodnější pro aplikace, které musí podporovat rozhraní starší předběžné Android 3.0 API (API úrovně 11). Jinak aplikace by měl používat v novější vlastnost animace rozhraní API z důvodů, které byly uvedených výše.

Všechny tyto architektury jsou přijatelná možnosti, ale kde je to možné, by měl dává animací vlastnost, protože je flexibilnější rozhraní API pro práci s. Vlastnost animace umožňují animace logiky zapouzdřit do různých třídy, která umožňuje snadnější sdílení kódu a zjednodušuje kód údržby.


## <a name="accessibility"></a>Usnadnění

Grafika a animací umožňující vytváření aplikací pro Android přitažlivé a fun se použije. je ale důležité si pamatovat, že dojde k některé interakce prostřednictvím screenreaders, alternativní vstupní zařízení, nebo s odbornou přiblížení.
Některé interakce může dojít bez funkce zvuku.

Aplikace jsou více použitelné v těchto situacích, pokud byly navrženy s usnadnění v paměti: poskytnutí pomocné parametry a pomoc navigace v uživatelském rozhraní a zajištění není textového obsahu a popisy pro obrazové prvky uživatelského rozhraní.

Odkazovat na [Google usnadnění průvodce](http://developer.android.com/guide/topics/ui/accessibility/) Další informace o tom, jak využívat usnadnění Android na rozhraní API.



## <a name="2d-graphics"></a>2D grafiky

Drawable prostředky jsou oblíbených technika v aplikacích Android. Jako další prostředky, Drawable prostředky jsou deklarativní &ndash; jste definované v souborů XML. Tento přístup umožňuje čistou oddělení kódu z prostředků. To může zjednodušit vývoj a údržba, protože není potřeba změnit kód pro aktualizace nebo změna grafiky v aplikaci pro Android. Však Drawable prostředky jsou užitečné pro mnoho jednoduché a běžné grafické požadavky, nemají napájení a řízení plátno rozhraní API.

Další techniku, pomocí [plátno](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) objektu, je velmi podobný jiné tradiční rozhraní API například System.Drawing nebo kreslení jádra pro iOS. Pomocí objektu plátno poskytuje maximální kontrolu jak 2D grafiky jsou vytvořeny. Je vhodný pro situace, kdy prostředek Drawable nebude fungovat, nebo bude obtížné pracovat. Například může být nutné k vykreslení vlastní ovládací prvek typu jezdec jejichž vzhled se změní na základě výpočtů související s hodnotu jezdce.

Umožňuje prozkoumat Drawable prostředky. Jsou jednodušší a zahrnují nejběžnější případy vlastní vykreslení.


### <a name="drawable-resources"></a>Drawable prostředky

Drawable prostředky jsou definovány v souboru XML v adresáři `/Resources/drawable`. Na rozdíl od vložení, PNG nebo JPEG společnosti, není potřeba zadat hustotu specifické verze Drawable prostředků.
Za běhu bude aplikace platformy Android načíst tyto prostředky a postupujte podle pokynů obsažených v tyto soubory XML pro vytváření 2D grafiky.
Android definuje několika různých typů Drawable prostředků:

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; jde Drawable objekt, který nevykresluje primitivní geometrické obrazce a použije omezenou sadu grafické efekty v tomto tvaru. Jsou užitečné pro akcí, například přizpůsobení tlačítka nebo nastavení na pozadí TextViews. Vidíte příklad použití `ShapeDrawable` dále v tomto článku.

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; jde Drawable prostředek, který bude změnit vzhled na základě stavu pomůcky a ovládání. Tlačítko může například změnit její vzhled v závislosti na tom, jestli je stisknutí nebo ne.

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; tento Drawable prostředek, který se bude skládat několik drawables jeden na jiný. Příklad *LayerDrawable* je znázorněno na následujícím snímku obrazovky:

    ![Příklad LayerDrawable](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; jde *LayerDrawable* s jedním rozdílem. A *TransitionDrawable* je schopen animace jedné vrstvy zobrazovat nad horní jiné.

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; to je velmi podobné *StateListDrawable* v, aby zobrazoval image podle určité podmínky. Ale na rozdíl od *StateListDrawable*, *LevelListDrawable* zobrazí obrázek založené na celočíselnou hodnotu. Příklad *LevelListDrawable* by zobrazíte sílu signál Wi-Fi. Jako sílu změn signál Wi-Fi drawable, který se zobrazí se změní.

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; jako jejich název napovídá, zadejte tyto Drawables škálování i výstřižek funkce. *ScaleDrawable* bude škálovat jiné Drawable, než *ClipDrawable* bude v případě potřeby upraví jiné Drawable.

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; tento Drawable budou platit vsazení na stranách Drawable jiný prostředek. Používá se při zobrazení musí na pozadí, která je menší než skutečná rozsah zobrazení.

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; tento soubor je sada pokynů v XML, které se mají provést na skutečné rastrového obrázku. Některé akce, které může provádět Android jsou dlaždicové vyplnění, tónování a vyhlazení. Jedním z velmi běžná použití tohoto objektu je dlaždici rastrový obrázek na pozadí a rozložení.


#### <a name="drawable-example"></a>Příklad drawable

Podívejme se na zběžný příklad vytvoření 2D grafické pomocí `ShapeDrawable`. A `ShapeDrawable` můžete definovat jeden čtyři základní tvary: obdélníku, oval, řádku a prstenec. Je také možné použít základní důsledky, jako je například přechodu, barva a velikost. Následující kód XML je `ShapeDrawable` , najdete v *AnimationsDemo* doprovodné projektu (v souboru `Resources/drawable/shape_rounded_blue_rect.xml`).
Definuje obdélníku s fialové barevného přechodu pozadí a zaokrouhlené rozích:

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
<!-- Specify a gradient for the background -->
<gradient android:angle="45"
          android:startColor="#55000066"
          android:centerColor="#00000000"
          android:endColor="#00000000"
          android:centerX="0.75" />

<padding android:left="5dp"
          android:right="5dp"
          android:top="5dp"
          android:bottom="5dp" />

<corners android:topLeftRadius="10dp"
          android:topRightRadius="10dp"
          android:bottomLeftRadius="10dp"
          android:bottomRightRadius="10dp" />
</shape>
```

Jsme odkazovat tento prostředek Drawable deklarativně v rozložení nebo jiných Drawable, jak je znázorněno v následující soubor XML:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#33000000">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_centerInParent="true"
              android:background="@drawable/shape_rounded_blue_rect"
              android:text="@string/message_shapedrawable" />
</RelativeLayout>
```

Drawable prostředkům můžete použít také prostřednictvím kódu programu. Následující fragment kódu ukazuje, jak nastavit prostřednictvím kódu programu na pozadí TextView:

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Pokud chcete zjistit, jaké to bude vypadat, spusťte *AnimationsDemo* projektu a vyberte tvar Drawable položku z hlavní nabídky. Bychom měli vidět něco podobného jako na následujícím snímku obrazovky:

![TextView s vlastní pozadí drawable s přechodu a zaokrouhlené rozích](graphics-and-animation-images/image1.png)

Další podrobnosti o syntaxi Drawable prostředků a elementy XML, najdete [Google dokumentaci](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).


### <a name="using-the-canvas-drawing-api"></a>Pomocí rozhraní API pro kreslení plátno

Drawables jsou efektivní, ale mají svá omezení. Některé věci není možné nebo velmi složité (například: použití filtru na obrázek, který byl provedenou fotoaparát v zařízení). Je velmi obtížné použít redukce červených očí s využitím Drawable prostředků.
Místo toho rozhraní API plátno umožňuje aplikaci tak, aby měl velmi jemně odstupňovanou kontrolu selektivně Změna barev v konkrétní část obrázku.

Jednu třídu, která se běžně používá s plátno je [Malování](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) třídy. Tato třída obsahuje barvy a stylu informace o tom, jak kreslení. Slouží k poskytování věcí barvy a průhlednost.

Používá rozhraní API plátno *kopírovat na modelu* k vykreslení grafiky 2D.
Operace se použijí v následných vrstvy na sebe. Každé operace se bude zabývat některé oblasti základní rastrového obrázku. Když oblasti překrývá dříve malovaného oblasti, nové Malování bude částečně nebo zcela skrývat starý. Toto je stejným způsobem, že mnoho dalších kreslení rozhraní API například System.Drawing a iOS je základní grafiky fungovat.

Existují dva způsoby, jak získat `Canvas` objektu. První způsob zahrnuje definování [rastrový obrázek](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) objekt a potom vytváření instancí `Canvas` objektu s ním. Například následující fragment kódu vytvoří nové plátno s základní rastrového obrázku:

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Jiný způsob, jak získat `Canvas` objekt je [OnDraw –](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) metoda zpětného volání, která je k dispozici [zobrazení](https://developer.xamarin.com/api/type/Android.Views.View/) základní třídy. Android volá tuto metodu, když rozhoduje, zobrazení potřebuje k vykreslení sám a předá `Canvas` objekt pro zobrazení pro práci s.

Třída plátno zpřístupní metody jako prostřednictvím kódu programu kreslení pokyny. Příklad:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; zadaný Malování vyplní celé plátno rastrového obrázku.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; nevykresluje zadaný geometrické obrazce pomocí zadané Malování.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; nevykresluje text na plátně se zadanou barvou. Text je vykreslovat umístění `x,y` .



#### <a name="drawing-with-the-canvas-api"></a>Kreslení pomocí rozhraní API na plátno

Podíváme se na příklad plátno rozhraní API v akci. Následující fragment kódu ukazuje, jak k vykreslení zobrazení:

```csharp
public class MyView : View
{
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        Paint green = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0x99, 0xcc, 0),
        };
        green.SetStyle(Paint.Style.FillAndStroke);

        Paint red = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0xff, 0x44, 0x44)
        };
        red.SetStyle(Paint.Style.FillAndStroke);

        float middle = canvas.Width * 0.25f;
        canvas.DrawPaint(red);
        canvas.DrawRect(0, 0, middle, canvas.Height, green);
    }
}
```

Tento kód výše nejprve vytvoří red Malování a zelená Malování objektu. Vyplní celé plátno obsah s červenou a potom dá pokyn na plátno zelená obdélník, který je 25 % šířku na plátno. Příkladem může zobrazit v `AnimationsDemo` projekt, který je součástí zdrojového kódu v tomto článku. Spuštění aplikace a výběrem kreslení položku z hlavní nabídky, jsme měli obrazovky, který je podobný následujícímu:

![Obrazovky red Malování a objekty zelená Malování](graphics-and-animation-images/image3.png)


## <a name="animation"></a>Animace

Uživatelé jako věcí, které přepínají ve svých aplikacích. Animace jsou skvělý způsob, jak zlepšit uživatelské prostředí aplikace a pomáhají zvýraznění. Nejlepší animace jsou ty, které uživatelé nejsou Všimněte si, protože budou mít přirozené. Android poskytuje následující tři API pro animací:

-   **Zobrazit animace** &ndash; Toto je původní rozhraní API. Tyto animací, jsou svázané s konkrétní zobrazení a můžete provádět jednoduché transformace na obsah zobrazení. Z důvodu jeho jednoduchost jako například toto rozhraní API stále užitečné pro věcí alfa animací, otáčení a tak dále.

-   **Vlastnost animace** &ndash; vlastnost animací byly zavedeny v systému Android 3.0. Umožňují animace prakticky jakoukoli aplikaci. Vlastnost animace se používat ke změně žádnou vlastnost libovolného objektu, i když tento objekt není viditelný na obrazovce.

-   **Drawable animace** &ndash; to speciální Drawable prostředek, který se používá k aplikování velmi jednoduché animace ovlivňuje rozložení.

Obecně platí vlastnost animace je upřednostňovaný systému použít, protože je flexibilnější a nabízí další funkce.


### <a name="view-animations"></a>Zobrazení animací

Animace zobrazení jsou omezeny na zobrazení a může provádět pouze animací na hodnotách, jako je například počáteční a koncové body, velikosti, otáčení a průhlednost.
Tyto typy animací se obvykle označují jako *doplnění animací*. Dva způsoby, jak lze definovat zobrazení animací &ndash; prostřednictvím kódu programu v kódu nebo pomocí souborů XML. Soubory XML jsou upřednostňovaný způsob deklarace animace zobrazení, protože se jedná o čitelnější a snadněji provádět údržbu.

Soubory XML animace bude uložen v `/Resources/anim` adresář projektu Xamarin.Android. Tento soubor musí mít jednu z následujících prvků jako kořenový element:

-   `alpha` &ndash; Vysouvaly nebo fade-out animace.

-   `rotate` &ndash; Otočení animace.

-   `scale` &ndash; Změny velikosti animace.

-   `translate` &ndash; Vodorovné nebo svislé pohybu.

-   `set` &ndash; Kontejner, který může obsahovat jednu nebo více dalších prvků animace.

Standardně se použijí všechny animace v souboru XML, který současně. Chcete-li animací dojít postupně, nastavte `android:startOffset` atribut na jeden z elementů definované výše.

Je možné mít vliv na rychlost změny v animace pomocí *interpolator*. Interpolátor umožňuje animace efekty accelerated, opakované nebo decelerated. Rozhraní Android poskytuje několik interpolators předinstalované, jako třeba (ale bez omezení):

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; Tyto interpolators zvýšení nebo snížení míry změny v animace.

-   `BounceInterpolator` &ndash; Změna nedoručitelných zpráv na konci.

-   `LinearInterpolator` &ndash; Míra změn je konstantní.


Následující kód XML je příkladem soubor animace, který kombinuje některé z těchto prvků:

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android=http://schemas.android.com/apk/res/android
     android:shareInterpolator="false">

    <scale android:interpolator="@android:anim/accelerate_decelerate_interpolator"
           android:fromXScale="1.0"
           android:toXScale="1.4"
           android:fromYScale="1.0"
           android:toYScale="0.6"
           android:pivotX="50%"
           android:pivotY="50%"
           android:fillEnabled="true"
           android:fillAfter="false"
           android:duration="700" />

    <set android:interpolator="@android:anim/accelerate_interpolator">
        <scale android:fromXScale="1.4"
               android:toXScale="0.0"
               android:fromYScale="0.6"
               android:toYScale="0.0"
               android:pivotX="50%"
               android:pivotY="50%"
               android:fillEnabled="true"
               android:fillBefore="false"
               android:fillAfter="true"
               android:startOffset="700"
               android:duration="400" />

        <rotate android:fromDegrees="0"
                android:toDegrees="-45"
                android:toYScale="0.0"
                android:pivotX="50%"
                android:pivotY="50%"
                android:fillEnabled="true"
                android:fillBefore="false"
                android:fillAfter="true"
                android:startOffset="700"
                android:duration="400" />
    </set>
</set>
```

Tato animace provede všechny animací současně. První animace škálování bude roztažení obrázku vodorovně a svisle ho zmenšit a pak bude otočený 45 stupňů proti směru hodinových ručiček a zmenšit, ztrácejí ze obrazovky bitovou kopii současně.

Animace můžete programově použijí na zobrazení nafouknutí animace a jeho použitím na zobrazení. Pomocná třída poskytuje Android `Android.Views.Animations.AnimationUtils` , bude narůstat prostředek animace a vrátit instanci třídy `Android.Views.Animations.Animation`. Tento objekt se použije k zobrazení voláním `StartAnimation` a předání `Animation` objektu. Následující fragment kódu ukazuje příklad tohoto:

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Teď, když máme základní znalosti o fungování animace zobrazení, umožňuje přesunout na vlastnost animace.


### <a name="property-animations"></a>Vlastnost animace

Vlastnost animátoři jsou nové rozhraní API, která byla zavedena v systému Android 3.0.
Poskytují více rozšiřitelné rozhraní API, které slouží k animace libovolné vlastnosti libovolného objektu.

Všechny vlastnosti animace jsou vytvořené pomocí instance [Animator](https://developer.xamarin.com/api/type/Android.Animation.Animator/) podtřídy. Aplikace přímo nepoužívejte tuto třídu, místo toho používají jeden z jeho podtřídy:

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; Tato třída je nejdůležitější – třída v celé vlastnost animace rozhraní API. Vypočítá hodnoty vlastností, které se musí změnit. `ViewAnimator` Nelze přímo aktualizovat tyto hodnoty; místo toho vyvolá události, které můžete použít k aktualizaci animovaný objekty.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; Tato třída je podtřídou třídy `ValueAnimator` . Smyslem je ke zjednodušení procesu Animování objektů přijetím cílový objekt a vlastnost aktualizovat.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; Tato třída je zodpovědná za Orchestrace, jak spouštět animací ve vztahu k sobě. Animace může spustit současně, postupně nebo s zadaný zpožděním mezi nimi.


*Vyhodnocovací filtry* jsou speciální třídy, které jsou používány animátoři k výpočtu nové hodnoty během animace. Ihned poskytuje Android následující vyhodnocovací filtry:

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; vypočítá hodnoty vlastností celé číslo.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; vypočítá hodnoty pro vlastnosti typu float.

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; vypočítá hodnoty pro vlastnosti barvy.

Pokud není vlastnost, která je právě animovaný `float`, `int` nebo barvy, aplikace mohou vytvářet své vlastní vyhodnocování implementací `ITypeEvaluator` rozhraní. (Implementace vlastní vyhodnocovací filtry je nad rámec tohoto tématu.)

#### <a name="using-the-valueanimator"></a>Pomocí ValueAnimator

Pro všechny animace skládá ze dvou částí: výpočet hodnot animovaný a potom tyto hodnoty nastavení na vlastnosti některé objektu. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) pouze vypočítá hodnoty, ale jeho nebude fungovat u objektů přímo. Místo toho se budou aktualizovat objekty uvnitř obslužné rutiny, které bude vyvolán při životnost animace. Tento návrh umožňuje několik vlastností aktualizovat z animovaný jedné hodnoty.

Získat instanci `ValueAnimator` voláním jednu z následujících metod vytváření:

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

Potom, `ValueAnimator` , je nutné nastavit jeho trvání, a pak jeho spuštění. Následující příklad ukazuje, jak animace hodnotu od 0 do 1 přes rozpětí 1000 milisekund:

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Ale samostatně, výše uvedeném fragmentu kódu není velmi užitečné &ndash; animator budou spuštěny, ale neexistuje žádný cíl pro aktualizované hodnoty. `Animator` Třída vyvolá událost aktualizace, když se rozhodne, že je nutné k informování naslouchací procesy nové hodnoty. Aplikace může poskytovat obslužné rutiny události reagovat na tuto událost, jak je znázorněno v následující fragment kódu:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

animator.Update += (object sender, ValueAnimator.AnimatorUpdateEventArgs e) =>
{
    int newValue = (int) e.Animation.AnimatedValue;
    // Apply this new value to the object being animated.
    myObj.SomeIntegerValue = newValue;
};
```

Teď, když máme představu o `ValueAnimator`, umožňuje zobrazit další informace o `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>Pomocí ObjectAnimator

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) je podtřídou třídy `ViewAnimator` který kombinuje výpočet časování modul a hodnotu z `ValueAnimator` s logiku potřebnou k propojit až obslužné rutiny událostí. `ValueAnimator` Vyžaduje aplikace explicitně propojit až obslužné rutiny události &ndash; `ObjectAnimator` se postará o tento krok pro nás.

Rozhraní API pro `ObjectAnimator` je velmi podobný rozhraní API pro `ViewAnimator`, ale vyžaduje, aby zadali objektu a název vlastnosti aktualizace. Následující příklad ukazuje příklad použití `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Jak je vidět na předchozím fragmentu kódu `ObjectAnimator` může snížit a zjednodušit kód, který je potřeba animace objekt.


### <a name="drawable-animations"></a>Drawable animace

Poslední animace rozhraní API je Drawable API animace. Drawable animace načíst řadu Drawable prostředky jedna po druhé a zobrazit postupně, podobně jako komiks překlopit it.

Drawable prostředky jsou definovány v souboru XML, který má `<animation-list>` element jako kořenový element a řadu `<item>` prvky, které definují každý snímek animace. Tento soubor XML je uložen v `/Resource/drawable` složky aplikace. Následující kód XML je příkladem drawable animace:

```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@drawable/asteroid01" android:duration="100" />
  <item android:drawable="@drawable/asteroid02" android:duration="100" />
  <item android:drawable="@drawable/asteroid03" android:duration="100" />
  <item android:drawable="@drawable/asteroid04" android:duration="100" />
  <item android:drawable="@drawable/asteroid05" android:duration="100" />
  <item android:drawable="@drawable/asteroid06" android:duration="100" />
</animation-list>
```

Tato animace se spustí až šest rámce. `android:duration` Atribut uvádí, jak dlouho se zobrazí každý snímek. Následující fragment kódu ukazuje příklad vytvoření Drawable animace a potom ji spustit, když uživatel klikne na tlačítko na obrazovce:

```csharp
AnimationDrawable _asteroidDrawable;

protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    _asteroidDrawable = (Android.Graphics.Drawables.AnimationDrawable)
    Resources.GetDrawable(Resource.Drawable.spinning_asteroid);

    ImageView asteroidImage = FindViewById<ImageView>(Resource.Id.imageView2);
    asteroidImage.SetImageDrawable((Android.Graphics.Drawables.Drawable) _asteroidDrawable);

    Button asteroidButton = FindViewById<Button>(Resource.Id.spinAsteroid);
    asteroidButton.Click += (sender, e) =>
    {
        _asteroidDrawable.Start();
    };
}
```

V tuto chvíli jsme mít zahrnutých základů animace rozhraní API dostupná v aplikaci pro Android.


## <a name="summary"></a>Souhrn

Tento článek zavedená mnoho nových konceptů a rozhraní API je můžete přidat některé grafiky do aplikace systému Android. Nejdřív popsané různé 2D grafické rozhraní API a ukázán jak Android umožňuje aplikacím kreslení přímo na obrazovku pomocí objektu plátno. Také jsme viděli některé alternativní technik, které umožňují grafiky deklarativně vytvořit pomocí souborů XML. Potom jsme se na zabývat v staré a nové rozhraní API pro vytváření animace v Android.



## <a name="related-links"></a>Související odkazy

- [Ukázkové animace (ukázka)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [Animace a grafiky](http://developer.android.com/guide/topics/graphics/index.html)
- [Oživte své mobilní aplikace pomocí animace](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Plátno](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [Objekt Animator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [Hodnota Animator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
