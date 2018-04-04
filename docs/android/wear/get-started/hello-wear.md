---
title: Hello, opotřebení
description: Vytvoření první aplikace Android nosit a spusťte ho na opotřebení emulátoru nebo zařízení. Tento názorný postup obsahuje podrobné pokyny pro vytvoření malé projekt Android nosit, který zpracovává kliknutí na tlačítko a zobrazí čítače kliknutím na zařízení a opotřebením motoru. Vysvětluje postup ladění aplikace pomocí emulátoru opotřebení nebo opotřebení ze zařízení, která je připojená přes Bluetooth na Android telefon. Také poskytuje sadu ladění tipy pro Android nosit.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 742a10ce0042d2bbf6d5690cb7a7a6eca529a57e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="hello-wear"></a>Hello, opotřebení

_Vytvoření první aplikace Android nosit a spusťte ho na opotřebení emulátoru nebo zařízení. Tento názorný postup obsahuje podrobné pokyny pro vytvoření malé projekt Android nosit, který zpracovává kliknutí na tlačítko a zobrazí čítače kliknutím na zařízení a opotřebením motoru. Vysvětluje postup ladění aplikace pomocí emulátoru opotřebení nebo opotřebení ze zařízení, která je připojená přes Bluetooth na Android telefon. Také poskytuje sadu ladění tipy pro Android nosit._

![Snímek obrazovky aplikace opotřebení provést v tomto kurzu](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>První opotřebení aplikace

Postupujte podle těchto kroků k vytvoření první aplikace Xamarin.Android opotřebení:

### <a name="1-create-a-new-android-project"></a>1. Vytvořit nový projekt Android

Vytvořte novou **aplikace pro Android nosit**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vytvoření nové nosit aplikace pro Android v dialogu Nový projekt](hello-wear-images/vs/new-solution-sml.png)](hello-wear-images/vs/new-solution.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vytvoření nové nosit aplikace pro Android v dialogovém okně nové řešení](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


Tato šablona automaticky zahrne **Xamarin Android Wearable knihovny** NuGet (a závislosti), budete mít přístup k opotřebení konkrétní pomůcky. Pokud nevidíte šablonu a opotřebením motoru, přečtěte si [instalace a](~/android/wear/get-started/installation.md) průvodce znovu zkontrolovat, jestli máte nainstalovanou podporovanou sadou SDK pro Android. 

### <a name="2-choose-the-correct-target-framework"></a>2. Vyberte správný **cílové rozhraní**

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ujistěte se, že **minimální Android k cíli** je nastaven na **Android 5.0 (typu Lupa)** nebo novější: 

[![Nastavení rozhraní Target Framework na Android 5.0 v sadě Visual Studio](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Zkontrolujte cílový framework je nastavená na **Android 5.0 (typu Lupa)** nebo novější:

[![Nastavení rozhraní Target Framework na Android 5.0 v sadě Visual Studio pro Mac](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Další informace o nastavení cílové rozhraní najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).


### <a name="3-edit-the-mainaxml-layout"></a>3. Upravit **Main.axml** rozložení

Konfigurovat jeho rozložení tak, aby obsahovala `TextView` a `Button` pro ukázku: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4. Upravit **MainActivity.cs** zdroje

Přidejte kód, který zvýší čítače a zobrazit je vždy, když po kliknutí na tlačítko: 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5. Instalační program emulátoru nebo zařízení

Dalším krokem je nastavený na emulátoru nebo zařízení k nasazení a spuštění aplikace. Pokud si nejste ještě obeznámeni s procesem nasazení a spuštění aplikace Xamarin.Android obecné naleznete v tématu [Hello, Android rychlý Start](~/android/get-started/hello-android/hello-android-quickstart.md).

Pokud zařízení se systémem Android nosit například Android Smartwatch nosit nemáte, můžete spustit aplikaci v emulátoru. Informace o ladění aplikace opotřebení na emulátoru najdete v tématu [ladění Android nosit na emulátoru](~/android/wear/deploy-test/debug-on-emulator.md).

Pokud máte zařízení s Androidem nosit například Android Smartwatch nosit, můžete spustit aplikaci na zařízení místo použití emulátoru. Další informace o ladění na opotřebení ze zařízení najdete v tématu [ladění na zařízení nosit](~/android/wear/deploy-test/debug-on-device.md).


### <a name="6-run-the-android-wear-app"></a>6. Spuštění aplikace Android opotřebení

Zařízení Android nosit objevit v rozevírací nabídku zařízení. Nezapomeňte vybrat správné zařízení Android nosit nebo AVD před zahájením ladění. Po výběru zařízení, klikněte na tlačítko Přehrát akci nasazení aplikace na emulátoru nebo zařízení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Výběr AVD se nosit v nabídce zařízení sady Visual Studio](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Výběr AVD se nosit v sadě Visual Studio pro Mac zařízení nabídky](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Může se zobrazit **jenom několik minut...**  na první zprávy (nebo některé vkládaná obrazovky): 

![Podívejte se na emulátoru zobrazí, jenom několik minut...](hello-wear-images/please-wait.png)

Pokud používáte emulátor sledovat, může trvat nějakou dobu pro spuštění aplikace. Pokud používáte Bluetooth, trvá déle, než by tomu bylo přes USB nasazení aplikace. (Například trvá asi 5 minut k nasazení této aplikace na sledování G kontaktní skupina, která je připojení Bluetooth na telefon Nexus 5.)

Po aplikaci úspěšně nasadí, by měl obrazovce opotřebení ze zařízení zobrazí obrazovka takto:

[![Úvodní obrazovka opotřebení aplikace](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Klepněte **klikněte na tlačítko Poslat mi!** tlačítko na přední opotřebení ze zařízení a zjistit, o počtu přírůstek s každou klepněte na:

[![Snímek obrazovky nosit aplikace po kliknutí na 3](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>Další kroky

Podívejte se [nosit ukázky](https://developer.xamarin.com/samples/android/Android%20Wear/) včetně aplikace Android nosit s doprovodné telefonní aplikace.

Jakmile budete připraveni distribuovat aplikace, najdete v části [práce s balení](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Související odkazy

- [Klikněte na tlačítko Poslat mi aplikace (ukázka)](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
