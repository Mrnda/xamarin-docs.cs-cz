---
title: "Obory názvů jazyka XAML"
description: "XAML používá atribut XML xmlns pro deklarace oboru názvů. Tento článek představuje syntaxe oboru názvů jazyka XAML a ukazuje, jak deklarace oboru názvů jazyka XAML pro přístup k typu."
ms.topic: article
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2017
ms.openlocfilehash: b0afba90dab5cba4bad385f8d6447d8b83c1de3d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-namespaces"></a>Obory názvů jazyka XAML

_XAML používá atribut XML xmlns pro deklarace oboru názvů. Tento článek představuje syntaxe oboru názvů jazyka XAML a ukazuje, jak deklarace oboru názvů jazyka XAML pro přístup k typu._

## <a name="overview"></a>Přehled

Existují dvě deklarace oboru názvů jazyka XAML, které jsou vždy v rámci kořenový element souboru XAML. První definuje výchozí obor názvů, jak je znázorněno v následujícím příkladu kódu XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Určuje výchozí obor názvů, že elementy, které jsou definované v souboru XAML s žádná předpona. získáte na platformě Xamarin.Forms třídy, jako například [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Druhý deklaraci oboru názvů používá `x` předpony, jak je znázorněno v následujícím příkladu kódu XAML:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML používá předpony deklarovat jiné než výchozí obory názvů, s předponou používá při odkazování na typy v rámci oboru názvů. `x` Deklaraci oboru názvů určuje, že prvky definované v rámci XAML s předponou `x` se používají pro elementy a atributy, které jsou vnitřní do jazyka XAML (konkrétně specifikace jazyka XAML 2009).

V následující tabulce jsou podrobněji popsány dále `x` nepodporuje Xamarin.Forms atributy oboru názvů:

<table>
 <thead>
   <tr>
     <td><strong>konstrukce</strong></td>
     <td><strong>Popis</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><code>x:Arguments</code></td>
     <td>Určuje argumenty konstruktoru pro jiné než výchozí konstruktor, nebo na prohlášení metoda objektu factory.</td>
   </tr>
   <tr>
     <td><code>x:Class</code></td>
     <td>Určuje název oboru názvů a třídy pro třídy definované v jazyce XAML. Název třídy musí odpovídat názvu třídy souboru kódu na pozadí. Všimněte si, že tento konstruktor může vyskytovat pouze v kořenovém elementu souboru XAML.</td>
   </tr>
   <tr>
     <td><code>x:FactoryMethod</code></td>
     <td>Určuje metoda factory, která slouží k inicializaci objektu.</td>
   </tr>
   <tr>
     <td><code>x:Key</code></td>
     <td>Určuje jedinečný klíč uživatelem definované pro každý zdroj v <code>ResourceDictionary</code>. Hodnota klíče se používá k načtení prostředek XAML a obvykle se používá jako argument pro <code>StaticResource</code> – rozšíření značek.</td>
   </tr>
   <tr>
     <td><code>x:Name</code></td>
     <td>Určuje název objektu modulu runtime pro XAML element. Nastavení <code>x:Name</code> je podobná deklarace proměnné v kódu.</td>
   </tr>
   <tr>
     <td><code>x:TypeArguments</code></td>
     <td>Určuje argumenty obecného typu do konstruktoru objektu obecného typu.</td>
   </tr>
 </tbody>
</table>

Další informace o `x:Arguments`, `x:FactoryMethod`, a `x:TypeArguments` atributy, najdete v části [předání argumentů v jazyce XAML](~/xamarin-forms/xaml/passing-arguments.md).

V jazyce XAML deklarace oboru názvů dědí z nadřazeného prvku pro podřízený element. Proto při definování oboru názvů v kořenovém elementu souboru XAML, zdědí všechny elementy v souboru deklaraci oboru názvů.

## <a name="declaring-namespaces-for-types"></a>Deklarace oborů názvů pro typy

Typy můžete odkazovat v jazyce XAML deklarace oboru názvů jazyka XAML s předponou, o deklaraci oboru názvů zadání oboru názvů Common Language Runtime (CLR) a volitelně název sestavení. Toho dosáhnete tak, že definujete hodnoty pro následující klíčová slova v deklaraci oboru názvů:

- **CLR – obor názvů:** nebo **pomocí:** – obor názvů CLR deklarované v rámci sestavení, které obsahuje typy, které mají zveřejnit jako elementů XAML. This – klíčové slovo je povinný.
- **sestavení =** – sestavení, které obsahuje odkazovaný obor názvů CLR. Tato hodnota je název sestavení, bez přípony souboru. Cesta k sestavení by se mělo vytvořit jako odkaz v souboru projektu, který obsahuje soubor XAML, který bude odkazovat sestavení. This – klíčové slovo lze vynechat, pokud **clr – obor názvů** hodnota je ve stejném sestavení jako aplikační kód, který odkazuje na typy.

Všimněte si, že znak oddělující `clr-namespace` nebo `using` tokenu z její hodnota je dvojtečkou, zatímco znak oddělení `assembly` tokenu od hodnoty symbolem rovná se. Znak, který má používat mezi dvěma tokeny je středníkem.

Následující příklad kódu ukazuje deklaraci oboru názvů jazyka XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternativně to může být zapsán jako:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local` Předpona je konvence slouží k určení, že typy v rámci oboru názvů jsou místní vzhledem k aplikaci. Případně pokud typy v jiném sestavení, název sestavení je také nutné definovat v deklaraci oboru názvů, jak je ukázáno v následujícím příkladu kódu XAML:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Předpona oboru názvů je potom zadat při deklarace instance typu z importovaných oboru názvů, jako ukázáno v následujícím příkladu kódu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Souhrn

Tento článek zavedená syntaxe oboru názvů jazyka XAML a ukázal, jak lze deklarovat oboru názvů jazyka XAML pro přístup k typu. Používá XAML `xmlns` atribut XML pro deklarace oboru názvů a typy může odkazovat v jazyce XAML deklarace oboru názvů jazyka XAML s předponou.


## <a name="related-links"></a>Související odkazy

- [Předávání argumentů v jazyce XAML](~/xamarin-forms/xaml/passing-arguments.md)
