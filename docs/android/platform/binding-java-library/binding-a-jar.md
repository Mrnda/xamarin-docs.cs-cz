---
title: Vytvoření vazby. JAR
description: Tento názorný postup obsahuje podrobné pokyny pro vytvoření vazby knihovny Xamarin.Android Java z Android. Soubor JAR.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/11/2018
ms.openlocfilehash: 2d9f2198dbb88e7614944ac73729a4e6eca42647
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="binding-a-jar"></a>Vytvoření vazby. JAR

_Tento názorný postup obsahuje podrobné pokyny pro vytvoření vazby knihovny Xamarin.Android Java z Android. Soubor JAR._

## <a name="overview"></a>Přehled

Android komunity nabízí mnoho Java knihovny, které můžete používat ve vaší aplikaci. Tyto knihovny Java jsou často součástí. Formát JAR (Java archiv), ale můžete balíček. V JAR *knihovna Java vazby* tak, aby jeho funkce je k dispozici pro aplikace Xamarin.Android. Účelem knihovna Java vazby je aby rozhraní API v. Soubor JAR kódu C# pomocí automaticky generovaný kód obálky k dispozici.

Xamarin nástrojů můžete vygenerovat knihovnu vazby z minimálně jeden vstup. JAR soubory. Knihovna vazby (. Sestavení knihovny DLL) obsahuje následující: 

-   Obsah původní. JAR soubory.

-   Spravované s obálky (MCW), které jsou C# typy této wrap odpovídající Java typů v rámci. JAR soubory.

Generovaný kód MCW JNI (nativní rozhraní Java) používá k předávání voláními rozhraní API pro základní. Soubor JAR. Můžete vytvořit vazby knihovny pro všechny. Soubor JAR, která byla původně cílem pro použití s Android (Všimněte si, že Xamarin nástrojů aktuálně nepodporuje vazba knihoven jiných - Android Java). Můžete také zvolit, aby sestavení knihovny vazby bez včetně obsahu. JAR souboru tak, aby knihovnu DLL má závislost na. JAR za běhu.

V této příručce jsme projdete kroky základní informace o vytvoření vazby knihovny pro jeden. Soubor JAR. Jsme budete ilustrují s příklad kde všechno půjde správné &ndash; kde neexistuje vlastní nastavení, nebo ladění vazby je požadovaná. 
[Vytváření vazby pomocí metadat](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) nabízí příklad pokročilejší scénáře, kde proces vytváření vazby není zcela automatické a je nutné určitou část ruční zásah. Přehled Java knihovny vazby obecně (s příklad základní kódu), najdete v tématu [vazby knihovna Java](~/android/platform/binding-java-library/index.md). 

 
## <a name="walkthrough"></a>Návod

V následujícím návodu vytvoříme knihovnu vazby pro [Picasso](http://square.github.io/picasso/), oblíbených Android. JAR, který obsahuje bitovou kopii, načítání a funkce mezipaměti. Budeme používat následující kroky k vytvoření vazby **picasso 2.x.x.jar** vytvořit nové sestavení .NET, které můžeme použít v projektu Xamarin.Android: 

1. Vytvoření nového projektu Java vazby knihovny.

2. Přidat. Soubor JAR do projektu.

3. Nastavení akce odpovídající sestavení pro. Soubor JAR.

4. Vyberte cílové rozhraní, která. JAR podporuje.

5. Sestavení knihovny vazby.

Po vytvoření jsme knihovně vazby jsme budete vyvíjet malé aplikace pro Android, která demonstruje naše schopnost volání rozhraní API v knihovně vazby. V tomto příkladu chceme přístup metody **picasso 2.x.x.jar**:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}

```csharp
After we generate a Bindings Library for **picasso-2.x.x.jar**,
we can call these methods from C#. For example:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>Vytvoření vazby knihovny

Před zahájením pomocí následujících kroků, stáhněte si prosím [picasso 2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar).

Nejdřív vytvořte nový projekt knihovny vazby. V sadě Visual Studio pro Mac nebo Visual Studio, vytvořte nové řešení a vyberte *knihovna pro Android vazby* šablony. (Na snímcích obrazovky v tomto názorném postupu použijte sadu Visual Studio, ale je velmi podobný jako Visual Studio pro Mac.) Název řešení **JarBinding**: 

[![Vytvoření projektu knihovny JarBinding](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

Šablona obsahuje **Jars** složku, kde můžete přidat vaše. JAR(s) na projekt knihovny vazby. Klikněte pravým tlačítkem myši **Jars** složky a vyberte **Přidat > existující položka**: 

[![Přidat existující položku](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Přejděte na **picasso 2.x.x.jar** předtím stáhli soubor, vyberte ho a klikněte na tlačítko **přidat**: 

[![Vyberte soubor jar a klikněte na tlačítko Přidat](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Ověřte, zda **picasso 2.x.x.jar** soubor byl úspěšně přidán do projektu: 

[![JAR přidat do projektu](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Když vytvoříte projekt knihovny vazby Java, je nutné zadat zda. JAR je vložených v knihovně vazby nebo zabalené samostatně. K tomu, zadáte, jednu z následujících *akce sestavení*: 

-   **EmbeddedJar** &ndash; . JAR vložený v knihovně vazby.

-   **InputJar** &ndash; . JAR bude udržovány odděleně od knihovně vazby.

Obvykle se používá **EmbeddedJar** akce sestavení tak, aby. JAR je automaticky zabalené do knihovny vazby. Toto je nejjednodušší možnost &ndash; bajtového kódu Java v. JAR je převeden do bajtového Dex kódu a vložené (společně se na spravované – obálky s možností) do vaší APK. Pokud chcete zachovat. JAR odděleně od vazby knihovny, můžete použít **InputJar** možnost; ale musíte zajistit, aby. Soubor JAR je k dispozici na zařízení, které spouští vaše aplikace. 

Nastavit akce sestavení na **EmbeddedJar**: 

[![Vyberte akci EmbeddedJar sestavení](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Dále otevřete vlastnosti pro konfiguraci projektu *cílové rozhraní*. Pokud. JAR používá žádné rozhraní Android API, nastavte cílové rozhraní API úrovně. JAR očekává. Obvykle se na vývojáře. Soubor JAR označí, které rozhraní API na úrovni (nebo úrovně),. JAR je kompatibilní s. (Další informace o nastavení cílové rozhraní a úrovně rozhraní API systému Android v obecné najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).)

Nastavte úroveň cílové rozhraní API pro knihovnu vazby (v tomto příkladu používáme API úrovně 19): 

[![Úroveň rozhraní API cíl nastavená na 19 rozhraní API](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


Nakonec sestavte knihovně vazby. I když některé zprávy upozornění, může se zobrazit, by měl projektu knihovny vazby sestavení úspěšně a vytvořit výstup. Knihovny DLL v následujícím umístění: **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>Použití knihovny vazby

To využívat. Knihovny DLL v aplikaci Xamarin.Android, postupujte takto:

1.  Přidáte odkaz na knihovnu vazby.

2.  Volání do. JAR prostřednictvím spravované obálky s možností. 

V následujících krocích vytvoříme minimální aplikaci, která používá ke stažení a zobrazit obrázek v knihovně vazby `ImageView`; "těžký lifting" se provádí kód, který se nachází v. Soubor JAR. 

Nejprve vytvořte novou aplikaci pro Xamarin.Android, který využívá knihovně vazby. Klikněte pravým tlačítkem na řešení a vyberte **přidat nový projekt**; název nového projektu **BindingTest**. Vytváříme tuto aplikaci ve stejném řešení jako vazby knihovny za účelem zjednodušení Tento názorný postup; aplikace, které zabírá knihovně vazby může, ale místo toho nacházet v jiné řešení: 

[![Přidat nový projekt BindingTest](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

Klikněte pravým tlačítkem myši **odkazy** uzlu **BindingTest** projektu a vyberte **přidat odkaz na...** :

[![Práva Přidat odkaz](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Zkontrolujte **JarBinding** projektu dříve vytvořili a klikněte na tlačítko **OK**:

[![Vyberte projekt JarBinding](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Otevřete **odkazy** uzlu **BindingTest** projektu a ověřte, zda **JarBinding** nachází odkaz: 

[![JarBinding se zobrazí pod odkazy](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Změnit **BindingTest** rozložení (**Main.axml**) tak, aby měl jeden `ImageView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

Přidejte následující `using` příkaz, který má **MainActivity.cs** &ndash; díky tomu je možné snadno přístup metody založené na jazyce Java `Picasso` třídu, která se nachází v knihovně vazby:

```csharp
using Com.Squareup.Picasso;
```

Změnit `OnCreate` metoda tak, aby používala `Picasso` třídy načíst obrázek z adresy URL a zobrazit ji v `ImageView`: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

Zkompilování a spuštění **BindingTest** projektu. Aplikace bude spuštěna, a po chvíli trvat (v závislosti na stavu sítě), by měla stáhnout a zobrazit bitovou kopii podobně jako na následujícím snímku obrazovky:

[![Snímek obrazovky BindingTest spuštěná](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Blahopřejeme! Úspěšně jste vázaný knihovna Java. JAR a použít ho v aplikaci Xamarin.Android.
 
 
## <a name="summary"></a>Souhrn

V tomto návodu jsme vytvořili knihovnu vazby pro třetí strany. Knihovna vazby JAR souboru, přidány do minimální testování aplikace a pak spustili aplikaci ověřte, že naše kódu C# můžete volat kód v jazyce Java umístěných v. Soubor JAR. 



## <a name="related-links"></a>Související odkazy

- [Vytváření vazby knihovnu Java (video)](https://university.xamarin.com/classes#10090)
- [Vytvoření vazby knihovny Java](~/android/platform/binding-java-library/index.md)
