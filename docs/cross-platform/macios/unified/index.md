---
title: "Jednotné rozhraní API"
description: "Styl nové rozhraní API je jednodušší než kdy dřív přístup ke sdílení kódu mezi Mac a iOS a také umožňuje podporu 32 až 64 bitové aplikace se stejným binární."
ms.topic: article
ms.prod: xamarin
ms.assetid: 12027F75-70DD-436B-8668-4FF66567B4A8
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 4845b603fd7877e4bada5f452ef006f0341f0e61
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="unified-api"></a>Jednotné rozhraní API

_Styl nové rozhraní API je jednodušší než kdy dřív přístup ke sdílení kódu mezi Mac a iOS a také umožňuje podporu 32 až 64 bitové aplikace se stejným binární._

Účelem vylepšení kód sdílení mezi Mac a iOS a umožňuje vývojářům tak, aby měl jeden základu kódu, které funguje na 32 až 64 bitů v brzké 2015 zavedli jsme nové rozhraní API v produktech Xamarin.Mac a Xamarin.iOS volat rozhraní API Unified.

> [!IMPORTANT]
> **Classic profil vyřazení:** při přidání nové platformy v Xamarin.iOS jsme spouštějí postupně přestat používat funkce z klasického profilu (monotouch.dll). Například možnost bez NRC (-ref počet nových) byla odebrána. NRC vždy byla povolená pro všechny jednotná aplikace (tj. bez NRC se nikdy možnost) a nemá žádné známé problémy. Možnost použití Boehm jako garbage collector v budoucích verzích dojde k odebrání. Je také možnost Nikdy dostupné pro jednotné aplikace. Úplné odebrání classic podpory naplánován další patří verze Xamarin.iOS 10.0.




## <a name="overviewoverviewmd"></a>[Přehled](overview.md)

Popisuje důvody unifikované API a vysvětluje rozdíly z rozhraní API Classic podrobně. Odkazovat na tuto stránku pochopit změny, které byly provedeny v unifikované API.

## <a name="updating-classic-api-based-apps"></a>Aktualizace aplikace klasické založené na rozhraní API

Postupujte podle příslušných pokynů pro vaši platformu:

- [Aktualizovat existující aplikace](updating-apps.md)
- [Aktualizovat stávající iOS aplikace](updating-ios-apps.md)
- [Aktualizovat stávající aplikace pro Mac](updating-mac-apps.md)
- [Aktualizovat stávající aplikace Xamarin.Forms](updating-xamarin-forms-apps.md)
- [Migrace vazbu na jednotné rozhraní API](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[Tipy pro aktualizaci kódu jednotné rozhraní API](updating-tips.md)

Bez ohledu na to, jaké aplikace při migraci, podívejte se na [tyto tipy](updating-tips.md) vám pomůže úspěšně aktualizovat jednotné rozhraní API.



# <a name="the-road-to-64-bits"></a>Silniční na 64 bitů

Pozadí na podporu 32 až 64-bitové aplikace a informace o rozhraní najdete v článku [32 a 64 bitů platformy aspekty](~/cross-platform/macios/32-and-64.md).

 <a name="new-data-types" />

## <a name="new-data-types"></a>Nové datové typy

Základní rozdíl Mac a iOS rozhraní API použijte specifické pro architekturu datové typy, které jsou vždy 32bitové na platformách 32bitová a 64bitová verze na 64bitových platformách.

Například jazyka Objective-C mapuje `NSInteger` datový typ pro `int32_t` na 32bitové systémy a na `int64_t` v 64bitových systémech.

Tak, aby odpovídaly toto chování, v našem rozhraní API Unified jsme nahradit předchozí použití `int` (který v rozhraní .NET je definován jako vždy `System.Int32`) na nový datový typ: `System.nint`.  Si můžete představit "n" jako význam "nativní", takže nativní celé číslo, zadejte na platformě.

Představujeme `nint`, `nuint` a `nfloat` také poskytuje datové typy vytvořená na základě jejich potřeby.

Další informace o těchto změnách typu dat, najdete v článku [nativní typy](~/cross-platform/macios/nativetypes.md) dokumentu.

#<a name="how-to-detect-the-architecture-of-ios-apps"></a>K zjištění architektura aplikací pro iOS

Můžou nastat situace, kdy aplikace musí vědět, pokud je spuštěn na 32bitová nebo 64bitová verze systému iOS. Následující kód slouží ke kontrole architekturu:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```


<a name="namespace-changes" />

# <a name="library-split"></a>Knihovna rozdělení

Z tohoto bodu na rozhraní API se zobrazí dvěma způsoby:

-  **Klasické rozhraní API:** omezené na 32bitovou (pouze) a zveřejněné v `monotouch.dll` a `XamMac.dll` sestavení.
-  **Jednotné rozhraní API:** podporují 32 až 64 bit vývoj pomocí jediného rozhraní API dostupná v `Xamarin.iOS.dll` a `Xamarin.Mac.dll` sestavení.

To znamená, že pro podnikové vývojáře (ne které se budou zaměřovat obchod), můžete nadále používat existující klasické rozhraní API, protože jsme se zachovat zachování jejich navždy nebo můžete upgradovat na nových rozhraní API.

## <a name="namespace-changes"></a>Namespace změny

Pokud chcete zkrátit třecí sdílet kód mezi našich produktů Mac a iOS, měníme obory názvů pro rozhraní API v produkty.

Nemůžeme se dávka předponu "MonoTouch" z našich produktů iOS a "MonoMac" z našich produktů Mac pro datové typy.

To umožňuje jednodušší sdílet mezi platformami Mac a iOS bez nutnosti Podmíněná kompilace kódu a bude snížení šumu v horní části zdrojové soubory vašeho kódu.

-  **Klasické rozhraní API:** použijte obory názvů `MonoTouch.` nebo `MonoMac.` předponu.
-  **Jednotné rozhraní API:** žádná předpona oboru názvů

## <a name="api-changes"></a>Rozhraní API změny

Rozhraní API Unified odebere zastaralé metody a existuje několik instancí tam, kde existuje byla překlepům v názvech rozhraní API, pokud byly vázány na původní MonoTouch a MonoMac obory názvů v klasické rozhraní API. Tyto instance byla opravena v nových rozhraní API Unified a bude potřeba aktualizovat v součásti, iOS a Mac aplikace. Tady je seznam nejčastějších ty, které můžete narazit na:

<table width="100%" border="1">
<tr>
    <th>Název metody klasické rozhraní API</th>
    <th>Název metody jednotné rozhraní API</th>
</tr>
<tr>
    <td>UINavigationController.PushViewControllerAnimated()</td>
    <td>UINavigationController.PushViewController()</td>
</tr>
<tr>
    <td>UINavigationController.PopViewControllerAnimated()</td>
    <td>UINavigationController.PopViewController()</td>
</tr>
<tr>
    <td>CGContext.SetRGBFillColor()</td>
    <td>CGContext.SetFillColor()</td>
</tr>
<tr>
    <td>NetworkReachability.SetCallback()</td>
    <td>NetworkReachability.SetNotification()</td>
</tr>
<tr>
    <td>CGContext.SetShadowWithColor</td>
    <td>CGContext.SetShadow</td>
</tr>
<tr>
    <td>UIView.StringSize</td>
    <td>UIKit.UIStringDrawing.StringSize</td>
</tr>
</table>

Úplný seznam změny přepnutím z klasického unifikované API, najdete v tématu naše [vs Classic (monotouch.dll) rozhraní API Unified (Xamarin.iOS.dll) rozdíly](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) dokumentaci.


## <a name="updating-to-unified"></a>Aktualizace na jednotné

Několik starý nebo porušený/zastaralá rozhraní API v **classic** nejsou k dispozici v **Unified** rozhraní API. Může být snazší opravit `CS0616` upozornění před zahájením vaší (ruční nebo automatické) upgradovat, protože budete mít `[Obsolete]` navést na pravém API atribut zprávy (součást upozornění).

Všimněte si, že jsme publikování [ *rozdílové* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) z klasického vs jednotné rozhraní API změny, které lze použít buď před nebo po aktualizace vašeho projektu. Stále opravě nahrazuje volání v klasickém se často být spoustu času (méně hledání dokumentaci).

Postupujte podle těchto pokynů můžete [aktualizovat stávající aplikace pro iOS](~/cross-platform/macios/unified/updating-ios-apps.md), nebo [Mac aplikace](~/cross-platform/macios/unified/updating-mac-apps.md) jednotné rozhraní API.
Zkontrolujte zbývající části této stránky a [tyto tipy](~/cross-platform/macios/unified/updating-tips.md) Další informace o migraci vašeho kódu.


## <a name="nuget"></a>NuGet

Balíčky NuGet, které dřív podporované Xamarin.iOS prostřednictvím klasické rozhraní API publikovat své sestavení s využitím **Monotouch10** Přezdívka platformy.

Rozhraní API Unified zavádí nový identifikátor platformu pro kompatibilní balíčky - **Xamarin.iOS10**. Existující balíčky NuGet potřebovat při vytváření rozhraní API Unified aktualizovat přidání podpory pro tuto platformu.

> [!IMPORTANT]
> **Poznámka:** Pokud máte chybu ve formě _"Chyba 3 nesmí obsahovat 'monotouch.dll' a"Xamarin.iOS.dll"ve stejném projektu Xamarin.iOS – 'Xamarin.iOS.dll' odkazuje explicitně, zatímco 'monotouch.dll' odkazuje ' xxx, Verze = 0.0.000, Culture = neutral, PublicKeyToken = null. "_ po převedení aplikace jednotné rozhraní API, je obvykle kvůli s komponenta nebo balíček NuGet do projektu, která nebyla aktualizována jednotné rozhraní API. Budete muset odebrat existující součásti nebo NuGet, aktualizujte na verzi podporující rozhraní API Unified a provést čisté sestavení.





# <a name="the-road-to-64-bits"></a>Silniční na 64 bitů

Pozadí na podporu 32 až 64-bitové aplikace a informace o rozhraní najdete v článku [32 a 64 bitů platformy aspekty](~/cross-platform/macios/32-and-64.md).

 <a name="new-data-types" />

## <a name="new-data-types"></a>Nové datové typy

Základní rozdíl Mac a iOS rozhraní API použijte specifické pro architekturu datové typy, které jsou vždy 32bitové na platformách 32bitová a 64bitová verze na 64bitových platformách.

Například jazyka Objective-C mapuje `NSInteger` datový typ pro `int32_t` na 32bitové systémy a na `int64_t` v 64bitových systémech.

Tak, aby odpovídaly toto chování, v našem rozhraní API Unified jsme nahradit předchozí použití `int` (který v rozhraní .NET je definován jako vždy `System.Int32`) na nový datový typ: `System.nint`.  Si můžete představit "n" jako význam "nativní", takže nativní celé číslo, zadejte na platformě.

Představujeme `nint`, `nuint` a `nfloat` také poskytuje datové typy vytvořená na základě jejich potřeby.

Další informace o těchto změnách typu dat, najdete v článku [nativní typy](~/cross-platform/macios/nativetypes.md) dokumentu.

#<a name="how-to-detect-the-architecture-of-ios-apps"></a>K zjištění architektura aplikací pro iOS

Můžou nastat situace, kdy aplikace musí vědět, pokud je spuštěn na 32bitová nebo 64bitová verze systému iOS. Následující kód slouží ke kontrole architekturu:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```


<a name="deprecated-apis" />

#<a name="arrays-and-systemcollectionsgeneric"></a>Pole a System.Collections.Generic –

C# indexery očekává typ `int`, budete muset explicitně přetypovat `nint` hodnoty k `int` pro přístup k elementů v kolekci nebo pole. Příklad:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Toto chování je očekávané, protože přetypování z `int` k `nint` je Hotovo míru ztrát na 64bitové implicitní převod není.

# <a name="converting-datetime-to-nsdate"></a>Převod datového typu DateTime NSDate

Když používáte rozhraní API Unified implicitní převod `DateTime` k `NSDate` hodnoty už probíhá. Tyto hodnoty budou muset explicitně převést z jednoho typu do jiného. Následující metody rozšíření můžete použít k automatizaci tohoto procesu:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

<a name="deprecated-typos" />

# <a name="deprecated-apis-and-typos"></a>Zastaralá rozhraní API a překlepům

Uvnitř Xamarin.iOS klasické rozhraní API (monotouch.dll) `[Obsolete]` byl použit atribut dvěma různými způsoby:

-  **Zastaralá rozhraní API pro iOS:** to je, když vám přestat používat rozhraní API, protože je právě nahrazena novější pomocné parametry Apple. Klasické rozhraní API je stále dobře a často vyžaduje (pokud podporujete starší verzi systému iOS).
 Taková rozhraní API (a `[Obsolete]` atribut) jsou zahrnuty do nové sestavení Xamarin.iOS.
-  **Nesprávný API** některá API měl překlepům v jejich názvy.


Pro původní sestavení (monotouch.dll a XamMac.dll) jsme zachován původní kód, který je k dispozici pro kompatibilitu ale byly odebrány ze sestavení unifikované API (Xamarin.iOS.dll a Xamarin.Mac)

<a name="NSObject_ctor" />

# <a name="nsobject-subclasses-ctorintptr"></a>Podtřídy .ctor(IntPtr) NSObject

Každý `NSObject` podtřídami má konstruktor, který přijímá `IntPtr`. Toto je, jak jsme můžete vytvořit nové spravované instanci z nativní ObjC popisovač.

V klasickém to byla `public` konstruktor. Ale byl snadno zneužít tuto funkci v uživatelském kódu, například vytváření několik spravované instance pro jednu instanci ObjC *nebo* vytvoření spravované instance, která by chybí očekávaný stav spravovaných (pro podtřídy).

Aby se zabránilo těchto druh problémy `IntPtr` jsou konstruktory `protected` v **jednotná** rozhraní API, který se má použít pouze pro vytváření podtříd. To zajistí, že opravte nebo safe rozhraní API slouží k vytvoření spravované instance z obslužné rutiny, tj.

    var label = Runtime.GetNSObject<UILabel> (handle);

Toto rozhraní API vrátí existující spravované instance (Pokud již existuje) nebo vytvoří novou (Pokud je vyžadováno). Již je k dispozici v classic i jednotné rozhraní API.

Všimněte si, že `.ctor(NSObjectFlag)` je teď zároveň `protected` , ale tato byla málo používané mimo vytváření podtříd.

<a name="NSAction" />

# <a name="nsaction-replaced-with-action"></a>NSAction nahradí akce

Pomocí rozhraní API Unified `NSAction` byla odebrána považuje standardní .NET `Action`. Toto je velký zlepšování, protože `Action` je běžný typ formátu .NET, zatímco `NSAction` specifická pro Xamarin.iOS. Obě přesně stejnou věc udělat, ale byly odlišné a nekompatibilní typy a další kód museli být zapsána do dosažení stejného výsledku.

Například pokud aplikace pro Xamarin zahrnuty následující kód:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
Můžete ho teď nahrazený nástrojem jednoduché lambda:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Dříve, bude k chybě kompilátoru protože `Action` nemůže být přiřazen `NSAction`, ale protože `UITapGestureRecognizer` nyní trvá `Action` místo `NSAction` je platná v jednotné rozhraní API.

# <a name="custom-delegates-replaced-with-actiont"></a>Vlastní delegáti nahradí akce<T>

V **jednotná** některé jednoduché (např. jeden parametr) delegáty rozhraní .net byla nahrazena `Action<T>`. Například

    public delegate void NSNotificationHandler (NSNotification notification);

je teď možné použít jako `Action<NSNotification>`. Tento kód povýšit opakovaně používat a snížit zdvojení kódu uvnitř Xamarin.iOS a vaše vlastní aplikace.


# <a name="taskbool-replaced-with-taskbooleannserror"></a>Úloha<bool> nahradí úloh < logická hodnota, NSError >>

V **classic** nebyly některé asynchronní rozhraní API vrací `Task<bool>`. Ale některé z nich kde se má použít při `NSError` byl součástí podpisu, tj. `bool` již byla `true` a jste měli k zachycení výjimek získat `NSError`.

Vzhledem k tomu, že některé chyby jsou velmi běžné a návratovou hodnotu nebyl užitečné tohoto vzoru se změnilo v **jednotná** vrátit `Task<Tuple<Boolean,NSError>>`. To umožňuje zkontrolujte úspěch a všechny chyby, které může dojít během asynchronního volání.

# <a name="nsstring-vs-string"></a>Řetězec NSString vs

V určitých případech musel změnit z některé konstanty `string` k `NSString`, například `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

Obecně jsme raději .NET `System.String` typu. Však navzdory pokynů společnosti Apple, jsou některé nativní rozhraní API porovnání ukazatelů konstantní (není řetězec sám sebe), a to fungovat jenom při zveřejňujeme konstanty jako `NSString`.


 <a name="protocols" />

# <a name="objective-c-protocols"></a>Protokoly jazyka Objective-C

Původní MonoTouch neměl plnou podporu pro ObjC protokoly a některé,-optimální, byly přidány rozhraní API pro podporu nejběžnější scénáře. Toto omezení není už existuje, ale z důvodu zpětné kompatibility několik rozhraní API udržovaly kolem uvnitř `monotouch.dll` a `XamMac.dll`.

Tato omezení byly odebrány a vyčistit na jednotné rozhraní API. Většinu změn bude vypadat takto:

**Classic**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

`I` Předpony znamená **jednotná** vystavit rozhraní, místo konkrétního typu ObjC protokolu. To bude usnadňují případech, kde nechcete k podtřídami konkrétní typ, který poskytuje Xamarin.iOS.

Povolená také některé rozhraní API jako přesnější a snadno použitelný, například:

**Classic**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

Taková rozhraní API je nyní jednodušší do us, bez refering dokumentaci a doplňování kódu vaší IDE vám poskytne užitečnější návrhy založené na protokolu nebo rozhraní.

## <a name="nscoding-protocol"></a>Protokol NSCoding

Naše vazba původní zahrnuté .ctor(NSCoder) pro každý typ - i v případě, že nepodporuje `NSCoding` protokolu.  Jediný `Encode(NSCoder)` metoda nacházel v `NSObject` ke kódování objektu.
Ale tato metoda by fungovat pouze v případě instance potvrzeny NSCoding protokolu.

Rozhraní API služby Unified vyřešili jsme to.  Nové sestavení budou mít pouze `.ctor(NSCoder)` Pokud typ odpovídá `NSCoding`. Také tyto typy teď mají `Encode(NSCoder)` metodu, která odpovídá `INSCoding` rozhraní.

Nízký dopad: Ve většině případů tuto změnu nebude mít vliv na aplikace jako starý, odebrané konstruktory nelze použít.


## <a name="further-tips"></a>Další tipy

Další změny mít na paměti, které jsou uvedeny v [tipy pro aktualizaci aplikace na unifikované API](~/cross-platform/macios/unified/updating-tips.md).

# <a name="sample-code"></a>Ukázkový kód

Od července 31 jsme publikovali porty iOS vzorků pro toto nové rozhraní API, které se na `magic-types` větev v [monotouch-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Pro počítače Mac, probíhá kontrola ukázky v obou [mac-samples](https://github.com/xamarin/mac-samples) úložišti (zobrazující nových rozhraní API v Mavericks nebo Yosemite) a také 32 nebo 64bitový ukázky ve větvi magic typy [mac-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).
-->


## <a name="related-links"></a>Související odkazy

- [Aktualizace aplikací iOS](updating-ios-apps.md)
- [Aktualizace aplikací pro Mac](updating-mac-apps.md)
- [Aktualizace aplikace Xamarin.Forms](updating-xamarin-forms-apps.md)
- [Aktualizace vazby](update-binding.md)
- [Aktualizace tipy](updating-tips.md)
- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
