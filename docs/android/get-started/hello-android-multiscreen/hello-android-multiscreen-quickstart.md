---
title: "Hello, Android Multiobrazovka: rychlý start"
description: "Tato příručka dvě části rozšíří Phoneword aplikaci zpracovávat druhý obrazovky. Na této cestě základní stavební bloky Android aplikace jsou zavedené o podrobnější prohlídku do Android architektury."
ms.topic: article
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 4c61a588eafdf0a86f4124d264c41cabef3e7a14
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="hello-android-multiscreen-quickstart"></a>Hello, Android Multiobrazovka: rychlý start

_Tato příručka dvě části rozšíří Phoneword aplikaci zpracovávat druhý obrazovky. Na této cestě základní stavební bloky Android aplikace jsou zavedené o podrobnější prohlídku do Android architektury._

## <a name="hello-android-multiscreen-quickstart"></a>Hello, Android Multiobrazovka rychlý start

V části návod v této příručce přidáte druhý obrazovky [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) aplikace ke sledování historie čísel přeložit pomocí aplikace. [Konečné aplikace](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen/) bude mít druhý obrazovky, který zobrazuje čísla, která měla "přeložit", které jsou popsány v na snímku obrazovky na pravé straně:

[![Snímky obrazovky příklad aplikace](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png)

Doprovodných [podrobné informace](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md) zkontroluje, co byla vytvořena a popisuje architekturu, navigace a dalších nových Android konceptů došlo na cestě.


## <a name="requirements"></a>Požadavky

Protože tato příručka převezme kde [Hello, Android](~/android/get-started/hello-android/index.md) nechá vypnout, vyžaduje dokončení [Hello, Android rychlý Start](~/android/get-started/hello-android/hello-android-quickstart.md).
Pokud chcete přejít přímo do návod níže, dokončené verzi můžete stáhnout [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) (z Hello Android rychlý start) a použít ho ke spuštění průvodce.

## <a name="walkthrough"></a>Návod

V tomto návodu přidáte **překlad historie** obrazovky na **Phoneword** aplikace.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Začněte otevřením **Phoneword** aplikace v sadě Visual Studio a úpravy **Main.axml** souboru z **Průzkumníku řešení**.

### <a name="updating-the-layout"></a>Aktualizace rozložení

Z **sada nástrojů**, přetáhněte ji **tlačítko** na návrh surface a umístěte ji níže **TranslatedPhoneWord** TextView. V **vlastnosti** podokně změňte tlačítko **Id** na `@+id/TranslationHistoryButton` 

[![Přetáhněte nového tlačítka](hello-android-multiscreen-quickstart-images/vs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/vs/02-new-button.png)

Nastavte **Text** vlastnost tlačítko pro `@string/translationHistory`. Android návrháře bude interpretovat to oznámena, ale se chystáte provést několik změn, aby na tlačítko text se zobrazí správně:

[![Nastavit text tlačítka historie překlad](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string-sml.png)](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string.png)

Rozbalte **hodnoty** pod uzlem **prostředky** složky v **Průzkumníku řešení** a dvakrát klikněte na soubor prostředků řetězec **Strings.xml**:

[![Open Strings.xml](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file.png)

Přidat `translationHistory` řetězec název a hodnotu **Strings.xml** souboru a uložte jej:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**Překlad historie** tlačítko text by měl aktualizovat tak, aby odrážely novou řetězcovou hodnotu:

[![Tlačítko odráží novou řetězcovou hodnotu](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)

S **překlad historie** tlačítko vybrané na návrhovou plochu, vyhledejte `enabled` nastavení v **vlastnosti** podokně a jeho hodnotu nastavte `false` zakázat tlačítko. To způsobí, že na tlačítko se na návrhovou plochu tmavšího:

[![Zakázat tlačítko historie překlad](hello-android-multiscreen-quickstart-images/vs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/vs/06-enabled-false.png)

### <a name="creating-the-second-activity"></a>Vytváření druhé aktivity

Vytvořte druhý aktivitu zapnutí druhý obrazovky. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **Phoneword** projektu a zvolte **Přidat > novou položku...** :

[![Přidat nový soubor](hello-android-multiscreen-quickstart-images/vs/07-add-new-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/07-add-new-file.png)

V **přidat novou položku** dialogovém okně, vyberte **Visual C# > aktivity** a název souboru aktivity **TranslationHistoryActivity.cs**.

Nahraďte kód šablony v **TranslationHistoryActivity.cs** následujícím kódem:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

V této třídě, kterou vytváříte `ListActivity` a naplnění prostřednictvím kódu programu, takže není nutné vytvořit nový soubor rozložení pro tuto aktivitu. To je podrobněji popsána v [Hello, Android Multiobrazovka podrobné informace](~/android/get-started/hello-android/hello-android-deepdive.md).

### <a name="adding-translation-history-code"></a>Přidání kódu historie překlad

Tato aplikace shromažďuje telefonní čísla (ke které má uživatel přeložit na první obrazovce) a předá je k obrazovce pro druhý. Telefonní čísla, která jsou uloženy jako seznam řetězců. Chcete-li podporovat seznamy, přidejte následující `using` direktivy do horní části `MainActivity` – třída:

```csharp
using System.Collections.Generic;
```

Dále vytvořte prázdný seznam, který může být vyplněna s telefonními čísly.
`MainActivity` Třída bude vypadat například takto:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

V `MainActivity` třídy, přidejte následující kód k registraci **překlad historie** tlačítko (umístit tento řádek po `translationHistory` deklarace):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Přidejte následující kód do konce `OnCreate` metodu propojit až **překlad historie** tlačítko:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Aktualizace **přeložit** tlačítko přidáte telefonní číslo do seznamu `phoneNumbers`. `Click` Obslužné rutiny pro `TranslateHistoryButton` by měla vypadat přibližně následující kód:

```csharp
// Add code to translate number
string translatedNumber = string.Empty;
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

Uložte a sestavte aplikaci a ujistěte se, že nejsou žádné chyby.

### <a name="running-the-app"></a>Spuštění aplikace

Nasazení aplikace na emulátoru nebo zařízení. Na následujících snímcích obrazovky ilustraci spuštění **Phoneword** aplikace:

[![Příklad snímky obrazovky](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Začněte otevřením **Phoneword** projektu v sadě Visual Studio pro Mac a úpravy **Main.axml** souboru z **řešení Pad**.

### <a name="updating-the-layout"></a>Aktualizace rozložení

Z **sada nástrojů**, přetáhněte ji **tlačítko** na návrh surface a umístěte ji níže **TranslatedPhoneWord** TextView. V **vlastnosti** odsadí, změňte tlačítko **Id** na `@+id/TranslationHistoryButton` 

[![Přetáhněte nového tlačítka](hello-android-multiscreen-quickstart-images/xs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/xs/02-new-button.png)

Nastavte **Text** vlastnost tlačítko pro `@string/translationHistory`. Android návrháře bude interpretovat to oznámena, ale se chystáte provést několik změn, aby na tlačítko text se zobrazí správně:

[![Nastavit text tlačítka historie překlad](hello-android-multiscreen-quickstart-images/xs/03-call-history-string-sml.png)](hello-android-multiscreen-quickstart-images/xs/03-call-history-string.png)


Rozbalte položku **hodnoty** pod uzlem **prostředky** složky v **Pad řešení** a dvakrát klikněte na soubor prostředků řetězec **Strings.xml**:

[![Otevřete řetězce](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file.png)


Přidat `translationHistory` řetězec název a hodnotu **Strings.xml** souboru a uložte jej:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**Překlad historie** tlačítko text by měl aktualizovat tak, aby odrážely novou řetězcovou hodnotu:

[![Tlačítko odráží novou řetězcovou hodnotu](hello-android-multiscreen-quickstart-images/xs/05-new-string-value-sml.png)](hello-android-multiscreen-quickstart-images/xs/05-new-string-value.png)


S **překlad historie** tlačítko vybrané na návrhovou plochu, otevřete **chování** ve **vlastnosti Pad** a dvakrát klikněte na **povoleno**  zaškrtávací políčko Zakázat tlačítko. To způsobí, že na tlačítko se na návrhovou plochu tmavšího:

[![Zakázat tlačítko historie překlad](hello-android-multiscreen-quickstart-images/xs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/xs/06-enabled-false.png)

### <a name="creating-the-second-activity"></a>Vytváření druhé aktivity

Vytvořte druhý aktivitu zapnutí druhý obrazovky. V **řešení Pad**, klikněte na ikonu šedé ozubené kolečko vedle **Phoneword** projektu a zvolte **Přidat > Nový soubor...** :

Z **nový soubor** dialogové okno, zvolte **Android > aktivity**, název aktivity `TranslationHistoryActivity`, pak klikněte na tlačítko **přidat**.

Nahraďte kód šablony v `TranslationHistoryActivity` následujícím kódem:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

V této třídě `ListActivity` je vytvořeny a naplněny prostřednictvím kódu programu, takže není nutné vytvořit nový soubor rozložení pro tuto aktivitu. To je vysvětleno v podrobněji [Hello, Android Multiobrazovka podrobné informace](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).

### <a name="adding-translation-history-code"></a>Přidání kódu historie překlad

Tato aplikace shromažďuje telefonní čísla (ke které má uživatel přeložit na první obrazovce) a předá je k obrazovce pro druhý. Telefonní čísla, která jsou uloženy jako seznam řetězců. Chcete-li podporovat seznamy, přidejte následující `using` direktivy do horní části `MainActivity` – třída:

```csharp
using System.Collections.Generic;
```

Dále vytvořte prázdný seznam, který může být vyplněna s telefonními čísly. `MainActivity` Třída bude vypadat například takto:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

V `MainActivity` třídy, přidejte následující kód k registraci **TranslationHistory historie** tlačítko (umístit tento řádek po `TranslationHistoryButton` deklarace):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Přidejte následující kód do konce `OnCreate` metodu propojit až **překlad historie** tlačítko:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Aktualizace **přeložit** tlačítko přidáte telefonní číslo do seznamu `phoneNumbers`. `Click` Obslužné rutiny pro `TranslateHistoryButton` by měla vypadat přibližně následující kód:

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

### <a name="running-the-app"></a>Spuštění aplikace

Nasazení aplikace na emulátoru nebo zařízení. Na následujících snímcích obrazovky ilustraci spuštění **Phoneword** aplikace:

[![Příklad snímky obrazovky](hello-android-multiscreen-quickstart-images/screenshot.png)](hello-android-multiscreen-quickstart-images/screenshot.png)

-----

Blahopřejeme k dokončení vaší první aplikace Xamarin.Android více obrazovky! Nyní je čas dissect nástroje a dovednosti právě naučili &ndash; až následuje [Hello, Android Multiobrazovka podrobné informace](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).


## <a name="related-links"></a>Související odkazy

- [Ikony aplikace Xamarin & spuštění obrazovky (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (ukázka)](https://developer.xamarin.com/samples/monodroid/Phoneword)
- [PhonewordMultiscreen (ukázka)](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen)
