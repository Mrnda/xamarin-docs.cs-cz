---
title: Začínáme s iOS
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 34afdd9e91ebfbe7ad57c7eec6ba7f05fff1a2aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-ios"></a>Začínáme s iOS


## <a name="requirements"></a>Požadavky

Kromě požadavků z našich [Začínáme s jazyka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) Průvodce také budete potřebovat:

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/) nebo novější

## <a name="hello-world"></a>Ahoj světe

První Vytvořme jednoduchý hello world – ukázka v jazyce C#.

### <a name="create-c-sample"></a>Vytvoření ukázkové C#

Otevřete Visual Studio pro Mac, vytvořte nový projekt iOS knihovny tříd, pojmenujte ji `hello-from-csharp`a uložte ho do `~/Projects/hello-from-csharp`.

Nahraďte kód v `MyClass.cs` souboru následujícím fragmentem kódu:

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

Sestavení projektu, se uloží výsledné sestavení jako `~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`.

### <a name="bind-the-managed-assembly"></a>Vytvoření vazby spravované sestavení.

Spusťte embeddinator vytvoření nativní rozhraní pro spravované sestavení:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Rozhraní bude uložena v umístění `~/Projects/hello-from-csharp/output/hello-from-csharp.framework`.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Použití generovaný výstup v projektu Xcode

Otevřete Xcode a vytvořit nové iOS jediné zobrazení aplikace, název `hello-from-csharp` a vyberte **jazyka Objective-C** jazyk.

Otevřete `~/Projects/hello-from-csharp/output` adresář v hledání, vyberte `hello-from-csharp.framework`, přetáhněte jej do projektu Xcode a umístěte jej právě vyšší `hello-from-csharp` složky v projektu.

! [Přetažení framework] Images/Hello-from-CSharp-IOS-Drag-Drop-Framework.PNG)

Zajistěte, aby `Copy items if needed` je zaškrtnuta možnost v dialogovém okně, která se objeví a klikněte na tlačítko `Finish`.

![Kopírovat položky v případě potřeby](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Vyberte `hello-from-csharp` projektu a přejděte do `hello-from-csharp` cíle **karta Obecné**. V **Embedded binární** přidejte `hello-from-csharp.framework`.

![Vložené binárních souborů](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Otevřete ViewController.m a nahraďte jeho obsah se:

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

Nakonec spusťte projekt Xcode a přibližně toto se zobrazí:

![Hello z ukázky C# v simulátoru](ios-images/hello-from-csharp-ios.png)
