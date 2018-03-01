---
title: "RecyclerView částí a funkce"
ms.topic: article
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b1ddcca25fd83a806e8383a5717462b518b46d0b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView částí a funkce


`RecyclerView` obslužné rutiny některé z těchto úloh interně (například posouvání a recyklace zobrazení), ale je v podstatě správce, který koordinuje pomocné třídy pro zobrazení kolekce. `RecyclerView` na následující pomocné rutiny třídy delegáti úloh:

-   **`Adapter`** &ndash; Zvýšení kapacity rozložení položek (vytvoří obsah souboru rozložení) a sváže data zobrazení, které se zobrazují v rámci `RecyclerView`. Adaptér také sestavy události kliknutí na položku.

-   **`LayoutManager`** &ndash; Měří a umisťuje položku zobrazení v rámci `RecyclerView` a spravuje zásady pro zobrazení recyklace.

-   **`ViewHolder`** &ndash; Vyhledává a ukládá odkazy na zobrazení. Držitel zobrazení také pomáhá s zjišťování klikne na tlačítko zobrazení položek.

-   **`ItemDecoration`** &ndash; Umožňuje aplikaci přidat speciální kreslení a rozložení posuny do konkrétní zobrazení pro kreslení oddělovače mezi položky, program a hranice visual seskupení.

-   **`ItemAnimator`** &ndash; Definuje animace, které probíhat během akce položky nebo jako změny, které jsou vytvářeny adaptéru.

Vztah mezi `RecyclerView`, `LayoutManager`, a `Adapter` třídy je znázorněn v následujícím diagramu:

![Diagram RecyclerView obsahující LayoutManager, pomocí adaptéru pro přístup k datové sadě](parts-and-functionality-images/01-recyclerview-diagram.png)

Jak ukazuje následující obrázek, `LayoutManager` si lze představit jako zprostředkovatel mezi `Adapter` a `RecyclerView`. `LayoutManager` Provádí volání do `Adapter` metody jménem `RecyclerView`. Například `LayoutManager` volání `Adapter` metoda, když je čas vytvořit nové zobrazení pro konkrétní položky pozice v `RecyclerView`. `Adapter` Zvýšení kapacity rozložení pro danou položku a vytvoří `ViewHolder` instance (není vidět) do mezipaměti odkazy na zobrazení na této pozici. Když `LayoutManager` volání `Adapter` k vytvoření vazby určité položky. datová sada `Adapter` vyhledá data pro tuto položku, načte z datové sady a zkopíruje je do zobrazení související položku.

Při použití `RecyclerView` v aplikaci, vytváření odvozené typy následující třídy se vyžaduje:

-   **`RecyclerView.Adapter`** &ndash; Poskytuje vazbu z vaší aplikace datové sady (což je specifické pro vaši aplikaci) k zobrazení položek, které se zobrazují v rámci `RecyclerView`. Adaptér umí přidružit každý pozice zobrazení položek v `RecyclerView` do určitého umístění v datovém zdroji. Kromě toho adaptéru zpracovává rozložení obsahu v rámci každé jednotlivé položky zobrazení a vytvoří držitele zobrazení pro každý zobrazení. Adaptér také sestavy události kliknutí na položky, které byly zjištěny nástrojem zobrazení položek.

-   **`RecyclerView.ViewHolder`** &ndash; Ukládá do mezipaměti odkazy na zobrazení v souboru rozložení položky tak, aby vyhledávání prostředků nejsou opakuje zbytečně. Držitel zobrazení uspořádá taky pro události kliknutí na položku k přeposlání do adaptéru, když uživatel klepnutím zobrazení držitele zobrazení související položku.

-   **`RecyclerView.LayoutManager`** &ndash; Umisťuje položky v rámci `RecyclerView`. Můžete použít jednu, několik předdefinovaných rozložení správce nebo můžete implementovat vlastní správce vlastní rozložení.
    `RecyclerView` Delegáti rozložení zásady tak, aby Správce rozložení, tak můžete zařadit manažera různých rozložení bez nutnosti provádět významné změny do vaší aplikace.

Navíc můžete případně rozšířit následující třídy, chcete-li změnit vzhled a chování `RecyclerView` ve vaší aplikaci:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Pokud nerozšíříte `ItemDecoration` a `ItemAnimator`, `RecyclerView` používá výchozí implementace. Tato příručka nevysvětluje, jak vytvořit vlastní `ItemDecoration` a `ItemAnimator` třídy; Další informace o těchto tříd naleznete v tématu [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) a [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Jak zobrazit recyklace funguje

`RecyclerView` pro každou položku ve zdroji dat nepřidělí zobrazení položek. Místo toho se přiděluje jenom počet položek zobrazení, která vejít na obrazovce a ho znovu použije tyto položky rozložení jako viditelné pro uživatele. Při zobrazení nejdřív posunu mimo nebyl zřejmý, prochází recyklace procesu zobrazené na následujícím obrázku:

[ ![Diagram ilustrující šest kroků recyklace zobrazení](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png)

1.  Když zobrazení posune mimo nebyl zřejmý a se už nezobrazuje, stane se *vyřadit zobrazení*.

2.  Zobrazení Zmetky je umístěn ve fondu a že bude *recyklovat zobrazení*.
    Tento fond je mezipaměť a zobrazit stejný typ dat zobrazení.

3.  Po který se má zobrazit novou položku zobrazení je převzat ze recyklaci fondu pro další použití. Protože toto zobrazení musí být znovu vázána adaptér než se zobrazí, se nazývá *nesprávné zobrazení*.

4.  Změny zobrazení je recykluje: adaptér vyhledá data pro další položky, který se má zobrazit a zkopíruje data na zobrazení pro tuto položku. Odkazy na tato zobrazení se načítají z držitele zobrazení související s recykluje zobrazení.

5.  Recykluje zobrazení je přidán do seznamu položek v `RecyclerView` který se chystáte přejděte na obrazovce.

6.  Recykluje zobrazení přejde na obrazovce jako viditelné pro uživatele `RecyclerView` na další položku v seznamu. Současně jiného zobrazení posune mimo nebyl zřejmý a dojde k recyklování podle kroků uvedených výše.

Kromě zobrazení položek opakované použití `RecyclerView` také používá jiný optimalizaci efektivity: Zobrazit držitele. A *zobrazení držitel* je jednoduchý třída, mezipamětí zobrazit odkazy. Pokaždé, když je adaptér zvýšení kapacity soubor rozložení položky, také vytvoří odpovídající držitel zobrazení. Držitel zobrazení používá `FindViewById` získat odkazy na zobrazení v souboru zvýšeným rozložení položky. Tyto odkazy se používají k načtení nová data do zobrazení pokaždé, když dojde k recyklování zobrazíte nová data rozložení.
 

<a name="layoutmanager" />

### <a name="the-layout-manager"></a>Správce rozložení

Správce rozložení je zodpovědná za umístění položky v `RecyclerView` zobrazit; Určuje typ prezentace (seznam nebo Mřížka), orientaci (jestli položky se zobrazují vodorovně nebo svisle) a směr položek, které mají být zobrazeny (normální, nebo v opačném pořadí). Rozložení manager zodpovídá taky za výpočet velikost a umístění pro každou položku v **RecycleView** zobrazení.

Rozložení správce má další účely: Určuje zásady při recyklace zobrazení položky, které již nejsou viditelné pro uživatele.
Vzhledem k tomu, že Správce rozložení vědět, která zobrazení jsou viditelné (a které nejsou), je nejlepší možnost rozhodnout, kdy může být recyklována zobrazení. Recyklovat zobrazení, rozložení správce obvykle provádí volání k adaptéru na nahraďte obsah recykluje zobrazení s odlišnými daty, jak je popsáno dříve v [jak zobrazení recyklace funguje](#recycling).

Můžete rozšířit `RecyclerView.LayoutManager` k vytvoření vlastního rozložení správce, nebo můžete použít předdefinované rozložení správce. `RecyclerView` poskytuje následující předdefinované rozložení správci:

-   **`LinearLayoutManager`** &ndash; Uspořádá položky ve sloupci, který svisle posunout nebo v řádku, který vodorovně posunout.

-   **`GridLayoutManager`** &ndash; Zobrazí položky v mřížce.

-   **`StaggeredGridLayoutManager`** &ndash; Zobrazí položky v mřížce postupný, kde některé položky mají různé výšky a šířky.

Pokud chcete zadat Správce rozložení, vytvoření instance vašeho správce zvolené rozložení a předejte jej `SetLayoutManager` metoda. Všimněte si, které *musí* zadejte Správce rozložení &ndash; `RecyclerView` nevybere předdefinované rozložení manager ve výchozím nastavení.

Další informace o správci rozložení najdete v tématu [referenci třídy RecyclerView.LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).

<a name="viewholder" />

### <a name="the-view-holder"></a>Držitel zobrazení

Držitel zobrazení je třída, která definujete pro ukládání do mezipaměti odkazy na zobrazení. Karta používá tyto odkazy zobrazení pro vazbu jednotlivých zobrazení na jeho obsah. Každá položka v `RecyclerView` má instanci držitel přidružené zobrazení, která ukládá do mezipaměti odkazy na zobrazení pro danou položku. K vytvoření zobrazení držitel, použijte následující postup můžete definovat třídu pro uložení přesnou sadu zobrazení na položku:

1.  Podtřída `RecyclerView.ViewHolder`.
2.  Implementujte konstruktor, který vyhledává a ukládá odkazy na zobrazení.
3.  Implementovat vlastnosti, které je adaptér použít pro přístup tyto odkazy.

Podrobný příklad `ViewHolder` implementace jsou poskytovány [A základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Další informace o `RecyclerView.ViewHolder`, najdete v článku [referenci třídy RecyclerView.ViewHolder](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).

<a name="adapter" />

### <a name="the-adapter"></a>Adaptér

Většina o "těžký odstranění" `RecyclerView` integrace kódu probíhá v adaptéru. `RecyclerView` vyžaduje zadání adaptér odvozen od `RecyclerView.Adapter` k přístupu ke zdroji dat a naplnit každou položku se obsah ze zdroje dat.
Protože zdroj dat je specifický pro aplikace, je nutné implementovat adaptér funkce, která funguje s technologií přístupu k datům. Adaptér extrahuje informace ze zdroje dat a načte ji do jednotlivé položky `RecyclerView` kolekce.

Následující ilustrace jak adaptér mapuje obsah ve zdroji dat prostřednictvím držitelé zobrazení jednotlivých zobrazení v rámci každého řádku položky v `RecyclerView`:

[ ![Diagram ilustrující adaptér připojení zdroje dat k ViewHolders](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png)

Adaptér načte každý `RecyclerView` řádek s daty pro položku konkrétního řádku. Pro pozice řádku *P*, například adaptér vyhledá přidružená data na pozici *P* v rámci zdroj dat a zkopíruje položky tato data na řádek na pozici *P* v `RecyclerView` kolekce.
Ve výše uvedené výkresu, například adaptér používá držitele zobrazení pro odkazy pro vyhledání `ImageView` a `TextView` na této pozici, takže nemusí opakovaně volat `FindViewById` pro tyto zobrazení jako uživatel posune prostřednictvím kolekce a opětovně používá zobrazení.

Pokud implementujete adaptér, je nutné přepsat následující `RecyclerView.Adapter` metody:

-   **`OnCreateViewHolder`** &ndash; Vytvoří instanci držitele soubor nebo zobrazit položky rozložení.

-   **`OnBindViewHolder`** &ndash; Načte data na zadané pozici do zobrazení, jehož odkazy jsou uložené v držitele daného zobrazení.

-   **`ItemCount`** &ndash; Vrátí počet položek v datovém zdroji.

Správce rozložení volání těchto metod, přičemž se je umístění položky v rámci `RecyclerView`. 


<a name="datachanges" />

### <a name="notifying-recyclerview-of-data-changes"></a>Upozornění RecyclerView změny dat

`RecyclerView` automaticky neaktualizuje jeho zobrazení při změně; obsah jeho datového zdroje adaptér musíte upozornit `RecyclerView` když dojde ke změně v datové sadě. Sada dat, můžete změnit v mnoha směrech; Například můžete změnit obsah v rámci položky nebo celková struktura dat mohou být změněny.
`RecyclerView.Adapter` poskytuje několik metod, které můžete volat tak, aby `RecyclerView` reaguje na změny dat nejefektivnějším způsobem:

-  **`NotifyItemChanged`** &ndash; Signály, které došlo ke změně položky na zadané pozici.

-  **`NotifyItemRangeChanged`** &ndash; Signály, které se změnily položky v zadaném rozsahu pozic.

-  **`NotifyItemInserted`** &ndash; Signály nově vložené položky v konkrétní pozici.

-  **`NotifyItemRangeInserted`** &ndash; Signalizuje, že položky v zadaném rozsahu pozic byla nově vložena.

-  **`NotifyItemRemoved`** &ndash; Signalizuje, že byla odebrána položka v konkrétní pozici.

-  **`NotifyItemRangeRemoved`** &ndash; Signalizuje, že byly odebrány položky v zadaném rozsahu pozic.

-  **`NotifyDataSetChanged`** &ndash; Signály, které v datové sadě došlo ke změně (vynutí úplnou aktualizaci).

Pokud znáte, přesně jak sadu dat došlo ke změně, můžete volat výše a obnovit příslušná metody `RecyclerView` nejefektivnějším způsobem. Pokud si nejste jisti, přesně jak sadu dat došlo ke změně, můžete zavolat `NotifyDataSetChanged`, což je mnohem méně efektivní protože `RecyclerView` musíte aktualizovat všechna zobrazení, které jsou viditelné pro uživatele. Další informace o těchto metodách v tématu [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

V dalším tématu [A základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), například aplikace je implementována k předvedení příklady skutečné kódu částí a funkcí uvedených výše.


## <a name="related-links"></a>Související odkazy

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Základní příklad RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [Příklad RecyclerView rozšíření](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
