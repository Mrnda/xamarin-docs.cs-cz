---
title: XAML Standard (Preview)
description: Tento článek vysvětluje, jak vám umožní začít zkoumat standardní náhled XAML v Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 61e0fa2587ce9a8794dbd32ff9de1f13da857342
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38838007"
---
# <a name="xaml-standard-preview"></a>XAML Standard (Preview)

![Náhled](~/media/shared/preview.png)

Postupujte podle těchto kroků můžete experimentovat s XAML Standard v Xamarin.Forms:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Stáhněte si [ve verzi preview zde balíček NuGet](https://aka.ms/xf-xamlstandard-nuget).
2. Přidat **Xamarin.Forms.Alias** balíček NuGet do vašich projektů Xamarin.Forms .NET Standard a platformu.
3. Inicializujte balíček s `Alias.Init()`
4. Přidat `xmlns:a` odkaz `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Použití typů v XAML – viz [řídí odkaz](controls.md) Další informace.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Stáhněte si [ve verzi preview zde balíček NuGet](https://aka.ms/xf-xamlstandard-nuget).
2. Přidat **Xamarin.Forms.Alias** balíček NuGet do vašich projektů Xamarin.Forms .NET Standard a platformu.
3. Inicializujte balíček s `Alias.Init()`
4. Přidat `xmlns:a` odkaz `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Použití typů v XAML – viz [řídí odkaz](controls.md) Další informace.

-----

Ukazuje některé používá v Xamarin.Forms XAML standardní ovládací prvky XAML následující `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Vyžadování xmlns `a:` předpony u XAML standardní ovládací prvky se o omezení aktuální verze preview.


## <a name="related-links"></a>Související odkazy

- [NuGet ve verzi Preview](https://aka.ms/xf-xamlstandard-nuget)
- [Referenční informace o ovládacích prvcích](controls.md)
