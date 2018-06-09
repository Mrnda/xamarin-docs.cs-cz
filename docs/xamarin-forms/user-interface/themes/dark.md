---
title: Motiv světlý Xamarin.Forms
description: Tento článek vysvětluje, jak využívat tmavým motivem Xamarin.Forms v aplikaci.
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1fc329f506afde04b0dc59dc637d999865aafbe1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245921"
---
# <a name="xamarinforms-dark-theme"></a>Motiv světlý Xamarin.Forms

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!NOTE]
> Motivy vyžadují Xamarin.Forms 2.3 verzi preview. Zkontrolujte [tipy pro odstraňování potíží](~/xamarin-forms/user-interface/themes/index.md) Pokud dojde k chybám.

Použití tmavým motivem:

## <a name="1-add-nuget-packages"></a>1. Přidání balíčků Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. Přidejte do slovníku prostředků

V **App.xaml** soubor přidat nové vlastní `xmlns` pro motiv a pak zkontrolujte prostředky v motivu jsou sloučeny s slovník prostředků aplikace.
Příklad souboru XAML je zobrazena níže:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Načtení třídy motiv

Potom zadejte [řešení potíží s krok](~/xamarin-forms/user-interface/themes/index.md) a přidejte požadované kód v projekty aplikace pro Android a iOS.

## <a name="4-use-styleclass"></a>4. Použití StyleClass

Tady je příklad tlačítka a popisky v tmavý motiv, společně s kód, který vytváří je.

[![](dark-images/dark-theme-sml.png "Popisky v tmavým motivem a tlačítka")](dark-images/dark-theme.png#lightbox "popisky v tmavým motivem a tlačítka")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

[Úplný seznam předdefinovaných třídy](~/xamarin-forms/user-interface/themes/index.md) ukazuje, jaké styly jsou dostupné pro některé běžné ovládací prvky.
