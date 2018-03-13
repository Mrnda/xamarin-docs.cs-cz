---
title: "Rozšíření aplikace pokročilé zpráv"
description: "Tento článek ukazuje pokročilé techniky pro práci s rozšíření zpráv aplikace Xamarin.iOS řešení, které se integruje se službou aplikace zprávy a představuje nové funkce pro uživatele."
ms.topic: article
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fcfd1fd2ec9271bb5e8d9e09b43b7dc4cf3b3f12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="advanced-message-app-extensions"></a>Rozšíření aplikace pokročilé zpráv

_Tento článek ukazuje pokročilé techniky pro práci s rozšíření zpráv aplikace Xamarin.iOS řešení, které se integruje se službou aplikace zprávy a představuje nové funkce pro uživatele._


Nové rozšíření aplikace zpráv do systému iOS 10, se integruje s **zprávy** aplikaci a uvede nové funkce pro uživatele. Rozšíření můžete odeslat text, štítky, souborů médií a interaktivní zprávy.

## <a name="about-message-app-extensions"></a>O rozšíření aplikace zpráv

Jak jsme uvedli výše, rozšíření aplikace zpráva se integruje s **zprávy** aplikaci a uvede nové funkce pro uživatele. Rozšíření můžete odeslat text, štítky, souborů médií a interaktivní zprávy. K dispozici jsou dva typy rozšíření aplikace zpráva:

- **Balíčky štítku** – obsahuje kolekci štítky, které uživatel může přidávat do zprávy. Štítku balíčky lze vytvořit bez psaní jakéhokoli kódu.
- **iMessage aplikace** -může být vlastní uživatelské rozhraní v rámci aplikace zprávy pro výběr štítky, zadáním textu, včetně mediální soubory (s převody volitelné typu) a vytváření, úpravy a odesílání zpráv interakce.

Rozšíření zpráv aplikace poskytují tři hlavní typy obsahu:

- **Interaktivní zprávy** -jsou pro typ obsahu vlastní zprávu, který generuje aplikaci, když uživatel klepnutím na zprávu, aplikace bude spuštěna v popředí.
- **Štítky** -jsou obrázky generované aplikací, které můžou být součástí zprávy odeslané mezi uživateli. V tématu naše [zmrzlinová Tvůrce](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) ukázková aplikace pro příklad implementace štítku Pack aplikace.
- **Další obsah podporované** -aplikace můžete zadat obsah, jako jsou fotografie, videa, text nebo odkazy na typ, který vždy nepodporuje aplikace zprávy.

Nové do systému iOS 10, aplikace zprávu nyní obsahuje vlastní vyhrazené, integrované aplikace úložiště. Všechny aplikace, které zahrnují rozšíření zpráv aplikace se zobrazí a povýší v tomto úložišti. Panel nové zprávy aplikace se zobrazí všechny aplikace, které byly staženy z obchodu s aplikacemi zprávy zajistit rychlý přístup uživatelům.

Také nové v iOS 10, Apple přidala vložené uvedení aplikace, která umožňuje uživatelům snadno zjistit, aplikace. Například pokud jeden uživatel odesílá obsah do jiné aplikaci, která 2. uživatel nemá nainstalovaný (např. pásek příkladu), název aplikace pro odesílání je uvedený v části obsah v historii zpráv. Pokud uživatel klepnutím aplikace název, jsme otevřít úložiště zpráv aplikace a aplikaci vybrali v úložišti.

Rozšíření zpráv aplikace jsou podobné pro existující aplikace iOS, že vývojář je dobře známé pro vytváření a budou mít přístup ke všem standardní rozhraní a funkcí standardní iOS aplikace. Příklad:

- Mají přístup k nákupy v aplikaci.
- Mají přístup k Apple platit.
- Mají přístup k hardwaru zařízení, jako jsou fotoaparát.

Rozšíření zpráv aplikace jsou podporovány pouze na iOS 10, ale je viditelný v zařízení watchOS a systému macOS obsah, který tato rozšíření odeslat. Nové _nedávno stránky_ přidá do watchOS 3, zobrazí poslední štítky, které byly odeslány z telefonu, včetně těch, které z rozšíření zpráv aplikace, a umožní uživatelům odesílat tyto štítky z hodinek.

## <a name="about-interactive-messages"></a>O interaktivní zprávy

Interaktivní zprávy jsou poskytovány rozšíření aplikace zpráv a bublinový vlastní zprávy k dispozici. Povolit uživatelům vytvářet interaktivní obsah zprávy, se vložte do pole zpráva vstup a odešlete ji.

[![](advanced-message-app-extensions-images/interactive01.png "Vytváření obsahu interaktivní zprávy")](advanced-message-app-extensions-images/interactive01.png#lightbox)

Přijímající uživatel může odpovědět interaktivní zprávu klepnutím jeho bublin zpráva v historii zpráv načíst rozšíření aplikace zprávu, která ji vytvořila. Rozšíření bude spuštěného celé obrazovce a umožní uživateli napište odpověď a odešlete ji zpět do původního uživatele.

[![](advanced-message-app-extensions-images/interactive02.png "Rozšíření spuštění celé obrazovky")](advanced-message-app-extensions-images/interactive02.png#lightbox)


V následujících tématech se budeme rozpis naleznete níže:

- Přehled rozhraní API zprávy
- Životní cyklus rozšíření
- Vytváření zprávy
- Odesílání zprávy

## <a name="messages-api-overview"></a>Přehled rozhraní API zprávy

Při vyvolání uživatelem, zobrazí se v dolní části historii zpráv v režimu compact zobrazení rozšíření aplikace zpráv:

[![](advanced-message-app-extensions-images/interactive03.png "Přehled rozhraní API zprávy")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController` Objekt v rozšíření zpráv aplikace je hlavní třídy, která je volána, když rozšíření se zobrazí uživateli.
2. Konverzace se zobrazí uživateli jako `MSConversation` instance objektu.
3. `MSMessage` Třída reprezentuje dané bublin zpráva v konverzaci.
4. `MSSession` Určuje, jak je odeslána zpráva.
5. `MSMessageTemplateLayout` Určuje, jak se zobrazí zpráva

## <a name="the-extension-lifecycle"></a>Životní cyklus rozšíření

Podívejte se na proces rozšíření aplikace zpráva, aby se aktivovala:

[![](advanced-message-app-extensions-images/interactive04.png "Proces rozšíření aplikace zpráva, aby se aktivovala")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. Při spuštění rozšíření (například ze panel aplikace) bude aplikace zpráva spustit proces.
2. `DidBecomeActive` Metoda je volána a předána `MSConversation` představující konverzace, který rozšíření zpráv aplikace běží v.
3. Protože je založen rozšíření odhlásit z `UIViewController` i `ViewWillAppear` a `ViewDidAppear` se označují jako.

Potom si prohlédněte proces stal deaktivováno rozšíření aplikace zpráva:

[![](advanced-message-app-extensions-images/interactive05.png "Proces rozšíření aplikace zpráva stal deaktivováno")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. Po deaktivaci rozšíření aplikace zprávy `ViewWillDisappear` první volání metody.
2. Pak se `ViewDidDisappear` volání metody.
3. `WillResignActive` Metoda je volána a předána `MSConversation` představující konverzace, který rozšíření zpráv aplikace běží v. V tomto okamžiku je spojení mezi aplikace zprávy a rozšíření budou vydané.
4. V určitém okamžiku novější aplikací zprávy ukončení procesu.

Vzhledem k tomu, že je rozšíření zkrátí procesu, je intenzivně ukončen systémem krátkodobě zpracování a stav baterie. Vývojář musí mějte na paměti při návrhu a implementace rozšíření aplikace zpráv.

## <a name="composing-a-message"></a>Vytváření zprávy

Po rozšíření zpráv aplikace běží v procesu a má zobrazí jeho uživatelské rozhraní, můžete použít následující kód k vytvoření nové zprávy:

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

Tento kód vytvoří novou `MSMessage` a nastaví několik vlastností (jako například `Url`). Při zprávu lze vytvořit pouze v systému iOS, odesláním iOS i systému macOS který se má zobrazit.

Pokud uživatel klikne na bublin zpráva v konverzaci v systému macOS, Mac se pokusí otevřít adresu určenou v adrese URL ve webovém prohlížeči. Web pro vývojáře v důsledku toho by mohli zobrazit některé reprezentace zprávy v webový prohlížeč v systému macOS založené na počítače.

`AccessibilityLabel` Vlastnost je používána čtečky obrazovky na číst zápis konverzace pro uživatele. `Layout` Vlastnost určuje, jak zpráva se zobrazí, aktuálně pouze `MSMessageTemplateLayout` je podporována a vypadá takto:

[![](advanced-message-app-extensions-images/interactive06.png "Šablona MSMessageTemplateLayout")](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image` Vlastnost `MSMessageTemplateLayout` poskytuje obsah pro hlavní části MessageBubble na obrazovce. `MediaFileUrl` Vlastnost také poskytuje obsah pro tělo zprávy bublin, ale umožňuje pro obsah, který nepodporuje `UIImage` (například video souboru, který by cykly na pozadí). Pokud `Image` a `MediaFileUrl` vlastnosti jsou k dispozici, `Image` vlastnost bude mít přednost. `MediaFileUrl` Podporuje PNG, JPEG, GIF a videa (v libovolném formátu, který může být přehráván rámcem Media Player) formátů médií.

Doporučená velikost je 300 x 300 pixelů na řešení 3 x. Přijímat jsou také mírně větší a menší prostředky a Apple navrhne testování pomocí několika různých velikostech k dosažení nejlepších výsledků. Zpráva bude nižší sample a aplikace škálovat toto médium podle potřeby.

Když jsou prostředky k příjemce, všechna média připojené bude automaticky převodu aplikací zprávy k optimalizaci je z přenosu přes sítě. Z tohoto důvodu se nedoporučuje Apple včetně textu v média, který je připojen ke zprávě, protože média bude škálovat a komprimované pro přenos, proto potenciálně vykreslování textu nečitelná.

`ImageTitle` a `ImageSubtitle` vlastnosti zadejte popis pro médium, které jsou poskytovány bublin zprávy. Tyto vlastnosti bude odeslán jako text do přijímajícího zařízení kde budou crisply vykresleny v levém dolním rohu bitovou kopii.

`Caption`, `SubCaption`, `TrailingCaption` a `TrailingSubcaption` vlastnosti další popisují bitovou kopii a bude vykreslen v části níže bitovou kopii. Všechny tyto vlastnosti k nastavení `null` vytvoří zprávu bublin bez popisek oblasti:

[![](advanced-message-app-extensions-images/interactive07.png "Zpráva bublin bez oblasti popisek")](advanced-message-app-extensions-images/interactive07.png#lightbox)

Poslední věcí, kterou si uvědomit, je, že aplikace zprávy bude kreslení ikonu rozšíření aplikace zpráva v levém horním rohu bublin zprávy.

## <a name="sending-a-message"></a>Odesílání zprávy

Jednou `MSMessage` byl složený, následující kód slouží k odesílání:

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

`ActiveConversation` Vlastnost `MSMessagesAppViewController` bude obsahovat aktuální konverzace, která rozšíření aplikace zpráva byla spuštěná v.

Volání `InsertMessage` z `MSConversation` zahrnout zprávy konverzace a zpracovávat všechny chyby, které by mohly nastat. Pokud zpráva je úspěšně zahrnuty, zobrazí se v poli vstup bublin zpráva.

Kromě toho můžete rozšíření odeslat různé typy dat konverzace jako:

- **Text** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Přílohy** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Štítky**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` kde `sticker` je `MSSticker`.

Jakmile nový obsah na pole vstupu, uživatel se bude moct odeslat zprávu klepnutím blue **odeslat** tlačítko (stejně jako jakékoli typické zprávy). Neexistuje žádný způsob pro rozšíření zpráv aplikace automaticky odeslat obsah, tento proces je zcela pod kontrolou uživatele.

## <a name="handling-the-compact-and-expanded-modes"></a>Zpracování režimů Compact a rozšířená

Rozšíření aplikace zprávy lze zobrazit v jednom ze dvou režimů jiné zobrazení:

[![](advanced-message-app-extensions-images/interactive08.png "Rozšíření zpráva aplikace zobrazí ve dvou režimech jiné zobrazení: Compact & Rozšířené")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -Toto je výchozí režim, kde rozšíření aplikace zpráva zabírají 25 % dolní zobrazení zpráv. V kompaktním režimu aplikace nemá přístup ke klávesnici a vodorovného posouvání a nástroje pro rozpoznávání gesto prstem. Mít přístup k poli vstup a volá, aby se aplikace `InsertMessage` se okamžitě zobrazí pro uživatele existuje.
- **Rozšířit** -rozšíření aplikace zprávu doplní celého zobrazení zpráv. Nemá přístup do pole vstupu, ale nemá přístup ke klávesnici, vodorovného posouvání a nástroje pro rozpoznávání gesto prstem.

Rozšíření aplikace zpráv je možné přepnout mezi těmito režimy buď programově, nebo ručně uživatelem kdykoli a musí být okamžitě reaguje na všechny změny v zobrazení režimu.

Podívejte se na následující příklad zpracovávat přepínat mezi dva režimy jiné zobrazení. Dva různé řadiče zobrazení se bude vyžadovat pro každý stav. `StickerBrowserViewController` Obslužné rutiny **Compact** zobrazení a `AddStickerViewController` zpracuje **rozšířené** zobrazení:

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
}
```

`DidTransition` Je metoda potlačena za účelem zpracování přepínání mezi dvěma způsoby:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
```

Volitelně může mít aplikace používat `WillTransition` metoda se zpracovat změnu režimu zobrazení, než se zobrazí uživatelům (jak je tomu v předchozím příkladu Tvůrce Icecream). Další informace najdete v tématu naše [další přizpůsobení štítku](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) dokumentaci.

## <a name="replying-to-a-message"></a>Odpovědi na zprávu

Existují dva případy, vyžadující rozšíření aplikace zpráv pro zpracování v odpovědi zprávu:

[![](advanced-message-app-extensions-images/interactive09.png "Rozšíření aplikace zpráva v režimech neaktivní a aktivní")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **Rozšíření je neaktivní** -existuje jedno rozšíření aplikace zpráva zpráva bublin v přepisu zpráv, který uživatel může klepnout aktivovat rozšíření a pokračujte interaktivní konverzace.
- **Rozšíření je aktivní** – uživatel může klepnout bublin zpráva rozšíření aplikace zpráva v přepisu zprávy zadejte režim rozšířené zobrazení a pokračovat v procesu interaktivní z tam, kde skončil.

### <a name="the-extension-is-inactive"></a>Rozšíření je neaktivní.

Když je zpráva bublin stisknuté uživatelem v přepisu zprávu a rozšíření zpráv aplikace je neaktivní, se stane následující proces:

[![](advanced-message-app-extensions-images/interactive10.png "Zpracování neaktivní bublin zpráv")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. Uživatel klepnutím bublin zpráva rozšíření.
2. Při spuštění rozšíření zpráv aplikace spustí proces.
3. `DidBecomeActive` Metoda je volána a předána `MSConversation` představující konverzace, který rozšíření zpráv aplikace běží v.
4. Protože je založen rozšíření odhlásit z `UIViewController` i `ViewWillAppear` a `ViewDidAppear` se označují jako.

Po dokončení procesu se zobrazí rozšíření zpráv aplikace v režimu Rozšířené zobrazení.

### <a name="the-extension-is-active"></a>Rozšíření není aktivní

Když je zpráva bublin stisknuté uživatelem v přepisu zprávu a rozšíření zpráv aplikace je aktivní, se stane následující proces:

[![](advanced-message-app-extensions-images/interactive11.png "Zpracování active bublin zpráv")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. Uživatel klepnutím bublin zpráva rozšíření.
2. Protože je již aktivní, rozšíření aplikace zprávu `WillTransition` metodu `MSMessagesAppViewController` je volána pro zpracování přepnutí z Compact do režimu zobrazení rozšířené.
3. `DidSelectMessage` Metodu `MSMessagesAppViewController` je volána a předána `MSMessage` a `MSConversation` náležející do bublin zpráva.
4. `DidTransition` Metodu `MSMessagesAppViewController` je volána pro zpracování přepínání z Compact do režimu Rozšířené zobrazení.

Znovu po dokončení procesu rozšíření aplikace zpráva zobrazí v režimu Rozšířené zobrazení.

### <a name="accessing-the-selected-message"></a>Přístup k vybrané zprávy

V obou případech, když uživatel klepnutím zpráva bublin, které patří k rozšíření aplikace zprávu, ji budou muset získat přístup k `MSMessage` , byl klepli `SelectedMessage` vlastnost `MSConversation`.

Příklad:

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

V uživatelském rozhraní rozšíření zpráv aplikace se mají vybrané zprávy a uživatel má být povoleno se vytvořit odpověď.

## <a name="removing-partially-completed-messages"></a>Odebrání částečně dokončit zprávy

Probíhá odesílání různé kroky interaktivní konverzaci mezi dvěma uživatele v konverzace, můžete začít částečně dokončené bublinách zpráva zbytečných souborů přepis zpráva:

[![](advanced-message-app-extensions-images/interactive12.png "Částečně dokončené bublinách zprávu můžete zbytečného přepis zpráv")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Místo toho by měla rozšíření aplikace zprávu sbalit předchozí zpráva bublinách do stručného komentář v přepisu zpráva:

[![](advanced-message-app-extensions-images/interactive13.png "Sbalení předchozí zpráva bublinách v přepisu zpráv")](advanced-message-app-extensions-images/interactive13.png#lightbox)

To je ke zpracování pomocí `MSSession` na Sbalit všechny existující kroky. Proto `DidSelectMessage` metodu `MSMessagesAppViewController` třída může upravit tak, aby vypadat třeba takto:

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

Pokud vybraná zpráva již má ukončení `MSSession`, použije se jinak a nové `MSSession` je vytvořena. `SummaryText` Vlastnost `MSMessage` se používá k přidání titulku k sbalené předchozí kroky. Pokud `SummaryText` je nastavena na `null`, předchozí kroky v konverzaci bude zcela odebrána z přepis konverzace.

## <a name="advanced-message-api-features"></a>Pokročilé funkce rozhraní API zpráv

Se základními funkcemi nové rozhraní API zpráva podrobně výše dále zkontrolujte některé z dalších pokročilých funkcí, které Apple má integrovaný do rozhraní.

Nejprve existuje několik dalších metod přepsání v `MSMessagesAppViewController` třídu, která poskytují podrobnější přístup k konverzace:

- `DidStartSendingMessage` -Je volána, když uživatel klepnutím na tlačítko Odeslat. To neznamená, že zpráva byla doručení k příjemce, stačí, aby se spustil proces odesílání.
- `DidCancelSendingMessage` – To se stane, když uživatel klepnutím *X* tlačítko v pravém horním rohu bublin zpráva v přepisu konverzace.
- `DidReceiveMessage` -Tato metoda je volána, když je aktivní rozšíření aplikace zprávy novou zprávu bylo přijato z jeden z účastníků v konverzaci.

### <a name="group-conversations"></a>Skupina konverzace

Rozšíření aplikace zpráv lze použít při uživatelé jsou součástí skupiny konverzace (s 3 nebo více osob), a to bude nutné vzít v úvahu při navrhování a implementaci rozšíření aplikace zpráv.

Podívejte se na následující interakce skupiny konverzace s tři uživatelé:

[![](advanced-message-app-extensions-images/interactive14.png "Interakce skupiny konverzace s tři uživatelé")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. Uživatel 1 odešle interaktivní zprávy skupinu žádostí uživatele 2 a 3 uživatelů, zvolit burger přičemž.
2. Uživatel 2 zvolí rajčata.
3. Uživatel 3 vybere kyselé okurky.
4. Možnosti uživatele 2 a 3 uživatele přicházejí zpět na hodnotu 1 uživatele na téměř stejnou dobu. V důsledku toho uživatel 2 volba sbalena do souhrnu řádku a není k dispozici. Tento případ může mít také byla obráceně, s volbou uživatele 2 je právě sbalené bude a 3 uživatele.

V obou případech je toto chování nežádoucí, protože uživatel 1, byste měli mít přístup k možnosti uživatele 2 a 3 uživatele. Tuto situaci můžete vyřešit, je návrhy Apple, že rozšíření zpráv aplikace ukládá stav zpráv v cloudu a použít vlastnost adresa URL `MSMessage` (která se odešlou mezi uživatelů) pro přístup k této stavu.

Když uživatel odešle zprávu, je token relace generuje a instaluje do cloudu s aktuálním stavem zprávy. Když uživatel klepnutím na zprávu bublin v přepisu konverzace, token relace se používá k načtení stavu aktuální relace z cloudu.

### <a name="sender-identifiers"></a>Odesílatel identifikátory

Chcete-li diskutovat o přístup k identifikátor odesílatele zprávy, proveďte v příkladu výše uvedené skupiny konverzace:

[![](advanced-message-app-extensions-images/interactive15.png "Skupina konverzace odesílání identifikátorů")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Znovu odešle uživatele 1 interaktivní zprávy skupinu žádostí uživatele 2 a 3 uživatelů, zvolit burger přičemž.
2. Uživatele 3 vybere kyselé okurky.
3. Volba uživatele na 3 dorazí zpět na uživatele 1 a 2 uživatele ještě neodpoví.
4. Vzhledem k tomu, že Apple zajišťuje ochranu osobních údajů uživatele, rozšíření aplikace zprávu pouze informován o jedinečný identifikátor (jako `NSUUID`) přiřazené každý účastník v konverzaci. Na místním zařízení se označuje jenom identifikátor aktuálního uživatele.
5. `MSMessage` Má `SenderIdentifier` vlastnost, která odpovídá jednomu z uživatel je v seznamu účastník ví, že rozšíření.
6. Každé zařízení uživatele má svou vlastní kopii seznamu účastník, kde znovu, je známý jenom identifikátor místního uživatele.
7. Pokud je odeslána zpráva, jeho `SenderIdentifier` vlastnost je také známá jako u místního uživatele.

Identifikátorů odesílatele, mohou být používány následujícími způsoby:

- Podle seznamu účastník můžete rozšíření získat počet uživatelů v konverzaci.
- Když rozšíření obdrží zprávu od uživatele, ho můžete sledovat určité identifikátor odesílatele. Pokud obdrží další zprávu se stejným identifikátorem odesílatele, rozšíření ví, že je ze stejného uživatele.
- Jejich slouží k identifikaci konkrétního uživatele v konverzaci.

Identifikátor odesílatele lze použít v některém z textového pole `MSMessageTemplateLayout` pomocí prefixu s znak dolaru (`$`). Příklad:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

Pokud aplikace zprávy zobrazí zpráva bublin s tímto typem formátování, nahradí `$uuid...` s jméno kontaktní osoby účastníka konverzace, která zprávu odeslala.

Identifikátor odesílatele je jedinečná na každém zařízení, takže vyhledávání v diagramu výše znovu Poznámka zařízení tohoto uživatele 1 a 3 uživatele zařízení má jiný jedinečný identifikátor odesílatele u každého účastníka v konverzaci.

K instalaci rozšíření zpráv aplikace jsou vymezeny identifikátorů odesílatele. Proto pokud je uživatel odinstaluje a přeinstaluje rozšíření aplikace zpráva nové identifikátorů odesílatele budou generovány pro novou instalaci.

Pro přístup k identifikátorů odesílatele, rozšíření můžete použít následující kód:

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>Podporované platformy

Na následujících platformách Apple budou doručeny interaktivní zprávy generované příponu aplikace zpráva:

- watchOS 3
- macOS Sierra
- iOS 10

Tři platforem jenom iOS 10 umožní uživatelům vygenerovat zprávu interaktivní. V systému macOS Sierra, pokud uživatel klikne na interaktivní bublin zprávu, adresa URL připojené k `MSMessage` se otevře v prohlížeči Safari a reprezentaci zprávy, které mají být zobrazeny existuje.

Aplikace zprávy na watchOS, můžete aby handoff interaktivní zprávy na zařízení s iOS připojené, kde uživatel může vytvořit odpověď.

Nové rozhraní API zpráv má podporu pro záložní přihlašování, pokud doručení interaktivní zprávy na starších platformách, Apple:

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

Se bude používat v záložní format jako dva samostatné zprávy:

- Jeden bude bitovou kopii, protože poskytuje šablony rozložení.
- Druhý bude adresa URL, protože součástí `MSMessage`.

## <a name="summary"></a>Souhrn

Tento článek má uvedené pokročilé techniky pro práci s rozšíření zpráv aplikace Xamarin.iOS řešení, které se integruje s **zprávy** aplikace a přítomen nových funkcí pro uživatele.


## <a name="related-links"></a>Související odkazy

- [Tvůrce zmrzlinová (ukázka)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Odkaz na zprávy](https://developer.apple.com/reference/messages)
- [Průvodce programováním v rozšíření aplikace](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
