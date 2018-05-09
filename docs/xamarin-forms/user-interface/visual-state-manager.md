---
title: Správce stavu Visual Xamarin.Forms
description: Pomocí Visual správce stavu můžete provádět změny v elementů XAML podle visual stavy nastavit z kódu.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: f511f5c33b947704a42df850d2772c0b26511173
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="the-xamarinforms-visual-state-manager"></a>Správce stavu Visual Xamarin.Forms

_Pomocí Visual správce stavu můžete provádět změny v elementů XAML podle visual stavy nastavit z kódu._

Správce stavu Visual (VSM) je nová v Xamarin.Forms 3.0. VSM poskytuje strukturovaných způsob, jak změnit visual uživatelské rozhraní z kódu. Ve většině případů je definována uživatelské rozhraní aplikace v jazyce XAML, a tento jazyk XAML obsahuje kód popisující, jak Visual správce stavu ovlivňuje vizuální prvky uživatelského rozhraní.

VSM zavádí koncepci _visual stavy_. Zobrazení Xamarin.Forms, jako `Button` může mít několik různých visual vzhled v závislosti na jeho stav podkladového &mdash; jestli je vypnutá, nebo stisknutí nebo má vstupu fokus. Jedná se o stavy tlačítka.

Visual stavy se shromažďují v _visual stavu skupiny_. Všechny stavy visual v rámci skupiny visual stavu se vzájemně vylučují. Visual stavy i visual stavu skupiny se označují pomocí jednoduchého textového řetězce.

V původní verze správce stavu Visual Xamarin.Florms definuje jednu skupinu visual stavu s názvem "CommonStates" se tři visual stavy:

- "Normální"
- "Zakázáno"
- "Zaměřuje"

Tato skupina visual stavu je podporována pro všechny třídy, které jsou odvozeny od [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), což je základní třídu pro [ `View` ](xref:Xamarin.Forms.View) a [ `Page` ](xref:Xamarin.Forms.Page). 

Můžete také definovat vlastní skupiny visual stavu a visual stavy, jako tento článek popisuje.

> [!NOTE]
> Vývojáři Xamarin.Forms, kteří znají [aktivační události](~/xamarin-forms/app-fundamentals/triggers.md) víte, že aktivační události můžete také provádět změny vizuální prvky v uživatelském rozhraní na základě změn v vlastností zobrazení nebo pálení událostí. Jak nakládat s různé kombinace těchto změn pomocí aktivační události však může stát velmi matoucí. V minulosti Visual správce stavu byla zavedena v prostředích založených na Windows XAML ke zmírnění nedorozuměním vyplývající z kombinace visual stavy. Pomocí funkce VSM vždy se vzájemně vylučují visual stavy v rámci skupiny visual stavu V každém okamžiku pouze jeden stav v každé skupině je aktuální stav.

## <a name="the-common-states"></a>Běžné stavy

V původní verze Visual správce stavu můžete obsahovat části v souboru XAML, který můžete změnit vzhled zobrazení, pokud zobrazení Normální nebo zakázáno, nebo má zaměření pro vstup. Toto jsou známé jako _běžné stavy_.

Předpokládejme například, že máte `Entry` zobrazení na stránku. Tady je způsob vzhled `Entry` změnit:

- `Entry` By měl mít růžová na pozadí při `Entry` je zakázána.
- `Entry` By měl mít pozadí vápna normálně.
- `Entry` By měl rozbalit na dvakrát výšku normální, pokud ho má vstupu fokus.

Kód VSM za provozu můžete připojit jednotlivých zobrazení, nebo můžete ji definovat v styl Pokud se vztahuje na více zobrazení. V následujících dvou částech popisují tyto přístupy.

### <a name="vsm-markup-on-a-view"></a>Značka VSM za provozu na zobrazení

Připojit VSM značek pro `Entry` zobrazit, nejprve oddělení `Entry` do počáteční a koncové značky:

```xaml
<Entry FontSize="18">

</Entry>
```

Udělil velikosti písma explicitní, protože jeden z stavů použije `FontSize` vlastnost zdvojnásobit velikost textu v `Entry`.

V dalším kroku vložit `VisualStateManager.VisualStateGroups` značky mezi tyto značky:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

To může vypadat trochu neobvyklé. Za normálních okolností je pouze kód, který se zobrazí mezi dvě značky toto řazení pro obsah nebo vlastnost elementy a `VisualStateManager.VisualStateGroups` značka je ani jeden z nich.

Toto je syntaxe právní XAML, protože [ `VisualStateGroups` ](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) připojené vazbu vlastnost definované [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) třídy. (Další informace o přidružené vlastnosti vazbu, najdete v článku [přidružené vlastnosti](~/xamarin-forms/xaml/attached-properties.md).) Jedná se jak `VisualStateGroups` vlastnost je připojen k `Entry` objektu.

`VisualStateGroups` Vlastnost je typu [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList), což je kolekce [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) objekty. V rámci `VisualStateManager.VisualStateGroups` značky, vložte pár `VisualStateGroup` značky pro každou skupinu visual stavy, které chcete zahrnout:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Všimněte si, že `VisualStateGroup` má `x:Name` atribut, který určuje název skupiny. `VisualStateGroup` Třída definuje `Name` vlastnost, kterou můžete použít místo:

```xaml
<VisualStateGroup Name="CommonStates">
```

`VisualStateGroup` Třída definuje vlastnost s názvem [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States), což je kolekce [ `VisualState` ](xref:Xamarin.Forms.VisualState) objekty. `States` Vlastnost obsahu je `VisualStateGroups` , můžete zahrnout `VisualState` přímo mezi značky `VisualStateGroup` značky.

Dalším krokem je pro zahrnutí pár značky pro každý stav visual do této skupiny. Také lze je identifikovat pomocí `x:Name` nebo `Name`:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` definuje vlastnost s názvem [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters), což je kolekce [ `Setter` ](xref:Xamarin.Forms.Setter) objekty. Tyto jsou stejné `Setter` objekty, které používáte ve [ `Style` ](xref:Xamarin.Forms.Style) objektu.

`Setters` je _není_ obsahu vlastnost `VisualState`, takže je nutné zahrnout značky elementu vlastnost pro `Setters` vlastnost:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
    
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Nyní můžete vložit jeden nebo více `Setter` objektů mezi každý pár `Setters` značky. Jedná se o `Setter` objekty, které definují visual stavy popsané výše:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Každý `Setter` značka označuje hodnotu, která konkrétní vlastnosti, když tento stav je aktuální. Vlastnost odkazuje `Setter` objekt musí být zajištěna vazbu vlastnosti.

Podobně jako tento kód je základem **VSM za provozu na zobrazení** stránku **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** ukázka programu. Stránka obsahuje tři `Entry` zobrazení, ale jenom na druhou má kód VSM za provozu k němu připojen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />

        <Label Text="Entry with VSM: " />

        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Všimněte si, že druhá `Entry` má také `DataTrigger` jako součást jeho `Trigger` kolekce. To způsobí, že `Entry` na zakázáno, dokud nebude něco je zadán do třetí `Entry`. Zde je stránka při spuštění se systémem iOS, Android a univerzální platformu Windows (UWP):

[![VSM za provozu na zobrazení: Zakázáno](vsm-images/VsmOnViewDisabled.png "VSM za provozu na zobrazení – zakázáno")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Aktuální stav visual je "Zakázat" proto pozadí druhý `Entry` je růžový na iOS a Android obrazovky. Implementace UWP `Entry` není povoleno nastavení na pozadí barvu, kdy `Entry` je zakázána. 

Zadáte-li něco do třetí `Entry`, druhý `Entry` přepínače do stavu "Normální" a na pozadí je nyní vápna:

[![VSM za provozu na zobrazení: normální](vsm-images/VsmOnViewNormal.png "VSM za provozu na zobrazení – Normální")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Když touch druhý `Entry`, získá zaměření pro vstup. Se přepne do stavu "Focused" a zasahuje do dvakrát na jeho výšku:

[![VSM za provozu na zobrazení: zaměřuje](vsm-images/VsmOnViewFocused.png "VSM za provozu na zobrazení – zaměřuje")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Všimněte si, že `Entry` nezachovává vápna pozadí, když získá zaměření pro vstup. Visual správce stavu přepínat mezi visual stavy, jsou nastavení vlastnosti, která nastavuje předchozího stavu. Uvědomte si, že visual stavy se vzájemně vylučují. Stav "Normální" neznamená výhradně `Entry` je povoleno. Znamená to, že `Entry` zapnutá a nemá zaměření pro vstup. 

Pokud chcete `Entry` tak, aby měl vápna pozadí ve stavu "Focused", přidejte další `Setter` na tento visual stav:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

V pořadí pro tyto `Setter` objekty fungovalo správně, `VisualStateGroup` musí obsahuje `VisualState` objekty pro všechny stavy, které jsou v této skupině. Pokud je visual stavu, který nemá žádné `Setter` objekty, zahrnují ji přesto jako prázdný značky:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="vsm-markup-in-a-style"></a>Značka VSM za provozu ve stylu

Často je nezbytné pro sdílení kód Visual správce stavu mezi dva nebo více zobrazení. V takovém případě budete chtít umístit značku do `Style` definice.

Tady je existující implicitní `Style` pro `Entry` elementů v **VSM za provozu na zobrazení** stránky:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Přidat `Setter` značky pro `VisualStateManager.VisualStateGroups` přidružená vazbu vlastnost:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

Vlastnost obsahu pro `Setter` je `Value`, takže hodnota `Value` vlastnost lze zadat přímo v rámci těchto značek. Zda je vlastnost typu `VisualStateGroupList`:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style> 
```

V rámci těchto značek může obsahovat jeden či více `VisualStateGroup` objekty:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style> 
```

Zbývající část kód VSM za provozu je stejná jako předtím.

Tady je **VSM za provozu ve stylu** stránky zobrazující kód dokončení VSM za provozu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">

                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />
        
        <Label Text="Entry with VSM: " />

        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Nyní všechny `Entry` zobrazení na této stránce reagovat stejným způsobem jako na jejich visual stavy. Všimněte si také, že stav "Focused" teď obsahuje druhý `Setter` udávající každý `Entry` vápna na pozadí i když ho má vstupu fokus:

[![VSM za provozu ve stylu](vsm-images/VsmInStyle.png "VSM za provozu ve stylu")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Definování vlastní visual stavy

Každá třída, která je odvozena z `VisualElement` podporuje tři běžné stavy "Normální", "Zaměřuje" a "Zakázáno". Interně [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) třída rozpozná, pokud se stává stále povolený nebo zakázaný, nebo cílených nebo nezaostřená a volá statických [ `VisualStateManager.GoToState` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualStateManager.GoToState/p/Xamarin.Forms.VisualElement/System.String/) metoda takto:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Toto je velmi důležitý metoda a je kód pouze Visual správce stavu najdete v `VisualElement` třídy. Protože `GoToState` je volána pro každý objekt v závislosti na každé vazby třída odvozená z `VisualElement`, Visual správce stavu můžete použít s žádným `VisualElement` objekt, který má odpovědět na tyto změny.

Interestingly, název skupiny visual stavu "CommonStates" není ve výslovně odkazována `VisualElement`. Název skupiny není součástí rozhraní API pro Visual správce stavu. V jednom ze dvou ukázka programu, pokud se zobrazí můžete změnit název skupiny z "CommonStates" na jakoukoli jinou a program bude i nadále fungovat. Název skupiny je jenom obecný popis stavů v této skupině. Implicitně předpokládá se, že visual stavy v kterékoli skupině se vzájemně vylučují: jeden stav a pouze jeden stav je aktuální kdykoli.

Pokud chcete implementovat vlastní visual stavy, budete muset volat `VisualStateManager.GoToState` z kódu. Nejčastěji budete provedete toto volání ze souboru kódu vaší třídy stránky.

**VSM ověření** stránku **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** příklad ukazuje, jak používat Visual správce stavu souvislosti s ověření vstupu. Soubor XAML se skládá ze dvou `Label` elementů `Entry`, a `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">
        
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

Značka VSM za provozu je připojen k druhý `Label` (s názvem `helpLabel`) a `Button` (s názvem `submitButton`). Existují dva stavy vzájemně vylučují, s názvem "Platné" a "Neplatný". (Uvidíte soubor kódu, který nastaví tyto stavy krátce.) Všimněte si, že každé dvě skupiny "ValidationState" obsahuje `VisualState` značky pro "Platné" a "Neplatná", i když jeden z nich je prázdná v každém případu. 

Pokud `Entry` neobsahuje platné telefonní číslo, a aktuální stav je "Neplatná". Druhý `Label` je viditelná a `Button` je zakázaná:

[![Ověření VSM za provozu: Neplatný stav](vsm-images/VsmValidationInvalid.png "VSM ověření - neplatná")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Pokud je zadáno platné telefonní číslo, pak aktuální stav se změní na "Platné". Druhý `Entry` zmizí a `Button` je teď povolená:

[![Ověření VSM za provozu: Neplatný stav](vsm-images/VsmValidationValid.png "VSM ověření - platný")](vsm-images/VsmValidationValid-Large.png#lightbox)

Je příslušné pro zpracování souboru kódu `TextChanged` událost z `Entry`. Obslužná rutina používá regulární výraz k určení, jestli vstupní řetězec je platný, nebo ne. Metoda v souboru kódu s názvem `GoToState` volá statických `VisualStateManager.GoToState` metodu pro obě `helpLabel` a `submitButton`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

Všimněte si také, že `GoToState` metoda je volána z konstruktoru k chybě při inicializaci stavu. Měla by existovat vždy aktuální stav. Ale nikde v kódu je všechny odkazy na název skupiny visual stavu, i když se odkazuje v XAML jako "ValidationStates" pro účely přehlednost. 

Všimněte si, že soubor kódu musí vzít v úvahu každého objektu na stránku, která má vliv tyto visual státy a k volání `VisualStateManager.GoToState` pro každý z těchto objektů. V tomto příkladu je pouze dva objekty ( `Label` a `Button`), ale může být několik další.

Může vás zajímat: Pokud souboru kódu musí odkazovat na stránce, která jsou ovlivněná tyto visual stavy každý objekt, proč nelze soubor kódu jednoduše objekty přímý přístup? Surely může. Však pomocí Visual správce stavu můžete řídit, jak tyto objekty reagovat na různých visual stavů zcela v jazyce XAML, který uchovává všechny návrh uživatelského rozhraní na jednom místě.

Může to být tempting vzít v úvahu odvození třídy z `Entry` a případně definování vlastnosti, která můžete nastavit, aby funkce externího ověřování. Třída odvozená z `Entry` pak můžete volat `VisualStateManager.GoToState` metoda. Toto schéma by pracovat správně, ale pouze tehdy, pokud `Entry` měla pouze objekt vliv různých visual stavů. V tomto příkladu `Label` a `Button` jsou také mít vliv. Neexistuje žádný způsob pro VSM značek připojené k `Entry` k řízení jiné objekty, na stránce a nijak pro připojené VSM značek tyto další objekty tak, aby odkazovaly změny ve visual stavu z jiného objektu.

<a name="adaptive-layout" />

## <a name="using-the-vsm-for-adaptive-layout"></a>Pomocí VSM adaptivní rozložení

Program Xamarin.Forms spuštěná na telefonu obvykle lze zobrazit v poměru stran výšku nebo na šířku a běžící v desktopovém programu Xamarin.Forms velikost lze změnit předpokládat, že mnoho různou velikost a poměr stran obrázku. Dobře navržených aplikace může zobrazit svůj obsah pro tyto různé typy zařízení stránky nebo okna. 

Tento postup se někdy označuje jako _adaptivní rozložení_. Protože adaptivní rozložení výhradně zahrnuje programu vizuály, je ideální aplikace Visual správce stavu.

Jednoduchým příkladem je program, který zobrazí malá skupina tlačítek, které by ovlivnily obsah aplikace. V režimu na výšku mohou být zobrazeny tato tlačítka v horní části stránky vodorovném řádku:

[![Adaptivní rozložení VSM za provozu: Na výšku](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM adaptivní rozložení - na výšku")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

V režimu na šířku pole tlačítka může být přesunout na jedné straně a zobrazí ve sloupci:

[![Adaptivní rozložení VSM za provozu: Na šířku](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM adaptivní rozložení - na šířku")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Shora dolů že je spuštěna pro univerzální platformu Windows, Android a iOS.

Toto je úloha pro Visual správce stavu. **Adaptivní rozložení VSM** stránku [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) ukázka definuje skupinu s názvem "OrientationStates" s dvěma stavy visual s názvem "Výšku" a "na šířku". (Složitější přístup může být založen na několik různých šířky stránky nebo okno.) 

Značka VSM za provozu se zobrazí v čtyři místa v souboru XAML. `StackLayout` s názvem `mainStack` obsahuje v nabídce a obsah, který je `Image` elementu. To `StackLayout` by měl mít svislou orientaci v režimu na výšku a vodorovné orientaci na šířku:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">

                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>

                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        
        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">

                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>

                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">

                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>

                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

Vnitřní `ScrollView` s názvem `menuScroll` a `StackLayout` s názvem `menuStack` implementovat nabídky tlačítka. Orientace těchto rozložení je opačné z `mainStack`: V nabídce by měla být v režimu na výšku vodorovného a svislého v režimu na šířku.

U čtvrtý bloku kódu VSM za provozu je v implicitní stylu pro tlačítka sami. Tento kód nastaví `VerticalOptions`, `HorizontalOptions`, a `Margin` vlastnosti specifické pro orienations portait a na šířku.

Nastaví souboru kódu `BindingContext` vlastnost `menuStack` implementovat `Button` tvorba příkazů a také připojí obslužnou rutinu do `SizeChanged` událostí stránky:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged` Volání obslužné rutiny `VisualStateManager.GoToState` pro dva `StackLayout` a `ScrollView` prvky a pak smyčky prostřednictvím podřízené objekty daného `menuStack` volat `VisualStateManager.GoToState` pro `Button` elementy.

Na první pohled může zdát, jako kdyby souboru kódu může zpracovávat změny orientace více přímo nastavením vlastnosti elementů v souboru XAML, ale Visual správce stavu je výborný více strukturovanými přístup. Všechny vizuálech udržovaly v souboru XAML, kde se bude snazší, zkontrolujte, spravovat a upravovat.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Správce stavu Visual s Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Správce stavu Visual Xamarin.Forms 3.0, pomocí [univerzity Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

