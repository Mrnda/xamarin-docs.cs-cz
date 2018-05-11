---
title: Začínáme s systému macOS
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 85510168f37e724563eae59dfca438177b4feffe
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-macos"></a>Začínáme s systému macOS

## <a name="what-you-will-need"></a>Co budete potřebovat

* Postupujte podle pokynů v [Začínáme s jazyka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) průvodce.

## <a name="hello-world"></a>Ahoj světe

Nejprve vytvořte jednoduchý hello world – ukázka v jazyce C#.

### <a name="create-c-sample"></a>Vytvoření ukázkové C#

Otevřete Visual Studio pro Mac, vytvoření nového projektu knihovny tříd Mac s názvem **hello z csharp**a uložte ho do **~/Projects/hello-from-csharp**.

Nahraďte kód v **MyClass.cs** souboru následujícím fragmentem kódu:

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

Sestavte projekt. Výsledné sestavení se uloží jako **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Vytvoření vazby spravované sestavení.

Jakmile máte spravované sestavení, vazbu vyvoláním vložení .NET.

Jak je popsáno v [instalace](~/tools/dotnet-embedding/get-started/install/install.md) průvodce, to lze provést jako krok po sestavení ve vašem projektu s cílem vlastní MSBuild nebo ručně:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Rozhraní bude uložena v umístění **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Použití generovaný výstup v projektu Xcode

Otevřete Xcode a vytvořte novou aplikaci kakao. Pojmenujte ji **hello z csharp** a vyberte **jazyka Objective-C** jazyk.

Otevřete **~/Projects/hello-from-csharp/output** adresář v hledání, vyberte **hello z csharp.framework**, přetáhněte jej do projektu Xcode a umístěte jej právě vyšší **hello z csharp**  složky v projektu.

![Přetáhnout myší framework](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Zajistěte, aby **kopírovat položky v případě potřeby** je zaškrtnuta možnost v dialogovém okně, která se objeví a klikněte na tlačítko **Dokončit**.

![Kopírovat položky v případě potřeby](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Vyberte **hello z csharp** projektu a přejděte do **hello z csharp** cíle **Obecné** kartě. V **Embedded binární** přidejte **hello z csharp.framework**.

![Vložené binárních souborů](macos-images/hello-from-csharp-mac-embedded-binaries.png)

Otevřete **ViewController.m**a nahraďte jeho obsah se:

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

Nakonec spusťte projekt Xcode a přibližně toto se zobrazí:

![Hello z ukázky C# v simulátoru](macos-images/hello-from-csharp-mac.png)

Ukázka více úplný a lepšího [Zde jsou k dispozici](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
