---
title: Část 3. Rozšíření značek XAML
description: Rozšíření značek XAML tvoří důležitou funkcí v XAML, díky kterým můžou vlastnosti nastavit na objekty nebo hodnoty, které jsou nepřímo odkazovány z jiných zdrojů.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: 6fcb051d2c24c7da169106b06ad5ebfc91edafa6
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935612"
---
# <a name="part-3-xaml-markup-extensions"></a>Část 3. Rozšíření značek XAML

_Rozšíření značek XAML tvoří důležitou funkcí v XAML, díky kterým můžou vlastnosti nastavit na objekty nebo hodnoty, které jsou nepřímo odkazovány z jiných zdrojů. Rozšíření značek XAML jsou obzvláště důležité pro sdílení obsahu objektů a odkazování na ně konstanty používané v celé aplikaci, ale naleznou. jejich největší nástroj v datové vazby._

## <a name="xaml-markup-extensions"></a>Rozšíření značek XAML

Obecně platí pomocí XAML můžete nastavit vlastnosti objektu na explicitní hodnoty, jako je řetězec, číslo, na člena výčtu nebo řetězec, který je převedena na hodnotu na pozadí.

V některých případech však vlastnosti musí odkazovat místo hodnoty definované někde jinde, nebo které mohou vyžadovat trochu zpracování kódu za běhu. Pro tyto účely, XAML *– rozšíření značek* jsou k dispozici.

Tato rozšíření značek XAML nejsou rozšíření XML. XAML je zcela právní XML. Jsou volány "rozšíření", protože se zálohují ve třídách, které implementují kód `IMarkupExtension`. Můžete napsat vlastní rozšíření vlastních značek.

V mnoha případech jsou rozšíření značek XAML pozná okamžitě v souborech XAML, protože se zobrazují jako nastavení atributů oddělených ve složených závorkách: {a}, ale někdy – rozšíření značek se zobrazí v kódu jako konvenční elementy.

## <a name="shared-resources"></a>Sdílené prostředky

Některé stránky XAML obsahuje několik zobrazení s vlastnosti nastavené stejné hodnoty. Například mnoho nastavení vlastností pro tyto `Button` jsou objekty stejné:

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
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

Pokud některou z těchto vlastností je třeba změnit, budete pravděpodobně chtít provést změny pouze jednou, spíše než třikrát. Pokud to bylo kódu, budete pravděpodobně používat konstanty a statické objekty jen pro čtení k udržení tyto hodnoty, konzistentní a snadno upravit.

V XAML, jedním z oblíbených řešení je uložit tyto hodnoty nebo objekty v *slovník prostředků*. `VisualElement` Třída definuje vlastnost s názvem `Resources` typu `ResourceDictionary`, což je slovník pomocí klíče typu `string` a hodnoty typu `object`. Můžete převést objekty do tohoto slovníku a odkázat na ně ze značek, vše v XAML.

Použití slovníku prostředků na stránce, patří pár `Resources` značky element vlastnosti. Je nejvhodnější pro vložit v horní části stránky:

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

Je také nutné explicitně zahrnout symbol `ResourceDictionary` značky:

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

Nyní objekty a hodnoty různých typů, je přidat do slovníku prostředků. Tyto typy musí být instantiable. Například nemohou být abstraktní třídy. Tyto typy musí také mít veřejný konstruktor bez parametrů. Každá položka vyžaduje klíč slovníku zadaný `x:Key` atribut. Příklad:

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

Tyto dvě položky jsou hodnoty typu Struktura `LayoutOptions`a každá má jedinečný klíč a jeden nebo dva nastaveny. V kódu a kódu, je mnohem častější použít statické pole `LayoutOptions`, ale tady je pohodlnější k nastavení vlastností.

Nyní je nutné nastavit `HorizontalOptions` a `VerticalOptions` vlastnosti těchto tlačítek k těmto prostředkům, a, který se použije `StaticResource` – rozšíření značek XAML:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` – Rozšíření značek jsou vždy odděleny složených závorek a obsahuje klíč slovníku.

Název `StaticResource` odlišuje jej od `DynamicResource`, která také podporuje Xamarin.Forms. `DynamicResource` je pro klíče slovníku přidružené hodnoty, které mohou změnit za běhu, zatímco `StaticResource` přistupuje k elementy ze slovníku pouze jednou, když jsou vytvořeny elementy na stránce.

Pro `BorderWidth` vlastnost, je nezbytné k uložení typu double ve slovníku. XAML pohodlně definuje značky pro běžné typy dat, jako je `x:Double` a `x:Int32`:

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

Není nutné ho umístit na tři řádky. Tato položka slovník pro tuto úhel otočení přijímá pouze jeden řádek nahoru:

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

Tyto dva prostředky mohou odkazovat stejným způsobem jako `LayoutOptions` hodnoty:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Pro prostředky typu `Color`, můžete použít stejné řetězcových reprezentací, které používáte při přiřazování přímo atributy z těchto typů. Převaděče typů jsou vyvolány, když je prostředek vytvořený. Zde je prostředek typu `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

Často, programy sady `FontSize` vlastnost člena `NamedSize` výčet jako `Large`. `FontSizeConverter` Funguje na pozadí ho převést na hodnotu závislého na platformě pomocí třídy `Device.GetNamedSized` metody. Ale při definování prostředků velikost písma, je vhodnější použít číselnou hodnotu, zobrazí jako zde `x:Double` typu:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Nyní všechny vlastnosti s výjimkou `Text` se definuje na základě nastavení prostředků:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Je také možné použít `OnPlatform` v rámci slovníku prostředků, chcete-li definovat různé hodnoty pro platformy. Tady je způsob `OnPlatform` objekt může být součástí slovníku prostředků pro různé text barvy:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Všimněte si, že `OnPlatform` získá obě `x:Key` atribut, protože jde o objekt ve slovníku a `x:TypeArguments` atribut, protože se jedná o obecnou třídu. `iOS`, `Android`, A `UWP` atributy jsou převedeny na `Color` hodnoty při inicializaci objektu.

Zde je poslední úplný soubor XAML s tři tlačítka přístup k šest sdílené hodnoty:

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
                FontSize="{StaticResource fontSize}" />

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

Snímky obrazovky ověřte konzistentní styly a styly závislého na platformě:

[![](xaml-markup-extensions-images/sharedresources.png "Ovládacích prvků")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "ovládacích prvků")

I když je pro definování nejběžnějších `Resources` kolekce v horní části stránky, mějte na paměti, který `Resources` je definována vlastnost `VisualElement`, a může mít `Resources` kolekce v jiných elementy na stránce. Zkuste například přidáme do `StackLayout` v tomto příkladu:

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

Dozvíte se, už modrou barvu textu tlačítka. Vždy, když analyzátor XAML v podstatě narazí `StaticResource` – rozšíření značek, hledá se vizuální strom a použije první `ResourceDictionary` narazí, který obsahuje daný klíč.

Jedním z nejběžnějších typů objekty uložené ve slovnících prostředků je Xamarin.Forms `Style`, která definuje sadu nastavení vlastností. Styly jsou popsány v následujícím článku [styly](~/xamarin-forms/user-interface/styles/index.md).

Někdy vývojářům nové XAML zajímat, pokud jsou vizuální prvek vložit jako `Label` nebo `Button` v `ResourceDictionary`. I když je možné seděl, to moc nedává smysl. Účelem `ResourceDictionary` je sdílet objekty. Prvek visual nelze sdílet. Stejnou instanci se nemůže objevit dvakrát na jednu stránku.

## <a name="the-xstatic-markup-extension"></a>X: Static – rozšíření značek

Bez ohledu na podobnosti jejich názvy `x:Static` a `StaticResource` se velmi liší. `StaticResource` Vrátí objekt ze slovníku prostředků při `x:Static` přistupuje k jedné z následujících akcí:

- Veřejné statické pole
- veřejná statická vlastnost
- veřejné konstanty pole
- na člena výčtu.

`StaticResource` Podporuje – rozšíření značek XAML implementace, které definují slovníku prostředků, zatímco `x:Static` je vnitřní součástí XAML, jako `x` předpony zjistí informace o.

Tady je pár příkladů, které ukazují, jak `x:Static` můžete explicitně odkazovat na statická pole a členy výčtu:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

Toto zatím není velmi působivé. Ale `x:Static` – rozšíření značek lze také odkazovat na statické pole nebo vlastnosti z vlastního kódu. Tady je příklad, `AppConstants` třídu, která obsahuje některé statická pole, které můžete chtít použít na více stránkách v celé aplikaci:

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

Odkaz na statické pole této třídy v souboru XAML, budete potřebovat způsob, jak určit v souboru XAML, ve kterém se tento soubor nachází. Můžete to provést pomocí deklarace oboru názvů XML.

Připomínáme, že vytvořené jako součást standardní šablonu Xamarin.Forms XAML soubory XAML obsahovat dvě deklarace oboru názvů XML: jeden pro přístup k třídy Xamarin.Forms a druhou pro odkazování na tagy a atributy, které jsou přirozené pro XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Budete potřebovat další deklarace oboru názvů XML pro přístup k jiné třídy. Každý další deklarace oboru názvů XML definuje novou předponu. Pro přístup k třídy místní do knihovny .NET Standard sdílené aplikace, jako například `AppConstants`, programátoři XAML často používají předponu `local`. Deklarace oboru názvů musíte uvést název oboru názvů CLR (Common Language Runtime), označované také jako název oboru názvů .NET, což je název, který se zobrazí v jazyce C# `namespace` definice nebo `using` – direktiva:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Deklarace oboru názvů XML pro obory názvů .NET můžete také definovat žádné sestavení, na které odkazuje na knihovny .NET Standard. Tady je příklad, `sys` předpona pro standard .NET `System` obor názvů, který se nachází v **mscorlib** sestavení, které jednou jednoduchému "Knihovny Runtime běžné objekt Microsoft", ale teď znamená "překlady Standard Běžné objekt Runtime Library". Protože je to jiné sestavení, musíte zadat také název sestavení, v tomto případě **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Všimněte si, že klíčové slovo `clr-namespace` následovaný dvojtečkou a potom název oboru názvů .NET, za nímž následuje středníkem, klíčové slovo `assembly`, znaménko rovná se a název sestavení.

Ano, následuje dvojtečka `clr-namespace` ale následuje symbol rovná `assembly`. Syntaxe byla definována v tomto způsobem záměrně: deklarace oboru názvů XML nejvíce odkazovat na identifikátor URI, který začíná název schématu identifikátoru URI jako `http`, což je vždy následuje dvojtečka. `clr-namespace` Část tohoto řetězce je určena k napodobení úmluvy.

Obě tyto deklarace oboru názvů jsou součástí **StaticConstantsPage** vzorku. Všimněte si, že `BoxView` dimenze jsou nastaveny na `Math.PI` a `Math.E`, ale škálován faktorem 100:

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

Velikost výsledné `BoxView` vzhledem k obrazovce závisí na platformě:

 [![](xaml-markup-extensions-images/staticconstants.png "Ovládací prvky pomocí x: Static – rozšíření značek")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "ovládacích prvků pomocí x: Static – rozšíření značek")

## <a name="other-standard-markup-extensions"></a>Další rozšíření pro standardní značky

Několik rozšíření značek jsou přirozené pro XAML a podporované v souborech XAML Xamarin.Forms. Některé z nich nepoužívají velmi často, ale jsou nezbytné, když je budete potřebovat:

-  Pokud je vlastnost non - `null` má hodnotu ve výchozím nastavení ale můžete ji nastavit na hodnotu `null`, nastavte ho na `{x:Null}` – rozšíření značek.
-  Pokud je vlastnost typu `Type`, můžete je přiřadit `Type` pomocí rozšíření značek `{x:Type someClass}`.
-  Můžete definovat pole pomocí XAML `x:Array` – rozšíření značek. Toto rozšíření značek nemá požadovaný atribut s názvem `Type` , který určuje typ prvků v poli.
- `Binding` – Rozšíření značek je podrobněji popsána [část 4. Datové vazby Základy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>Rozšíření značek ConstraintExpression

Rozšíření značek může mít vlastnosti, ale nejsou nastaveny jako atributy ve formátu XML. V rozšíření značek nastavení vlastností jsou odděleny čárkami a žádné uvozovky uvnitř složených závorek objevit.

To lze ukázat pomocí rozšíření značek Xamarin.Forms s názvem `ConstraintExpression`, který se používá s `RelativeLayout` třídy. Můžete zadat umístění nebo velikost zobrazení podřízených jako konstanta, nebo relativní k nadřazené nebo jiných pojmenované zobrazení. Syntaxe `ConstraintExpression` umožňuje nastavit umístění a velikost zobrazení pomocí `Factor` časy vlastnost jiného zobrazení a navíc `Constant`. Vyžaduje něco složitější než kód.

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

Pravděpodobně nejdůležitější lekci byste měli podniknout od této ukázky je syntaxe rozšíření značek: žádné uvozovky musí být uvedena ve složených závorkách rozšíření značek. Při psaní rozšíření značek v souboru XAML, je přirozeně vhodné hodnoty vlastností uzavřete do uvozovek. Odolejte pokušení!

Tady je spuštěn program:

[![](xaml-markup-extensions-images/relativelayout.png "Relativní rozložení pomocí omezení")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "relativní rozložení pomocí omezení")

## <a name="summary"></a>Souhrn

Rozšíření značek XAML je vidět tady poskytují důležité podporu pro soubory XAML. Ale možná se nejvíc hodí v situaci rozšíření značek XAML `Binding`, který je popsaný v další části této série [část 4. Datové vazby Základy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Část 1. Začínáme s jazykem XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Část 2. Základní syntaxe jazyka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Část 4. Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Část 5. Z datové vazby k MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
