---
title: Xamarin.Forms using Visual Basic.NET
description: Šablona projektu Xamarin.Forms PCL lze upravit pomocí jazyka Visual Basic pro hlavní sestavení, umožňuje efektivně vytvářet různé platformy mobilních aplikací pomocí VB.NET.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e26d330d62e6ffdfdb3f9b8eab59e57a6377a86c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin.Forms using Visual Basic.NET

Xamarin nepodporuje jazyka Visual Basic přímo – postupujte podle pokynů na této stránce, pokud chcete vytvořit řešení Xamarin.Forms PCL C# a nahradit běžné PCL projektu kódu v jazyce Visual Basic.

[![](xamarin-forms-images/hero-sml.png "Vytvoření řešení Xamarin.Forms PCL a potom můžete nahradit běžné PCL projektu kódu v jazyce Visual Basic")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual Studio v systému Windows, musíte použít programu v jazyce Visual Basic.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Xamarin.Forms v průvodci jazyka Visual Basic

Postupujte podle těchto kroků můžete vytvořit jednoduché Xamarin.Forms projekt, který používá jazyka Visual Basic:

1. Vytvořte novou *Xamarin.Forms C#* řešení, které využívá přenosných třída knihovny PCL ().
Přejděte na **soubor > Nový projekt** a v **nový projekt** okno přejděte na **nainstalovaná > šablony > Visual C# > křížové platformy** zvolte  **Mezi aplikace platformy (Xamarin.Forms nebo nativní) > Xamarin.Forms**.

2. Klikněte pravým tlačítkem na řešení a **Přidat > Nový projekt**.

3. Vyberte **jazyka Visual Basic > knihovny tříd (přenositelností)** typ projektu:

   [![](xamarin-forms-images/add-vb-2-sml.png "Přidání nového projektu knihovny přenosných tříd")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Vybrat platformy, jak je znázorněno konfigurace správného profilu PCL (nezapomeňte zahrnout Xamarin.iOS a Xamarin.Android):

   ![](xamarin-forms-images/add-vb-3-sml.png "Vyberte platformy pro podporu")

5. Klikněte pravým tlačítkem na projekt Visual Basic a zvolte **vlastnosti**, pak změňte **výchozí obor názvů** tak, aby odpovídala stávající jazyka C# projekty:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Ujistěte se, že aplikace platformě Xamarin.Forms odpovídá kořenového oboru názvů jazyka Visual Basic")

6. Klikněte pravým tlačítkem na nový projekt jazyka Visual Basic a zvolte **spravovat balíčky Nuget**, potom nainstalovat **Xamarin.Forms** a zavřete okno Správce balíčků.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formuláře a zavře okno Správce balíčků")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Přejmenování výchozí **Class1** soubor *a* třídy k `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Přejmenujte soubor výchozího Class1 a třídy aplikace")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Vložte následující kód do **App.vb** souboru, který se stane výchozí bod aplikace Xamarin.Forms. Nezapomeňte zahrnout `Imports Xamarin.Forms` a přidejte `Inherits Application` pro třídu:

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

9. Teď musíme bod iOS a Android projekty v novém projektu jazyka Visual Basic.
Klikněte pravým tlačítkem na **odkazy** uzel v iOS a Android projekty otevřete **správce odkazů**. Zrušení značek přenosné knihovny jazyka C# a značek přenosné knihovny jazyka Visual Basic (nemáte zapomenete, to udělat pro iOS a Android projekty).

   [![](xamarin-forms-images/add-vb-8-sml.png "Odebrat starý odkaz na projekt, přidejte referenční dokumentace jazyka Visual Basic")](xamarin-forms-images/add-vb-8.png#lightbox)

10. Odstranění přenosné projektu C#. Přidat nové **VB** soubory k sestavení mimo aplikaci Xamarin.Forms. Šablonu pro nové `ContentPage`s v jazyce Visual Basic je zobrazena níže:

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

Jak je uvedeno na [přenosné Visual Basic.NET stránky](~/cross-platform/platform/visual-basic/index.md), Xamarin nepodporuje jazyk Visual Basic. To znamená, že existují určitá omezení, na které můžete použít jazyka Visual Basic:

 - V jazyce Visual Basic nelze zapsat vlastní nástroji pro vykreslování, se musí být napsané v C# v projektech nativní platformy.

 - V jazyce Visual Basic nelze zapsat implementace služby závislostí, se musí být napsané v C# v projektech nativní platformy.

 - XAML stránky nemůže být zahrnut v projektu jazyka Visual Basic – může vytvořit pouze generátor kódu jazyka C#. Je možné zahrnout XAML samostatný, odkazovaná, C# přenosné knihovny tříd a použití datové vazby k naplnění soubory XAML přes modely jazyka Visual Basic (příkladem je součástí [ukázka](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin nepodporuje jazyk Visual Basic.NET.

## <a name="related-links"></a>Související odkazy

- [XamarinFormsVB (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Vývoj pro různé platformy s rozhraním .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
