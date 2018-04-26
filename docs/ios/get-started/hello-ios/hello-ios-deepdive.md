---
title: 'Dobrý den, iOS: podrobné informace'
description: Tato příručka dvě části popisuje, jak vytvářet základní aplikace pro Xamarin.iOS pomocí sady Visual Studio pro Mac nebo Visual Studio a pochopili jejich základní informace o vývoj aplikací pro iOS pomocí Xamarin. Zavede nástroje, koncepty a kroky potřebné k sestavení a nasazení aplikace pro Xamarin.iOS.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 71bfccdcab73b651f458dd8d9c5396bffd55004b
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="hello-ios-deep-dive"></a>Hello, iOS podrobné informace

Návod rychlý start, zavedl sestavení a spuštění základní aplikace pro Xamarin.iOS. Nyní je čas k vývoji lépe pochopili, jak fungují aplikace pro iOS, můžete vytvořit složitější programy. Tento průvodce zkontroluje kroky v Hello, návodu iOS Principy základní koncepty pro vývoj aplikací pro iOS.

V tomto článku jsou prozkoumali v následujících tématech:


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- **Úvod k sadě Visual Studio pro Mac** – Úvod k sadě Visual Studio pro Mac a vytvoření nové aplikace.
- **Anatomie aplikace pro Xamarin.iOS** -nezbytné součásti aplikace Xamarin.iOS.
- **Architektura a aplikace Základy** – kontrolu částí aplikace pro iOS a vztahů mezi nimi.
- **Uživatelské rozhraní (UI)** – vytvoření uživatelského rozhraní pomocí návrháře iOS.
- **Zobrazení řadičů a životního cyklu zobrazení** – Úvod do životního cyklu zobrazení a správa obsahu zobrazení hierarchie s řadiče zobrazení.
- **Testování, nasazení a všechny změny** -dokončení vaší aplikace pomocí Rady k testování, nasazení, generování kresby a další.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **Úvod k sadě Visual Studio** – Úvod k sadě Visual Studio a vytvoření nové aplikace.
- **Anatomie aplikace pro Xamarin.iOS** -nezbytné součásti aplikace Xamarin.iOS.
- **Architektura a aplikace Základy** – kontrolu částí aplikace pro iOS a vztahů mezi nimi.
- **Uživatelské rozhraní (UI)** – vytvoření uživatelského rozhraní pomocí návrháře iOS.
- **Zobrazení řadičů a životního cyklu zobrazení** – Úvod do životního cyklu zobrazení a správa obsahu zobrazení hierarchie s řadiče zobrazení.
- **Testování, nasazení a všechny změny** -dokončení vaší aplikace pomocí Rady k testování, nasazení, generování kresby a další.

-----

Tento průvodce vám pomůže vytvořit dovednosti a znalosti potřebné k vytvoření aplikace iOS jedním obrazovky. Po pracovní jeho prostřednictvím, byste měli mít k pochopení různých součástí aplikace pro Xamarin.iOS a jak je umístit společně.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Úvod k sadě Visual Studio pro Mac

Visual Studio pro Mac je zdarma, který open-source IDE, které se kombinuje funkce ze sady Visual Studio a XCode. Jeho součástí jsou plně integrované vizuálního návrháře, textový editor, který je kompletní s refaktoringu nástroje, sestavení prohlížeče, integrace zdrojového kódu a další. Tento průvodce uvádí některé základní Visual Studio pro Mac funkce, ale pokud začínáte k sadě Visual Studio pro Mac, podívejte se na [Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/) dokumentaci.

Visual Studio pro Mac následuje postup sadě Visual Studio uspořádání kód do *řešení* a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (například iOS nebo Android), pomocné knihovny, testovací aplikace a další. V aplikaci Phoneword byl přidán nový iPhone projekt pomocí **jediné zobrazení aplikace** šablony. Počáteční řešení hledá takto:

![](hello-ios-deepdive-images/image30.png "Snímek obrazovky prvotního řešení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Úvod k sadě Visual Studio

Visual Studio je výkonný IDE od společnosti Microsoft. Jeho součástí jsou plně integrované vizuálního návrháře, textový editor, který je kompletní s refaktoringu nástroje, sestavení prohlížeče, integrace zdrojového kódu a další. Tato příručka zavádí některé základní funkce sady Visual Studio s Xamarinem modulu plug-in.

Visual Studio organizuje kód do _řešení_ a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (například iOS nebo Android), pomocné knihovny, testovací aplikace a další. V aplikaci Phoneword byl přidán nový iPhone projekt pomocí **jediné zobrazení aplikace** šablony. Počáteční řešení hledá takto:

![](hello-ios-deepdive-images/vs-image30.png "Snímek obrazovky prvotního řešení")

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinios-application"></a>Anatomie aplikace pro Xamarin.iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Na levé straně je *řešení Pad*, který obsahuje strukturu adresáře a všechny soubory přidružené k řešení:

![](hello-ios-deepdive-images/image31.png "Odsazení řešení, která obsahuje strukturu adresáře a všechny soubory přidružené k řešení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Na pravé straně je *řešení podokně*, který obsahuje strukturu adresáře a všechny soubory přidružené k řešení:

![](hello-ios-deepdive-images/vs-image31.png "V podokně řešení, která obsahuje strukturu adresáře a všechny soubory přidružené k řešení")

-----

V [Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) návodu jste vytvořili řešení názvem **Phoneword** a umístit iOS projekt – **Phoneword_iOS** – uvnitř ho. Položky v rámci projektu patří:

-  **Odkazy na** – obsahuje sestavení požadovaná sestavení a spuštění aplikace. Rozbalte adresář zobrazíte odkazy na sestavení .NET [systému](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx) , System.Core a [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx) , a také odkaz na sestavení Xamarin pro Xamarin.iOS.
-  **Balíčky** -adresář balíčky obsahuje předem připravené balíčky NuGet.
-  **Prostředky** -složku prostředky ukládá další média.
-  **Main.cs** – obsahuje hlavní vstupní bod aplikace. Ke spuštění aplikace, název třídy hlavní aplikace, `AppDelegate`, je předaná.
-  **AppDelegate.cs** – tento soubor obsahuje třídu hlavní aplikace a zodpovídá za vytvoření okna, vytvoření uživatelského rozhraní a naslouchá událostem z operačního systému.
-  **Main.Storyboard** -The Storyboard obsahuje visual návrh uživatelského rozhraní pro aplikace. Scénáře soubory otevřete v grafického editoru názvem iOS návrháře.
-  **ViewController.cs** – View Controller pohání obrazovky (zobrazení), které uživatel vidí a dotykem. Řadiče zobrazení je zodpovědná za zpracování interakce mezi uživateli a zobrazení.
-  **ViewController.designer.cs** – `designer.cs` je automaticky generovaný soubor, který slouží jako spojovací mezi ovládacími prvky v zobrazení a jejich vyjádření kódu v Kontroleru zobrazení. Protože se jedná o soubor interní vložení, rozhraní IDE přepíše jakékoli ruční změny a ve většině případů, které tento soubor můžete ignorovat. Další informace o vztah mezi vizuálního návrháře a kód zálohování, naleznete [Úvod do systému iOS Návrhář](~/ios/user-interface/designer/introduction.md) průvodce.
-  **Info.plist** – `Info.plist` je, kde jsou nastaveny vlastnosti aplikace, jako je například název aplikace, ikony, spouštěcí bitové kopie a další. Toto je výkonný soubor a je k dispozici v důkladné Úvod k němu [práce s seznamů vlastností](~/ios/app-fundamentals/property-lists.md) průvodce.
-  **Entitlements.plist** – seznam vlastností oprávnění umožňují nám zadejte aplikaci *možnosti* (také nazývané aplikace úložiště technologie) jako je iCloud, PassKit a další. Další informace o `Entitlements.plist` lze nalézt v [práce s seznamů vlastností](~/ios/app-fundamentals/property-lists.md) průvodce. Obecný úvod do oprávnění, najdete v části [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

## <a name="architecture-and-app-fundamentals"></a>Architektura a základní informace o aplikaci

Předtím, než aplikace pro iOS můžete načíst uživatelské rozhraní, musí být na místě dvě věci. Nejdřív je potřeba určit aplikace *vstupní bod* – první kód, který se spustí v případě, že proces aplikace je načten do paměti. Druhý je nutné definovat třídu pro zpracování událostí celou aplikaci a interakci s operačním systémem.

Tato část studie vztahy znázorněné v následujícím diagramu:

[![](hello-ios-deepdive-images/image32.png "Architektura a aplikace základy vztahy jsou zobrazené v tomto diagramu")](hello-ios-deepdive-images/image32.png#lightbox)

Můžeme začít na začátku a zjistěte, co se stane při spuštění aplikace.

### <a name="main"></a>Main

Hlavní vstupní bod aplikace pro iOS je `Main.cs` souboru. `Main.cs` obsahuje statickou metodu Main, který vytvoří novou instanci aplikace Xamarin.iOS a předává název *delegáta aplikace* třídu, která bude zpracovávat události operačního systému. Kód šablony pro statické `Main` metoda se zobrazí níže:

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="application-delegate"></a>Delegát aplikace

V iOS *delegáta aplikace* třída zpracovává události systému; Tato třída je umístěn uvnitř `AppDelegate.cs`. `AppDelegate` Třída spravuje aplikace *okno*. Okno je jedna instance `UIWindow` třídu, která slouží jako kontejner pro uživatelské rozhraní. Ve výchozím nastavení, aplikace získá pouze jedno okno, na kterém se načíst jeho obsah a okna je připojen k *obrazovky* (jeden `UIScreen` instance), který poskytuje ohraničující obdélník odpovídající dimenze fyzický obrazovky zařízení.

*AppDelegate* zodpovídá taky za odběr aktualizací systému o události důležité aplikace, například když aplikace dokončí, spuštění nebo při nedostatku paměti.

Kód šablony pro AppDelegate je uveden níže:

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

Jakmile aplikace má definovány její okno, můžete začít, načítání uživatelského rozhraní. Jsou zde popsány v další části Vytvoření uživatelského rozhraní.

## <a name="user-interface"></a>Uživatelské rozhraní

Uživatelské rozhraní aplikace iOS se podobá výkladní skříň – aplikace obvykle získá jeden interval, ale ho může zaplnit okno s jako mnoho objektů na to potřebuje, a objekty a uspořádání lze změnit v závislosti na tom, co aplikace chce, zobrazte. Objekty v tomto scénáři - věcí, které uživatel vidí - se nazývají zobrazení. K vytvoření jedné obrazovky v aplikaci, jsou na sobě v skládaný zobrazení *obsahu zobrazení hierarchie*, a hierarchii spravuje jeden řadič zobrazení. Aplikace s více obrazovek mají více obsahu zobrazení hierarchie, každou s vlastní řadiče zobrazení a aplikace umístí v okně vytvořit jiné obsahu zobrazení hierarchie založené na obrazovce, jejímž je uživatel v zobrazení.

Tato část dives do uživatelského rozhraní prostřednictvím popisu zobrazení obsahu zobrazení hierarchie a iOS Designer.

### <a name="ios-designer-and-storyboards"></a>iOS Designer a scénářů

IOS Designer je nástroj visual pro vytváření uživatelského rozhraní v Xamarin. Návrhář může být spuštěn poklikáním na soubor všechny scénáře (.storyboard), který se otevře zobrazení, která se podobá následující snímek obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "iOS rozhraní návrháře")

A *Storyboard* je soubor, který obsahuje vizuální návrhy obrazovky naše aplikace a také přechody a vztahy mezi obrazovky. Reprezentace obrazovky aplikace v scénář nazývá _scény_. Každé scény představuje řadič zobrazení a zásobníku zobrazení spravuje (obsahu zobrazení hierarchie). Pokud nový **jediné zobrazení aplikace** projektu je vytvořené ze šablony, Visual Studio pro Mac automaticky vygeneruje Storyboard soubor s názvem `Main.storyboard` a naplní s jedné scény, které jsou popsány v na snímku obrazovky níže:

![](hello-ios-deepdive-images/image34.png "Visual Studio pro Mac automaticky generuje Storyboard soubor s názvem Main.storyboard a naplní s jedné scény")

Zvolit řadiče zobrazení pro scény lze vybrat černým panelu v dolní části obrazovky scénáře. Představuje instanci řadiče zobrazení `UIViewController` třídu, která obsahuje kód zálohování pro hierarchii zobrazení obsahu. Vlastnosti na tento řadič zobrazení můžete zobrazit a nastavit uvnitř **vlastnosti Pad**, které jsou popsány v následující snímek obrazovky:

![](hello-ios-deepdive-images/image35.png "V podokně vlastností")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "iOS rozhraní návrháře")

A *Storyboard* je soubor, který obsahuje vizuální návrhy obrazovky naše aplikace a také přechody a vztahy mezi obrazovky. Reprezentace obrazovky aplikace v scénář nazývá _scény_. Každé scény představuje řadič zobrazení a zásobníku zobrazení spravuje (obsahu zobrazení hierarchie). Pokud nový **jediné zobrazení aplikace** projektu je vytvořené ze šablony, Visual Studio automaticky vygeneruje Storyboard soubor s názvem `Main.storyboard` a naplní s jedné scény, které jsou popsány v následující snímek obrazovky:

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio automaticky generuje Storyboard soubor s názvem Main.storyboard a naplní s jedné scény")

Na panelu v dolní části obrazovky Storyboard lze vybrat zvolit řadiče zobrazení pro scény. Představuje instanci řadiče zobrazení `UIViewController` třídu, která obsahuje kód zálohování pro hierarchii zobrazení obsahu. Vlastnosti na tento řadič zobrazení můžete zobrazit a nastavit uvnitř **podokno Properties**, které jsou popsány v následující snímek obrazovky:

![](hello-ios-deepdive-images/vs-image35.png "V podokně vlastností")

-----

_Zobrazení_ lze vybrat kliknutím uvnitř bílé část scény. Zobrazení je instance `UIView` třídu, která definuje oblast na obrazovce a poskytuje rozhraní pro práci s obsahem v této oblasti. Výchozí zobrazení je jediný *kořenovému zobrazení* , vyplní celé obrazovce celého zařízení.

Nalevo od scény je šedé šipka s ikonou vlajky, které jsou popsány v následující snímek obrazovky:

 [![](hello-ios-deepdive-images/image37.png "Šedé šipka doplněná ikonou vlajky")](hello-ios-deepdive-images/image37.png#lightbox)

Šedé šipku představuje přechod Storyboard názvem *Segue* (vyslovováno "seg – způsob"). Vzhledem k tomu, že tento Segue nemá žádné původu, se nazývá *Sourceless Segue*. Sourceless Segue odkazuje na první scény, jejichž zobrazení bude načten do okna naše aplikace při spuštění aplikace. Scény a zobrazení v jeho bude první věc, kterou uživatel uvidí, když aplikace načte.

Při vytváření uživatelského rozhraní, další zobrazení může být přetažen z **sada nástrojů** do hlavního zobrazení na návrhovou plochu, které jsou popsány v následující snímek obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "Další zobrazení můžete přetáhnout z panelu nástrojů na hlavní zobrazení na návrhovou plochu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "Další zobrazení můžete přetáhnout z panelu nástrojů na hlavní zobrazení na návrhovou plochu")

-----

Tyto další zobrazení, se nazývají *dílčích zobrazení*. Společně kořenové zobrazení a dílčích zobrazení jsou součástí *obsahu zobrazení hierarchie* , který je spravován nástrojem `ViewController`. Obrys všechny elementy ve scény lze zobrazit tak, že prověří ho **Osnova dokumentu** odsazení:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "Osnova dokumentu odsazení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "Osnova dokumentu odsazení")

-----

V následujícím diagramu jsou vyznačené dílčích zobrazení:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "V diagramu jsou vyznačené dílčích zobrazení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "V diagramu jsou vyznačené dílčích zobrazení")

-----

Dojde-li hierarchii obsahu zobrazení reprezentována tento scény v další části.

## <a name="content-view-hierarchy"></a>Zobrazení obsahu hierarchie

A _obsahu zobrazení hierarchie_ je zásobník zobrazení a dílčích zobrazení spravuje jeden řadič zobrazení, které jsou popsány v následujícím diagramu:

 [![](hello-ios-deepdive-images/image41.png "Hierarchie zobrazení obsahu")](hello-ios-deepdive-images/image41.png#lightbox)

Můžeme provádět hierarchii zobrazení obsahu naše `ViewController` snazší zjistit tak, že dočasně změníte barvu pozadí kořenové zobrazení na žlutou v sekci zobrazení z **vlastnosti Pad**, které jsou popsány v následující snímek obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "Změna barvy pozadí kořenové zobrazení na žlutý v části zobrazit vlastnosti odsazení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "Změna barvy pozadí kořenové zobrazení na žlutý v části zobrazit vlastnosti odsazení")

-----

Následující obrázek znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a View Controller, které přinášejí uživatelského rozhraní na obrazovce zařízení:

 [![](hello-ios-deepdive-images/image43.png "Vztahy mezi oken, zobrazení, dílčích zobrazení a View Controller")](hello-ios-deepdive-images/image43.png#lightbox)

V další část popisuje, jak pracovat se zobrazeními v kódu a naučte se programovat pro interakci s uživatelem pomocí řadiče zobrazení a zobrazení životního cyklu.

## <a name="view-controllers-and-the-view-lifecycle"></a>Zobrazení řadičů a životního cyklu zobrazení

Každý obsahu zobrazení hierarchie má odpovídající řadič zobrazení do interakce s uživatelem napájení. Role řadiče zobrazení je Správa zobrazení v hierarchii zobrazení obsahu. Řadiče zobrazení není součástí obsahu zobrazení hierarchie a není element v rozhraní. Místo toho poskytuje kód, který zajišťuje interakce uživatele s objekty, na obrazovce.

### <a name="view-controllers-and-storyboards"></a>Zobrazení řadičů a scénářů

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Řadiče zobrazení je reprezentován v scénář panelu v dolní části scény. Výběr řadiče zobrazení zobrazí jeho vlastnosti v **Pad vlastnosti**:

![](hello-ios-deepdive-images/image44.png "Výběr řadiče zobrazení zobrazí jeho vlastnosti v podokně Vlastnosti.")

Vlastní třída řadiče zobrazení pro hierarchii obsahu zobrazení reprezentována tento scény můžete nastavit úpravou **– třída** vlastnost v **Identity** části **vlastnosti Pad**. Například naše **Phoneword** sady aplikace `ViewController` jako řadič zobrazení pro naše první obrazovce, které jsou popsány v následující snímek obrazovky:

![](hello-ios-deepdive-images/image45new.png "Aplikace Phoneword nastaví ViewController jako řadiče zobrazení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Řadiče zobrazení je reprezentován v scénář panelu v dolní části scény. Výběr řadiče zobrazení zobrazí jeho vlastnosti v **podokno Properties**:

![](hello-ios-deepdive-images/vs-image44.png "Výběr řadiče zobrazení zobrazí jeho vlastnosti v podokně Vlastnosti.")

Vlastní třída řadiče zobrazení pro hierarchii obsahu zobrazení reprezentována tento scény můžete nastavit úpravou **– třída** vlastnost v **Identity** části **podokno Properties**. Například naše **Phoneword** sady aplikace `ViewController` jako řadič zobrazení pro naše první obrazovce, které jsou popsány v následující snímek obrazovky:

![](hello-ios-deepdive-images/vs-image45.png "Aplikace Phoneword nastaví ViewController jako řadiče zobrazení")

-----

Tím propojí reprezentace Storyboard řadiče zobrazení na `ViewController` třída jazyka C#. Otevřete `ViewController.cs` souboru a Všimněte si řadič zobrazení *podtřídami* z `UIViewController`, které jsou popsány v následující kód:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

`ViewController` Nyní jednotky interakce obsahu zobrazení hierarchie přiřazených k tomuto Kontroleru zobrazení ve scénáři. Dále získáte informace o řadiče zobrazení role ve správě zobrazení zavedením procesu označovaného jako životního cyklu zobrazení.

> [!NOTE]
> Pro jen visual obrazovky, která nevyžaduje zásah uživatele **třída** vlastnost může být ponecháno prázdné v **Pad vlastnosti**. To nastaví základní třídu řadiče zobrazení jako výchozí implementaci `UIViewController`, což je vhodné, pokud neplánujete na přidání vlastní kód.

### <a name="view-lifecycle"></a>Životní cyklus zobrazení

Řadiče zobrazení má na starosti načítání a uvolňování obsahu zobrazení hierarchie z okna. Když se stane něco význam zobrazení v hierarchii zobrazení obsahu, oznámí operačního systému řadiče zobrazení prostřednictvím událostí v průběhu životního cyklu zobrazení. Přepsáním metody v průběhu životního cyklu zobrazení můžete pracovat s objekty, na obrazovce a vytvořit dynamické a reagující uživatelské rozhraní.

Toto jsou základní životního cyklu metody a jejich funkce:

-  **ViewDidLoad** – volané *po* poprvé načte řadiče zobrazení jeho obsahu zobrazení hierarchie do paměti. Toto je vhodná k počáteční instalaci, protože se jedná, kdy dílčích zobrazení nejdříve k dispozici v kódu.
-  **ViewWillAppear** -volá se pokaždé, když je zobrazení řadič zobrazení přidat do obsahu zobrazit hierarchii a na obrazovce.
-  **ViewWillDisappear** -volá se pokaždé, když je zobrazení View Controller Chystáte se odebrat z hierarchie zobrazení obsahu a zmizí z obrazovky. Tato událost životního cyklu se používá pro čištění a ukládání stavu.
-  **ViewDidAppear** a **ViewDidDisappear** -volá se při zobrazení získá přidat nebo odebrat z obsahu zobrazení hierarchie, v uvedeném pořadí.


Po přidání vlastního kódu do všechny fáze životního cyklu, že metoda životního cyklu na *základní implementaci* musí být *elementem*. Toho dosáhnete tak, že klepnete na do stávající metodu životního cyklu, který má nějaký kód už je připojený, a rozšíření s další kód. Základní implementaci je volána z uvnitř metody a ujistěte se, že původní kód běží před nový kód. Příkladem je znázorněn v další části.

Další informace o práci s řadiče zobrazení, najdete v části společnosti Apple [Průvodce programováním v zobrazení řadiče pro iOS](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/ViewLoadingandUnloading/ViewLoadingandUnloading.html) a [UIViewController odkaz](https://developer.apple.com/library/ios/documentation/uikit/reference/UIViewController_Class/Reference/Reference.html).

### <a name="responding-to-user-interaction"></a>Neodpovídá na požadavky interakci s uživatelem

Nejdůležitější roli řadiče zobrazení reaguje na interakci s uživatelem, jako je například stisknutí tlačítek, navigace a další. Nejjednodušší způsob, jak se zpracovávají interakci s uživatelem se má propojit až vstupní ovládací prvek pro naslouchání na uživatele a připojte obslužné rutiny události reagovat na vstup. Tlačítko například může být drátové až reagovat na událost dotykového ovládání, jak je předvedeno v aplikaci Phoneword.

Teď, když je mít lepší představu o zobrazeních a řadičích, zobrazení, podíváme, jak to funguje.
V `Phoneword_iOS` projektu, tlačítko byl přidán volané `TranslateButton` obsahu zobrazení hierarchie:

 [![](hello-ios-deepdive-images/image1.png "Tlačítko přidala volané TranslateButton obsahu zobrazení hierarchie")](hello-ios-deepdive-images/image1.png#lightbox)

Při **název** je přiřazena k **tlačítko** řídit ve **vlastnosti Pad**, návrháře iOS automaticky mapují do ovládacího prvku v  **ViewController.designer.cs**, které `TranslateButton` dostupné uvnitř `ViewController` třídy. Ovládací prvky nejdříve k dispozici ve `ViewDidLoad` fáze životního cyklu zobrazení, takže tato metoda životního cyklu se používá reagovat na touch uživatele:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Aplikace Phoneword používá touch událost související s názvem `TouchUpInside` pro naslouchání na uživatele touch. `TouchUpInside` čeká na touch až událostí (prstem zrušení z obrazovky), který následuje touch dolů (prstem klepnou na obrazovce) uvnitř hranice ovládacího prvku. Opakem `TouchUpInside` je `TouchDown` událost, která aktivuje se při stisknutí v ovládacím prvku. `TouchDown` Událost zaznamená spoustu šumu a umožňuje uživateli možnost zrušit stiskem klouzavé jejich prstem vypnout ovládací prvek. `TouchUpInside` je nejběžnější způsob, jak reagovat na **tlačítko** touch a vytvoří činnost koncového uživatele očekává při stisknutí tlačítka. Další informace o tomto je k dispozici ve společnosti Apple [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html).

Aplikace se zpracovávají `TouchUpInside` událost s lambda, ale delegáta nebo obslužná rutina s názvem událostí může také byly použity. Kód poslední tlačítko se podobaly následující:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty představené v Phoneword

Aplikace Phoneword zavedl několik konceptů, které nejsou zahrnuté v této příručce. Tyto koncepty patří:

- **Změňte Text tlačítka** – aplikace Phoneword ukázal, jak lze změnit text **tlačítko** voláním `SetTitle` na **tlačítko** a předávání v nového textu a  **Tlačítko**na _řízení stavu_. Například následující kód změní CallButton text na "Volání":

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **Povolení a zakázání tlačítka** – **tlačítka** může být v `Enabled` nebo `Disabled` stavu. A zakázané **tlačítko** nebude reakci na vstup uživatele. Například následující kód zakáže `CallButton`: CallButton.Enabled = false; Další informace o tlačítka, najdete v části [tlačítka](~/ios/user-interface/controls/buttons.md) průvodce.
- **Zavření klávesnice** – Pokud uživatel odposlouchávání textové pole, iOS zobrazí klávesnice, aby mohl uživatel zadání. Bohužel neexistuje žádné integrované funkce pro zavření klávesnice. Následující kód je přidán do `TranslateButton` pro zavření klávesnice, když uživatel stiskne `TranslateButton`: PhoneNumberText.ResignFirstResponder (); Další příklad zavření klávesnice, najdete v části [zavření klávesnice](https://developer.xamarin.com/recipes/ios/input/keyboards/dismiss_the_keyboard) recepturách.
- **Místní telefonní hovor s adresou URL** – v aplikaci Phoneword je schéma adresy URL Apple použitý ke spuštění systému telefonní aplikaci. Vlastní schéma adresy URL se skládá z "Telefon:" předponu a přeložený telefonní číslo, které jsou popsány v následující kód:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **Zobrazit výstrahu** – Pokud se uživatel pokusí o umístit telefonní hovor na zařízení, která nepodporuje volání – například simulátoru nebo iPod Touch – dialogového okna výstrah se zobrazí uživateli oznamuje, nemůže být umístěn telefonní hovor. Následující kód vytvoří a naplní řadič výstrah:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

Další informace o zobrazení výstrah iOS, najdete v části [výstraha řadiče recepturách](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/).

## <a name="testing-deployment-and-finishing-touches"></a>Testování, nasazení a všechny změny

Visual Studio pro Mac a Visual Studio poskytují mnoho možností pro testování a nasazení aplikace. Tato část popisuje možnosti ladění, ukazuje testování aplikací na zařízení a zavádí nástroje pro vytváření ikony vlastní aplikace a bitové spouštěcí kopie.

### <a name="debugging-tools"></a>Ladicí nástroje

Někdy je těžké diagnostikovat problémy v kódu aplikace. Pro usnadnění diagnostiky problémů složitý kód, může [zarážku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [kód prostřednictvím kroku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), nebo [výstupní informace do okna protokolu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

### <a name="deploy-to-a-device"></a>Nasazení do zařízení

Simulátoru iOS je rychlý způsob, jak testovat aplikaci. Simulátor má počet užitečné optimalizace pro testování, včetně imitované umístění [simulaci přesun](https://developer.xamarin.com/recipes/ios/multitasking/test_location_changes_in_simulator/)a další. Uživatelé však nebude využívat konečné aplikaci v simulátoru. Všechny aplikace by měla být testována na skutečné zařízení včas a často.

Zařízení trvá určitou dobu zřídit a vyžaduje účet pro vývojáře Apple. [Zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) nabízí podrobný návod na Příprava zařízení pro vývoj.

> [!NOTE]
> V současné době z důvodu požadavku od společnosti Apple, je potřeba mít vývojový certifikát nebo _podepisování identity_ sestavení kódu pro zařízení ani simulátor. Postupujte podle kroků v [zřizování zařízení Průvodce](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) chcete nastavit tuto možnost.

Po zřízení zařízení můžete nasadit do ní zapojením do změna cíl v panelu nástrojů sestavení na zařízení iOS a stisknutím klávesy **spustit** ( **přehrání**) vidíte na následujícím snímku obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Stiskněte Start nebo Play")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Stiskněte Start nebo Play")

-----

Aplikace nasadí na zařízení s iOS:

[![](hello-ios-deepdive-images/image1.png "Aplikace nasadí na zařízení s iOS a spustit")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Generovat vlastními ikonami a spuštění bitové kopie

Ne každý má k dispozici pro vytvoření vlastní ikony a spuštění bitové kopie, které aplikace potřebuje k zvýraznění návrháře. Tady je několik alternativních přístupech ke generování kresby vlastní aplikaci:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- [**Nákresu** ](https://www.sketchapp.com") – nákresu je aplikace Mac pro návrh uživatelského rozhraní, ikony a další. Toto je aplikace, která byla použita k návrhu sadě ikon aplikace Xamarin a spuštění bitové kopie. Nákresu 3 je k dispozici na webu App Store. Můžete vyzkoušet bezplatnou [nákresu nástroj](http://bohemiancoding.com/sketch/tool/) také.
- [**Pixelmator** ](http://www.pixelmator.com/) – bitovou kopii univerzální úpravy aplikace pro Mac, které stojí o $30.
- [**Glyphish** ](http://www.glyphish.com/) – ikona předem vysoce kvalitní nastaví zdarma stažení a nákup.
- [**Fiverr** ](http://www.fiverr.com/) – vybrat z mnoha různých Designer vytváření ikony nastavení za vás, počínaje $5.  Může být hit nebo miss ale dobrý prostředků, pokud potřebujete ikony navržen za chodu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

* Visual Studio – tu můžete použít k vytvoření jednoduché ikony nastavit pro vaši aplikaci přímo v prostředí IDE.
- [**Glyphish** ](http://www.glyphish.com/) – ikona předem vysoce kvalitní nastaví zdarma stažení a nákup.
- [**Fiverr** ](http://www.fiverr.com/) – vybrat z mnoha různých Designer vytváření ikony nastavení za vás, počínaje $5.  Může být hit nebo miss ale dobrý prostředků, pokud potřebujete ikony navržen za chodu

-----

Další informace o velikosti ikonu a spouštěcí bitové kopie a požadavky najdete v části [práce s obrázky Průvodce](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Souhrn

Blahopřejeme! Nyní máte solidní znalosti součástí aplikace pro Xamarin.iOS, jakož i nástroje používané k jejich vytvoření.
V [další kurz v této sérii Začínáme](~/ios/get-started/hello-ios-multiscreen/index.md), budete rozšířit aplikace pro zpracování více obrazovky. Na této cestě budete implementovat řadič navigace, další informace o Segues scénáře a způsobit Model, zobrazení, Controller (MVC) vzor jako rozšířit aplikace pro zpracování více obrazovky.


## <a name="related-links"></a>Související odkazy

- [Hello, iOS (ukázka)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS Provisioning Portal](https://developer.apple.com/ios/manage/overview/index.action)
