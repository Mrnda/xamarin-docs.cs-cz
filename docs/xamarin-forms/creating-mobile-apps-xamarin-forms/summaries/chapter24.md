---
title: Shrnutí kapitoly 24. Navigace stránky
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9d1a226a4532b745fddee28e943562fb51d34e65
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-24-page-navigation"></a>Shrnutí kapitoly 24. Navigace stránky

Mnoho aplikací se skládá z více stránek, mezi které uživatel přejde. Aplikace vždy má *hlavní* stránky nebo *domácí* stránky, a z ní uživatel přejde na jiné stránky, které se udržují v zásobníku pro navigaci zpět. Další navigační možnosti jsou popsané v [ **kapitoly 25. Stránka typy**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Stránky modální a nemodální stránky

`VisualElement` definuje [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) vlastnost typu [ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/), což zahrnuje následující dvě metody přejít na novou stránku:

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

Obě metody přijímají `Page` instance jako argument a vraťte se `Task` objektu. Následující dvě metody přejděte zpět na předchozí stránce:

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

Pokud uživatelské rozhraní má svou vlastní **zpět** tlačítko (stejně jako Android a Windows Phone), pak není nutné pro aplikace pro volání těchto metod.

I když tyto metody jsou dostupné z libovolného `VisualElement`, obecně se nazývají z `Navigation` vlastnost aktuálního `Page` instance.

Aplikace obecně používat modální stránky, když uživatel je potřeba k poskytování určité informace na stránce před vrácením na předchozí stránku. Stránky, které nejsou modální se někdy označují jako nemodální nebo *hierarchické*. Nic v samotné stránce odlišuje jej jako modální nebo nemodální; ho se řídí místo toho metodu použitou k přejděte k němu. Postup pro všechny platformy musí modální stránky přejdete zpět na předchozí stránce zadat své vlastní uživatelské rozhraní.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) ukázka umožňuje prozkoumat rozdíl mezi modální a nemodální stránky. Všechny aplikace, která používá navigaci na stránce musí projít jeho domovskou stránku [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) konstruktoru, obvykle v programu `App` třídy. Jeden bonusové je, že už muset nastavit `Padding` na stránce pro iOS.

Zjistíte, pro nemodální stránky, stránky na [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) se zobrazí vlastnosti. IOS, Android a platformy tablet a vzdálené ploše systému Windows zadejte přejděte zpět na předchozí stránce jako element uživatelského rozhraní. Kurz, Android a Windows phone zařízení mají standard **zpět** tlačítko přejdete zpět.

Pro modální stránky, stránky `Title` nezobrazí, a je k dispozici žádné elementy uživatelského rozhraní se vrátíte k předchozí stránce. Přestože je možné použít u standardního Android a Windows phone **zpět** tlačítko vrátit na předchozí stránku modální stránky na jiných platformách musí vytvořit vlastní mechanismus pro přejděte zpět.

### <a name="animated-page-transitions"></a>Přechody animovaný stránek

Jsou k dispozici alternativní verze různé metody navigační s druhým argumentem logická hodnota, kterou můžete nastavit na `true` Pokud chcete zahrnout animace přechod stránky:

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

Ale zahrnují standardní navigaci na stránce metody animace ve výchozím nastavení, tak, aby byly jenom vhodné při navigaci na konkrétní stránku při spuštění (jak je popsáno na konci této kapitoly), nebo pokud poskytuje vlastní animace vstupu (jak je popsáno v [ **Chapter22. Animace**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Varianty Visual a funkční

`NavigationPage` zahrnuje dvě vlastnosti, které můžete nastavit při vytvořit instanci třídy ve vaší `App` metoda:

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` také obsahuje čtyři přidružené vazbu vlastnosti, které ovlivňují konkrétní stránky, na kterém jsou nastavené:

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) A [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) A [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) a [ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/) pracovní na jenom iOS
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) a [ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/) pracovní na iOS a Android pouze

### <a name="exploring-the-mechanics"></a>Zkoumání mechanismů

Jsou všechny asynchronní metody navigace stránky a musí být použit s `await`. Dokončení nebude znamenat, že navigaci na stránce byl dokončen, ale pouze, je bezpečné prozkoumat zásobník navigaci na stránce.

Když jednu stránku přejde na jiný, první stránka obecně získá volání jeho [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) metoda a druhé stránce získá volání jeho [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) metoda. Podobně když se vrátí jednu stránku do jiného, na první stránku získá volání jeho `OnDisappearing` metoda a druhé stránce obecně získá volání jeho `OnAppearing` metoda. Pořadí těchto volání (a dokončení asynchronní metody, která volá navigaci) je platforma závislé. Použití aplikace word "obecně" ve dvou předchozích příkazech je z důvodu Android navigační modální page, ve kterém těchto volání metod nedojde.

Navíc volá, aby se `OnAppearing` a `OnDisappearing` metody neoznačují nutně navigaci na stránce.

`INavigation` Rozhraní obsahuje dvě vlastnosti kolekce, které vám umožní prozkoumat navigační zásobník:

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) typu `IReadOnlyList<Page>` pro nemodální zásobníku
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) typu `IReadOnlyList<Page>` pro modální zásobníku

Je nejbezpečnější pro přístup z těchto zásobníky `Navigation` vlastnost `NavigationPage` (hodnota by měla být `App` třídy [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) vlastnost). Jenom je bezpečné Zkontrolujte tyto zásobníky po dokončení metody asynchronní navigaci na stránce. [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/) Vlastnost `NavigationPage` neindikuje aktuální stránku Pokud aktuální stránku modální stránka, ale označuje místo poslední nemodální stránky.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) ukázka umožňuje prozkoumat navigaci na stránce a zásobníky a právní typy navigace stránky:

- Nemodální stránky můžete přejít na jinou stránku nemodální nebo modální stránky
- Modální stránky můžete přejít jenom na jinou stránku modální

### <a name="enforcing-modality"></a>Způsob vynucení

Aplikace používá modální stránky, když je potřeba získat některé informace od uživatele. Uživatel by mělo být zakázáno vrácení na předchozí stránku, dokud nebude poskytnuta tyto informace. V systému iOS, je snadné k poskytování **zpět** tlačítko a povolte ji jenom v případě, že uživatel dokončí se na stránku. Pro zařízení Android a Windows phone, by měly přepsat aplikaci, ale [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/) metodu a vrátí `true` má-li program **zpět** tlačítko samostatně, jak je předvedeno v [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) ukázka.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) příklad ukazuje, jak to funguje ve scénáři s modelem MVVM.

## <a name="navigation-variations"></a>Navigace variant

Pokud konkrétní modální stránky můžete přesměrováni do více než jednou, ho tak, aby uživatel můžete upravovat informace místo zadání musí uchovávat informace ve všech znovu. To může zpracovat zachováním konkrétní instanci modální stránky, ale je vhodnější (zejména v systému iOS) se zachovávají informace v zobrazení modelu.

### <a name="making-a-navigation-menu"></a>Vytvoření navigační nabídky

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) příklad ukazuje použití `TableView` do seznamu položek nabídky. Každá položka je přidružena `Type` objekt pro konkrétní stránky. Pokud je vybraná tato položka, program vytvoří stránky a přejde k němu.

[![Trojitá snímek obrazovky zobrazení galerie typ](images/ch24fg21-small.png "položky nabídky výpis zobrazení Tabulka")](images/ch24fg21-large.png#lightbox "zobrazení Tabulka výpis položky nabídky")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) ukázka je mírně liší v nabídce obsahuje instance každé stránky, nikoli typy. To pomůže uchovávat informace z každé stránce, ale všechny stránky musí být vytvořena instance při spuštění programu.

### <a name="manipulating-the-navigation-stack"></a>Manipulace s navigační zásobníku

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) ukazuje několik funkcí, které jsou definované `INavigation` které umožňují pracovat s zásobníku navigační strukturovaných způsobem:

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) a [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/) s volitelné animace

### <a name="dynamic-page-generation"></a>Generování dynamických stránky

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) příklad znázorňuje vytváření stránky v době běhu založené na vstup uživatele.

## <a name="patterns-of-data-transfer"></a>Vzory přenosu dat

Často je nutné sdílet data mezi stránkami &mdash; k přenosu dat navigated stránky a stránky vrátit data na stránku, která je volána. Existuje několik postupů tohoto postupu.

### <a name="constructor-arguments"></a>Argumenty konstruktoru

Při přechodu na novou stránku, je možné vytvořit instanci třídy stránky s argumentem konstruktor, který umožňuje vlastní inicializaci stránky. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) příklad znázorňuje to. Je také možné navigated stránky tak, aby měl jeho `BindingContext` nastavit podle stránky, který odkazuje na ni.

### <a name="properties-and-method-calls"></a>Vlastnosti a volání metod

Příklady zbývajících přenos dat zkoumat problém předávání informací mezi stránkami, když jednu stránku přejde na jinou stránku a zpět. Tyto diskuze *domácí* stránky přejde na *informace* stránky a musíte přenést inicializovaného informace, které *informace o* stránky. *Informace* stránky získá další informace od uživatele a přenáší informace, které *domácí* stránky.

*Domácí* stránky snadno dostanete veřejné metody a vlastnosti ve *informace* stránka při vytvoření instance této stránce. *Informace* stránka můžete také přistupovat k veřejné metody a vlastnosti ve *domácí* stránky, ale volba vhodná doba na to může být složité. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) ukázka tomu jeho `OnDisappearing` přepsat. Nevýhodou je, že *informace* stránky musí znát typ *domácí* stránky.

### <a name="messagingcenter"></a>MessagingCenter

Platformě Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) třída poskytuje další možnost pro dvě stránky pro komunikaci mezi sebou. Zprávy jsou identifikovány jako textový řetězec a mohou být spojena s libovolného objektu.

Program, který chce dostávat zprávy z konkrétního typu, musí se přihlásit k jejímu použití [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/) a určit funkce zpětného volání. Později můžete odhlásit voláním [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/). Funkce zpětného volání přijímá všechny zprávy odeslané ze zadaného typu se zadaným názvem odeslaných [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) metoda.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) program ukazuje, jak k přenosu dat pomocí zasílání zpráv centra, ale znovu to vyžaduje, aby *informace* stránky znát typ *domácí* stránky.

### <a name="events"></a>Události

Událost je time-honored přístup pro jednu třídu k odeslání informací do jiné třídy bez znalosti typu této třídy. V [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) ukázka *informace* třída definuje událost, která se aktivuje se v případě informace je připraven. Neexistuje však žádný vhodné místo pro *domácí* stránky odpojit obslužné rutiny události.

### <a name="the-app-class-intermediary"></a>Zprostředkovatel – třída aplikace

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) příklad ukazuje, jak získat přístup k vlastnosti definované v `App` třída oběma *domácí* stránky a *informace*stránky. Toto je dobrým řešením, ale další část popisuje něco lépe.

### <a name="switching-to-a-viewmodel"></a>Přepnutí na ViewModel

Použití ViewModel pro umožňuje informace *domácí* stránky a *informace* stránky sdílet instanci třídy informace. Tento postup je znázorněn v [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) ukázka.

### <a name="saving-and-restoring-page-state"></a>Uložení a obnovení stavu stránky

`App` Třídy zprostředkovatele nebo ViewModel přístupu je ideální, pokud aplikace musíte uložit informace, pokud program přejde do režimu spánku při *informace* stránky je aktivní. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) příklad znázorňuje to.

## <a name="saving-and-restoring-the-navigation-stack"></a>Uložení a obnovení navigační zásobníku

V případě obecné vícestránkové program, který přejde do režimu spánku musí přejít na stránku stejné po obnovení. To znamená, že takového programu měli uložit obsah zásobníku navigace. V této části ukazuje, jak k automatizaci tohoto procesu v třídě určené pro tento účel. Tato třída také voláním jednotlivé stránky, aby se mohly k uložení a obnovení stavu jejich stránky.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny definuje rozhraní s názvem [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) že třídy implementovat k uložení a obnovení položky v `Properties`slovníku.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovna je odvozena z `Application`. Potom můžete odvodit vaše `App` třídy z `MultiPageRestorableApp` a provádět některé housekeeping.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) demonstruje použití `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Něco jako reálnými aplikace

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) ukázku také využívá `MultiPageRestorableApp` a umožňuje zadávání a úpravy poznámek, které jsou uloženy ve `Properties` slovníku.



## <a name="related-links"></a>Související odkazy

- [Úplný text 24 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Ukázky kapitoly 24](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hierarchická navigace](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Modální stránky](~/xamarin-forms/app-fundamentals/navigation/modal.md)
