---
title: Životní cyklus aplikace Xamarin.Forms
description: Tento článek vysvětluje, jak reagovat na životního cyklu aplikací, včetně metody životního cyklu, stránka navigační události a události modální navigace.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: fb651494b63a77ede47dd246ee054b5c67af2a35
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240266"
---
# <a name="xamarinforms-app-lifecycle"></a>Životní cyklus aplikace Xamarin.Forms

[ `Application` ](xref:Xamarin.Forms.Application) Základní třída nabízí následující funkce:

* [Životní cyklus metody](#Lifecycle_Methods) `OnStart`, `OnSleep`, a `OnResume`.
* [Stránka události navigace](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing).
* [Modální navigační události](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, a `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Životní cyklus metody

[ `Application` ](xref:Xamarin.Forms.Application) Třída obsahuje tři virtuální metody, které může být potlačena za účelem zpracování životního cyklu metody:

* **OnStart** -volat při spuštění aplikace.

* **OnSleep** -volá se pokaždé, když aplikace přejde na pozadí.

* **OnResume** -volána, když se obnoví aplikaci, po odesílána na pozadí.

Všimněte si, že je *žádné* metoda pro ukončení aplikace.
Za normálních okolností (ie. není havárie) ukončení aplikace se stane z *OnSleep* stavu, bez jakékoli další oznámení do vašeho kódu.

Chcete-li sledovat, kdy jsou volány těchto metod, implementujte `WriteLine` volání v každé (jak je znázorněno níže) a testování na každou platformu.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

Při aktualizaci *starší* Xamarin.Forms aplikace (např. vytvořit s Xamarin.Forms 1.3 nebo starší), ujistěte se, že Android hlavní aktivitu zahrnuje `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` v `[Activity()]` atribut. Pokud není zadán, bude sledovat `OnStart` metoda je volána na otočení i při prvním spuštění aplikace. Tento atribut je automaticky součástí aktuální šablony aplikaci Xamarin.Forms.

<a name="page" />

## <a name="page-navigation-events"></a>Navigace stránky události

Existují dvě události na [ `Application` ](xref:Xamarin.Forms.Application) třídu, která poskytují oznámení stránek, které jsou uvedeny a jindy mizí:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -vyvolá, když na stránce se zobrazí na obrazovce.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -vyvolá, když na stránce je zmizí z obrazovky.

Tyto události můžete použít ve scénářích, kde chcete sledovat stránky, jako jsou uvedené na obrazovce.

> [!NOTE]
> [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) a [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) události jsou vyvolány z [ `Page` ](xref:Xamarin.Forms.Page) základní třída ihned po [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) a [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) událostí, v uvedeném pořadí.

<a name="modal" />

## <a name="modal-navigation-events"></a>Modální navigační události

Existují čtyři události na [ `Application` ](xref:Xamarin.Forms.Application) třída, každou s vlastní argumenty událostí, které vám umožní reagovat na modální stránky se zobrazí a zavře:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** – `ModalPoppingEventArgs` třída obsahuje `Cancel` vlastnost. Když `Cancel` je nastaven na `true` modální pop byla zrušena.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Implementace metody životního cyklu aplikací a události modální navigace, všechny předběžné`Application` metody vytváření aplikace na platformě Xamarin.Forms (ie. aplikace napsané v verze 1.2 nebo starší, které používají statického `GetMainPage` metoda) byly aktualizovány k vytvoření výchozí `Application` který je nastaven jako nadřazeného `MainPage`.
>
> Xamarin.Forms aplikace, které používají toto chování starší verze musí být aktualizované kvůli `Application` podtřídami, jak je popsáno na [třídy aplikace](~/xamarin-forms/app-fundamentals/application-class.md) stránky.
