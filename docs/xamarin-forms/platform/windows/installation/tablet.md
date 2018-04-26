---
title: Přidání aplikace pro Windows
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0d2ef44896c9352776443c2fec318d40d27d7539
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-windows-app"></a>Přidání aplikace pro Windows


1. **Klikněte pravým tlačítkem na řešení > Přidat > Nový projekt...**  a přidejte **prázdná aplikace (Windows)**

 ![](tablet-images/add-wu.png "Přidat dialogové okno Nový projekt")

2. **Klikněte pravým tlačítkem na projekt > spravovat balíčky NuGet...**  a přidejte **Xamarin.Forms** balíčku.

3. **Klikněte pravým tlačítkem na projekt > Přidat > odkaz** a vytvořit odkaz na projekt na sdílený projekt aplikace Xamarin.Forms.

  ![](tablet-images/addref.png "Dialogové okno Správce odkazů")

4. Upravit **App.xaml.cs** zahrnout `Init()` volání metody uvnitř `OnLaunched` kolem řádku 65 – metoda

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Upravit **MainPage.xaml** -změnit kořenový element `<Page` k `<forms:WindowsPage` *a* definovat `xmlns:forms` který používá:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . Upravit **MainPage.xaml.cs** odebrat `: Page` dědičnosti specifikátor pro název třídy.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . Pořád ještě v **MainPage.xaml.cs**, přidejte `LoadApplication` volání v `MainPage` – konstruktor (kolem řádku 28) spusťte aplikaci Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Klikněte dvakrát na **Package.appxmanifest** nastavit tyto možnosti, které jsou často vyžaduje:

  Nastavte možnosti:

  * Internet (klient)
  * Umístění

9 . Nakonec přidejte místním prostředkům (např. soubory obrázků) z existující projekty platformy, které jsou požadovány.

