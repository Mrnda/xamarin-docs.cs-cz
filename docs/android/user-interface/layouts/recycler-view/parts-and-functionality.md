---
title: RecyclerView části a funkce
description: Přehled Správce rozložení RecyclerView, adaptéru a vlastník zobrazení.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 4d55124e9a02489d1f55e900c537939ff3450509
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038505"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView části a funkce


`RecyclerView` obslužné rutiny některé z těchto úloh interně (například posouvání a recyklaci zobrazení), ale to je v podstatě správce, který koordinuje pomocné třídy pro zobrazení kolekce. `RecyclerView` Deleguje na následujících tříd pomocných rutin úloh:

-   **`Adapter`** &ndash; Zvýšení kapacity rozložení položek (vytvoří obsah souboru rozložení) a sváže data pro zobrazení, která se zobrazují v rámci `RecyclerView`. Adaptér také hlásí události kliknutí na položku.

-   **`LayoutManager`** &ndash; Měří a umístí zobrazení položek v rámci `RecyclerView` a spravuje zásady pro zobrazení recyklace.

-   **`ViewHolder`** &ndash; Vyhledává a ukládá zobrazit odkazy. Držitel zobrazení vám také pomůže s detekci kliknutí zobrazení položky.

-   **`ItemDecoration`** &ndash; Umožňuje aplikacím přidat zvláštní posunutí pro vykreslování a rozložení pro konkrétní zobrazení pro kreslení oddělovače mezi položkami, vybraná vystoupení a vizuální seskupení hranice.

-   **`ItemAnimator`** &ndash; Definuje animace, které provedou během akce položek nebo jako změny se provedly adaptér.

Vztah mezi `RecyclerView`, `LayoutManager`, a `Adapter` třídy je znázorněn v následujícím diagramu:

![Diagram RecyclerView obsahující LayoutManager, pomocí adaptéru pro přístup k datové sadě](parts-and-functionality-images/01-recyclerview-diagram.png)

Jak ukazuje následující obrázek `LayoutManager` si lze představit jako zprostředkovatel mezi `Adapter` a `RecyclerView`. `LayoutManager` Provede volání do `Adapter` metody jménem `RecyclerView`. Například `LayoutManager` volání `Adapter` metodu, když je čas vytvořit nové zobrazení pro určité položky pozici v `RecyclerView`. `Adapter` Zvýší rozložení pro danou položku a vytváří `ViewHolder` instance (není vidět) do mezipaměti odkazy na zobrazení na této pozici. Při `LayoutManager` volání `Adapter` svázat konkrétní položku na sadu dat `Adapter` vyhledá data pro tuto položku, načte z datové sady a zkopíruje se do zobrazení související položku.

Při použití `RecyclerView` v aplikaci, vytváření odvozené typy následující třídy se vyžaduje:

-   **`RecyclerView.Adapter`** &ndash; Poskytuje vazbu z vaší aplikace datovou sadu (což je specifické pro vaši aplikaci) na položku zobrazení, která se zobrazují v rámci `RecyclerView`. Adaptér ví, jak přidružit každou pozici zobrazení položek v `RecyclerView` do určitého umístění v datovém zdroji. Kromě toho adaptéru zpracovává rozložení obsahu v rámci každé jednotlivé položky zobrazení a vytvoří vlastník zobrazit pro každé zobrazení. Adaptér také hlásí události kliknutí na položku, zjištěné položky zobrazení.

-   **`RecyclerView.ViewHolder`** &ndash; Ukládá do mezipaměti odkazy na zobrazení v souboru položky rozložení tak, aby se zbytečně opakuje vyhledávání prostředků. Držitel zobrazení také řídí události kliknutí na položku mají být předány adaptér, když uživatel klepne držitele zobrazit související položky zobrazení.

-   **`RecyclerView.LayoutManager`** &ndash; Umístění položky v rámci `RecyclerView`. Můžete použít jednu z několika předdefinovaných rozložení správce nebo správce vlastní rozložení můžete implementovat.
    `RecyclerView` Při změnách zásad rozložení pro Správce rozložení, tak můžete zařadit správce jiné rozložení bez nutnosti provádět významné delegáti do vaší aplikace.

Navíc můžete případně rozšířit následující třídy, chcete-li změnit vzhled a chování `RecyclerView` ve vaší aplikaci:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Pokud nerozšíříte `ItemDecoration` a `ItemAnimator`, `RecyclerView` používá výchozí implementace. Tato příručka nevysvětluje, jak vytvořit vlastní `ItemDecoration` a `ItemAnimator` třídy; Další informace o těchto tříd naleznete v tématu [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) a [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Jak zobrazit recyklace funguje

`RecyclerView` pro každou položku ve zdroji dat nepřidělí zobrazení položky. Místo toho přidělí pouze počet zobrazení položek na obrazovku vejde a ho znovu použije tyto položky rozložení jako uživatel posune. Posune zobrazení nejprve z pohledu, prochází recyklace procesu znázorněné na následujícím obrázku:

[![Diagram znázorňující šest kroků recyklace zobrazení](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  Při zobrazení posune z pohledu a už nebude zobrazovat, se stane *vyřadit zobrazení*.

2.  Zobrazení související s vadnými výrobky, nachází ve fondu a změní *recyklovat zobrazení*.
    Tento fond je zobrazení, která zobrazují stejný typ dat v mezipaměti.

3.  Při vytvoření nové položky se zobrazení, zobrazení je převzata z recyklace fondu pro další použití. Protože toto zobrazení musí být znovu vázán na adaptér před zobrazením, je volána *nesprávné zobrazení*.

4.  Změny zobrazení recykluje: adaptér vyhledá data pro další položky, který se má zobrazit a zkopíruje data do zobrazení pro tuto položku. Odkazy pro tato zobrazení se načítají ze zobrazení držitele přidružený k zobrazení recyklován.

5.  Recyklovat zobrazení se přidá do seznamu položek v `RecyclerView` , který se chystáte se přejít na obrazovce.

6.  Zobrazení recyklován přejde na obrazovce jako uživatel posune `RecyclerView` na další položku v seznamu. Mezitím jiného zobrazení posune z pohledu a recykluje podle výše uvedených kroků.

Kromě zobrazení položky znovu `RecyclerView` také používá jiný optimalizace efektivity: Zobrazit zástupné znaky. A *zobrazení držitel* je, že mezipaměť zobrazit odkazy na jednoduchou třídu. Pokaždé, když soubor rozložení položky Tento adaptér vytvoří také odpovídající držitel zobrazení. Držitel zobrazení používá `FindViewById` zobrazíte odkazy na zobrazení v souboru zvýšeným rozložení položky. Tyto odkazy se používají k načtení nových dat do zobrazení pokaždé, když rozložení recykluje zobrazit nová data.
 


### <a name="the-layout-manager"></a>Správce rozložení

Manager rozložení je odpovědná za umístění položky v `RecyclerView` zobrazit; Určuje typ prezentace (seznam nebo Mřížka), orientace (Určuje, zda položky jsou zobrazeny vodorovně nebo svisle) a směr položky, které má být zobrazena. (normální, nebo v opačném pořadí). Rozložení správce zodpovídá také pro výpočet velikosti a umístění každé položky v **RecycleView** zobrazení.

Správce má další účel: Určuje zásady pro při recyklaci položku zobrazení, které již nejsou viditelné pro uživatele.
Vzhledem k tomu, že Správce rozložení je vědět, která zobrazení jsou viditelné (a které nejsou), je nejlepší pozici rozhodnout, kdy může být zobrazení recyklován. Recyklovat zobrazení, správce obvykle provede volání adaptéru pro nahrazení obsahu recyklován zobrazení s různými daty, jak je popsáno výše v [jak zobrazení recyklace funguje](#recycling).

Můžete rozšířit `RecyclerView.LayoutManager` vytvořit si vlastní rozložení můžete použít správce, nebo správce předdefinované rozložení. `RecyclerView` poskytuje následující předdefinované rozložení manažeři:

-   **`LinearLayoutManager`** &ndash; Uspořádá položky ve sloupci, který můžete posunout svisle, nebo v řádku, který může být seznam vodorovně posunout.

-   **`GridLayoutManager`** &ndash; Zobrazí položky v mřížce.

-   **`StaggeredGridLayoutManager`** &ndash; Zobrazí položky v postupný mřížky, kde některé položky mají různé výšky a šířky.

Zadat rozložení správce, vytvořit instanci správce zvolený rozložení a předejte jej `SetLayoutManager` metody. Všimněte si, které jste *musí* zadejte Správce rozložení &ndash; `RecyclerView` nevybere předdefinované rozložení manager ve výchozím nastavení.

Další informace o správci rozložení, najdete v článku [referenční třída RecyclerView.LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).


### <a name="the-view-holder"></a>Držitel zobrazení

Držitel zobrazení je třída, která definujete pro ukládání do mezipaměti odkazy na zobrazení. Adaptér používá tyto zobrazit odkazy pro vazbu každé zobrazení jeho obsahu. Každá položka v `RecyclerView` má přidružené zobrazení vlastníka instance, která ukládá do mezipaměti odkazy na zobrazení pro danou položku. Držitel zobrazení vytvoříte pomocí následujících kroků k definování třídy k umístění přesnou sadu zobrazení jednu položku:

1.  Podtřídy `RecyclerView.ViewHolder`.
2.  Implementuje konstruktor, který vyhledává a ukládá zobrazit odkazy.
3.  Implementace vlastnosti, které je adaptér můžete použít pro přístup k tyto odkazy.

Podrobný příklad `ViewHolder` implementace je uveden v [A základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Další informace o `RecyclerView.ViewHolder`, najdete v článku [referenční třída RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="the-adapter"></a>Adaptér

Většina "těžkých – zrušení" `RecyclerView` integrace kódu probíhá v adaptéru. `RecyclerView` zadání adaptéru odvozený od `RecyclerView.Adapter` přístupu ke zdroji dat a naplňte jimi každou položku se obsah ze zdroje dat.
Vzhledem k tomu, že zdroj dat je specifický pro aplikace, musí implementovat adaptér funkce, která analyzuje jak přistupovat ke svým datům. Adaptér extrahuje informace ze zdroje dat a nahraje je do každé položky v `RecyclerView` kolekce.

Následující ilustrace jak adaptér mapuje na jednotlivá zobrazení v rámci položky každý řádek v obsahu ve zdroji dat prostřednictvím zobrazení zástupné znaky `RecyclerView`:

[![Diagram ilustrující adaptér pro připojení zdroje dat ViewHolders](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

Adaptér načte každý `RecyclerView` řádků s daty pro položku konkrétního řádku. Pozice řádku *P*, například adaptér vyhledá přidružená data na pozici *P* v rámci zdroje dat a kopie položky na řádku dat na pozici *P* v `RecyclerView` kolekce.
Ve výše uvedené výkresu, například adaptér používá zobrazení držitel vyhledat odkazy na `ImageView` a `TextView` na této pozici, aby neobsahovalo opakovaně volat `FindViewById` těchto zobrazení jako uživatel procházení kolekce a opětovně používá zobrazení.

Při implementaci adaptér, je nutné přepsat následující `RecyclerView.Adapter` metody:

-   **`OnCreateViewHolder`** &ndash; Vytvoří instanci položky rozložení souboru a zobrazení vlastník.

-   **`OnBindViewHolder`** &ndash; Načte data na konkrétní pozici do zobrazení, jejichž odkazy jsou uloženy v vlastník daného zobrazení.

-   **`ItemCount`** &ndash; Vrátí počet položek ve zdroji dat.

Správce rozložení volání těchto metod, přičemž to je umístění položky v rámci `RecyclerView`. 



### <a name="notifying-recyclerview-of-data-changes"></a>Upozornění RecyclerView změny dat

`RecyclerView` se neaktualizuje automaticky zobrazení při změně; obsah svých dat zdroje adaptér oznámí `RecyclerView` když dojde ke změně v datové sadě. Datové sady můžete změnit mnoha způsoby; Například můžete změnit obsah v rámci položky nebo celkovou strukturu dat mohou být změněny.
`RecyclerView.Adapter` poskytuje několik metod, které můžete volat tak, aby `RecyclerView` reaguje na změny dat nejefektivnějším způsobem:

-  **`NotifyItemChanged`** &ndash; Signály, které položky na zadané pozici změnila.

-  **`NotifyItemRangeChanged`** &ndash; Signály, které se změnily položky v zadaném rozsahu pozic.

-  **`NotifyItemInserted`** &ndash; Signály nově vložené položky na zadané pozici.

-  **`NotifyItemRangeInserted`** &ndash; Signalizuje, že položky v zadaném rozsahu pozice se nově vložené.

-  **`NotifyItemRemoved`** &ndash; Signalizuje, že byla odebrána položka v zadané pozici.

-  **`NotifyItemRangeRemoved`** &ndash; Signalizuje, že byly odebrány položky v zadaném rozsahu pozic.

-  **`NotifyDataSetChanged`** &ndash; Signály, které datová sada změnila (vynutí úplnou aktualizaci).

Pokud víte, přesně jak byl změněn vaši datovou sadu, můžete volat odpovídající metod uvedených výše na Aktualizovat `RecyclerView` nejefektivnějším způsobem. Pokud si nejste jisti, přesně jak byl změněn vaši datovou sadu, můžete volat `NotifyDataSetChanged`, což je mnohem méně efektivní protože `RecyclerView` musíte aktualizovat všechna zobrazení, které jsou viditelné pro uživatele. Další informace o těchto metodách v tématu [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

V dalším tématu [A základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), ukázkovou aplikaci je implementováno s cílem prokázat skutečné příklady z části a funkce uvedených výše.


## <a name="related-links"></a>Související odkazy

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Rozšíření příklad RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
