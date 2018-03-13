---
title: "Vzdálené Siri a řadiče Bluetooth."
description: "Tento článek se zabývá podpora nové řadiče herní Siri Remote a Bluetooth v Xamarin.tvOS aplikací."
ms.topic: article
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: cef717a727b3b018b9eec3e8a402ae4f927f7cb8
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="siri-remote-and-bluetooth-controllers"></a>Vzdálené Siri a řadiče Bluetooth.

_Tento článek se zabývá podpora nové řadiče herní Siri Remote a Bluetooth v Xamarin.tvOS aplikací._


Uživatele vaší aplikace Xamarin.tvOS nebude interakci s jeho rozhraní přímo jako s iOS kde jejich klepněte na Image na displeji zařízení, ale nepřímo z napříč místnosti pomocí [Siri vzdálené](#The-Siri-Remote).

Pokud je vaše aplikace hry, můžete volitelně vytvořit v podpora pro 3. stran provedené pro iOS (MFI s Připojením) [herní zařízení Bluetooth](#Bluetooth-Game-Controllers) ve vaší aplikaci také.

[![](remote-bluetooth-images/intro01.png "Bluetooth Remote a herní zařízení")](remote-bluetooth-images/intro01.png#lightbox)

Tento článek popisuje [Siri vzdálené](#The-Siri-Remote), [Touch gesta prostor](#Touch-Surface-Gestures) a [vzdáleného tlačítka Siri](#Siri-Remote-Buttons) a ukazuje, jak pracovat s nimi prostřednictvím [gesta a Scénářů](#Gestures-and-Storyboards), [gesta a kód](#Gestures-and-Code) a [zpracování nízké úrovně událostí](#Low-Level-Event-Handling). Nakonec popisuje [práce s herní zařízení](#Working-with-Game-Controllers) v Xamarin.tvOS aplikaci.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Vzdálené Siri

Hlavní způsobu, jakým uživatelé budou interakci s Apple TV a aplikace Xamarin.tvOS je prostřednictvím zahrnuté vzdálené Siri. Apple určená vzdálené přemostění vzdálenost mezi uživatele uložený na pohovce a Apple televizoru uživatelské rozhraní zobrazí napříč místnosti na obrazovce TV.

Vaše výzvy jako vývojář aplikace tvOS je rychlý, snadno použitelný a vizuálně poutavé uživatelské rozhraní, které využívá vzdálené Siri touch prostor, zrychlení, tlačítka a volný setrvačník vytvořit.

[![](remote-bluetooth-images/remote01.png "Vzdálené Siri")](remote-bluetooth-images/remote01.png#lightbox)

Vzdálené Siri má následující funkce a očekávané použití aplikace pro tvOS:

<table width="100%" border="1px">
<tr>
    <td><b>Funkce</b></td>
    <td><b>Obecné využití aplikací</b></td>
    <td><b>Herní využití aplikací</b></td>
</tr>
<tr>
    <td valign="top"><b>Prostor dotykového ovládání</b><br/>Prstem přejděte, stiskněte a vyberte přidržte kontextové nabídky.</td>
    <td valign="top"><b>Klepněte na nebo prstem:</b><br/>Uživatelské rozhraní navigace mezi položkami může získat fokus.<br/><br/><b>Klikněte na:</b><br/>Aktivuje vybrané položky (vybraný).</td>
    <td valign="top"><b>Klepněte na nebo prstem:</b><br/>Závisí na herní návrhu a může sloužit jako D-Pad klepnutím na okrajích.<br/><br/><b>Klikněte na:</b><br/>Provedení funkce primární tlačítko.</td>
</tr>
<tr>
    <td valign="top"><b>Nabídka</b><br/>Stisknutím vrátit na předchozí obrazovce nebo nabídku.</td>
    <td valign="top">Vrátí předchozí obrazovku a ukončí Apple TV Domů obrazovku na obrazovce hlavní aplikace.</td>
    <td valign="top">Pozastavení a obnovení hraní her, vrátí na předchozí obrazovku a ukončí Apple TV Domů obrazovku na obrazovce hlavní aplikace.</td>
</tr>
<tr>
    <td valign="top"><b>Siri nebo vyhledávání</b><br/>V zemích s Siri stiskněte a podržte pro řízení hlasové, ve všech dalších zemích, zobrazí obrazovky vyhledávání.</td>
    <td valign="top">není k dispozici</td>
    <td valign="top">není k dispozici</td>
</tr>
<tr>
    <td valign="top"><b>Přehrát či pozastavit</b><br/>Přehrávání a pozastavení média nebo poskytuje sekundární funkce v aplikacích.</td>
    <td valign="top">Spustí přehrávání médií a přehrávání pozastavení nebo obnovení.</td>
    <td valign="top">Provede funkci sekundární tlačítka nebo přeskočí video úvod (pokud existuje).</td>
</tr>
<tr>
    <td valign="top"><b>domácí</b><br/>Stisknutím klávesy se vraťte na domovskou obrazovku, dvakrát klikněte na Zobrazit spuštěné aplikace, stiskněte a podržte do režimu spánku zařízení.</td>
    <td valign="top">není k dispozici</td>
    <td valign="top">není k dispozici</td>
</tr>
<tr>
    <td valign="top"><b>Svazek</b><br/>Ovládací prvky připojit zařízení zvuku a videa svazku.</td>
    <td valign="top">není k dispozici</td>
    <td valign="top">není k dispozici</td>
</tr>
</table>

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Prostor gesta dotykového ovládání

Vzdálené Siri Touch prostor je schopna zjistit celou řadu gesta jedním prstem, které mohou reagovat na ve vaší aplikaci Xamarin.tvOS:

<table width="100%">
<tr>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture01.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture02.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture03.png"></td>
</tr>
<tr>
    <td valign="top"><b>Prstem:</b><br/>Přesune výběr (aktivní) mezi elementy uživatelského rozhraní na obrazovce (nahoru, dolů vlevo, vpravo). Procházejte velké seznam obsahu rychle pomocí nečinnosti lze použít k načtení.</td>
    <td valign="top"><b>Klikněte na:</b><br/>Aktivuje vybrané položky (vybraný) nebo funguje jako primární tlačítko ve hře. Kliknutím a podržením můžete aktivovat kontextové nabídky nebo sekundární funkce.</td>
    <td valign="top"><b>Klepněte na:</b><br/>Lehce klepnutím na povrch Touch na okrajích funguje jako směrovou tlačítka na D-list, Přesun zaměření nahoru, dolů, doleva nebo doprava v závislosti na oblasti stisknuté. V závislosti na aplikaci můžete použít na nich skrytý ovládací prvky.</td>
</tr>
</table>

Apple poskytuje následující návrhy pro práci s gesta Touch prostor:

* **Rozlišit mezi kliknutí a odposlouchávání** – kliknutím na tlačítko je úmyslné zásah uživatele a je dobře hodí pro výběr, aktivace a tlačítko primární hry. Klepnutím na další menší je a měli šetřit vzhledem k tomu, že uživatel je často podržte vzdálené Siri v jejich ruční a může nechtěně aktivovat snadno klepněte na události.
* **Nemusíte znovu definovat standardní gesta** -uživatel má očekává, že konkrétní gesta bude provádět určité akce, by neměl měníte význam nebo funkce tyto gesta ve vaší aplikaci. Jedinou výjimkou je herní aplikace během hraní v aktivním režimu.
* **Definovat pouze nové gesta** -znovu, má uživatel očekává, že konkrétní gesta bude provádět určité akce. Vyhněte se definování vlastních gest k provádění akcí standardní. A znovu, hry jsou obvykle výjimky, kde vlastních gest zábavné, dokonalé play přidat na příslušnou hru.
* **V případě potřeby reagovat na klepne na řídicí** – lehkým klepnutím na rohu okraje plochy Touch bude reagovat jako D Pad na přesunutí fokus herní zařízení nebo směrem nahoru, dolů, left nebo pravým. V případě potřeby má odpovědět na tyto gesta v aplikaci nebo hra.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Vzdálené Siri tlačítka

Kromě gesta na povrchu Touch můžete aplikaci reagovat na uživatel kliknutím na povrchu Touch nebo stiskněte tlačítko Přehrát či pozastavit. Při přístupu ke vzdálené Siri pomocí rozhraní herní řadiče, můžete také zjistit se stisknutí tlačítka nabídky. 

Kromě toho stisknutí tlačítka nabídky můžete zjistit pomocí standard pro rozpoznávání gesto `UIKit` elementy. Pokud je zachytit se stisknutí tlačítka nabídky, budete mít na starost zavření aktuálního zobrazení a View Controller a vrátit k předchozí.

> [!IMPORTANT]
> **Poznámka:** byste měli **vždy** přiřadit tlačítko Přehrát či pozastavit na vzdáleném funkce. S funkční tlačítko můžete nastavit aplikaci vypadat porušený pro koncového uživatele. Pokud nemáte platnou funkcí pro toto tlačítko, přiřadíte stejnou funkci jako primární tlačítko (Touch prostor kliknutím).




<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Gesta a scénářů

Nejjednodušší způsob, jak pracovat s vzdálené Siri ve vaší aplikaci Xamarin.tvOS je přidání nástroje pro rozpoznávání gesto do zobrazení v Návrháři rozhraní.

Pokud chcete přidat funkce rozpoznávání gesta, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy návrháři rozhraní.
2. Přetáhněte **klepněte na rozpoznávání rukopisu gesto** z **knihovny** na zobrazení: 

    [![](remote-bluetooth-images/storyboard01.png "Pro rozpoznávání klepněte na gesto")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Zkontrolujte **vyberte** v **tlačítko** části **atribut Inspector**: 

    [![](remote-bluetooth-images/storyboard02.png "Zkontrolujte vyberte")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Vyberte** znamená bude odpovídat gesto uživatele kliknutím **Touch prostor** na vzdáleném Siri. Máte také možnost neodpovídá na požadavky **nabídky**, **přehrát či pozastavit**, **až**, **dolů**, **doleva** a **Vpravo** tlačítka.
5. V dalším kroku se propojit **akce** z **klepněte na rozpoznávání rukopisu gesto** a pojmenujte ji `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "Akce z rozpoznávání rukopisu gesto klepněte na")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Uložte změny a vrátit k sadě Visual Studio for Mac.

Upravit View Controller (například `FirstViewController.cs`) souboru a přidejte následující kód pro zpracování gesto se spustí:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md). Konkrétně [vytváření uživatelské rozhraní](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) a [psaní kódu s výstupy a akce](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) oddíly.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Gesta a kódu

Volitelně můžete vytvořit gesta přímo v kódu jazyka C# a přidejte je do zobrazení v uživatelském rozhraní. Například přidat řadu prstem gesto rozpoznávání, upravte View Controller a přidejte následující kód:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();    

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>Zpracování událostí nízké úrovně

Při vytváření vlastního typu na základě `UIKit` ve vaší aplikaci Xamarin.tvOS (například `UIView`), máte také možnost využívat nízké úrovně zpracování stiskněte tlačítko prostřednictvím `UIPress` události. 

A `UIPress` událostí je tvOS, co `UITouch` událostí je pro iOS, s výjimkou `UIPress` vrátí informace o tlačítko stiskem tlačítka na vzdáleném Siri nebo jiným připojenými zařízeními Bluetooth (jako herní řadič). `UIPress` události popisují se stisknutí tlačítka a jeho stav (Began, zrušení, změněno nebo ukončeno). 

Pro analogovým tlačítka na zařízení, jako Bluetooth herní zařízení `UIPress` také vrátí hodnotu množství síla pro tlačítko. `Type` Vlastnost `UIPress` událostí definuje, které fyzické tlačítko obsahuje změněné stav, při posunování vlastnosti popisují změnu, která došlo k chybě.

Následující kód ukazuje příklad zpracování nízké úrovně `UIPress` událostí `UIView`:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region 
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

Stejně jako u `UITouch` událostí, pokud potřebujete implementovat některé z `UIPress` přepsání událostí, měli byste implementovat všechny čtyři.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Herní Bluetooth zařízení

Kromě standardní vzdálené Siri, který se dodává s Apple TV, 3. stran, provedené pro iOS můžete spárovaný s Apple TV (MFI s Připojením) Bluetooth herní zařízení a používat k ovládání Xamarin.tvOS aplikace.

[![](remote-bluetooth-images/game01.png "Herní Bluetooth zařízení")](remote-bluetooth-images/game01.png#lightbox)

Herní zařízení slouží k vylepšení hraní her a poskytnout představu o záchranných ve hře. Můžete také používají k řízení standardní rozhraní Apple TV, takže použití nemá přepínat mezi vzdálených a řadič.

> [!IMPORTANT]
> **Poznámka:** Bluetooth herní zařízení jsou volitelné nákupu, které by mohly způsobit koncovým uživatelům, aplikace nemůže vynutit uživateli zakoupit. Pokud vaše aplikace podporuje herní zařízení se musí taky podporovat vzdálené Siri tak, aby hra funkční všichni uživatelé Apple TV.


Řadič herní má následující funkce a očekávané použití aplikace pro tvOS:
<table width="100%" border="1px">
<tr>
    <td><b>Funkce</b></td>
    <td><b>Obecné využití aplikací</b></td>
    <td><b>Herní využití aplikací</b></td>
</tr>
<tr>
    <td valign="top"><b>D-Pad</b></td>
    <td valign="top">Přejde na prvky uživatelského rozhraní (fokus změny).</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>A</b></td>
    <td valign="top">Aktivuje vybrané položky (vybraný).</td>
    <td valign="top">Provede funkci primární tlačítka a potvrdí, dialogové okno akce.</td>
</tr>
<tr>
    <td valign="top"><b>B</b></td>
    <td valign="top">Vrátí na předchozí obrazovku nebo ji opustí na domovské obrazovce, pokud na obrazovce hlavní aplikace.</td>
    <td valign="top">Provede funkci sekundární tlačítko nebo vrátí na předchozí obrazovku.</td>
</tr>
<tr>
    <td valign="top"><b>X</b></td>
    <td valign="top">Spustí přehrávání médií nebo pozastavení nebo obnoví přehrávání.</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>Y</b></td>
    <td valign="top">není k dispozici</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>Nabídka</b></td>
    <td valign="top">Vrátí na předchozí obrazovku nebo ji opustí na domovské obrazovce, pokud na obrazovce hlavní aplikace.</td>
    <td valign="top">Pozastavení nebo obnovení hraní her, vrátí na předchozí obrazovku nebo ukončí na domovské obrazovce Pokud na obrazovce hlavní aplikace.</td>
</tr>
<tr>
    <td valign="top"><b>Levé osazení tlačítko</b></td>
    <td valign="top">Přejde na levé straně.</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>Levé aktivační události</b></td>
    <td valign="top">Přejde na levé straně.</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>Tlačítko správné osazení</b></td>
    <td valign="top">Přejde vpravo.</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>Pravé aktivační události</b></td>
    <td valign="top">Přejde vpravo</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>Levé Thumbstick</b></td>
    <td valign="top">Přejde na prvky uživatelského rozhraní (fokus změny).</td>
    <td valign="top">Závisí na hra.</td>
</tr>
<tr>
    <td valign="top"><b>Pravé Thumbstick</b></td>
    <td valign="top">není k dispozici</td>
    <td valign="top">Závisí na hra.</td>
</tr>
</table>

Apple poskytuje následující návrhy pro práci s herní zařízení:

- **Potvrďte herní řadič připojení** -tvOS aplikace může být spuštění a zastavení kdykoli koncovým uživatelem. Vždy zkontrolujte přítomnost řadič herní při spuštění aplikace nebo vzhůru časy a provádět potřebné kroky.
- **Ujistěte se, vaše aplikace funguje na vzdálené Siri a herní zařízení** -nevyžadují uživatelé přepínat mezi vzdálené Siri a herní řadiče používají vaši aplikaci. Testování aplikace často s oběma typy řadičů pro zajištění, že vše je snadné přejděte a funguje podle očekávání.
- **Zadejte zpět způsob** -stisknutí tlačítka nabídky vždy vrátit na předchozí obrazovku. Pokud je uživatel na obrazovce hlavní aplikace, by měl je k obrazovce Apple TV Domů vrátit tlačítka nabídky. Během hraní her tlačítko nabídky by měl zobrazit výstrahu, že uživatel umožňuje pozastavit nebo obnovit hraní her nebo vrátit do hlavní nabídky.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Práce s herní zařízení

Jak jsme uvedli výše, kromě standardní, které můžete volitelně připojit vzdálené Siri, který se dodává s Apple TV, uživatele 3. stran, provedené pro iOS (MFI s Připojením) Bluetooth herní zařízení a použít ho k řízení Xamarin.tvOS aplikace.

Nízké úrovně řadiče vstup potřeby vaší aplikace můžete používá společnosti Apple [herní řadič Framework](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) jehož následujících úprav pro tvOS:

- Profil Kontroleru herní Micro (`GCMicroGamepad`) byl přidán do cíle vzdálené Siri.
- Nové `GCEventViewController` třídu lze použít ke směrování herní zařízení události prostřednictvím vaší aplikace. Najdete v článku [určení vstup řadiče herní](#Determining-Game-Controller-Input) části níže další podrobnosti.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Požadavky na podporu herní zařízení

Apple má několik specifické požadavky, které musí být splněny, pokud vaše aplikace Xamarin.tvOS podporuje herní zařízení:

- **Musí podporovat vzdálené Siri** -vždy musí podporovat vzdálené Siri. Vaše hra nemohou vyžadovat 3. stran herní zařízení být možné přehrát.
- **Musí podporovat rozšířené rozložení ovládacích prvků** – všechny tvOS herní zařízení jsou bez formfitting, rozšířené řadiče.
- **Hry musí být Playable s řadiči samostatné** – Pokud vaše aplikace podporuje řadič rozšířené hra, musí být možné přehrát výhradně se herní Kontroleru.
- **Musí podporovat tlačítko Přehrát či pozastavit** -během hraní her, pokud uživatel stiskne tlačítko Přehrát či pozastavit by měl zobrazit výstrahu, že umožňuje hraní her v režimu pozastavení nebo obnovení, uživatele nebo vrátit do hlavní nabídky.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Povolení podpory herní zařízení

Chcete-li povolit podporu řadiče herní ve vaší aplikaci Xamarin.tvOS, dvakrát klikněte na `Info.plist` v soubor **Průzkumníku řešení** otevřete pro úpravy:

[![](remote-bluetooth-images/game02.png "Info.plist editor")](remote-bluetooth-images/game02.png#lightbox)

V části **herní řadič** část, zaškrtněte podle **Povolit herní zařízení**, zkontrolujte všechny typy herní řadiče, které bude podporovat aplikace.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Použití vzdáleného Siri jako herní zařízení

Vzdálené Siri, které jsou součástí Apple TV slouží jako řadič omezená hra. Jako další herní zařízení se zobrazí v prostředí řadiče herní jako `GCController` objekt a podporuje `GCMotion` a `GCMicroGamepad` profily.

Při použití jako řadič herní, vzdálené Siri má následující vlastnosti:

- Prostor Touch slouží jako D-plocha, která poskytuje analogovým vstupní data.
- Vzdálená mohou být používány výšku nebo na šířku a aplikace rozhodne, pokud objekt profil by měl překlopit vstupní data automaticky.
- Kliknutím na povrchu Touch chová jako stisknutím tlačítka **A** na řadiči hra.
- Tlačítko Přehrát či pozastavit pracuje stejně jako tlačítko **X** na řadiči hra.
- Tlačítko nabídky by měl zobrazit výstrahu, že uživatel umožňuje pozastavit nebo obnovit hraní her nebo vrátit do hlavní nabídky.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Určení vstup herní zařízení

Na rozdíl od iOS, kde můžete přijímat události herní řadiče paralelně s událostmi dotykového ovládání, tvOS zpracovává všechny nízké úrovně události, k poskytování vysoké úrovně `UIKit` události. Výsledkem je, pokud budete potřebovat přístup k nízké úrovni události řadiče hra, musíte vypnout `UIKit`je výchozí chování.

Na tvOS, pokud chcete zpracovat vstup herní zařízení přímo budete muset použít `GCEventViewController` (nebo podtřídy) zobrazení hry je uživatelské rozhraní. Vždy, když `GCEventViewController` je *první respondér*, herní řadič vstup bude zachytit a doručit do vaší aplikace pomocí rozhraní herní řadiče.

Můžete použít `UserInteractionEnabled` vlastnost `GCEventViewController` třída k přepnutí jak zpracování a zpracovává události.

Informace o implementaci podporu herní řadiče, najdete v tématu společnosti Apple [práce s herní zařízení](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) tématu [aplikace Průvodce programováním pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) a [herní řadiče Průvodce programováním](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých nové vzdálené Siri, který se dodává s Apple TV, gesta Touch prostor a vzdálené Siri tlačítka. V dalším kroku zahrnutých práci s gesta a scénářů, gesta a kódu a nízké úrovně události. Navíc pokud popsané práce herní zařízení.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
