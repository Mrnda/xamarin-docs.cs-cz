---
title: Vytváření Fragment
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: eae25dbfaf1125191a83b3cc4326abc19105f22f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-fragment"></a>Vytváření Fragment

Pokud chcete vytvořit Fragment, musí třída dědit z `Android.App.Fragment` a potom přepsáním `OnCreateView` metoda. `OnCreateView` bude volána hostování aktivitou, když je na čase put Fragment na obrazovce a vrátí `View`. Typické `OnCreateView` vytvoří tuto položku `View` nafouknutí soubor rozložení a připojíte ho k nadřazený kontejner. Vlastnosti kontejneru jsou důležité jako Android uplatní na rozhraní fragmentu parametry rozložení nadřazeného objektu. Následující příklad ilustruje toto:

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

Výše uvedený kód bude narůstat zobrazení `Resource.Layout.Example_Fragment`a přidejte ji jako podřízené zobrazení `ViewGroup` kontejneru.


> [!NOTE]
> Fragment dílčí třídy musí mít výchozí veřejný konstruktor bez argumentů.

## <a name="adding-a-fragment-to-an-activity"></a>Přidání fragmentu k aktivitě

Existují dva způsoby, může být hostována Fragment uvnitř aktivity:

-   **Deklarativně** &ndash; fragmenty lze deklarativně uvnitř `.axml` soubory rozložení pomocí `<Fragment>` značky.

-   **Prostřednictvím kódu programu** &ndash; fragmenty může být také dynamicky vytvořena pomocí `FragmentManager` třídy a rozhraní API.

Programová využití prostřednictvím `FragmentManager` třída bude probírat později v této příručce.

### <a name="using-a-fragment-declaratively"></a>Fragment deklarativně pomocí

Přidání fragmentu uvnitř rozložení vyžaduje použití `<fragment>` značky a určením Fragment zadáním buď `class` atribut nebo `android:name` atribut. Následující fragment kódu ukazuje způsob použití `class` atribut deklarovat `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Tato další fragment kódu ukazuje, jak deklarovat `fragment` pomocí `android:name` atribut k identifikaci Fragment třídy:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Při vytvoření aktivity se Android doložit každý Fragment zadaný v souboru rozložení a vložit zobrazení, který je vytvořený z `OnCreateView` místě `Fragment` elementu.
Fragmenty deklarativně přidané do aktivity jsou statické a zůstane zachována na aktivitu, dokud byla; není možné dynamicky nahradit nebo odebrat takové Fragment po dobu platnosti na aktivitu, ke kterému je připojen.

Každý Fragment musí být přiřazen jedinečný identifikátor:

-  **Android: id** &ndash; stejně jako u jiných prvky uživatelského rozhraní v souboru rozložení, to je jedinečný identifikátor.

-  **Android: značka** &ndash; tento atribut je do jedinečného řetězce.

Pokud žádná z předchozích dvou metod se používá, bude předpokládat Fragment ID kontejneru zobrazení. V následujícím příkladu kde ani `android:id` ani `android:tag` je zadáno, bude Android přiřadit ID `fragment_container` na Fragment:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="+@id/fragment_container"
                android:orientation="horizontal"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

        <fragment class="com.example.android.apis.app.TitlesFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
</LinearLayout>
```

### <a name="package-name-case"></a>Případ název balíčku

Android nepovoluje velká písmena v názvech balíček; Při pokusu o rozšířené zobrazení, pokud se název balíčku obsahuje velké písmeno vyvolá výjimku. Ale Xamarin.Android dovolí více a budou tolerovat velká písmena v oboru názvů.

Například obě následující fragmenty bude fungovat s Xamarin.Android. Ale druhý fragment kódu způsobí, že `android.view.InflateException` vyvolání čistý Android aplikace založené na jazyce Java.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

NEBO

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>Fragment životního cyklu

Fragmenty mají své vlastní životního cyklu, který je poněkud nezávislé, ale stále ovlivněných, [životního cyklu hostování aktivity](~/android/app-fundamentals/activity-lifecycle/index.md).
Například když aktivita pozastaví, všechny její přidružené fragmenty jsou pozastavena. Následující diagram popisuje životní cyklus Fragment.

[![Vývojový diagram ilustrující Fragment životního cyklu](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>Fragment vytvoření životního cyklu metody

Níže uvedeného seznamu zobrazuje tok různé zpětných volání v životním cyklu Fragment, jako je vytvořena:

-   **`OnInflate()`** &ndash; Voláno, když je Fragment vytváří jako součást rozložení zobrazení. Toto může být voláno ihned po Fragment vytvoření deklarativně ze souboru XML rozložení. Fragment ještě není přidružen k jeho aktivity ale **aktivity**, **sady**, a **AttributeSet** ze zobrazení hierarchie se předávají v jako parametry. Tato metoda je nejvhodnější pro analýzu **AttributeSet** a pro ukládání atributy, které by mohly používat později Fragment.

-   **`OnAttach()`** &ndash; Volá se po Fragment je spojené s aktivitou. Toto je první metoda má být spuštěn při Fragment je připravený k použití. Obecně platí by neměl fragmenty implementovat konstruktor nebo přepsat výchozí konstruktor. Tato metoda se musí inicializovat všech komponent, které jsou požadovány pro Fragment.

-   **`OnCreate()`** &ndash; Voláno rozhraním aktivity k vytvoření Fragment. Když tato metoda je volána, zobrazení hierarchie hostování aktivity nemusí být vytvořena instance úplně, takže Fragment neměli spoléhat na všechny části aktivity zobrazení hierarchie až později v průběhu životního cyklu ve fragmentu. Například nepoužívejte tuto metodu provést jakékoli vylepšení nebo úpravy uživatelského rozhraní aplikace. Toto je Nejdřívější čas, kdy může Fragment zahájit shromažďování dat, které potřebuje. Fragment běží v vlákna uživatelského rozhraní v tomto okamžiku, proto nedocházelo k žádné zdlouhavé zpracování nebo proveďte toto zpracování na vlákna na pozadí. Tato metoda může být přeskočeny, pokud **SetRetainInstance(true)** je volána.
    Tato alternativní budou popsané v podrobněji níže.

-   **`OnCreateView()`** &ndash; Vytvoří zobrazení pro Fragment.
    Tato metoda je volána jednou aktivity **OnCreate()** metoda je dokončena. V tomto okamžiku je bezpečné pro interakci s hierarchii zobrazení aktivity. Tato metoda by měla vrátit zobrazení, který se použije Fragment.

-   **`OnActivityCreated()`** &ndash; Volá se po **Activity.OnCreate** dokončení hostování aktivitou.
    V tuto chvíli je třeba provést poslední vylepšení s uživatelským rozhraním.

-   **`OnStart()`** &ndash; Volá se po obsahující aktivitu byl obnoven. Díky tomu Fragment viditelné pro uživatele. V mnoha případech bude obsahovat Fragment kódu, které by jinak v **OnStart()** metoda aktivity.

-   **`OnResume()`** &ndash; Toto je poslední metoda volána, než může uživatel zasahovat Fragment. Příkladem druh kódu, která má být provedena u této metody by povolení funkce zařízení, které může uživatel komunikovat, jako je například fotoaparát, umístění služeb. Službám, jako je to může způsobit nadměrné baterie vyprazdňování, ale a aplikace by měla minimalizovat jejich použití k prodlužovat výdrž.


### <a name="fragment-destruction-lifecycle-methods"></a>Fragment odstraňování životního cyklu metody

Další seznam vysvětluje metod životního cyklu, které se říká zničena Fragment:

-   **`OnPause()`** &ndash; Uživatel je již nebude moci pracovat s Fragment. Tuto situaci existuje, protože jiné operace. Fragment provádí změny v tomto fragmentu nebo hostitelských aktivity je pozastavena. Je možné, že aktivita hostování Fragment nemusí být ještě viditelné, to znamená, aktivity v fokus je částečně transparentní nebo nezabírá na celé obrazovce. Když tato metoda stane aktivní, je první znamenat, že uživatel po skončení Fragment. Fragment měli uložit provedené změny.

-   **`OnStop()`** &ndash; Fragment již není viditelný. Hostitel aktivity je pravděpodobně zastavená nebo operaci Fragment provádí změny v aktivitě. Tato zpětného volání slouží ke stejnému účelu jako **Activity.OnStop**.

-   **`OnDestroyView()`** &ndash; Tato metoda je volána k vyčištění prostředky přidružené k zobrazení. Volá se, když byl zničen zobrazení související s Fragment.

-   **`OnDestroy()`** &ndash; Tato metoda je volána, když Fragment je již používán. Je stále spojena se aktivity, ale Fragment již není funkční. Tato metoda by měla uvolnění prostředků, které jsou používány Fragment, například [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) , může být použito k fotoaparátu. Tato metoda může být přeskočeny, pokud **SetRetainInstance(true)** je volána. Tato alternativní budou popsané v podrobněji níže.

-   **`OnDetach()`** &ndash; Tato metoda je volána těsně před Fragment je již přidružen aktivity. Zobrazení hierarchie fragmentu již neexistuje a všechny prostředky, které jsou používány Fragment by měly být v tomto okamžiku uvolněny.


### <a name="using-setretaininstance"></a>Pomocí SetRetainInstance

Je možné pro Fragment k určení, že by neměl Pokud aktivity je znovu vytvořit zničen úplně. `Fragment` Třída poskytuje metody `SetRetainInstance` pro tento účel. Pokud `true` je předaná této metodě, pak při restartování aktivity se použije stejnou instanci Fragment. Pokud k tomu dojde, pak všechny metody zpětného volání, který bude vyvolán s výjimkou `OnCreate` a `OnDestroy` zpětná volání životního cyklu. Tento proces je znázorněn v diagramu životního cyklu výše uvedenou (zelený tečkovaná řádky).


## <a name="fragment-state-management"></a>Fragment stav správy

Fragmenty může uložení a obnovení stavu během cyklu Fragment pomocí instance `Bundle`. Sada umožňuje Fragment uložit data jako páry klíč/hodnota a je vhodný pro jednoduché data, která nevyžaduje mnoho paměti. Fragment můžete uložit stav pomocí volání `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

Novou instanci třídy Fragment vytvoření, uloží do stavu `Bundle` bude k dispozici do nové instance prostřednictvím `OnCreate`, `OnCreateView`, a `OnActivityCreated` metody nové instance.
Následující příklad ukazuje, jak načíst hodnotu `current_choice` z `Bundle`:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    if (savedInstanceState != null)
    {
        _currentCheckPosition = savedInstanceState.GetInt("current_choice", 0);
    }
}
```

Přepsání `OnSaveInstanceState` je vhodný mechanismus pro ukládání dat přechodný v Fragment napříč orientaci změny, jako `current_choice` hodnota v předchozím příkladu. Nicméně výchozí implementaci `OnSaveInstanceState` postará ukládání přechodný dat v uživatelském rozhraní pro každé zobrazení, který má ID přiřazené. Například, podívejte se na aplikace, která má `EditText` element definovaný v XML následujícím způsobem:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

Vzhledem k tomu `EditText` má ovládací prvek `id` přiřazen, Fragment automaticky uloží data ve widgetu při `OnSaveInstanceState` je volána.


### <a name="bundle-limitations"></a>Sadu omezení

I když pomocí `OnSaveInstanceState` díky snadno uložit přechodný dat pomocí této metody má určitá omezení:

-  Pokud Fragment nebyla přidána do back zásobníku, pak je její stav nebude obnoven, když uživatel stiskne **zpět** tlačítko.

-  Pokud se sada použije k uložení dat, je serializováno tato data. To může vést k zpracování zpoždění.


## <a name="contributing-to-the-menu"></a>Přidání do nabídky

Fragmenty může přispět položky nabídky jejich hostování aktivity.
Aktivita zpracovává první položky nabídky. Pokud aktivita nemá obslužnou rutinu, pak události se předá Fragment, který bude poté zpracovávat.

Chcete-li přidat položky do nabídky aktivity, musíte udělat Fragment dvě věci.
Nejprve Fragment musí implementovat metodu `OnCreateOptionsMenu` a umístěte jeho položky do nabídky, jak je znázorněno v následujícím kódu:

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

V nabídce v předchozím fragmentu kódu je zvětšený z následující kód XML, nachází v souboru `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

V dalším kroku musí volat Fragment `SetHasOptionsMenu(true)`. Volání této metody ohlášen Android, zda Fragment má položky nabídky přispívání do nabídky možnost. Pokud je uskutečněn hovor této metodě, položky nabídky pro Fragment nepřidají do nabídky možnost aktivity. To se obvykle provádí v metodě životního cyklu `OnCreate()`, jak ukazuje následující fragment kódu:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

Následující obrazovka ukazuje, jak tato nabídka vypadat:

[![Příklad snímek obrazovky aplikace My služebních cest zobrazení položky nabídky](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
