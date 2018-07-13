---
title: Výkon Xamarin.Forms
description: Pro zvýšení výkonu aplikací Xamarin.Forms mnoha způsoby. Společně tyto postupy mohou výrazně snížit množství práce prováděné procesoru a paměti spotřebované aplikací. Tento článek popisuje a těchto technik.
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: d7719f231a6d70594985a1158340104d68367ffe
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998605"
---
# <a name="xamarinforms-performance"></a>Výkon Xamarin.Forms

_Pro zvýšení výkonu aplikací Xamarin.Forms mnoha způsoby. Společně tyto postupy mohou výrazně snížit množství práce prováděné procesoru a paměti spotřebované aplikací. Tento článek popisuje a těchto technik._

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Rozvoj 2016: Optimalizace výkonu aplikací pomocí Xamarin.Forms**

## <a name="overview"></a>Přehled

Nízký výkon aplikace prezentuje v mnoha způsoby. Může být aplikace vypadá to, že nereaguje, může způsobit pomalé posouvání a může snížit výdrži baterie. Ale optimalizace výkonu zahrnuje více než jen implementace efektivního kódu. Prostředí uživatele s výkonem aplikace musíte také zvážit. Třeba zajistit, že operace spuštění bez blokování uživatele od provádění dalších aktivit může pomoct vylepšit uživatelské prostředí.

Existuje několik metod pro zvýšení výkonu a dosahovaný výkon aplikace Xamarin.Forms. Mezi ně patří:

- [Povolit kompilátor XAML](#xamlc)
- [Výběr správné rozložení](#correctlayout)
- [Povolení komprese rozložení](#layoutcompression)
- [Použít rychlé Renderery](#fastrenderers)
- [Snížit nepotřebné vazby](#databinding)
- [Optimalizace výkonu rozložení](#optimizelayout)
- [Optimalizace výkonu ListView](#optimizelistview)
- [Optimalizace prostředků obrázků](#optimizeimages)
- [Zmenšit velikost vizuálního stromu.](#visualtree)
- [Zmenšit velikost slovníku prostředků aplikace](#resourcedictionary)
- [Použití vzoru vlastního Rendereru](#rendererpattern)

> [!NOTE]
>  Před čtením tohoto článku byste si měli nejdřív přečíst [Cross-Platform výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md), který popisuje konkrétní postupy jiných platforem zlepšit využití paměti a výkonu aplikace založené na platformě Xamarin.

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>Povolit kompilátor XAML

Přímo do jazyka intermediate language (IL) s kompilátorem XAML (XAMLC) můžete volitelně zkompilovat XAML. XAMLC nabízí celou řadu výhody:

- Provádí kontrolu za kompilace XAML upozornění uživatele nějaké chyby.
- Odebere určitá doba načtení a vytvoření instance pro elementy XAML.
- To pomáhá snížit velikost souboru v konečném sestavení už zahrnutím soubory .xaml.

XAMLC je zakázané ve výchozím nastavení k zajištění zpětné kompatibility. Nicméně je možné povolit na úrovni třídy i sestavení. Další informace najdete v tématu [kompilace XAML](~/xamarin-forms/xaml/xamlc.md).

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>Výběr správné rozložení

Rozložení, která je schopná zobrazit více podřízených položek, ale který má jenom jednu podřízenou je plýtváním. Například následující příklad kódu ukazuje [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) s jednu podřízenou:

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

Toto je plýtvání a [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) element by měl odebrat, jak je znázorněno v následujícím příkladu kódu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

Kromě toho Nepokoušejte se reprodukovat vzhled specifické rozložení pomocí kombinace jiné rozložení, jako tento výsledky ve výpočtech zbytečné rozložení právě probíhá. Například, nepokoušejte se reprodukovat [ `Grid` ](xref:Xamarin.Forms.Grid) rozložení pomocí kombinace [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) instancí. Následující příklad kódu ukazuje příklad tohoto postupu chybný:

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

To je plýtvání, protože se provádí výpočty zbytečné rozložení. Místo toho na požadované rozložení můžete lépe dosáhnout pomocí [ `Grid` ](xref:Xamarin.Forms.Grid), jak je znázorněno v následujícím příkladu kódu:

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

## <a name="enable-layout-compression"></a>Povolení komprese rozložení

Komprese rozložení odebere zadané rozložení z vizuálního stromu, za účelem zvýšení výkonu vykreslování stránky. Zlepšuje výkon, který to poskytuje se liší v závislosti na složitosti stránku, verze operačního systému se používají a zařízení, na kterém je aplikace spuštěna. Největší zvýšení výkonu se však projeví na starší zařízení. Další informace najdete v tématu [komprese rozložení](~/xamarin-forms/user-interface/layouts/layout-compression.md).

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>Použít rychlé Renderery

Rychlé renderery snížit inflaci a náklady na vykreslení ovládacích prvků Xamarin.Forms v Androidu linearizovat hierarchii výsledný nativní ovládací prvek. To dále zlepšuje výkon tím, že vytvoříte méně objektů, která v výsledky v méně složitých vizuální strom a nižší využití paměti. Další informace najdete v tématu [rychlé Renderery](~/xamarin-forms/internals/fast-renderers.md).

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>Snížit nepotřebné vazby

Nepoužívejte vazby pro obsah, který je možné snadno nastavit staticky. Neexistuje žádná výhoda ve vazbě dat, která nemusí být vázán, protože vazby nejsou nákladově efektivnější. Například nastavení `Button.Text = "Accept"` má nižší režijní náklady než vazby [ `Button.Text` ](xref:Xamarin.Forms.Button.Text) k ViewModel `string` vlastnost s hodnotou "Přijmout".

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>Optimalizace výkonu rozložení

Xamarin.Forms 2 zavedl modul optimalizované rozložení, který má vliv na aktualizaci rozložení. Pokud chcete získat nejlepší výkon možné rozložení, postupujte podle následujících pokynů:

- Snižte hloubku hierarchie rozložení tak, že zadáte [ `Margin` ](xref:Xamarin.Forms.View.Margin) hodnoty vlastností, umožní vytvoření rozložení s menším počtem zabalení zobrazení. Další informace najdete v tématu [okraje a výplň](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- Při použití [ `Grid` ](xref:Xamarin.Forms.Grid), pokuste se zajistit, že počet řádků a sloupců nejvíce nastavené na [ `Auto` ](xref:Xamarin.Forms.GridLength.Auto) velikost. Každý Automatická velikost řádku nebo sloupce způsobí, že modul rozložení provádět výpočty další rozložení. Místo toho použijte pevné velikosti řádků a sloupců, pokud je to možné. Můžete také nastavit řádků a sloupců, aby obsadily poměrné množství prostoru se [ `GridUnitType.Star` ](xref:Xamarin.Forms.GridUnitType.Star) hodnota výčtu, k dispozici, že stromu nadřazené následuje tyto pokyny rozložení.
- Nemají nastavený [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) a [ `HorizontalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti rozložení Pokud nevyžaduje. Výchozí hodnoty [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) a [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) povolit osvědčené optimalizace rozložení. Změna těchto vlastností má svou cenu a spotřebovává paměť, i v případě nastavení na výchozí hodnoty.
- Vyhněte se použití [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) kdykoli je to možné. Bude výsledkem by bylo nutné provést mnohem více práce procesoru.
- Při použití [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), vyhněte se použití [ `AbsoluteLayout.AutoSize` ](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) vlastnost kdykoli je to možné.
- Při použití [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), ujistěte se, že pouze jeden podřízený prvek je nastavena na [ `LayoutOptions.Expands` ](xref:Xamarin.Forms.LayoutOptions.Expands). Tato vlastnost se zajistí, že budou zaměstnávat zadanou podřízenou položku. největší prostor, který `StackLayout` můžete přidělit a je plýtvání k provedení těchto výpočtů více než jednou.
- Nemůžete volat metody [ `Layout` ](xref:Xamarin.Forms.Layout) třídy, protože se provádí výpočty náročné rozložení. Místo toho je pravděpodobné, aby bylo možné získat požadované rozložení chování tak, že nastavíte [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) vlastnosti. Alternativně podtřídy [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) třídy k dosažení požadované rozložení chování.
- Nechcete aktualizovat některé [ `Label` ](xref:Xamarin.Forms.Label) instance častěji, než se požaduje, jak změnit velikost popisku může vést k rozložení celé obrazovky se přepočítávají.
- Nemají nastavený [ `Label.VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) vlastnost Pokud nevyžaduje.
- Nastavte [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) žádné [ `Label` ](xref:Xamarin.Forms.Label) instance na [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap) kdykoli je to možné.

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>Optimalizace výkonu ListView

Při použití [ `ListView` ](xref:Xamarin.Forms.ListView) několik položek uživatelským prostředím, které by mělo být optimalizované ovládací prvek:

- **Inicializace** – časový interval spuštění při vytvoření ovládacího prvku a končí, když položky se zobrazí na obrazovce.
- **Posouvání** – možnost procházet seznam a ujistěte se, že uživatelské rozhraní nepodporuje zaostávat za touch gesta.
- **Interakce** pro přidávání, odstraňování a výběr položek.

[ `ListView` ](xref:Xamarin.Forms.ListView) Ovládací prvek vyžaduje, aby aplikace k poskytnutí dat a buňky šablony. Jak toho dosáhnout, bude mít velký dopad na výkon ovládacího prvku. Další informace najdete v tématu [ListView výkonu](~/xamarin-forms/user-interface/listview/performance.md).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimalizace prostředků obrázků

Zobrazení prostředků obrázků, může výrazně zlepšit nároky na paměť aplikace. Proto jsou by měl pouze vytvořit při vyžaduje a by měly být vydány ihned poté, co aplikace již nevyžaduje. Například pokud aplikace zobrazuje obrázek načtením dat z datového proudu, ujistěte se, že tohoto datového proudu se vytvoří pouze v případě, že je to požadováno a ujistěte se, že datový proud uvolní se, když už nebude potřeba. Toho lze dosáhnout vytvořením datového proudu při vytvoření stránky, nebo když [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) dojde k aktivaci události a pak odstraňování datového proudu při [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) dojde k aktivaci události.

Při stahování obrázku pro zobrazení s [ `ImageSource.FromUri` ](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) metoda mezipaměti stažený obraz tak zajistit, aby [ `UriImageSource.CachingEnabled` ](xref:Xamarin.Forms.UriImageSource.CachingEnabled) je nastavena na `true`. Další informace najdete v tématu [práce s obrázky](~/xamarin-forms/user-interface/images.md).

Další informace najdete v tématu [optimalizovat prostředky obrázků](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>Zmenšit velikost vizuálního stromu.

Snížení počtu prvků na stránce způsobí, že na stránce rychlejšímu vykreslování. Pro dosažení tohoto cíle dvěma způsoby. První je skrýt prvky, které nejsou viditelné. [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) Vlastnost každého prvku určuje, zda element by měla být součástí vizuálního stromu, nebo ne. Proto pokud element není zobrazen, protože je skrytá za další prvky, buď odeberte element nebo nastavte jeho `IsVisible` vlastnost `false`.

Odebrání nepotřebných prvků jako druhý postup se používá. Například následující příklad kódu ukazuje rozložení stránky, které se zobrazuje řada z [ `Label` ](xref:Xamarin.Forms.Label) prvky:

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

Stejné rozložení stránky může být udržována s počtem sníženou prvků, jak je znázorněno v následujícím příkladu kódu:

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

## <a name="reduce-the-application-resource-dictionary-size"></a>Zmenšit velikost slovníku prostředků aplikace

Všechny prostředky, které se používají v celé aplikaci by měla být uložena ve slovníku prostředků aplikace, aby se zabránilo duplicitě. To vám pomůže snížit objem XAML, který je analyzovat v aplikaci. Následující příklad kódu ukazuje `HeadingLabelStyle` prostředků, což je používaná aplikace široké a proto je definován ve slovníku prostředků aplikace:

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

Nicméně, XAML, který je specifický pro stránku by neměl být zařazen slovník prostředků aplikace, jako prostředky se poté analyzovat při spuštění aplikace místo v případě potřeby stránkou. Pokud prostředek používá stránku, která není spouštěcí stránky, se má umístit ve slovníku prostředků pro tuto stránku, proto pomáhá snížit XAML, který je analyzovat při spuštění aplikace. Následující příklad kódu ukazuje `HeadingLabelStyle` prostředek, který je pouze na jednu stránku a proto je definován ve slovníku prostředků na stránce:

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

Další informace o prostředcích aplikací, najdete v části [ `Working with Styles` ](~/xamarin-forms/user-interface/styles/index.md).

<a name="rendererpattern" />

## <a name="use-the-custom-renderer-pattern"></a>Použití vzoru vlastního Rendereru

Většina renderer třídy vystavení `OnElementChanged` metodu, která je volána při vytvoření vlastního ovládacího prvku Xamarin.Forms pro vykreslení odpovídající nativní ovládací prvek. Vlastní zobrazovací jednotky tříd v každé třídě renderer specifické pro platformu potom přepsáním této metody můžete vytvořit instanci a upravte nativní ovládací prvky. `SetNativeControl` Metoda se používá k vytvoření instance nativní ovládací prvek, a to tato metoda také odkaz na ovládací `Control` vlastnost.

Nicméně v některých případech `OnElementChanged` metodu lze volat více než jednou. Proto aby se zabránilo nevracení paměti, což může mít dopad na výkon, musí dbát při vytváření instance nový nativní ovládací prvek. Přístup k použití při vytváření instance nový nativní ovládací prvek ve vlastní zobrazovací jednotky můžete vidět v následujícím příkladu kódu:

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

Nový nativní ovládací prvek by měl vytvořit jenom jednou, když `Control` vlastnost `null`. Ovládací prvek by měl být nakonfigurovaný jenom a když vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms přihlásit k odběru obslužných rutin událostí. Podobně všechny obslužné rutiny, které byly k odběru pouze by Odhlášený při vykreslování element je připojen k změny. Přijmout tento přístup vám pomůže vytvořit efektivní provádění vlastního rendereru, který není trpí nevracení paměti.

Další informace o vlastní renderery, naleznete v tématu [přizpůsobení ovládacích prvků na každé platformě](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="summary"></a>Souhrn

Tento článek popisuje a popsané techniky pro zvýšení výkonu aplikací s Xamarin.Forms. Společně tyto postupy mohou výrazně snížit množství práce prováděné procesoru a paměti spotřebované aplikací.


## <a name="related-links"></a>Související odkazy

- [Výkon napříč platformami](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Výkon ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [Rychlé renderery](~/xamarin-forms/internals/fast-renderers.md)
- [Komprese rozložení](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Ukázka změnu velikosti obrázku Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/XamFormsImageResize/)
- [XamlCompilation](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [XamlCompilationOptions](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
