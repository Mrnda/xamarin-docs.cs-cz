---
title: Zobrazení karty aplikace
description: Pomůcka zobrazení karty aplikace je součást uživatelského rozhraní, která uvede textových a obrázkových obsah v zobrazení, které se podobají karty. Tato příručka vysvětluje, jak používat a přizpůsobit zobrazení karty aplikace v aplikacích Xamarin.Android při zachování zpětné kompatibility s předchozími verzemi systému Android.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 21e2a2e8ef04936664344cb4fb758bc2af3b4d05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771831"
---
# <a name="cardview"></a>Zobrazení karty aplikace

_Pomůcka zobrazení karty aplikace je součást uživatelského rozhraní, která uvede textových a obrázkových obsah v zobrazení, které se podobají karty. Tato příručka vysvětluje, jak používat a přizpůsobit zobrazení karty aplikace v aplikacích Xamarin.Android při zachování zpětné kompatibility s předchozími verzemi systému Android._


## <a name="overview"></a>Přehled

`Cardview` Pomůcky, zavedená v systému Android 5.0 typu (Lupa), je součástí uživatelského rozhraní, která uvede textových a obrázkových obsah v zobrazení, které se podobají karty. `CardView` je implementovaný jako `FrameLayout` pomůcky s zaoblenými hranami a stínu. Obvykle `CardView` používá se k předložení jednoho řádku položky v `ListView` nebo `GridView` zobrazení skupiny. Například na následujícím snímku obrazovky je příklad aplikaci rezervace cesta, která implementuje `CardView`– na základě cesta cílové karty ve posouvatelného `ListView`:

![Příklad aplikace pomocí zobrazení karty aplikace pro každý cíl cesty](card-view-images/01-cardview-example.png)

Tato příručka vysvětluje, jak přidat `CardView` balíčku do vaší Xamarin.Android projektu, jak přidat `CardView` rozložení a postup přizpůsobení vzhledu `CardView` ve vaší aplikaci. Kromě toho tato příručka obsahuje podrobný seznam `CardView` atributy, které je možné změnit, včetně atributů, které vám pomohou při použití `CardView` ve verzích systému Android starší než typu Lupa Android 5.0.

<a name="requirements" />

## <a name="requirements"></a>Požadavky

K použití nového systému Android 5.0 a novější funkce se vyžaduje následující (včetně `CardView`) v aplikacích pro Xamarin:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 nebo novější musí být nainstalovaná a nakonfigurovaná s Visual Studio nebo Visual Studio for Mac.

-  **Sady SDK pro Android** &ndash; Android 5.0 (21 rozhraní API) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.

-  **Java JDK 1.8** &ndash; JDK 1.7 lze použít, pokud je konkrétně zaměřen API úroveň 23 a starší. JDK 1.8 je k dispozici z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Aplikace musí obsahovat také `Xamarin.Android.Support.v7.CardView` balíčku. Chcete-li přidat `Xamarin.Android.Support.v7.CardView` balíčku v sadě Visual Studio pro Mac:

1. Otevřete projekt, klikněte pravým tlačítkem na **balíčky** a vyberte **přidat balíčky**.

2. V **přidat balíčky** dialogové okno, vyhledejte **zobrazení karty aplikace**.

3. Vyberte **zobrazení karty v7 aplikace Xamarin knihovna podpory**, pak klikněte na tlačítko **přidat balíček**.

Chcete-li přidat `Xamarin.Android.Support.v7.CardView` balíčku v sadě Visual Studio:

1. Otevřete projekt, klikněte pravým tlačítkem myši **odkazy** uzlu (v **Průzkumníku řešení** podokně) a vyberte **spravovat balíčky NuGet...** .

2. Když **spravovat balíčky NuGet** se zobrazí dialogové okno, zadejte **zobrazení karty aplikace** do vyhledávacího pole.

3. Když **zobrazení karty v7 aplikace Xamarin knihovna podpory** se zobrazí, klikněte na tlačítko **nainstalovat**.

Naučte se konfigurovat projekt aplikace Android 5.0, najdete v tématu [nastavení nahoru Android 5.0 projektu](~/android/platform/lollipop.md).
Další informace o instalaci balíčků NuGet najdete v tématu [návod: včetně NuGet ve vašem projektu](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


## <a name="introducing-cardview"></a>Představení zobrazení karty aplikace

Výchozí hodnota `CardView` podobá bílé karta s minimálně zaoblenými hranami a mírné stínové. Následující příklad **Main.axml** rozložení zobrazí jeden `CardView` pomůcku, která obsahuje `TextView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Pokud použijete tento XML nahradit existující obsah **Main.axml**, nezapomeňte komentář žádný kód v **MainActivity.cs** který odkazuje na prostředky v předchozí kód XML.

Tento příklad rozložení vytvoří výchozí `CardView` s jedním řádkem textu, jak je znázorněno na následujícím snímku obrazovky:

[![Snímek obrazovky ze zobrazení karty aplikace s bílým pozadím a řádku textu](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

V tomto příkladu je nastaven styl aplikace světlý motiv materiálu (`Theme.Material.Light`) tak, aby `CardView` stíny a okrajů je snazší zjistit. Další informace o aplikacích motivů Android 5.0 najdete v tématu [materiálu motiv](~/android/user-interface/material-theme.md). V další části, jsme získáte informace, jak přizpůsobit `CardView` pro aplikaci.


## <a name="customizing-cardview"></a>Přizpůsobení zobrazení karty aplikace

Můžete upravit základní `CardView` atributy, které mají přizpůsobení vzhledu `CardView` ve vaší aplikaci. Například zvýšení `CardView` může být zvýšena přetypovat větší stín (který karty se zdá, že float vyšší výše na pozadí). Navíc můžete poloměr zvýší tak, aby rozích další karty zaokrouhlené.

V dalším příkladu rozložení, který je přizpůsobený `CardView` slouží k vytváření simulace tiskové fotografie ("snímek"). `ImageView` Je přidán do `CardView` pro zobrazení bitovou kopii a `TextView` je umístěno pod `ImageView` pro zobrazení název bitové kopie.
V tomto příkladu rozložení `CardView` má následující přizpůsobení:

-  `cardElevation` Je zvýšena na 4dp přetypovat větší stínové.
-  `cardCornerRadius` Je zvýšena na 5dp Chcete-li zobrazit více zaokrouhlené rozích.

Protože `CardView` je zadané ve knihovnu podpory Android v7, nejsou k dispozici z jeho atributy `android:` oboru názvů. Proto je nutné definovat vlastní obor názvů XML a použít tento obor názvů, jako `CardView` předponu atributu. V následujícím příkladu rozložení použijeme tohoto řádku zadat obor názvů s názvem `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Tento obor názvů můžete volat `card_view` nebo i `myapp` Pokud se rozhodnete (je přístupné pouze v rámci oboru tohoto souboru). Cokoli si zvolíte volat tento obor názvů, je nutné použít jako předpona `CardView` atribut, který chcete upravit. V tomto příkladu rozložení `cardview` je předponu pro obor názvů `cardElevation` a `cardCornerRadius`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Když v tomto příkladu rozložení se používá k zobrazení obrázku v aplikaci zobrazení fotografií, `CardView` má vzhled fotografie snímek, jak je znázorněno na následujícím snímku obrazovky:

[![Zobrazení karty aplikace s bitové kopie a titulku pod obrázkem](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

Tento snímek obrazovky jsou převzaty z [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) ukázkovou aplikaci, která používá `RecyclerView` pomůcky nabídne posouvání seznam `CardView` bitových kopií pro zobrazení fotografií. Další informace o `RecyclerView`, najdete v článku [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) průvodce.

Všimněte si, že `CardView` více než jeden podřízené zobrazení můžete zobrazit v oblasti jeho obsahu. Ve výše uvedené fotografii zobrazení příklad aplikace, například oblast obsahu se skládá z `ListView` obsahující `ImageView` a `TextView`. I když `CardView` instance jsou často svisle uspořádané, můžete také uspořádat je vodorovně (najdete v části [vytváření vlastní styl zobrazení](~/android/user-interface/material-theme.md#customview) pro – snímek obrazovky příkladu).


### <a name="cardview-layout-options"></a>Možnosti zobrazení karty aplikace rozložení

`CardView` rozložení lze přizpůsobit pomocí nastavení jeden nebo více atributů, které ovlivňují jeho odsazení, zvýšení oprávnění, rohu radius a barvu pozadí:

[![Diagram zobrazení karty aplikace atributů](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Každý atribut lze také změnit dynamicky voláním protějšek `CardView` – metoda (Další informace o `CardView` metody, najdete v článku [referenci třídy zobrazení karty aplikace](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Všimněte si, že tyto atributy (s výjimkou barvu pozadí) přijměte hodnotu dimenze, která je desetinné číslo následuje jednotka. Například `11.5dp` určuje 11.5 nezávislé na hustotě pixelů.


#### <a name="padding"></a>Odsazení
`
CardView` nabízí pěti atributů odsazení pro umístění obsahu v rámci kartu. Můžete je nastavit v rozvržení XML nebo volat metody podobá ve vašem kódu:

[![Diagram zobrazení karty aplikace odsazení atributy](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Odsazení atributy jsou vysvětleny takto:

-  `contentPadding` &ndash; Vnitřní odsazení mezi podřízené zobrazení `CardView` a všech hran karty.

-  `contentPaddingBottom` &ndash; Vnitřní odsazení mezi podřízené zobrazení `CardView` a dolního okraje karty.

-  `contentPaddingLeft` &ndash; Vnitřní odsazení mezi podřízené zobrazení `CardView` a levé hrany kartu.

-  `contentPaddingRight` &ndash; Vnitřní odsazení mezi podřízené zobrazení `CardView` a pravý okraj karty.

-  `contentPaddingTop` &ndash; Vnitřní odsazení mezi podřízené zobrazení `CardView` a jsou horní okraje kartu.

Atributy obsahu odsazení jsou relativní vzhledem k hranici oblasti obsahu, místo na jakékoli dané pomůcky umístěný v oblasti obsahu.
Například pokud `contentPadding` byly dostatečně vyšší v aplikaci zobrazení fotografií `CardView` by oříznout bitové kopie a text zobrazený na kartě.



#### <a name="elevation"></a>Zvýšení oprávnění

`CardView` nabízí dva atributy zvýšení oprávnění k řízení zvýšení jeho oprávnění a v důsledku toho velikost jeho stín:

[![Diagram zobrazení karty aplikace zvýšení atributů](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Atributy zvýšení oprávnění jsou vysvětleny takto:

-  `cardElevation` &ndash; Zvýšení úrovně `CardView` (představuje ose Z).

-  `cardMaxElevation` &ndash; Maximální hodnota, která `CardView`pro zvýšení oprávnění.

Vyšší hodnoty z `cardElevation` zvýšit velikost stínu aby `CardView` se zdá, že float vyšší výše na pozadí. `cardElevation` Atribut také určuje pořadí vykreslování překrývajících se oblastí zobrazení; který je, `CardView` budou vykreslovat pod jinou překrývající se zobrazením s vyšší nastavení zvýšení oprávnění a vyšší žádné překrývající se zobrazení s nižší nastavení zvýšení oprávnění.
`cardMaxElevation` Nastavení je užitečné pro když aplikace pro zvýšení oprávnění se dynamicky mění &ndash; stín zabrání rozšíření za limit definující s tímto nastavením.


#### <a name="corner-radius-and-background-color"></a>Rohu Radius a barvu pozadí

`CardView` nabízí atributy, které můžete použít k řízení jeho rohu radius a jeho barvu pozadí. Tyto dvě vlastnosti povolit, změňte celkové styl `CardView`:

[![Diagram zobrazení karty aplikace rohu radious a atributy Barva pozadí](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Tyto atributy jsou vysvětleny takto:

-  `cardCornerRadius` &ndash; U všech rozích poloměr `CardView`.

-  `cardBackgroundColor` &ndash; Barva pozadí `CardView`.

V tomto diagramu `cardCornerRadius` je nastaven na více zaokrouhlené 10dp a `cardBackgroundColor` je nastaven na `"#FFFFCC"` (světle žlutý).


## <a name="compatibility"></a>Kompatibilita

Můžete použít `CardView` ve verzích systému Android starší než typu Lupa Android 5.0. Protože `CardView` je součástí knihovnu podpory Android v7, můžete použít `CardView` se systémem Android 2.1 (API úrovně 7) a vyšší.
Nicméně je nutné nainstalovat `Xamarin.Android.Support.v7.CardView` balíček, jak je popsáno v [požadavky](#requirements)výše.

`CardView` projevuje mírně odlišné chování na zařízení před typu Lupa (API úrovně 21):

-  `CardView` používá programový stínové implementace, která přidává další odsazení.

-  `CardView` není oříznutí podřízené zobrazení, které průnik s `CardView`je zaokrouhlené rozích.

Abyste při správě tyto rozdíly kompatibility `CardView` poskytuje několik další atributy, které můžete konfigurovat v rozvržení:

-   `cardPreventCornerOverlap` &ndash; Nastavte tento atribut `true` přidat odsazení, když aplikace běží na dříve Android verze (API úrovně 20 a starší). Toto nastavení zabrání `CardView` obsahu z protínající se operátory s `CardView`je zaokrouhlené rozích.

-   `cardUseCompatPadding` &ndash; Nastavte tento atribut `true` přidat odsazení, když aplikace běží v verze Android na nebo větší než úroveň rozhraní API 21. Pokud chcete použít `CardView` na předběžné typu Lupa zařízení a mějte ho vypadají stejně na typu Lupa (nebo novější), nastavte tento atribut na `true`. Pokud tento atribut je povoleno, `CardView` přidá další odsazení k vykreslení stínů, když je spuštěna na předběžné typu Lupa zařízení. To umožňuje překonat rozdíly v odsazení, které jsou zavedené při předběžné typu Lupa programový stínové implementace jsou platné.

Další informace o zachování kompatibility s předchozími verzemi systému Android, najdete v části [zachování kompatibility](https://developer.android.com/training/material/compatibility.html).


## <a name="summary"></a>Souhrn

Tato příručka zavedl nový `CardView` pomůcky zahrnuté v systému Android 5.0 (typu Lupa). Je prokázat výchozí `CardView` vzhled a vysvětlení, jak přizpůsobit `CardView` změnou jeho zvýšení oprávnění, zaoblení obsahu odsazení a barva pozadí. Je uvedený `CardView` rozložení atributy (pomocí odkazu diagramů) a vysvětlení, jak používat `CardView` na zařízení se systémem Android starší než typu Lupa Android 5.0. Další informace o `CardView`, najdete v článku [referenci třídy zobrazení karty aplikace](https://developer.android.com/reference/android/support/v7/widget/CardView.html).


## <a name="related-links"></a>Související odkazy

- [RecyclerView (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Úvod do typu Lupa](~/android/platform/lollipop.md)
- [Odkaz na zobrazení karty aplikace – třída](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
