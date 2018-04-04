---
title: CallKit
description: Tento článek se zabývá nového rozhraní API CallKit této Apple vydala v iOS 10 a postupy implementace v aplikacích Xamarin.iOS VOIP.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 67c761aa6656b571f16632dd1a076ff11737a424
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="callkit"></a>CallKit

_Tento článek se zabývá nového rozhraní API CallKit této Apple vydala v iOS 10 a postupy implementace v aplikacích Xamarin.iOS VOIP._


Nové rozhraní API CallKit v iOS 10 poskytuje způsob, jak aplikace VOIP integrovat iPhone uživatelského rozhraní a poskytují obeznámeni rozhraní a prostředí pro koncového uživatele. S tímto rozhraním API uživatelé zobrazení a interakci s VOIP volání z zamykací obrazovce zařízení s iOS a spravovat kontakty pomocí aplikace Phone **Oblíbené** a **nedávno** zobrazení.

## <a name="about-callkit"></a>O CallKit

Podle společnosti Apple je CallKit nové rozhraní, které bude aplikace Voice Over IP (VOIP) 3. stran straně prostředí v systému iOS 10 1. zvýšení oprávnění. Rozhraní API CallKit umožňuje aplikacím VOIP integrovat iPhone uživatelského rozhraní a poskytují obeznámeni rozhraní a prostředí pro koncového uživatele. Stejně jako integrovaná telefonní aplikace, může uživatel zobrazení a interakci s VOIP volání z zamykací obrazovce zařízení s iOS a spravovat kontakty pomocí aplikace Phone **Oblíbené** a **nedávno** zobrazení.

Rozhraní API CallKit navíc poskytuje možnost vytvářet přípony aplikace, která se dá přidružit telefonní číslo s názvem (ID volajícího) nebo zjistit, že v systému, pokud by měla být číslo blokované (volání blokování).

### <a name="the-existing-voip-app-experience"></a>Existující VOIP aplikační prostředí

Před hovoříte o nové rozhraní API CallKit a jeho schopnosti, proveďte podívejte se na aktuální činnost koncového uživatele s 3. stran VOIP aplikace v iOS 9 (a nižší úrovně) pomocí fiktivní VOIP aplikace volá MonkeyCall. MonkeyCall je jednoduchou aplikaci, která umožňuje uživatelům odesílat a přijímat volání VOIP pomocí stávající iOS rozhraní API.

Pokud uživatel je přijetí příchozího hovoru na MonkeyCall a jejich iPhone uzamčeno, oznámení na zamykací obrazovce se v současné době lišit od jiný typ oznámení (jako jsou ty ze zprávy nebo poštovní aplikace například).

Pokud uživatel chtěli zodpovědět volání, by mají Vysuňte MonkeyCall oznámení otevřete aplikaci a zadejte své heslo (nebo Touch ID uživatele) k odemčení telefonu předtím, než může přijmout volání a spustit konverzace.

Prostředí je stejně náročná, pokud je odemčený telefonu. Příchozí volání MonkeyCall se znovu zobrazí jako standardní oznámení informační zpráva, která snímky v z horní části obrazovky. Vzhledem k tomu, že se oznámení o dočasné, ho můžete být snadno vynechat uživatel musel otevřete Centrum oznámení a najít konkrétní oznámení. odpovězte pak volání nebo najít a spustit aplikaci MonkeyCall ručně.

### <a name="the-callkit-voip-app-experience"></a>CallKit VOIP aplikační prostředí

Implementací nových rozhraní API CallKit v aplikaci MonkeyCall je možné činnost koncového uživatele s příchozího hovoru VOIP výrazně zlepšit v iOS 10. Trvat příklad uživatele přijímání VOIP volání, když je uzamčené svůj Telefon z výše. Implementací CallKit volání se zobrazí na zamykací obrazovce pro iPhone, stejně, jako kdyby bylo volání přijato integrovaná telefonní aplikace, s celou obrazovku, nativní uživatelského rozhraní a standardní funkce prstem odpovědí.

Znovu Pokud pro iPhone odemknout při přijetí hovoru MonkeyCall VOIP, stejné přes celou obrazovku, nativní uživatelského rozhraní a standardní prstem odpovědí a klepněte na odmítnout funkce integrované telefonní aplikace se zobrazí a MonkeyCall má možnost přehrávání vlastní vyzváněcího .

CallKit poskytuje další funkce MonkeyCall, umožní její VOIP volá komunikovat s jinými typy volání, než se objeví v vestavěné nedávno a Oblíbené položky seznamů, chcete-li použít integrované funkce nebudou narušovat a bloku, spusťte MonkeyCall volání z Siri a nabízí možnosti pro uživatele k přiřazení volání MonkeyCall lidem v aplikaci kontakty.

V následujících částech se vztahují na architektuře CallKit, příchozí a odchozí volání toky a rozhraní API CallKit podrobně.


## <a name="the-callkit-architecture"></a>Architektura CallKit

V iOS 10 přijal Apple CallKit ve všech služeb System tak, že jsou volání provedená na CarPlay, například známé uživatelského rozhraní systému prostřednictvím CallKit. V příkladu dále uvedených vzhledem k tomu, že MonkeyCall přijme CallKit ji není systému znám stejným způsobem jako těchto předdefinovaných systémových služeb a získá všechny stejné funkce:

[![](callkit-images/callkit01.png "Služba CallKit zásobníku")](callkit-images/callkit01.png#lightbox)

Prohlédněte si blíže MonkeyCall aplikace z diagramu výše. Aplikace obsahuje všechny jeho kódu ke komunikaci s vlastní síť a obsahuje vlastní uživatelská rozhraní. Odkazuje v CallKit ke komunikaci s systému:

[![](callkit-images/callkit02.png "Architektura MonkeyCall aplikace")](callkit-images/callkit02.png#lightbox)

Existují dvě hlavní rozhraní v CallKit, který používá aplikaci:

- `CXProvider` -Umožňuje aplikaci MonkeyCall informovat systém žádné out-of-band oznámení, které můžou nastat.
- `CXCallController` – Umožňuje aplikaci MonkeyCall informovat systém akcí místní uživatele.

### <a name="the-cxprovider"></a>CXProvider

Jak jsme uvedli výše, `CXProvider` umožňuje aplikaci k informování systému žádné out-of-band oznámení, které můžou nastat. Toto jsou oznámení, že nedojde k kvůli akce místního uživatele, ale nastat v důsledku externí událostmi, jako je například příchozí volání.

Aplikace by měl používat `CXProvider` pro následující:

- Sestavy příchozí volání do systému.
- Sestavy, který odchozí volání je připojená k systému.
- Sestavy ukončení volání v systému vzdáleného uživatele.

Pokud aplikace chce, aby se pro komunikaci se systém, použije `CXCallUpdate` třídy a když se systém potřebuje ke komunikaci s aplikací, použije `CXAction` třídy:

[![](callkit-images/callkit03.png "Komunikuje s systému prostřednictvím CXProvider")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` Umožňuje aplikacím informovat systém místního uživatele akcí, například spouštění VOIP volání uživatele. Implementací `CXCallController` aplikace získá do vztahu s jinými typy volání v systému. Pokud je již aktivní telefonní volání v průběhu, například `CXCallController` můžete povolit aplikaci VOIP pozdržení toto volání a spustit nebo příjem VOIP volání.

Aplikace by měl používat `CXCallController` pro následující:

- Sestava, když uživatel bylo zahájeno odchozí volání do systému.
- Sestava, když uživatel přijme příchozí volání do systému.
- Sestava, když uživatel ukončí volání v systému.

Pokud aplikace chce komunikovat akce místního uživatele k systému, použije `CXTransaction` třídy:

[![](callkit-images/callkit04.png "Vytváření sestav do systému pomocí CXCallController")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Implementace CallKit

Následující části vysvětlují, jak implementovat CallKit v aplikaci Xamarin.iOS VOIP. Z důvodu příklad bude tento dokument pomocí kódu z fiktivní MonkeyCall VOIP aplikace. Kód uvedený v tomto poli představuje několik podpůrných tříd, CallKit bude konkrétní části podrobně popsány v následujících částech.

### <a name="the-activecall-class"></a>ActiveCall – třída

`ActiveCall` Třída se používá aplikace MonkeyCall pro uložení všech informací o VOIP volání, který je aktuálně aktivní následujícím způsobem:

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall` obsahuje několik vlastností, které definují stav volání a dvě události, které může být vyvolána při změně stavu volání. Protože se jedná pouze o příklad, existují tři metody simulated počáteční, když si odpovíte a koncovou volání.

### <a name="the-startcallrequest-class"></a>StartCallRequest – třída

`StartCallRequest` Statická třída obsahuje několik pomocné metody, které budou použity při práci s odchozí volání:

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL` a `CallHandleFromActivity` třídy se používají v AppDelegate k získání kontaktní popisovač uživatele volaná v odchozí volání. Další informace najdete v tématu [zpracování odchozích volání](#Handling-Outgoing-Calls) části níže.

### <a name="the-activecallmanager-class"></a>ActiveCallManager – třída

`ActiveCallManager` Třída zpracovává všechny otevřené volání v aplikaci MonkeyCall.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID == uuid) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

Znovu, protože to je simulace pouze `ActiveCallManager` pouze udržuje kolekci `ActiveCall` objekty a má rutina pro vyhledání daného volání pomocí jeho `UUID` vlastnost. Obsahuje také metody pro spuštění, ukončení a změní stav blokované odchozí volání. Další informace najdete v tématu [zpracování odchozích volání](#Handling-Outgoing-Calls) části níže.

### <a name="the-providerdelegate-class"></a>ProviderDelegate – třída

Jak je popsáno výše, `CXProvider` poskytuje obousměrné komunikace mezi systém a aplikace pro out-of-band oznámení. Vývojář musí poskytnout vlastní `CXProviderDelegate` a připojte ji k `CXProvider` pro aplikaci pro zpracování událostí CallKit out-of-band. MonkeyCall používá následující `CXProviderDelegate`:

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Mask
            var maskImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconMaskImageData = maskImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

Když je vytvořena instance tohoto delegáta, je předaná `ActiveCallManager` , použijete pro zpracování všech aktivit volání. V dalším kroku definuje typy popisovač (`CXHandleType`), `CXProvider` bude reagovat na:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

A získá maska, která bude použita na ikonu aplikace, když probíhá volání:

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

Tyto hodnoty získat seskupeny do `CXProviderConfiguration` který se použije ke konfiguraci `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Delegát potom vytvoří novou `CXProvider` se tyto konfigurace a se připojí k ní:

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

Při použití CallKit, aplikace se už vytvořit a zpracovat vlastní zvuk relací, místo toho je nutné nakonfigurovat a používat relaci zvuk, která bude systém vytvořit a zpracovat pro něj. 

Pokud to byly skutečné aplikace, `DidActivateAudioSession` metoda se používá ke spuštění volání s předem nakonfigurovaná `AVAudioSession` poskytující systému:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Využívá také `DidDeactivateAudioSession` metodu za účelem dokončení a jeho připojení k systému verze zadané zvuk relace:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Zbytek kód bude podrobně v následujících částech.

### <a name="the-appdelegate-class"></a>AppDelegate – třída

MonkeyCall používá k ukládání instancí AppDelegate `ActiveCallManager` a `CXProviderDelegate` které budou použity v celé aplikaci:

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl` a `ContinueUserActivity` přepsání metody se používají při aplikace zpracovává odchozí volání. Další informace najdete v tématu [zpracování odchozích volání](#Handling-Outgoing-Calls) části níže.

## <a name="handling-incoming-calls"></a>Zpracování příchozích hovorů

Existuje několik stavy a procesy, které příchozího hovoru VOIP můžete projít během obvyklý pracovní postup příchozí volání, jako:

- Informuje o uživatele (a systému), existuje příchozího hovoru.
- Příjem oznámení, když chce uživatel zodpovědět volání a inicializaci volání s jiným uživatelem.
- Pokud chce uživatel ukončení hovoru aktuální informujte systém a síťové komunikace.

V následujících částech bude trvat podrobné podívejte se na tom, jak aplikaci můžete použít CallKit pro zpracování příchozí volání pracovní postup, znovu s použitím aplikace MonkeyCall VOIP jako příklad.

### <a name="informing-user-of-incoming-call"></a>Informování uživatelů příchozí volání

Když vzdálený uživatel má spustili VOIP konverzace s místního uživatele, dojde k následující položky:

[![](callkit-images/callkit05.png "Vzdálený uživatel bylo zahájeno VOIP konverzace")](callkit-images/callkit05.png#lightbox)

1. Aplikace získá oznámení z jeho komunikační síť se příchozího hovoru VOIP.
2. Aplikace používá `CXProvider` k odeslání `CXCallUpdate` systému, jelikož se volání.
3. Systém publikuje volání rozhraní systému, systémové služby a všechny ostatní aplikace VOIP pomocí CallKit.

Například v `CXProviderDelegate`:

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

Tento kód vytvoří novou `CXCallUpdate` instance a připojí k němu identifikující volající popisovač. Dále je použita `ReportNewIncomingCall` metodu `CXProvider` třída k informování systému volání. Pokud je úspěšné, volání se přidá do kolekce aplikace active volání, pokud není, chyba je třeba ohlásit uživateli.

### <a name="user-answering-incoming-call"></a>Uživatel volaného příchozí volání

Pokud chce uživatel odpovíte na příchozí volání VOIP, dojde k následující položky:

[![](callkit-images/callkit06.png "Uživatel přijme příchozí volání VOIP")](callkit-images/callkit06.png#lightbox)

1. Uživatelského rozhraní systému informuje systému, že uživatel chce přijetí hovoru VOIP.
2. Odešle systému `CXAnswerCallAction` na aplikaci `CXProvider` Jelikož se záměrem odpovědí.
3. Aplikace informuje jeho síťové komunikace, uživatel je odpoví volání a volání VOIP pokračuje jako obvykle.

Například v `CXProviderDelegate`:

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Tento kód nejprve hledá dané volání ve svém seznamu active volání. Pokud volání nebyl nalezen, je upozornění systému a metodu ukončí. Pokud se najde, `AnswerCall` metodu `ActiveCall` třídy se nazývá zahájíte volání a systém nemá informace, pokud je úspěšná nebo neúspěšná.

### <a name="user-ending-incoming-call"></a>Uživatel ukončení příchozí volání

Pokud uživatel chce ukončit volání z v uživatelském rozhraní aplikace, dojde k následující položky:

[![](callkit-images/callkit07.png "Uživatel ukončí volání z v uživatelském rozhraní aplikace")](callkit-images/callkit07.png#lightbox)

1. Aplikace vytvoří `CXEndCallAction` , získá seskupeny do `CXTransaction` odeslaná do systému informovat, že volání ukončuje.
2. Systém ověřuje koncové volání záměr a odešle `CXEndCallAction` zpět do aplikace pomocí `CXProvider`.
3. Aplikace pak informuje jeho síťové komunikace, volání ukončuje.

Například v `CXProviderDelegate`:

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Tento kód nejprve hledá dané volání ve svém seznamu active volání. Pokud volání nebyl nalezen, je upozornění systému a metodu ukončí. Pokud se najde, `EndCall` metodu `ActiveCall` třídy se nazývá k ukončení hovoru a systém nemá informace, pokud je úspěšná nebo neúspěšná. Pokud bylo úspěšné, odeberou se z kolekce aktivní volání volání.

## <a name="managing-multiple-calls"></a>Správa více volání

Většinu aplikací VOIP může najednou zpracovat více volání. Například, pokud je aktuálně aktivní volání VOIP a upozornění aplikace získá, že dispozici je nový příchozí volání, uživatel může pozastavit nebo Zavěsit při prvním volání k odpovědi na druhou.

V situaci, udávají výše, odešle systému `CXTransaction` na aplikaci, která bude obsahovat seznam více akcí (například `CXEndCallAction` a `CXAnswerCallAction`). Všechny tyto akce se musí splňovat zvlášť, tak, aby v systému správně aktualizace uživatelského rozhraní.

## <a name="handling-outgoing-calls"></a>Zpracování odchozích volání

Pokud uživatel klepnutím položku ze seznamu nedávno (v telefonní aplikace), například, který je volání náležící k aplikaci, bude odeslána _spustit volání záměr_ systémem:

[![](callkit-images/callkit08.png "Přijímá volání záměr spuštění")](callkit-images/callkit08.png#lightbox)

1. Aplikace vytvoří _spustit akci volání_ podle spustit volání záměr přijatou ze systému. 
2. Aplikace bude používat `CXCallController` k vyžádání akce spustit volání ze systému.
3. Pokud systém akceptuje akci, bude vrácen do aplikace pomocí `XCProvider` delegovat.
4. Odchozí volání spuštění aplikace s jeho síťové komunikace.

Další informace o záměry, najdete v tématu naše [tříd Intent a rozšíření tříd Intent uživatelského rozhraní](~/ios/platform/sirikit/understanding-sirikit.md) dokumentaci. 

### <a name="the-outgoing-call-lifecycle"></a>Odchozí životního cyklu volání

Při práci s CallKit a odchozí volání, bude nutné informovat systém následující události životního cyklu aplikace:

1. **Spouštění** -informovat o tom, systém, který odchozí volání je spustit.
2. **Spuštění** -informovat o tom, systém, který odchozí volání bylo zahájeno.
3. **Připojení** -informovat o tom, systém, který se připojuje odchozí volání.
4. **Připojení** -informování odchozích volání připojil a že obě strany můžete nyní komunikovat.

Například následující kód spustí odchozí volání:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Vytvoří `CXHandle` a používá ke konfiguraci `CXStartCallAction` který je seskupeny do `CXTransaction` tedy posílá systém pomocí `RequestTransaction` metodu `CXCallController` – třída. Při volání `RequestTransaction` metoda systému můžete umístit všechny existující volání blokované, bez ohledu na to zdroji (telefonní aplikace, FaceTime, VOIP, atd.), před zahájením nové volání.

Požadavek na spuštění odchozí volání VOIP mohou pocházet z několika různých zdrojů, jako je Siri, položku na kartě Kontakt (v aplikaci kontakty) nebo v nedávno seznamu (Phone app). V těchto situacích aplikace odešle spustit volání záměr uvnitř `NSUserActivity` a AppDelegate bude nutné ji zpracovat:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

Zde `CallHandleFromActivity` metoda pomocná třída `StartCallRequest` používá získat popisovač osobě volané (najdete v části [StartCallRequest třída](#The-StartCallRequest-Class) výše). 

`PerformStartCallAction` Metodu [ProviderDelegate třída](#The-ProviderDelegate-Class) slouží k nakonec spusťte vlastní odchozí volání a informovat systém životního cyklu:

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Vytvoří instanci `ActiveCall` třídy (pro uchování informací o volání v průběhu) a naplní s osobou volána. `StartingConnectionChanged` a `ConnectedChanged` události se používají k monitorování a hlášení odchozí životního cyklu volání. Volání je spuštěná a systému informováni, že splnil akce.

### <a name="ending-an-outgoing-call"></a>Koncová odchozí volání

Když uživatel dokončí se odchozí volání a chce ho ukončit, lze použít následující kód:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Pokud vytvoří `CXEndCallAction` s UUID volání na konec obsahuje ureitou v `CXTransaction` tedy posílá systém pomocí `RequestTransaction` metodu `CXCallController` – třída. 

## <a name="additional-callkit-details"></a>Podrobnosti o další CallKit

Tato část popisuje některé další podrobnosti, které vývojář bude potřeba vzít v úvahu při práci s CallKit, jako například:

- Konfigurace zprostředkovatele
- Akce chyby
- Omezení systému
- VOIP zvuk

### <a name="provider-configuration"></a>Konfigurace zprostředkovatele

Konfigurace zprostředkovatele umožňuje aplikaci pro iOS 10 VOIP přizpůsobit činnost koncového uživatele (uvnitř nativní uživatelského rozhraní v volání) při práci s CallKit.

Aplikace můžete provést následující typy přizpůsobení:

- Lokalizovaný název zobrazení.
- Povolte podporu videa volání.
- Přizpůsobení tlačítka v Uživatelském rozhraní ve volání prezentací vlastní ikonu maskované bitové kopie. Interakce uživatele s vlastní tlačítka je zasílán přímo na aplikaci na zpracování. 

### <a name="action-errors"></a>Akce chyby

aplikace pro iOS 10 VOIP pomocí CallKit potřebujete zpracování akce selhání řádně a mít neustále informováni o stavu akce uživatele. 

Následující příklad vzít v úvahu:

1. Aplikace přijal spustit volání akce, byl zahájen proces inicializace nové volání VOIP s jeho síťové komunikace.
2. Kvůli omezené nebo žádné síťové komunikace schopnosti toto připojení se nezdaří.
3. Aplikace *musí* odeslat **nezdaří** zprávy zpět na akce spustit volání (`Action.Fail()`) k informování systému selhání.
4. To umožňuje systému informovat uživatele o stavu volání. Chcete-li například zobrazit uživatelské rozhraní selhání volání.

Kromě toho aplikace pro iOS 10 VOIP muset odpovědět na _chyby vypršení časového limitu_ , může dojít, když očekávanou akci nelze zpracovat v rámci dané množství času. Každý typ akce poskytované CallKit má maximální hodnotu časového limitu s ním spojená. Tyto hodnoty časového limitu Ujistěte se, že žádnou akci CallKit požadované uživatelem se zpracovává přizpůsobivý způsobem, proto zachování operačního systému plynulá práce a rychlé reakce také.

Existuje několik metod na zprostředkovatele delegáta (`CXProviderDelegate`), by měla být potlačena pro pohodlné zpracování také situacích tento časový limit.

### <a name="system-restrictions"></a>Omezení systému

Na základě aktuálního stavu spuštění aplikace iOS 10 VOIP zařízení iOS, může být vynuceno určitá omezení systému.

Například příchozího hovoru VOIP lze omezit systém, pokud:

1. Osoba volající je na seznamu blokovaných volající uživatele.
2. Zařízení s iOS uživatele je v režimu naruší není proveďte.

Pokud VOIP volání je omezené na základě všech těchto situacích, použijte následující kód se nezdařilo:

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VOIP zvuk

CallKit poskytuje několik výhod pro zpracování audio prostředky, které aplikace iOS 10 VOIP bude vyžadovat během provozu VOIP volání. Jednou z největších výhod je aplikace zvuk relace bude mít zvýšená priority při spuštění v iOS 10. To se stejnou úrovní priority jako integrované v telefonu a FaceTime aplikací a tato úroveň rozšířené priority zabrání ostatní spuštěné aplikace by došlo k přerušení relace zvuk VOIP aplikace.

Kromě toho CallKit má přístup k jiné zvuk směrování pomocné parametry, které mohou zvýšit výkon a inteligentně směrování VOIP zvuk do konkrétní výstupní zařízení během volání za provozu na základě uživatelských předvoleb a stavy zařízení. Například založené na připojené zařízení, jako například sluchátka bluetooth, živé připojení CarPlay nebo nastavení usnadnění.

Během životního cyklu typické VOIP volat pomocí CallKit, bude nutné nakonfigurovat zvuk datový proud, který ho poskytne CallKit aplikace. Podívejte se na následující příklad:

[![](callkit-images/callkit09.png "Počáteční sekvence volání akce")](callkit-images/callkit09.png#lightbox)

1. Aplikace k přijetí příchozího hovoru přijme spustit volání akce.
2. Než tuto akci je splněna aplikací, poskytuje konfiguraci, která se bude vyžadovat pro jeho `AVAudioSession`.
3. Aplikace informuje systém splnění akce.
4. Před voláním připojí, CallKit poskytuje vysokou prioritou `AVAudioSession` odpovídající konfiguraci, která požadované aplikace. Aplikace, budete informování prostřednictvím `DidActivateAudioSession` metodu jeho `CXProviderDelegate`.

## <a name="working-with-call-directory-extensions"></a>Práce s volání rozšíření adresáře

Při práci s CallKit, _volání rozšíření adresáře_ poskytnout způsob, jak přidat blokované volání čísla a čísla, která jsou specifická pro danou aplikaci VOIP ke kontaktům v aplikaci obraťte se na zařízení s iOS identifikovat.

### <a name="implementing-a-call-directory-extension"></a>Implementace příponu Directory volání

K implementaci příponu volání adresáře v aplikaci Xamarin.iOS, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otevřít řešení aplikace v sadě Visual Studio for Mac.
2. Klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a vyberte **přidat** > **přidat nový projekt**.
3. Vyberte **iOS** > **rozšíření** > **volání rozšíření adresáře** a klikněte na **Další** tlačítko: 

    [![](callkit-images/calldir01.png "Vytvoření nové rozšíření adresáře volání")](callkit-images/calldir01.png#lightbox)
4. Zadejte **název** pro rozšíření a klikněte na **Další** tlačítko: 

    [![](callkit-images/calldir02.png "Zadejte název pro rozšíření")](callkit-images/calldir02.png#lightbox)
5. Upravit **název projektu** nebo **název řešení** dle potřeby **vytvořit** tlačítko: 

    [![](callkit-images/calldir03.png "Vytvoření projektu")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otevřete řešení aplikace v sadě Visual Studio.
2. Klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a vyberte **přidat** > **přidat nový projekt**.
3. Vyberte **iOS** > **rozšíření** > **volání rozšíření adresáře** a klikněte na **Další** tlačítko: 

    [![](callkit-images/calldir01w.png "Vytvoření nové rozšíření adresáře volání")](callkit-images/calldir01.png#lightbox)
4. Zadejte **název** pro rozšíření a klikněte na **OK** tlačítko

-----


Bude přidáno `CallDirectoryHandler.cs` třídu do projektu, který vypadá takto:

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

`BeginRequest` Metoda v adresáři obslužnou rutinu volání bude nutné upravit tak, aby zadejte požadovanou funkci. V případě ukázce výše pokusí se nastavit seznam blokovaných a k dispozici čísla v databázi aplikace VOIP kontakty. Pokud z nějakého důvodu buď požádá o selhání, vytvořit `NSError` popsat selhání a předejte ji `CancelRequest` metodu `CXCallDirectoryExtensionContext` třídy.

Nastavení použití blokované čísla `AddBlockingEntry` metodu `CXCallDirectoryExtensionContext` třídy. Čísla poskytnutá metodě _musí_ být ve vzestupném pořadí podle čísel. Pro zajištění optimálního výkonu a využití paměti zvažte době, kdy jsou že mnoho telefonních čísel, pouze načtení podmnožinu čísel v daném okamžiku a použití autorelease fondy k uvolnění objektů přidělené během každé dávky čísel, která jsou načtena.

Pokud chcete informovat o tom, obraťte se na aplikaci kontaktní čísla ví, že aplikace VOIP, použijte `AddIdentificationEntry` metodu `CXCallDirectoryExtensionContext` třídy a zadejte číslo a identifikační popisek. Znovu čísla poskytnutá metodě _musí_ být ve vzestupném pořadí podle čísel. Pro zajištění optimálního výkonu a využití paměti zvažte době, kdy jsou že mnoho telefonních čísel, pouze načtení podmnožinu čísel v daném okamžiku a použití autorelease fondy k uvolnění objektů přidělené během každé dávky čísel, která jsou načtena.


## <a name="summary"></a>Souhrn

Tento článek má zahrnutých nového rozhraní API CallKit této Apple vydala v iOS 10 a postupy implementace v aplikacích Xamarin.iOS VOIP. Ukázaly, jak CallKit umožňuje aplikaci, jak integrovat do systému iOS, jak poskytuje parity funkcí s integrované aplikace (například Phone) a jak zvyšuje viditelnost aplikace v iOS v umístění například zámku a domů obrazovky prostřednictvím Siri interakce a prostřednictvím Kontakty aplikace.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
