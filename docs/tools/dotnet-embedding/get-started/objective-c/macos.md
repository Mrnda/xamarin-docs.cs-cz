---
title: "Začínáme s systému macOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 5e2f14e7b29f85e838563914089743f56239d7bb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-macos"></a>Začínáme s systému macOS


## <a name="what-you-will-need"></a>Co budete potřebovat

* Postupujte podle pokynů v našem [Začínáme s jazyka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) průvodce.

* Sestavení .NET pro použití s **Embeddinator 4000**.

* MacOS kakao aplikace

Pokračovat po jste postupovali podle pokynů v našem [Začínáme s jazyka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) průvodce. Pokud již máte sestavení .NET můžete přeskočit přímo do **pomocí Embeddinator-4000** části.

## <a name="creating-a-net-assembly"></a>Vytváření sestavení rozhraní .NET

K vytvoření sestavení .NET, budete muset otevřít [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/) a vytvořte novou **projektu knihovny .NET** nástrojem *soubor > nové řešení > jiné > .NET > knihovny*. Klikněte na tlačítko Další a poskytněte *počasí* jako *název projektu*a klikněte na *vytvořit*.

Provedením následujících kroků:

1. Odstranit **MyClass.cs** souboru a **vlastnosti** složky.

2. Klikněte pravým tlačítkem na *počasí Projekt > Přidat > Nový soubor.*

3. Vyberte *prázdné třídy* a používat **XAMWeatherFetcher** jako název, potom na tlačítko Nový.

4. Nahraďte obsah *XAMWeatherFetcher.cs* následujícím kódem:

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

Bude zjistíte, že `Using System.Json;` vrátí chybu; Chcete-li tomu je potřeba udělat následující:

1. Dvakrát klikněte na **odkazy** složky.

2. Klikněte na **balíčky** kartě.

3. Zkontrolujte **System.Json**.

4. Klikněte na tlačítko **Ok**.

Jakmile výše dokončí všechny budeme muset udělat je sestavení naše sestavení .NET kliknutím na *nabídkou sestavit > sestavení všechny* nebo ⌘ + b. Vytvořte úspěšný by se zobrazit zpráva v horní stavový řádek.

Nyní klikněte pravým tlačítkem na *počasí* uzel projektu a vyberte *odhalit v hledání*. Přejděte ve vyhledávací *bin/Debug* složky; uvnitř ho zjistíte **Weather.dll.**

## <a name="using-embeddinator-4000"></a>Pomocí Embeddinator 4000

Pokud jste nainstalovali Embeddinator-4000 pomocí instalačního programu pkg a spustí novou relaci terminálu po instalaci, byste měli použít **objcgen** příkazu (jinak můžete použít absolutní cesty: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`); **objcgen** je nástroj, který je potřeba generovat nativní knihovny ze sestavení rozhraní .NET.

Otevřete terminál `cd` do složky obsahující Weather.dll a spusťte **objcgen** s argumenty vidíte níže:

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

Všechno, co budete potřebovat se umístí do **výstup** adresář Další *Weather.dll*. Pokud máte vlastní sestavení .NET, nahraďte *Weather.dll* s ho a postupujte podle stejné kroky výše.

## <a name="using-the-generated-output-in-an-xcode-project"></a>Pomocí generovaný výstup v projektu Xcode

Otevřete Xcode a vytvořte novou **systému macOS kakao aplikace** a pojmenujte ji **MyWeather**. Klikněte pravým tlačítkem na *uzel projektu MyWeather*, vyberte *přidat soubory do "MyWeather"*, přejděte na **výstup** adresář vytvořený *Embeddinator 4000* a přidejte následující soubory:

* bindings.h
* embeddinator.h
* glib.h
* mono-support.h
* mono_embeddinator.h
* objc-support.h
* libWeather.dylib
* Weather.dll

Zajistěte, aby **kopírovat položky v případě potřeby** se změnami panelu Možnosti dialogového okna souboru.

Teď musíme Ujistěte se, že **libWeather.dylib** a **Weather.dll** dostat do sady prostředků aplikace:

* Klikněte na *uzel projektu MyWeather*.
* Vyberte *fáze buildu* kartě.
* Přidejte nový *fáze kopírování souborů*.
* Na *cílové* vyberte **rozhraní** a přidejte **libWeather.dylib**.
* Přidejte nový *fáze kopírování souborů*.
* Na *cílové* vyberte **spustitelné soubory**, přidat **Weather.dll** a zajistěte, aby *kód přihlašování kopie* je zaškrtnuté.

Nyní otevřete **ViewController.m** a nahraďte jeho obsah s:

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

Nakonec spusťte projekt Xcode a přibližně toto se zobrazí:

![Spuštění ukázkové MyWeather](macos-images/weather-from-csharp-macos.png)

Obsáhlejší a lépe vypadající ukázka je dostupná [zde](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
