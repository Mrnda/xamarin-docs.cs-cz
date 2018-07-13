---
title: Rozložení v Xamarin.Forms
description: Xamarin.Forms má několik rozložení a funkce pro uspořádání obsahu na obrazovce, a tento článek vysvětluje je.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: cff5f9c15f4608ecfb643d2c49dd636df8b18b5c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995752"
---
# <a name="layouts-in-xamarinforms"></a>Rozložení v Xamarin.Forms

Xamarin.Forms má několik rozložení a funkce pro uspořádání obsahu na obrazovce.

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Rozložení Xamarin.Forms, [Xamarin University](https://university.xamarin.com/)**

Každý ovládací prvek rozložení je popsáno níže, a také podrobnosti o tom, jak zpracovat změny orientace obrazovky:

* **[StackLayout](stack-layout.md)**  &ndash; použít k zobrazení lineárně, uspořádat vodorovně nebo svisle. Zobrazení StackLayout může být zarovnaný na System center, doleva nebo doprava rozložení.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; umožňuje uspořádat zobrazení tak, že nastavíte souřadnice & velikost jako absolutní hodnoty nebo poměry. AbsoluteLayout slouží k zobrazení vrstvy a také ukotvit vlevo, vpravo nebo System center.
* **[RelativeLayout](relative-layout.md)**  &ndash; používá se k uspořádání zobrazení nastavením omezení vzhledem k jejich nadřazené rozměry a umístění.
* **[Mřížka](grid.md)**  &ndash; používá se k uspořádání zobrazení v mřížce. Řádky a sloupce lze z hlediska absolutní hodnoty nebo poměry.
* **[FlexLayout](flex-layout.md)**  &ndash; používá se k uspořádání zobrazení obtékané vodorovně nebo svisle.
* **[ScrollView](scroll-view.md)**  &ndash; používají k zajištění posouvání při zobrazení se nemůže vejít zcela v rámci hranice obrazovky.
* **[LayoutOptions](layout-options.md)**  &ndash; definovat zarovnání a rozšíření pro zobrazení, relativně k nadřazenému.
* **[Vstup transparentnosti](#input_transparency)**  &ndash; Určuje, zda element přijímá vstup.
* **[Okraje a odsazení](margin-and-padding.md)**  &ndash; ukazuje, jak řídit rozložení chování při vykreslení elementu v uživatelském rozhraní.
* **[Orientace zařízení](device-orientation.md)**  &ndash; vysvětluje, jak zpracovat změny orientace zařízení.
* **[Rozložení na zařízeních tablet a desktop](tablet.md)**  &ndash; ukazuje, jak optimalizovat pro větší obrazovky na jednotlivých platformách.
* **[Vytvoření vlastního rozložení](custom.md)**  &ndash; vysvětluje, jak vytvořit vlastní rozložení třídy.
* **[Komprese rozložení](layout-compression.md)**  &ndash; odebere zadané za účelem zvýšení výkonu vykreslování stránky rozložení z vizuálního stromu.

Platforma ovládacích prvků lze také přímo v rozložení Xamarin.Forms s [ **nativní vkládání** ](~/xamarin-forms/platform/native-views/index.md) (nového v Xamarin.Forms 2.2), a můžete je [ **vytvářet vlastní rozložení** ](custom.md) podle specifických požadavků.

Na následujícím obrázku vizualizuje rozložení ovládacích prvků:

[![](images/layouts-sml.png "Rozložení Xamarin.Forms")](images/layouts.png#lightbox "rozložení Xamarin.Forms")

## <a name="choosing-the-right-layout"></a>Výběr správné rozložení

Rozložení, které zvolíte ve vaší aplikaci můžete pomoci nebo nepříznivě ovlivnit, jak vytváříte aplikace Xamarin.Forms atraktivní a použitelný. Trvá nějakou dobu vzít v úvahu, jak funguje každá rozložení vám může pomoci psát přehlednější a škálovat kód uživatelského rozhraní. Na obrazovce můžete mít kombinaci různá rozložení k dosažení konkrétního návrhu.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` Se používá pro zobrazení zobrazení na řádek, který je vodorovně nebo svisle. Umístění a velikost v rámci rozložení závisí na zobrazení `HeightRequest`, `WidthRequest`, `HorizontalOptions` a `VerticalOptions`. `StackLayout` se často používá jako základní rozložení uspořádání jiné rozložení na obrazovce.

Příklad nad tím, kdy `StackLayout` by být dobrou volbou, vezměte v úvahu aplikaci, která potřebuje rychle zobrazit tlačítko a popisek s popiskem zarovnané vlevo a tlačítko zarovnaný doprava.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="flexlayoutflex-layoutmd"></a>[FlexLayout](flex-layout.md)

`FlexLayout` Je podobný `StackLayout` , zobrazí zobrazení podřízených vodorovně nebo svisle:

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">

    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

Nicméně, pokud existuje příliš mnoho podřízených objektů pro jeden řádek nebo sloupec, `FlexLayout` se taky může wrapping tato zobrazení. `FlexLayout` je založená na modulu šablon stylů CSS flexibilní pole rozložení a má mnoho stejné integrované možnosti pro umístění a zarovnání své podřízené objekty.

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` Se používá pro zobrazení zobrazení s velikostí a umístění se zadat buď jako explicitní hodnoty nebo hodnoty vzhledem k velikosti rozložení. Na rozdíl od `StackLayout` a `Grid`, `AbsoluteLayout` umožňuje podřízené zobrazení, které se překrývají. Na rozdíl od `RelativeLayout`, `AbsoluteLayout` neumožňuje umístit prvky mimo obrazovku.

Příklad nad tím, kdy `AbsoluteLayout` by být dobrou volbou, vezměte v úvahu aplikace, kterou je potřeba k dispozici kolekce objektů jako zásobníků. To je často vidět při zobrazení alba fotografií nebo skladeb. Následující kód obsahuje vzhled balík, s prvky otočeny pomocného parametru na obsah celé:

V XAML:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

Mějte na paměti následující aspekty výše uvedený kód:

- Každý `Image` se zobrazí na stejné pozici (uprostřed vodorovné mezery)
- `Padding` Považuje `AbsoluteLayout`, na rozdíl od `RelativeLayout`, které ignoruje.
- `AbsoluteLayout.LayoutFlags` Určuje, jak budou interpretovány hranice rozložení. V tomto případě `PositionProportional`, znamená, že souřadnice poměr velikost rozložení, při velikosti, bude vyhodnocen jako explicitní velikost.
- `AbsoluteLayout.Layoutbounds` Určuje vodorovné umístění, svislou pozici, šířku a výšku v uvedeném pořadí.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` Se používá pro zobrazení zobrazení s velikost a umístění, zadat jako hodnoty vzhledem k hodnoty rozložení nebo v jiném zobrazení. Relativní hodnoty tak, aby odpovídaly má odpovídající hodnotu v související zobrazení není nutné. Jako příklad, je možné nastavit zobrazení `Width` vlastnost jako proporcionální do jiného zobrazení `X` vlastnost.

RelativeLayout slouží k vytváření uživatelského rozhraní, které se škálují proporcionálně velikosti zařízení. Následující XAML implementuje návrh s použitím polí v nejvyšší rozích s flagpole s příznakem v centru:

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

Mějte na paměti následující aspekty výše uvedený kód:

- Pozice a velikosti jsou zadané jako omezení.
- Flagpole jmenuje tak, aby příznaku (zelený pole) je možné nastavit pozici vzhledem k flagpole.
- Výrazy omezení `Factor` a `Constant` vlastnosti, které je možné použít k definování pozice a velikosti jako násobky (nebo podíly) z vlastností objektu jiných objektů včetně konstantu. Konstanty může být záporné.

### <a name="gridgridmd"></a>[Mřížka](grid.md)

`Grid` Slouží k zobrazení prvků v řádky a sloupce. Všimněte si, že mřížce se nejedná o tabulku, takže nemá koncept buněk, řádků záhlaví a zápatí nebo ohraničení mezi řádky a sloupce. Obecně platí není vhodná pro tabulková data zobrazení mřížky. Pro použití, vezměte v úvahu [ListView](~/xamarin-forms/user-interface/listview/index.md) nebo [zobrazení Tabulka](~/xamarin-forms/user-interface/tableview.md).

Příklad nad tím, kdy `Grid` je správné rozložení chcete použít, zvažte číselného vstupu pro kalkulačku. Číselného vstupu pro kalkulačku může obsahovat čtyři řádky a tři sloupce, každý s tlačítkem. Následující kód implementuje tohoto návrhu:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

Mějte na paměti následující aspekty výše uvedený kód:

- Tabulky a sloupce jsou explicitně zadán, není odvozen z obsahu.
- `Height` a `Width` hodnoty je možné nastavit na hvězdičkami, což znamená, že mřížky, nastaví tyto hodnoty tak, aby vyplnil dostupné místo.
- Je určená pozice každé tlačítko `Grid.Row`  &  `Grid.Column` vlastnosti.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktury lze definovat zarovnání a rozšíření pro zobrazení, relativně k nadřazenému.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Okraje a odsazení](margin-and-padding.md)

[ `Margin` ](xref:Xamarin.Forms.View.Margin) a [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) vlastnosti řídit rozložení chování při vykreslení elementu v uživatelském rozhraní.

<a name="input_transparency" />

### <a name="input-transparency"></a>Vstupní transparentnosti

Každý prvek má [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) vlastnost, která slouží k určení, zda elementu přijímá vstup. Výchozí hodnota je `false`, zajištění, že element přijímá vstup.

Když tato vlastnost nastavena na třídu kontejneru, jako je rozložení třídy, jeho hodnota přenosy do podřízených elementů. Proto se nastavení [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) vlastnost `true` na rozložení třídy způsobí elementů v rámci rozložení nepřijímá vstup.

### <a name="device-orientationdevice-orientationmd"></a>[Orientace zařízení](device-orientation.md)

Xamarin.Forms a jeho integrované rozložení jsou schopné zpracovat změny v orientace zařízení. Vezměte v úvahu orientace, které bude aplikace podporovat, a jak si využití místa k dispozici v režimu na šířku a výšku.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Rozložení pro Tablet a Desktop](tablet.md)

iOS, Android a univerzální platformu Windows podporují větší velikosti obrazovky na tablet zařízení (a také laptopů a stolních počítačů pro Windows). Xamarin.Forms umožňuje optimalizovat aplikaci pro větší obrazovky detekce typu zařízení a úpravě rozložení stránky nebo zcela pomocí zcela různé stránky pro větší obrazovky.

### <a name="creating-a-custom-layoutcustommd"></a>[Vytvoření vlastního rozložení](custom.md)

Xamarin.Forms definuje čtyři rozložení třídy – [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout), a [ `Grid` ](xref:Xamarin.Forms.Grid), a každý uspořádá podřízené jiným způsobem. Ale někdy poskytované jeho nezbytné k uspořádání obsahu stránky, ne pomocí rozložení Xamarin.Forms. Tento článek vysvětluje, jak psát vlastní rozložení třídy a ukazuje orientace citlivé `WrapLayout` třídu, která uspořádá podřízené vodorovně na stránce a tento algoritmus pak zabalí zobrazení další podřízené objekty další řádky.

### <a name="layout-compressionlayout-compressionmd"></a>[Komprese rozložení](layout-compression.md)

Komprese rozložení odebere zadané rozložení z vizuálního stromu za účelem zvýšení výkonu vykreslování stránky. Zlepšuje výkon, který to poskytuje se liší v závislosti na složitosti stránku, verze operačního systému se používají a zařízení, na kterém je aplikace spuštěna. Největší zvýšení výkonu se však projeví na starší zařízení.

## <a name="making-your-choice"></a>Provedení výběru

Mějte na paměti, že ve většině případů více než jednu možnost rozložení je možné provádět požadované návrhu. Když existuje několik možností platný, zvažte, jaký přístup bude nejjednodušší pro vaši situaci.
Většina návrhy nelze dají realizovat s využitím právě jedno rozložení, tak vnoření rozložení jako potřebné k vytvoření složitějších návrhy.


## <a name="related-links"></a>Související odkazy

- [Pokyny pro lidské rozhraní společnosti Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Návrh webu Android](https://developer.android.com/design/index.html)
- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
