---
title: Funkce položku Bean želé
description: 'Tento dokument poskytne podrobný přehled nových funkcí pro vývojáře, které byly zavedeny v systému Android 4.1. Tyto funkce patří: rozšířeného oznámení, aktualizace Android světla sdílet velké soubory, aktualizace zjišťování sítě multimédia, peer-to-peer, animací, nová oprávnění.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d8068ccfc8d0f159a88704370261ec5f20d8b7c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771548"
---
# <a name="jelly-bean-features"></a>Funkce položku Bean želé

_Tento dokument poskytne podrobný přehled nových funkcí pro vývojáře, které byly zavedeny v systému Android 4.1. Tyto funkce patří: rozšířeného oznámení, aktualizace Android světla sdílet velké soubory, aktualizace zjišťování sítě multimédia, peer-to-peer, animací, nová oprávnění._



## <a name="overview"></a>Přehled

Android 4.1 (rozhraní API Level 16), také známé jako "želé položku Bean", byla verze 9. července 2012. Tento článek poskytne nejvyšší úrovni Úvod do některé z nových funkcí v systému Android 4.1 pro vývojáře, kteří používají Xamarin.Android. Některé tyto nové funkce, zavedená jsou vylepšené tak, aby animací pro spuštění aktivity, nové zvuky pro fotoaparátu a vylepšenou podporou pro navigaci v zásobníku aplikace. Nyní je možné vyjmout a vložit pomocí tříd Intent.

Zlepšení stability aplikace pro Android umožňují izolovat závislost na nestabilním poskytovatelů obsahu. Služby mohou být také izolované tak, že jsou přístupné pouze aktivitou, která je spuštěna.

Byla přidána podpora pro pomocí zjišťování sítě služby Bonjour, technologii UPnP nebo vícesměrového vysílání DNS založené na služby. Je nyní možné pro širší oznámení, které mají formátovaný text tlačítka akce a velkých obrázků.

Nakonec bylo přidáno několik nová oprávnění v Android 4.1.



## <a name="requirements"></a>Požadavky

K vývoji aplikací Xamarin.Android pomocí položku Bean želé vyžaduje Xamarin.Android 4.2.6 nebo vyšší a Android 4.1 (rozhraní API Level 16) být nainstalovány prostřednictvím Android SDK Manager, jak je znázorněno na následujícím snímku obrazovky:

[![Výběr Android 4.1 ve správci sady SDK pro Android](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>Co je nového



### <a name="animations"></a>Animace

Aktivity může být spuštěn pomocí přiblížení animací nebo vlastní animace pomocí `ActivityOptions` třídy. K dispozici jsou následující nové metody pro podporu těchto animací:

-   `MakeScaleUpAnimation` – Tím se vytvoří animace, která škálování okno s aktivitu, od počáteční pozice a velikosti na obrazovce.
-   `MakeThumbnailScaleUpAnimation` – Animace, která škálování tím se vytvoří z miniaturu z konkrétní pozici na obrazovce.
-   `MakeCustomAnimation` – Animace tím se vytvoří z prostředků v aplikaci. Existuje jedna animace pro když aktivita otevře a jiná pro při zastavení aktivity.


Nové `TimeAnimator` třída poskytuje rozhraní `TimeAnimator.ITimeListener` který upozornění aplikace pokaždé, když rámeček změn v animace. Zvažte například následující implementace `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

A teď použití třídy, instance `TimeAnimator` je vytvořen, a je nastavená naslouchací proces:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

Jako `TimeAnimator` je spuštěna instance, bude vyvolání `ITimeAnimator.ITimeListener`, který se potom se přihlaste jak dlouho animator byl spuštěný a jak dlouho se jako byla od posledního metodu metoda byla volána.



### <a name="application-stack-navigation"></a>Navigační zásobník aplikace

Android 4.1 zlepšuje na straně zásobníku aplikace, která byla zavedena v systému Android 3.0. Zadáním `ParentName` vlastnost `ActivityAttribute`, Android můžete otevřít správné nadřazené aktivity, když uživatel stiskne [tlačítko](http://developer.android.com/design/patterns/navigation.html#up-vs-back) na panelu akcí - Android vytvoří instanci aktivity určeného `ParentName`vlastnost. To umožňuje aplikacím zachování hierarchie aktivit, které danou úlohu.

Pro většinu aplikací nastavení `ParentName` aktivita je dost informací pro Android zajistit správné chování pro procházení zásobníku aplikace; Android bude syntetizace nezbytné back zásobníku vytvořením řadu záměry pro každé nadřazené aktivity. Protože se jedná zásobník umělé aplikací, nebude mít každá aktivita syntetické však uloženém stavu, která by měla mít přirozené aktivity. Poskytnutí uloženého stavu na syntetické nadřazené aktivity, může přepsat aktivitu `OnPrepareNavigationUpTaskStack` metoda. Tato metoda přijímá `TaskStackBuilder` instanci, která bude obsahovat soubor záměru objekty, Android použijete k vytvoření back zásobníku. Aktivita může upravit tyto záměry tak, aby při vytváření syntetické aktivity, obdrží informace o správném stavu.

Pro složitější scénáře existují nové metody pro třídu aktivity, který se dá použít ke zpracování chování až navigační a vytvořit back zásobníku:

-   `OnNavigateUp` – Přepsáním této metody je možné provést vlastní akci při <span class="ui">až</span> stisknutí tlačítka.
-   `NavigateUpTo` – Tuto metodu zavoláte způsobí, že aplikace přejděte z aktuální aktivitu do aktivity určeného dané záměr.
-   `ParentActivityIntent` – Slouží k získání záměrem, který se spustí Nadřazená aktivita aktuální aktivity.
-   `ShouldUpRecreateTask` – Tato metoda se používá k dotazování, pokud musí být vytvořený syntetické back zásobníku přejděte do nadřazené aktivity. Vrátí `true` Pokud syntetické zásobníku musí být vytvořen. 
-   `FinishAffinity` – Voláním této metody se dokončí aktuální aktivitu a všechny aktivity pod ním v aktuální úloze které mají stejné spřažení úloh.
-   `OnCreateNavigateUpTaskStack` – Tato metoda je potlačena, pokud je potřeba mít plnou kontrolu nad vytváření syntetické zásobníku.




### <a name="camera"></a>Fotoaparát

Je nové rozhraní `Camera.IAutoFocusMoveCallback`, které lze zjistit, kdy má fokus automatické spouštění nebo zastavování, přesunutí. Příklad toto nové rozhraní můžete vidět v následující fragment kódu:

```csharp
public class AutoFocusCallbackActivity : Activity, Camera.IAutoFocusCallback
{
    public void OnAutoFocus(bool success, Camera camera)
    {
        // camera is an instance of the camera service object.

        if (success)
        {
            // Auto focus was successful - do something here.
        }
        else
        {
            // Auto focus didn't happen for some reason - react to that here.
        }
    }
}
```

Nová třída `MediaActionSound` poskytuje sadu rozhraní API pro vytvoření zvuků pro různé akce média. Existuje několik akcí, které mohou nastat u fotoaparátu, tyto jsou definované za je výčet `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` – Tento zvuk, který se přehraje při zaměřením byla dokončena.
-   `MediaActionSoundType.ShutterClick` – Tento zvuk přehrávané při pořízení image snímek.
-   `MediaActionSoundType.StartVideoRecording` – Tento zvuk, který se používá označení začátku záznam videa.
-   `MediaActionSoundType.StopVideoRecording` – Tento zvuk přehrávané k označení konce záznam videa.


Příklad použití `MediaActionSound` třída najdete v následující fragment kódu:

```csharp
var mediaActionPlayer = new MediaActionSound();

// Preload the sound for a shutter click.
mediaActionPlayer.Load(MediaActionSoundType.ShutterClick);
var button = FindViewById<Button>(Resource.Id.MyButton);

// Play the sound on a button click.
button.Click += (sender, args) => mediaActionPlayer.Play(MediaActionSoundType.ShutterClick);

// This releases the preloaded resources. Don’t make any calls on
// mediaActionPlayer after this.
mediaActionPlayer.Release();
```



### <a name="connectivity"></a>Připojení k



#### <a name="android-beam"></a>Android Beam

Android světla je NFC, na základě technologie, která umožňuje dvě zařízení se systémem Android ke komunikaci mezi sebou. Android 4.1 poskytuje lepší podporu pro přenos velkých souborů. Při použití nové metody `NfcAdapter.SetBeamPushUris()` Android dojde k přepnutí mezi mechanismy alternativní přenosu (například Bluetooth) na dosáhnout rychlost rychlého přenosu.



#### <a name="network-services-discovery"></a>Zjišťování sítě služby

Android 4.1 obsahuje nové rozhraní API pro zjišťování služby vícesměrového vysílání na základě DNS.
Umožňuje aplikaci ke zjišťování a připojení přes Wi-Fi s jinými zařízeními, jako jsou tiskárny, kamery a média zařízení. Tyto nové rozhraní API se `Android.Net.Nsd` balíčku.

Chcete-li vytvořit službu, která může být spotřebované jinými službami `NsdServiceInfo` třída se používá k vytvoření objektu, který definuje vlastnosti služby. Tento objekt pak zajišťuje `NsdManager.RegisterService()` společně s implementace `NsdManager.ResolveListener`. Implementace `NsdManager.ResolveListener` se používají pro upozornění na úspěšné registrace a zrušení registrace služby.

Ke zjišťování služby na síť a provádění `Nsd.DiscoveryListener` předaný `NsdManager.discoverServices()`.



#### <a name="network-usage"></a>Využití sítě

Nová metoda `ConnectivityManager.IsActiveNetworkMetered` umožňuje zařízení zkontrolujte, zda je připojena k měřené síti. Tato metoda slouží ke správě využití dat pomocí přesně informování uživatelů, může být nákladné poplatků za data operací.



#### <a name="wifi-direct-service-discovery"></a>Zjišťování sítě Wi-Fi Direct služby

`WifiP2pManager` Třída byla zavedena v systému Android 4.0 pro podporu *zeroconf*. Zeroconf (nulové konfigurace sítě) je sada techniky, které umožňuje zařízení (počítače, tiskárny, telefony) pro připojení k sítím automaticky, s zásahu operátory lidského sítě nebo zvláštní konfiguraci serverů.

V položku Bean želé `WifiP2pManager` můžete zjišťovat okolní zařízení pomocí buď *Bonjour* nebo *Upnp*. Bonjour je implementace zeroconf společnosti Apple. UPnP je sada síťových protokolů, které podporuje také zeroconf. Následující metody přidat do `WiFiP2pManager` pro podporu služby zjišťování sítě Wi-Fi:

-   `AddLocalService()` – Tato metoda se používá pro zjišťování podle partnerské uzly oznamujeme aplikace jako službu přes Wi-Fi.
-   `AddServiceRequest(` ) – Tato metoda je k odeslání požadavku na zjišťování služby na rozhraní. Slouží k inicializaci služby zjišťování sítě Wi-Fi.
-   `SetDnsSdResponseListeners()` – Tato metoda se používá k registraci zpětná volání a volána pro příjem odpovědí na požadavky na zjišťování z Bonjour.
-   `SetUpnpServiceResponseListener()` – Tato metoda se používá k registraci zpětná volání a volána pro příjem odpovědí na požadavky na zjišťování Upnp.




### <a name="content-providers"></a>Poskytovatelů obsahu

`ContentResolver` Třída přijal nová metoda `AcquireUnstableContentProvider`. Tato metoda umožňuje aplikaci získat poskytovatele "nestabilním" obsahu. Za normálních okolností, kdy aplikace získá poskytovateli obsahu a že poskytovateli obsahu dojde k chybě, proto bude aplikace. S toto volání metody aplikace nebude selhat, pokud dojde k chybě poskytovatele obsahu. Místo toho `Android.OS.DeadObjectionException` bude vyvolána z volání na poskytovateli obsahu k informování aplikaci, která poskytovateli obsahu se příliš rychle. Poskytovatele "nestabilním" obsahu je užitečné při interakci s poskytovatelům obsahu z jiných aplikací – je méně pravděpodobné, že buggy kód z jiné aplikace bude mít vliv jiná aplikace.



### <a name="copy-and-paste-with-intents"></a>Zkopírujte a vložte pomocí tříd Intent

`Intent` Teď může mít třídy `ClipData` objekt přidružený k pomocí `Intent.ClipData` vlastnost. Tato metoda umožňuje přenášet se záměrem doplňující data ze schránky. Instance `ClipData` může obsahovat jednu nebo více `ClipData.Item`. `ClipData.Item`pro položky z následujících typů:

-   **Text** – Toto je libovolný řetězec textu, buď HTML nebo libovolný řetězec, jehož formát podporuje integrované Android styl zahrnuje.
-  **Záměr** – všechny `Intent` objektu.
-   **Identifikátor URI** – to může být libovolný identifikátor URI, například záložku HTTP nebo identifikátor URI pro poskytovatele obsahu.




### <a name="isolated-services"></a>Izolované služby

Izolované služba je služba spuštěná pod zvláštní zpracování a nemá žádné oprávnění. Jenom komunikace se službou je při spuštění služby a vazbu k němu prostřednictvím rozhraní API služby. Je možné deklarovat službu jako izolované nastavením vlastnosti `IsolatedProcess="true"` v `ServiceAttribute` , adorns třídu služby.


### <a name="media"></a>Média

Nové `Android.Media.MediaCodec` třída poskytuje rozhraní API pro kodeky nízké úrovně média. Aplikace může dotaz na systém a zjistěte, jaké nízké úrovni kodeky jsou k dispozici v zařízení.

Nové `Android.Media.Audiofx.AudioEffect` podtřídy byly přidány pro podporu dalších zvuk předběžné zpracování v zaznamenané zvuk:

-   `Android.Media.Audiofx.AcousticEchoCanceler` – Tato třída se používá pro předběžné zpracování zvuk signál odebrání vzdálené strany z zaznamenané zvukový signál. Například odebrání echo z aplikace hlasové komunikace.
-   `Android.Media.Audiofx.AutomaticGainControl` – Tato třída se používá k normalizaci zaznamenané signál zvýšení nebo snížení vstupní signál, aby byla výstupu signál konstantní.
-   `Android.Media.Audiofx.NoiseSuppressor` – Tato třída odebere ze zaznamenané signál hluku na pozadí.


Ne všechna zařízení budou podporovat tyto důsledky. Metoda `AudioEffect.IsAvailable` určitou aplikací, pokud je na zařízení spuštěný aplikace podporovaný zvuk účinek dotyčném by měla být volána.

`MediaPlayer` Třída nyní podporuje bez mezer přehrávání s `SetNextMediaPlayer()` metoda. Tato nová metoda určuje další Media Player spustit po dokončení aktuální přehrávač jeho přehrávání.

Následující nové třídy poskytovat standardních mechanismů a uživatelského rozhraní pro výběr, kde se přehrávají média:

-   `MediaRouter` – Tato třída umožňuje aplikacím řídit směrování kanály média ze zařízení na externí mluvčí nebo jiné zařízení.
-   `MediaRouterActionProvider` a `MediaRouteButton` – tyto třídy pomáhají zajistit konzistentní uživatelského rozhraní pro výběr a přehrávání média.




### <a name="notifications"></a>Oznámení

Android 4.1 umožňuje aplikacím větší flexibilitu a řízení pomocí zobrazení oznámení. Aplikace můžete nyní zobrazit větší lepší oznámení pro uživatele. Nová metoda `NotificationBuilder.SetStyle()` umožňuje pro jednu z nové tři nový styl nastavení na oznámení:

-   `Notification.BigPictureStyle` – Toto je pomocná třída, která bude generovat oznámení, která je v nich bude mít bitovou kopii. Následující obrázek ukazuje příklad oznámení s velký obrázek:


 [![Snímek obrazovky příklad BigPictureStyle oznámení](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` – Toto je pomocná třída, která bude generovat oznámení, které budou mít více řádků textu, například e-mailu. Příkladem tento nový styl oznámení můžete zobrazit na následujícím snímku obrazovky:


 [![Snímek obrazovky příklad BigTextStyle oznámení](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` – Toto je pomocná třída, která bude generovat oznámení, které obsahují seznam řetězců, jako je například fragmenty kódu z e-mailovou zprávu, jak je vidět na tomto snímku obrazovky:


 [![Snímek obrazovky příklad Notification.InboxStyle oznámení](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Je možné přidat až dvě tlačítka akce v dolní části zprávy oznámení, když oznámení používá styl Normální nebo větší.
Příklady najdete v následující snímek obrazovky, které jsou viditelné v dolní části oznámení tlačítka akce:

 [![Příklad snímek obrazovky tlačítka akce zobrazí následující zprávu oznámení](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification` Třída přijal nový konstanty, které umožňují vývojář chcete zadat jednu z pěti úrovně priority pro oznámení. To lze nastavit u oznámení pomocí `Priority` vlastnost.



### <a name="permissions"></a>Oprávnění

Byly přidány následující nové oprávnění:

-   `READ_EXTERNAL_STORAGE` -Aplikace vyžaduje čtení pouze přístup k externím úložišti. Aktuálně všechny aplikace mají přístup pro čtení ve výchozím nastavení, ale budoucích verzích systému Android bude vyžadovat, že aplikace explicitní žádost o přístup pro čtení.
-   `READ_USER_DICTIONARY` – Umožňuje přístup pro čtení do slovníku word uživatele.
-   `READ_CALL_LOG` – Umožňuje aplikaci získat informace o příchozích a odchozích volání načtením protokol volání.
-   `WRITE_CALL_LOG` – Umožňuje aplikaci k zápisu do protokolu volání na telefon.
-   `WRITE_USER_DICTIONARY` – Umožňuje aplikaci k zápisu do slovníku word uživatele.


O změnu důležité si uvědomit, `READ_EXTERNAL_STORAGE` – aktuálně toto oprávnění je automaticky přiděleno Android. Budoucích verzích systému Android bude vyžadovat aplikace k vyžádání toto oprávnění před uděleno oprávnění.



## <a name="summary"></a>Souhrn

Tento článek se zavedl některé nové rozhraní API pro které jsou k dispozici v systému Android 4.1 (16 úroveň rozhraní API). Zvýrazněná některé změny pro animace a animace spuštění aktivity a zavedená v nové rozhraní API pro zjišťování sítě jiných zařízení, pomocí protokolů, jako je například Bonjour nebo UPnP. Jiné změny rozhraní API byly zvýrazněná i, jako je například možnost vyjmout a vložit data pomocí tříd Intent, možnost používat izolované služby nebo "nestabilním" poskytovatelů obsahu.

Tento článek pak se aktualizace představit oznámení a popsané některé nová oprávnění, která byla zavedená s Android 4.1


## <a name="related-links"></a>Související odkazy

- [Příklad animace čas (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 rozhraní API](http://developer.android.com/about/versions/android-4.1.html)
- [Úlohy a zpět zásobníky](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Navigace pomocí zpět a až](http://developer.android.com/design/patterns/navigation.html)
