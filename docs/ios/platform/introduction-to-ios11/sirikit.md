---
title: SiriKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/07/2017
ms.openlocfilehash: 6331912eedcd52df45c0d25d83a5b599c55ca7d2
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="sirikit"></a>SiriKit

SiriKit byla zavedena v iOS 10 s číslem domén služby (včetně cvičení nebo, pravé rezervace a volání). Odkazovat [SiriKit části](~/ios/platform/sirikit/index.md) SiriKit koncepty a postupy pro implementaci SiriKit ve vaší aplikaci.

![Ukázkový seznamu úkolů Siri](sirikit-images/sirikit.png)

SiriKit v iOS 11 přidá tyto nové a aktualizované záměrné domény:

- [**Obsahuje seznam a poznámky k** ](#listsnotes) – nové! Poskytuje rozhraní API pro aplikace pro zpracování úkolů a poznámek.
- **Visual kódy** – nové! Siri můžete zobrazit kódy QR sdílet kontaktní údaje ani účastnit platebních transakcí.
- **Platby** – přidat záměry vyhledávání a přenosu pro platbu interakce.
- **Přepravují rezervace** – přidání zrušit záměry pravé a zpětné vazby.

Mezi další nové funkce patří:

- [**Názvy aplikací alternativní** ](#alternativenames) – poskytuje aliasy, které pomáhají zákazníkům říct Siri prostřednictvím nabídky alternativní názvy nebo výslovnosti cíle vaší aplikace.
- **Spouštění cvičení nebo** – poskytuje možnost start cvičení na pozadí.

Některé z těchto funkcí jsou vysvětleny níže. Další podrobnosti pro ostatní [společnosti Apple SiriKit dokumentaci](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Seznamy a poznámky

Nové seznamy a poznámky k doméně poskytuje rozhraní API pro aplikace pro zpracování úkolů a poznámek prostřednictvím Siri hlasové požadavků.

**Úlohy**

- Máte název a stav dokončení.
- Volitelně můžete zahrnout konečným termínem a umístění.

**Poznámky**

- Máte název a pole obsahu.

Úkoly a poznámky můžete uspořádat do skupin. Zbývající část tohoto oddílu popisuje, jak implementovat toto nové doméně se SiriKit, pomocí [TasksNotes SiriKit příklad](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/).

### <a name="how-to-process-a-sirikit-request"></a>Tom, jak zpracovat žádost o SiriKit

Zpracovat žádost o SiriKit pomocí následujících kroků:

1. **Vyřešte** – ověření parametrů a vyžadovat další informace od uživatele (v případě potřeby).
2. **Potvrďte** – poslední ověření a ověření, že může požadavek zpracovat.
3. **Zpracování** – provedení operace (aktualizace dat nebo provádění síťových operací).

První dva kroky jsou volitelné (i když podporovat), a poslední krok je povinný.
Podrobnější pokyny v jsou [SiriKit části](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Vyřešte a potvrďte metody

Tyto metody volitelné umožní kódu provést ověření, vyberte výchozí hodnoty nebo Další informace o požadavku od uživatele.

Jako příklad pro `IINCreateTaskListIntent` rozhraní, je požadovaná metoda `HandleCreateTaskList`. Existují čtyři volitelné metody, které poskytují větší kontrolu nad interakce Siri:

- `ResolveTitle` – Ověřuje název, nastaví výchozí název (v případě potřeby) nebo signalizuje, že data se nevyžaduje.
- `ResolveTaskTitles` – Ověřuje seznam úloh oznamována uživatelem.
- `ResolveGroupName` – Ověřuje název skupiny, vybere výchozí skupinu nebo signalizuje, že data se nevyžaduje.
- `ConfirmCreateTaskList` – Ověří, že váš kód můžete provést požadovanou operaci, ale nebude provádět (pouze `Handle*` metody by měla upravovat data).

### <a name="handle-the-intent"></a>Popisovač záměr

Existují šesti záměry v doméně, seznamy a poznámky, tři pro úlohy a tři pro poznámky.
Metody, které je nutné implementovat pro zpracování těchto tříd Intent jsou následující:

- Pro úlohy:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Poznámky k:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Každá z metod má určitý záměrné typ předaný, který obsahuje všechny informace, které má Siri Analyzovaná z požadavku uživatele (a případně v `Resolve*` a `Confirm*` metody).
Aplikace musí analyzovat data poskytnutá, potom provést některé akce pro uložení nebo jinak proces data a vrátit výsledek, který Siri hovoří a zobrazí uživateli.

### <a name="response-codes"></a>Kódy odpovědí

Požadované `Handle*` a volitelné `Confirm*` metody označit kód odpovědi nastavením hodnoty na objektu, která se předají na jejich dokončení obslužnou rutinu. Odpovědi pocházet z `INCreateTaskListIntentResponseCode` výčtu:

- `Ready` – Vrátí ve fázi potvrzení (ie. z `Confirm*` metoda, ale nikoli z `Handle*` metoda).
- `InProgress` – Používá se pro dlouhotrvající úlohy (například síť nebo server operace).
- `Success` – Odpoví podrobnosti o úspěšnou operaci (pouze z `Handle*` metoda).
- `Failure` – Znamená, že došlo k chybě, a operaci nebylo možné dokončit.
- `RequiringAppLaunch` – Nelze zpracovat pomocí záměr, ale tuto operaci je možné v aplikaci.
- `Unspecified` – Nepoužívejte: Zobrazí se chybová zpráva pro uživatele.

Další informace o těchto metod a odpovědi v společnosti Apple [SiriKit uvádí a poznámky k dokumentaci](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Implementace seznamy a poznámky

[TasksNotes SiriKit příklad](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) byla vytvořena pomocí následujícího postupu přidat podporu SiriKit prázdnou aplikaci iOS.

Nejprve přidat podporu SiriKit, postupujte takto pro vaše aplikace pro iOS:

1. Značky **SiriKit** v **Entitlements.plist**.
2. Přidat **o ochraně osobních údajů – popis používání Siri** klíče na **Info.plist**, společně s zprávu pro vaše zákazníky.
3. Volání `INPreferences.RequestSiriAuthorization` metoda do aplikace, zobrazí výzva k povolení interakce Siri.
4. Přidejte SiriKit do vašeho ID aplikace na portálu pro vývojáře a znovu vytvořit váš zřizovacích profilů zahrnout nové nárok.

Pak přidejte nový projekt rozšíření do vaší aplikace pro zpracování žádostí Siri:

1. Klikněte pravým tlačítkem na řešení a zvolte **Přidat > Přidat nový projekt...** .
2. Vyberte **iOS > Rozšíření > rozšíření tříd Intent** šablony.
3. Přidá dva nové projekty: záměr a IntentUI. Přizpůsobení uživatelského rozhraní je volitelné, takže ukázku pouze obsahuje kód v **záměr** projektu.

Rozšíření projektu je, kde budou zpracovány všechny SiriKit požadavky. Jako samostatné přípony nemá automaticky jakkoli ke komunikaci s hlavní aplikace – to je obvykle vyřešeny implementace sdíleným úložištěm souborů pomocí skupin aplikací.

#### <a name="configure-the-intenthandler"></a>Konfigurace IntentHandler

`IntentHandler` Třída je vstupní bod pro požadavky Siri – každých záměr předána `GetHandler` metoda, která vrátí objekt, který dokáže zpracovat žádost.

Následující kód ukazuje jednoduchý implementace:

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

Třída musí dědit z `INExtension`, a protože vzorku bude zpracování seznamy a poznámky k záměry, také implementuje `IINNotebookDomainHandling`.

> [!NOTE]
> - Je v rozhraní .NET pro rozhraní obsahovat předponu na velké konvence `I`, které dodržuje Xamarin při vytváření vazby protokolů z iOS SDK.
> - Xamarin také zachovává názvy typů z iOS, a Apple používá první dva znaky v názvech typ tak, aby odrážela rozhraní, které patří typu.
> - Pro `Intents` framework, typy jsou předponou `IN*` (např. `INExtension`), ale toto jsou _není_ rozhraní.
> - Také vyplývá, že protokoly (které stát rozhraní v jazyce C#) skončili se dvěma `I`s, jako například `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>Zpracování záměry

Každý záměr (Přidat úloha, nastavte atribut úkol atd.) je implementovaná v jedné metody podobné vidíte níže. Metoda by měla provést tři hlavní funkce:

1. **Zpracování záměr** – data analyzovat pomocí Siri je k dispozici v `intent` objektu, které jsou specifické pro daný typ záměr. Aplikace může mít ověření dat pomocí volitelné `Resolve*` metody.
2. **Ověření a aktualizovat úložiště dat** – ukládání dat na systém souborů (pomocí skupin aplikací, se kterým je aplikace pro iOS hlavní můžete také), nebo prostřednictvím žádosti o síti.
3. **Zadejte odpověď** – pomocí `completion` obslužnou rutinu k odeslání odpověď zpět do Siri pro čtení nebo zobrazení pro uživatele:

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

Všimněte si, že `null` je předána jako druhý parametr odpovědi – toto je parametr aktivity uživatele, a když ho není zadaný, použije výchozí hodnotu.
Typ vlastní aktivity můžete nastavit tak dlouho, dokud ji prostřednictvím podporuje vaší aplikace pro iOS `NSUserActivityTypes` klíče v **Info.plist**. Potom můžete zpracovávat tento případ při otevření aplikace a provádět určité operace (například otevírání na příslušné zobrazení kontroler a načítání dat z operace Siri).

Tento příklad také hardcodes `Success` výsledek, ale ve skutečných scénářích měli přidat správné zasílání zpráv o chybách.

### <a name="test-phrases"></a>Test fráze

Následujících frází Zkouška by měla fungovat v ukázkovou aplikaci:

- "Zkontrolujte seznam supermarketu s jablek, banánů a hrušek v TasksNotes"
- "Přidat úloha WWDC v TasksNotes"
- "Přidat úloha WWDC v TasksNotes seznamu školení"
- "Označit účastní WWDC jako dokončení v TasksNotes"
- "V TasksNotes připomenout koupit zařízení typu iphone při doručení Domů"
- "Označit koupit iPhone jako dokončené v TasksNotes"
- "Připomeňte mi opustit Domů v 8: 00 v TasksNotes"

![Vytvořit nový příklad seznamu](sirikit-images/createtasklist-sml.png) ![Úloha sady jako kompletní příklad](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 simulátoru podporuje testování pomocí Siri (na rozdíl od předchozích verzí).
>
> Pokud testujete na skutečné zařízení, nezapomeňte nakonfigurovat vaše ID aplikace a profily pro podporu SiriKit zřizování.

<a name="alternativenames" />

## <a name="alternative-names"></a>Alternativní názvy

Tato nová funkce iOS 11 znamená, že můžete nakonfigurovat alternativní názvy pro vaši aplikaci, aby pomohla uživatelům aktivovat správně s Siri. Přidejte následující klíče **Info.plist** souboru projektu aplikace pro iOS:

![Info.plist zobrazující aplikace alternativní název klíče a hodnoty](sirikit-images/alternative-names.png)

Názvy sadou alternativní aplikaci, bude následujících frází také fungovat pro ukázkovou aplikaci (což je ve skutečnosti s názvem **TasksNotes**):

- "Vytvořit seznam supermarketu s jablek, banánů a hrušek v _MonkeyNotes_"
- "Přidat úloha WWDC v _MonkeyTodo_"


## <a name="troubleshooting"></a>Poradce při potížích

Některé chyby, které se mohou vyskytnout při spuštění ukázky nebo přidání SiriKit do vlastní aplikace:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Došlo k výjimce jazyka Objective-C.  Název: NSInternalInconsistencyException důvod: použití třídy < INPreferences: 0x60400082ff00 > z aplikace vyžaduje com.apple.developer.siri nárok. Povolili jste Siri funkce ve vašem projektu Xcode?_

- SiriKit je zaškrtnuta ve **Entitlements.plist**.
- **Entitlements.plist** je nakonfigurovaný v **možnosti projektu > sestavení > iOS podepisování sady**.

  [![Možnosti projektu zobrazující, že správně nastavena oprávnění](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (pro nasazení zařízení) ID aplikace má SiriKit povolené a profil pro zřizování stáhli.


## <a name="related-links"></a>Související odkazy

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [Ukázka SiriKit TasksNotes](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [Co je nového v SiriKit (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/214/)
