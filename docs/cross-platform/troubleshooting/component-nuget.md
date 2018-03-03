---
title: "Aktualizace komponenty odkazů na NuGet"
description: "Nahraďte vaší součásti odkazy na balíčky NuGet pro osvědčení do budoucna aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f3dbfb52d4fbcb4dd65f695a862f6b041d2b22c0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="updating-component-references-to-nuget"></a>Aktualizace komponenty odkazů na NuGet

_Nahraďte vaší součásti odkazy na balíčky NuGet pro osvědčení do budoucna aplikace._

Tato příručka vysvětluje, jak aktualizovat existující Xamarin řešení Chcete-li změnit součást odkazy na balíčky NuGet.

- [Součásti, které obsahují balíčky NuGet](#contain)
- [Komponenty s náhrady NuGet](#replace)

Většina komponent spadat do jednoho z výše uvedených kategorií.
Pokud používáte komponenty, která vypadá to, že máte ekvivalentní balíček NuGet, přečtěte si [součásti bez cesty migrace NuGet](#require-update) části níže.

Odkazovat na těchto stránkách podrobnější pokyny pro přidání balíčků NuGet v [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) nebo [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

<a name="contain" />

## <a name="components-that-contain-nuget-packages"></a>Součásti, které obsahují balíčky NuGet

Celá řada komponent již obsahovat balíčků NuGet a cestu migrace je jednoduše odstranit odkaz na komponentu.

Můžete určit, zda součást již obsahuje balíček NuGet dvojitým kliknutím na komponentu v řešení:

![Uzel součástí rozšířit](component-nuget-images/solution-sml.png)

**Balíčky** karta zobrazí seznam všech balíčků NuGet, které jsou zahrnuty v součásti:

![Karta balíčky obsahuje NuGet](component-nuget-images/packages-tab-sml.png)

Všimněte si, že **sestavení** karta bude prázdný:

![Karta sestavení je prázdný](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Aktualizace řešení

Chcete-li aktualizovat vaše řešení, odstraňte **součást** položku z řešení:

![Odstranění součásti](component-nuget-images/delete-component-sml.png)

Balíček NuGet zůstane v seznamu **balíčky** uzel a aplikace bude zkompilování a spuštění jako obvykle. V budoucnu se provede aktualizace pro tento balíček pomocí **Nuget** funkce aktualizace:

![Balíček NuGet aktualizace](component-nuget-images/nuget-update-sml.png)


<a name="replace" />

## <a name="components-with-nuget-replacements"></a>Komponenty s náhrady NuGet

Pokud stránku s informacemi o součást **sestavení** karta obsahuje položky, jak je uvedeno níže, budete muset ručně najít ekvivalentní balíček NuGet.

![Obsahuje sestavení](component-nuget-images/assemblies-tab-sml.png)

Všimněte si, že **balíčky** karta bude pravděpodobně prázdný:

![](component-nuget-images/packages-tab-empty-sml.png)

_Může obsahovat závislosti NuGet, ale ty lze ignorovat._


Potvrďte náhradní NuGet balíček existuje hledání [NuGet.org](https://www.nuget.org/packages), pomocí názvu komponenty, případně autorem.

Jako příklad můžete najít oblíbených **sqlite. net pcl** balíčku tak, že:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) – Název produktu.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – profil autora.


### <a name="updating-the-solution"></a>Aktualizace řešení

Jakmile ověříte, že součást je k dispozici v NuGet, postupujte takto:

#### <a name="delete-the-component"></a>Odstranit komponentu

Klikněte pravým tlačítkem na komponentu v řešení a zvolte **odebrat**:

![Odebrat součásti](component-nuget-images/remove-component-sml.png)

Tato akce odstraní součást a všechny odkazy. Tím dojde k přerušení buildu, dokud nepřidáte ekvivalentní balíček NuGet jej nahradit.

#### <a name="add-the-nuget-package"></a>Přidejte balíček NuGet

1. Klikněte pravým tlačítkem na **balíčky** uzel a zvolte **přidat balíčky...** .
2. Hledat nahrazení NuGet podle názvu nebo Autor:

  ![](component-nuget-images/nuget-search-sml.png)

3. Stiskněte klávesu **přidejte balíček**.

Balíček NuGet se zařadí do projektu, včetně případných závislostí.
To by měl opravte sestavení. Pokud sestavení nebude dařit i nadále, zjistěte jednotlivé chyby, pokud chcete zobrazit, pokud bylo rozhraní API rozdíly mezi komponentu a balíček NuGet.

<a name="require-update" />

## <a name="components-without-a-nuget-migration-path"></a>Součásti bez cesty migrace NuGet

Nemusíte dělat starosti, Pokud nenajdete okamžitě náhradní server pro komponenty používané ve vaší aplikaci. Existující součásti bude pokračovat v práci v aplikaci Visual Studio 15,5 a **součásti** uzel se objeví ve vašem řešení jako obvykle.

Budoucí vydání sady Visual Studio, ale bude _není_ obnovit nebo aktualizovat součásti.
To znamená, pokud otevřete řešení na nový počítač, nebude možné komponentu stáhli a nainstalovali; a Autor nebude moci aktualizace se. Měli byste naplánovat:

* Rozbalte sestavení z komponenty a odkazujte na ně přímo ve vašem projektu.
* Obraťte se na autora součásti a požádejte o plánujete migrovat na NuGet.
* Prozkoumat alternativní balíčky NuGet, nebo Hledat ve zdrojovém kódu, pokud je součást open source.

Mnoho dodavatelů součásti jsou stále práce o migraci na NuGet a ostatní (včetně komerčně dostupných produktů) může být příčin možnosti Alternativní doručení.


## <a name="related-links"></a>Související odkazy

- [Instalace a použití balíčku NuGet (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Včetně balíček NuGet (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
