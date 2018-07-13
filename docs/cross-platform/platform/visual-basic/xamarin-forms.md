---
title: Xamarin.Forms pomocí Visual Basic
description: Šablona projektu Xamarin.Forms PCL lze upravit pomocí jazyka Visual Basic pro hlavní sestavení umožňuje efektivně vytvářet multiplatformní mobilní aplikace pomocí VB.NET.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 256d5c81475be095c8fa0ab0408cbcf673c6b301
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997081"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin.Forms pomocí Visual Basic

Xamarin nepodporuje jazyka Visual Basic přímo – postupujte podle pokynů na této stránce vytvořit řešení, PCL Xamarin.Forms v jazyce C# a potom nahraďte běžné projekt PCL kódu pomocí jazyka Visual Basic.

[![](xamarin-forms-images/hero-sml.png "Vytvořit řešení, Xamarin.Forms PCL a potom nahraďte běžné projekt PCL kódu pomocí jazyka Visual Basic")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Musíte použít Visual Studio na Windows do aplikace pomocí jazyka Visual Basic.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Xamarin.Forms s názorným postupem jazyka Visual Basic

Postupujte podle těchto kroků můžete vytvořit jednoduchý projekt Xamarin.Forms, která používá jazyka Visual Basic:

1. Vytvořte nový *Xamarin.Forms jazyka C#* řešení, které používá přenosné knihovny tříd (PCL).
Přejděte na **soubor > Nový projekt** a **nový projekt** okno Přejít na **nainstalováno > šablony > Visual C# > pro různé platformy** klikněte na tlačítko  **Cross Platform App (Xamarin.Forms nebo Native) > Xamarin.Forms**.

2. Klikněte pravým tlačítkem na řešení a **Přidat > Nový projekt**.

3. Zvolte **jazyka Visual Basic > Knihovna tříd (přenosná)** typ projektu:

   [![](xamarin-forms-images/add-vb-2-sml.png "Přidat nový projekt knihovny přenosných tříd")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Vybrat platformy, jak je znázorněno nakonfigurovat správné profilem PCL (ji nezapomeňte zahrnout Xamarin.iOS a Xamarin.Android):

   ![](xamarin-forms-images/add-vb-3-sml.png "Vyberte platformy pro podporu")

5. Klikněte pravým tlačítkem na projekt jazyka Visual Basic a zvolte **vlastnosti**, změňte **výchozí obor názvů** tak, aby odpovídala stávající jazyka C# projekty:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Ujistěte se, že kořenový obor názvů jazyka Visual Basic odpovídá aplikace Xamarin.Forms")

6. Klikněte pravým tlačítkem na nový projekt jazyka Visual Basic a zvolte **spravovat balíčky Nuget**, nainstalujte **Xamarin.Forms** a zavřete okno Správce balíčků.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formuláře a zavřete okno Správce balíčků")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Přejmenujte výchozí **Class1** souboru *a* třídu `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Přejmenovat soubor výchozího Class1 a třídy do aplikace")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Vložte následující kód do **App.vb** soubor, který se stane výchozí bod aplikace Xamarin.Forms. Nezapomeňte zahrnout `Imports Xamarin.Forms` a přidejte `Inherits Application` do třídy:

    ```vb 
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

9. Nyní potřebujeme tak, aby odkazovala na nový projekt jazyka Visual Basic pro iOS a Android projekty.
Klikněte pravým tlačítkem na **odkazy** uzlu v iOS a Android projekty otevřít **správce odkazů**. Zrušit značek přenosné knihovny jazyka C# a značky přenosné knihovny jazyka Visual Basic (není zapomenout, proveďte to pro iOS a Android projektů).

   [![](xamarin-forms-images/add-vb-8-sml.png "Odebrat starý odkaz na projekt, přidejte referenční dokumentace jazyka Visual Basic")](xamarin-forms-images/add-vb-8.png#lightbox)

10. Odstranění projektu přenosné C#. Přidat nový **.vb** výstupní soubory k sestavení aplikace Xamarin.Forms. Šablonu pro nové `ContentPage`s v jazyce Visual Basic je uveden níže:

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Omezení jazyka Visual Basic v Xamarin.Forms

Jak je uvedeno na [Visual Basic.NET Portable stránky](~/cross-platform/platform/visual-basic/index.md), Xamarin nepodporuje jazyk Visual Basic. To znamená, že narazíte na určitá omezení, na které můžete použít Visual Basic:

 - Vlastní Renderery nelze napsané v jazyce Visual Basic, se musí být napsané v jazyce C# v projektech pro nativní platformy.

 - Implementace služby závislostí nelze zapsat v jazyce Visual Basic, se musí být napsané v jazyce C# v projektech pro nativní platformy.

 - Stránky XAML nemůže být součástí projektu jazyka Visual Basic – může vytvořit pouze generátor kódu jazyka C#. Je možné zahrnout XAML v samostatné, odkazovaná, C# přenosnou knihovnu tříd a používat datové vazby k naplnění souborů XAML pomocí jazyka Visual Basic modely (příkladem je součástí [ukázka](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin nepodporuje jazyk Visual Basic.NET.

## <a name="related-links"></a>Související odkazy

- [XamarinFormsVB (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Vývoj Multiplatformních aplikací pomocí rozhraní .NET Framework](https://docs.microsoft.com/dotnet/standard/cross-platform/)
