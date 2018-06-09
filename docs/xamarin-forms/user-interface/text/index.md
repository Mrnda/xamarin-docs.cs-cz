---
title: Text v Xamarin.Forms
description: Xamarin.Forms má tři primární zobrazení pro práci s textem a tento článek vysvětluje, jakým způsobem je použít k zadání a zobrazení textu v Xamarin.Forms aplikace.
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: c5bd157299c9388b561f316e65f2ba290bd15224
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244984"
---
# <a name="text-in-xamarinforms"></a>Text v Xamarin.Forms

_Zadejte nebo zobrazit text pomocí Xamarin.Forms._

Xamarin.Forms má tři primární zobrazení pro práci s textem:

- **[Popisek](#Label)**  &mdash; pro prezentování jednoho nebo více řádků textu. Může zobrazit text v několika variantách, formátování na stejném řádku.
- **[Položka](#Entry)**  &mdash; pro zadávání textu, který je pouze jeden řádek. Položka má režim heslo.
- **[Editor](#Editor)**  &mdash; pro zadávání textu, který může trvat víc než jeden řádek.

Vzhled textu se dá změnit pomocí předdefinovaných nebo vlastní [styly](#Styles) a některé ovládací prvky podporují vlastní [písem](#Fonts).

<a name="Label" />

## <a name="labellabelmd"></a>[Popisek](label.md)

`Label` Zobrazení se používá k zobrazení textu. Může zobrazit více řádků textu nebo na jednom řádku textu. `Label` může představovat více možnosti formátování, které jsou používány vložený text. Popisek zobrazení můžete zabalit nebo zkrátit text, když nemůže vejít na jeden řádek.

![](images/label.png "Příklad popisku")

Najdete v článku [popisek](label.md) článku, kde naleznete podrobnější informace.

Informace o přizpůsobení písmo použité v štítek, najdete v části [písem](fonts.md).

<a name="Entry" />

## <a name="entryentrymd"></a>[Položka](entry.md)

`Entry` slouží k přijmout textových vstup. `Entry` nabízí možnost ovládat barvy, ale nemůže mít přizpůsobit písma. `Entry` má režim heslo a můžete zobrazit zástupný text, dokud nebude zadán text.

![](images/entry.png "Příklad položky")

Najdete v článku [položka](entry.md) Další informace najdete v článku.

Všimněte si, že na rozdíl od `Label`, `Entry` nemůže mít nastavení vlastní písma.

<a name="Editor" />

## <a name="editoreditormd"></a>[Editor](editor.md)

`Editor` se používá k přijetí Víceřádkový textový vstup. `Editor` může mít vlastní barvu pozadí, ale barvy a nelze je měnit písma.

![](images/editor.png "Příklad editoru")

Najdete v článku [Editor](editor.md) Další informace najdete v článku.

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[Písma](fonts.md)

`Label` Řízení podporuje nastavení jiné písmo pomocí předdefinovaných písem na každou platformu nebo vlastní písma, které jsou součástí vaší aplikace. Najdete v článku [písem](fonts.md) článku, kde naleznete podrobnější informace.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[Styly](styles.md)

Odkazovat na [práce se styly](~/xamarin-forms/user-interface/styles/index.md) se dozvíte, jak nastavit písmo [barva](~/xamarin-forms/user-interface/colors.md)a dalších vlastností zobrazení, které platí mezi více ovládacích prvků.



## <a name="related-links"></a>Související odkazy

- [Text (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
