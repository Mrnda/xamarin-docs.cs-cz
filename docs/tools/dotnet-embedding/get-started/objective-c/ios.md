---
title: Začínáme s iOS
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ab841c461356bd435db0c68e82c5e30d398d806a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-ios"></a>Začínáme s iOS

## <a name="requirements"></a>Požadavky

Kromě požadavků z našich [Začínáme s jazyka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) průvodce, budete také potřebovat:

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/) nebo novější

## <a name="hello-world"></a>Ahoj světe

Nejprve vytvořte jednoduchý hello world – ukázka v jazyce C#.

### <a name="create-c-sample"></a>Vytvoření ukázkové C#

Otevřete Visual Studio pro Mac, vytvořte nový projekt iOS knihovny tříd, pojmenujte ji **hello z csharp**a uložte ho do **~/Projects/hello-from-csharp**.

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

### <a name="bind-the-managed-assembly"></a>Vytvoření vazby spravované sestavení.

Jakmile máte spravované sestavení, vazbu vyvoláním vložení .NET.

Jak je popsáno v [instalace](~/tools/dotnet-embedding/get-started/install/install.md) průvodce, to lze provést jako krok po sestavení ve vašem projektu s cílem vlastní MSBuild nebo ručně:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Rozhraní bude uložena v umístění **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Použití generovaný výstup v projektu Xcode

Otevřete Xcode, vytvořte nový iOS jediné zobrazení aplikace, název **hello z csharp**a vyberte **jazyka Objective-C** jazyk.

Otevřete **~/Projects/hello-from-csharp/output** adresář v hledání, vyberte **hello z csharp.framework**, přetáhněte jej do projektu Xcode a umístěte jej právě vyšší **hello z csharp**  složky v projektu.

! [Přetažení framework] Images/Hello-from-CSharp-IOS-Drag-Drop-Framework.PNG)

Zajistěte, aby **kopírovat položky v případě potřeby** je zaškrtnuta možnost v dialogovém okně, která se objeví a klikněte na tlačítko **Dokončit**.

![Kopírovat položky v případě potřeby](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Vyberte **hello z csharp** projektu a přejděte do **hello z csharp** cíle **karta Obecné**. V **Embedded binární** přidejte **hello z csharp.framework**.

![Vložené binárních souborů](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Otevřete **ViewController.m**a nahraďte jeho obsah se:

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

Vložení .NET aktuálně nepodporuje bitcode na iOS, která je povolena pro některé šablony projektu Xcode. 

V nastavení projektu, zakažte ho:

![Možnost Bitcode](../../images/ios-bitcode-option.png)

Nakonec spusťte projekt Xcode a přibližně toto se zobrazí:

![Hello z ukázky C# v simulátoru](ios-images/hello-from-csharp-ios.png)
