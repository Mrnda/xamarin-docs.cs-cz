---
title: Xamarin.Forms Visual State Manager
description: Pomocí Visual State Managerem provádět změny na elementy XAML podle vizuální stavy nastavení z kódu.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 0fdcbd6467547647089b436a894b1bc490ba5ee1
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854815"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms Visual State Manager

_Pomocí Visual State Managerem provádět změny na elementy XAML podle vizuální stavy nastavení z kódu._

Vizuální stav správce (VSM) je nového v Xamarin.Forms 3.0. Migrace VSM za provozu poskytuje strukturovaných způsob, jak provádět vizuální změny uživatelského rozhraní z kódu. Ve většině případů uživatelského rozhraní aplikace definované v XAML, a tento XAML obsahuje značky popisující, jak ovlivňuje vizuály uživatelské rozhraní Visual State Managerem.

Migrace VSM za provozu zavádí koncepci _vizuálních stavů_. Například zobrazení Xamarin.Forms `Button` může mít několik různých vzhledů visual v závislosti na jeho základní stav &mdash; Určuje, zda je zakázána, nebo stisknutí nebo má vstupní fokus. Jedná se o stavy tlačítka.

Vizuální stavy jsou shromážděny v _vizuálního stavu skupiny_. Všechny vizuální stavy v rámci skupiny vizuálního stavu se vzájemně vylučují. Jednoduché textové řetězce jsou označeny vizuálních stavů a skupin vizuální stav.

Xamarin.Forms Visual State Managerem definuje vizuální stav skupinu s názvem "CommonStates" se třemi vizuální stavy:

- "Normální"
- "Zakázáno"
- "Zaměřuje"

Tato skupina vizuálního stavu se podporuje pro všechny třídy, které jsou odvozeny z [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), což je základní třída pro [ `View` ](xref:Xamarin.Forms.View) a [ `Page` ](xref:Xamarin.Forms.Page). 

Můžete také definovat vlastní skupiny vizuálního stavu a vizuální stavy, jako tento článek vám ukáže.

> [!NOTE]
> Vývojáři Xamarin.Forms, kteří znají [triggery](~/xamarin-forms/app-fundamentals/triggers.md) si vědomi, že aktivační události můžete také provádět změny vizuály v uživatelském rozhraní na základě změn v vlastností zobrazení nebo vyvolanou událostí. Pomocí aktivační události se různé kombinace těchto změn však může být velmi matoucí. V minulosti Visual State Manager byla zavedena v prostředích založených na XAML pro Windows, abychom omezili zmatek vyplývající z kombinace vizuálních stavů. Pomocí funkce VSM vždy se vzájemně vylučují vizuálních stavů ve skupině vizuálních stavů V každém okamžiku pouze jeden stav v každé skupině je aktuální stav.

## <a name="the-common-states"></a>Běžné stavy

Visual State Managerem můžete zahrnout oddílů v souboru XAML, který můžete změnit vzhled zobrazení, pokud zobrazení je normální, nebo zakázaný nebo má vstupní fokus. Toto jsou známé jako _běžné stavy_.

Předpokládejme například, že máte `Entry` zobrazení na stránce, a chcete je vizuální vzhled `Entry` změnit následujícími způsoby:

- `Entry` By měly mít růžová na pozadí, když `Entry` je zakázaná.
- `Entry` By měl obvykle mít Limetkově pozadí.
- `Entry` By měli rozbalit dvakrát normální výšku, pokud ji má vstupní fokus.

Značek migrace VSM za provozu lze připojit k jednotlivým zobrazení, nebo můžete ji definovat ve stylu pokud platí pro několik zobrazení. V následujících dvou částech těchto přístupů.

### <a name="vsm-markup-on-a-view"></a>Migrace VSM za provozu značek na zobrazení

Připojit kód migrace VSM za provozu do `Entry` zobrazení, nejprve oddělení `Entry` do počáteční a koncovou značku:

```xaml
<Entry FontSize="18">

</Entry>
```

Udělil explicitní písmo, protože budou používat některý z stavů `FontSize` vlastnost na dvojnásobek velikosti textu `Entry`.

V dalším kroku vložit `VisualStateManager.VisualStateGroups` značky mezi značky:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) je připojené umožňujících vazbu vlastnosti definované [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) třídy. (Další informace o přidružené vlastnosti umožňující vazbu, najdete v článku [připojených vlastností](~/xamarin-forms/xaml/attached-properties.md).) Toto je způsob, jakým `VisualStateGroups` vlastnost je připojen k `Entry` objektu.

`VisualStateGroups` Vlastnost je typu [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList), což je kolekce [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) objekty. V rámci `VisualStateManager.VisualStateGroups` značky, vložit pár `VisualStateGroup` značky pro každou skupinu vizuální stavy, které chcete zahrnout:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Všimněte si, že `VisualStateGroup` značka nemá `x:Name` atribut název skupiny. `VisualStateGroup` Definuje třídu `Name` vlastnost, která můžete místo toho použít:

```xaml
<VisualStateGroup Name="CommonStates">
```

Můžete použít buď `x:Name` nebo `Name` , ale nikoli oba současně ve stejném elementu.

`VisualStateGroup` Třída definuje vlastnost s názvem [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States), což je kolekce [ `VisualState` ](xref:Xamarin.Forms.VisualState) objekty. `States` je _Vlastnost ContentProperty_ z `VisualStateGroups` tak můžete zahrnout `VisualState` přímo mezi značky `VisualStateGroup` značky. (Obsahu vlastnosti jsou popsány v následujícím článku [základní syntaxe XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Dalším krokem je této skupiny přidat pár značek pro každý vizuální stav. Také lze je identifikovat za použití `x:Name` nebo `Name`:

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

`VisualState` definuje vlastnost s názvem [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters), což je kolekce [ `Setter` ](xref:Xamarin.Forms.Setter) objekty. Jde o stejný `Setter` objekty, které můžete použít [ `Style` ](xref:Xamarin.Forms.Style) objektu.

`Setters` je _není_ vlastnost content `VisualState`, takže je potřeba zahrnout tagy vlastnosti elementu `Setters` vlastnost:

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

Nyní můžete vložit jeden nebo více `Setter` objektů mezi každý pár `Setters` značky. Jedná se o `Setter` objekty, které definují vizuálních stavů popsané výše:

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

Každý `Setter` značka označuje hodnoty konkrétní vlastnosti při tomto stavu je aktuální. Jakoukoli vlastnost odkazuje `Setter` objekt musí být se opírá o vlastnost s vazbou.

Podobně jako tento kód je základem **migrace VSM za provozu na zobrazení** stránku **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** ukázkový program. Na stránce obsahuje tři `Entry` zobrazení, ale pouze pro druhou kolekci je k němu připojená značky migrace VSM za provozu:

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

Všimněte si, že druhá `Entry` má také `DataTrigger` jako součást jeho `Trigger` kolekce. To způsobí, že `Entry` na zakázáno, dokud nebude něco je zadán do třetí `Entry`. Tady je stránku při spuštění počítače se systémem iOS, Android a univerzální platformu Windows (UPW):

[![Migrace VSM za provozu pro zobrazení: Zakázáno](vsm-images/VsmOnViewDisabled.png "migrace VSM za provozu na zobrazení – zakázáno")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Aktuální vizuální stav je "zakázáno" proto pozadí druhého `Entry` je růžový v Iosu a Androidu obrazovky. Implementace UPW `Entry` není povoleno nastavení na pozadí barvu při `Entry` je zakázaná. 

Když zadáte nějaký text do třetí `Entry`, druhý `Entry` přepne do stavu "Normální" a na pozadí je nyní Limetkově:

[![Migrace VSM za provozu pro zobrazení: normální](vsm-images/VsmOnViewNormal.png "migrace VSM za provozu na zobrazení – Normální")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Po stisknutí druhý `Entry`, získá fokus vstupu. Přepne do stavu "Focused" a rozšíří na dvojnásobkem výšky:

[![Migrace VSM za provozu pro zobrazení: zaměřuje](vsm-images/VsmOnViewFocused.png "migrace VSM za provozu na zobrazení - fokus")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Všimněte si, že `Entry` nezachovává Limetkově pozadí, když dostane zaměření pro vstup. Jak Visual State Manager Přepne mezi vizuálních stavů, se zrušit nastavení vlastnosti nastavením předchozího stavu. Mějte na paměti, že vizuál se vzájemně vylučují. Stav "Normální" neznamená výhradně `Entry` je povolená. To znamená, že `Entry` zapnutá a nemá fokus vstupu. 

Pokud chcete, aby `Entry` Pokud chcete, aby na pozadí Limetkově ve stavu "Focused", přidejte další `Setter` do tohoto vizuálního stavu:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Aby se tyto `Setter` objekty fungovalo správně, `VisualStateGroup` musí obsahovat `VisualState` objekty pro všechny státy v této skupině. Pokud je vizuální stav, který nemá žádné `Setter` objekty, zahrnout i přesto jako prázdné značky:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Vizuální stav správce značek ve stylu

Často je potřeba sdílet stejný kód Visual State Managerem mezi dva nebo více zobrazení. V takovém případě budete chtít umístit značku `Style` definice.

Tady je existující implicitní `Style` pro `Entry` prvků v **migrace VSM za provozu na zobrazení** stránky:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Přidat `Setter` značky pro `VisualStateManager.VisualStateGroups` přidružená vlastnost podporující vazby:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

Vlastnost obsahu pro `Setter` je `Value`, takže hodnota `Value` vlastnost se dá zadat přímo v rámci těchto značek. Vlastnost je typu `VisualStateGroupList`:

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

Zbývající část značky migrace VSM za provozu je stejná jako předtím.

Tady je **migrace VSM za provozu ve stylu** stránky značkami kompletní migrace VSM za provozu:

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

Nyní vše `Entry` reagovat stejným způsobem jejich vizuálních stavů zobrazení na této stránce. Všimněte si také, stav "Focused" teď zahrnuje sekundy `Setter` , který poskytuje každý `Entry` Limetkově pozadí také, když má vstupní fokus:

[![Migrace VSM za provozu ve stylu](vsm-images/VsmInStyle.png "migrace VSM za provozu ve stylu")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Definování vlastních vizuálních stavů

Každá třída, která je odvozena z `VisualElement` podporuje tři běžné stavy "Normální", "Zaměřuje" a "Zakázáno". Interně [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) třídy rozpozná, pokud je stále povolený nebo zakázaný, nebo cílené nebo bez fokusu a volá statický [ `VisualStateManager.GoToState` ](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) metody:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Toto je pouze kód Visual State Managerem, který najdete v `VisualElement` třídy. Protože `GoToState` je volána pro každý objekt, na základě každé třídy, která je odvozena z `VisualElement`, Visual State Managerem můžete použít s žádným `VisualElement` objekt reakce na tyto změny.

Zajímavé, název vizuálního stavu skupiny "CommonStates" není odkazuje explicitně `VisualElement`. Název skupiny není součástí rozhraní API pro Visual State Managerem. V jednom ze dvou ukázkový program zobrazí zatím můžete změnit název skupiny z "CommonStates" cokoli, a program bude i nadále fungovat. Název skupiny je pouze obecný popis státy v této skupině. Implicitně předpokládá se, že se navzájem vylučují vizuálních stavů v libovolné skupině: jeden stav a pouze jeden stav jsou aktuální v okamžiku.

Pokud chcete implementovat vlastní vizuálních stavů, budete muset volat `VisualStateManager.GoToState` z kódu. Nejčastěji budete provedete toto volání ze souboru modelu code-behind třídy stránky.

**Ověření migrace VSM za provozu** stránku **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** příklad ukazuje, jak používat Visual State Managerem v souvislosti s ověření vstupu. Soubor XAML se skládá ze dvou `Label` prvky, `Entry`, a `Button`:

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

Migrace VSM za provozu značek je připojen k druhé `Label` (s názvem `helpLabel`) a `Button` (s názvem `submitButton`). Existují dva stavy vzájemně vylučují s názvem "Platný" a "Neplatný". Všimněte si, že každý dvě skupiny "ValidationState" obsahuje `VisualState` značky pro "Platný" a "Neplatný", i když se jeden z nich je v každém případě prázdné. 

Pokud `Entry` neobsahuje platné telefonní číslo, pak jeho aktuální stav je "Neplatný" a tak druhá `Label` je viditelná a `Button` zakázaná:

[![Ověření migrace VSM za provozu: Neplatný stav](vsm-images/VsmValidationInvalid.png "ověření migrace VSM za provozu – neplatný")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Po zadání platné telefonní číslo, pak aktuální stav se změní na "Platné". Druhá `Entry` zmizí a `Button` je nyní povolena:

[![Ověření migrace VSM za provozu: Neplatný stav](vsm-images/VsmValidationValid.png "ověření migrace VSM za provozu - platný")](vsm-images/VsmValidationValid-Large.png#lightbox)

Soubor kódu na pozadí je příslušné pro zpracování `TextChanged` událost z `Entry`. Obslužná rutina používá regulární výraz k určení, zda vstupní řetězec je platný, nebo ne. Metoda v souboru kódu na pozadí s názvem `GoToState` volání statické `VisualStateManager.GoToState` metody pro oba `helpLabel` a `submitButton`:

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

Všimněte si také, že `GoToState` metoda je volána z konstruktoru inicializace stavu. Vždy by měl být aktuální stav. Ale nikde v kódu existuje odkaz na název vizuálního stavu skupiny, i když se odkazuje v XAML jako "ValidationStates" v zájmu přehlednosti. 

Všimněte si, že soubor kódu na pozadí musí vzít v úvahu každého objektu na stránce, která je tím ovlivněná tyto vizuálních stavů a volat `VisualStateManager.GoToState` pro každý z těchto objektů. V tomto příkladu je pouze dva objekty ( `Label` a `Button`), ale může být několik další.

Může vás zajímat: Pokud soubor kódu na pozadí musí odkazovat na všechny objekty na stránce, která jsou ovlivněná tyto vizuální stavy, proč nelze soubor použití modelu code-behind jednoduše objekty přímý přístup? Seděl může. Výhodou použití migrace VSM za provozu je však řízení způsobu, jakým visual prvky reagovat na jiný stav zcela v XAML, který uchovává všechny návrhu uživatelského rozhraní na jednom místě. Tím se vyhnete nastavení vzhled přístupem k vizuální prvky přímo z kódu.

Může být lákavé vzít v úvahu odvození třídy z `Entry` a pravděpodobně definovat vlastnost, která můžete nastavit na externí ověřovací funkce. Třída odvozená z `Entry` pak můžete volat `VisualStateManager.GoToState` metody. Toto schéma by fungovalo správně, ale pouze v případě, `Entry` byly pouze objekt ovlivněn různých vizuálních stavů. V tomto příkladu `Label` a `Button` se použije i. Neexistuje žádný způsob u VSM značek připojené k `Entry` řízení další objekty na stránce a žádný způsob pro značky migrace VSM za provozu připojené k těmto jinými objekty pro změnu vizuálního stavu odkazovat z jiného objektu.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Pomocí Visual State Managerem přizpůsobivé rozložení

Můžete změnit velikost Xamarin.Forms, které aplikace běžící na telefonu můžete zobrazit obvykle v portrét nebo poměr stran na šířku a běžící v desktopovém programu Xamarin.Forms předpokládat, že mnoho různých velikostí a poměry stran. Dobře navržená aplikace může zobrazit svůj obsah pro tyto různé typy zařízení stránky nebo okna. 

Tato technika se někdy označuje jako _přizpůsobivé rozložení_. Protože přizpůsobivé rozložení pouze zahrnuje vizuály programu, je ideální aplikace z Visual State Managerem.

Jednoduchým příkladem je aplikace, která se zobrazí malá skupina tlačítek, které by ovlivnily obsah aplikace. V režimu na výšku může zobrazit tato tlačítka v horní části stránky vodorovnou:

[![Migrace VSM za provozu přizpůsobivé rozložení: Na výšku](vsm-images/VsmAdaptiveLayoutPortrait.png "přizpůsobivé rozložení migrace VSM za provozu – na výšku")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

V režimu na šířku pole tlačítka může být přesunout na jedné straně a zobrazí ve sloupci:

[![Migrace VSM za provozu přizpůsobivé rozložení: Šířku](vsm-images/VsmAdaptiveLayoutLandscape.png "migrace VSM za provozu přizpůsobivé rozložení – na šířku")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Shora dolů je program spuštěn na univerzální platformu Windows, Android a iOS.

**Přizpůsobivé rozložení migrace VSM za provozu** stránku [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) ukázka definuje skupinu s názvem "OrientationStates" se dvěma vizuálních stavů s názvem "Na výšku" a "Krajina". (Složitější přístup může být založená na několik různých šířkách stránky nebo okna.) 

Migrace VSM za provozu značek probíhá čtyři místa v souboru XAML. `StackLayout` s názvem `mainStack` obsahuje nabídky a obsah, který je `Image` elementu. To `StackLayout` by měl mít svislou orientaci na výšku v režimu a vodorovné orientaci na šířku v režimu:

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

Vnitřní `ScrollView` s názvem `menuScroll` a `StackLayout` s názvem `menuStack` implementovat v nabídce tlačítka. Orientace tyto rozloženích je opačné z `mainStack`. V nabídce by měl být v režimu na výšku vodorovného a svislého v režimu na šířku.

Čtvrtá část značky migrace VSM za provozu je v implicitní styl tlačítek sami. Tento kód nastaví `VerticalOptions`, `HorizontalOptions`, a `Margin` vlastnosti specifické pro orientace portait a na šířku.

Nastaví soubor použití modelu code-behind `BindingContext` vlastnost `menuStack` k implementaci `Button` příkazů a také připojí obslužnou rutinu pro `SizeChanged` událostí stránky:

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

`SizeChanged` Volání obsluhy `VisualStateManager.GoToState` dvou `StackLayout` a `ScrollView` prvky a pak cyklicky projde podřízené objekty daného `menuStack` volat `VisualStateManager.GoToState` pro `Button` elementy.

To může zdát, jako kdyby soubor kódu na pozadí může zpracovávat změny orientace více přímo nastavením vlastnosti elementů v XAML souboru, ale Visual State Managerem je jednoznačně více strukturovaný přístup. Všechny vizuály, které jsou uloženy v souboru XAML, ve kterém budou snadněji zkontrolovat, spravovat a upravovat.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager s Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 Visual State Manager, [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Související odkazy

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

