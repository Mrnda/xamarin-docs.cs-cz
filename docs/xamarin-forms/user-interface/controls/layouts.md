---
title: "Rozložení Xamarin.Forms"
description: "Rozložení Xamarin.Forms se používají k uspořádání ovládacích prvků uživatelského rozhraní do logické struktury."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Rozložení Xamarin.Forms

_Rozložení Xamarin.Forms se používají k uspořádání ovládacích prvků uživatelského rozhraní do logické struktury._

<style>.tableimg {maximální šířka: none! důležité;}</style>

## <a name="layouts"></a>Rozložení

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Třídy v Xamarin.Forms je specializované podtypem zobrazení, která slouží jako kontejner pro ostatní rozložení nebo zobrazení. Obvykle obsahuje logiku a nastavit umístění a velikost podřízených elementů v aplikacích Xamarin.Forms.

 [ ![](layouts-images/layouts-sml.png "Typy rozložení Xamarin.Forms")](layouts-images/layouts.png "typy rozložení Xamarin.Forms")

<br clear="all" />

Xamarin.Forms podporuje:

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Typ</strong>
    </th>
    <th>
      <strong>Popis</strong>
    </th>
    <th style="min-width:400px">
      <strong>snímek obrazovky</strong>
    </th>
  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
Rozložení správce pro šablony zobrazení. V rámci používají <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a> k označení, kde se zobrazí obsah předkládané.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="Příklad ContentPresenter" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
Element s obsahem jeden. ContentView má velmi malé použijte své vlastní. Jeho účelem je sloužit jako základní třída pro uživatelem definované složené zobrazení.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="Příklad ContentView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Rámce</a>
    </td>
    <td valign="top">
Element obsahující jednu podřízenou s některé možnosti rámcovacích. Rámce mají výchozí <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> 20.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="Příklad rámce" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
Element schopná posouvání, pokud je obsah vyžaduje.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="Příklad ScrollView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
Element, který zobrazí obsah pomocí šablony ovládacího prvku a základní třídu pro <a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="Příklad TemplatedView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
Podřízené elementy se umístí na absolutní požadované umístění. Uživatel s přiřazenou kotvy a meze definuje pozice a velikosti ovládacího prvku.
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="Příklad AbsoluteLayout" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Mřížky</a>
    </td>
    <td valign="top">
Rozložení obsahující zobrazení uspořádané řádků a sloupců.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="Příklad mřížky" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">rozložení</a> používající <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">omezení</a>s rozložení své podřízené objekty.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="Příklad RelativeLayout" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">rozložení</a> který umisťuje podřízených elementů na jediném řádku, což může být orientované vodorovně nebo svisle. Toto rozložení podřízených hranice automaticky nastaví během cyklu rozložení. Uživatel s přiřazenou hranice budou přepsány a proto by neměl být nastavený na podřízený element uživatelem.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="Příklad StackLayout" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Galerie Xamarin.Forms (ukázka)](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
