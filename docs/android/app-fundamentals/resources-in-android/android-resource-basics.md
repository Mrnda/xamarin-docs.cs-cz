---
title: Základní informace o prostředcích androidu
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 207644f5a5d3d346214ba090dcd450e55fde2657
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241315"
---
# <a name="android-resource-basics"></a>Základní informace o prostředcích androidu

Téměř všechny aplikace pro Android bude mít nějaké prostředky v nich; minimálně často mají rozložení uživatelského rozhraní v podobě souborů XML. Při prvním vytvoření aplikace Xamarin.Android výchozí prostředky jsou nastavené šablonou projektu Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Soubory prostředků](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Soubory prostředků](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Pět souborů, které tvoří výchozí prostředky byly vytvořeny ve složce Resources:

-  **Icon.PNG** &ndash; výchozí ikona pro aplikace

-  **Main.axml** &ndash; soubor rozložení výchozí uživatelské rozhraní pro aplikaci. Všimněte si, že při Android používá **.xml** používá Xamarin.Android příponu souboru **.axml** příponu souboru.

-  **Strings.xml** &ndash; tabulka řetězců, abychom vám pomohli s lokalizace aplikace

-  **AboutResources.txt** &ndash; to není nutné a může být bezpečně odstranit. Poskytuje jenom základní přehled prostředků složky a soubory, které obsahuje.

-  **Resource.Designer.cs nejde** &ndash; tento soubor je automaticky generován a udržuje Xamarin.Android a obsahuje jedinečné ID přiřazené každému zdroji. To je velmi podobné a stejný účel jako R.java soubor, který by měla aplikace pro Android napsané v jazyce Java. Pomocí nástrojů pro Xamarin.Android se automaticky vytvoří a bude znovu vygenerováno čas od času.


## <a name="creating-and-accessing-resources"></a>Vytváření a přístup k prostředkům

Vytváření prostředků je stejně jednoduché jako přidání souborů do adresáře pro typ prostředku, který je nejistá. Následující snímek obrazovky ukazuje prostředky řetězců pro národní prostředí německé byly přidány do projektu. Když **Strings.xml** byl přidán do souboru **akce sestavení** byl automaticky nastaven na **AndroidResource** nástroji Xamarin.Android:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Akce pro Strings.xml nastavena na AndroidResource sestavení](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Akce pro Strings.xml nastavena na AndroidResource sestavení](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

To umožňuje nástroje Xamarin.Android správně kompilace a vložit prostředky v souboru APK. Pokud z nějakého důvodu **akce sestavení** není nastavená na **prostředcích Androidu**, pak se soubory vyloučí z APK a jakýkoliv pokus o načtení nebo přístup k prostředkům způsobí chybu za běhu a aplikaci dojde k chybě.

Kromě toho je důležité si uvědomit, že Android podporuje jenom malá písmena názvy souborů pro položky prostředků, Xamarin.Android je o něco méně striktní; bude podporovat názvy souborů velká a malá písmena. Konvence pro názvy imagí je jako oddělovače použít malá podtržítky (například **Moje\_image\_name.png**). Všimněte si, že názvy prostředků nelze zpracovat, pokud pomlčky ani mezery jsou použity jako oddělovače.

Po přidání prostředků do projektu existují dva způsoby, jak je používat v aplikaci &ndash; prostřednictvím kódu programu (uvnitř kód) nebo ze souborů XML.


## <a name="referencing-resources-programmatically"></a>Odkazy na prostředky prostřednictvím kódu programu

K těmto souborům přístup prostřednictvím kódu programu, jsou přiřazeny ID jedinečné prostředku. Toto ID prostředku je celé číslo definované ve speciální třídu s názvem `Resource`, který se nachází v souboru **Resource.Designer.cs nejde**, a vypadá přibližně takto:

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

Každé ID prostředku nachází uvnitř vnořené třídy, která odpovídá typu prostředku. Například, když soubor **Icon.png** byl přidán do projektu Xamarin.Android aktualizovat `Resource` vytvoření vnořené třídy třída, nazývá `Drawable` s konstantní uvnitř `Icon`.
To umožňuje soubor **Icon.png** odkazování v kódu jako `Resource.Drawable.Icon`. `Resource` Třídy by neměla být upravována ručně, jak Xamarin.Android přepíše všechny změny, které se provedly.

Při odkazování na prostředky prostřednictvím kódu programu (v kódu), jsou dostupné prostřednictvím hierarchie tříd prostředků, která používá následující syntaxi:

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **Název balíčku** &ndash; balíček, který poskytuje prostředek a jenom je požadováno, pokud se používají prostředky z jiných balíčků.

-  **Element ResourceType** &ndash; Toto je typ vnořeného prostředku, který je v rámci třídy prostředků je popsáno výše.

-  **Název prostředku** &ndash; Toto je název souboru prostředku (bez přípony) nebo hodnotu atributu android: name pro prostředky, které jsou v elementu jazyka XML.


## <a name="referencing-resources-from-xml"></a>Odkazy na prostředky ze souboru XML

Prostředky v souboru XML jsou dostupné přes následující speciální syntaxi:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **Název balíčku** &ndash; balíček, který poskytuje prostředek a jenom je požadováno, pokud se používají prostředky z jiných balíčků.

-  **Element ResourceType** &ndash; Toto je typ vnořeného prostředku, který je v rámci třídy prostředků.

-  **Název prostředku** &ndash; Toto je název souboru prostředku (*bez* rozšíření typu souboru) nebo hodnoty `android:name` atribut pro prostředky, které jsou v elementu jazyka XML.

Například obsah souboru rozložení **Main.axml**, jsou následující:

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

V tomto příkladu má [ `ImageView` ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) , která vyžaduje drawable prostředek s názvem **příznak**. `ImageView` Má jeho `src` atribut nastaven na **@drawable/flag**. Při spuštění aktivity s Androidem bude vypadat do adresáře **prostředek/Drawable** pro soubor s názvem **flag.png** (přípona souboru může být jiný formát obrázku, třeba **flag.jpg**) načte soubor a zobrazí ho v `ImageView`.
Při spuštění této aplikace bude vypadat podobně jako na následujícím obrázku:

![Lokalizované ImageView](android-resource-basics-images/03-localized-screenshot.png)

