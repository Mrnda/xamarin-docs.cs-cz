---
title: iOS 9 kompatibility
description: I v případě, že nechcete přímo okamžitě přidat funkce iOS 9 do vaší aplikace, je třeba znovu sestavit aplikace pomocí nejnovější verzi Xamarinu.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2f22fdeaad1b276bb94d2b1ee5af4a6c24d22cb7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350778"
---
# <a name="ios-9-compatibility"></a>iOS 9 kompatibility

_I v případě, že nechcete přímo okamžitě přidat funkce iOS 9 do vaší aplikace, je třeba znovu sestavit aplikace pomocí nejnovější verzi Xamarinu._

> [!IMPORTANT]
> Informace na této stránce je určená pro zákazníky s aplikacemi již v App Store cílících na iOS 8 nebo starší, který již neodeslali aktualizací pro iOS 9 kompatibility. Pokud už používáte nejnovější verze – Xcode 7 a 9 Xamarin.iOS – při vývoji aplikace najdete [Úvod do Iosu 9](~/ios/platform/introduction-to-ios9/index.md).

Při první beta verze iOS 9 objevily, myslíme, že dva problémy se staršími verzemi nástroje Xamarin, označované jako starší aplikace se nepovedlo spustit v systému iOS 9.

Tyto dva problémy (jako [na našich fórech](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) byly:

- Vytváření aplikací pro iOS 8 nebo starším, nebude možné spustit na zařízeních s 32-bit (včetně aplikace vytvořené pomocí [sjednocené rozhraní API](~/cross-platform/macios/unified/index.md)).
- Nezadal se deklarace P/Invoke služeb při selhání s úplnou cestou.

Aktualizace Xamarinu instalaci nejnovější kanál stabilní verzi a pak znovu sestavovat a nasazovat aplikace, řeší tyto dva problémy.

_I když nemáte v úmyslu hned aktualizovat aplikace pomocí funkce iOS 9, doporučujeme, abyste znovu sestavit nejnovější verzi Xamarinu a znovu odeslat do App Store_.



Tím se zajistí, že vaše aplikace poběží v systému iOS 9 po upgradu vašich zákazníků.
Můžete nadále podporovat iOS 8 - znovu sestavit s nejnovější verzí nemá vliv na cílovou verzi aplikace.

Pokud máte další problémy při testování vaší existující aplikace v systému iOS 9, přečtěte si [zlepšení kompatibility](#compat) níže v části.


### <a name="updating-with-visual-studio"></a>Aktualizuje se sadou Visual Studio

Doporučuje se explicitně zkontrolujte, že Visual Studio se aktualizuje na nejnovější stabilní verzi.

## <a name="what-about-components-nugets-and-other-libraries"></a>Co o součástech, balíčky Nuget a další knihovny?

Provedete **není** muset počkat, než pro nové verze jakékoli součásti a balíčky Nuget pomocí adresy dva problémy uvedené výše.
Jednoduše tak, že nové sestavení aplikace s využitím nejnovější stabilní verze Xamarin.iOS opravě těchto problémů.

Podobně, dodavateli komponent a autoři Nuget se **není** předkládat nové buildy to opravit dva problémy uvedené výše. Ale případné fungování součásti nebo Nuget používá `UICollectionView` nebo načíst zobrazení z **Xib** soubory aktualizace *může* nemusíte řešit problémy s kompatibilitou s Iosem 9 uvedených níže.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Zlepšení kompatibility ve vašem kódu

Existuje několik případů kódu vzor, který *použít* pracovat ve starších verzích systému iOS dopadem na dřívější kód v systému iOS 9. Tady jsou některé informace o možných problémech (a jejich řešení), které mohou vzniknout při testování v systému iOS 9:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView má hodnotu null v konstruktorech

**Důvod:** v systému iOS 9 `initWithFrame:` konstruktor je nyní požadováno, díky změnám chování systému iOS 9 jako [UICollectionView dokumentaci stavy](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Pokud jste zaregistrovali třídu pro zadaný identifikátor se musí vytvořit novou buňku, buňku nyní inicializován pomocí volání jeho `initWithFrame:` metoda.

**Oprava:** přidat `initWithFrame:` konstruktor takto:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Související ukázky: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView nepodaří init s kodér při načítání zobrazení z Xib/Nib

**Důvod:** `initWithCoder:` konstruktoru je volán při načtení zobrazení od souboru Xib Tvůrce rozhraní. Pokud není exportována tento konstruktor nelze volat nespravovaný kód naše spravovaná verze. Dříve (např.) IOS 8) `IntPtr` konstruktoru byla vyvolána k inicializaci zobrazení.

**Oprava:** vytvoření a export `initWithCoder:` konstruktor takto:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Související ukázkové: [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Zpráva Dyld: žádné mezipaměti image s názvem...

Může dojít při selhání s těmito informacemi v protokolu:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Důvod:** jedná o chybu v nativní linkeru společnosti Apple, který se stane, když využívají privátní framework veřejné (JavaScriptCore zveřejněných v iOS 7, předtím, že byl privátní framework), a cíl nasazení aplikace je pro verzi iOS při rozhraní je privátní. V tomto případě bude propojovat propojovací program společnosti Apple k privátní verzi rozhraní framework místo ve veřejné verzi.

**Oprava:** to vyřešíme pro iOS 9, ale není snadno alternativní řešení můžete sami provést do té doby: stačí cílová iOS novější verze ve vašem projektu (v tomto případě je můžete zkusit systému iOS 7). Další platformy může vykazovat s podobnými problémy, například rozhraní komponenty WebKit zveřejněných v iOS 8 (a tak cílících na iOS 7 bude výsledkem následující chyba, můžete zaměřit iOS 8 použít komponenty WebKit ve vaší aplikaci).



## <a name="related-links"></a>Související odkazy

- [informace o verzi systému iOS 9 kompatibility](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Úvod do iOSu 9](~/ios/platform/introduction-to-ios9/index.md)
- [Aktualizace aplikace na platformě Xamarin.iOS iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
