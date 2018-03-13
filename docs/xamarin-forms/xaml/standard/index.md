---
title: "Standardní XAML (Preview)"
description: "Jak začít seznamovat se standardní Preview XAML v Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: e53df69fdfd5b5c1fc98b667d4b75d06c16c35dc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="xaml-standard-preview"></a>Standardní XAML (Preview)

![Náhled](~/media/shared/preview.png)

Postupujte podle těchto kroků a experimentovat s standardní XAML v Xamarin.Forms:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Stažení [náhled zde balíček NuGet](https://aka.ms/xf-xamlstandard-nuget).
2. Přidat **Xamarin.Forms.Alias** balíček NuGet do vašich projektů Xamarin.Forms PCL, .NET Standard a platformu.
3. Inicializace balíček s `Alias.Init()`
4. Přidat `xmlns:a` odkaz `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Použít typy v jazyce XAML – viz [řídí odkaz](controls.md) Další informace.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Stažení [náhled zde balíček NuGet](https://aka.ms/xf-xamlstandard-nuget).
2. Přidat **Xamarin.Forms.Alias** balíček NuGet do vašich projektů Xamarin.Forms PCL, .NET Standard a platformu.
3. Inicializace balíček s `Alias.Init()`
4. Přidat `xmlns:a` odkaz `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Použít typy v jazyce XAML – viz [řídí odkaz](controls.md) Další informace.

-----

Následující XAML ukazuje některé XAML standardní ovládací prvky použitá v platformě Xamarin.Forms `ContentPage`:

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
> Vyžadování atribut xmlns `a:` předponu na XAML standardní ovládací prvky se o omezení aktuální verze preview.


## <a name="related-links"></a>Související odkazy

- [Náhled NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [Referenční informace o ovládacích prvcích](controls.md)
