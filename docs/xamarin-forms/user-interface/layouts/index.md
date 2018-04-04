---
title: Rozložení
description: Rozložení zobrazení na obrazovce.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 24d9dd332275fd811c0ff60fc514ae0f84c6ee07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="layouts"></a>Rozložení

Xamarin.Forms má několik rozložení a funkce pro uspořádání obsahu na obrazovce. 

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Rozložení Xamarin.Forms, pomocí [univerzity Xamarin](https://university.xamarin.com/)**

Každý ovládací prvek rozložení je popsáno níže, a také podrobnosti o tom, jak zpracovat změny orientace obrazovky:

* **[StackLayout](stack-layout.md) ** &ndash; použitá k uspořádání zobrazení lineárně, vodorovně nebo svisle. Zobrazení StackLayout může být zarovnaný na střed doleva nebo doprava na rozložení.
* **[AbsoluteLayout](absolute-layout.md) ** &ndash; umožňuje uspořádat zobrazení podle nastavení souřadnic & velikost jako absolutní hodnoty nebo poměry. AbsoluteLayout slouží k zobrazení vrstvy a také je ukotvení vlevo, vpravo nebo center.
* **[RelativeLayout](relative-layout.md) ** &ndash; použitá k uspořádání zobrazení nastavením omezení vzhledem k jejich nadřazené rozměry a umístění.
* **[Mřížky](grid.md) ** &ndash; použitá k uspořádání zobrazení v mřížce. Řádků a sloupců lze zadat jako absolutní hodnoty nebo poměry.
* **[ScrollView](scroll-view.md) ** &ndash; použitý k poskytnutí posouvání při zobrazení se nemůže vejít zcela v rámci hranice na obrazovce.
* **[LayoutOptions](layout-options.md) ** &ndash; definovat zarovnání a rozšíření pro zobrazení, relativně k jeho nadřazený objekt.
* **[Vstupní průhlednost](#input_transparency) ** &ndash; Určuje, jestli element obdrží vstup.
* **[Okraj a odsazení](margin-and-padding.md) ** &ndash; ukazuje, jak můžete řídit chování rozložení po vykreslení elementu v uživatelském rozhraní.
* **[Orientace zařízení](device-orientation.md) ** &ndash; vysvětluje, jak zpracovat změny orientace zařízení.
* **[Rozložení v zařízení tablet a vzdálené ploše](tablet.md) ** &ndash; ukazuje, jak optimalizovat pro větší obrazovky na každou platformu.
* **[Vytvoření vlastního rozložení](custom.md) ** &ndash; vysvětluje, jak vytvořit vlastní rozložení třídu.
* **[Komprese rozložení](layout-compression.md) ** &ndash; Odebere zadaný ve snaze zvýšit výkon vykreslování stránky rozložení z vizuálním stromu.

Ovládací prvky platformy můžete taky použít přímo v Xamarin.Forms rozložení s [ **nativní vložení** ](~/xamarin-forms/platform/native-views/index.md) (nové v Xamarin.Forms 2.2), a můžete [ **vytvořit vlastní rozložení** ](custom.md) podle specifických požadavků.

Na následujícím obrázku vizualizuje rozložení ovládacích prvků:

[![](images/layouts-sml.png "Rozložení Xamarin.Forms")](images/layouts.png#lightbox "Xamarin.Forms rozložení")

## <a name="choosing-the-right-layout"></a>Výběr správné rozložení

Rozložení, který zvolíte ve vaší aplikaci můžete pomoci nebo narušit vám, jak vytváříte aplikaci Xamarin.Forms přitažlivé a dá se použít. Zvažte, jak funguje každé rozložení můžete napsat kód uživatelského rozhraní větší škálovatelnost a čisticí nějakou dobu trvá. Na obrazovce může mít kombinaci různých rozložení a dosáhnout konkrétní návrh.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` Se používá pro zobrazení zobrazení podél řádek, na kterém je vodorovně nebo svisle. Pozice a velikosti v rámci rozložení je určen na základě pro zobrazení `HeightRequest`, `WidthRequest`, `HorizontalOptions` a `VerticalOptions`. `StackLayout` se často používá jako základní rozložení, uspořádání jiná rozložení klávesnice na obrazovce.

Příklad při `StackLayout` by být vhodné použít, zvažte aplikaci, která potřebuje k zobrazení tlačítka a popisek, popisek zarovnaný doleva a tlačítko zarovnaný doprava.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` Se používá pro zobrazení zobrazení, velikosti a pozice se zadat buď jako explicitní hodnoty nebo hodnoty vzhledem k velikosti rozložení. Na rozdíl od `StackLayout` a `Grid`, `AbsoluteLayout` umožňuje podřízené zobrazení, která překrývají. Na rozdíl od `RelativeLayout`, `AbsoluteLayout` neumožní umístit elementy mimo obrazovku.

Příklad při `AbsoluteLayout` by být vhodné použít, zvažte aplikaci, která je k dispozici kolekce objektů jako zásobníky. To je často vidět během zobrazení alba fotografií nebo skladeb. Následující kód dává vzhled relačních operátorů, u elementů s otočením pomocného parametru na obsah relačních operátorů:

V jazyce XAML:

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

Všimněte si následujících charakteristik ve výše uvedeném kódu:

- Každý `Image` se zobrazí ve stejné pozici (uprostřed vodorovný prostor)
- `Padding` Považuje `AbsoluteLayout`, na rozdíl od `RelativeLayout`, která bude ignorovat.
- `AbsoluteLayout.LayoutFlags` Určuje, jak se interpretují hranice rozložení. V takovém případě `PositionProportional`, znamená, že souřadnice poměr velikosti rozložení, při velikosti, bude vyhodnocen jako explicitní velikost.
- `AbsoluteLayout.Layoutbounds` v tomto pořadí určuje pozice ve vodorovném směru, pozice ve svislém směru, šířku a výšku.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` Se používá pro zobrazení zobrazení, velikosti a pozice zadána jako relativní vzhledem k hodnoty rozložení nebo jiného zobrazení hodnoty. Relativní hodnoty nemusí odpovídat mu odpovídající hodnotu v relaci zobrazení. Jako příklad, je možné nastavit zobrazení `Width` vlastnost, která má být přímo úměrná do jiného zobrazení `X` vlastnost.

RelativeLayout lze použít k vytvoření uživatelská rozhraní, které úměrně napříč velikosti zařízení. Následující XAML implementuje návrh s polí v nejhornější rozích s flagpole s příznakem v centru:

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

Všimněte si následujících charakteristik ve výše uvedeném kódu:

- Pozice a velikosti jsou zadané jako omezení.
- Flagpole jmenuje tak, aby příznaku (zelený pole) lze nastavit pozice relativně k flagpole.
- Výrazy omezení mít `Factor` a `Constant` vlastnosti, které se dají použít k definování pozice a velikosti jako násobky (nebo zlomků) vlastností jiné objekty, plus konstanta. Konstanty může být záporná.

### <a name="gridgridmd"></a>[Mřížka](grid.md)

`Grid` Se používá pro zobrazení prvků v řádky a sloupce. Všimněte si, že mřížky se nejedná o tabulku, takže nemá koncept buněk, řádků záhlaví a zápatí nebo ohraničení mezi řádky a sloupce. Obecně platí není vhodná pro zobrazení tabulková data mřížky. Pro použití, vezměte v úvahu [ListView](~/xamarin-forms/user-interface/listview/index.md) nebo [zobrazení Tabulka](~/xamarin-forms/user-interface/tableview.md).

Příklad při `Grid` není pravé rozložení chcete použít, zvažte číselný vstup kalkulačky. Číselný vstup kalkulačky může obsahovat čtyři řádků a sloupců tři, každý s tlačítko. Následující kód implementuje tohoto návrhu:

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

Všimněte si následujících charakteristik ve výše uvedeném kódu:

- Mřížky a sloupce jsou explicitně určena, nelze odvodit z obsahu.
- `Height` a `Width` hodnoty může být nastaven na hvězdičkami, což znamená, že mřížky nastaví těchto hodnot vyplňování dostupného místa.
- Pozice každé tlačítko je zadána `Grid.Row`  &  `Grid.Column` vlastnosti.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktura můžete používat k definování zarovnání a rozšíření pro zobrazení, relativně k jeho nadřazený objekt.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Okraje a odsazení](margin-and-padding.md)

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) a [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) vlastnosti řízení rozložení chování při vykreslení elementu v uživatelském rozhraní.

<a name="input_transparency" />

### <a name="input-transparency"></a>Vstupní průhlednosti

Každý prvek má [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) vlastnost, která se používá k definování, jestli element obdrží vstup. Výchozí hodnota je `false`, zajistíte, že element obdrží vstup.

Když je tato vlastnost nastavena na třídu kontejneru, například třída rozložení, jeho hodnota přenosy do podřízených elementů. Proto nastavení [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) vlastnost `true` na rozložení třída způsobí elementů v rámci rozložení nepřijaté vstup.

### <a name="device-orientationdevice-orientationmd"></a>[Orientace zařízení](device-orientation.md)

Xamarin.Forms a jeho předdefinované rozložení jsou umožňuje zpracovávat změny v orientace zařízení. Zvažte, které orientace aplikace podporovat, a jak budete provedete využití místa v orientaci na výšku a režimy.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Rozložení pro aplikace, tablety a vzdálené ploše](tablet.md)

iOS, Android a Windows tablet všechny podporují větší velikost obrazovky na platformách zařízení (i laptopy a desktopy pro Windows). Xamarin.Forms umožňuje optimalizovat vaší aplikace pro větší obrazovky zjišťování typu zařízení a úpravě rozložení stránky nebo zcela pomocí uvidíte úplně jiné stránky pro větší obrazovky.

### <a name="creating-a-custom-layoutcustommd"></a>[Vytvoření vlastního rozložení](custom.md)

Definuje Xamarin.Forms třídy čtyři rozložení - [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), a [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), a všechny její podřízené položky uspořádá jiným způsobem. Ale někdy poskytované jeho nezbytné k uspořádání obsahu stránce pomocí rozložení není Xamarin.Forms. Tento článek vysvětluje, jak napsat vlastní rozložení třídu a ukazuje orientaci citlivé `WrapLayout` třídu, která uspořádá podřízené vodorovně na stránce a potom zabalí zobrazení následné podřízené objekty další řádky.

### <a name="layout-compressionlayout-compressionmd"></a>[Komprese rozložení](layout-compression.md)

Komprese rozložení Odebere zadaný rozložení vizuálním stromu ve snaze zvýšit výkon vykreslování stránky. Výhody výkonu, který to doručí se liší v závislosti na složitosti stránky, na verzi operačního systému používá a zařízení, na kterém je aplikace spuštěna. Ale největších zvýšení výkonu se zobrazí na starší zařízení.

## <a name="making-your-choice"></a>Provedení výběru

Upozorňujeme, že ve většině případů více než jednu možnost rozložení lze použít k implementaci návrhu požadované. Pokud nejsou k dispozici více možností platný, zvažte, jaký přístup budou nejjednodušší pro vaši situaci.
Většina návrhy nelze nastane s jedním rozložení, aby vnoření rozložení jako vytvořit složitější návrhů.


## <a name="related-links"></a>Související odkazy

- [Lidské rozhraní pokynů společnosti Apple](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android návrh webu](https://developer.android.com/design/index.html)
- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
