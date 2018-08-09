---
title: Začínáme s Iosem
description: Tento dokument popisuje, jak začít pracovat s Iosem pomocí vkládání technologie .NET. Tento článek popisuje požadavky a představuje ukázkovou aplikaci do ukazují, jak vytvořit vazbu spravovaného sestavení a použít výstup v projektu Xcode.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: d61eb8f1ad1def764c8552b2f047aa46cd712018
ms.sourcegitcommit: ef04a4ae1b19c1854a8e4e8315516d4030f4bbd6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39654825"
---
# <a name="getting-started-with-ios"></a>Začínáme s Iosem

## <a name="requirements"></a>Požadavky

Kromě požadavků z našich [Začínáme se službou Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) průvodce, budete také potřebovat:

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/) nebo novější

## <a name="hello-world"></a>Ahoj světe

Nejdříve vytvořte jednoduché hello world – ukázka v jazyce C#.

### <a name="create-c-sample"></a>Vytvoření ukázky jazyka C#

Otevřít Visual Studio pro Mac, vytvořte nový projekt knihovny tříd s Iosem, pojmenujte ho **hello z csharp**a uložit ho. tím **~/Projects/hello-from-csharp**.

Nahraďte kód v **MyClass.cs** souboru následujícím fragmentem kódu:

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Sestavte projekt a výsledné sestavení se uloží jako **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Vytvořit vazbu spravovaného sestavení

Jakmile budete mít spravovaným sestavením, navazují vyvolání vkládání technologie .NET.

Jak je popsáno v [instalace](~/tools/dotnet-embedding/get-started/install/install.md) průvodce, to lze provést jako krok po sestavení v projektu, s vlastní cíl nástroje MSBuild, nebo ručně:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Rozhraní budou umístěny do **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Generovaný výstup použít v projektu Xcode

Otevřete Xcode, vytvořit nový iOS jediné zobrazení aplikace, pojmenujte ho **hello z csharp**a vyberte **Objective-C** jazyka.

Otevřít **~/Projects/hello-from-csharp/output** adresáře ve Finderu, vyberte **hello z csharp.framework**, přetáhněte ho do projektu Xcode a umístěte ho přímo nad **hello z csharp**  složky v projektu.

![Přetáhnout myší framework](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

Ujistěte se, že **kopírovat položky v případě potřeby** se změnami, která se otevře dialogové okno a klikněte na tlačítko **Dokončit**.

![Kopírovat položky v případě potřeby](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Vyberte **hello z csharp** projektu a přejděte **hello z csharp** cíle **kartu Obecné**. V **vložený binární** části, přidejte **hello z csharp.framework**.

![Vložených binárních souborů](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Otevřít **ViewController.m**a nahraďte jeho obsah se:

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Vkládání technologie .NET aktuálně nepodporují bitcode na iOS, která je povolena pro některé šablony projektu Xcode. 

Zakažte ho v nastavení projektu:

![Možnost Bitcode](../../images/ios-bitcode-option.png)

Nakonec spusťte projekt Xcode a vypadat přibližně takto se zobrazí:

![Dobrý den ze ukázka C# v simulátoru](ios-images/hello-from-csharp-ios.png)
