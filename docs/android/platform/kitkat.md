---
title: Funkce KitKat
description: Android 4.4 (KitKat) se dodává s cornucopia funkcí pro uživatele a vývojářům načíst. Tato příručka označuje některé z těchto funkcí a poskytuje příklady kódu a podrobnosti implementace, který vám pomůže provádět naplno KitKat.
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 3c3eafc8dc18113080dd6c906025556292c43e1c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773735"
---
# <a name="kitkat-features"></a>Funkce KitKat

_Android 4.4 (KitKat) se dodává s cornucopia funkcí pro uživatele a vývojářům načíst. Tato příručka označuje některé z těchto funkcí a poskytuje příklady kódu a podrobnosti implementace, který vám pomůže provádět naplno KitKat._

## <a name="overview"></a>Přehled

Android 4.4 (API úrovně 19), také známé jako "KitKat", byla vydána v pozdní 2013. KitKat nabízí celou řadu nových funkcí a vylepšení, včetně:

-  [Činnost koncového uživatele](#user_experience) &ndash; snadné animace s framework přechodu, průhledná stavu a navigační panely a dokonalé režim celé obrazovky pomohou vytvořit lepší prostředí pro uživatele.

-  [Obsah uživatele](#user_content) &ndash; zjednodušená správa souborů uživatelů s úložiště přístup framework; tisku obrázků, webů a jiný obsah je snazší s tiskem vylepšené rozhraní API.

-  [Hardware](#hardware) &ndash; zapnout jakékoliv aplikace do NFC karet se NFC emulace karty založené na hostiteli; spustit senzorů úsporného režimu s `SensorManager` .

-  [Nástroje pro vývojáře](#developer_tools) &ndash; aplikace záznam dění na monitoru v akci s Android ladění most klienta, k dispozici v rámci sady SDK pro Android.


Tato příručka obsahuje pokyny k migraci stávající aplikace Xamarin.Android KitKat a také přehled KitKat pro vývojáře v Xamarin.Android.

## <a name="requirements"></a>Požadavky

K vývoji aplikací Xamarin.Android pomocí KitKat, je nutné *Xamarin.Android 4.11.0* nebo vyšší a Android 4.4 (API úrovně 19), nainstalovaný prostřednictvím Android SDK Manager vidíte na následujícím snímku obrazovky:

[![Výběr Android 4.4 ve správci sady SDK pro Android](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Migrace vaší aplikace do KitKat

Tato část obsahuje některé položky první odpověď, abyste přechod stávající aplikace pro Android 4.4.

### <a name="check-system-version"></a>Zkontrolujte verzi systému

Pokud aplikace musí být kompatibilní se staršími verzemi systému Android, ujistěte se, že jste zalomení kódu pro všechny konkrétní KitKat v kontrolu verze systému, které jsou popsány v následující ukázka kódu:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Dávkování výstrahy

Android používá služby alarmů probuzení aplikace na pozadí v zadanou dobu. KitKat to trvá další krok ve dávkování výstrahy pro zachování napájení. To znamená, že místo na přesný čas probuzení každou aplikaci, KitKat upřednostní seskupit několik aplikací, které jsou registrovány k probuzení během stejné časového intervalu a probuzení je ve stejnou dobu.
Říct Android probuzení aplikace během zadaného časového intervalu, volání `SetWindow` na [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/)a předejte minimální a maximální dobu v milisekundách, která může uplynout, než je probuzený aplikace a na provedení operace Při probuzení.
Následující kód představuje příklad aplikace, která musí být probuzený půl hodiny až jednu hodinu od okamžiku, kdy je okno nastavit:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Chcete-li pokračovat v přesný čas probuzení aplikace, použijte `SetExact`a předejte přesný čas, který by měl být probuzený aplikace a provádět operace:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat už umožňuje nastavit alarm opakovaných přesný. Aplikace, které používají [ `SetRepeating` ](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/) a vyžadují přesně výstrahy pro práci se teď třeba ruční aktivaci každé upozornění.

### <a name="external-storage"></a>Externího úložiště

Externí úložiště je nyní rozdělit do dvou typů - úložiště, které jsou jedinečné pro vaše aplikace a data sdílet mezi více aplikacemi. Čtení a zápis do určitého umístění vaší aplikace na externí úložiště vyžaduje žádná zvláštní oprávnění. Vyžaduje interakci s daty ve sdíleném úložišti nyní `READ_EXTERNAL_STORAGE` nebo `WRITE_EXTERNAL_STORAGE` oprávnění. Existují dva typy můžou být klasifikované jako takový:

-  Pokud vám cestu k souboru nebo adresáře voláním metody na `Context` – například [ `GetExternalFilesDir` ](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/) nebo [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - vaše aplikace vyžaduje žádná další oprávnění.

-  Pokud vám přístup k vlastnosti nebo volání metody na cestu k souboru nebo adresáře `Environment` , jako například [ `GetExternalStorageDirectory` ](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/) nebo [ `GetExternalStoragePublicDirectory` ](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/) , vaše aplikace vyžaduje `READ_EXTERNAL_STORAGE` nebo `WRITE_EXTERNAL_STORAGE` oprávnění.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` znamená `READ_EXTERNAL_STORAGE` oprávnění, takže vždy jen by měl třeba nastavit jedno oprávnění.

### <a name="sms-consolidation"></a>Konsolidace SMS

KitKat zjednodušuje zasílání zpráv pro uživatele agregací veškerý obsah služby SMS v aplikaci jedno vybrané uživatelem. Vývojář je odpovědná za vytváření aplikace lze vybrat jako výchozí aplikace pro zasílání zpráv a chovají správně v kódu a životnosti Pokud není vybrané aplikace. Další informace týkající se přechodu KitKat v aplikaci SMS najdete v části [získávání vaše SMS aplikace připravené pro KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) průvodce z Google.

### <a name="webview-apps"></a>Webové zobrazení aplikací

[Webové zobrazení](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) tu makeover v KitKat. Největší změnou se přidá zabezpečení pro načítání obsahu do `WebView`. Zatímco většina aplikací pro starší verze rozhraní API by měla fungovat podle očekávání, testování aplikací, které používají `WebView` třída důrazně doporučujeme. Další informace o rozhraní API dotčené webové zobrazení najdete v části pro Android [migrace na webové zobrazení v Android 4.4](http://developer.android.com/guide/webapps/migrating.html) dokumentaci.

<a name="user_experience" />

## <a name="user-experience"></a>Zkušenosti uživatele

KitKat obsahuje několik nových rozhraní API k vylepšení zkušeností uživatele, včetně nové architektury přechodu pro zpracování vlastnost animace a průhledná možnost uživatelského rozhraní pro motivů. Tyto změny jsou popsané níže.

### <a name="transition-framework"></a>Přechod Framework

Rozhraní framework přechod snazší implementovat animace. KitKat umožňuje provádět jednoduché vlastnost animace s jedním řádkem kódu, nebo si přizpůsobit přechodů pomocí *scény*.

#### <a name="simple-property-animation"></a>Jednoduché vlastnosti animace

Nové knihovny Android přechody zjednodušuje kódu vlastnost animace. Rozhraní umožňuje provádět jednoduché animací minimální kódem. Například následující kód používá ukázka [ `TransitionManager.BeginDelayedTransition` ](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/) pro animaci zobrazení a skrytí `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

V předchozím příkladu používá rozhraní přechod k vytvoření automatické, výchozí přechod mezi změna hodnoty vlastností. Vzhledem k tomu, že animace se zpracovává souborem jeden řádek kódu, můžete snadno vytvořit to kompatibilní se staršími verzemi systému Android pomocí zabalení `BeginDelayedTransition` volání v kontrolu verze systému. Najdete v článku [migrace vaše aplikace k KitKat](#Migrating_Your_App_to_KitKat) části Další informace.

Následující snímek obrazovky ukazuje aplikace před animace:

[![Snímek obrazovky aplikace před spuštěním animace](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

Následující snímek obrazovky ukazuje aplikace po animace:

[![Snímek obrazovky aplikace po dokončení animace](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Můžete získat větší kontrolu nad přechodu se děje to, které jsou popsané v další části.

#### <a name="android-scenes"></a>Android scény

[Scény](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) byly zavedeny v rámci rozhraní přechod umožnit vývojář větší kontrolu nad animace. Scény vytvořit dynamické oblasti v uživatelském rozhraní: Zadejte kontejner a několik verzí nebo "scény" obsah XML uvnitř kontejneru, a Android nemá zbývající práce, kterou použije animaci přechodů mezi na pozadí. Android scény umožňují vytvořit komplexní animací s minimálním úsilím na straně vývoj.

Statické element uživatelského rozhraní nachází dynamický obsah je názvem *kontejneru* nebo *scény základní*. Následující příklad používá návrháře Android k vytvoření `RelativeLayout` názvem `container`:

[![Vytvořte kontejner RelativeLayout pomocí návrháře Android](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

Ukázka rozložení také definuje tlačítka nazvaného `sceneButton` níže `container`. Toto tlačítko aktivují přechodu.

Dynamický obsah uvnitř kontejneru vyžaduje dvě nové Android rozložení. Tyto rozložení zadat jenom kód *uvnitř* kontejneru.
Následující ukázkový kód definuje rozložení názvem *Scene1* , který obsahuje dvě pole text čtení "Kit" a "Kat" v uvedeném pořadí a s názvem druhý rozložení *Scene2* , která obsahuje stejnou textové pole vrátit zpět. Soubor XML je následující:

 **Scene1.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

V příkladu výše používá `merge` kratší zobrazení kód a zjednodušit hierarchii zobrazení. Další informace o `merge` rozložení [zde](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Scény je vytvořená voláním [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/), předejte do objektu kontejneru, ID prostředku scény rozložení souboru a aktuální `Context`, které jsou popsány v následujícím příkladu kódu:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Kliknutím na tlačítko převrátí mezi dvěma scény, které Android animuje s výchozími hodnotami přechod:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

Následující snímek obrazovky ukazuje scény před animace:

[![Snímek obrazovky aplikace před spuštěním animace](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Následující snímek obrazovky ukazuje scény po animace:

[![Snímek obrazovky aplikace po dokončení animace](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> Je [známého problému](https://code.google.com/p/android/issues/detail?id=62450) v Android přechody knihovny, která způsobí, že scény vytvořený `GetSceneForLayout` pro přerušení, když uživatel přejde aktivitu na druhém. Alternativní řešení java je popsán [zde](http://www.doubleencore.com/2013/11/new-transitions-framework/).


##### <a name="custom-transitions-in-scenes"></a>Vlastní přechody v scény

V souboru xml prostředků v lze definovat vlastní přechod `transition` adresář, ve `Resources`, které jsou popsány v následující snímek obrazovky:

[![Umístění souboru transition.xml v adresáři prostředky nebo přechod](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

Následující ukázka kódu definuje přechod, který animuje 5 sekund a používá [překročení interpolator](http://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Přechod je vytvořen v aktivitě pomocí [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/), které jsou popsány v následující kód:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Nový přechod se pak přidá do `Go` volání, která začíná animace:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Průhledné uživatelského rozhraní

KitKat umožňuje více ovládat motivů vaší aplikace pomocí volitelné transclucent stav a navigační panely. Můžete změnit průsvitnosti prvků uživatelského rozhraní systému ve stejném souboru XML, které můžete použít k definování motivu Android. KitKat zavádí následující vlastnosti:

-  `windowTranslucentStatus` -Pokud nastavíte na hodnotu true, má nejvyšší stavový řádek průhledná.

-  `windowTranslucentNavigation` -Pokud nastavíte na hodnotu true, má dolní navigační panel průhledná.

-  `fitsSystemWindows` -Nastavení panelu horní nebo dolní transcluent posune obsah v části transparentní prvky uživatelského rozhraní ve výchozím nastavení. Nastavení této vlastnosti na `true` je jednoduchý způsob, jak zabránit obsahu se prvky uživatelského rozhraní systému průhledná.


Následující kód definuje motiv s průhledná stav a navigační panely:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

Následující snímek obrazovky ukazuje motiv výše se stavem průhledné a navigační panely:

[![Příklad snímek obrazovky aplikace s průhledná stav a navigační panely](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>Obsah uživatele

### <a name="storage-access-framework"></a>Přístup k úložišti Framework

Úložiště přístup Framework (SAF) je nový způsob, jak uživatelům interakci s uložený obsah, jako je například obrázků, videí a dokumenty. Místo uživatelů prezentuje dialogové okno Vybrat aplikaci pro zpracování obsahu, KitKat otevře nové uživatelské rozhraní, které umožňuje uživatelům přístup k datům v jednom místě, agregace. Po byla vybrána obsah, vrátí se uživatel k aplikaci, která požadovaný obsah a aplikační prostředí budou pokračovat normální.

Tato změna vyžaduje dvě akce na straně developer: nejprve nutné aktualizovat tak, aby nový způsob reqesting obsahu aplikací, které vyžadují obsah od poskytovatelů. Druhý, aplikace, které zápis dat do `ContentProvider` potřeba upravit tak, aby pomocí nové architektury. Oba scénáře závisí na novém [ `DocumentsProvider` ](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/) rozhraní API.

#### <a name="documentsprovider"></a>DocumentsProvider

V KitKat, interakce s `ContentProviders` jsou abstrahované s `DocumentsProvider` třídy. To znamená, že není SAF vás kterém jsou data, fyzicky, dokud je přístupný prostřednictvím `DocumentsProvider` rozhraní API. Místní poskytovatelů cloudových službách a zařízení externího úložiště všechny použijte stejné rozhraní a jsou považovány stejným způsobem, uživatel a vývojář poskytování jednom místě pracovat s obsahem uživatele.

Tato část obsahuje informace o spouštění a uložit obsah s rozhraní úložiště přístup.

#### <a name="request-content-from-a-provider"></a>Obsah žádosti od zprostředkovatele

Jsme napoví KitKat že chceme vyberte obsahu pomocí uživatelského rozhraní SAF s `ActionOpenDocument` záměr, která znamená, že má být pro připojení k všech poskytovatelů obsahu k dispozici pro zařízení. Můžete přidat některé filtrování tento záměr zadáním `CategoryOpenable`, což znamená, že pouze obsah, který lze otevřít (tj přístupný, použitelný obsah) bude vrácen. KitKat také umožňuje filtrování obsahu s `MimeType`. Například kód pod filtry výsledků bitové kopie zadáním bitovou kopii `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Volání metody `StartActivityForResult` spustí SAF uživatelského rozhraní, které uživatel může vybrat bitovou kopii vyhledejte:

[![Příklad snímek obrazovky aplikace pomocí rozhraní úložiště přístup pro procházení do obrázku](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

Po výběru image, uživatelem `OnActivityResult` vrátí `Android.Net.Uri` zvolené souboru. Následující ukázka kódu zobrazuje výběr image uživatele:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>Zápis obsahu do zprostředkovatele

Kromě načítání obsahu z uživatelského rozhraní SAF, KitKat také umožňuje uložit obsah do jakéhokoli `ContentProvider` , která implementuje `DocumentProvider` rozhraní API. Ukládání obsahu používá `Intent` s `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Výše uvedený ukázkový kód načte rozhraní SAF, takže uživatel změňte název souboru a vyberte adresář, který bude obsahovat nový soubor:

[![Snímek obrazovky uživatele Změna názvu souboru na NewDoc v adresáři stahování](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Když uživatel stiskne **Uložit**, `OnActivityResult` předána `Android.Net.Uri` z nově vytvořený soubor, který lze přistupovat pomocí `data.Data`. Identifikátor uri lze použít na datový proud dat do nového souboru:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Všimněte si, že [ `ContentResolver.OpenOutputStream(Android.Net.Uri)` ](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri)) vrátí `System.IO.Stream`, takže celý proces streamování může být napsán v rozhraní .NET.

Další informace o načítání, vytváření a úpravy obsahu s přístup rozhraní úložiště najdete v části [Android dokumentaci k rozhraní přístup úložiště](http://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Tisk

Tisk obsahu je zjednodušené KitKat se zavedením [tiskové služby](https://developer.xamarin.com/api/namespace/Android.PrintServices/) a `PrintManager`. KitKat je také první verze rozhraní API můžete plně využít [API Google Cloud tiskové služby](https://developers.google.com/cloud-print/) pomocí [aplikace Google Cloud tiskových](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
Většina zařízení, které se dodávají spolu s KitKat automaticky stáhnout aplikaci pro Google Cloud tisku a [modul plug-in služby tiskových HP](https://play.google.com/store/apps/details?id=com.hp.android.printservice)při prvním připojení k Wi-Fi. Uživatele můžete zkontrolovat nastavení tisku své zařízení tak, že přejdete do **Nastavení > Systém > Tisk**:

[![Příklad snímek obrazovky nastavení tisku](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> I když tisk rozhraní API pro práci s Google Cloud tisku se nastaví ve výchozím nastavení, Android stále umožňuje vývojářům připravte tiskové obsahu pomocí nových rozhraní API a odeslat do jiných aplikací ke zpracování tisku.



#### <a name="printing-html-content"></a>Obsah HTML tisk

KitKat automaticky vytvoří [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) pro webové zobrazení s `WebView.CreatePrintDocumentAdapter`. Tisk webového obsahu je koordinovaný proces mezi [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) , čeká na obsah HTML, který se načíst a umožňuje aktivity vědět, chcete-li k dispozici v nabídce Možnosti možnost tisku a Actvity, která čeká na uživateli Vyberte možnost tisku a volání `Print`na `PrintManager`. Tato část obsahuje základní nastavení potřebné k tisku na obrazovce obsah HTML.

Všimněte si, že načtení a tisk webového obsahu vyžaduje oprávnění k Internetu:

[![Nastavení oprávnění Internetu v možnostech aplikace](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Tisk položky nabídky

Možnosti tisku se obvykle zobrazuje v rámci aktivity [nabídka možnosti](http://developer.android.com/guide/topics/ui/menus.html#options-menu).
V nabídce Možnosti mohou uživatelé provádět akce na aktivitu. V pravém horním rohu obrazovky a vypadá takto:

[![Příklad snímek obrazovky dispalyed položky nabídky tisku v pravém horním rohu obrazovky](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


Položky nabídky dalších lze definovat v *nabídky*adresář, ve *prostředky*. Následující kód definuje nabídky ukázkové položky názvem [tiskových](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Interakce se v nabídce Možnosti v rámci aktivity se nakonfigurují `OnCreateOptionsMenu` a `OnOptionsItemSelected` metody.
`OnCreateOptionsMenu` je místo pro přidání nové položky nabídky, podobně jako u možnosti tisku z *nabídky* adresáře prostředků.
`OnOptionsItemSelected` čeká na uživatele, vyberete možnost tisku z místní nabídky a začne tisk:

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

Výše uvedený kód také definuje proměnnou s názvem `dataLoaded` ke sledování stavu obsah HTML. `WebViewClient` Bude tuto proměnnou na hodnotu true při načetl veškerý obsah, aby věděl, může aktivity k přidání položky nabídky Tisk do nabídky Možnosti nastavit.

##### <a name="webviewclient"></a>WebViewClient

Práci `WebViewClient` , je potřeba zajistit data v `WebView` je úplným načtením před tiskové možnost se zobrazí v nabídce, která v případě `OnPageFinished` metoda. `OnPageFinished` čeká na dokončení načtení obsahu webu a říká aktivity k opětovnému vytvoření jeho nabídka možnosti s `InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` také nastaví `dataLoaded` hodnotu `true`, takže `OnCreateOptionsMenu` můžete znovu vytvořit s možností tisku v místní nabídce.

##### <a name="printmanager"></a>PrintManager

Následující příklad kódu vytiskne obsah `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` vezme jako argumenty: název tiskové úlohy ("MyWebPage" v tomto příkladu), [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) který generuje tisku dokumentu z obsahu, a [ `PrintAttributes` ](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/) (`null` v Příklad výše). Můžete zadat `PrintAttributes` pomohou rozložení obsahu na tištěných stránce, i když většina scenarions by měla řídit tyto výchozí atributy.

Volání metody `Print` načte tiskové uživatelské rozhraní, které je uveden seznam možností pro tiskové úlohy. Uživatelské rozhraní nabízí uživatelům možnost tisku nebo ukládání obsah HTML, který do PDF, které jsou popsány v následujících snímcích obrazovky:

[![Snímek obrazovky PrintHtmlActivity nabídce Tisk zobrazení](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Snímek obrazovky PrintHtmlActivity zobrazení uložit jako PDF nabídky](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>Hardware

KitKat přidá několik rozhraní API lze používat nové funkce zařízení. Nejvíce zajímavé těchto jsou založené na hostiteli emulace karty a nové `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Emulace karty založené na hostitele v NFC

Na hostiteli emulace karty (HCE) umožňuje aplikacím se bude chovat, jako jsou karty NFC nebo čtečky karet NFC bez spoléhání na Element poskytovatel proprietární zabezpečení. Před nastavením HCE, zkontrolujte HCE je k dispozici na zařízení s `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE vyžaduje, aby funkci HCE a `Nfc` oprávnění zaregistrovat u aplikace `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Nastavení oprávnění NFC v možnostech aplikace](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

Pro práci, musí být možné spustit na pozadí HCE a má spustit, když uživatel provede transakci NFC i v případě, že aplikace pomocí HCE není spuštěna. Nemůžeme se dá dosáhnout vytvořením kód HCE jako `Service`. Implementuje služby HCE `HostApduService` rozhraní, která implementuje následujících metod:

-  *ProcessCommandApdu* -protokolu aplikaci částí Data (APDU) je odeslán mezi čtečky NFC a službou HCE. Tato metoda využívá ADPU ze čtečky a vrátí datové jednotky v odpovědi.

-  *OnDeactivated* – `HostAdpuService` je deaktivována při službu HCE je už nekomunikují s čtečky NFC.


Služby HCE taky musí být zaregistrována manifestu aplikace a označených pomocí správné permissons, záměrné filtru a metadata. Následující kód slouží jako příklad `HostApduService` zaregistrována pomocí Android Manifest `Service` atribut (Další informace o atributech, najdete v části Xamarin [práce s Android Manifest](~/android/platform/android-manifest.md) průvodce):

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("andorid.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

Výše uvedené služba poskytuje způsob, jak čtečky NFC pro interakci s aplikací, ale čtečky NFC má stále žádný způsob, jak zjistit, pokud tato služba je emulace karty NFC, které potřebuje ke kontrole. Pomoc při identifikaci služby čtečky NFC, jsme můžete přiřadit služby jedinečný *ID aplikace (podpora)*. Jsme podporu, spolu s další metadata týkající se služby HCE, zadejte v souboru prostředků jazyka xml zaregistrována `MetaData` atribut (viz výše uvedený příklad kódu). Tento soubor prostředků obsahuje jeden nebo více podpory filtrů – jedinečný identifikátor řetězce v šestnáctkovém formátu, které odpovídají pomůcky jeden nebo více zařízení čtečky NFC:

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

Kromě podpory filtry prostředek souboru xml obsahuje také popis zobrazující se uživatelům služby HCE, určuje skupinu podpory (platebních aplikace versus "ostatní") a v případě aplikace platebních, banner 260 x 96 distribučního bodu zobrazit uživateli.

Instalační program uvedených výše poskytuje základní stavební bloky pro aplikaci emulace NFC karty. NFC samotné vyžaduje některé další kroky a další testování konfigurace. Další informace o emulace karty založené na hostiteli, najdete v části [portálu Android dokumentace](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Další informace o používání technologie NFC s Xamarinem, podívejte se [Xamarin NFC ukázky](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Snímače

Poskytuje přístup k zařízení senzorů prostřednictvím KitKat [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/).
`SensorManager` Umožňuje operačního systému a naplánovat doručování senzor informace k aplikaci v dávkách, zachování výdrž baterie.

KitKat také dodává s dva nové typy senzor pro sledování kroky uživatele. Tyto jsou založené na zrychlení a zahrnují:

-  *StepDetector* -aplikace upozornění nebo probuzený když uživatel provede krok a detektor poskytuje hodnotu času pro když došlo k chybě v kroku.

-  *StepCounter* -uchovává informace o počet kroků uživatele trvá vzhledem k tomu, že byl zaregistrován senzoru *až po dalším restartování zařízení*.

Následující snímek obrazovky znázorňuje čítač krok v akci:

[![Snímek obrazovky aplikace SensorsActivity zobrazení čítač krok](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Můžete vytvořit `SensorManager` voláním `GetSystemService(SensorService)` a přetypování výsledek v podobě `SensorManager`. Chcete-li používat čítač krok, volejte `GetDeafultSensor` na `SensorManager`. Můžete zaregistrovat senzoru a naslouchání na změny v počtu krok pomocí [ `ISensorEventListener` ](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/) rozhraní, které jsou popsány v následující ukázka kódu:

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` je volána, pokud počet krok aktualizuje, zatímco aplikace je v popředí. Pokud aplikace přejde na pozadí, nebo zařízení přejde do režimu spánku, `OnSensorChanged` nebude volána, ale bude dál kroky, které se mají spočítat, dokud `UnregisterListener` je volána.

Mějte na paměti, *hodnota počtu krok je kumulativní ve všech aplikacích, které registrují senzoru*. To znamená, že i když odinstalovat a znovu nainstalovat aplikaci a inicializace `count` proměnné na 0 při spuštění aplikace, hodnotu hlášenou senzoru zůstane celkový počet kroků pořídí za běhu byl zaregistrován senzoru, jestli se ve vaší aplikace nebo jiné. Můžete zabránit aplikaci v přidání do čítač krok voláním `UnregisterListener` na `SensorManager`, které jsou popsány v následující kód:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Krok počet restartování zařízení resetuje na hodnotu 0. Vaše aplikace bude vyžadovat další kód zajistit, že hlásí aktuální přehled pro aplikace, bez ohledu na ostatní aplikace pomocí senzoru nebo stav zařízení.


> [!NOTE]
> Ne všechny telefony jsou při rozhraní API pro detekci kroku a poté se dodává s KitKat outfitted s senzoru. Můžete zkontrolovat, zda senzoru je k dispozici spuštěním `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, nebo zkontrolovat zajistit vrácená hodnota z `GetDefaultSensor` není `null`.


<a name="developer_tools" />

## <a name="developer-tools"></a>Nástroje pro vývojáře

### <a name="screen-recording"></a>Záznam obrazovky

KitKat zahrnuje nové obrazovky, zaznamenávání možnosti, aby vývojáři mohou zaznamenat aplikace v akci. Záznam obrazovky je k dispozici prostřednictvím [Android ladění most (ADB)](http://developer.android.com/tools/help/adb.html) klienta, které lze stáhnout jako část sady SDK pro Android.

Chcete-li záznam obrazovky, připojte zařízení; pak vyhledat instalaci sady SDK pro Android, přejděte **nástrojů platformy** adresáře a spusťte **adb** klienta:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Výše uvedený příkaz zaznamená video 3 minut výchozí na výchozí rozlišení 4Mbps. Chcete-li upravit délku, přidejte *– časový limit* příznak.
Chcete-li změnit na řešení, přidejte *– rychlost* příznak. Následující příkaz bude záznam videa minutu dlouho na 8Mbps:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Můžete najít videa na vašem zařízení – se objeví v galerii po dokončení záznamu.


## <a name="other-kitkat-additions"></a>Přidání dalších KitKat

Kromě změn popsaných výše KitKat umožňuje:

-  *Použít na celé obrazovce* -KitKat představuje novou [dokonalé režimu](http://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) pro procházení obsahu, hraní her a spuštění jiných aplikací, které může mít užitek z prostředí přes celou obrazovku.

-  *Oznámení přizpůsobit* -získat další podrobnosti o systémová oznámení s [ `NotificationListenerService` ](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) . Díky tomu je k dispozici informace jiným způsobem uvnitř vaší aplikace.

-  *Zrcadlení Drawable prostředky* -Drawable prostředky mít nový [ `autoMirrored` ](http://developer.android.com/reference/android/R.attr.html#autoMirrored) atribut, který říká systému vytvořit zrcadlené verze pro bitové kopie, které vyžadují překlopení pro rozložení zleva doprava.

-  *Pozastavit animace* -pozastavení a obnovení animací vytvořené pomocí [ `Animator` ](https://developer.xamarin.com/api/type/Android.Animation.Animator/) třída.

-  *Čtení dynamicky Změna textu* -označují části uživatelského rozhraní, které dynamicky aktualizovat novým textem jako "za provozu oblasti" s novým [ `accesibilityLiveRegion` ](http://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion) atribut tak nového textu bude automaticky načíst v režimu usnadnění.

-  *Zajištění lepších možností zvuk* -zkontrolujte sleduje hlasitost s [ `LoudnessEnhancer` ](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/) , můžete vyhledat ve špičce a RMS zvukový datový proud s [ `Visualizer` ](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/) třídy a získání informací ze [zvuk časové razítko](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) usnadní audio a video synchronizace.

-  *Synchronizovat ContentResolver v intervalu vlastní* -KitKat přidá některé variabilita na čas, který provádí žádost o synchronizaci. Synchronizace `ContentResolver` na vlastní čas nebo interval voláním `ContentResolver.RequestSync` a předejte `SyncRequest`.

-  *Rozlišit mezi řadiče* – v KitKat řadiče jsou přiřazeny identifikátory jedinečné celé číslo, které budou přístupné prostřednictvím zařízení `ControllerNumber` vlastnost. Díky tomu je snazší zjistit od sebe přehrávače ve hře.

-  *Vzdálené řízení* -KitKat několik změn na straně hardwaru a softwaru, umožňuje zapnout zařízení outfitted s Infračervený vysílač do vzdáleného řízení pomocí `ConsumerIrService`a interakci s periferní zařízení s novou [ `RemoteController` ](https://developer.xamarin.com/api/type/Android.Media.RemoteController/) Rozhraní API.

Další informace o výše změny rozhraní API, naleznete ke službě Google [rozhraní API systému Android 4.4](http://developer.android.com/about/versions/android-4.4.html) Přehled.


## <a name="summary"></a>Souhrn

Tento článek zavedená některé z nových rozhraní API k dispozici v systému Android 4.4 (API úrovně 19) a zahrnutých osvědčené postupy při přechodu aplikaci KitKat. IT prostředí popsané změnách rozhraní API, které mají vliv na uživatele, včetně *přechod framework* a nové možnosti pro *motivů*. V dalším kroku zavedená *přístup k úložišti Framework* a `DocumentsProvider` třídu, stejně jako nový *tisk rozhraní API*. Ho prozkoumali *NFC karty založené na hostiteli emulace* a jak pracovat s *úsporného režimu senzorů*, včetně dva nové senzorů pro sledování kroky uživatele. Nakonec ukázán zaznamenávání v reálném čase ukázky aplikací s *záznam obrazovky*a poskytuje podrobný seznam KitKat API změnami a dodatky.


## <a name="related-links"></a>Související odkazy

- [Ukázka KitKat](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 rozhraní API](http://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](http://developer.android.com/about/versions/kitkat.html)
