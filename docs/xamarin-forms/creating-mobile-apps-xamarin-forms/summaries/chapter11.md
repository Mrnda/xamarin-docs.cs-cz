---
title: Souhrn kapitoly 11. Vazbu infrastruktury
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6e0f1abf04695dfb5348b631a9fbdbd2c81bc431
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Souhrn kapitoly 11. Vazbu infrastruktury

Každý programování v C# je obeznámeni s C# *vlastnosti*. Vlastnosti obsahovat *nastavit* přistupujícího objektu nebo *získat* přistupujícího objektu. Se často nazývají *vlastnosti CLR* pro modul Common Language Runtime.

Xamarin.Forms definuje definici rozšířené vlastnosti názvem *vazbu vlastnosti* zapouzdřené pomocí [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) třídy a nepodporuje [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)třídy. Tyto třídy jsou související ale poměrně odlišné: `BindableProperty` se používá k definování samotné; vlastnosti `BindableObject` jako `object` v, že je základní třída pro třídy, které definují vazbu vlastnosti.

## <a name="the-xamarinforms-class-hierarchy"></a>Hierarchie tříd Xamarin.Forms

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) ukázce se používá k zobrazení hierarchie Xamarin.Forms třídy a předvedení zásadní roli, kterou hraje reflexe `BindableObject` v této hierarchii. `BindableObject` odvozená z `Object` a je nadřazená třída k [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) ze kterého [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) odvozena. Toto je nadřazená třída k [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) a [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), což je nadřazená třída k [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![Trojitá snímek obrazovky hierarchie tříd sdílení](images/ch11fg01-small.png "sdílení hierarchie třídy")](images/ch11fg01-large.png#lightbox "sdílení hierarchie – třída")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Funkce Náhled do BindableObject a BindableProperty

Do tříd, které jsou odvozeny od `BindableObject` mnoho vlastností CLR řečeno, aby byl "zálohovaný" vazbu vlastnosti. Například [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) vlastnost `Label` třída je vlastnost CLR, ale `Label` třída také definuje veřejné statické jen pro čtení pole s názvem [ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/) z typ `BindableProperty`.

Aplikace můžete nastavit nebo získat `Text` vlastnost `Label` normálně, nebo můžete nastavit aplikaci `Text` voláním [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metoda definované `BindableObject` s `Label.TextProperty` argument. Podobně můžete aplikaci získat hodnotu `Text` vlastnost voláním [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) metoda, znovu s `Label.TextProperty` argument. Tento postup je znázorněn pomocí [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) ukázka.

Ve skutečnosti `Text` vlastnost CLR je zcela implementovaná pomocí `SetValue` a `GetValue` metody definované `BindableObject` ve spojení s `Label.TextProperty` statickou vlastnost.

`BindableObject` a `BindableProperty` poskytují podporu pro:

- Poskytnutí výchozí hodnoty vlastností
- Ukládání jejich aktuální hodnoty
- Poskytuje mechanismus pro ověření hodnot vlastností
- Zachování konzistence mezi souvisejících vlastností do jedné třídy
- Reakce na změny vlastností
- Spouštěcí oznámení, když vlastnost změní nebo se změnila
- Podpora datová vazba
- Podpora styly
- Podpora dynamických prostředky

Vždy, když vlastnosti, která je zálohovaný díky vazbu vlastnosti změny, `BindableObject` aktivuje [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) událostí identifikující vlastnosti, která se změnila. Tato událost není aktivována, jestliže je vlastnost nastavena na stejnou hodnotu.

Některé vlastnosti není podporovaný vazbu vlastnosti a některé Xamarin.Forms třídy & #x 2014; například `Span` & #x 2014; není odvozena od `BindableObject`. Pouze třídu odvozenou z `BindableObject` může podporovat vlastnosti vazbu, protože `BindableObject` definuje `SetValue` a `GetValue` metody.

Protože `Span` není odvozena od `BindableObject`, žádné jeho vlastnosti & #x 2014; například `Text` & #x 2014; jsou zajišťované vazbu vlastnosti. To je důvod, proč `DynamicResource` nastavení na `Text` vlastnost `Span` vyvolá výjimku v [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) ukázku v předchozích kapitol. [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) příklad ukazuje, jak nastavit dynamické prostředky v kódu pomocí [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/) metoda definované `Element`. První argument je objekt typu `BindableProperty`.

Podobně [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metoda definované `BindableObject` má první argument typu `BindableProperty`.

## <a name="defining-bindable-properties"></a>Definování vlastnosti vazbu

Můžete definovat vlastní vlastnosti vazbu pomocí statické [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metoda nebo mírně kratší [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) přetížení pro vytvoření statické pole jen pro čtení typu `BindableProperty`.

Tento postup je znázorněn v [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. Třída odvozená z `Label` a umožňuje vám zadat velikost písma v bodech. Je znázorněn v [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) ukázka.

Čtyři argumenty `BindableProperty.Create` metoda jsou povinné:

- `propertyName`: textový název vlastnosti (stejný jako název vlastnosti CLR)
- `returnType`: typ CLR vlastnosti
- `declaringType`: typ třídy deklarace vlastnosti
- `defaultValue`: výchozí hodnota vlastnosti

Protože `defaultValue` je typu `object`, kompilátor musí být schopní určit typ výchozí hodnoty. Například pokud `returnType` je `double`, `defaultValue` musí být nastavena na něco podobného jako 0.0 spíše než jenom 0 nebo Neshoda typu aktivuje výjimku za běhu.

Také je velmi běžné pro vazbu vlastnosti, které chcete zahrnout:

- `propertyChanged`: statickou metodu volá se při změně vlastnosti hodnotu. První argument je instance třídy, jejichž vlastnost byl změněn.

Další argumenty, které mají `BindableProperty.Create` nejsou jako běžné:

- `defaultBindingMode`: používá ve spojení s datová vazba (jak je popsáno v [ **kapitoly 16. Datová vazba**](chapter16.md))
- `validateValue`: zpětné volání pro kontrolu platné hodnoty
- `propertyChanging`: zpětné volání k označení, když je vlastnost Chystáte se změnit
- `coerceValue`: zpětné volání pro coerce nastavte hodnotu na jinou hodnotu
- `defaultValueCreate`: zpětné volání pro vytvoření výchozí hodnoty, které nemohou být sdíleny mezi instancí třídy (například kolekce)

### <a name="the-read-only-bindable-property"></a>Vlastnost vázat jen pro čtení

Vazbu vlastnost může být jen pro čtení. Vytvoření vazbu vlastnosti jen pro čtení vyžaduje volání statickou metodu [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) nebo kratší [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) k definici privátní statické jen pro čtení pole typu [ `BindablePropertyKey` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

Poté definujte vlastnost CLR `set` accesor jako `private` k volání [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/) přetížení s `BindablePropertyKey` objektu. To brání tomu, aby vlastnost mimo třídu.

Tento postup je znázorněn v [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) třída používaná v [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) ukázka.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 11 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Ukázky kapitoly 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
