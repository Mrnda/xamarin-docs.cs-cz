---
title: Náhled XAML pro Xamarin.Forms
description: Tento článek vysvětluje, jak použít prohlížeč XAML a zkontrolujte vaše Xamarin.Forms rozložení vykresluje během psaní. Náhled XAML je k dispozici v Visual Studio 2017 a Visual Studio for Mac.
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
ms.openlocfilehash: 25c8e1a34f8be5ab2f8491e75fa5aac470d55bc8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245856"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Náhled XAML pro Xamarin.Forms

_V tématu vaše Xamarin.Forms rozložení se vykresluje jako zadáte!_

## <a name="requirements"></a>Požadavky

Projekty vyžadovat nejnovější balíček Xamarin.Forms NuGet pro prohlížeč XAML pro práci. Zobrazení náhledu aplikace pro Android vyžaduje [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Další informace naleznete v [poznámky k verzi](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## <a name="getting-started"></a>Začínáme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Použití **zobrazení > ostatní okna > Náhled Xamarin.Forms** nabídky v sadě Visual Studio otevřete okno náhledu. Použití **okno > nové svislém kartě skupiny** nabídky na místo, vedle sebe.

[![Náhled ovládacího prvku ListView v sadě Visual Studio](xaml-previewer-images/xamlp-list-vs-sml.png "prohlížeč formulářů v sadě Visual Studio")](xaml-previewer-images/xamlp-list-vs.png#lightbox "prohlížeč formulářů v sadě Visual Studio")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**Preview** tlačítko lze zobrazit v editoru pravým tlačítkem myši na soubor XAML a výběrem **otevřít v > Náhled Forms dokumentu**. V podokně náhledu pak můžete zobrazen nebo skryt stisknutím **Preview** tlačítko v pravém horním rohu okna dokumentu žádné XAML:

[![Náhled ovládacího prvku ListView v sadě Visual Studio pro Mac](xaml-previewer-images/xamlp-list-sml.png "prohlížeč formulářů v sadě Visual Studio pro Mac")](xaml-previewer-images/xamlp-list.png#lightbox "prohlížeč formulářů v sadě Visual Studio pro Mac")

-----

## <a name="xaml-preview-options"></a>Možnosti Preview XAML

Možnosti v horní části podokna náhledu jsou:

* **Phone** – vykreslení na obrazovce phone velikost
* **Tablet** – vykreslení na obrazovce tablet velikost (Všimněte si, existují ovládací prvky zvětšení v pravém dolním podokně)
* **Android** – zobrazit verzi systému Android obrazovky
* **iOS** – zobrazit verze iOS na obrazovce
* Portait (ikona) – používá orientaci na výšku v verzi Preview.
* Na šířku (ikona) – používá šířku v verzi Preview.

## <a name="adding-design-time-data"></a>Přidání dat návrhu

Některá rozložení může být obtížné vizualizovat bez jakékoli data vázaná na ovládacích prvků uživatelského rozhraní. Chcete-li ve verzi preview užitečnější, zařadit některé statických dat k ovládacím prvkům hardcoding kontext vazby, (buď v modelu code-behind nebo pomocí XAML).

Odkazovat na James Montemagno [příspěvku na blogu o přidání dat návrhu](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) chcete zjistit, jak vytvořit vazbu na statické ViewModel v jazyce XAML.

## <a name="detecting-design-mode"></a>Zjišťování režimu návrhu

Statické [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) vlastnost můžete prověřit, abyste zjistili, zda je aplikace spuštěna v náhledu. To umožňuje zadat kód, který spustí, pouze když je aplikace spuštěna v náhledu:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>Poradce při potížích

Zkontrolujte níže, problémy a [Xamarin fóra](https://forums.xamarin.com/categories/xamarin-forms), pokud dojde k potížím.

### <a name="xaml-preview-isnt-showing"></a>XAML Preview není zobrazen.

Zkontrolujte následující:

* Projekt by měly být vytvořeny (kompilované) před pokusem o náhled souborů XAML.
* Návrhář agenta musí být nastavení při prvním náhled souboru XAML – indikátor průběhu se zobrazí v náhledu, společně s zprávy o průběhu, dokud nebude připraven.
* Zkuste ukončit a znovu otevírání souboru XAML.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>Neplatný jazyk XAML: Projektu pro Android musí vytvořené před vytvořením preview

Náhled XAML vyžaduje, aby projektu vytvořeny před vykreslením stránky.
Pokud chybová zpráva se zobrazí v horní části podokna náhledu, znovu sestavte aplikaci a zkuste to znovu.

![Chybová zpráva: nejprve musí být vytvořená projektu](xaml-previewer-images/error-not-built-sml.png "chybová zpráva: projekt znovu sestavte")
