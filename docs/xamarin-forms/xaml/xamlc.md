---
title: Kompilace jazyka XAML
description: "XAML můžete volitelně zkompilovat přímo do převodní jazyk (IL) s kompilátoru jazyka XAML (XAMLC)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: 3afb7608838a2c34f143d0563b50f03ad7f6ecf4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-compilation"></a>Kompilace jazyka XAML

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
> **Poznámka:**: `XamlCompilation` atribut a `XamlCompilationOptions` výčtu jsou umístěny ve `Xamarin.Forms.Xaml0` názvů, který musí být importovány pro použití je.


## <a name="related-links"></a>Související odkazy

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
