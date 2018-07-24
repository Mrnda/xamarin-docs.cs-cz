---
title: Hello, iOS – podrobně
description: Tento dokument se podíváme podrobněji na Hello, iOS ukázkovou aplikaci, zvažujete jeho architektuře, uživatelské rozhraní, zobrazení obsahu hierarchie, testování, nasazení a další.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 16920f27a1830dc6a3ab1a3cb0a267eb3b1d90ea
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203020"
---
# <a name="hello-ios--deep-dive"></a>Hello, iOS – podrobně

Rychlý start návodu, zavedená sestavováním a spouštěním základní aplikace pro Xamarin.iOS. Nyní je doba k rozvoji lepší představu o fungování aplikací pro iOS, abyste mohli sestavit složitější programy. Tato příručka kontroly kroky, které v Hello, iOS návodu znalost základní koncepty vývoje aplikací pro iOS.

Tento průvodce vám pomůže vytvořit dovednosti a znalosti potřebné k sestavení aplikace pro iOS jednu obrazovku. Po ukončení práce přes něj, byste měli mít znalost různé části aplikace pro Xamarin.iOS a jak je umístit společně.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Úvod do sady Visual Studio pro Mac

Visual Studio for Mac je zdarma, open source IDE, které jsou k dispozici funkce z Visual Studio a XCode. Nabízí plně integrovaná vizuálního návrháře, kompletní nástroje pro refaktoring textového editoru, prohlížeči sestavení, integrace zdrojového kódu a další. Tento průvodce uvádí některé základní sady Visual Studio pro Mac funkce, ale pokud jste ještě sadu Visual Studio pro Mac, podívejte se [Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/) dokumentaci.

Visual Studio pro Mac odpovídá uspořádání kódu do sady Visual Studio praxe *řešení* a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (iOS nebo Android), podpůrné knihovny, testovací aplikace a další. V aplikaci Phoneword novém Iphonu projektu byla přidána pomocí **jediné zobrazení aplikace** šablony. Počáteční řešení se hledá takto:

![](hello-ios-deepdive-images/image30.png "Snímek obrazovky s počáteční řešení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Úvod do sady Visual Studio

Visual Studio je výkonné integrované vývojové prostředí společnosti Microsoft. Nabízí plně integrovaná vizuálního návrháře, kompletní nástroje pro refaktoring textového editoru, prohlížeči sestavení, integrace zdrojového kódu a další. Tento průvodce uvádí některé základní funkce sady Visual Studio s Xamarinem modulu plug-in.

Visual Studio slouží k uspořádání kódu do _řešení_ a *projekty*. Řešení je kontejner, který může obsahovat jeden nebo více projektů. Projekt může být aplikace (iOS nebo Android), podpůrné knihovny, testovací aplikace a další. V aplikaci Phoneword novém Iphonu projektu byla přidána pomocí **jediné zobrazení aplikace** šablony. Počáteční řešení se hledá takto:

![](hello-ios-deepdive-images/vs-image30.png "Snímek obrazovky s počáteční řešení")

-----

## <a name="anatomy-of-a-xamarinios-application"></a>Anatomie aplikace Xamarin.iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Na levé straně je *oblasti řešení*, který obsahuje adresářovou strukturu a všechny soubory přidružené k řešení:

![](hello-ios-deepdive-images/image31.png "Oblasti řešení, která obsahuje adresářovou strukturu a všechny soubory přidružené k řešení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Na pravé straně je *podokno řešení*, který obsahuje adresářovou strukturu a všechny soubory přidružené k řešení:

![](hello-ios-deepdive-images/vs-image31.png "V podokně řešení, která obsahuje adresářovou strukturu a všechny soubory přidružené k řešení")

-----

V [Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) návodu jste vytvořili řešení, nazývá **Phoneword** a je umístěná iOS Project - **Phoneword_iOS** – dovnitř. Položky v projektu zahrnují:

-  **Odkazy na** – obsahuje sestavení, které jsou potřebné k sestavení a spuštění aplikace. Rozbalte adresář zobrazíte odkazy na sestavení .NET [systému](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx) , System.Core a [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx) , a také odkaz na sestavení Xamarinu pro Xamarin.iOS.
-  **Balíčky** – The balíčky adresář obsahuje předem připravená balíčky NuGet.
-  **Prostředky** -složce prostředky jsou uloženy jiného média.
-  **Main.cs** – obsahuje hlavní vstupní bod aplikace. Ke spuštění aplikace, název třídy hlavní aplikace `AppDelegate`, je předáno.
-  **AppDelegate.cs** – tento soubor obsahuje třídu hlavní aplikaci a je zodpovědný za vytváření okna, vytváření uživatelského rozhraní a naslouchá událostem z operačního systému.
-  **Main.Storyboard** – The scénáře obsahuje vizuální návrh uživatelského rozhraní pro aplikace. Scénáře soubory otevřené v grafickém editoru volána v iOS designeru.
-  **ViewController.cs** – The kontroler zobrazení využívá obrazovky (View), který uživatel vidí a týká. Kontroler zobrazení je zodpovědná za zpracování interakce mezi uživateli a zobrazení.
-  **ViewController.designer.cs** – `designer.cs` je automaticky generovaný soubor, který slouží jako spojovací mezi ovládacími prvky v zobrazení a jejich reprezentace kód v Kontroleru zobrazení. Protože to je soubor interní vložení, rozhraní IDE přepíše všechny ruční změny a ve většině případů, které tento soubor můžete ignorovat. Další informace o vztahu mezi vizuálního návrháře a základní kód, naleznete [Úvod do Iosu návrháře](~/ios/user-interface/designer/introduction.md) průvodce.
-  **Soubor info.plist** – `Info.plist` je, kde jsou nastaveny vlastnosti aplikace, jako je například název aplikace, ikony, spouštěcí obrázky a další. Toto je soubor výkonné a je k dispozici v důkladný Úvod k němu [práce se seznamy vlastností](~/ios/app-fundamentals/property-lists.md) průvodce.
-  **Do souboru Entitlements.plist** – seznam vlastností oprávnění umožňuje nám zadat aplikace *možnosti* (také nazývané App Store technologie) jako serveru služby iCloud, PassKit a další. Další informace o `Entitlements.plist` najdete v [práce se seznamy vlastností](~/ios/app-fundamentals/property-lists.md) průvodce. Obecný úvod k oprávnění, najdete [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) průvodce.

## <a name="architecture-and-app-fundamentals"></a>Základy architektury a aplikace

Předtím, než aplikace pro iOS můžete načíst uživatelské rozhraní, musí být splněné dvě věci. Nejprve je potřeba definovat aplikace *vstupní bod* – první kód, který spouští proces aplikací je načtena do paměti. Za druhé je potřeba definovat třídu pro zpracování událostí celou aplikaci a pracovat s operačním systémem.

Tato část studie vztahy znázorněno v následujícím diagramu:

[![](hello-ios-deepdive-images/image32.png "V tomto diagramu jsou znázorněny vztahy architektury a principy aplikace")](hello-ios-deepdive-images/image32.png#lightbox)

Pojďme začít na začátku a zjistěte, co se stane při spuštění aplikace.

### <a name="main"></a>Hlavní

Je hlavní vstupní bod aplikace pro iOS `Main.cs` souboru. `Main.cs` obsahuje statickou metodu Main, která vytvoří novou instanci aplikace Xamarin.iOS a předá název *delegáta aplikace* třídu, která bude zpracovávat události operačního systému. Kód šablony pro statické `Main` metoda se zobrazí níže:

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

### <a name="application-delegate"></a>Delegáta aplikace

V Iosu *delegáta aplikace* třída zpracovává události systému; Tato třída se nachází uvnitř `AppDelegate.cs`. `AppDelegate` Třída spravuje aplikace *okno*. V okně je jedna instance `UIWindow` třídu, která slouží jako kontejner pro uživatelské rozhraní. Ve výchozím nastavení, kterým aplikace získává pouze jedno okno, do které chcete načíst její obsah a v okně je připojen k *obrazovky* (jeden `UIScreen` instance), která poskytuje ohraničující obdélník odpovídající dimenze fyzický obrazovce zařízení.

*AppDelegate* zodpovídá také pro přihlášení k odběru aktualizací systému o událostech důležité aplikace, například když aplikace dokončí, spouštění nebo při nedostatku paměti.

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

Po definování jeho okno aplikace ji můžete začít načítání uživatelského rozhraní. V další části se věnuje vytváření uživatelského rozhraní.

## <a name="user-interface"></a>Uživatelské rozhraní

Uživatelské rozhraní aplikace pro iOS se trochu prezentace – aplikace obvykle získá jedno okno, ale to můžete vyplnit v okně s velký počet objektů na to potřebuje, a objekty a pravidla lze změnit v závislosti na tom, co aplikace potřebuje k zobrazení. Objekty v tomto scénáři – věcí, které se uživateli zobrazí – se nazývají zobrazení. K sestavení na jedné obrazovce v aplikaci, jsou navršeny zobrazení nad sebou *obsahu zobrazit hierarchii*, a hierarchii spravuje Kontroleru jednoho zobrazení. Aplikace s více obrazovkami mají více obsahu zobrazení hierarchie, každý s vlastní kontroler zobrazení a aplikace umístí zobrazení v okně vytvoření různých obsahu zobrazení hierarchie založený na obrazovce, jejímž je uživatel v.

Tato část se věnuje do uživatelského rozhraní zadáním popisu vašeho nového zobrazení obsahu zobrazení hierarchie a v iOS designeru.

### <a name="ios-designer-and-storyboards"></a>iOS Designer a scénářů

IOS Designer je vizuální nástroj pro vytváření uživatelských rozhraní v Xamarinu. Návrhář se dají spustit dvojitým kliknutím na jakýkoli soubor scénáře (.storyboard), které se otevře zobrazení, která vypadá podobně jako na následujícím snímku obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "iOS Designer rozhraní")

A *scénáře* je soubor, který obsahuje vizuální návrhy naší aplikace obrazovky a přechody a vztahy mezi obrazovkami. Reprezentace aplikace obrazovky ve scénáři se volá _scény_. Každý scény reprezentuje kontroler zobrazení a zásobníku zobrazení spravuje (Zobrazit hierarchii obsahu). Když je nový **jediné zobrazení aplikace** vytvoření projektu ze šablony, sady Visual Studio pro Mac automaticky generuje soubor scénáře s názvem `Main.storyboard` a naplní ji jeden scény, jak na snímku obrazovky níže:

![](hello-ios-deepdive-images/image34.png "Visual Studio pro Mac automaticky generuje soubor scénáře s názvem Main.storyboard a naplní jednu scény")

Černý pruh v dolní části obrazovky scénáři nejde ani zvolit zvolte kontroler zobrazení pro scénu. Kontroler zobrazení je instance `UIViewController` třídu, která obsahuje základní kód pro hierarchii obsahu zobrazení. Vlastnosti v tomto Kontroleru zobrazení můžete zobrazit a nastavit **oblasti vlastnosti**, jak je znázorněno v následujícím snímku obrazovky:

![](hello-ios-deepdive-images/image35.png "Podokno vlastností")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "iOS Designer rozhraní")

A *scénáře* je soubor, který obsahuje vizuální návrhy naší aplikace obrazovky a přechody a vztahy mezi obrazovkami. Reprezentace aplikace obrazovky ve scénáři se volá _scény_. Každý scény reprezentuje kontroler zobrazení a zásobníku zobrazení spravuje (Zobrazit hierarchii obsahu). Když je nový **jediné zobrazení aplikace** vytvoření projektu ze šablony, sady Visual Studio automaticky vygeneruje soubor scénáře s názvem `Main.storyboard` a naplní ji jeden scény, jak je znázorněno v následujícím snímku obrazovky:

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio automaticky vygeneruje soubor scénáře s názvem Main.storyboard a naplní jednu scény")

Na panelu v dolní části obrazovky scénáři nejde ani zvolit zvolte kontroler zobrazení pro scénu. Kontroler zobrazení je instance `UIViewController` třídu, která obsahuje základní kód pro hierarchii obsahu zobrazení. Vlastnosti v tomto Kontroleru zobrazení můžete zobrazit a nastavit **podokno vlastností**, jak je znázorněno v následujícím snímku obrazovky:

![](hello-ios-deepdive-images/vs-image35.png "Podokno vlastností")

-----

_Zobrazení_ můžete vybrat kliknutím uvnitř bílé části scény. Zobrazení je instance `UIView` třídu, která definuje oblast na obrazovce a poskytuje rozhraní pro práci s obsahem v této oblasti. Výchozí zobrazení je jedinou *kořenovému zobrazení* , který vyplní celé obrazovky celé zařízení.

Nalevo od scény je Šedá šipka s ikonou vlajky, jak je znázorněno v následujícím snímku obrazovky:

 [![](hello-ios-deepdive-images/image37.png "Šedá šipka s ikonou vlajky")](hello-ios-deepdive-images/image37.png#lightbox)

Šedá šipka představuje přechod scénář nazývá *Segue* (vyslovováno "seg způsob"). Protože tato Segue nemá žádné původu, je volána *Sourceless přechod na to*. Sourceless přechod na to ukazuje na první scény, jejichž zobrazení získá načíst do naší aplikace okno při spuštění aplikace. První věc, která se uživateli zobrazí při načítání aplikace budou mít scény a zobrazení dovnitř.

Při vytváření uživatelského rozhraní, další zobrazení můžete přetáhnout z **nástrojů** do hlavního zobrazení na návrhové ploše, jak je znázorněno v následujícím snímku obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "Další zobrazení můžete přetáhnout z panelu nástrojů do hlavního zobrazení na návrhové ploše")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "Další zobrazení můžete přetáhnout z panelu nástrojů do hlavního zobrazení na návrhové ploše")

-----

Tyto další zobrazení, se nazývají *dílčích zobrazení*. Společně kořenové zobrazení a dílčích zobrazení jsou součástí *obsahu zobrazit hierarchii* , který je spravován `ViewController`. Osnovy všechny elementy ve scéně zobrazením prozkoumáním v **Osnova dokumentu** panel:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "Panel osnovu dokumentu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "Panel osnovu dokumentu")

-----

Dílčích zobrazení jsou vyznačené na následujícím diagramu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "Jsou zvýrazněné dílčích zobrazení diagramu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "Jsou zvýrazněné dílčích zobrazení diagramu")

-----

V další části rozdělí obsah zobrazit hierarchii reprezentována tento scény.

## <a name="content-view-hierarchy"></a>Zobrazit hierarchii obsahu

A _obsahu zobrazit hierarchii_ je zásobník zobrazení a dílčích zobrazení spravuje Kontroleru jednoho zobrazení, jak je znázorněno v následujícím diagramu:

 [![](hello-ios-deepdive-images/image41.png "Hierarchie zobrazení obsahu")](hello-ios-deepdive-images/image41.png#lightbox)

Bychom mohli obsahu zobrazit hierarchii z našich `ViewController` snazší zjistit tak, že dočasně změníte barvu pozadí kořenového zobrazení na žlutou v části zobrazení z **oblasti vlastnosti**, jak je znázorněno v následujícím snímku obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "Změna barvy pozadí kořenového zobrazení na žlutou v části zobrazení panelu Vlastnosti")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "Změna barvy pozadí kořenového zobrazení na žlutou v části zobrazení panelu Vlastnosti")

-----

Následující diagram znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a kontroler zobrazení, které vdechnou uživatelského rozhraní na obrazovce zařízení:

 [![](hello-ios-deepdive-images/image43.png "Vztahy mezi oken, zobrazení, dílčích zobrazení a kontroler zobrazení")](hello-ios-deepdive-images/image43.png#lightbox)

V další části popisuje, jak pracovat se zobrazeními v kódu a naučte se programovat pro interakci s uživatelem pomocí životního cyklu zobrazení a Kontrolery zobrazení.

## <a name="view-controllers-and-the-view-lifecycle"></a>Životního cyklu zobrazení a kontrolery zobrazení

Každý obsahu zobrazení hierarchie má odpovídající kontroler zobrazení pro interakci s uživatelem napájení. Role Kontroleru zobrazení je Správa zobrazení v hierarchii obsahu zobrazení. Kontroler zobrazení není součástí obsahu zobrazit hierarchii a není je prvek v rozhraní. Místo toho poskytuje kód, který využívá interakce uživatele s objekty na obrazovce.

### <a name="view-controllers-and-storyboards"></a>Kontrolery zobrazení a scénářů

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kontroler zobrazení je reprezentován ve scénáři pruh v dolní části scény. Vyberte kontroler zobrazení zobrazí její vlastnosti v **oblasti vlastnosti**:

![](hello-ios-deepdive-images/image44.png "Vyberte kontroler zobrazení zobrazí její vlastnosti v podokně vlastností")

Vlastní třídu Kontroleru zobrazení pro obsah zobrazit hierarchii reprezentována tento scény můžete nastavit úpravou **třídy** vlastnost **Identity** část **oblasti vlastnosti**. Například náš **Phoneword** aplikace nastaví `ViewController` jako kontroler zobrazení pro naši první obrazovku, jak je znázorněno v následujícím snímku obrazovky:

![](hello-ios-deepdive-images/image45new.png "Aplikace Phoneword nastaví ViewController jako kontroler zobrazení")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kontroler zobrazení je reprezentován ve scénáři pruh v dolní části scény. Vyberte kontroler zobrazení zobrazí její vlastnosti v **podokno vlastností**:

![](hello-ios-deepdive-images/vs-image44.png "Vyberte kontroler zobrazení zobrazí její vlastnosti v podokně vlastností")

Vlastní třídu Kontroleru zobrazení pro obsah zobrazit hierarchii reprezentována tento scény můžete nastavit úpravou **třídy** vlastnost **Identity** část **podokno vlastností**. Například náš **Phoneword** aplikace nastaví `ViewController` jako kontroler zobrazení pro naši první obrazovku, jak je znázorněno v následujícím snímku obrazovky:

![](hello-ios-deepdive-images/vs-image45.png "Aplikace Phoneword nastaví ViewController jako kontroler zobrazení")

-----

Toto odkazuje kontroler zobrazení pro reprezentaci scénáře `ViewController` třída jazyka C#. Otevřít `ViewController.cs` soubor a Všimněte si, že je kontroler zobrazení *podtřídy* z `UIViewController`, jak je znázorněno v následujícím kódem:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

`ViewController` Nyní jednotky interakce obsahu zobrazit hierarchii přiřazených k tomuto Kontroleru zobrazení ve scénáři. Dále se dozvíte o roli kontroler zobrazení v zobrazení Správa zavedením procesu nazývaného životního cyklu zobrazení.

> [!NOTE]
> Pouze visual obrazovek, které nevyžaduje zásah uživatele **třídy** vlastnost může být ponecháno prázdné v **oblasti vlastnosti**. Tím se nastaví kontroler zobrazení pomocné třídy jako výchozí implementaci `UIViewController`, která je vhodná, pokud se nechystáte na přidáte vlastní kód.

### <a name="view-lifecycle"></a>Životní cyklus zobrazení

Kontroler zobrazení má na starosti načítání a uvolňování obsahu zobrazení hierarchie z okna. Při zobrazení v hierarchii zobrazení obsahu se stane něco význam, operační systém oznámení kontroler zobrazení prostřednictvím události v zobrazení životního cyklu. Přepsáním metody v zobrazení životního cyklu můžete pracovat s objekty na obrazovce a vytvářet dynamické a responzivní uživatelské rozhraní.

Jedná se o základní životní cyklus metod a jejich funkce:

-  **ViewDidLoad** – voláno *po* poprvé kontroler zobrazení načte hierarchii obsahu zobrazení do paměti. To je vhodné místo má být provedeno počáteční nastavení, protože je při dílčích zobrazení nejdříve k dispozici v kódu.
-  **ViewWillAppear** – volána pokaždé, když kontroler zobrazení zobrazení se přidají do obsahu zobrazit hierarchii a na obrazovce.
-  **ViewWillDisappear** – volána pokaždé, když je zobrazení Kontroleru zobrazení Chystáte se odebrat z obsahu zobrazit hierarchii a zmizí z obrazovky. Tato událost životního cyklu se používá pro čištění a uložení stavu.
-  **ViewDidAppear** a **ViewDidDisappear** – volána, když zobrazení získá přidávat nebo odebírat obsahu zobrazit hierarchii, v uvedeném pořadí.


Při přidání vlastního kódu do jakékoli fáze životního cyklu, tato metoda životního cyklu od *základní implementaci* musí být *přepsaný*. Tím se dosahuje proniknutí do existující životního cyklu metodu, která má nějaký kód už k němu připojená, a rozšíření s další kód. Základní implementaci je volána z v rámci metody, která Ujistěte se, že původní kód spuštěn dříve, než nový kód. Příkladem je znázorněn v následujícím oddílu.

Další informace o práci se Kontrolery zobrazení najdete společnosti Apple [Průvodce programováním v zobrazení Kontroleru pro iOS](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1) a [UIViewController odkaz](https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc).

### <a name="responding-to-user-interaction"></a>Reagovat na interakci uživatele

Nejdůležitější roli kontroler zobrazení reaguje na interakci uživatele, jako jsou stisknutí tlačítek, navigace a další. Nejjednodušší způsob, jak se zpracovávají interakci s uživatelem se propojí nahoru vstupní ovládací prvek tak, aby naslouchala na uživatele a připojte obslužné rutiny události pro reakci na vstup. Například tlačítko by mohla být připraveno, reagovat na událost dotykového ovládání, jak je ukázáno v Phoneword aplikace.

Teď, když je mít lepší představu o zobrazení a Kontrolery zobrazení, Zkusíme zjistit, jak to funguje.
V `Phoneword_iOS` projektu, tlačítko byl přidán volané `TranslateButton` do obsahu zobrazení hierarchie:

 [![](hello-ios-deepdive-images/image1.png "Tlačítko byl přidán do obsahu zobrazit hierarchii volané TranslateButton")](hello-ios-deepdive-images/image1.png#lightbox)

Při **název** přiřazen **tlačítko** v ovládacím prvku **oblasti vlastnosti**, iOS designer automaticky mapují na ovládací prvek v  **ViewController.designer.cs**a `TranslateButton` k dispozici uvnitř `ViewController` třídy. Ovládací prvky nejdříve k dispozici v `ViewDidLoad` fáze životního cyklu zobrazení, takže tato metoda životního cyklu se používá na dotykové ovládání pro uživatele:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Aplikace Phoneword používá touch události, volá `TouchUpInside` tak, aby naslouchala na dotykové ovládání pro uživatele. `TouchUpInside` čeká na dotykové ovládání nahoru událostí (prstem přenesení mimo obrazovku), který následuje dotykové ovládání dolů (prstem klepnou na obrazovce) uvnitř hranic ovládacího prvku. Opak `TouchUpInside` je `TouchDown` událost, která je vyvoláno, když uživatel stiskne klávesu na ovládacím prvku. `TouchDown` Událost zaznamená spoustu šumu a umožňuje uživateli není možnost zrušit dotykem posunutím jejich prstem mimo ovládací prvek. `TouchUpInside` je nejběžnější způsob, jak reagovat na **tlačítko** dotykového ovládání a vytvoří prostředí, kde je uživatel očekává při stisknutí tlačítka. Další informace o tomto je k dispozici v od Applu [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html).

Aplikace zpracovává `TouchUpInside` událost s výraz lambda, ale delegáta nebo obslužnou rutinu události s názvem může také se používají. Tímto se podobaly konečný kód tlačítka:

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

## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty představenými v Phoneword

Aplikace Phoneword zavedli několik konceptů, které nejsou součástí této příručky. Tyto koncepty patří:

- **Změnit Text na tlačítku** – The Phoneword aplikace ukázal, jak lze změnit text **tlačítko** voláním `SetTitle` na **tlačítko** a předání do nového textu a  **Tlačítko**společnosti _stav ovládacího prvku_. Například následující kód změní CallButton text "Volání":

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **Povolení a zakázání tlačítka** – **tlačítka** může být v `Enabled` nebo `Disabled` stavu. A je zakázaná. **tlačítko** nebude reagovat na vstup uživatele. Například následující kód zakáže `CallButton`: CallButton.Enabled = false;. Další informace na tlačítka [tlačítka](~/ios/user-interface/controls/buttons.md) průvodce.
- **Zavřít klávesnice** – když je uživatel odposlouchávání textové pole s Iosem zobrazí klávesnice, umožníte uživateli zadejte vstup. Bohužel neexistuje žádná vestavěná funkce zavřete klávesnice. Následující kód je přidán do `TranslateButton` zavřete klávesnici, když uživatel stiskne klávesu `TranslateButton`: PhoneNumberText.ResignFirstResponder (); Další příklad zavření klávesnice, najdete [zavřít klávesnice](https://github.com/xamarin/recipes/tree/master/Recipes/ios/input/keyboards/dismiss_the_keyboard) předpisu.
- **Místo telefonního hovoru s adresou URL** – v aplikaci Phoneword schéma adresy URL Apple použít ke spuštění systému telefonní aplikace. Vlastní schéma adresy URL se skládá z "tel:" předponu a přeložený telefonní číslo, jak je znázorněno v následujícím kódem:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **Zobrazit výstrahu** – když se uživatel pokusí na zařízení, která nepodporuje volání – například simulátoru umístěte telefonního hovoru nebo ipodu Touch – dialogového okna výstrah se zobrazí, umožníte uživateli vědět, nemůže být umístěn telefonního hovoru. Následující kód vytvoří a naplní řadič výstrahy:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

Další informace o zobrazení výstrah pro iOS, najdete [upozornění řadič předpisu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller).

## <a name="testing-deployment-and-finishing-touches"></a>Testování, nasazení a doladění

Visual Studio pro Mac a Visual Studio poskytují mnoho možností pro testování a nasazení aplikace. Tato část popisuje možnosti ladění, ukazuje testování aplikací na zařízení a přináší nástroje pro vytváření ikony vlastní aplikace a spouštěcí obrázky.

### <a name="debugging-tools"></a>Nástroje pro ladění

Někdy je obtížné diagnostikovat problémy v kódu aplikace. Pro usnadnění diagnostiky potíží se komplexního kódu, může [nastavte zarážku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [kód prostřednictvím kroku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), nebo [výstupní informace do okna protokolu](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

### <a name="deploy-to-a-device"></a>Nasazení do zařízení

Simulátor Iosu je rychlý způsob, jak testovat aplikaci. Simulátor má několik užitečných optimalizace pro testování, včetně mock umístění [simulaci přesun](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/test_location_changes_in_simulator)a provádění dalších akcí. Uživatelé však nebude využívat konečné aplikaci v simulátoru. Všechny aplikace by měl být testován na skutečných zařízeních již v rané fázi a často.

Zařízení trvá určitou dobu ke zřízení a vyžaduje účet pro vývojáře Apple. [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) Průvodce poskytuje podrobný návod na Příprava zařízení pro vývoj.

> [!NOTE]
> V současné době z důvodu požadavku od společnosti Apple, je potřeba mít certifikát pro vývoj nebo _Podpisová identita_ k vytváření kódu pro zařízení nebo simulátor. Postupujte podle kroků v [Device Provisioning průvodce](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) nastavení.

Po zřízení zařízení můžete nasadit do ní zapojením v změnit cíl na panelu nástrojů sestavení na zařízení iOS a stisknutím klávesy **Start** ( **Přehrát**) vidíte na následujícím snímku obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Stisknutím klávesy Start/přehrávání")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Stisknutím klávesy Start/přehrávání")

-----

Aplikace se nasadí do zařízení s Iosem:

[![](hello-ios-deepdive-images/image1.png "Bude možné nasadit do zařízení s Iosem a spustit aplikaci")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Generovat vlastní ikony a spouštěcí obrázky

Ne všichni mají k dispozici k vytvoření vlastních ikon a spuštění imagí, které aplikace potřebuje, abyste se odlišili návrháře. Tady je několik alternativních přístupech ke generování kresby vlastní aplikace:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- [**Náčrt** ](https://www.sketchapp.com") – nákresu je aplikace pro Mac k návrhu uživatelského rozhraní, ikony a další. Toto je aplikace, která byla použita k návrhu sady ikon aplikací Xamarin a obrázky po spuštění. Sketch 3 je k dispozici na App Store. Budete moct vyzkoušet bezplatnou [Sketch nástroj](http://bohemiancoding.com/sketch/tool/) také.
- [**Pixelmator** ](http://www.pixelmator.com/) – bitovou kopii univerzální aplikace pro úpravy pro Mac, která stojí přibližně 30 USD.
- [**Glyphish** ](http://www.glyphish.com/) – ikona předem připravených vysoce kvalitní nastaví zdarma ke stažení a nákup.
- [**Fiverr** ](http://www.fiverr.com/) – zvolte z mnoha návrhářů vytváření ikony pro vás od 5 USD.  Může být hit nebo miss ale Dobrým zdrojem informací je potřebujete ikony navržené v reálném čase

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

* Visual Studio – můžete k vytvoření jednoduché ikony nastavte pro vaši aplikaci přímo v integrovaném vývojovém prostředí.
- [**Glyphish** ](http://www.glyphish.com/) – ikona předem připravených vysoce kvalitní nastaví zdarma ke stažení a nákup.
- [**Fiverr** ](http://www.fiverr.com/) – zvolte z mnoha návrhářů vytváření ikony pro vás od 5 USD.  Může být hit nebo miss ale Dobrým zdrojem informací je potřebujete ikony navržené v reálném čase

-----

Další informace o velikosti obrázku ikony a spuštění a požadavky najdete [práce s obrázky Průvodce](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Souhrn

Blahopřejeme! Teď máte důkladného porozumění komponenty aplikace pro Xamarin.iOS, stejně jako nástroje používané k jejich vytvoření.
V [další kurz v této sérii Začínáme](~/ios/get-started/hello-ios-multiscreen/index.md), budete rozšířit naše aplikace pro zpracování více obrazovek. Na cestě budete implementovat kontroler navigace, přečtěte si o scénáři přechody a zavést Model, zobrazení, Controller (MVC) vzoru rozšířit naše aplikace pro zpracování více obrazovek.


## <a name="related-links"></a>Související odkazy

- [Hello, iOS (ukázka)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)
- [Portál zřizování iOS](http://developer.apple.com/account/#/overview)
