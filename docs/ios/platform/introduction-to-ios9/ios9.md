---
title: iOS 9 kompatibility
description: "I když nemáte v úmyslu okamžitou přidat funkce iOS 9 do vaší aplikace, je třeba znovu sestavit aplikace s nejnovější verzi Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 3f52ef88224e65f12502c9fbf82e240233890a82
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="ios-9-compatibility"></a>iOS 9 kompatibility

_I když nemáte v úmyslu okamžitou přidat funkce iOS 9 do vaší aplikace, je třeba znovu sestavit aplikace s nejnovější verzi Xamarin._

> [!IMPORTANT]
> Informace na této stránce jsou pro zákazníky s aplikací již v App Storu iOS 8 cílení nebo starší, který již neodeslali aktualizace pro iOS 9 kompatibility. Pokud už používáte nejnovější verze - Xcode 7 a 9 Xamarin.iOS – pro vývoj aplikací pro vaše naleznete na [Úvod do systému iOS 9](~/ios/platform/introduction-to-ios9/index.md).

Při první beta iOS 9 zobrazovaly, jsme našli dva problémy s starší verze Xamarin, který označované jako starší aplikace, které se nepodařilo spustit v systému iOS 9.

Tyto dva problémy (jako [podrobné na našich fórech](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) měla:

- Vytvoření aplikace pro iOS 8 nebo starší, nebude možné spustit na 32bitových zařízení (včetně aplikace vytvořené s [unifikované API](~/cross-platform/macios/unified/index.md)).
- Selhání s úplnou cestou P/Invoke není zadán.

Aktualizace instalace Xamarin na nejnovější verzi stabilní kanál a pak znovu sestavit a opětovného nasazení aplikace, řeší tyto dva problémy.

_I když nemáte v plánu aktualizujete aplikaci s iOS 9 funkce hned, doporučujeme, abyste znovu sestavit pomocí nejnovější verze Xamarin a znovu odeslat na obchod s aplikacemi_.



Tím bude zajištěno, že vaše aplikace poběží v systému iOS 9, po upgradu vašich zákazníků.
Můžete dál zajišťovat podporu iOS 8 - znovu sestavit v nejnovější verzi nemá vliv na cílovou verzi aplikace.

Pokud máte další problémy při testování vaše stávající aplikace v systému iOS 9, přečtěte si [zvýšení kompatibility](#compat) části níže.


### <a name="updating-with-visual-studio"></a>Aktualizace pomocí sady Visual Studio

Doporučuje se explicitně zkontrolovala, Visual Studio se aktualizuje na nejnovější stabilní verze.

## <a name="what-about-components-nugets-and-other-libraries"></a>Co o součástech, Nugets a další knihovny?

Provedete **není** muset počkat, než pro nové verze součásti nebo Nugets používáte řeší dva problémy uvedené výše.
Jednoduše tak, že opětovné sestavení aplikace pomocí nejnovější stabilní verze Xamarin.iOS opravě těchto problémů.

Podobně součást dodavateli a autoři Nuget jsou **není** potřebné k odeslání nových sestavení automaticky jenom k opravte dva problémy uvedené výše. Ale případných komponenta nebo Nuget používá `UICollectionView` nebo zatížení zobrazení z **Xib** soubory aktualizace *může* muset vyřešit problémy s kompatibilitou iOS 9 uvedených níže.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Zvýšení kompatibility v kódu

Vzory několik případů kódu, který je *používá* pro práci v starší verzí systému iOS nejnovější ve iOS 9. Tady jsou některé možné problémy (a jejich řešení), může nastat při testování v systému iOS 9:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView má hodnotu null v konstruktorech

**Důvod:** v iOS 9 `initWithFrame:` konstruktor je nyní požadováno, díky změnám chování iOS 9 jako [UICollectionView dokumentace stavy](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Pokud jste zaregistrovali třídu pro zadaný identifikátor a musí být vytvořeny nové buňky, buňky se teď inicializuje pomocí volání jeho `initWithFrame:` metoda.

**Oprava:** přidat `initWithFrame:` konstruktor takto:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Související ukázky: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView nepodaří init s kodér při načítání zobrazení z Xib nebo Nib

**Důvod:** `initWithCoder:` konstruktor je volána, když se načtení zobrazení ze souboru Xib Tvůrce rozhraní. Pokud tento konstruktor není exportovaný nelze volat nespravovaný kód naše spravované verzi. Dříve (např. v iOS 8) `IntPtr` konstruktor byl vyvolán k chybě při inicializaci zobrazení.

**Oprava:** vytvoření a export `initWithCoder:` konstruktor takto:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Související ukázka: [chatu](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Zpráva Dyld: žádné mezipaměti obrázek s názvem...

Můžete zaznamenat havárie s následujícími informacemi v protokolu:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Důvod:** jedná o chybu v nativní linkeru společnosti Apple, který se stane, když se zveřejnit privátní framework (JavaScriptCore byl proveden veřejné v iOS 7, než že byl privátní rozhraní), a cíl nasazení aplikace se pro verzi iOS při Framework byl privátní. V takovém případě společnosti Apple linkeru odkaz s privátní verzi rozhraní framework místo veřejné verze.

**Oprava:** to bude řešit pro iOS 9, ale není snadno alternativní řešení sami do té doby můžete použít: právě cíle iOS novější verze ve vašem projektu (v takovém případě můžete můžete zkusit iOS 7). Ostatní platformy může vykazovat podobné problémy, například rozhraní WebKit byl proveden veřejné v iOS 8 (a tak cílení na iOS 7 bude výsledkem této chybě; má cíl iOS 8 použít WebKit v aplikaci).



## <a name="related-links"></a>Související odkazy

- [informace o verzi kompatibility iOS 9](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Úvod do iOSu 9](~/ios/platform/introduction-to-ios9/index.md)
- [Aktualizace aplikace Xamarin.iOS na iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
