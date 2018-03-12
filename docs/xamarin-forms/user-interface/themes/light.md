---
title: "Motiv světlý"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 21d5478c7e8317510a6f46b72a261582c9432301
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="light-theme"></a>Motiv světlý

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!NOTE]
> Motivy vyžadují Xamarin.Forms 2.3 verzi preview. Zkontrolujte [tipy pro odstraňování potíží](~/xamarin-forms/user-interface/themes/index.md) Pokud dojde k chybám.

Použití motiv světlý:

## <a name="1-add-nuget-packages"></a>1. Přidání balíčků Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. Přidejte do slovníku prostředků

V **App.xaml** soubor přidat nové vlastní `xmlns` pro motiv a pak zkontrolujte prostředky v motivu jsou sloučeny s slovník prostředků aplikace.
Příklad souboru XAML je zobrazena níže:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:light="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light">
    <Application.Resources>
        <ResourceDictionary MergedWith="light:LightThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. Načtení třídy motiv

Potom zadejte [řešení potíží s krok](~/xamarin-forms/user-interface/themes/index.md) a přidejte požadované kód v projekty aplikace pro Android a iOS.

## <a name="4-use-styleclass"></a>4. Použití StyleClass

Tady je příklad tlačítka a popisky v motiv světlý, společně s kód, který vytváří je.

[ ![](light-images/light-theme-sml.png "Popisky v motiv světlý a tlačítka")](light-images/light-theme.png "popisky v motiv světlý a tlačítka")

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
