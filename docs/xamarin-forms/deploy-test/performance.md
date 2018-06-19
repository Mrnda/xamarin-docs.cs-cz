---
title: Výkon Xamarin.Forms
description: Pro zvýšení výkonu aplikací Xamarin.Forms mnoha způsoby. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace. Tento článek popisuje a tyto postupy.
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 37d99add473203d90cb1b420536827e34e834a2b
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209320"
---
# <a name="xamarinforms-performance"></a>Výkon Xamarin.Forms

_Pro zvýšení výkonu aplikací Xamarin.Forms mnoha způsoby. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace. Tento článek popisuje a tyto postupy._

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Momentální 2016: Optimalizace výkonu aplikace s Xamarin.Forms**

## <a name="overview"></a>Přehled

Výkon nízký aplikace prezentuje mnoha způsoby. Aplikace může díky pravděpodobně reagovat, může způsobit pomalé posouvání a může snížit z baterie. Ale optimalizace výkonu zahrnuje více než jen implementace efektivní kódu. Musíte také zvážit možnosti pro uživatele s výkonem aplikace. Například zajistíte, že operace spustit bez blokování uživatele z jiné aktivity vám může pomoct vylepšit možnosti pro uživatele.

Existuje několik postupů pro zvýšení výkonu a dosahovaný výkon aplikace Xamarin.Forms. Mezi ně patří:

- [Povolit kompilátor jazyka XAML](#xamlc)
- [Vyberte správný rozložení](#correctlayout)
- [Povolit kompresi rozložení](#layoutcompression)
- [Použít pro rychlé vykreslování](#fastrenderers)
- [Snižte nepotřebné vazby](#databinding)
- [Optimalizace výkonu rozložení](#optimizelayout)
- [Optimalizace výkonu ListView](#optimizelistview)
- [Optimalizovat prostředky obrázků](#optimizeimages)
- [Snižte velikost vizuálním stromu](#visualtree)
- [Snižte velikost slovník prostředků aplikace](#resourcedictionary)
- [Použití vzoru vlastní zobrazovací jednotky](#rendererpattern)

> [!NOTE]
>  Před přečtení tohoto článku měli nejdřív přečíst [napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md), který popisuje konkrétní techniky jiné platformy ke zlepšení využití paměti a výkon aplikace vytvořené pomocí platformy Xamarin.

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>Povolit kompilátor jazyka XAML

XAML můžete volitelně zkompilovat přímo do převodní jazyk (IL) s kompilátoru jazyka XAML (XAMLC). XAMLC nabízí řadu výhody:

- Provede kompilaci kontrola XAML, upozornění uživatele o chybách.
- Odebere některé zatížení a vytváření instancí času elementů XAML.
- Pomáhá snížit velikost souboru poslední sestavení už zahrnutím souborů XAML.

XAMLC vypnutá ve výchozím nastavení pro zajištění zpětné kompatibility. Však může být povoleno v sestavení a úrovni třídy. Další informace najdete v tématu [kompilování XAML](~/xamarin-forms/xaml/xamlc.md).

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>Vyberte správný rozložení

Rozložení, pro který je schopná zobrazit více podřízených položek, ale který má jenom jednu podřízenou, je plýtvání. Například následující příklad kódu ukazuje [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) s jediné podřízené:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <StackLayout>
            <Image Source="waterfront.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Toto je plýtvání a [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) element měla by být odebrána, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

Kromě toho není pokus o reprodukujte vzhled konkrétní rozložení pomocí kombinace jiné rozložení jako výsledkem nepotřebné rozložení výpočty provádí. Například nemáte pokusí reprodukujte [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) rozložení pomocí kombinace [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) instance. Následující příklad kódu ukazuje příklad tento chybný postup:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" />
                <Entry Placeholder="Enter your name" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Age:" />
                <Entry Placeholder="Enter your age" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Occupation:" />
                <Entry Placeholder="Enter your occupation" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Address:" />
                <Entry Placeholder="Enter your address" />
            </StackLayout>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Toto je plýtvání, protože nepotřebné rozložení výpočty probíhají. Místo toho na požadované rozložení můžete lépe dosáhnout pomocí [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), jak ukazuje následující příklad kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="100" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
            </Grid.RowDefinitions>
            <Label Text="Name:" />
            <Entry Grid.Column="1" Placeholder="Enter your name" />
            <Label Grid.Row="1" Text="Age:" />
            <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
            <Label Grid.Row="2" Text="Occupation:" />
            <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
            <Label Grid.Row="3" Text="Address:" />
            <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

<a name="layoutcompression" />

## <a name="enable-layout-compression"></a>Povolit kompresi rozložení

Komprese rozložení Odebere zadaný rozložení ze stromu visual ve snaze zvýšit výkon vykreslování stránky. Výhody výkonu, který to doručí se liší v závislosti na složitosti stránky, na verzi operačního systému používá a zařízení, na kterém je aplikace spuštěna. Ale největších zvýšení výkonu se zobrazí na starší zařízení. Další informace najdete v tématu [rozložení komprese](~/xamarin-forms/user-interface/layouts/layout-compression.md).

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>Použít pro rychlé vykreslování

Rychlé nástroji pro vykreslování snížit inflace a náklady na vykreslování ovládacích prvků Xamarin.Forms v systému Android pomocí sloučení výsledná hierarchie nativní ovládací prvek. Tato další zlepšuje výkon vytvořením menší počet objektů, které v změní výsledky v méně složitých vizuálním stromu a menší využití paměti. Další informace najdete v tématu [rychlého nástroji pro vykreslování](~/xamarin-forms/internals/fast-renderers.md).

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>Snižte nepotřebné vazby

Nepoužívejte vazby pro obsah, který jde snadno nastavit staticky. V vazby dat, která nemusí být vázaný, protože vazby nejsou efektivní náklady se žádné výhody. Například nastavení `Button.Text = "Accept"` má nižší režijní náklady než vazby [ `Button.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/) k ViewModel `string` vlastnost s hodnotou "Přijmout".

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>Optimalizace výkonu rozložení

Xamarin.Forms 2 se zavedl modul optimalizované rozložení, který ovlivňuje rozložení aktualizace. Pokud chcete získat nejlepšího výkonu dosáhnete možné rozložení, postupujte podle následujících pokynů:

- Snižte hloubku hierarchie rozložení zadáním [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) hodnoty vlastností, umožní vytvoření rozložení s méně zabalení zobrazení. Další informace najdete v tématu [okraje a odsazení](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- Při použití [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), pokuste se zjistit, že jako několik řádků a sloupců nejdříve jsou nastaveny pro [ `Auto` ](https://developer.xamarin.com/api/property/Xamarin.Forms.GridLength.Auto/) velikost. Každý sloupce či Automatická velikost řádku způsobí, že modul rozložení provádět výpočty další rozložení. Místo toho použijte pevné velikosti řádků a sloupců, pokud je to možné. Alternativně nastavte řádků a sloupců tak, aby zabíral přímo úměrná množství místa s [ `GridUnitType.Star` ](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) hodnota výčtu, zadat, že stromu nadřazené řídí tyto pokyny rozložení.
- Není nastavený [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) a [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnosti rozložení Pokud to není vyžadováno. Výchozí hodnoty [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) a [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) povolit pro nejlepší optimalizace rozložení. Změna těchto vlastností má náklady a odebírá paměti, i když jejich nastavením na výchozí hodnoty.
- Nepoužívejte [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) kdykoli je to možné. Výsledkem bude procesoru museli provádět výrazně další práci.
- Při použití [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), nepoužívejte [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) vlastnost kdykoli je to možné.
- Při použití [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), ujistěte se, že pouze jeden podřízený je nastaven na [ `LayoutOptions.Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/). Tato vlastnost zajišťuje, že bude zabírat zadaný podřízený na největší místo `StackLayout` můžete předat a je plýtvání provést tyto výpočty více než jednou.
- Nemůžete volat metody [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) třídy, jak vyplývá ve výpočtech nákladné rozložení provádí. Místo toho je pravděpodobné, že je možné získat požadované rozložení chování nastavení [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) vlastnosti. Alternativně podtřídami [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) třída k dosažení požadované rozložení chování.
- Nemáte žádné aktualizace [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance častěji, než je třeba, protože celou obrazovku rozložení se znovu počítané může způsobit změnu velikosti popisku.
- Není nastavený [ `Label.VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) vlastnost Pokud to není vyžadováno.
- Nastavte [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) všech [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) instance na [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/) kdykoli je to možné.

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>Optimalizace výkonu ListView

Při použití [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) existuje mnoho činností koncového uživatele, které by mělo být optimalizované ovládacího prvku:

- **Inicializace** – časový interval spuštění při vytvoření ovládacího prvku a až do položky se zobrazují na obrazovce.
- **Posouvání** – možnost vyhledejte v seznamu a ujistěte se, že rozhraní není funkce lag touch gesta.
- **Interakce** pro přidávání, odstraňování a výběr položek.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Aplikace a dodávají data buňky šablony vyžaduje ovládací prvek. Jak to se dá dosáhnout může mít velký dopad na výkon ovládacího prvku. Další informace najdete v tématu [ListView výkonu](~/xamarin-forms/user-interface/listview/performance.md).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimalizovat prostředky obrázků

Zobrazení prostředků bitové kopie může výrazně zvýšit spotřeba paměti aplikace. Proto se musí jenom vytvářet, pokud vyžaduje a by měly být uvolněny, jakmile je aplikace již nevyžaduje. Například pokud aplikace zobrazuje bitovou kopii přečíst data z datového proudu, ujistěte se, že tohoto datového proudu se vytvoří jenom v případě, že je to požadováno a ujistěte se, že datový proud vydání, když už musí. Toho lze dosáhnout vytvořením datového proudu, když vytvoření stránky, nebo když [ `Page.Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) aktivuje událost a potom uvolnění datového proudu při [ `Page.Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Disappearing/) je aktivována událost.

Při stahování obrázek pro zobrazení s [ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) metoda mezipaměti stažené obrázek podle zajistit, aby [ `UriImageSource.CachingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) je nastavena na `true`. Další informace najdete v tématu [práce s obrázky](~/xamarin-forms/user-interface/images.md).

Další informace najdete v tématu [optimalizovat prostředky obrázků](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>Snižte velikost vizuálním stromu

Snižuje počet elementů na stránce budou rychlejší vykreslení stránky. Pro dosažení tohoto cíle dvěma způsoby. První je skrýt prvky, které nejsou viditelné. [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) Vlastnost jednotlivých prvků určuje, jestli element musí být součástí vizuálním stromu, nebo ne. Proto pokud element není viditelný, protože je skrytá za další prvky, buď odeberte element nebo nastavte její `IsVisible` vlastnost `false`.

Druhý způsob spočívá je odeberte nepotřebné elementy. Například následující příklad kódu ukazuje rozložení stránky, která zobrazuje řadu [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) prvky:

```xaml
<ContentPage.Content>
    <StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Hello" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Welcome to the App!" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Downloading Data..." />
        </StackLayout>
    </StackLayout>
</ContentPage.Content>
```

Stejné rozložení stránky se dají udržovat s počtem snížené elementu, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ContentPage.Content>
  <StackLayout Padding="20,20,0,0" Spacing="25">
    <Label Text="Hello" />
    <Label Text="Welcome to the App!" />
    <Label Text="Downloading Data..." />
  </StackLayout>
</ContentPage.Content>
```

<a name="resourcedictionary" />

## <a name="reduce-the-application-resource-dictionary-size"></a>Snižte velikost slovník prostředků aplikace

Všechny prostředky, které se používají v rámci aplikace by měly být uložené ve slovníku prostředků aplikace předejdete duplikace. To vám pomůže snížit množství XAML, který má být analyzovat v celé aplikaci. Následující příklad kódu ukazuje `HeadingLabelStyle` prostředku, který je používané aplikace široké a proto je definována v slovník prostředků aplikace:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
         <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
         </ResourceDictionary>
     </Application.Resources>
</Application>
```

Ale XAML, které jsou specifické pro stránku by neměly být obsažené ve slovníku prostředků aplikace, jako prostředky budou analyzovat poté při spuštění aplikace místo, pokud to vyžaduje na stránce. Pokud prostředek je používá stránky, který není úvodní stránce, musí být umístěny ve slovníku prostředků pro tuto stránku, proto pomáhá snížit XAML, který je analyzovat při spuštění aplikace. Následující příklad kódu ukazuje `HeadingLabelStyle` prostředku, který je jenom na jedné stránce a proto je definována v slovník prostředků stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
     <ContentPage.Resources>
          <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
     </ContentPage.Resources>
    <ContentPage.Content>
      ...
    </ContentPage.Content>
</ContentPage>

```

Další informace o prostředky aplikace najdete v tématu [ `Working with Styles` ](~/xamarin-forms/user-interface/styles/index.md).

<a name="rendererpattern" />

## <a name="use-the-custom-renderer-pattern"></a>Použití vzoru vlastní zobrazovací jednotky

Většina renderer třídy zveřejněte `OnElementChanged` metodu, která je volána, když Xamarin.Forms vlastního ovládacího prvku se vytvoří pro vykreslení odpovídající nativní ovládacího prvku. Třídy vlastní zobrazovací jednotky, v každé třídě renderer specifické pro platformu pak přepsat tuto metodu za účelem vytváření instancí a přizpůsobení nativní ovládací prvek. `SetNativeControl` Metoda se používá k vytvoření instance nativní ovládací prvek a tato metoda bude přiřadit také odkaz na ovládací prvek `Control` vlastnost.

Ale v některých případech `OnElementChanged` metodu lze volat vícekrát. Proto aby se zabránilo nevracení paměti, což může mít dopad na výkon, musí dát pozor při vytvoření instance nového nativní ovládacího prvku. Přístup použít při vytvoření instance nového nativní ovládacího prvku ve vlastní zobrazovací jednotky je znázorněno v následujícím příkladu kódu:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Nový nativní ovládací prvek by měla být vytvořená pouze jednou, pokud `Control` vlastnost je `null`. Ovládací prvek by měl být nakonfigurovaný jenom a po vlastní zobrazovací jednotky k nové element Xamarin.Forms přihlásit k odběru obslužné rutiny událostí. Podobně, všechny obslužné rutiny, které byly přihlásit k odběru by měly být jenom v odhlásit po vykreslení elementu k změny. Přijetí tento přístup vám pomůže vytvořit efektivní provádění vlastní zobrazovací jednotky, která není trpí nevracení paměti.

Další informace o nástroji pro vykreslování vlastní najdete v tématu [přizpůsobení ovládacích prvků na každou platformu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="summary"></a>Souhrn

Tento článek popisuje a popsané techniky pro zvýšení výkonu aplikací Xamarin.Forms. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace.


## <a name="related-links"></a>Související odkazy

- [Napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Výkon ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [Rychlé renderery](~/xamarin-forms/internals/fast-renderers.md)
- [Komprese rozložení](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Ukázka Úprava velikosti obrázku Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/XamFormsImageResize/)
- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilation/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
