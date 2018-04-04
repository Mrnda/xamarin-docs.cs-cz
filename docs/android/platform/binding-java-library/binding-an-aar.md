---
title: Vazba. AAR
description: Tento názorný postup obsahuje podrobné pokyny pro vytvoření vazby knihovny Xamarin.Android Java z Android. Soubor AAR.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 101fb28add97749549de9c44292a1ef99a717dde
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="binding-an-aar"></a>Vazba. AAR

_Tento názorný postup obsahuje podrobné pokyny pro vytvoření vazby knihovny Xamarin.Android Java z Android. Soubor AAR._


## <a name="overview"></a>Přehled

*Android archivu (. AAR)* soubor je formát souboru pro Android knihovny.
K. Soubor AAR. Archivu ZIP, který obsahuje následující:

-   Zkompilovaný kód Java
-   ID prostředků
-   Prostředky
-   Metadata (například aktivita deklarace, oprávnění)

V této příručce jsme projdete kroky základní informace o vytvoření vazby knihovny pro jeden. Soubor AAR. Přehled Java knihovny vazby obecně (s příklad základní kódu), najdete v tématu [vazby knihovna Java](~/android/platform/binding-java-library/index.md).


> [!IMPORTANT]
> Vazba projektu může obsahovat jenom jeden. Soubor AAR. Pokud. AAR závislosti na druhé. AAR a pak tyto závislosti by měl být obsažené v projektu vlastní vazby a pak odkazuje. V tématu [chyb 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).


## <a name="walkthrough"></a>Návod

Pro příklad Android archivu souboru, který byl vytvořen v Android studiu, vytvoříme knihovnu vazby [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). To. Obsahuje AAR `TextCounter` třídy pomocí statických metod, které počet samohlásky a souhlásky v řetězci. Kromě toho **textanalyzer.aar** obsahuje prostředek obrázku ke znázornění počítání výsledků.

Použijeme následující kroky k vytvoření vazby knihovny z. Soubor AAR:

1. Vytvoření nového projektu Java vazby knihovny.

2. Přidejte jeden. Soubor AAR do projektu. Vazba projektu může obsahovat jenom jeden. AAR.

3. Nastavení akce odpovídající sestavení pro. Soubor AAR.

4. Vyberte cílové rozhraní, která. Podporuje AAR.

5. Sestavení knihovny vazby.

Po vytvoření jsme knihovně vazby jsme budete vyvíjet malé aplikace pro Android, která vyzve uživatele k textového řetězce, volání. Metody AAR k analýze textu, načte bitovou kopii z. AAR a zobrazí výsledky společně s bitovou kopii.

Ukázková aplikace bude mít přístup `TextCounter` třídu **textanalyzer.aar**:

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

Kromě toho se tato ukázková aplikace načíst a zobrazit prostředek obrázku, který je součástí **textanalyzer.aar**:

[![Obrázek opic Xamarin](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Tento zdroj obrázku se nachází v **res/drawable/monkey.png** v **textanalyzer.aar**.



### <a name="creating-the-bindings-library"></a>Vytvoření vazby knihovny

Před zahájením pomocí následujících kroků, stáhněte si prosím tento příklad [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android archivního souboru:

1.  Vytvoření nového projektu knihovny vazby od verze šablony knihovna pro Android vazby. Můžete použít Visual Studio pro Mac nebo Visual Studio (na následujících snímcích obrazovky zobrazit Visual Studio, ale je velmi podobný jako Visual Studio pro Mac). Název řešení **AarBinding**:

    [![Vytvoření projektu AarBindings](binding-an-aar-images/01-new-bindings-library-vs-sml.png)](binding-an-aar-images/01-new-bindings-library-vs.png#lightbox)

2.  Šablona obsahuje **Jars** složku, kde můžete přidat vaše. AAR(s) na projekt knihovny vazby. Klikněte pravým tlačítkem myši **Jars** složky a vyberte **Přidat > existující položka**:

    [![Přidat existující položku](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  Přejděte na **textanalyzer.aar** předtím stáhli soubor, vyberte ho a klikněte na tlačítko **přidat**:

    [![Přidat textanalayzer.aar](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  Ověřte, zda **textanalyzer.aar** soubor byl úspěšně přidán do projektu:

    [![Přidání souboru textanalyzer.aar](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  Nastavení akce sestavení pro **textanalyzer.aar** k `LibraryProjectZip`. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **textanalyzer.aar** nastavení akce sestavení. V sadě Visual Studio, akce sestavení může být nastavena v **vlastnosti** podokně):

    [![Nastavení akce sestavení textanalyzer.aar LibraryProjectZip](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  Otevřete projekt k nastavení vlastnosti *cílové rozhraní*. Pokud. AAR používá žádné rozhraní Android API, nastavte cílové rozhraní API úrovně. Očekává se AAR. (Další informace o nastavení cílové rozhraní a úrovně rozhraní API systému Android v obecné najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).)

    Nastavte cíl úroveň rozhraní API pro knihovnu vazby. V tomto příkladu jsme jsou volně používat nejnovější platformu úroveň rozhraní API (API úrovně 23), protože naše **textanalyzer** nemá závislost na rozhraní Android API:

    [![Nastavení úrovně cíl na rozhraní API 23](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  Sestavení knihovny vazby. Projektu knihovny vazby musí úspěšně sestavení a vytvoření výstupu. Knihovny DLL v následujícím umístění: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>Použití knihovny vazby

To využívat. Knihovny DLL v aplikaci Xamarin.Android, je nejprve nutno přidat odkaz na knihovnu vazby. Pomocí následujících kroků:

1.  Vytváříme tuto aplikaci ve stejném řešení jako knihovně vazby ke zjednodušení tohoto návodu. (Aplikaci, která využívá knihovně vazby může také nacházet v jiné řešení.) Vytvoření nové aplikace Xamarin.Android: klikněte pravým tlačítkem na řešení a vyberte **přidat nový projekt**. Název nového projektu **BindingTest**:

    [![Vytvoření nového projektu BindingTest](binding-an-aar-images/07-add-new-project-vs-sml.png)](binding-an-aar-images/07-add-new-project-vs.png#lightbox)

2.  Klikněte pravým tlačítkem myši **odkazy** uzlu **BindingTest** projektu a vyberte **přidat odkaz na...** :

    [![Klikněte na tlačítko Přidat odkaz](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  Vyberte **AarBinding** projektu dříve vytvořili a klikněte na tlačítko **OK**:

    [![Zkontrolujte AAR vazby projektu](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  Otevřete **odkazy** uzlu **BindingTest** projektu a ověřte, zda **AarBinding** nachází odkaz:

    [![AarBinding je uveden v části odkazy](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


Pokud chcete zobrazit obsah projektu vazby knihovny, můžete dvakrát kliknete na odkaz otevřít v **Prohlížeč objektů**. Můžete zobrazit namapované obsah `Com.Xamarin.Textcounter` obor názvů (namapovaný z Java `com.xamarin.textanalyzezr` balíčku) a lze je zobrazit členy `TextCounter` – třída:

[![Zobrazení Prohlížeč objektů](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

Výše uvedený snímek obrazovky označuje dva `TextAnalyzer` metody, které bude volat aplikaci příklad: `NumConsonants` (který zabalí základní Java `numConsonants` metoda), a `NumVowels` (který zabalí základní Java `numVowels` metoda).



### <a name="accessing-aar-types"></a>Přístup k. Typy AAR

Po přidání odkazu na aplikaci, která odkazuje na knihovnu vazby, můžete přístup k typům Java v. AAR podle by přístup k typům C# (díky C# obálky). Kód aplikace C# můžete volat `TextAnalyzer` metod, jak je ukázáno v tomto příkladu:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

V předchozím příkladu voláme statických metod `TextCounter` třídy. Můžete však také vytvořit instanci třídy a volání metody instance. Například pokud vaše. AAR zabalí třídy s názvem `Employee` s metodu instance `buildFullName`, můžete vytvořit instanci `MyClass` a použít ho jak je vidět tady:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Následující postup přidávání kódu do aplikace tak, aby ho vyzve uživatele k textu, používá `TextCounter` k analýze text a potom zobrazí výsledky.

Nahraďte **BindingTest** rozložení (**Main.axml**) s následující kód XML. Toto rozložení má `EditText` pro zadávání textu a dvě tlačítka pro inicializaci samohláskou a souhláska počty:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

Nahraďte obsah **MainActivity.cs** následujícím kódem. Jak je vidět v tomto příkladu, volání obslužné rutiny událostí tlačítko zabalené `TextCounter` metody, které jsou umístěny ve. AAR a používání informační zprávy zobrazíte výsledky. Upozornění `using` příkaz pro obor názvů vázané knihovny (v tomto případě `Com.Xamarin.Textcounter`):

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

Zkompilování a spuštění **BindingTest** projektu. Aplikace spustí a k dispozici na snímku obrazovky na levé straně ( `EditText` inicializován s textem, ale můžete klepněte na něj ho můžete změnit). Když klepnete **počet SAMOHLÁSKY**, oznámení zobrazí počet samohlásky, jak je znázorněno na pravé straně:

[![Snímky obrazovky ve spuštění BindingTest](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Zkuste klepnout na **počet SOUHLÁSKY** tlačítko. Také můžete upravit řádek textu a klepněte na tato tlačítka znovu k testování pro různé samohláskou a souhláska počty.



### <a name="accessing-aar-resources"></a>Přístup k. AAR prostředky

Xamarin sloučení tooling **R** data z. AAR do vaší aplikace **prostředků** třídy. V důsledku toho můžete přistupovat. AAR prostředků z vaší rozložení (a z kódu) v stejným způsobem, jak by přístup k prostředkům, které jsou v **prostředky** cesta k projektu.

Chcete-li získat přístup k prostředku bitové kopie, použijte **Resource.Drawable** název pro bitovou kopii zabaleny uvnitř. AAR. Například můžete odkazovat **image.png** v. Soubor AAR pomocí `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Můžete taky přejít rozložení prostředků, které jsou umístěny ve. AAR. K tomuto účelu použijete **Resource.Layout** název rozložení zabalené uvnitř. AAR. Příklad:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**Textanalyzer.aar** příklad obsahuje soubor obrázku, který se nachází v **res/drawable/monkey.png**. Umožňuje přístup k danému prostředku bitové kopie a použít ho v našem příkladu aplikace:

Upravit **BindingTest** rozložení (**Main.axml**) a přidejte `ImageView` na konec `LinearLayout` kontejneru. To `ImageView` zobrazí obrázek nalezený na **@drawable/monkey**; tuto bitovou kopii budou načteny z oddílu prostředků **textanalyzer.aar**:

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

Zkompilování a spuštění **BindingTest** projektu. Aplikace spustí a k dispozici na snímku obrazovky na levé straně &ndash; když klepnete **počet SOUHLÁSKY**, výsledky se zobrazí, jak je znázorněno na pravé straně:

[![BindingTest zobrazení souladu počet](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


Blahopřejeme! Úspěšně jste vázaný knihovna Java. AAR!



## <a name="summary"></a>Souhrn

V tomto návodu jsme vytvořili knihovnu vazeb pro. AAR souboru, přidat vazby knihovny do minimální testování aplikace a aplikace, která ověří, že naše kódu C# můžete volat kód Java, které se nacházejí v se spustil. Soubor AAR.
Kromě toho jsme aplikaci přístup a zobrazit prostředek obrázku, který se nachází v rozšířené. Soubor AAR.


## <a name="related-links"></a>Související odkazy

- [Vytváření vazby knihovnu Java (video)](https://university.xamarin.com/classes#10090)
- [Vytvoření vazby balíčku .JAR](~/android/platform/binding-java-library/binding-a-jar.md)
- [Vytvoření vazby knihovny Java](~/android/platform/binding-java-library/index.md)
- [AarBinding (ukázka)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [Chyby 44573 jeden projektu nelze vytvořit vazbu více .aar souborů](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
