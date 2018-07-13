---
title: Souhrn kapitoly 11. Infrastruktura s podporou vazeb
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 11. Infrastruktura s podporou vazeb'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 78a0a7d4ce3a3de52f281237429cebb38f3b8e52
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998778"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Souhrn kapitoly 11. Infrastruktura s podporou vazeb

Každý programátor C# je znáte C# *vlastnosti*. Vlastnosti obsahují *nastavit* přístupového objektu a/nebo *získat* přistupujícího objektu. Jsou často volány *vlastnosti CLR* pro modul Common Language Runtime.

Xamarin.Forms definuje definici rozšířené vlastnosti volá *vázanou vlastnost* zapouzdřena objektem [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) třídy a nepodporuje [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)třídy. Tyto třídy jsou související ale úplně odlišnému: `BindableProperty` se používá k definování vlastností sama o sobě; `BindableObject` je stejná jako `object` v tom, že je základní třída pro třídy, které definují vlastnosti umožňující vazbu.

## <a name="the-xamarinforms-class-hierarchy"></a>Hierarchie tříd Xamarin.Forms

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) Ukázka používá reflexi pro zobrazení hierarchie třídy Xamarin.Forms a ukazují zásadní roli hrají `BindableObject` v této hierarchii. `BindableObject` je odvozen od `Object` a je nadřazená třída [ `Element` ](xref:Xamarin.Forms.Element) odkud [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) je odvozena. Toto je nadřazená třída [ `Page` ](xref:Xamarin.Forms.Page) a [ `View` ](xref:Xamarin.Forms.View), což je nadřazená třída [ `Layout` ](xref:Xamarin.Forms.Layout):

[![Trojitá snímek hierarchie tříd sdílení](images/ch11fg01-small.png "sdílení hierarchie třídy")](images/ch11fg01-large.png#lightbox "sdílení hierarchie tříd")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Náhled do BindableObject a BindableProperty

Ve třídách, které jsou odvozeny z `BindableObject` mnoho vlastností CLR se říká, že byl "zálohovaný" vlastnosti umožňující vazbu. Například [ `Text` ](xref:Xamarin.Forms.Label.Text) vlastnost `Label` třídy je vlastnost CLR, ale `Label` třída také definuje veřejné statické pole jen pro čtení s názvem [ `TextProperty` ](xref:Xamarin.Forms.Label.TextProperty) z typ `BindableProperty`.

Aplikace můžete nastavit nebo získat `Text` vlastnost `Label` za normálních okolností nebo aplikaci můžete nastavit `Text` voláním [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody definované `BindableObject` s `Label.TextProperty` argument. Obdobně aplikace můžete získat hodnotu `Text` vlastnost voláním [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) metoda, znovu s `Label.TextProperty` argument. To je patrné podle [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) vzorku.

Ve skutečnosti `Text` vlastnost CLR je zcela implementován pomocí `SetValue` a `GetValue` metody definované `BindableObject` ve spojení s `Label.TextProperty` statickou vlastnost.

`BindableObject` a `BindableProperty` poskytovat podporu pro:

- Poskytuje výchozí hodnoty vlastnosti
- Ukládání jejich aktuálními hodnotami
- Poskytuje mechanismus pro ověřování hodnoty vlastností
- Zachování konzistence mezi související vlastnosti do jedné třídy
- Reagování na změny vlastností
- Aktivuje oznámení, když se chystá Změna vlastnosti nebo došlo ke změně
- Podpora vytváření datových vazeb
- Podpora styly
- Podpora dynamických prostředky

Pokaždé, když vlastnost, která je založená na vlastnost s vazbou změny, `BindableObject` aktivuje [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) událostí identifikující vlastnosti, které se změnily. Tato událost není vyvolána, když je nastavena na stejnou hodnotu.

Některé vlastnosti nejsou zajištěné vlastnosti umožňující vazbu a některé třídy Xamarin.Forms &mdash; například `Span` &mdash; není odvozen od `BindableObject`. Pouze třída odvozená z `BindableObject` může podporovat vlastnosti umožňující vazbu, protože `BindableObject` definuje `SetValue` a `GetValue` metody.

Protože `Span` není odvozen od `BindableObject`, žádný z její vlastnosti &mdash; například `Text` &mdash; se zálohují na vlastnost s vazbou. To je důvod, proč `DynamicResource` nastavení `Text` vlastnost `Span` vyvolá výjimku v [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) ukázku v předchozích kapitol. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) Ukázka předvádí, jak nastavit dynamické prostředky v kódu pomocí [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) metody definované `Element`. První argument je objekt typu `BindableProperty`.

Podobně [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metody definované `BindableObject` má první argument typu `BindableProperty`.

## <a name="defining-bindable-properties"></a>Definování vlastnosti umožňující vazbu

Můžete definovat vlastní vlastnosti umožňující vazbu pomocí statické [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metodu pro vytvoření statické pole jen pro čtení typu `BindableProperty`.

To je patrné [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. Třída je odvozena z `Label` a umožňuje určit velikost písma v bodech. To je ukázáno v [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) vzorku.

Čtyři argumenty `BindableProperty.Create` metody jsou požadovány:

- `propertyName`: textový název vlastnosti (stejný jako název vlastnosti CLR)
- `returnType`: typ vlastnosti CLR
- `declaringType`: typ třídy deklarující vlastnost
- `defaultValue`: výchozí hodnota vlastnosti

Protože `defaultValue` je typu `object`, kompilátor musí být schopní určit typ výchozí hodnoty. Například pokud `returnType` je `double`, `defaultValue` musí být nastavená na něco jako 0.0 spíše než jenom 0 nebo Neshoda typu se aktivuje výjimku za běhu.

Je také velmi běžné, že vlastnost s vazbou zahrnout:

- `propertyChanged`: statická metoda volá se při změně hodnoty vlastnosti. První argument je instance třídy, jehož vlastnost se změnil.

Další argumenty, které mají `BindableProperty.Create` nejsou jako běžné:

- `defaultBindingMode`: používá v souvislosti s datovou vazbu (jak je popsáno v [ **kapitola 16. Vytváření datových vazeb**](chapter16.md))
- `validateValue`: zpětné volání pro kontrolu platné hodnoty
- `propertyChanging`: zpětné volání k označení, když vlastnost se chystá změna
- `coerceValue`: zpětné volání k vynucení nastavit hodnotu s jinou hodnotou
- `defaultValueCreate`: zpětné volání pro vytvoření výchozí hodnotu, která se nedají sdílet mezi instancemi třídy (například kolekce)

### <a name="the-read-only-bindable-property"></a>Vlastnost jen pro čtení s možností vazby

Vázanou vlastnost může být jen pro čtení. Vytvoření s možností vazby vlastnosti jen pro čtení vyžaduje volání statické metody [ `BindableProperty.CreateReadOnly` ](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) k definování privátní statické pole jen pro čtení typu [ `BindablePropertyKey` ](xref:Xamarin.Forms.BindablePropertyKey).

Poté definujte vlastnost CLR `set` accesor jako `private` volat [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) přetížení s `BindablePropertyKey` objektu. To zabrání tomu, aby vlastnost mimo třídu.

To je patrné [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) třída používaná v [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) vzorku.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 11 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Ukázky kapitoly 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
