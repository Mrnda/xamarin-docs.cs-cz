---
title: Principy vzorku
description: Toto téma obsahuje návod ukázkovou aplikaci Xamarin.Forms, která ukazuje, jak komunikovat s jinou webové služby. Při každém webová služba používá samostatné ukázkové aplikace, jsou funkčně podobné a sdílejí společné třídy.
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 703441e3fc58beeb33e519f3781387a59c1c1cef
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="understanding-the-sample"></a>Principy vzorku

_Toto téma obsahuje návod ukázkovou aplikaci Xamarin.Forms, která ukazuje, jak komunikovat s jinou webové služby. Při každém webová služba používá samostatné ukázkové aplikace, jsou funkčně podobné a sdílejí společné třídy._

Ukázkovou aplikaci seznamu úkolů popsané níže slouží k ukazují, jak pro přístup k různé typy webové služby back-EndY s Xamarin.Forms. Poskytuje funkce:

- Zobrazení seznamu úloh.
- Přidání, úpravy a odstraňování úkolů.
- Nastavte stav úkolu, udělat'.
- Číst obsah pole název a poznámky k úkolu.

Ve všech případech úlohy jsou uloženy v back-end, které je přístupné přes webové služby.

Při spuštění aplikace se zobrazí na stránce, které jsou uvedeny všechny úlohy načíst z webové služby a umožňuje uživateli vytvořit novou úlohu. Kliknutím na úlohu přejde na aplikaci na druhé stránce, kde úlohy lze upravit, uložit, odstranit a používá se. Konečné aplikace je zobrazena níže:

![](walkthrough-images/app-example-1.png "Aplikace todo – první stránka")
![](walkthrough-images/app-example-2.png "aplikace Todo – druhé stránce")

V každém tématu v této příručce najdete odkaz ke stažení *různých* verze aplikace, která demonstruje konkrétní typ služby webové služby back-end. Stáhněte si relevantní ukázkový kód na stránce týkající se každý styl webové služby.

## <a name="understanding-the-application-anatomy"></a>Principy Anatomy aplikace

Projekt PCL pro každou ukázkovou aplikaci zahrnuje tři hlavní složky:

|Folder|Účel|
|--- |--- |
|Data|Obsahuje třídy a rozhraní používá ke správě datové položky a komunikovat s webovou službou. Minimálně to zahrnuje `TodoItemManager` třídy, která je k dispozici prostřednictvím na vlastnost ve `App` třída k vyvolání operace webové služby.|
|Modely|Obsahuje třídy modelu dat pro aplikaci. Minimálně to zahrnuje `TodoItem` třídy, která modelů jednu položku dat používá aplikace. Složka může zahrnovat žádné další třídy používané k uživatelským datům modelu.|
|zobrazení|Obsahuje stránky pro aplikaci. To obvykle se skládá z `TodoListPage` a `TodoItemPage` třídy a všechny další třídy používané pro účely ověření.|

Projekt PCL pro každou aplikaci se skládá z několika důležitých souborů:

|Soubor|Účel|
|--- |--- |
|Constants.cs|`Constants` Třídy, která určuje všechny konstanty aplikace používá ke komunikaci s webovou službou. Tyto konstanty vyžadují aktualizaci k přístupu ke službě back-end osobní vytvořena na zprostředkovatele.|
|ITextToSpeech.cs|`ITextToSpeech` Rozhraní, které určuje, že `Speak` metoda musí být poskytnuta žádnou implementující třídu.|
|Todo.cs|`App` Třídu, která zodpovídá za vytvoření instance obou na první stránku, která se zobrazí v aplikaci na každou platformu, a `TodoItemManager` třídu, která se použije k vyvolání operace webové služby.|

### <a name="viewing-pages"></a>Zobrazení stránky

Většina ukázkové aplikace obsahovat alespoň dvě stránky:

- **TodoListPage** – Tato stránka zobrazuje seznam `TodoItem` instancí a ikona značek Pokud `TodoItem.Done` vlastnost je `true`. Kliknutím na položku přejde `TodoItemPage`. Kromě toho můžete kliknutím na vytvořit nové položky *+* symbol.
- **TodoItemPage** – Tato stránka zobrazí podrobnosti pro vybranou `TodoItem`a umožňuje upravovat, uložit, odstranit a používá se.

Kromě toho některé ukázkové aplikace obsahují další stránky, které se používají ke správě procesu ověřování uživatele.

### <a name="modeling-the-data"></a>Modelování dat

Každý ukázková aplikace používá `TodoItem` třídy modelu data, která se zobrazí a odesílá na webovou službu pro úložiště. Následující příklad kódu ukazuje `TodoItem` třídy:

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

`ID` Vlastnost slouží k jednoznačné identifikaci jednotlivých `TodoItem` instance a webovou službou slouží k identifikaci dat aktualizovat ani odstranit.

### <a name="invoking-web-service-operations"></a>Vyvolání operace webové služby

Operace webové služby jsou přístupné prostřednictvím `TodoItemManager` třídy a instance třídy je možné přistupovat prostřednictvím `App.TodoManager` vlastnost. `TodoItemManager` Třída poskytuje následující metody k vyvolání operace webové služby:

- **GetTasksAsync** – tato metoda se používá k naplnění `ListView` řízení na `TodoListPage` s `TodoItem` instance načítají webovou službu.
- **SaveTaskAsync** – tato metoda se používá k vytvoření nebo aktualizaci `TodoItem` instance na webové službě.
- **DeleteTaskAsync** – tato metoda se používá k odstranění `TodoItem` instance na webové službě.

Kromě toho některé ukázkové aplikace obsahují další metody v `TodoItemManager` třídy, která se používají ke správě procesu ověřování uživatele.

Ne přímo, vyvolání operace webové služby `TodoItemManager` metody vyvolání metody pro třídu závislé, která je vloženy do `TodoItemManager` konstruktor. Například jeden ukázkovou aplikaci vloží `RestService` třídy do `TodoItemManager` konstruktor k zajištění implementace, který používá rozhraní REST API pro přístup k datům.

### <a name="translating-text-to-speech"></a>Převod textu na řeč překladu

Většina ukázkové aplikace obsahují funkce pro převod textu na ŘEČ mluvit hodnoty `TodoItem.Name` a `TodoItem.Notes` vlastnosti. To lze provést `OnSpeakActivated` obslužné rutiny událostí v `TodoItemPage` třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

Tato metoda jednoduše volá `Speak` metodu, která je implementována ve specifické platformy `Speech` třídy. Každý `Speech` třída implementuje `ITextToSpeech` rozhraní, a kód specifické pro platformu spuštění vytvoří instanci `Speech` třídu, která je přístupná prostřednictvím `App.Speech` vlastnost.

## <a name="summary"></a>Souhrn

Toto téma poskytuje návod ukázkovou aplikaci Xamarin.Forms, která se používá k ukazují, jak komunikovat s jinou webové služby. Při každém webová služba používá samostatné ukázkové aplikace, jsou všechny založené na stejné uživatelské rozhraní a obchodní logiku jak bylo popsáno výše – pouze mechanismus úložiště dat pro sadu web service se liší.


## <a name="related-links"></a>Související odkazy

- [Verze ASMX (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [Verze WCF (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [Verze REST (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Verze Azure (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
