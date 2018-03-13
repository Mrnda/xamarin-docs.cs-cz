---
title: "Úvodní obrazovka"
description: "Aplikace pro Android trvá delší dobu spuštění, zejména při prvním spuštění aplikace na zařízení. Úvodní obrazovka se může zobrazovat počáteční registrace probíhá na uživatele nebo k označení brandingu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 9f88899d390f7f268f1b2f435617dc952f9eb205
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="splash-screen"></a>Úvodní obrazovka

_Aplikace pro Android trvá delší dobu spuštění, zejména při prvním spuštění aplikace na zařízení. Úvodní obrazovka se může zobrazovat počáteční registrace probíhá na uživatele nebo k označení brandingu._


## <a name="overview"></a>Přehled

Aplikace pro Android chvíli o spuštění služby trvat, zejména při prvním spuštění aplikace na zařízení (někdy tento proces se označuje jako _studený start_). Úvodní obrazovka se může zobrazovat spuštění průběh uživateli, nebo se může zobrazit informace o konfiguraci identifikovat a podporovat aplikace.

Tato příručka popisuje jednoho technika, jak implementovat úvodní obrazovky v aplikaci pro Android. Zahrnuje následující kroky:

1.  Vytvoření drawable prostředku pro úvodní obrazovka.

2.  Definování nový motiv, který se zobrazí drawable prostředků.

3.  Přidává se nová aktivita k aplikaci, která se použije jako úvodní obrazovka definované motiv vytvořili v předchozím kroku.

[![Příklad Xamarin logo úvodní obrazovky následuje obrazovky aplikace](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>Požadavky

Tato příručka předpokládá, že cílem aplikace Android API úrovně 15 (Android 4.0.3) nebo vyšší. Aplikace musí mít také **Xamarin.Android.Support.v4** a **Xamarin.Android.Support.v7.AppCompat** přidán do projektu balíčky NuGet.

Všechny kód a kód XML v této příručce najdete v [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen) ukázkový projekt této příručce.


## <a name="implementing-a-splash-screen"></a>Implementace úvodní obrazovky

Nejrychlejší způsob, jak vykreslit a zobrazí úvodní obrazovka je vytvořte vlastní motiv a použít ji na aktivitu, pro jehož úvodní obrazovka. Když aktivita vykresleno, načte motiv a drawable prostředků (odkazuje motiv) se vztahuje na pozadí aktivity. Tento přístup zabraňuje potřeba pro vytvoření rozložení souboru.

Úvodní obrazovka je implementovaná jako aktivity, která se zobrazí partnerské drawable, provede všechny inicializacích a spuštění žádné úlohy. Jakmile aplikace má připravili, úvodní obrazovka aktivity spustí hlavní aktivitu a samotné odebere back zásobník aplikací.


### <a name="creating-a-drawable-for-the-splash-screen"></a>Vytváření Drawable pro úvodní obrazovka

Úvodní obrazovka se zobrazí na pozadí na úvodní obrazovce aktivity drawable XML. Je nutné použít bitovou kopii rastrové obrázky (například PNG nebo JPG) pro bitovou kopii k zobrazení.

V této příručce použijeme [seznamu vrstvy](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) na střed obrázek úvodní obrazovky v aplikaci. Následující fragment kódu je příkladem `drawable` pomocí prostředků `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

To `layer-list` bude center obrázek úvodní obrazovky **splash.png** na pozadí určeného `@color/splash_background` prostředků.
Umístěte tento soubor **prostředky/drawable** složky (například **Resources/drawable/splash_screen.xml**).

Po vytvoření drawable úvodní obrazovka, dalším krokem je vytvoření motiv pro úvodní obrazovka.


### <a name="implementing-a-theme"></a>Implementace motiv

Pokud chcete vytvořit vlastní motivy pro úvodní obrazovka aktivity, upravit (nebo přidejte) soubor **values/styles.xml** a vytvořte novou `style` element pro úvodní obrazovka. Ukázka **values/style.xml** souboru je uveden níže s `style` s názvem **MyTheme.Splash**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

**MyTheme.Splash** je velmi spartan &ndash; deklaruje pozadí okna, explicitně odebere záhlaví z okna a deklaruje, že je přes celou obrazovku. Pokud chcete vytvořit úvodní obrazovky, který emuluje uživatelského rozhraní aplikace před aktivity zvýšení kapacity první rozložení, můžete použít `windowContentOverlay` místo `windowBackground` ve vašem definici stylu. V takovém případě musíte také upravit **splash_screen.xml** drawable tak, aby zobrazil emulaci uživatelské rozhraní.


### <a name="create-a-splash-activity"></a>Vytvořit úvodní aktivitu

Nyní potřebujeme novou aktivitu pro Android ke spuštění, který má naše obrázek úvodní a provede všechny spuštění úlohy. Následující kód je příkladem na dokončení úvodní obrazovky implementace:

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` používá explicitně motivu, který byl vytvořen v předchozí části, přepsání výchozí motiv aplikace.
Není nutné načíst rozložení v `OnCreate` jako motiv deklaruje drawable jako na pozadí.

Je důležité nastavit `NoHistory=true` atributů tak, aby aktivity je odebrán, zpět. Aby tlačítko Zpět na zrušení spuštění procesu, můžete také přepsat `OnBackPressed` a mějte ho nedělat nic:

```csharp
public override void OnBackPressed() { }
```

Spuštění pracovních probíhá asynchronně v `OnResume`. To je nezbytné, tak, aby pracovní spuštění zpomalit nebo zpoždění vzhled úvodní obrazovka. Po dokončení práce `SplashActivity` spustí `MainActivity` a uživatel může začít interakci s aplikací.

Tento nový `SplashActivity` je nastavená jako aktivita Spouštěč aplikace nastavením `MainLauncher` atribut `true`. Protože `SplashActivity` je teď Spouštěče aktivity, je nutné upravit `MainActivity.cs`a odeberte `MainLauncher` atribut z `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```


## <a name="summary"></a>Souhrn

Tato příručka popsané jeden způsob, jak implementovat úvodní obrazovky v aplikaci Xamarin.Android; Konkrétně použití vlastních motivu spuštění aktivity.


## <a name="related-links"></a>Související odkazy

- [SplashScreen (ukázka)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [Drawable seznamu vrstev](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ Vzory návrhu podstatným - spuštění obrazovky](https://www.google.com/design/spec/patterns/launch-screens.html)
