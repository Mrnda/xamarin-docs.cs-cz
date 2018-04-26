---
title: Přidání aplikace na Windows Phone
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 55bd4bdcfde4c91ad5c9b94bef486207466e135d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-windows-phone-app"></a>Přidání aplikace na Windows Phone


Za prvé, pokud jste použili šabloně Xamarin.Forms PCL [profil aktualizovat](~/xamarin-forms/platform/windows/installation/index.md), postupujte podle pokynů níže:

1. **Klikněte pravým tlačítkem na řešení > Přidat > Nový projekt...**  a přidejte **prázdná aplikace (Windows Phone)**

  ![](phone-images/add-wp81.png "Přidat dialogové okno Nový projekt")

2. **Klikněte pravým tlačítkem na nově vytvořený projekt > spravovat balíčky NuGet...**  a přidejte **Xamarin.Forms** balíčku.

3. **Klikněte pravým tlačítkem na projekt > Přidat > odkaz** a vytvořit odkaz na projekt na sdílený projekt aplikace Xamarin.Forms.

  ![](phone-images/addref.png "Dialogové okno Správce odkazů")

4. Upravit **App.xaml.cs** zahrnout `Init()` volání metody, `OnLaunched` metoda kolem řádku 67:

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Upravit **MainPage.xaml** -změnit kořenový element `<Page` k `<forms:WindowsPhonePage` *a* definovat `xmlns:forms` který používá:

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6 . Upravit **MainPage.xaml.cs** odebrat `: PhonePage` dědičnosti specifikátor pro název třídy.

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7 . Pořád ještě v **MainPage.xaml.cs**, přidejte `LoadApplication` volání v `MainPage` – konstruktor (kolem řádku 28) spusťte aplikaci Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Klikněte dvakrát na **Package.appxmanifest** nastavit tyto možnosti, které jsou často vyžaduje:

  * Internet (klient a Server)

9 . Nakonec přidejte místním prostředkům (např. soubory obrázků) z existující projekty platformy, které jsou požadovány.

