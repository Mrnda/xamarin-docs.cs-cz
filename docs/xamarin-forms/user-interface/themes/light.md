---
title: Xamarin.Forms světlý motiv
description: Tento článek vysvětluje, jak využívat motiv světlý Xamarin.Forms v aplikaci.
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 7f40e375d653acec60f8848627234ab46fcce8de
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842749"
---
# <a name="xamarinforms-light-theme"></a>Xamarin.Forms světlý motiv

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!NOTE]
> Motivy vyžadují verzi preview Xamarin.Forms 2.3. Zkontrolujte, [tipy pro řešení potíží](~/xamarin-forms/user-interface/themes/index.md) Pokud dojde k chybě.

Chcete-li použít motiv světlý:

## <a name="1-add-nuget-packages"></a>1. Přidání balíčků Nuget

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. Přidejte do slovníku prostředků

V **App.xaml** souboru přidat nové vlastní `xmlns` motivu a potom ověřte motivu prostředky jsou sloučeny s slovník prostředků aplikace.
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

Použít tento [řešení potíží s krok](~/xamarin-forms/user-interface/themes/index.md) a přidejte požadovaný kód v iOS a aplikace pro Android projekty.

## <a name="4-use-styleclass"></a>4. Použití StyleClass

Tady je příklad tlačítek a popisky v motiv světlý, spolu s kód, který vytvoří je.

[![](light-images/light-theme-sml.png "Tlačítka a popisky v světlý motiv")](light-images/light-theme.png#lightbox "tlačítka a popisky v světlý motiv")

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

[Úplný seznam předdefinovaných tříd](~/xamarin-forms/user-interface/themes/index.md) ukazuje, jaké styly jsou k dispozici pro některé běžné ovládací prvky.
