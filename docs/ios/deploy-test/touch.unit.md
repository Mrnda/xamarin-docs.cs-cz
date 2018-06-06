---
title: Aplikace na platformě Xamarin.iOS testování částí
description: Tento dokument obsahuje přehled o tom, jak testování částí aplikace pro Xamarin.iOS. Popisuje postup vytvoření projektu testů jednotek, zápis testů a spouštění testů.
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ce2b452d50222ac3561dab5b76915b7ae634934b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785460"
---
# <a name="unit-testing-xamarinios-apps"></a>Aplikace na platformě Xamarin.iOS testování částí

Tento dokument popisuje, jak vytvářet testy částí pro Xamarin.iOS projekty.
Testování částí pomocí Xamarin.iOS se provádí pomocí rozhraní Touch.Unit, která obsahuje oba iOS pro spuštění testu, jakož i upravenou verzi NUnit názvem [Touch.Unit](https://github.com/xamarin/Touch.Unit) poskytující známé sada rozhraní API pro zápis testů částí.

## <a name="setting-up-a-test-project"></a>Nastavení testovacího projektu

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

K nastavení testování rozhraní pro váš projekt částí, všechny musíte udělat je přidat do řešení pro projekt typu **iOS projektu testů jednotek**. To můžete provést kliknutím pravým tlačítkem myši na řešení a výběrem **Přidat > Přidat nový projekt**. Ze seznamu vyberte **iOS > testy > unifikované API > iOS projektu testů jednotek** (můžete vybrat buď C# nebo F #).

![](touch.unit-images/00.png "Vyberte buď C# nebo F #")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

K nastavení testování rozhraní pro váš projekt částí, všechny musíte udělat je přidat do řešení pro projekt typu **iOS projektu testů jednotek**. To můžete provést kliknutím pravým tlačítkem myši na řešení a výběrem **Přidat > Nový projekt...** . Ze seznamu vyberte **Visual C# > iOS > jednotky testování aplikace (iOS)**.

![](touch.unit-images/00a.png "Jednotka testování aplikace iOS")

-----

Výše vytvoří základní projekt, který obsahuje program základní runner a který odkazuje na nové sestavení MonoTouch.NUnitLite, projekt bude vypadat například takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](touch.unit-images/01.png "Projekt v Průzkumníku řešení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](touch.unit-images/01a.png "Projekt v Průzkumníku řešení")

-----

`AppDelegate.cs` Třída obsahuje nástroj test runner a vypadá takto:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
        UIWindow window;
        TouchRunner runner;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
                // create a new window instance based on the screen size
                window = new UIWindow (UIScreen.MainScreen.Bounds);
                runner = new TouchRunner (window);

                // register every tests included in the main application/assembly
                runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

                window.RootViewController = new UINavigationController (runner.GetViewController ());

                // make the window visible
                window.MakeKeyAndVisible ();

                return true;
        }
}
```

## <a name="writing-some-tests"></a>Zápis některých testů

Nyní když máte základní prostředí na místě, byste měli zapsat svou první sadu testů.

Testy se zapisují vytvořením tříd, které mají `[TestFixture]` atribut na ně použity. V každé třídě TestFixture byste měli použít `[Test]` atribut každou metodu, kterou chcete nástroj test runner k vyvolání. Skutečné testovací zařízení můžete za provozu v všechny soubory v projektu testů.

Rychle začít vyberte **přidat nebo přidat nový soubor** a vyberte ve skupině pro Xamarin.iOS **UnitTests**. Bude přidáno kostru soubor, který obsahuje jeden předávání testů, jeden selhání testu a jeden ignorováno testů, vypadá takto:

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

        [TestFixture]
        public class Tests {

                [Test]
                public void Pass ()
                {
                        Assert.True (true);
                }

                [Test]
                public void Fail ()
                {
                        Assert.False (true);
                }

                [Test]
                [Ignore ("another time")]
                public void Ignore ()
                {
                        Assert.True (false);
                }
        }
}
```

## <a name="running-your-tests"></a>Spuštění testů

Ke spuštění tohoto projektu v řešení klikněte pravým tlačítkem na ho a vyberte **ladění položky** nebo **spustit položku**.

Nástroj test runner vám umožní vidět testy, které jsou zaregistrované a vybrat jednotlivě, které testy mohou být provedeny.

[![](touch.unit-images/02.png "Seznam registrovaných testů")](touch.unit-images/02.png#lightbox) 

[![](touch.unit-images/03.png "Jednotlivé textu")](touch.unit-images/03.png#lightbox) 

[![](touch.unit-images/04.png "Spuštění výsledky")](touch.unit-images/04.png#lightbox)

Komunikací jednotlivé testy můžete spustit výběrem upínací z vnořené zobrazení, nebo můžete spustit všechny testy vším"spustit". Pokud spustíte test výchozí, který by měl obsahovat jeden předávání testů, jednou chybou a jeden ignorováno test. Toto je, jak vypadá sestavy, a můžete přímo do testů selhání k podrobnostem a získat další informace o selhání:

[![](touch.unit-images/05.png "Ukázková sestava") ](touch.unit-images/05.png#lightbox) [ ![ ] (touch.unit-images/05.png "je vzorová sestava") ](touch.unit-images/05.png#lightbox) [ ![ ] (touch.unit-images/05.png "vzorku Sestava")](touch.unit-images/05.png#lightbox)

Můžete také prohlédnout v okně výstupu aplikace ve vaší IDE, pokud chcete zobrazit, které testy se spouštějí a jejich aktuální stav.

## <a name="writing-new-tests"></a>Zápis nové testů

NUnitLite je upravenou verzi NUnit názvem [Touch.Unit](https://github.com/xamarin/Touch.Unit) projektu. Je lightweight testování rozhraní pro platformu .NET, podle nápady v [NUnit](http://nunit.com/) a poskytování podmnožinu jeho funkcí.
Používá minimální prostředky a spustí na prostředků omezené platformách, jako jsou ty používané v vložené a mobilní vývoj. Můžete [procházet rozhraní API NUnitLite](https://developer.xamarin.com/api/namespace/NUnitLite/) dostupný v Xamarin.iOS. Základní kostru poskytované šablony test jednotky, jsou hlavní vstupní bod [Assert – třídy](https://developer.xamarin.com/api/type/NUnit.Framework.Assert/) metody.

Kromě metod třídy assert funkci testování částí je rozdělená na následující obory názvů, které jsou součástí NUnitLite:

-   [NUnit.Framework](https://developer.xamarin.com/api/namespace/NUnit.Framework/)
-   [NUnit.Constraints](https://developer.xamarin.com/api/namespace/NUnit.Framework.Constraints/)
-   [NUnitLite](https://developer.xamarin.com/api/namespace/NUnitLite/)
-   [NUniteLite.Runner](https://developer.xamarin.com/api/namespace/NUnitLite.Runner/)


Nástroj test runner specifické pro Xamarin.iOS jednotky jsou zde uvedeny:

-   [NUnit.UI.TouchRunner](https://developer.xamarin.com/api/type/NUnit.UI.TouchRunner/)
