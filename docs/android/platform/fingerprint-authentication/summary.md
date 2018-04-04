---
title: Otisk prstu ověřování pokyny
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2b66c3660f6d8af9217089a7615784957fcc6ed7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="fingerprint-authentication-guidance"></a>Otisk prstu ověřování pokyny

## <a name="fingerprint-authentication-guidance"></a>Otisk prstu ověřování pokyny

Teď, když jsme viděli, koncepty a rozhraní API, které obaluje Android 6.0 otisků ověřování, probereme některé obecné pokyny k používání rozhraní API otisků prstů.

1. **Použít rozhraní API systému Android knihovna podpory v4 kompatibility** &ndash; tím zjednodušit kód aplikace odebráním kontrola rozhraní API z kódu a povolit aplikace k nejvíce možné zařízení.
2. **Zadejte alternativy k ověřování otisk prstu** &ndash; otisk prstu ověřování je skvělé, rychlý způsob, jak aplikace k ověření uživatele, ale nelze předpokládat, že budou vždy fungovat nebo být k dispozici. Je možné, že může selhat skener otisků prstů, přehledu se možná nekonzistence, uživatel nemusí nakonfigurovali zařízení pro použití ověřování otisk prstu nebo prstů mít od ztratilo. Je také možné, že uživatel nemusí chcete použít ověřování otisků prstů s vaší aplikací. Z těchto důvodů by měl poskytovat aplikace platformy Android procesu ověřování, jako je například uživatelské jméno a heslo.
3. **Použijte ikonu otisk prstu Google** &ndash; všechny aplikace by měly používat stejný otisk prstu ikony poskytnuté Google. Použití standardní ikonu usnadňuje uživatelé s Androidem rozpoznat, kdy v aplikacích se používá ověřování otisk prstu: 
    
    ![Ikona Android otisk prstu](summary-images/ic-fp-40px.png)
    
4. **Upozornit uživatele,** &ndash; aplikace by se měla zobrazovat nějaký druh oznámení uživateli, že skener otisků prstů je aktivní a čeká na touch nebo prstem. 

## <a name="summary"></a>Souhrn

Otisk prstu ověřování je skvělým způsobem, jak povolit aplikace pro Xamarin.Android rychle ověření uživatele, a usnadnit uživatelům interakci s citlivé funkce, jako je nákupy v aplikaci. Tato příručka popsané koncepty a kód, který je potřeba začlenit otisk prstu Android 6.0 rozhraní API je v aplikaci Xamarin.Android.

Nejprve jsme probrali otisk prstu rozhraní API je sami `FingerprintManager` (a `FingerprintManagerCompat`). Jsme se zaměřili na jak `FingerprintManager.AuthenticationCallbacks` abstraktní třída musí být rozšířit pomocí aplikace a použít jako zprostředkovatel mezi otisk prstu hardwaru a vlastní aplikace. Potom jsme se zaměřili na tom, jak ověřit integritu výsledky skener otisk prstu pomocí Java `Cipher` objektu. Nakonec jsme dotýkal trochu na testování popisující, jak zaregistrovat otisk prstu na zařízení a použitím **adb** k simulaci prstem otisku prstu na emulátor. 

Pokud jste tak již neučinili, měli byste se podívat na [ukázkové aplikace](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) doprovodný této příručce. [Otisk prstu dialogové okno ukázka](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) byly přesně z prostředí Java do Xamarin.Android a představuje další příklad o tom, jak přidat otisk prstu ověřování do aplikace systému Android.



## <a name="related-links"></a>Související odkazy

- [Otisk prstu Průvodce ukázkové aplikace](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Ukázka dialogové okno otisk prstu](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Ikona otisk prstu](https://developer.android.comhttps://developer.xamarin.com/samples/FingerprintDialog/res/drawable-hdpi/ic_fp_40px.html)
