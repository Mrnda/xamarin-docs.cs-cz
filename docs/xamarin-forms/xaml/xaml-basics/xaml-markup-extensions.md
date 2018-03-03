---
title: "Část 3. XAML – rozšíření značek"
description: "XAML – rozšíření značek tvoří důležitou součást v jazyce XAML, které umožní vlastnosti, které chcete nastavit na objekty nebo hodnoty, které jsou nepřímo odkazované z jiných zdrojů. XAML – rozšíření značek jsou obzvláště důležité pro sdílení objekty a odkazování na konstanty používají v rámci aplikace, ale v datových vazeb najdou jejich největší nástroj."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 7aea7b1536efc952378c6a1df63640af191f1ebe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="part-3-xaml-markup-extensions"></a>Část 3. XAML – rozšíření značek

_XAML – rozšíření značek tvoří důležitou součást v jazyce XAML, které umožní vlastnosti, které chcete nastavit na objekty nebo hodnoty, které jsou nepřímo odkazované z jiných zdrojů. XAML – rozšíření značek jsou obzvláště důležité pro sdílení objekty a odkazování na konstanty používají v rámci aplikace, ale v datových vazeb najdou jejich největší nástroj._

## <a name="xaml-markup-extensions"></a>XAML – rozšíření značek

Obecně platí jazyka XAML použijete k nastavení vlastnosti objektu explicitní hodnoty jako řetězec, je číslo, člena výčtu nebo řetězec, který je převést na hodnotu na pozadí.

V některých případech však vlastnosti musí odkazovat místo hodnoty definované někde jinde, nebo může vyžadující málo zpracování kódem za běhu. Pro tyto účely, XAML *rozšíření značek* jsou k dispozici.

Tato rozšíření značek XAML nejsou rozšíření XML. XAML je zcela právní XML. Nazývají "rozšíření", protože se opírají o kód v třídy, které implementují `IMarkupExtension`. Můžete napsat vlastní rozšíření vlastních značek.

V mnoha případech jsou okamžitě rozpoznatelném v souborech XAML rozšíření značek v jazyce XAML, protože se objeví jako nastavení atributů oddělená složené závorky: {a}, ale někdy se rozšíření značek v kódu jako konvenční elementy.

## <a name="shared-resources"></a>Sdílené prostředky

Některé stránky XAML obsahovat několik zobrazení s nastaveny na stejné hodnoty vlastnosti. Například mnoho nastavení vlastností pro tyto `Button` objekty jsou stejné:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="Large" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="Large" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="Large" />

    </StackLayout>
</ContentPage>
```

Pokud jeden z těchto vlastností je třeba změnit, budete pravděpodobně chtít provést změnu pouze jednou, nikoli třikrát. Pokud to byly kódu, budete pravděpodobně používat konstanty a statické objekty jen pro čtení zajistit, aby byl tyto hodnoty, konzistentní a snadno upravit.

V jazyce XAML, jedním z oblíbených řešení je k uložení těchto hodnot nebo objekty v *slovník prostředků*. `VisualElement` Třída definuje vlastnost s názvem `Resources` typu `ResourceDictionary`, což je slovník s klíči typu `string` a hodnoty typu `object`. Můžete převést objekty do tohoto slovníku a potom je odkazovat z značek, všechny v jazyce XAML.

Pokud chcete používat slovník prostředků, a na stránce, patří pár `Resources` značky element vlastnosti. Je nejvhodnější pro umístění těchto v horní části stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

Je také nutné explicitně zahrnout `ResourceDictionary` značky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Nyní objekty a hodnoty různých typů lze přidat do slovníku prostředků. Tyto typy musí být instantiable. Například nemohou být abstraktní třídy. Tyto typy musí také mít konstruktor public bez parametrů. Každá položka vyžaduje slovník klíč zadaný `x:Key` atribut. Příklad:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Těchto dvou položek jsou hodnoty strukturu typu `LayoutOptions`a každá z nich má jedinečný klíč a jedno nebo dvě vlastnosti nastavit. V kódu a kódu, je mnohem víc společné pro používání statických polí z `LayoutOptions`, ale tady je pohodlnější a nastavte vlastnosti.

Nyní je nutné nastavit `HorizontalOptions` a `VerticalOptions` vlastnosti z těchto tlačítek na tyto prostředky, a které provádí pomocí `StaticResource` XAML – rozšíření značek:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="Large" />
```

`StaticResource` – Rozšíření značek jsou vždy odděleny složené závorky a obsahuje klíče slovníku.

Název `StaticResource` odlišuje jej od `DynamicResource`, který podporuje i Xamarin.Forms. `DynamicResource` pro klíče slovníku přidružené hodnoty, které mohou změnit za běhu, zatímco `StaticResource` přistupuje elementy ze slovníku pouze jednou, když se vytvářejí elementy na stránce.

Pro `BorderWidth` vlastnost, je nutné uložit dvojitou ve slovníku. XAML pohodlně definuje značky pro běžné typy dat jako `x:Double` a `x:Int32`:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Není nutné pro něj na tři řádky. Tato položka slovník pro tuto úhel otočení zabírají pouze jeden řádek:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Tyto dva prostředky, může být odkazován stejným způsobem jako `LayoutOptions` hodnoty:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="Large" />
```

Pro prostředky typu `Color`, můžete použít stejné řetězcové vyjádření, které používáte při přiřazování přímo atributy z těchto typů. Převaděče typů jsou volána, když je vytvořen prostředek. Zde je prostředek typu `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

`FontSize` Vlastnost představuje menší problém. Vlastnost je definována jako typu `double`. Když nastavíte vlastnost členem `NamedSize` výčtu, jako `Large`, `FontSizeConverter` třídy funguje na pozadí a převeďte ho na hodnotu závislé na platformě pomocí `Device.GetNamedSized` metoda.

Ale nelze definovat prostředku pro velikost písma jako `double` a nastavit hodnotu "Velké". V době, zda analyzátor XAML zpracovává prostředku neví, že hodnota se použije jako velikost písma. 

Řešení je k definování prostředků jako `string` pomocí `x:String` typu:

```xaml
<x:String x:Key="fontSize">Large</x:String>
```

Nyní všechny vlastnosti s výjimkou `Text` jsou definovány nastavení prostředků:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Je také možné použít `OnPlatform` v rámci slovníku prostředků můžete definovat různé hodnoty pro platformy. Tady je způsob `OnPlatform` objekt může být součástí slovníku prostředků pro jiné text barvy:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Všimněte si, že `OnPlatform` získá i `x:Key` atributů, protože je objekt ve slovníku a `x:TypeArguments` atributů, protože je obecné třídy. `iOS`, `Android`, A `UWP` atributy se převedou na `Color` hodnoty při inicializaci objektu.

Tady je poslední dokončení souboru XAML s tři tlačítka přístup k šesti sdílené hodnoty:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:String x:Key="fontSize">Large</x:String>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize"{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

Na snímcích obrazovky ověřte konzistentní stylů a stylu závislé na platformě:

[ ![](xaml-markup-extensions-images/sharedresources.png "Ovládací prvky ve")](xaml-markup-extensions-images/sharedresources-large.png "ve ovládací prvky")

I když se nejčastěji můžete definovat `Resources` kolekce v horní části stránky, mějte na paměti, `Resources` je definována vlastnost `VisualElement`, a může mít `Resources` kolekce na další prvky na stránce. Zkuste například přidávání jeden, který `StackLayout` v tomto příkladu:

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

Dozvíte se, barva textu tlačítka je nyní blue. Vždy, když analyzátor XAML v podstatě, dojde `StaticResource` – rozšíření značek, ho vyhledá nahoru k visual a použije první `ResourceDictionary` zjistí obsahující klíči.

Jednou z nejběžnějších typů objekty uložené ve slovnících prostředků je platformě Xamarin.Forms `Style`, která definuje kolekce vlastností nastavení. Styly jsou popsané v článku [styly](~/xamarin-forms/user-interface/styles/index.md).

Někdy vývojáři nové XAML zajímat, pokud se například umístit vizuální prvek `Label` nebo `Button` v `ResourceDictionary`. I když je surely možné, nemá mnoho smysl. Účelem `ResourceDictionary` je sdílet objekty. Vizuální prvek nelze sdílet. Stejnou instanci nelze vložit dvakrát na jedné stránce.

## <a name="the-xstatic-markup-extension"></a>X: Static – rozšíření značek

Bez ohledu podobnosti jejich názvy `x:Static` a `StaticResource` se příliš neliší. `StaticResource` Vrátí objekt ze slovníku prostředků při `x:Static` používá jednu z následujících:

- Veřejné statické pole
- Veřejné statické vlastnosti
- veřejné konstantní pole 
- člena výčtu. 

`StaticResource` – Rozšíření značek podporuje XAML implementace, které definují slovník prostředků při `x:Static` je vnitřní součástí XAML, jako `x` předpony zjistí informace o.

Tady je několik příkladů, které ukazují, jak `x:Static` explicitně odkazovat statická pole a členové výčtu:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Pokud to není velmi působivé. Ale `x:Static` – rozšíření značek lze také odkazovat statických polí nebo vlastnosti z vlastní kód. Zde je ukázka, `AppConstants` třídu, která obsahuje některá statické pole, které chcete použít na více stránkách v celé aplikaci:

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;
                    
                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

Chcete-li statických polí této třídy v souboru XAML, budete potřebovat některé, jak v souboru XAML, kde se nachází tento soubor. Můžete to udělat pomocí deklarace oboru názvů XML.

Odvolat, aby soubory XAML vytvořené jako součást standardní šablona Xamarin.Forms XAML obsahují dva deklarace oboru názvů XML: jeden pro přístup k Xamarin.Forms třídy a druhý pro odkazování na značky a atributy vnitřní do jazyka XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Budete potřebovat další deklarace oboru názvů XML pro přístup k jiné třídy. Každý další deklaraci oboru názvů XML definuje novou předponu. Pro přístup k třídy místní sdílené aplikaci PCL, jako například `AppConstants`, programátory v jazyce XAML často používají předponu `local`. Deklarace oboru názvů musí označovat název oboru názvů CLR (Common Language Runtime), také známé jako název oboru názvů .NET, což je název, který se zobrazí v jazyce C# `namespace` definice nebo v `using` – direktiva:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Deklarace oborů názvů XML pro obory názvů .NET můžete také definovat v jakékoli sestavení, která odkazuje PCL. Zde je ukázka, `sys` předponu pro standardní .NET `System` obor názvů, který je v **mscorlib** sestavení, které jednou v platnosti pro "Knihovna Runtime běžné objekt Microsoft", ale teď znamená "překlady standardní Běžné objektu Runtime knihovny." Vzhledem k tomu, že je sestavení, musíte také zadáte název sestavení, v takovém případě **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Všimněte si, že klíčové slovo `clr-namespace` je následovaný dvojtečkou a potom název oboru názvů .NET, za nímž následuje středníkem, klíčové slovo `assembly`, znaku rovná a název sestavení.

Ano, dvojtečkou následuje `clr-namespace` ale následuje rovná `assembly`. Syntaxe byla definována v tomto způsobem úmyslně: deklarace oboru názvů XML nejvíce odkazovat identifikátor URI, který začíná název schématu identifikátoru URI, jako `http`, který je vždy následovaným dvojtečkou. `clr-namespace` Součástí tento řetězec je určený tak, aby napodoboval této konvence.

Obě tyto deklarace oboru názvů jsou součástí **StaticConstantsPage** ukázka. Všimněte si, že `BoxView` dimenze jsou nastaveny na `Math.PI` a `Math.E`, ale škálovat faktorem 100:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

Velikost výsledné `BoxView` relativně k obrazovce je závislá na platformě:

 [ ![](xaml-markup-extensions-images/staticconstants.png "Ovládacích prvků pomocí x: Static – rozšíření značek")](xaml-markup-extensions-images/staticconstants-large.png "ovládacích prvků pomocí x: Static – rozšíření značek")

## <a name="other-standard-markup-extensions"></a>Další rozšíření standardní značek

Několik rozšíření značek jsou vlastní XAML a podporované v souborech Xamarin.Forms XAML. Některé z těchto nepoužívají velmi často, ale jsou důležité pro potřeby:

-  Pokud má vlastnost jinou hodnotu než `null` hodnotu ve výchozím nastavení, ale chcete ho nastavit na `null`, nastavte ji na `{x:Null}` – rozšíření značek.
-  Pokud je vlastnost typu `Type`, můžete je přiřadit `Type` pomocí rozšíření značek `{x:Type someClass}`.
-  Můžete definovat pole v jazyce XAML pomocí `x:Array` – rozšíření značek. Toto rozšíření značek má požadovaný atribut s názvem `Type` určující typ elementů v poli.
- `Binding` – Rozšíření značek je popsána v [část 4. Datové vazby Základy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>Rozšíření značek ConstraintExpression

Rozšíření značek může mít vlastnosti, ale nejsou nastaveny jako atributy XML. V rozšíření značek nastavení vlastností se oddělují čárkami a žádné uvozovky zobrazí do složených závorek.

To lze ukázat pomocí rozšíření značek Xamarin.Forms s názvem `ConstraintExpression`, který se používá s `RelativeLayout` třídy. Umístění nebo velikost podřízené zobrazení můžete zadat jako konstanta, nebo relativně k nadřazené nebo jiné s názvem zobrazení. Syntaxe `ConstraintExpression` umožňuje nastavit pozici nebo velikost zobrazení pomocí `Factor` časy vlastnost jiné zobrazení, plus `Constant`. Nic složitější než vyžaduje kód.

Tady je příklad:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

Možná je nejdůležitější lekce byste měli vzít od této ukázky syntaxe rozšíření značek: žádné uvozovky musí být do složených závorek rozšíření značek. Při psaní rozšíření značek v souboru XAML, je přirozené chcete uvést hodnoty vlastností v uvozovkách. Odolejte riziko!

Tady je spuštěn program:

[ ![](xaml-markup-extensions-images/relativelayout.png "Relativní rozložení pomocí omezení")](xaml-markup-extensions-images/relativelayout-large.png "relativní rozložení pomocí omezení")

## <a name="summary"></a>Souhrn

Rozšíření značek XAML tady uvedené podporují důležité soubory XAML. Ale možná se nejvíc hodí v situaci rozšíření značek XAML `Binding`, která je popsaná v další části této série [část 4. Datové vazby Základy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Část 1. Začínáme s jazykem XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Část 2. Základní syntaxe jazyka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Část 4. Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Část 5. Z datové vazby k rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
