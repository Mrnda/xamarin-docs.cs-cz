---
title: "Základy Android prostředků"
ms.topic: article
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: c8f6832f618c37b3593f28c8efaeb87e4df5df03
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="android-resource-basics"></a>Základy Android prostředků

Téměř všechny aplikace pro Android bude mít nějaká prostředků v nich; minimálně mají často rozložení rozhraní uživatele ve formě souborů XML. Při prvním vytvoření aplikace pro Xamarin.Android výchozí prostředky jsou instalace šablonou projektu Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Soubory prostředků](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Soubory prostředků](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Pět souborů, které tvoří výchozí prostředky byly vytvořeny ve složce prostředky:

-  **Icon.PNG** &ndash; výchozí ikonu pro aplikaci

-  **Main.axml** &ndash; soubor rozložení výchozí uživatelské rozhraní pro aplikaci. Všimněte si, že při Android používá **.xml** používá Xamarin.Android přípona souboru **.axml** příponu souboru.

-  **Strings.xml** &ndash; tabulku řetězec usnadní lokalizaci aplikace

-  **AboutResources.txt** &ndash; to není nezbytné a může bezpečně odstranit. Právě poskytuje podrobný přehled prostředků složky a soubory, které obsahuje.

-  **Resource.Designer.cs** &ndash; tento soubor je automaticky generuje a spravuje pomocí Xamarin.Android a blokování Jedinečný ID, která přiřazené každému zdroji. To je velmi podobné a stejný účel jako R.java soubor, který podléhaly Android aplikace napsané v jazyce Java. Je automaticky vytvořen nástroje pro Xamarin.Android a bude znovu vygenerováno od času.

<a name="Creating_and_Accessing_Resources" />

## <a name="creating-and-accessing-resources"></a>Vytváření a přístup k prostředkům

Vytváření prostředků je jednoduché, přidávání souborů do adresáře pro typ prostředku, který je nejistá. Následující kopie obrazovky ukazuje, že řetězcové prostředky pro národní prostředí němčině byly přidány do projektu. Když **Strings.xml** byla přidána do souboru **akce sestavení** byla automaticky nastavena na **AndroidResource** nástroje pro Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Akce pro Strings.xml nastavena na AndroidResource sestavení](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Akce pro Strings.xml nastavena na AndroidResource sestavení](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

To umožňuje nástroje Xamarin.Android správně zkompilovat a prostředky v souboru APK pro vložení. Pokud z nějakého důvodu **akce sestavení** není nastavený na **Android prostředků**, pak soubory budou vyloučeny z APK a jakýkoli pokus o načtení nebo přístup k prostředkům, bude výsledkem chyba spuštění a dojde k chybě aplikace.

Kromě toho je důležité si uvědomit, že při Android podporuje jenom malá písmena názvy souborů pro položky prostředků, Xamarin.Android je, dovolí trochu více; bude podporovat názvy souborů velká a malá písmena. Konvence pro názvy obrázků, je použít malá podtržítky jako oddělovače (například **Moje\_image\_name.png**). Všimněte si, že názvy prostředků nelze zpracovat, pokud se používají pomlčky ani mezery jako oddělovače.

Po přidání prostředků do projektu, existují dva způsoby jejich použití v aplikaci &ndash; prostřednictvím kódu programu (uvnitř kódu) nebo ze souborů XML.

<a name="Referencing_Resources_Programmatically" />

## <a name="referencing-resources-programmatically"></a>Odkazování na zdroje prostřednictvím kódu programu

Přístup k těmto souborům prostřednictvím kódu programu, jsou přiřazeny jedinečný prostředku. ID prostředku je celé číslo definované v speciální třídu s názvem `Resource`, které se nacházejí v souboru **Resource.designer.cs**, a vypadá takto:

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

Každý ID prostředku se nachází v vnořené třídy, která odpovídá typu prostředku. Například, pokud soubor **Icon.png** byl přidán do projektu Xamarin.Android aktualizovat `Resource` vytvoření vnořené třídy názvem třídy, `Drawable` s konstantní uvnitř s názvem `Icon`.
Díky tomu soubor **Icon.png** na uvedené v kódu jako `Resource.Drawable.Icon`. `Resource` Třída by neměla být upravována ručně, protože veškeré změny, které jsou vytvářeny k němu budou přepsány Xamarin.Android.

Při odkazování na prostředky prostřednictvím kódu programu (v kódu), jsou dostupné prostřednictvím hierarchie tříd prostředky, které se používá následující syntaxe:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **Název balíčku** &ndash; balíček, který poskytuje daný prostředek a je pouze požadováno, pokud jsou používány prostředky z dalších balíčků.

-  **Typ prostředku** &ndash; jedná se o typ vnořeného prostředku, který je v rámci třídy prostředků popsané výše.

-  **Název prostředku** &ndash; Toto je název souboru prostředků (bez přípony) nebo hodnotu atributu android: name pro prostředky, které jsou v elementu XML.

<a name="Referencing_Resources_from_XML" />

## <a name="referencing-resources-from-xml"></a>Odkazování na prostředky ze souboru XML

Následující jsou přístup k prostředkům v souboru XML speciální syntaxi:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **Název balíčku** &ndash; balíček, který poskytuje daný prostředek a je pouze požadováno, pokud jsou používány prostředky z dalších balíčků.

-  **Typ prostředku** &ndash; jedná se o typ vnořeného prostředku, který je v rámci třídy prostředků.

-  **Název prostředku** &ndash; Toto je název souboru prostředku (*bez* typ přípony souboru) nebo hodnotu `android:name` atribut pro prostředky, které jsou v elementu XML.

Rozložení souboru, třeba obsah **Main.axml**, jsou následující:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

V tomto příkladu má [ `ImageView` ](https://developer.xamarin.com/recipes/android/controls/imageview) vyžadující drawable prostředek s názvem **příznak**. `ImageView` Má jeho `src` atribut nastaven na  **@drawable/flag** . Při spuštění aktivity bude vypadat Android v adresáři **prostředků/Drawable** pro soubor s názvem **flag.png** (přípona souboru může být jiný formát obrázku, jako je třeba **flag.jpg**) Tento soubor a zobrazit ji v `ImageView`.
Při spuštění této aplikace bude vypadat podobně jako na následujícím obrázku:

![Lokalizované ImageView](android-resource-basics-images/03-localized-screenshot.png)

