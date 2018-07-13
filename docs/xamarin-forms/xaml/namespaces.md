---
title: Obory názvů XAML v Xamarin.Forms
description: XAML používá atribut XML xmlns deklarací oboru názvů. Tento článek představuje obor názvů syntaxe XAML a ukazuje, jak k deklarování oboru názvů XAML pro přístup k typu.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: 30cbb2c3aebdafe2ebf35598c520ae725e01ce65
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995141"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Obory názvů XAML v Xamarin.Forms

_XAML používá atribut XML xmlns deklarací oboru názvů. Tento článek představuje obor názvů syntaxe XAML a ukazuje, jak k deklarování oboru názvů XAML pro přístup k typu._

## <a name="overview"></a>Přehled

Existují dvě deklarace oboru názvů XAML, které jsou vždy v rámci kořenového elementu souboru XAML. První definuje výchozí obor názvů, jak je znázorněno v následujícím příkladu kódu XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Určuje výchozí obor názvů, že prvky definované v souboru XAML s žádná předpona odkazují Xamarin.Forms třídy, jako například [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

Druhý obor názvů deklarace používá `x` předpony, jak je znázorněno v následujícím příkladu kódu XAML:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML používá k deklaraci jiné než výchozí obory názvů, s předponou se používají při odkazování na typy v rámci oboru názvů předpony. `x` Deklarace oboru názvů určuje, že prvky definované v rámci XAML s předponou `x` jsou použité pro prvky a atributy, které jsou přirozené pro XAML (konkrétně specifikaci XAML 2009).

V následující tabulce jsou podrobněji popsány dále `x` podporuje Xamarin.Forms atributy oboru názvů:

|Konstrukce|Popis|
|--- |--- |
|`x:Arguments`|Určuje argumenty konstruktoru pro jiné než výchozí konstruktor, nebo pro deklaraci objektu factory metody.|
|`x:Class`|Určuje název oboru názvů a třída pro třídy definované v XAML. Název třídy musí odpovídat názvu třídy v souboru kódu na pozadí. Poznámka: Tento konstruktor může být použit pouze v kořenového elementu souboru XAML.|
|`x:FactoryMethod`|Určuje metodu factory, který slouží k inicializaci objektu.|
|`x:FieldModifier`|Určuje úroveň přístupu pro vygenerované pole pro pojmenované elementy XAML.|
|`x:Key`|Určuje jedinečný uživatelský klíč pro každý zdroj v `ResourceDictionary`. Hodnota klíče se používá k načtení prostředků XAML a se obvykle používá jako argument pro `StaticResource` – rozšíření značek.|
|`x:Name`|Určuje název objektu modulu runtime pro prvek XAML. Nastavení `x:Name` je podobná deklaraci proměnné v kódu.|
|`x:TypeArguments`|Určuje obecný typ argumentů konstruktoru obecného typu.|

Další informace o `x:FieldModifier` atributu naleznete v tématu [modifikátory pole](~/xamarin-forms/xaml/field-modifiers.md). Další informace o `x:Arguments`, `x:FactoryMethod`, a `x:TypeArguments` atributy, naleznete v tématu [Passing Arguments v XAML](~/xamarin-forms/xaml/passing-arguments.md).

V XAML deklarace oboru názvů dědí z nadřazeného elementu na podřízený prvek. Proto při definování oboru názvů v kořenového elementu souboru XAML, dědí všechny prvky v rámci tohoto souboru deklarace oboru názvů.

## <a name="declaring-namespaces-for-types"></a>Deklarace oborů názvů pro typy

Typy lze odkazovat v XAML pomocí deklarace oboru názvů XAML s předponou, pomocí deklarace oboru názvů zadáním názvu oboru názvů Common Language Runtime (CLR) a volitelně název sestavení. Toho můžete dosáhnout tak, že definujete hodnoty pro následující klíčová slova v rámci deklarace oboru názvů:

- **CLR-namespace:** nebo **pomocí:** – obor názvů CLR deklarované v rámci sestavení, který obsahuje typy jsou elementy XAML. Toto klíčové slovo je povinný.
- **sestavení =** – sestavení, který obsahuje odkazovaný obor názvů CLR. Tato hodnota je název bez přípony souboru sestavení. Cesta k sestavení by se mělo vytvořit jako odkaz v souboru projektu, který obsahuje soubor XAML, který bude odkazovat sestavení. Toto klíčové slovo lze vynechat, pokud **clr-namespace** hodnota je v rámci stejného sestavení jako kód aplikace, který odkazuje na typy.

Všimněte si, že znak oddělující `clr-namespace` nebo `using` token z jeho hodnota je dvojtečkou, zatímco znak oddělující `assembly` token z jeho hodnota je shodné znaménko. Znak, který má použít mezi dvěma tokeny je středníkem.

Následující příklad kódu znázorňuje deklaraci oboru názvů XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Další možností to může být zapsán jako:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local` Předpona je konvence slouží k označení, že typy v rámci oboru názvů jsou vůči aplikaci lokální. Případně pokud jsou tyto typy v jiném sestavení, název sestavení by měl lze také definovat deklarace oboru názvů, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Předpona oboru názvů je poté zadané při deklarování instance typu z importované oboru názvů jako ukázáno v následujícím příkladu kódu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Souhrn

Tento článek zavedly syntaxe oboru názvů XAML které jsme vám ukázali způsobu deklarace oboru názvů XAML pro přístup k typu. Používá XAML `xmlns` atribut XML pro deklarace oboru názvů a typy lze odkazovat v XAML pomocí deklarace oboru názvů XAML s předponou.


## <a name="related-links"></a>Související odkazy

- [Předávání argumentů v XAML](~/xamarin-forms/xaml/passing-arguments.md)
