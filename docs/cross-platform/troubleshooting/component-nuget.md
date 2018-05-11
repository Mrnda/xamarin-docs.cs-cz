---
title: Aktualizace komponenty odkazů na NuGet
description: Nahraďte vaší součásti odkazy na balíčky NuGet pro osvědčení do budoucna aplikace.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: asb3993
ms.author: amburns
ms.date: 04/18/2018
ms.openlocfilehash: 020a5a2182458e759626b9bdbf45b62b6e13d71a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="updating-component-references-to-nuget"></a>Aktualizace komponenty odkazů na NuGet

> [!NOTE]
> Xamarin součásti již nejsou podporovány v sadě Visual Studio a by měl být nahrazen balíčky NuGet. Postupujte podle následujících pokynů ručně odebrat součást odkazy z vašich projektů.

Postupovat podle těchto pokynů pro přidání balíčků NuGet v [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) nebo [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

## <a name="manually-removing-component-references"></a>Ručním odebráním součást odkazy

V listopadu 2017 byla [oznámeno](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) , úložišti součástí Xamarin by zastaveny. Ve snaze jak urychlit sunsetting součástí 15.6 verze sady Visual Studio a 7.4 verze sady Visual Studio pro Mac už nebude podporovat komponenty ve vašem projektu. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Při načítání projektu do sady Visual Studio, zobrazí se následující dialogové okno, která vysvětluje, že je třeba odebrat všechny komponenty ze svého projektu ručně:

![Upozornění, že komponentu v projektu nebyl nalezen a musí být odstraněn, která vysvětluje dialogové okno](component-nuget-images/component-alert-vs.png)

K vyloučení komponenty ze svého projektu:

1. Otevřete **.csproj** souboru. Chcete-li to provést, klikněte pravým tlačítkem na název projektu a vyberte **uvolnit projekt**. 

2. Znovu klikněte pravým tlačítkem na odpojen projekt a vyberte **upravit {vaše projektu name} .csproj**.

3. Najít všechny odkazy v souboru na `XamarinComponentReference`. By měl vypadat podobně jako v následujícím příkladu:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. Odebrat odkazy na `XamarinComponentReference` a soubor uložte. V předchozím příkladu je bezpečné odeberte celý `ItemGroup`.

5. Jakmile se soubor byla uložena, klikněte pravým tlačítkem na název projektu a vyberte **znovu načíst projekt**.

6. Opakujte předchozí kroky pro každý projekt ve vašem řešení.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Při načítání projektu do sady Visual Studio pro Mac, zobrazí se následující dialogové okno, která vysvětluje, že je třeba odebrat všechny komponenty ze svého projektu ručně:

![Upozornění, že komponentu v projektu nebyl nalezen a musí být odstraněn, která vysvětluje dialogové okno](component-nuget-images/component-alert.png)

K vyloučení komponenty ze svého projektu:

1. Otevřete soubor .csproj. Chcete-li to provést, klikněte pravým tlačítkem na název projektu a vyberte **nástroje > Upravit soubor**.

2. Najít všechny odkazy v souboru na `XamarinComponentReference`. By měl vypadat podobně jako v následujícím příkladu:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. Odebrat odkazy na `XamarinComponentReference` a soubor uložte. V předchozím příkladu je bezpečné odeberte celý `ItemGroup`

4. Opakujte předchozí kroky pro každý projekt ve vašem řešení.

-----

> [!WARNING]
> Podle následujících pokynů pracovat pouze s starší verze sady Visual Studio.
> **Součásti** uzlu již není k dispozici v aktuální verze Visual Studio 2017 nebo Visual Studio for Mac.

Následující části popisují, jak aktualizovat existující Xamarin řešení Chcete-li změnit součást odkazy na balíčky NuGet.

- [Součásti, které obsahují balíčky NuGet](#contain)
- [Komponenty s náhrady NuGet](#replace)

Většina komponent spadat do jednoho z výše uvedených kategorií.
Pokud používáte komponenty, která vypadá to, že máte ekvivalentní balíček NuGet, přečtěte si [součásti bez cesty migrace NuGet](#require-update) části níže.

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
