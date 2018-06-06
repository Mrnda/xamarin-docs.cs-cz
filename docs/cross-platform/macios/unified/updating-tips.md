---
title: Tipy pro aktualizaci kódu jednotné rozhraní API
description: Tento dokument popisuje běžné chyby a různé typy, které jsou užitečné při aktualizaci aplikace pro použití na Xamarin unifikované API.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
author: asb3993
ms.author: amburns
ms.openlocfilehash: cab27d5dc38eeab65728f242c6f11fd445601a88
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782115"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>Tipy pro aktualizaci kódu jednotné rozhraní API

Pomocí nástroje Migrace automatizované v sadě Visual Studio pro Mac převede iOS a Mac projekty odkazovat unifikované API (Xamarin.iOS.dll nebo Xamarin.Mac.dll) a pro vás provedl spoustu změny kódu (najdete v článku [verze Xamarin Studio Poznámky k části "Klasickém pro nástroj pro migraci kódu unifikované API"](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/) pro konkrétní podrobnosti).

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>Nelze najít NSInvalidArgumentException storyboard chyby

Je [chyb](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) v aktuální verzi sady Visual Studio pro Mac, který může vzniknout po pomocí nástroje Migrace automatizované převést projekt na jednotné rozhraní API. Po aktualizaci, pokud se zobrazí chybovou zprávu ve formátu:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

Můžete provést následující příkaz pro tento problém vyřešit, vyhledejte následující cílový soubor sestavení:

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

V tomto souboru, budete muset najít následující prohlášení cíl:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

A přidejte `DependsOnTargets="_CollectBundleResources"` atribut ho. Nějak tak:

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

Uložte soubor, restartujte Visual Studio pro Mac a provést čisté & opětovné sestavení projektu. Opravu pro tento problém by měl být vydal Xamarin krátce.

## <a name="useful-tips"></a>Užitečné tipy

Po použití nástroje migrace, může stále získat chybami kompilátoru, které vyžadují ruční zásah.
Některé kroky, které může být potřeba ručně opravit patří:

* Porovnání `enum`s může vyžadovat `(int)` přetypování.

* `NSDictionary.IntValue` nyní vrátí `nint`, dojde `Int32Value` který může být použit místo.

* `nfloat` a `nint` typy nemohou být označeny `const`;   `static readonly nint` je vhodnou alternativou.

* Věcí, které používají být přímo v `MonoTouch.` teď jsou obecně v oboru názvů `ObjCRuntime.` obor názvů (např: `MonoTouch.Constants.Version` je nyní `ObjCRuntime.Constants.Version`).

* Kód, který serializuje objekty může narušit při pokusu o serializaci `nint` a `nfloat` typy. Ujistěte se, že zkontrolujte, zda že kód serializace funguje podle očekávání po migraci.

* Někdy automatizovaného nástroje neúspěšných přístupů do kódu uvnitř `#if #else` direktivy podmíněného kompilátoru. V takovém případě budete muset provést opravy ručně (viz následující běžné chyby).

* Ručně exportovaných metod pomocí `[Export]` nemusí být opraven automaticky nástroj pro migraci, například v snippert tento kód je nutné ručně aktualizovat návratový typ, který má `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * Unifikované API neposkytuje implicitní převod mezi NSDate a .NET data a času, protože se nejedná o převodu z beze ztrát. Aby se zabránilo chyby související s `DateTimeKind.Unspecified` převést .NET `DateTime` na místní nebo v UTC před přetypování k `NSDate`.

 * Metody jazyka Objective-C kategorie jsou nyní generování jako rozšiřující metody v unifikované API. Například kód, který dřív používal `UIView.DrawString` by nyní odkazy `NSString.DrawString` v unifikované API.

 * Programování s využitím třídy AVFoundation s `VideoSettings` by se měl změnit používat `WeakVideoSettings` vlastnost. To vyžaduje `Dictionary`, která je k dispozici jako vlastnost na třídy nastavení, například:

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * NSObject `.ctor(IntPtr)` konstruktor se změnil z veřejné k chráněné ([zabránit neoprávněnému používání](~/cross-platform/macios/unified/overview.md#NSObject_ctor)).

 * `NSAction` byl [nahradit](~/cross-platform/macios/unified/overview.md#NSAction) s starndard .NET `Action`. Některé delegáti jednoduché (jeden parametr) také byla nahrazena `Action<T>`.

Nakonec odkazovat [rozdíly v klasickém unifikované API](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) k vyhledání změnách rozhraní API v kódu. Hledání [tuto stránku](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) vám pomůže najít klasické rozhraní API a co se jste aktualizovaná tak, aby.

**Poznámka:** `MonoTouch.Dialog` zůstane stejný obor názvů po migraci. Pokud váš kód používá **MonoTouch.Dialog** by měly být nadále používat tento obor názvů – provést *není* změnit `MonoTouch.Dialog` k `Dialog`!

## <a name="common-compiler-errors"></a>Běžné chyby kompilátoru

Další příklady běžných chyb jsou uvedené níže, společně s řešení:

**Chyba CS0012: Typ 'MonoTouch.UIKit.UIView' je definována v sestavení, které se neodkazuje.**

Oprava: To obvykle znamená, že projekt odkazuje na komponentu nebo balíčku NuGet, který nebyl sestaven s unifikované API. Doporučujeme odstranit a znovu přidat všechny součásti a NuGet balíčky. Pokud tato akce neodstraní chyba, vnější knihovny nemusí podporovat ještě unifikované API.

**Chyba MT0034: Nemůže obsahovat 'monotouch.dll' a "Xamarin.iOS.dll" ve stejném projektu Xamarin.iOS - 'Xamarin.iOS.dll' odkazuje explicitně, zatímco 'monotouch.dll' odkazuje ' Xamarin.Mobile, verze = 0.6.3.0, Culture = neutral, PublicKeyToken = null ".**

Oprava: Odstraňte komponentu, která je příčinou této chyby a znovu přidejte do projektu.

**Chyba CS0234: Typ nebo obor názvů, Foundation' neexistuje v oboru názvů 'MonoTouch'. Chybí odkaz na sestavení?**

Oprava: Automatizované nástroj pro migraci v sadě Visual Studio pro Mac *by* aktualizovat všechny `MonoTouch.Foundation` odkazuje na `Foundation`, ale v některých případech to bude potřeba aktualizovat ručně. Podobně jako chyby může být z jiných oborech názvů dříve součástí `MonoTouch`, jako například `UIKit`.

**CS0266 Chyba: Nelze implicitně převést double k 'System.float' – typ**

Oprava: Změňte typ a přetypovat `nfloat`. Této chybě může dojít také pro jiné typy s 64bitové ekvivalenty (například `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**CS0266 Chyba: Nelze implicitně převést typu 'CoreGraphics.CGRect' do 'System.Drawing.RectangleF'. Explicitní převod existuje (chybějící přetypování?)**

Oprava: Změňte instance na `RectangleF` k `CGRect`, `SizeF` k `CGSize`, a `PointF` k `CGPoint`. Obor názvů `using System.Drawing;` by měla být nahrazená s `using CoreGraphics;` (Pokud již není přítomen).

**Chyba CS1502: nejvhodnější přetížený odpovídající metoda ' CoreGraphics.CGContext.SetLineDash (System.nfloat, System.nfloat[])' obsahuje některé neplatné argumenty**

Oprava: Změnit typ pole na `nfloat[]` a explicitně vícesměrového vysílání `Math.PI`.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**Chyba CS0115: WordsTableSource.RowsInSection (UIKit.UITableView, int) je označena jako přepsání, ale k přepsání nalezena žádná vhodná metoda**

Oprava: Změnit návratovou hodnotu a parametr typy, které mají `nint`. K tomu obvykle dochází v metoda přepsání, například na `UITableViewSource`, včetně `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`atd.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Chyba CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView).**

Oprava: Pokud návratový typ se změní na `nint`, přetypovat návratovou hodnotu pro `nint`.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**Chyba CS1061: Typu 'CoreGraphics.CGPath' neobsahuje definici pro "AddElipseInRect.**

Oprava: Opravte pravopis k `AddEllipseInRect`. Ostatní změny názvu patří:

* Změňte 'Color.Black' `NSColor.Black`.
* Změňte MapKit 'AddAnnotation' `AddAnnotations`.
* Změňte AVFoundation 'DataUsingEncoding' `Encode`.
* Změňte AVFoundation 'AVMetadataObject.TypeQRCode' `AVMetadataObjectType.QRCode`.
* Změňte AVFoundation 'VideoSettings' `WeakVideoSettings`.
* Změnit PopViewControllerAnimated k `PopViewController`.
* Změňte CoreGraphics 'CGBitmapContext.SetRGBFillColor' `SetFillColor`.

**Chyba CS0546: nejde přepsat, protože 'MapKit.MKAnnotation.Coordinate' nemá přistupující objekt overridable set (CS0546)**

Při vytváření vlastní poznámka vytvoření podtřídy MKAnnotation souřadnic pole má žádné setter pouze příjemce.

[Opravte](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* Přidání pole ke sledování souřadnice
* Vrátí pole v metoda getter vlastnosti souřadnic
* Potlačí metodu SetCoordinate a nastavte vaše pole
* Volání SetCoordinate v vaší konstruktoru s parametrem předaná souřadnic

Výsledek by měl vypadat takto:

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>Související odkazy

- [Aktualizace aplikace](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualizace aplikací iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualizace aplikací pro Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualizace aplikace Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualizace vazby](~/cross-platform/macios/unified/update-binding.md)
- [Práce s nativní typy v multiplatformních aplikacích](~/cross-platform/macios/native-types-cross-platform.md)
- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
