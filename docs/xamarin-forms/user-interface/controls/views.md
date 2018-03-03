---
title: Xamarin.Forms Views
description: "Zobrazení Xamarin.Forms jsou jako stavební bloky pro různé platformy mobilních uživatelská rozhraní."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms Views

_Zobrazení Xamarin.Forms jsou jako stavební bloky pro různé platformy mobilních uživatelská rozhraní._

<style>.tableimg {maximální šířka: none! důležité;}</style>

## <a name="views"></a>zobrazení

Xamarin.Forms používá slovo *zobrazení* označují visual objekty, jako jsou tlačítka, popisky nebo textová vstupní pole – které může být více obvykle označuje jako ovládací prvky typu pomůcky.

Tyto prvky uživatelského rozhraní se obvykle jsou podtřídami [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).

<br clear="right" />

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
Visual ovládací prvek použitý k označení, že je něco probíhající. Tento ovládací prvek vizuálně indikují dává uživateli, které něco se děje, bez informací o jejím průběhu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="Příklad ActivityIndicator" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> použitý k vykreslení plnou barevný obdélník. BoxView je užitečné stand-in pro obrázky nebo vlastní elementy při provádění počáteční při vytváření prototypu. BoxView má žádost výchozí velikost 40 x 40. Pokud potřebujete jinou velikost, přiřadit <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> a <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="Příklad BoxView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Tlačítko</a>
    </td>
    <td align="center" valign="top">
Tlačítko <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> který reaguje na touch události.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="Příklad tlačítko" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">Ovládací prvek DatePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> umožňuje výběr data. Vizuální reprezentace ovládací prvek DatePicker je velmi podobný jako jeden z <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">položka</a>kromě toho, že speciální ovládací prvek pro výběr data se zobrazí místo klávesnice </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="Ovládací prvek DatePicker příklad" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Editor</a>
    </td>
    <td valign="top">
Ovládací prvek, který můžete upravit více řádků textu. Jeden řádek položky, naleznete v části <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">položka</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="Příklad editoru" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Položka</a>
    </td>
    <td valign="top">
Ovládací prvek, který můžete upravit na jednom řádku textu. Položka je jednořádkové textové položky. Je nejvhodnější pro shromažďování diskrétní malé informace, jako jsou uživatelská jména a hesla.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="Příklad položky" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Bitové kopie</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> který obsahuje bitovou kopii.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Obrázek rozhraní API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">Práce s obrázky</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">Ukázkový zdroj</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="Příklad bitové kopie" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Popisek</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> který zobrazí text ve formátu jen pro čtení. Štítek se používá k zobrazení textových elementy, jakož i bloky více řádků textu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="Příklad popisku" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">ItemView</a> který zobrazí kolekce dat jako svislý seznam.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView rozhraní API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">Dokumentace ListView</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="Příklad ListView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> , zobrazí OpenGL obsah.
    <ul>
      <li>Funguje pouze pro iOS a Android projekty (bez podpory pro Windows Phone).
      <li>Vyžaduje odkaz na <b>OpenTK 1.0</b> sestavení v iOS a Android projekty.</li>
      <li>Nejvhodnější pro použití v sdílených projektů; Pokud se používá PCL pak DependencyService také bude požadován.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="Příklad OpenGlView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Výběr.</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> ovládacího prvku pro výběr elementu v seznamu. Vizuální reprezentace výběr je podobná <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">položka</a>, ale ovládací prvek pro výběr se zobrazí místo klávesnici.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="Příklad výběru" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> řízení označující průběh.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="ProgressBar – Ukázka třídy ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> ovládací prvek, který poskytuje vyhledávací pole.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="Příklad SearchBar" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Posuvník</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> ovládací prvek, který vstup lineární hodnotu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="Příklad posuvníku" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> ovládací prvek, který vstup diskrétní hodnoty omezené na rozsah.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="Příklad krokovač" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">přepínače</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> ovládací prvek, který poskytuje přepnuto hodnotu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="Příklad přepínačů" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> , obsahuje řádky <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">buňky</a>s.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">Zobrazení tabulka rozhraní API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">Zobrazení Tabulka dokumentace</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">Ukázkový zdroj</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="Příklad zobrazení Tabulka" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> ovládací prvek, který poskytuje výdej čas. Vizuální znázornění TimePicker je velmi podobný jako jeden z <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">položka</a>kromě toho, že speciální ovládací prvek pro výběr čas se zobrazí místo klávesnici.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="Příklad TimePicker" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">zobrazení</a> , uvede obsah HTML.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Webové zobrazení rozhraní API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">Webové zobrazení dokumentace</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">Ukázkový zdroj</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="Příklad webového zobrazení" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Související odkazy

- [Úvod k platformě Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Galerie Xamarin.Forms (ukázka)](https://developer.xamarin.com/samples/FormsGallery/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Dokumentace rozhraní API Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
