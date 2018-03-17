---
title: "Jednotné rozhraní API – přehled"
description: "Styl nové rozhraní API je jednodušší než kdy dřív přístup ke sdílení kódu mezi Mac a iOS a také umožňuje podporu 32 až 64 bitové aplikace se stejným binární."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2f55edb27f33becca8d354f9a7bb65932b4fd924
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/17/2018
---
# <a name="unified-api-overview"></a>Jednotné rozhraní API – přehled

_Styl nové rozhraní API je jednodušší než kdy dřív přístup ke sdílení kódu mezi Mac a iOS a také umožňuje podporu 32 až 64 bitové aplikace se stejným binární._

Účelem vylepšení kód sdílení mezi Mac a iOS a umožňuje vývojářům tak, aby měl jeden základu kódu, které funguje na 32 až 64 bitů v brzké 2015 zavedli jsme nové rozhraní API v produktech Xamarin.Mac a Xamarin.iOS volat rozhraní API Unified.

> [!IMPORTANT]
> **Classic profil vyřazení:** při přidání nové platformy v Xamarin.iOS jsme spouštějí postupně přestat používat funkce z klasického profilu (monotouch.dll). Například možnost bez NRC (-ref počet nových) byla odebrána. NRC vždy byla povolená pro všechny jednotná aplikace (tj. bez NRC se nikdy možnost) a nemá žádné známé problémy. Možnost použití Boehm jako garbage collector v budoucích verzích dojde k odebrání. Je také možnost Nikdy dostupné pro jednotné aplikace. Úplné odebrání classic podpory naplánován další patří verze Xamarin.iOS 10.0.

## <a name="ios"></a>iOS

`Xamarin.iOS.dll` Sestavení součástí Xamarin.iOS 8.6 naše **první stabilní a podporované verze** jednotné rozhraní API pro iOS.
Předchozí verze preview jednotné rozhraní API jsou zavřít, ale není plně kompatibilní.

## <a name="mac"></a>Mac

`Xamarin.Mac.dll` Sestavení v stabilní kanál Xamarin.Mac naše **první stabilní a podporované verze** jednotné rozhraní API pro Mac.
Předchozí verze preview jednotné rozhraní API jsou zavřít, ale není plně kompatibilní.

## <a name="runtime-defaults"></a>Výchozí nastavení modulu runtime

Jednotné rozhraní API ve výchozím nastavení používá **SGen** systém uvolňování paměti a [počítání nový odkaz](~/ios/internals/newrefcount.md) systému pro sledování vlastnictví objektů. Tato stejná funkce je byly přesně do Xamarin.Mac.

To řeší řadu problémů, které vývojáři potýkají s původním systému a také usnadňují [Správa paměti](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Všimněte si, že je možné povolit nové Refcount i pro klasické rozhraní API, ale výchozí hodnota je konzervativní a nevyžaduje uživatelé žádné změny. S rozhraním API Unified jsme trvalo možnost změny výchozí a poskytují vývojářům všechna vylepšení ve stejnou dobu, aby Refaktorovat a znovu otestovat svůj kód.

<a name="namespace-changes" />

## <a name="library-split"></a>Knihovna rozdělení

Z tohoto bodu na rozhraní API se zobrazí dvěma způsoby:

-  **Klasické rozhraní API:** omezené na 32bitovou (pouze) a zveřejněné v `monotouch.dll` a `XamMac.dll` sestavení.
-  **Jednotné rozhraní API:** podporují 32 až 64 bit vývoj pomocí jediného rozhraní API dostupná v `Xamarin.iOS.dll` a `Xamarin.Mac.dll` sestavení.

To znamená, že pro podnikové vývojáře (ne které se budou zaměřovat obchod), můžete nadále používat existující klasické rozhraní API, protože jsme se zachovat zachování jejich navždy nebo můžete upgradovat na nových rozhraní API.

### <a name="namespace-changes"></a>Namespace změny

Pokud chcete zkrátit třecí sdílet kód mezi našich produktů Mac a iOS, měníme obory názvů pro rozhraní API v produkty.

Nemůžeme se dávka předponu "MonoTouch" z našich produktů iOS a "MonoMac" z našich produktů Mac pro datové typy.

To umožňuje jednodušší sdílet mezi platformami Mac a iOS bez nutnosti Podmíněná kompilace kódu a bude snížení šumu v horní části zdrojové soubory vašeho kódu.

-  **Klasické rozhraní API:** použijte obory názvů `MonoTouch.` nebo `MonoMac.` předponu.
-  **Jednotné rozhraní API:** žádná předpona oboru názvů

### <a name="api-changes"></a>Rozhraní API změny

Rozhraní API Unified odebere zastaralé metody a existuje několik instancí tam, kde existuje byla překlepům v názvech rozhraní API, pokud byly vázány na původní MonoTouch a MonoMac obory názvů v klasické rozhraní API. Tyto instance byla opravena v nových rozhraní API Unified a bude potřeba aktualizovat v součásti, iOS a Mac aplikace. Tady je seznam nejčastějších ty, které můžete narazit na:

|Název metody klasické rozhraní API|Název metody jednotné rozhraní API|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Úplný seznam změny přepnutím z klasického unifikované API, najdete v tématu naše [vs Classic (monotouch.dll) rozhraní API Unified (Xamarin.iOS.dll) rozdíly](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) dokumentaci.

### <a name="updating-to-unified"></a>Aktualizace na jednotné

Několik starý nebo porušený/zastaralá rozhraní API v **classic** nejsou k dispozici v **Unified** rozhraní API. Může být snazší opravit `CS0616` upozornění před zahájením vaší (ruční nebo automatické) upgradovat, protože budete mít `[Obsolete]` navést na pravém API atribut zprávy (součást upozornění).

Všimněte si, že jsme publikování [ *rozdílové* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) z klasického vs jednotné rozhraní API změny, které lze použít buď před nebo po aktualizace vašeho projektu. Stále opravě nahrazuje volání v klasickém se často být spoustu času (méně hledání dokumentaci).

Postupujte podle těchto pokynů můžete [aktualizovat stávající aplikace pro iOS](~/cross-platform/macios/unified/updating-ios-apps.md), nebo [Mac aplikace](~/cross-platform/macios/unified/updating-mac-apps.md) jednotné rozhraní API.
Zkontrolujte zbývající části této stránky a [tyto tipy](~/cross-platform/macios/unified/updating-tips.md) Další informace o migraci vašeho kódu.

## <a name="components-and-nuget"></a>Součásti a NuGet

Většina existující součásti a balíčky NuGet, budou muset být aktualizované kvůli podpoře unifikované API.
Součásti postavena klasické rozhraní API se nemůže odkazovat z projektu unifikované API.

Existující součásti, které nemá všechny odkazy na `monotouch.dll` (nebo `XamMac.dll`) by neměl vyžadovat aktualizace.

### <a name="components"></a>Součásti

iOS součásti v [úložišti součástí Xamarin](https://components.xamarin.com/) musí být aktualizovány k práci s projekty unifikované API. Xamarin pracuje aktualizovat součásti jsme publikování a podporovat jiné autorům totéž.

### <a name="nuget"></a>NuGet

Balíčky NuGet, které dřív podporované Xamarin.iOS prostřednictvím klasické rozhraní API publikovat své sestavení s využitím **Monotouch10** Přezdívka platformy.

Rozhraní API Unified zavádí nový identifikátor platformu pro kompatibilní balíčky - **Xamarin.iOS10**. Existující balíčky NuGet potřebovat při vytváření rozhraní API Unified aktualizovat přidání podpory pro tuto platformu.

> [!IMPORTANT]
> **Poznámka:** Pokud máte chybu ve formě _"Chyba 3 nesmí obsahovat 'monotouch.dll' a"Xamarin.iOS.dll"ve stejném projektu Xamarin.iOS – 'Xamarin.iOS.dll' odkazuje explicitně, zatímco 'monotouch.dll' odkazuje ' xxx, Verze = 0.0.000, Culture = neutral, PublicKeyToken = null. "_ po převedení aplikace jednotné rozhraní API, je obvykle kvůli s komponenta nebo balíček NuGet do projektu, která nebyla aktualizována jednotné rozhraní API. Budete muset odebrat existující součásti nebo NuGet, aktualizujte na verzi podporující rozhraní API Unified a provést čisté sestavení.

<a name="deprecated-apis" />

## <a name="arrays-and-systemcollectionsgeneric"></a>Pole a System.Collections.Generic –

C# indexery očekává typ `int`, budete muset explicitně přetypovat `nint` hodnoty k `int` pro přístup k elementů v kolekci nebo pole. Příklad:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Toto chování je očekávané, protože přetypování z `int` k `nint` je Hotovo míru ztrát na 64bitové implicitní převod není.

## <a name="converting-datetime-to-nsdate"></a>Převod datového typu DateTime NSDate

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

## <a name="deprecated-apis-and-typos"></a>Zastaralá rozhraní API a překlepům

Uvnitř Xamarin.iOS klasické rozhraní API (monotouch.dll) `[Obsolete]` byl použit atribut dvěma různými způsoby:

-  **Zastaralá rozhraní API pro iOS:** to je, když vám přestat používat rozhraní API, protože je právě nahrazena novější pomocné parametry Apple. Klasické rozhraní API je stále dobře a často vyžaduje (pokud podporujete starší verzi systému iOS).
 Taková rozhraní API (a `[Obsolete]` atribut) jsou zahrnuty do nové sestavení Xamarin.iOS.
-  **Nesprávný API** některá API měl překlepům v jejich názvy.

Pro původní sestavení (monotouch.dll a XamMac.dll) jsme zachován původní kód, který je k dispozici pro kompatibilitu ale byly odebrány ze sestavení unifikované API (Xamarin.iOS.dll a Xamarin.Mac)

<a name="NSObject_ctor" />

## <a name="nsobject-subclasses-ctorintptr"></a>Podtřídy .ctor(IntPtr) NSObject

Každý `NSObject` podtřídami má konstruktor, který přijímá `IntPtr`. Toto je, jak jsme můžete vytvořit nové spravované instanci z nativní ObjC popisovač.

V klasickém to byla `public` konstruktor. Ale byl snadno zneužít tuto funkci v uživatelském kódu, například vytváření několik spravované instance pro jednu instanci ObjC *nebo* vytvoření spravované instance, která by chybí očekávaný stav spravovaných (pro podtřídy).

Aby se zabránilo těchto druh problémy `IntPtr` jsou konstruktory `protected` v **jednotná** rozhraní API, který se má použít pouze pro vytváření podtříd. To zajistí, že opravte nebo safe rozhraní API slouží k vytvoření spravované instance z obslužné rutiny, tj.

    var label = Runtime.GetNSObject<UILabel> (handle);

Toto rozhraní API vrátí existující spravované instance (Pokud již existuje) nebo vytvoří novou (Pokud je vyžadováno). Již je k dispozici v classic i jednotné rozhraní API.

Všimněte si, že `.ctor(NSObjectFlag)` je teď zároveň `protected` , ale tato byla málo používané mimo vytváření podtříd.

<a name="NSAction" />

## <a name="nsaction-replaced-with-action"></a>NSAction nahradí akce

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

## <a name="custom-delegates-replaced-with-actiont"></a>Vlastní delegáti nahradí akce<T>

V **jednotná** některé jednoduché (např. jeden parametr) delegáty rozhraní .net byla nahrazena `Action<T>`. Například

    public delegate void NSNotificationHandler (NSNotification notification);

je teď možné použít jako `Action<NSNotification>`. Tento kód povýšit opakovaně používat a snížit zdvojení kódu uvnitř Xamarin.iOS a vaše vlastní aplikace.

## <a name="taskbool-replaced-with-taskbooleannserror"></a>Úloha<bool> nahradí úloh < logická hodnota, NSError >>

V **classic** nebyly některé asynchronní rozhraní API vrací `Task<bool>`. Ale některé z nich kde se má použít při `NSError` byl součástí podpisu, tj. `bool` již byla `true` a jste měli k zachycení výjimek získat `NSError`.

Vzhledem k tomu, že některé chyby jsou velmi běžné a návratovou hodnotu nebyl užitečné tohoto vzoru se změnilo v **jednotná** vrátit `Task<Tuple<Boolean,NSError>>`. To umožňuje zkontrolujte úspěch a všechny chyby, které může dojít během asynchronního volání.

## <a name="nsstring-vs-string"></a>Řetězec NSString vs

V určitých případech musel změnit z některé konstanty `string` k `NSString`, například `UITableViewCell`

**Classic**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

Obecně jsme raději .NET `System.String` typu. Však navzdory pokynů společnosti Apple, jsou některé nativní rozhraní API porovnání ukazatelů konstantní (není řetězec sám sebe), a to fungovat jenom při zveřejňujeme konstanty jako `NSString`.

 <a name="protocols" />

## <a name="objective-c-protocols"></a>Protokoly jazyka Objective-C

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

### <a name="nscoding-protocol"></a>Protokol NSCoding

Naše vazba původní zahrnuté .ctor(NSCoder) pro každý typ - i v případě, že nepodporuje `NSCoding` protokolu.  Jediný `Encode(NSCoder)` metoda nacházel v `NSObject` ke kódování objektu.
Ale tato metoda by fungovat pouze v případě instance potvrzeny NSCoding protokolu.

Rozhraní API služby Unified vyřešili jsme to.  Nové sestavení budou mít pouze `.ctor(NSCoder)` Pokud typ odpovídá `NSCoding`. Také tyto typy teď mají `Encode(NSCoder)` metodu, která odpovídá `INSCoding` rozhraní.

Nízký dopad: Ve většině případů tuto změnu nebude mít vliv na aplikace jako starý, odebrané konstruktory nelze použít.

## <a name="further-tips"></a>Další tipy

Další změny mít na paměti, které jsou uvedeny v [tipy pro aktualizaci aplikace na unifikované API](~/cross-platform/macios/unified/updating-tips.md).

## <a name="sample-code"></a>Ukázkový kód

Od července 31 jsme publikovali porty iOS vzorků pro toto nové rozhraní API, které se na `magic-types` větev v [monotouch-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

Pro počítače Mac, probíhá kontrola ukázky v obou [mac-samples](https://github.com/xamarin/mac-samples) úložišti (zobrazující nových rozhraní API v Mavericks nebo Yosemite) a také 32 nebo 64bitový ukázky ve větvi magic typy [mac-samples](https://github.com/xamarin/monotouch-samples/commits/magic-types).

## <a name="related-links"></a>Související odkazy

- [Aktualizace aplikací iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Aktualizace aplikací pro Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Aktualizace aplikace Xamarin.Forms](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [Aktualizace vazby](~/cross-platform/macios/unified/update-binding.md)
- [Práce s nativní typy v multiplatformních aplikacích](~/cross-platform/macios/native-types-cross-platform.md)
- [Aktualizace tipy](~/cross-platform/macios/unified/updating-tips.md)
- [Classic vs rozdíly unifikované API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
