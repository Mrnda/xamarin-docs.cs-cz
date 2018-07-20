---
title: Souhrn kapitoly 24. Navigace po stránkách
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 24. Navigace po stránkách'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 35130baac4025fe69dbc7aa9b6928f824b35c573
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156701"
---
# <a name="summary-of-chapter-24-page-navigation"></a>Souhrn kapitoly 24. Navigace po stránkách

Mnoho aplikací se skládají z více stránek, mezi které uživatel přejde. Má aplikace vždy *hlavní* stránky nebo *domácí* stránky, a z něj uživatel přejde na jiné stránky, které jsou zachovány v zásobníku pro navigaci zpět. Další navigační možnosti jsou popsané v [ **kapitoly 25. Stránka typy**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Stránky modální a nemodální stránky

`VisualElement` definuje [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) vlastnost typu [ `INavigation` ](xref:Xamarin.Forms.INavigation), což zahrnuje následující dvě metody pro navigaci na novou stránku:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Obě metody přijímají `Page` instanci jako argument a návrat `Task` objektu. Následující dvě metody přejděte zpět na předchozí stránku:

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Pokud uživatelské rozhraní má svůj vlastní **zpět** tlačítko (stejně jako zařízení s Androidem a Windows Phone), pak není nutné pro aplikaci pro volání těchto metod.

I když tyto metody jsou dostupné z libovolného `VisualElement`, obecně se volají z `Navigation` vlastnost aktuálního `Page` instance.

Aplikace obvykle používají modální stránky, když uživatel je potřeba poskytnout určité informace na stránce návrat zpět na předchozí stránku. Stránky, které nejsou modální jsou někdy označovány jako nemodálního nebo *hierarchické*. Nic v samotné stránky odlišuje jej jako modálním nebo nemodálním; To se řídí místo toho metodu použitou k přejít k němu. Modální stránky fungovat na všech platformách, musíte zadat svoje vlastní uživatelské rozhraní pro navigaci zpět na předchozí stránku.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) ukázka umožňuje procházet rozdíl mezi modální a nemodální stránky. Všechny aplikace, která používá navigaci na stránce musí projít jeho domovské stránky na [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) konstruktoru, obvykle v programu `App` třídy. Jeden bonusové je, že už nebude potřeba nastavit `Padding` na stránce pro iOS.

Zjistíte, která pro nemodální stránky, na stránce vaší [ `Title` ](xref:Xamarin.Forms.Page.Title) vlastnost se zobrazí. IOS, Android a Windows tablet a desktop platformy poskytují prvek uživatelského rozhraní pro navigaci zpět na předchozí stránku. Kurz, Android a Windows phone zařízení mají standardní **zpět** tlačítko Zpět.

Pro modální stránky, na stránce `Title` nezobrazí, a je k dispozici žádné elementy uživatelského rozhraní přejít zpět na předchozí stránku. I když použijete standardní zařízení s Androidem a Windows phone **zpět** se vrátit na předchozí stránku modální stránky na jiných platformách, musíte zadat vlastní mechanismus, který chcete přejít zpátky.

### <a name="animated-page-transitions"></a>Animované stránky přechody

Jsou k dispozici alternativní verze různé metody navigace s druhou logický argument, který nastavíte na `true` přechod stránky zahrnout animace Chcete-li:

- [Metodu PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

Ale obsahovat metody standardní navigace stránkami animace ve výchozím nastavení, tak tyto jsou užitečné pouze pro přechod na určitou stránku při spuštění (jak je popsáno na konci této kapitole), nebo při zadávání vstupu animace (jak je popsáno v [ **Chapter22. Animace**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Varianty Visual a funkční

`NavigationPage` zahrnuje dvě vlastnosti, které jste nastavili při vytváření instance třídy ve vašich `App` metody:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` také obsahuje čtyři připojené vlastnosti umožňující vazbu, které ovlivňují konkrétní stránce, na kterém jsou nastavené:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) a [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) a [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) a [ `GetBackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) pracovat jenom v Iosu
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) a [ `GetTitleIcon` ](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) pracovat na pouze iOS a Android

### <a name="exploring-the-mechanics"></a>Zkoumání mechanismů

Jsou všechny asynchronní metody navigace stránky a je nutné používat s `await`. Dokončení neukazuje, jestli navigaci na stránce se dokončil, ale pouze, který je bezpečný prozkoumat zásobník navigaci na stránce.

Když jednu stránku přejde na jiný, první stránka obecně získá volání jeho [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) metoda a na druhé stránce získá volání jeho [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) metoda. Podobně, když se do druhého vrátí jednu stránku, na první stránce získá volání jeho `OnDisappearing` metoda a na druhé stránce obecně získá volání jeho `OnAppearing` metoda. Pořadí těchto volání (a dokončení asynchronní metody, která volá navigaci) je platforma pro závislé. Použijte slova "obecně" v předchozích dvou příkazech je z důvodu Android navigaci na stránce modální okno, ve které tato volání metody nedojde.

Také, volání `OnAppearing` a `OnDisappearing` metody není nutně znamenat navigace po stránkách.

`INavigation` Rozhraní zahrnuje dvě vlastnosti kolekce, které umožňují prozkoumat navigační zásobník:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) typu `IReadOnlyList<Page>` nemodální zásobníku
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) typu `IReadOnlyList<Page>` pro modální zásobníku

Je nejbezpečnější pro přístup k tyto balíčky z `Navigation` vlastnost `NavigationPage` (což by mělo být `App` třídy [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) vlastnost). Je bezpečné prozkoumat tyto balíčky po dokončení metody asynchronní navigace stránky. [ `CurrentPage` ](xref:Xamarin.Forms.NavigationPage.CurrentPage) Vlastnost `NavigationPage` nenaznačuje, že aktuální stránku Pokud aktuální stránka je modální stránky, ale indikuje místo toho na poslední stránce nemodální.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) ukázky vám umožňuje zkoumat navigace stránky a zásobníky a navigace stránky právní typy:

- Nemodální stránku můžete přejít na jinou stránku nemodální nebo modální stránky
- Modální stránky můžete přejít jenom na jiné modální stránky

### <a name="enforcing-modality"></a>Způsob vynucení

Aplikace používá modální stránky, když je potřeba získat několik informací od uživatele. Uživatel by mělo být zakázáno vrácení na předchozí stránku, dokud nebude poskytnuta těchto informací. V systémech iOS, je snadné k poskytování **zpět** tlačítko a povolit pouze v případě, že uživatel dokončil se stránkou. Pro zařízení s Androidem a Windows phone, aplikace by měly přepsat, ale [ `OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) metodu a vrátí `true` Pokud program byla zpracována **zpět** tlačítko samostatně, jak je ukázáno v [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) vzorku.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) ukázka demonstruje, jak to funguje ve scénáři MVVM.

## <a name="navigation-variations"></a>Variace navigace

Pokud konkrétní modální stránky se dá Navigovat do více než jednou, je tak, aby uživatel mohl upravit informace spíše než jeho zadáním by měla uchovávat informace ve všech znovu. Dokáže zpracovat to tak, že zachová konkrétní instance modální stránky, ale vhodnější (zejména v Iosu) je zachování informací v zobrazení modelu.

### <a name="making-a-navigation-menu"></a>Provádění navigační nabídky

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) ukázka znázorňuje použití `TableView` k vytvoření seznamu položek nabídky. Každá položka je přidružena `Type` objekt konkrétní stránky. Při výběru této položky program vytvoří na stránce a přejde k němu.

[![Trojitá snímek obrazovky zobrazení galerie typu](images/ch24fg21-small.png "zobrazení Tabulka seznam položek nabídky")](images/ch24fg21-large.png#lightbox "zobrazení Tabulka seznam položek nabídky")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) vzorek se mírně liší v tom, že v nabídce obsahuje instance každé stránce, než typy. To pomůže uchovávat informace z každé stránky, ale všechny stránky musí být vytvořena instance při spuštění programu.

### <a name="manipulating-the-navigation-stack"></a>Manipulace s navigačním zásobníku

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) ukazuje několik funkcí, které jsou určené `INavigation` , která umožňuje pracovat s navigační zásobník strukturovaných způsobem:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) a [ `PopToRootAsync` ](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) s volitelné animace

### <a name="dynamic-page-generation"></a>Generování dynamických stránky

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) ukázka demonstruje sestavování stránku za běhu na základě uživatelského zadání.

## <a name="patterns-of-data-transfer"></a>Vzory přenosu dat

Často je potřeba sdílet data mezi stránkami &mdash; přenášet data do navigated stránky a stránky vrátit data na stránce, která vyvolala. Jak to udělat několika způsoby.

### <a name="constructor-arguments"></a>Argumenty konstruktoru

Při přechodu na novou stránku, je možné vytvořit instanci třídy stránky s argumentem konstruktor, který umožňuje, aby stránka inicializovat. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) příklad ukazuje to. Je také možné navigated stránky mít jeho `BindingContext` ve stránce, která přejde do něj nastavení.

### <a name="properties-and-method-calls"></a>Vlastnosti a volání metod

Zbývající příklady přenos dat si projděte problém předávání informací mezi stránkami, když jedna stránka přejde na jinou stránku a zpět. V těchto diskuzích *domácí* stránky přejde na *informace o* stránce a musíte přenést inicializované informace, které *informace* stránky. *Informace* stránky získá další informace od uživatele a přenáší informace, které *domácí* stránky.

*Domácí* stránky můžete snadný přístup k veřejné metody a vlastnosti v *informace* stránce co nejdříve vytvoří danou stránku. *Informace* stránky se dostanete také veřejné metody a vlastnosti v *domácí* stránky, ale volba vhodná doba pro to může být velmi obtížné. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) ukázka dělá to jeho `OnDisappearing` přepsat. Jednou z nevýhod je, že *informace* stránce je třeba znát typ *domácí* stránky.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) třída poskytuje způsob, další dvě stránky komunikovat mezi sebou. Zprávy jsou označeny jako textový řetězec a mohou být spojena s jakýmkoli objektem.

Program, který chce dostávat zprávy z určitého typu, musí se přihlásit k odběru je pomocí [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) a určit funkce zpětného volání. Později můžete odhlásit pomocí volání [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*). Funkce zpětného volání přijímá všechny zprávy odeslané ze zadaného typu se zadaným názvem odeslaných [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) metody.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) program ukazuje, jak přenášet data pomocí centra pro zasílání zpráv, ale znovu to vyžaduje, aby *informace* stránky znát typ *domácí* stránky.

### <a name="events"></a>Události

Událost je time-honored přístup pro jednu třídu k odesílání informací do jiné třídy bez znalosti typu dané třídy. V [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) ukázka *informace* třída definuje události, který se aktivuje při informace je připravený. Neexistuje však žádný vhodným místem pro *domácí* stránky k odpojení obslužné rutiny události.

### <a name="the-app-class-intermediary"></a>Třída zprostředkovatele aplikace

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) příklad ukazuje, jak získat přístup k vlastnosti definované ve `App` třídy i *domácí* stránky a *informace*stránky. Toto je dobrým řešením, ale další část popisuje něco lépe.

### <a name="switching-to-a-viewmodel"></a>Přepnutí ViewModel

Použití ViewModel pro informace. umožňuje *domácí* stránky a *informace* stránky sdílet instanci třídy informace. To je patrné [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) vzorku.

### <a name="saving-and-restoring-page-state"></a>Uložení a obnovení stavu stránky

`App` Třídy zprostředkovatele nebo ViewModel přístup je ideální, pokud aplikace musíte uložit informace, pokud program přejde do režimu spánku při *informace* stránky je aktivní. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) příklad ukazuje to.

## <a name="saving-and-restoring-the-navigation-stack"></a>Ukládání a obnova navigačním zásobníku

V tomto obecném případě by měl vícestránkové program, který přejde do režimu spánku, přejděte na stejnou stránku po obnovení. To znamená, že takový program by měla uložit obsah navigačního zásobníku. Tato část ukazuje, jak pro automatizaci tohoto procesu ve třídě navržené pro tento účel. Tato třída také voláním jednotlivé stránky a povolení jejich uložení a obnovení stavu stránky.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna definuje rozhraní s názvem [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) , že můžete implementovat třídy k uložení a obnovení položky v `Properties`slovníku.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) Třídy v **Xamarin.FormsBook.Toolkit** knihovny je odvozena z `Application`. Pak lze odvodit váš `App` třídy z `MultiPageRestorableApp` a provést některé údržbu.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) demonstruje použití `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Něco jako je třeba reálné aplikace

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) ukázka také používá `MultiPageRestorableApp` a umožňuje zadávat a upravovat poznámky, které jsou uloženy v `Properties` slovníku.



## <a name="related-links"></a>Související odkazy

- [Kapitola 24 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Ukázky kapitoly 24](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Hierarchická navigace](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Modální stránky](~/xamarin-forms/app-fundamentals/navigation/modal.md)
