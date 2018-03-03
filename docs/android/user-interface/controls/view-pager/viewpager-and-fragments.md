---
title: ViewPager fragmenty
description: "ViewPager je rozložení správce, který umožňuje implementovat posunkové navigace. Posunkové navigační umožňuje uživateli prstem levé a pravé krok prostřednictvím stránky data. Tato příručka vysvětluje, jak implementovat swipeable uživatelského rozhraní s ViewPager, pomocí fragmentů jako data stránky."
ms.topic: article
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 9b200bd335ea65bf46de00d2dc7382b7f838716b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="viewpager-with-fragments"></a>ViewPager fragmenty

_ViewPager je rozložení správce, který umožňuje implementovat posunkové navigace. Posunkové navigační umožňuje uživateli prstem levé a pravé krok prostřednictvím stránky data. Tato příručka vysvětluje, jak implementovat swipeable uživatelského rozhraní s ViewPager, pomocí fragmentů jako data stránky._

<a name="overview" />
 
## <a name="overview"></a>Přehled

`ViewPager` se často používá ve spojení s fragmenty, aby bylo snazší správa životního cyklu každé stránky `ViewPager`. V tomto návodu `ViewPager` se používá k vytvoření názvem aplikace **FlashCardPager** , uvede řadu problémů matematické karty flash. Každou kartu flash je implementovaný jako fragment. Swipes doleva a doprava prostřednictvím karty flash a klepne na matematické problému a jeho odpovědí odhalit uživatele. Tato aplikace vytvoří `Fragment` instance pro každý adaptér odvozené z karty flash a implementuje `FragmentPagerAdapter`. V [Viewpager a zobrazení](~/android/user-interface/controls/view-pager/viewpager-and-views.md), většinu práce bylo provedeno `MainActivity` metody životního cyklu. V **FlashCardPager**, se provádí většinu práce `Fragment` v jedné z metod jeho životního cyklu. 

Tato příručka nepopisuje základy fragmenty &ndash; Pokud ještě nejste obeznámeni s fragmenty v Xamarin.Android, přečtěte si téma [fragmenty](~/android/platform/fragments/index.md) můžete začít pracovat s fragmenty. 


<a name="start" />

## <a name="start-an-app-project"></a>Spusťte projekt aplikace

Vytvořit nový projekt Android s názvem **FlashCardPager**. Potom spusťte Správce balíčků NuGet (Další informace o instalaci balíčků NuGet najdete v tématu [návod: včetně NuGet ve vašem projektu](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Najít a nainstalovat **Xamarin.Android.Support.v4** balíček, jak je popsáno v [Viewpager a zobrazení](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 


<a name="datasource" />

## <a name="add-an-example-data-source"></a>Přidat k příklad zdroji dat

V **FlashCardPager**, zdroj dat je balíčku karet flash reprezentována `FlashCardDeck` třída; tato data zdroje zdroje `ViewPager` s obsahem položky. `FlashCardDeck` obsahuje kolekci připravených matematické problémy a odpovědi. `FlashCardDeck` Konstruktor požaduje žádné argumenty: 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

Kolekce karet flash v `FlashCardDeck` je organizovaná tak, aby každou kartu flash přístupná pomocí indexer. Například následující kód načte čtvrtý problém flash karty z balíčku: 

```csharp
string problem = flashCardDeck[3].Problem;
```

Tento řádek kódu načte odpovídající odpověď na předchozí problému:

```csharp
string answer = flashCardDeck[3].Answer;
```

Protože podrobnosti implementace `FlashCardDeck` nejsou důležité k porozumění `ViewPager`, `FlashCardDeck` kód není tady.
Zdrojový kód a `FlashCardDeck` je k dispozici na [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Stáhněte si tento zdrojový soubor (nebo zkopírujte a vložte kód do nové **FlashCardDeck.cs** souboru) a přidejte ji do projektu.


<a name="layout" />

## <a name="create-a-viewpager-layout"></a>Vytvoření ViewPager rozložení

Otevřete **Resources/layout/Main.axml** a nahraďte jeho obsah následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

Tato konfigurace XML definuje `ViewPager` který zabírá celou obrazovku. Všimněte si, že je nutné použít plně kvalifikovaný název **android.support.v4.view.ViewPager** protože `ViewPager` je součástí knihovny podpory. `ViewPager` je k dispozici pouze [podporu knihovna pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); není k dispozici v systému Android SDK.


<a name="setup" />

## <a name="set-up-viewpager"></a>Nastavit ViewPager

Upravit **MainActivity.cs** a přidejte následující `using` příkazy:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Změna `MainActivity` deklarace tak, aby je odvozená od třídy `AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` je odvozený od`FragmentActivity` (místo `Activity`) protože `FragmentActivity` umí ke správě podpoře fragmenty. Nahraďte `OnCreate` metoda následujícím kódem: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

Tento kód provede následující akce:

1.  Nastaví zobrazení **Main.axml** rozložení prostředků.

2.  Získá odkaz na `ViewPager` z rozložení.

3.  Vytvoří nový `FlashCardDeck` jako zdroj dat.

Když sestavíte a spustíte tento kód, byste měli vidět zobrazení, která se podobá následující snímek obrazovky: 

[![Snímek obrazovky FlashCardPager aplikace pomocí prázdné ViewPager](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png)

V tomto okamžiku `ViewPager` je prázdný vzhledem k tomu, že je nedostatečná fragmenty, které se používají naplnit `ViewPager`, a adaptér je nedostatečná pro vytvoření tyto fragmenty z dat v **FlashCardDeck**. 

V následujících částech `FlashCardFragment` vytvoří implementovat funkci každou kartu flash a `FragmentPagerAdapter` se vytvoří pro připojení `ViewPager` do vytvořené na základě dat v fragmenty `FlashCardDeck`. 


<a name="fragment" />

## <a name="create-the-fragment"></a>Vytvořit Fragment

Každou kartu flash bude spravovat fragment uživatelského rozhraní, která je volána `FlashCardFragment`. `FlashCardFragment`pro zobrazení se zobrazí informace obsažené s jedna karta flash. Každá instance `FlashCardFragment` hostitelem bude `ViewPager`. 
`FlashCardFragment`na zobrazení bude obsahovat `TextView` který zobrazí text problém flash karty. Toto zobrazení bude implementaci obslužné rutiny události, který používá `Toast` k zobrazení odpovědi, když uživatel klepnutím na flash karty otázku. 


<a name="layout" />

### <a name="create-the-flashcardfragment-layout"></a>Vytvoření FlashCardFragment rozložení

Před `FlashCardFragment` lze implementovat, musí být definován jeho rozložení. Toto rozložení je rozložení fragment kontejner pro jeden fragment. Přidat nové Android rozložení pro **prostředky, rozvržení** názvem **flashcard_layout.axml**. Otevřete **Resources/layout/flashcard_layout.axml** a nahraďte jeho obsah následujícím kódem: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

Toto rozložení definuje fragment jedna karta flash; Každý fragment se skládá z `TextView` , zobrazí se matematické potíže při použití písmo velké (100sp). Tento text je na střed vodorovně a svisle na kartě flash. 


<a name="fcfclass" />

### <a name="create-the-initial-flashcardfragment-class"></a>Vytvoření počáteční FlashCardFragment třídy

Přidat nový soubor s názvem **FlashCardFragment.cs** a nahraďte jeho obsah následujícím kódem:

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

Tento kód zástupných procedur se jejich hlavních `Fragment` definice, který se použije k zobrazení karet flash. Všimněte si, že `FlashCardFragment` je odvozený od verze knihovny podpory `Fragment` definované v `Android.Support.V4.App.Fragment`. Konstruktor je prázdný, aby `newInstance` metoda factory slouží k vytvoření nové `FlashCardFragment` místo konstruktor. 

`OnCreateView` Životního cyklu metoda vytvoří a nakonfiguruje `TextView`. Je zvýšení kapacity rozložení pro ve fragmentu `TextView` a vrátí zvýšeným `TextView` volajícímu. `LayoutInflater` a `ViewGroup` jsou předávány `OnCreateView` tak, aby ho můžete zvýšilo rozložení. `savedInstanceState` Sada obsahuje data, `OnCreateView` k opětovnému vytvoření používá `TextView` z uloženého stavu. 

Ve fragmentu zobrazení je explicitně zvětšený voláním `inflater.Inflate`. `container` Argument je nadřazeným prvkem zobrazení a `false` příznak dá pokyn inflater nepoužívejte přidávání zvýšeným zobrazení do nadřazeného zobrazení (přidá se při `ViewPager` volání je adaptéru `GetItem` metoda později v tomto návod). 


<a name="state" />

### <a name="add-state-code-to-flashcardfragment"></a>Přidejte do FlashCardFragment stavu kód

Stejně jako aktivity, fragment má `Bundle` tak používá k uložení a načtení stavu. V **FlashCardPager**tento `Bundle` se používá k uložení na otázku a odpovědět textu pro přidružené kartu flash. V **FlashCardFragment.cs**, přidejte následující `Bundle` klíče do horní části `FlashCardFragment` definici třídy: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Změnit `newInstance` metoda factory, které se vytvoří `Bundle` objektu a používá k ukládání předaný otázku a odpovězte textové ve fragmentu po vytvoření instance výše klíče: 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

Změňte metodu životního cyklu fragment `OnCreateView` k načtení těchto informací ze sady předané a načtení text otázky do `TextBox`: 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

`answer` Proměnná se zde nepoužívá, ale použije se později při přidání kód obslužné rutiny události pro tento soubor. 


<a name="adapter" />

## <a name="create-the-adapter"></a>Vytvoření adaptéru

`ViewPager` používá objekt řadič adaptéru, která se nachází mezi `ViewPager` a zdroj dat (viz obrázek v ViewPager [adaptér](~/android/user-interface/controls/view-pager/index.md#adapter) článku). Pro přístup k těmto datům `ViewPager` vyžaduje zadání vlastní adaptér odvozen od `PagerAdapter`. Protože tento příklad používá fragmenty, používá `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` je odvozený od `PagerAdapter`. 
`FragmentPagerAdapter` představuje každé stránce jako `Fragment` , je trvale uložen v fragment správce pro tak dlouho, dokud uživatel může vrátit na stránku. Jako uživatel swipes prostřednictvím stránky `ViewPager`, `FragmentPagerAdapter` extrahuje informace ze zdroje dat a používá k vytvoření `Fragment`s pro `ViewPager` k zobrazení. 

Při implementaci `FragmentPagerAdapter`, je nutné přepsat následující:

-   **Počet** &ndash; vlastnosti jen pro čtení, která vrátí počet dostupných zobrazení (stránky).

-   **GetItem** &ndash; vrátí fragment k zobrazení pro zadanou stránku.

Přidat nový soubor s názvem **FlashCardDeckAdapter.cs** a nahraďte jeho obsah následujícím kódem:

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

Tento kód zástupných procedur se jejich hlavních `FragmentPagerAdapter` implementace. V následujících částech každá z těchto metod se nahradí funkční kód. Účelem konstruktoru je předat správce fragment k `FlashCardDeckAdapter`pro konstruktor základní třídy. 


<a name="ctor" />

### <a name="implement-the-adapter-constructor"></a>Implementace adaptér konstruktoru

Když aplikace vytvoří `FlashCardDeckAdapter`, poskytne odkaz na fragment správce a vytvořenou instanci `FlashCardDeck`. Přidejte následující členské proměnné do horní části `FlashCardDeckAdapter` třídy v **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Přidejte následující řádek kódu `FlashCardDeckAdapter` konstruktor: 

```csharp
this.flashCardDeck = flashCards;
```

Tento řádek kódu úložišť `FlashCardDeck` instanci `FlashCardDeckAdapter` bude používat. 


<a name="count" />

### <a name="implement-count"></a>Počet implementací

`Count` Implementace je poměrně jednoduché: Vrátí počet karet flash v flash karet. Nahraďte `Count` následujícím kódem: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards` Vlastnost `FlashCardDeck` vrátí počet karet flash (počet fragmentů) v datové sadě. 


<a name="getitem" />

### <a name="implement-getitem"></a>GetItem – implementace

`GetItem` Metoda vrátí fragment přidružené k dané pozici. Když `GetItem` se nazývá pozice v flash karet vrátí `FlashCardFragment` nakonfigurován k zobrazení karet flash problém na této pozici. Nahraďte `GetItem` metoda následujícím kódem: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Tento kód provede následující akce:

1.  Vyhledá řetězec matematické problém v `FlashCardDeck` podlaží pro konkrétní pozici. 

2.  Vyhledá řetězec odpovědí v `FlashCardDeck` podlaží pro konkrétní pozici. 

3.  Volání `FlashCardFragment` metoda factory `newInstance`, předejte v řetězcích flash karty problému a odpovědí. 

4.  Vytvoří a vrátí novou kartu flash `Fragment` obsahující text otázku a odpověď pro tuto pozici. 

Při `ViewPager` vykreslí `Fragment` v `position`, se zobrazí `TextBox` obsahující řetězec matematické problém, které se nacházejí v `position` v flash karet. 


<a name="addadapter" />

## <a name="add-the-adapter-to-the-viewpager"></a>Přidejte do ViewPager adaptéru

Teď, když `FlashCardDeckAdapter` je implementována, je třeba ho přidat do `ViewPager`. V **MainActivity.cs**, přidejte následující řádek kódu na konec `OnCreate` metoda:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Tento kód vytvoří `FlashCardDeckAdapter`a předejte `SupportFragmentManager` v prvním argumentu. ( `SupportFragmentManager` Vlastnosti FragmentActivity lze získat odkaz na `FragmentManager` &ndash; Další informace o `FragmentManager`, najdete v části [Správa fragmenty](~/android/platform/fragments/managing-fragments.md).) 

Základní implementace je nyní dokončen &ndash; sestavení a spuštění aplikace.
Měli byste vidět první obrázek flash karet zobrazují na obrazovce, jak je znázorněno na levé straně na další snímku obrazovky. Prstem zleva najdete v části Další karet flash, pak prstem práva na přechod na předchozí flash karet:

[![Příklad snímky obrazovky aplikace FlashCardPager bez pager ukazatele](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png)


<a name="pagetabstrip" />

## <a name="add-a-pager-indicator"></a>Přidat na Pager ukazatel

Tento minimální `ViewPager` implementace zobrazí každou kartu flash z balíčku, ale neobsahuje žádnou indikaci, kde se uživatel nachází v balíčku. Dalším krokem je přidání `PagerTabStrip`. `PagerTabStrip` Informuje uživatele, které problém číslo se zobrazí a poskytuje kontext navigační tím, že zobrazuje nápovědu flash karet předchozí a další. 

Otevřete **Resources/layout/Main.axml** a přidejte `PagerTabStrip` k rozložení:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

  <android.support.v4.view.PagerTabStrip
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:layout_gravity="top"
      android:paddingBottom="10dp"
      android:paddingTop="10dp"
      android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

Při sestavení a spuštění aplikace, měli byste vidět prázdné `PagerTabStrip` zobrazí v horní části každé kartě flash: 

[![Closeup PagerTabStrip bez textu](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png)


<a name="title" />

### <a name="display-a-title"></a>Zobrazí nadpis

Chcete-li přidat název na každé kartě stránky, implementovat `GetPageTitleFormatted` metoda v adaptéru. `ViewPager` volání `GetPageTitleFormatted` (Pokud je implementována) k získání názvu řetězec, který popisuje stránku na zadané pozici. Přidejte následující metodu do `FlashCardDeckAdapter` třídy v **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Tento kód převádí pozice v flash karet na problém. Výsledný řetězec je převeden na Java `String` , je vrácen do `ViewPager`. Při spuštění aplikace s Tato nová metoda, každé stránce zobrazuje číslo problém v `PagerTabStrip`: 

[![Snímky obrazovky FlashCardPager číslem problém zobrazí výše každé stránce](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png)

Můžete přepínat prstem zobrazíte číslo problém z karty flash balíčku, který se zobrazí v horní části každé kartě flash. 


<a name="userinput" />

## <a name="handle-user-input"></a>Zpracování uživatelského vstupu

**FlashCardPager** uvede řady na základě fragment karty flash v `ViewPager`, ale ještě nemá způsob, jak odhalit odpověď pro jednotlivé problémy. V této části je do obslužné rutiny události `FlashCardFragment` k zobrazení odpovědi, když uživatel klepnutím na text problém flash karty. 

Otevřete **FlashCardFragment.cs** a přidejte následující kód do konce `OnCreateView` metoda těsně před zobrazení je vrácen volajícímu: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

To `Click` obslužné rutiny události se zobrazí odpověď v informační zprávy, která se zobrazí, když uživatel klepnutím `TextBox`. `answer` Proměnná byl inicializován dříve, pokud byl načten informace o stavu ze sady, který byl předán `OnCreateView`. Sestavení a spuštění aplikace a potom klepněte na text problém na každé kartě flash zobrazíte odpověď: 

[![Snímky obrazovky FlashCardPager aplikace informační zprávy, když je stisknuté matematické problém](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png)

**FlashCardPager** uvedené v tomto návodu používá `MainActivity` odvozené z `FragmentActivity`, ale můžete také odvodit `MainActivity` z `AppCompatActivity` (které také poskytuje podporu pro správu fragmenty). Chcete-li zobrazit `AppCompatActivity` příkladu najdete v části [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) v galerii ukázka. 


<a name="summary" />

## <a name="summary"></a>Souhrn

Tento názorný postup poskytuje podrobný příklad toho, jak vytvořit základní `ViewPager`– na základě aplikace pomocí `Fragment`s. Se zobrazí zdroj dat příklad obsahující flash karty otázky a odpovědi, `ViewPager` rozložení zobrazíte kartách flash a `FragmentPagerAdapter` podtřídami, která se připojuje `ViewPager` ke zdroji dat. Chcete-li uživatel procházet karty flash, byly zahrnuty pokyny která vysvětlují, jak přidat `PagerTabStrip` zobrazíte číslo problém v horní části každé stránce. Nakonec kód pro zpracování událostí byl přidán k zobrazení odpovědi, když uživatel klepnutím na flash karty problém. 



## <a name="related-links"></a>Související odkazy

- [FlashCardPager (ukázka)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
