---
title: Kompilace XAML v Xamarin.Forms
description: Tento článek vysvětluje, jak XAML může volitelně zkompilován přímo do jazyka intermediate language (IL) s kompilátorem Xamarin.Forms XAML (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/02/2018
ms.openlocfilehash: b828e62ef1037bf47a2ae5fb303fbf8fcace6549
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997130"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Kompilace XAML v Xamarin.Forms

_Přímo do jazyka intermediate language (IL) s kompilátorem XAML (XAMLC) můžete volitelně zkompilovat XAML._

XAMLC nabízí celou řadu výhody:

- Provádí kontrolu za kompilace XAML upozornění uživatele nějaké chyby.
- Odebere určitá doba načtení a vytvoření instance pro elementy XAML.
- To pomáhá snížit velikost souboru v konečném sestavení už zahrnutím soubory .xaml.

XAMLC je zakázané ve výchozím nastavení k zajištění zpětné kompatibility. Může být povoleno na úrovni sestavení a třída tak, že přidáte `XamlCompilation` atribut.

Následující příklad kódu ukazuje, povolení XAMLC na úrovni sestavení:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

V tomto příkladu kompilace kontrolu všech XAML, které jsou obsaženy v rámci sestavení se provede, s chybami XAML, které jsou uváděny v době kompilace místo za běhu. Proto `assembly` předpona, která `XamlCompilation` atribut určuje, že platí atribut pro celé sestavení.

Následující příklad kódu ukazuje, povolení XAMLC na úrovni třídy:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

V tomto příkladu kompilace XAML pro kontrolu `HomePage` provede třídy a chyby ohlásil jako součást procesu kompilace.

> [!NOTE]
> `XamlCompilation` Atribut a `XamlCompilationOptions` výčtu jsou umístěny v `Xamarin.Forms.Xaml` obor názvů, který musí být importovány na jejich použití.


## <a name="related-links"></a>Související odkazy

- [XamlCompilation](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [XamlCompilationOptions](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
