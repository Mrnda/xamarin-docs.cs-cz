---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF vs. Životní cyklus aplikace Xamarin.Forms
description: Principy procesu spuštění aplikace a týkajících se stavy pozadí
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4f9aebbbcab48290d37c5732c69267897238272
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF vs. Životní cyklus aplikace Xamarin.Forms

Xamarin.Forms trvá spoustu pokyny k návrhu z rozhraní založené na jazyce XAML, dodaných před jeho, zejména WPF. Ale jiné způsoby odchylují výrazně což může být trvalé bod pro pokus o migraci přes osoby. Tento dokument se pokusí identifikovat některé z těchto problémů a obsahují pokyny, kde je to možné do mostu WPF znalostní báze na platformě Xamarin.Forms.

## <a name="app-lifecycle"></a>Životní cyklus aplikace

Životní cyklus aplikace mezi WPF a Xamarin.Forms je podobný. Jak spustit v kódu externí (platforma) a spustit uživatelské rozhraní pomocí volání metody. Rozdíl je, že Xamarin.Forms vždy začínat v sestavení specifické pro platformu, která pak inicializuje a vytvoří uživatelského rozhraní pro aplikaci.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` Metoda se ve výchozím nastavení, automaticky generovaný a že nejsou viditelné v kódu.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Application – třída

WPF a Xamarin.Forms mají `Application` třída, která je vytvořena jako typ singleton. Ve většině případů bude aplikace odvozovat z této třídy poskytnout vlastní aplikaci, přestože to není nezbytně nutné v grafickém subsystému WPF. Jak vystavit `Application.Current` vlastnost najít vytvořený typu singleton.

### <a name="global-properties--persistence"></a>Vlastnosti globálních + trvalost

WPF a Xamarin.Forms `Application.Properties` slovník k dispozici kam můžete ukládat globální objekty úrovni aplikace, které jsou dostupné kdekoli v aplikaci. Klíčovým rozdílem je, že se Xamarin.Forms _zachovat_ všechny primitivní typy, které jsou uložené v kolekci, když je pozastaveno a aplikace je znovu načíst, když je relaunched. WPF nepodporuje automaticky daná chování – místo toho Většina vývojářů spoléhali na izolované úložiště, nebo využívat integrované `Settings` podporovat.

## <a name="defining-pages-and-the-visual-tree"></a>Definování stránky a vizuálním stromu

Používá grafického subsystému WPF `Window` jako kořenový element pro libovolný element nejvyšší úrovně visual. Definuje popisovačem HWND na světě Windows pro zobrazení informací. Můžete vytvořit a zobrazit tolik windows současně libovolně v grafickém subsystému WPF.

V Xamarin.Forms, nejvyšší úrovně visual vždy definována platformou – například v systému iOS, bude `UIWindow`. Vykreslí Xamarin.Forms je obsahu do těchto nativní platforma reprezentace pomocí `Page` třídy. Každý `Page` v Xamarin.Forms představuje jedinečné "stránku" v aplikaci, kde pouze jedna je viditelný v čase.

Obě WPFs `Window` a Xamarin.Forms `Page` zahrnují `Title` vlastnost k ovlivnění zobrazený název a obě mít `Icon` vlastnosti chcete zobrazit konkrétní ikonu pro stránku (**Poznámka** , název a ikona nejsou vždy viditelné v Xamarin.Forms). Kromě toho můžete změnit společných vlastností visual na obou například barvu pozadí nebo image.

Když je technicky možné k vykreslení do dvou samostatných platformy zobrazení (například definovat dvě `UIWindow` objekty a mít druhý jeden vykreslení na externí zobrazení nebo AirPlay), se vyžaduje kód specifický pro platformu Uděláte to tak a není přímo podporované funkce Xamarin.Forms sám sebe.

### <a name="views"></a>zobrazení

Je podobný visual hierarchie pro obě rozhraní. WPF je trochu podrobněji kvůli podpoře jeho WYSIWYG dokumentů.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifcycle"></a>Zobrazení Lifcycle

Xamarin.Forms je primárně orientovány kolem mobilních situacích. Jako takový aplikace jsou _aktivovat_, _pozastaveno_, a _znovu aktivovat_ jako uživatel pracuje s nimi. Toto je obdobou klepnutí na směrem od `Window` v aplikaci WPF a sadu metod a odpovídající události můžete přepsat nebo připojit do monitorování toto chování.

| Účel | WPF – metoda | Metoda Xamarin.Forms |
|--- |--- |--- |
|Původní aktivace|konstruktoru + Window.OnLoaded|konstruktoru + Page.OnStart|
|Zobrazí|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Pozastavení nebo ztráty fokusu|Window.OnDeactivated|Page.OnSleep|
|Aktivovat nebo máte odepřený fokusu|Window.OnActivated|Page.OnResume|
|Zavřeno|Window.OnClosing + Window.OnClosed|není k dispozici|


Obě podporu skrytí nebo zobrazení podřízených ovládacích prvků také v grafickém subsystému WPF jde o tři stavu vlastnost `IsVisible` (viditelné, skrytý a sbalené). V Xamarin.Forms, je právě viditelný nebo skrytý prostřednictvím `IsVisible` vlastnost.

### <a name="layout"></a>Rozložení

Rozložení stránky nastane stejný 2-průchodu (měr nebo uspořádat) se děje v grafickém subsystému WPF. Můžete připojit do rozložení stránky přepsáním následující metody v platformě Xamarin.Forms `Page` třídy:

| Metoda | Účel |
|--- |--- |
|OnChildMeasureInvalidated|Upřednostňovanou velikost podřízenou změnil.|
|OnSizeAllocated|Stránka byla přiřazena šířky a výšky.|
|LayoutChanged událostí|Došlo k změně rozložení na velikost stránky.|

Existuje není žádná událost globální rozložení, kterému se říká dnes, ani je k dispozici jako globální `CompositionTarget.Rendering` události, jako je nalezena v grafickém subsystému WPF.

#### <a name="common-layout-properties"></a>Běžné vlastnosti rozložení

WPF a Xamarin.Forms podporovaly `Margin` k řízení mezery okolo elementu, a `Padding` k řízení mezery _uvnitř_ elementu. Kromě toho většina zobrazení rozložení Xamarin.Forms má vlastnosti pro řízení mezery (např. řádek nebo sloupec).

Kromě toho většina elementů mít vlastnosti k ovlivnění, jak jsou umístěny do nadřazeného kontejneru:

| WPF | Xamarin.Forms | Účel |
|--- |--- |--- |
|HorizontalAlignment –|HorizontalOptions|Možnosti doleva nebo Center/vpravo/Stretch|
|VerticalAlignment|VerticalOptions|Možnosti horní nebo Center nebo dolní nebo Stretch|

> [!NOTE]
> Skutečné výklad tyto vlastnosti závisí na nadřazený kontejner.

#### <a name="layout-views"></a>Rozložení zobrazení

WPF a Xamarin.Forms pomocí rozložení ovládacích prvků na pozici podřízené elementy. Ve většině případů tyto jsou velmi podobné navzájem z hlediska funkce.

| WPF | Xamarin.Forms | Stylu rozložení |
|--- |--- |--- |
|StackPanel|StackLayout|Zleva doprava nebo shora dolů nekonečné překrývání|
|Mřížka|Mřížka|Tabulkovém formátu (řádků a sloupců)|
|DockPanel|není k dispozici|Hrany okno ukotvení|
|Plátno|AbsoluteLayout|Pixelu nebo souřadnici umístění|
|WrapPanel|není k dispozici|Zabalení zásobníku|
|není k dispozici|RelativeLayout|Relativní umístění založený na pravidlech|

> [!NOTE]
> Xamarin.Forms nepodporuje `GridSplitter`.

Použít obě platformy _přidružené vlastnosti_ a systém doladit podřízené objekty.

### <a name="rendering"></a>Vykreslování

Vykreslování mechanismů pro WPF a Xamarin.Forms se výrazně liší. V grafickém subsystému WPF vykreslení ovládacích prvků, které vytvoříte přímo obsah tak, aby pixelů na obrazovce. WPF udržuje dva objektu grafy (_stromy_) představují to – _logického stromu_ reprezentuje ovládací prvky, jak jsou definovány v kódu nebo v jazyce XAML a _vizuálním stromu_ představuje skutečné vykreslování, který se nachází na obrazovce, která je buď přímo vizuální prvek provést (prostřednictvím metody virtuální kreslení), nebo prostřednictvím XAML definované `ControlTemplate` která může nahradit nebo přizpůsobit. Obvykle je vizuálním stromu složitější to zahrnuje například ohraničení kolem ovládací prvky, popisky pro implicitní obsah atd. WPF obsahuje sadu rozhraní API (`LogicalTreeHelper` a `VisualTreeHelper`) a zkontrolujte tyto dvě objektu grafy.

V Xamarin.Forms, ovládací prvky definujete v `Page` jsou pouze jednoduché datové objekty. Jsou podobné k reprezentaci logického stromu, ale nikdy vykreslení obsahu samostatně. Místo toho jsou _datový model_ který vliv vykreslování elementů. Je potřeba skutečná vykreslování [samostatné sady _visual nástroji pro vykreslování_ které jsou namapované na každý – typ ovládacího prvku](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Tyto nástroji pro vykreslování registrovaných v každé z projektů specifických pro platformy v sestavení Xamarin.Forms specifické pro platformu. Zobrazí se seznam [zde](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). Kromě výměna nebo rozšíření zobrazovací jednotky, má Xamarin.Forms taky podporu pro [důsledky](~/xamarin-forms/app-fundamentals/effects/index.md) který lze použít k ovlivnění nativní vykreslování na základě jednotlivé platformy.

#### <a name="the-logicalvisual-tree"></a>Strom logického/Visual

Neexistuje žádné zveřejněné API vás logického stromu v Xamarin.Forms - ale reflexe můžete získat stejné informace. Například [tady je metoda, která můžete vytvořit výčet logické podřízené](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) pomocí reflexe.

## <a name="graphics"></a>Grafika

Xamarin.Forms nezahrnuje grafika systému pro primitiva nad rámec obdélníku jednoduché (`BoxView`). 3. stran knihovny můžete použít například [SkiaSharp](~/graphics-games/skiasharp/index.md) získat 2D kreslení a platformy, nebo [UrhoSharp](~/graphics-games/urhosharp/index.md) pro 3D.

## <a name="resources"></a>Prostředky

WPF a Xamarin.Forms mají obě koncept prostředků a slovnících prostředků. Můžete umístit jakéhokoli typu objektu do `ResourceDictionary` s klíčem a potom vyhledejte si s `{StaticResource}` pro věcí, které nedojde ke změně, nebo `{DynamicResource}` pro věcí, které můžete změnit ve slovníku za běhu. Využití a mechanismy jsou stejné s jedním rozdílem: Xamarin.Forms vyžaduje definování `ResourceDictionary` přiřadit `Resources` vlastnost zatímco WPF předem vytvoří jednu a přiřadí ji za vás.

Například viz definice níže:

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

Pokud nezadáte `ResourceDictionary`, je generována chyba za běhu.

## <a name="styles"></a>Styly

Styly jsou také plně podporovaný v Xamarin.Forms a lze použít k motiv Xamarin.Forms prvky, které tvoří uživatelského rozhraní. Podporují dědičnosti (vlastnost, události a data), aktivační události prostřednictvím `BasedOn`a vyhledávání prostředků pro hodnoty. Styly se použijí na elementy buď explicitně prostřednictvím `Style` vlastnost nebo implicitely není zadáním klíč prostředku – stejně jako WPF.

### <a name="device-styles"></a>Styly zařízení

WPF obsahuje sadu předdefinovaných vlastností (uložené jako statické hodnoty na sadu statické třídy, jako `SystemColors`) každém barvy, písma a metriky ve formě hodnoty a klíče prostředků, které určují. Xamarin.Forms se podobá, ale definuje sadu [zařízení styly](~/xamarin-forms/user-interface/styles/device.md) představují stejné operace. Tyto styly jsou poskytl frameowrk a nastavte hodnoty na základě prostředí runtime (například dostupnost).

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
