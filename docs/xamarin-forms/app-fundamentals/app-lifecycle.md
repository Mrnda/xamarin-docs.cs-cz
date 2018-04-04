---
title: Životní cyklus aplikace
description: Jak reagovat na životní cyklus aplikace
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 511591482a0e7512be34f6a210c6f44a1826be24
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="app-lifecycle"></a>Životní cyklus aplikace

`Application` Základní třída nabízí následující funkce:

* [Životní cyklus metody](#Lifecycle_Methods) `OnStart`, `OnSleep`, a `OnResume`.
* [Modální navigační události](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, a `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Životní cyklus metody

`Application` Třída obsahuje tři virtuální metody, které může být potlačena za účelem zpracování životního cyklu metody:

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

<a name="modal" />

## <a name="modal-navigation-events"></a>Modální navigační události

Existují čtyři nové události na `Application` třídy v Xamarin.Forms 1.4, každou s vlastní argumenty událostí:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** – `ModalPoppingEventArgs` třída obsahuje `Cancel` vlastnost. Když `Cancel` je nastaven na `true` modální pop byla zrušena.
* **ModalPopped** - `ModalPoppedEventArgs`

Tyto události vám pomohou lépe spravovat životním cyklu aplikací, když necháte reagovat na modální stránky se zobrazí a zavře.

> [!NOTE]
> Implementace metody životního cyklu aplikací a události modální navigace, všechny předběžné`Application` metody vytváření aplikace na platformě Xamarin.Forms (ie. aplikace napsané v verze 1.2 nebo starší, které používají statického `GetMainPage` metoda) byly aktualizovány k vytvoření výchozí `Application` který je nastaven jako nadřazeného `MainPage`.
>
> Xamarin.Forms aplikace, které používají toto chování starší verze musí být aktualizované kvůli `Application` podtřídami, jak je popsáno na [třídy aplikace](~/xamarin-forms/app-fundamentals/application-class.md) stránky.
