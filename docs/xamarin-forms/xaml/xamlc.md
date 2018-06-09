---
title: Kompilace XAML v Xamarin.Forms
description: Tento článek vysvětluje, jak XAML můžete volitelně zkompilovat přímo do převodní jazyk (IL) s kompilátorem Xamarin.Forms XAML (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: 0128fecebe9f6ba8f55e965a8fa65787d03d9ded
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245742"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Kompilace XAML v Xamarin.Forms

_XAML můžete volitelně zkompilovat přímo do převodní jazyk (IL) s kompilátoru jazyka XAML (XAMLC)._

XAMLC nabízí řadu výhody:

- Provede kompilaci kontrola XAML, upozornění uživatele o chybách.
- Odebere některé zatížení a vytváření instancí času elementů XAML.
- Pomáhá snížit velikost souboru poslední sestavení už zahrnutím souborů XAML.

XAMLC vypnutá ve výchozím nastavení pro zajištění zpětné kompatibility. Může být povoleno na úrovni sestavení a třída přidáním `XamlCompilation` atribut.

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

V tomto příkladu kompilaci kontroluje všechny XAML obsažené v `PhotoApp` obor názvů se provede, s chybami XAML nehlásí v kompilaci místo běhu.
`assembly` Předpona, která `XamlCompilation` atribut určuje atribut, které se vztahují na celou sestavení.

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

V tomto příkladu kontrola XAML pro kompilaci `HomePage` třídy se provede a chyby hlášené jako součást procesu kompilace.

> [!NOTE]
> `XamlCompilation` Atribut a `XamlCompilationOptions` výčtu jsou umístěny ve `Xamarin.Forms.Xaml` názvů, který musí být importovány pro použití je.


## <a name="related-links"></a>Související odkazy

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
