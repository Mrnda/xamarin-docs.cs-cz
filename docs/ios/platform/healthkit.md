---
title: HealthKit
description: HealthKit je zavedená v iOS 8, která poskytuje centralizovaný, koordinované a zabezpečené úložiště pro informace týkající se stavu rozhraní. Operační systém zajišťuje ochranu osobních údajů a zabezpečení s informacemi o stavu a stavu aplikace, řídicí panel pro uživatele. Aplikace s oprávněními uživatele lze číst a zapisovat širokou škálu informací o stavu.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a569bcff3ac33d008788bb0b946bd027fa5c0ea8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="healthkit"></a>HealthKit

_HealthKit je zavedená v iOS 8, která poskytuje centralizovaný, koordinované a zabezpečené úložiště pro informace týkající se stavu rozhraní. Operační systém zajišťuje ochranu osobních údajů a zabezpečení s informacemi o stavu a stavu aplikace, řídicí panel pro uživatele. Aplikace s oprávněními uživatele lze číst a zapisovat širokou škálu informací o stavu._

Kit stavu poskytuje zabezpečené úložiště pro informace týkající se stavu uživatele. Stav Kit aplikace může s výslovná oprávnění uživatele, čtení a zápis do tohoto úložiště dat a přijímat upozornění, když je přidána příslušná data. Aplikace s sebou může nést data nebo uživatele můžete použít zadaný stavu aplikace společnosti Apple k zobrazení řídicího panelu všechna svá data.

Protože data související s stavu je tak citlivé a velmi důležitý, stavu Kit je silného typu, s měrné jednotky explicitní přidružení typu informací, dále zaznamenávána (například na úrovni glukosy krve nebo vysílat rychlost). Kromě toho aplikace Kit stavu musíte použít explicitní oprávnění, musí požádat o přístup k určité typy informací a uživatel musí explicitně udělit přístup k aplikaci na tyto typy dat.

Tento článek vás seznámí:

- Požadavky na zabezpečení Kit stavu, včetně aplikací, zřizování a žádá o oprávnění uživatele pro přístup k databázi stavu Kit;
- Typ systému Kit stavu, což minimalizuje možnost nemá použití nebo chybné interpretaci závorek tvořených dat;
- Zápis do stavu Kit úložiště sdílené, celého systému.

Tento článek se nevztahuje na pokročilejším tématům, jako je například dotaz na databázi, převod mezi měrné jednotky nebo příjem oznámení nová data.

V tomto článku budeme vytvářet ukázková aplikace k zaznamenání míra vysílat uživatele:

[![](healthkit-images/image01.png "Ukázková aplikace k zaznamenání rychlost vysílat uživatelů")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>Požadavky

K dokončení kroky uvedené v tomto článku se vyžadují následující text:

- **Xcode 7 a iOS 8 (nebo vyšší)** – nejnovější Xcode společnosti Apple a iOS rozhraní API musí být nainstalovaná a nakonfigurovaná na počítači pro vývojáře.
- **Visual Studio pro Mac nebo Visual Studio** – nejnovější verze sady Visual Studio pro Mac musí být nainstalovaná a nakonfigurovaná na počítači pro vývojáře.
- **iOS 8 (nebo vyšší) zařízení** – zařízení s iOS s nejnovější verzí iOS 8 nebo novější pro testování.

> [!IMPORTANT]
> Stav Kit byla zavedena v systému iOS 8. V současné době Kit stavu není k dispozici v simulátoru iOS a ladění vyžaduje připojení na fyzickém zařízení iOS.




## <a name="creating-and-provisioning-a-health-kit-app"></a>Vytváření a zřizování aplikace Kit na stav
Předtím, než aplikace Xamarin iOS 8 můžete použít rozhraní API HealthKit, musí být správně nakonfigurované a zřízený. Tato část popisuje kroky potřebné k správně nastavení aplikace Xamarin.

Vyžadují aplikace Kit stavu:

- Explicitního **ID aplikace**.
- A **profil zřizování** přidruženou s explicitní **ID aplikace** a s **stavu Kit** oprávnění.
- `Entitlements.plist` s `com.apple.developer.healthkit` vlastnost typu `Boolean` nastavena na `Yes`.
- `Info.plist` Jejichž `UIRequiredDeviceCapabilities` klíč obsahuje položku s `String` hodnotu `healthkit`.
- `Info.plist` Musí mít také položky odpovídající ochrany osobních údajů vysvětlení: `String` vysvětlení pro klíč `NSHealthUpdateUsageDescription` Pokud aplikace bude k zápisu dat a `String` vysvětlení pro klíč `NSHealthShareUsageDescription` Pokud aplikace má číst stavu Kit data.

Chcete-li získat další informace o zřizování aplikace pro iOS, [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) článek v Xamarin na **Začínáme** řady popisuje vztah mezi vývojáře certifikáty, ID aplikace Profily zřizování a oprávnění aplikace.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>ID explicitní aplikace a profil pro zřizování

Vytvoření explicitního **ID aplikace** a odpovídající **profil zřizování** se provádí v rámci společnosti Apple [iOS Dev Center](https://developer.apple.com/devcenter/ios/index.action). 

Vaše aktuální **ID aplikace** jsou uvedeny v rámci [certifikáty, identifikátory a profily](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) části webu Dev Center. Často se zobrazí tento seznam **ID** hodnoty `*`, která udává, **ID aplikace** - **název** lze použít s libovolným počtem přípony. Takové *ID aplikace zástupné* nelze použít s Kit stavu.
 
K vytvoření explicitního **ID aplikace**, klikněte na tlačítko **+** tlačítko v pravém horním provést, abyste **registrace iOS ID aplikace** stránky:


[![](healthkit-images/image02.png "Registrace aplikace na portálu pro vývojáře Apple")](healthkit-images/image02.png#lightbox)

Jak je znázorněno na obrázku výš, po vytvoření popis aplikace, použijte **explicitní ID aplikace** části k vytvoření ID pro vaši aplikaci. V **App Services** část, zkontrolujte **stavu Kit** v **povolit služby** části.

Až skončíte, stiskněte **pokračovat** tlačítko zaregistrovat **ID aplikace** ve vašem účtu. Je přesměrován zpět zpět **identifikátory, certifikátů a profilů** stránky. Klikněte na tlačítko **profily zřizování** můžete přejít na seznam vaše aktuální zřizovacích profilů, a klikněte na **+** tlačítko v pravém horním rohu provést, abyste **přidat iOS Profil pro zřizování** stránky. Vyberte **vývoj aplikací pro iOS** možnost a klikněte na tlačítko **pokračovat** zobrazíte **vyberte ID aplikace** stránky. Zde vyberte explicitní **ID aplikace** který jste dřív zadali:


[![](healthkit-images/image03.png "Vyberte explicitní ID aplikace")](healthkit-images/image03.png#lightbox)

Klikněte na tlačítko **pokračovat** , tak i pracovní prostřednictvím zbývající obrazovky, kde zadáte vaše **vývojáře autority**, **zařízení**a **název** pro tento **profil pro zřizování**:

[![](healthkit-images/image04.png "Generování profilu pro zřizování")](healthkit-images/image04.png#lightbox)

Klikněte na tlačítko **generování** a operátoru await vytvoření vašeho profilu. Stáhněte si soubor a dvojím kliknutím na instalovat v Xcode. Můžete potvrdit, jeho instalace v části **Xcode > Předvolby > účty > Zobrazit podrobnosti...** Měli byste vidět nově nainstalovaného profilu zřizování a měl by mít na ikonu stavu Kit a všech dalších speciální služeb v jeho **oprávnění** řádek:

[![](healthkit-images/image05.png "Zobrazení profil v Xcode")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Přidružení ID aplikace a zřizování profilu s aplikace Xamarin.iOS

Jakmile jste vytvořili a nainstalovat odpovídající **profil zřizování** jak je popsáno, bude obvykle čas k vytvoření řešení v sadě Visual Studio pro Mac nebo Visual Studio. Stav Kit přístup je k dispozici žádné iOS C# nebo projektu F #.

Místo provede procesem vytvoření projektu Xamarin iOS 8 ručně, otevřete ukázkovou aplikaci připojit k tomuto článku (která zahrnuje předem scénáře a kód). Ukázkovou aplikaci přidružit vaší stavu Kit povoleno **profil zřizování**v **Pad řešení**, klikněte pravým tlačítkem na projekt a otevřete jeho **možnosti** dialogové okno. Přepnout **iOS aplikace** panelu a zadejte explicitní **ID aplikace** jste předtím vytvořili jako aplikace **identifikátor svazku**:

[![](healthkit-images/image06.png "Zadejte explicitní ID aplikace")](healthkit-images/image06.png#lightbox)

Nyní přepnout **iOS podepisování sady** panelu. Vaše-nainstalován v nedávné době **profil zřizování**, s jeho přidružení k explicitní **ID aplikace**, bude nyní k dispozici jako **profil zřizování**:

[![](healthkit-images/image07.png "Vyberte profil zřizování")](healthkit-images/image07.png#lightbox)

Pokud **profil zřizování** není k dispozici, zkontrolujte **identifikátor svazku** v **iOS aplikace** panely versus ve stanoveném **iOS Dev Center** a že **profil zřizování** je nainstalována (**Xcode > Předvolby > účty > Zobrazit podrobnosti...** ).

Když povoleno stavu Kit **profil zřizování** je vybraný, klikněte na tlačítko **OK** zavřete dialogové okno Možnosti projektu.

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist a Info.plist hodnoty

Ukázková aplikace zahrnuje `Entitlements.plist` souboru (což je potřebné pro Kit stavu v aplikacích s podporou) a nejsou zahrnuty v každé šabloně projektu. Pokud váš projekt nezahrnuje oprávnění, klikněte pravým tlačítkem na projekt, vyberte **soubor > Nový soubor... > iOS > Entitlements.plist** jeden přidat ručně.

Nakonec vaše `Entitlements.plist` musí mít následující pár klíč a hodnotu:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

Podobně `Info.plist` pro aplikace musí mít hodnotu `healthkit` přidružené `UIRequiredDeviceCapabilities` klíč:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

Ukázková aplikace součástí tento článek obsahuje předkonfigurované `Entitlements.plist` zahrnující všechny požadované klíče.

<a name="programming" />

## <a name="programming-health-kit"></a>Programování stavu Kit

Úložiště stavu Kit je privátní, uživatelská úložiště dat, která je sdílena mezi aplikací. Vzhledem k tomu, že je proto citlivé informace o stavu, musí uživatel provést kladné kroky povolí přístup k datům. Tento přístup může být částečné (zápisu, ale není pro čtení, přístup pro některé typy dat, ale jiné ne, atd.) a může kdykoliv odvolat. Stav Kit aplikace by měly být zapsány obranu, s tím, že mnoho uživatelů bude odhodlání o ukládání jejich informace týkající se stavu.

Data o stavu Kit je omezený na Apple zadané typy. Tyto typy jsou definovány výhradně: jiné, jako je například typ krve jsou omezené na konkrétní hodnoty výčtu Apple zadaný, zatímco jiné kombinovat rozsahem s jednotkou míry (například gram, kaloriích a litry). I data, která sdílejí kompatibilní jednotku míry jsou rozlišené jejich `HKObjectType`; pro instanci systém typů zachytí chybné pokusu o uložení `HKQuantityTypeIdentifier.NumberOfTimesFallen` hodnotu pole očekává `HKQuantityTypeIdentifier.FlightsClimbed` i když obě použít` HKUnit.Count` měrné jednotky.

Typy uložitelného v úložišti dat stavu Kit jsou všechny měly podtřídy `HKObjectType`. `HKCharacteristicType` objekty ukládat biologické pohlaví, typ krve a datum narození. Další běžné jsou ale `HKSampleType` objekty, které představují data, která je vzorkovat v určitém čase nebo po určitou dobu. 

[![](healthkit-images/image08.png "Graf objektů HKSampleType")](healthkit-images/image08.png#lightbox)

`HKSampleType` je abstraktní a má čtyři konkrétní podtřídy. Aktuálně je pouze jeden typ `HKCategoryType` data, která se analýza režimu spánku. Velká většina dat ve stavu Kit je typu `HKQuantityType` a ukládají data v `HKQuantitySample` objekty, které jsou vytvořeny pomocí známých vzoru návrhu Factory:

[![](healthkit-images/image09.png "Velká většina dat v Kit stavu typu HKQuantityType a ukládají data v HKQuantitySample objekty")](healthkit-images/image09.png#lightbox)

`HKQuantityType` typy v rozsahu od `HKQuantityTypeIdentifier.ActiveEnergyBurned` k `HKQuantityTypeIdentifier.StepCount`. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>Od uživatele žádá o oprávnění

Koncoví uživatelé musí provést kladné postup, který umožňuje aplikaci číst nebo zapisovat data stavu Kit. To se provádí prostřednictvím stavu aplikace, která se dodává předem nainstalovaná na zařízeních s iOS 8. Při prvním spuštění Kit stavu aplikace, zobrazí se uživateli se systém řídí **stavu přístupu** dialogové okno:

[![](healthkit-images/image10.png "Zobrazí se uživateli se systém řídí dialogové okno Stav přístupu")](healthkit-images/image10.png#lightbox)

Později, uživatel může změnit oprávnění pomocí aplikace stavu **zdroje** dialogové okno:

[![](healthkit-images/image11.png "Uživatel může změnit oprávnění pomocí dialogu zdroje stavu aplikace")](healthkit-images/image11.png#lightbox)

Vzhledem k tomu, že je velmi citlivé informace o stavu, vývojáři aplikace by měla zápis jejich programy obranu, s tím, že oprávnění odmítl se změnila. Přestože aplikace běží. Nejběžnější stylu se s žádostí o oprávnění v `UIApplicationDelegate.OnActivated` metoda a uživatelském rozhraní podle potřeby upravit.

### <a name="permissions-walkthrough"></a>Návod oprávnění

Otevřete v projektu zřízený stavu Kit `AppDelegate.cs` souboru. Všimněte si pomocí příkazu `HealthKit`; v horní části souboru.


Následující kód má vztah k stavu sady oprávnění:

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

Všechny kód tyto metody lze provést vložené `OnActivated`, ale ukázková aplikace používá samostatné metody jejich záměr jasnější: `ValidateAuthorization()` obsahuje kroky potřebné k požádat o přístup na určité typy zapisovaný (a pro čtení, v případě potřeby aplikace) a `ReactToHealthCarePermissions()` je zpětné volání, které se aktivuje poté, co uživatel má zpracoval s dialogové okno oprávnění ve Health.app.

Úkolem `ValidateAuthorization()` vytvoření sadu `HKObjectTypes` , bude zapisovat a požádat o oprávnění k aktualizaci dat aplikace. V ukázkové aplikace `HKObjectType` je pro klíč `KHQuantityTypeIdentifierKey.HeartRate`. Tento typ je přidat do sady `typesToWrite`, při sadu `typesToRead` je prázdné. Tyto sady a odkaz na `ReactToHealthCarePermissions()` zpětného volání, je předaná do `HKHealthStore.RequestAuthorizationToShare()`.

`ReactToHealthCarePermissions()` Zpětného volání bude volána poté, co uživatel má zpracoval s dialogové okno oprávnění a je předán dva údaje: `bool` hodnotu, která bude `true` Pokud má uživatel zpracoval se dialogové okno oprávnění a `NSError`, což jinou hodnotu než null, pokud naznačuje nějaký druh chyby související s zobrazení dialogu oprávnění.

> [!IMPORTANT]
> Mít o argumentů této funkci: _úspěch_ a _chyba_ parametry neoznačují, zda uživatel má uděleno oprávnění k přístupu data stavu Kit! Pouze indikují, že uživatel nebyla zadána možnost povolit přístup k datům.

Potvrďte, zda má aplikace přístup k datům, `HKHealthStore.GetAuthorizationStatus()` se používá předávání v `HKQuantityTypeIdentifierKey.HeartRate`. Na základě stavu vrátil, aplikace povolí nebo zakáže možnost zadat data. Neexistuje žádné standardní uživatelské prostředí pro práci s odepření přístupu a celá řada možných možností. V příkladu aplikaci, je stav nastaven na `HeartRateModel` objekt typu singleton, která zase, vyvolá relevantní události.

## <a name="model-view-and-controller"></a>Model, zobrazení a kontroler

Chcete-li zkontrolovat `HeartRateModel` objekt typu singleton, otevřete `HeartRateModel.cs` souboru:

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

V první části je často používaný kód pro vytvoření obecné události a obslužné rutiny. Počáteční části `HeartRateModel` třída je také často používaný pro vytvoření objektu singleton bezpečné pro přístup z více vláken.

Potom `HeartRateModel` zpřístupní 3 události: 

- `EnabledChanged` -Označuje, že má vysílat míra úložiště povolit nebo zakázat (Všimněte si, že úložiště je původně zakázáno). 
- `ErrorMessageChanged` – V této ukázkové aplikaci máme velmi jednoduchý model zpracování chyb: řetězec s poslední chyby. 
- `HeartRateStored` -Vyvolá, když na míru vysílat jsou uloženy v databázi stavu Kit.

Všimněte si, že vždy, když při vyvolání těchto událostí, se provádí prostřednictvím `NSObject.InvokeOnMainThread()`, což umožňuje odběratele, kteří mají aktualizace uživatelského rozhraní. Případně se události může být zdokumentovaný jako vyvolaných v vlákna na pozadí a odpovědnost za zajištění kompatibility může být ponecháno na jejich obslužné rutiny. Protože mnoho funkcí, jako je například požadavek oprávnění jsou asynchronní a spustit jejich zpětná volání na jiný hlavní vláken, jsou vlákno faktory důležité v aplikacích Kit stavu.

Konkrétního kódu stavu Kit v `HeartRateModel` v dvě funkce `HeartRateInBeatsPerMinute()` a `StoreHeartRate()`. 

`HeartRateInBeatsPerMinute()` Převede její argument stavu sady silného typu `HKQuantity`. Typ množství je, že určeného `HKQuantityTypeIdentifierKey.HeartRate` a jednotky množství `HKUnit.Count` dělený `HKUnit.Minute` (jinými slovy, je jednotka *Údery za minutu*). 

`StoreHeartRate()` Funkce trvá `HKQuantity` (ukázkovou aplikaci, jednu vytvořené `HeartRateInBeatsPerMinute()` ). Ověřit jeho data, použije `HKQuantity.IsCompatible()` metoda, která vrátí `true` Pokud objektu jednotky můžete převést do jednotky v argumentu. Pokud množství byl vytvořen s `HeartRateInBeatsPerMinute()` to samozřejmě vrátí `true`, ale měla by vrátit `true` Pokud množství byly vytvořeny jako je například *Údery za hodinu*. Běžně `HKQuantity.IsCompatible()` slouží k ověření velkokapacitních, vzdálenost a energie, která může vstup uživatele nebo zařízení, nebo zobrazit v jednom systému měření (například britské jednotky), ale které se pravděpodobně uloží do jiného systému (například metriky jednotky). 

Po ověření kompatibility množství `HKQuantitySample.FromType()` metoda factory slouží k vytvoření silného typu `heartRateSample` objektu. `HKSample` objekty mají počáteční a koncové datum; pro okamžitou odečty tyto hodnoty by měly být stejné jako v příkladu. Ukázka také nenastaví žádná data klíč hodnota jeho `HKMetadata` argument, ale jeden použít kód například následující kód a zadejte umístění senzor:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

Jednou `heartRateSample` byl vytvořen, kód vytvoří nové připojení k databázi s její pomocí bloku. V tomto bloku `HKHealthStore.SaveObject()` metoda o asynchronní zápis do databáze. Výsledný volání výrazu lambda aktivuje relevantní události buď `HeartRateStored` nebo `ErrorMessageChanged`.

Teď, když model naprogramování, je čas zobrazíte jak řadičem odráží stav modelu. Otevřete `HKWorkViewController.cs` souboru. Konstruktor jednoduše sváže `HeartRateModel` jednotlivého prvku obslužné metody událostí (znovu tomu lze vložením s výrazy lambda, ale samostatné metody byl záměr trochu zřejmější):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

Zde jsou relevantní obslužné rutiny:

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

Samozřejmě v aplikaci s jedním řadičem, by bylo možné Vyhněte se vytváření objektu samostatné modelu a použití události pro tok řízení, ale použití objekty modelu je vhodnější pro skutečné aplikace.

## <a name="running-the-sample-app"></a>Spuštění ukázkové aplikace

Simulátoru iOS nepodporuje Kit stavu. Ladění, je třeba provést na fyzického zařízení s iOS 8.

Vývoj pro zařízení správně zřízený iOS 8 připojte k systému. Vyberte jako cíl nasazení v sadě Visual Studio pro Mac a z nabídky zvolte **spustit > ladění**.

> [!IMPORTANT]
> V tomto okamžiku se surface chyby týkající se zřizování. Chcete-li vyřešit chyby, zkontrolujte vytváření a zřizování výše uvedené části stavu Kit aplikace. Komponenty jsou: 
>
> - **iOS Dev Center** -explicitní ID aplikace a stavu sady povolené profil zřizování. 
> - **Možnosti projektu** -identifikátor svazku (explicitní ID aplikace) a profil zřizování.
> - **Zdrojový kód** -Entitlements.plist & Info.plist

Za předpokladu, že zřizuje byly správně nastaveny, spustí se vaše aplikace. Když se dosáhne jeho `OnActivated` metoda, bude vyžadovat stavu Kit autorizace. Při prvním zjistil se v operačním systému, uživatelů zobrazí se následující dialogové okno:


[![](healthkit-images/image12.png "Zobrazí uživatele se toto dialogové okno")](healthkit-images/image12.png#lightbox)

Povolit aplikaci, kterou chcete aktualizovat data vysílat rychlost a aplikace se znovu zobrazí. `ReactToHealthCarePermissions` Zpětného volání, aktivuje se asynchronně. To způsobí, že `HeartRateModel’s` `Enabled` vlastnosti chcete změnit, která bude vyvolána `EnabledChanged` událost, která způsobí, že `HKPermissionsViewController.OnEnabledChanged()` obslužné rutiny události spuštění, které umožňuje `StoreData` tlačítko. Následující diagram znázorňuje sekvenci:


[![](healthkit-images/image13.png "Tento diagram zobrazuje posloupnost událostí")](healthkit-images/image13.png#lightbox)

Stiskněte **záznam** tlačítko. To způsobí, že `StoreData_TouchUpInside()` obslužná rutina spouštěla, který se bude snažit analyzovat hodnotu `heartRate` textové pole, převést do `HKQuantity` prostřednictvím dříve popsaných `HeartRateModel.HeartRateInBeatsPerMinute()` funkce a předejte daného množství `HeartRateModel.StoreHeartRate()`. Jak je popsáno dříve, to se pokusí o uložení dat a vyvolá buď `HeartRateStored` nebo `ErrorMessageChanged` událostí.

Dvakrát klikněte **Domů** tlačítko na vašem zařízení a otevřete aplikaci stavu. Klikněte **zdroje** se zobrazí ukázková aplikace uvedené. Zvolte jej a zakáže oprávnění k aktualizaci dat míra vysílat. Dvakrát klikněte **Domů** tlačítko a přepínače zpět do aplikace. Ještě jednou `ReactToHealthCarePermissions()` bude volána, ale tentokrát, protože byl odepřen přístup, **StoreData** tlačítko bude zakázán (Upozorňujeme, že k tomu dochází asynchronně a změny v uživatelském rozhraní může být viditelné pro koncového uživatele).

## <a name="advanced-topics"></a>Pokročilá témata

Čtení dat z Kit stavu databáze je velmi podobný zápis dat: jeden Určuje typy dat jeden se pokouší o přístup, žádosti o autorizaci, a pokud jsou udělena této autorizace, data jsou k dispozici, s automatického převodu do kompatibilní jednotek míra.

Existuje několik sofistikovanější dotazu funkcí, která umožňuje na základě predikát dotazy a dotazy, které provést aktualizace v případě, že relevantní data se aktualizují. 

Vývojáři aplikací stavu Kit by si měli projít části stavu Kit společnosti Apple [aplikace, přečtěte si pokyny](https://developer.apple.com/app-store/review/guidelines/#healthkit).

Jakmile jsou pochopeny zabezpečení a systém typů modelů, ukládání a čtení dat ve sdílené databázi stavu Kit je poměrně jednoduché. Mnoho funkcí v rámci stavu Kit fungovat asynchronně a vývojáři aplikace musí správně zápis jejich programy.

Před sepsáním tohoto článku, není aktuálně žádný ekvivalent Kit stavu v Android nebo Windows Phone.

## <a name="summary"></a>Souhrn

V tomto článku jste viděli, jak stavu Kit umožňuje aplikacím ukládat načtení, stav sdílené složky a související informace, a zároveň poskytuje standardní stavu aplikace, který umožňuje uživateli přístup a kontrolu nad tato data. 

Jsme také viděli, jak se ochrany osobních údajů, zabezpečení a integrita dat přepsání potížích pro informace týkající se stavu a aplikací pomocí sady stavu musí nakládat s nárůstem v složitost v aplikaci aspekty správy (zřizování), kódování (typ stavu Kit systém) a uživatelské prostředí (Řízení uživatelských oprávnění prostřednictvím systému dialogová okna a stavu aplikací). 

Nakonec jsme trvá podívejte se na jednoduchou implementaci Kit stavu pomocí dodávanou ukázkovou aplikaci, která zapisuje data prezenčního signálu k úložišti stavu Kit a má asynchronní deklaracemi návrhu.

## <a name="related-links"></a>Související odkazy

- [HKWork (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [Úvod do iOSu 8](~/ios/platform/introduction-to-ios8.md)
