---
title: Xamarin.Android Performance
description: "Existuje mnoho postupů pro zvýšení výkonu aplikace sestavené s Xamarin.Android. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace. Tento článek popisuje a tyto postupy."
ms.topic: article
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 825b566ed45e8c337a1a452ec2c76a23e6a16462
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/16/2018
---
# <a name="xamarinandroid-performance"></a>Xamarin.Android Performance

_Existuje mnoho postupů pro zvýšení výkonu aplikace sestavené s Xamarin.Android. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace. Tento článek popisuje a tyto postupy._

## <a name="performance-overview"></a>Přehled výkonnostní

Výkon nízký aplikace prezentuje mnoha způsoby. Aplikace může díky pravděpodobně reagovat, může způsobit pomalé posouvání a může snížit z baterie. Ale optimalizace výkonu zahrnuje více než jen implementace efektivní kódu. Musíte také zvážit možnosti pro uživatele s výkonem aplikace. Například zajistíte, že operace spustit bez blokování uživatele z jiné aktivity vám může pomoct vylepšit možnosti pro uživatele.

Existuje několik postupů pro zvýšení výkonu a dosahovaný výkon aplikace sestavené s Xamarin.Android. Mezi ně patří:

- [Optimalizace rozložení hierarchie](#optimizelayout)
- [Optimalizace zobrazení seznamu](#optimizelistviews)
- [Odebrat obslužné rutiny událostí v aktivity](#removeeventhandlers)
- [Omezit životnost služeb](#limitservices)
- [Uvolnění prostředků při upozornění](#releaseresources)
- [Uvolnění prostředků, když je skrytý uživatelského rozhraní](#releaseresourcesuihidden)
- [Optimalizovat prostředky obrázků](#optimizeimages)
- [Uvolnění prostředků nepoužívané bitové kopie](#disposeimages)
- [Vyhněte se aritmetické s plovoucí desetinnou čárkou](#avoidfloats)
- [Zavřít dialogová okna](#dismissdialogs)


> [!NOTE]
> Před přečtení tohoto článku měli nejdřív přečíst [napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md), který popisuje konkrétní techniky jiné platformy ke zlepšení využití paměti a výkon aplikace vytvořené pomocí platformy Xamarin.

<a name="optimizelayout" />

## <a name="optimize-layout-hierarchies"></a>Optimalizace rozložení hierarchie

Každé rozložení přidat do aplikace vyžaduje inicializace, rozložení a kreslení. Rozložení průchodu může být nákladné při vnoření [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) instance, které používají `weight` parametr, protože jednotlivých podřízených se měří dvakrát. Použití vnořených instancí `LinearLayout` může vést k přímé zobrazení hierarchii, což může vést k nižšímu výkonu pro rozložení, které jsou zvětšený vícekrát, například jako v [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/). Proto je důležité, že jsou optimalizované takové rozložení, jako výkonu se pak násobí výhody.

Představte si třeba [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) pro řádek zobrazení seznamu, který má ikonu, název a popis. `LinearLayout` Bude obsahovat [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) a svislé `LinearLayout` obsahující dva [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) instancí:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

Toto rozložení úrovněmi 3 a je plýtvání při zvětšený pro každou [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) řádek. Však může být zvýšena pomocí sloučení rozložení, jak je znázorněno v následujícím příkladu kódu:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

Předchozí 3 úrovně hierarchie byla snížena na 2 úrovni hierarchie a jedinou [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) nahradili dva [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) instance. K výraznému zvýšení výkonu se získávají, když nafouknutí rozložení pro každou [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) řádek.

<a name="optimizelistviews" />

## <a name="optimize-list-views"></a>Optimalizace zobrazení seznamu

Uživatelé očekávají, že plynulé posouvání a rychlé zatížení dobu [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) instance. Ale posouvání výkonu může dojít k když každý řádek zobrazení seznamu obsahuje hluboko vložené zobrazení hierarchie, nebo když obsahovat komplexní rozložení řádků zobrazení seznamu. Existují však technik, které umožňuje vyhnout nízká `ListView` výkonu:

- Znovu použít zobrazení řádek pro další informace najdete v tématu [znovu použít zobrazení řádek](#reuserowviews).
- Vyrovnání rozložení, kde je to možné.
- Obsah řádek mezipaměti, které se načítají z webové služby.
- Vyhněte se škálování bitové kopie.

Kolektivně tyto postupy pomohou zachovat [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) instance posouvání bez problémů.

<a name="reuserowviews" />

### <a name="reuse-row-views"></a>Znovu použít zobrazení řádků

Při zobrazení stovky řádků v [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/), bylo by plýtvání paměti k vytvoření stovky [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) objekty při pouze malý počet je jsou zobrazené na obrazovce najednou. Místo toho pouze `View` objekty, které jsou viditelné v řádcích na obrazovce může být načtena do paměti, se **obsah** načítá do těchto znovu použít objekty. Tím se zabrání instance stovky další objekty, ukládání času a paměti.

Proto když řádek zmizí z obrazovky jeho zobrazení lze umístit do fronty pro opakované použití, jak je znázorněno v následujícím příkladu kódu:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Jako uživatel posune, [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) volání `GetView` přepsat požádat o nový pohledů pro zobrazení – Pokud je k dispozici předává nepoužívané zobrazení v `convertView` parametr. Pokud je tato hodnota `null` pak kód vytvoří novou [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) instanci, jinak `convertView` vlastnosti můžete obnovit a znovu použít.

Další informace najdete v tématu [řádek zobrazení znovu použít](~/android/user-interface/layouts/list-view/populating.md#row-view-re-use) v [naplnění ListView s daty](~/android/user-interface/layouts/list-view/populating.md).

<a name="removeeventhandlers" />

## <a name="remove-event-handlers-in-activities"></a>Odebrat obslužné rutiny událostí v aktivity

Když v modulu runtime Android zničení aktivitu, ho může být stále aktivní v Mono runtime. Proto odebrat obslužných rutin událostí pro externí objekty v `Activity.OnPause` zabránit modulu runtime udržuje odkaz na aktivitu, která byl zničen.

V aktivitě deklarujte handler(s) událostí na úrovni třídy:

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

Potom implementovat obslužné rutiny v aktivity, například v `OnResume`:

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

Při ukončení aktivity do stavu spuštěno `OnPause` je volána. V `OnPause` implementace obslužných rutin odeberte následujícím způsobem:

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices" />

## <a name="limit-the-lifespan-of-services"></a>Omezit životnost služeb

Při spuštění služby Android udržuje proces služba spuštěna. Díky tomu proces nákladné protože jeho paměť se nedá stránkovaného fondu, nebo použít jinde. Opouštění spuštěna služba při není nutné, proto zvyšuje se ohrožení aplikace, který vykazuje nedostatečný výkon z důvodu omezení paměti. Také může být aplikace přepínání méně efektivní, protože snižuje počet procesů, které Android může ukládat do mezipaměti.

Životnost služby může být omezen pomocí `IntentService`, který ukončí sám po jeho byla zpracována záměr, který spustil.

<a name="releaseresources" />

## <a name="release-resources-when-notified"></a>Uvolnění prostředků při upozornění

Během životního cyklu aplikací [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) zpětné volání poskytuje oznámení, když je málo paměti zařízení. Tato zpětného volání by měla být implementována pro naslouchání následující úrovně oznámení paměti:

- [`TrimMemoryRunningModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate/) – aplikace *může* chcete uvolnit některé nepotřebné prostředky.
- [`TrimMemoryRunningLow`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningLow/) – aplikace *by měl* verze nepotřebné prostředky.
- [`TrimMemoryRunningCritical`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical/) – aplikace *by měl* verze tolik nekritické procesy, jak je to možné.

Kromě toho, když proces aplikace se uloží do mezipaměti, následující úrovně oznámení paměti může být přijatých [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) zpětného volání:

- [`TrimMemoryBackground`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryBackground/) – uvolnění prostředků, které mohou být rychle a efektivně znovu sestavit po návratu do aplikace.
- [`TrimMemoryModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryModerate/) – uvolnění prostředků může pomoci zachovat jiné procesy uložená v mezipaměti pro lepší celkový výkon systému.
- [`TrimMemoryComplete`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryComplete/) – proces aplikace bude ukončena brzy, pokud není brzy obnovit více paměti.

Oznámení by měla být odpověděl uvolněním prostředky na základě přijatých úrovně.

<a name="releaseresourcesuihidden" />

## <a name="release-resources-when-the-user-interface-is-hidden"></a>Uvolnění prostředků, když je skrytý uživatelského rozhraní

Uvolněte všechny prostředky využívané třídou uživatelské rozhraní aplikace, když uživatel přejde na jiné aplikaci, jak může značně zvýšit kapacitu pro Android v mezipaměti procesy, které pak mohou mít vliv na kvalitu činnost koncového uživatele.

Pro příjem oznámení při ukončení uživatelem uživatelského rozhraní, implementovat [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) zpětného volání v `Activity` třídy a naslouchat [ `TrimMemoryUiHidden` ](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryUiHidden/) úroveň, která označuje, že je uživatelského rozhraní v zobrazení skrytá. Toto oznámení bude přijímat pouze tehdy, když *všechny* součásti uživatelského rozhraní aplikace skryté od uživatele. Uvolnění prostředků uživatelského rozhraní při příjmu tohoto oznámení zajišťuje, aby v případě, že uživatel přejde zpět z jiné aktivity v aplikaci, prostředky uživatelského rozhraní byly stále k dispozici pro rychle obnovit aktivity.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimalizovat prostředky obrázků

Bitové kopie jsou některé nejnákladnější prostředky, které aplikace používají, a jsou často zaznamenané v vysoké řešení. Proto při zobrazení bitovou kopii, zobrazte ho v řešení vyžaduje obrazovky zařízení. Pokud bitová kopie je vyšší než obrazovky řešení, by měl být škálovat.

Další informace najdete v tématu [optimalizovat prostředky obrázků](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) v [napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md) průvodce.

<a name="disposeimages" />

## <a name="dispose-of-unused-image-resources"></a>Uvolnění prostředků nepoužívané bitové kopie

Pokud chcete uložit na využití paměti, je vhodné k uvolnění prostředků velký obrázek, které už nejsou potřeba. Je důležité zajistit, že jsou správně odstraněny bitové kopie. Místo použití explicitního `.Dispose()` volání, můžete využít výhod [pomocí](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement) příkazy zajistit správné použití `IDisposable` objekty. 

Například [rastrový obrázek](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) třída implementuje `IDisposable`. Zabalení instance `BitMap` objekt v `using` bloku zajistí, že ho bude prodávat správně na ukončení z bloku:

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

Další informace o uvolnění na jedno použití prostředků najdete v tématu [verzi rozhraní IDisposable prostředky](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).  


<a name="avoidfloats" />

## <a name="avoid-floating-point-arithmetic"></a>Vyhněte se aritmetické s plovoucí desetinnou čárkou

Na zařízeních s Androidem s plovoucí desetinnou čárkou aritmetické je o 2 x pomalejší než celočíselné aritmetiky. Proto nahraďte s plovoucí desetinnou čárkou aritmetické celé číslo aritmetické Pokud je to možné. Neexistuje však žádný provádění časový rozdíl mezi `float` a `double` aritmetické na poslední hardwaru.

> [!NOTE]
> I pro celočíselné aritmetiky rozdělují hardwaru chybí některé procesory možnosti. Proto jsou v softwaru často provést operace dělení a modulus celé číslo.

<a name="dismissdialogs" />

## <a name="dismiss-dialogs"></a>Zavřít dialogová okna

Při použití [ `ProgressDialog` ](https://developer.xamarin.com/api/type/Android.App.ProgressDialog/) – třída (nebo všechny dialogové okno nebo výstrahu), namísto volání [ `Hide` ](https://developer.xamarin.com/api/member/Android.App.Dialog.Hide()/) volání metody po dokončení, dialogovém okně účel [ `Dismiss` ](https://developer.xamarin.com/api/member/Android.App.Dialog.Dismiss()/) metoda. Dialogové okno, jinak bude stále aktivní a bude úniku aktivity tím, že se na něj odkaz.

## <a name="summary"></a>Souhrn

Tento článek popisuje a popsané techniky pro zvýšení výkonu aplikace sestavené s Xamarin.Android. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace.


## <a name="related-links"></a>Související odkazy

- [Napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md)
