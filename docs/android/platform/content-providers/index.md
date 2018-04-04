---
title: Úvod do ContentProviders
description: Operační systém Android používá k usnadnění přístupu ke sdíleným datům, například soubory médií, kontakty a informací z kalendáře poskytovatelů obsahu. Tento článek představuje třídu ContentProvider a poskytuje dva příklady, jak ji používat.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: e534c02820bfeab3a5bc1211bf0cbb20b9821af3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="intro-to-contentproviders"></a>Úvod do ContentProviders

_Operační systém Android používá k usnadnění přístupu ke sdíleným datům, například soubory médií, kontakty a informací z kalendáře poskytovatelů obsahu. Tento článek představuje třídu ContentProvider a poskytuje dva příklady, jak ji používat._


## <a name="content-providers-overview"></a>Přehled poskytovatelů obsahu

A *ContentProvider* zapouzdří úložiště dat a poskytuje rozhraní API pro přístup k ní. Zprostředkovatel existuje jako součást aplikace Android, která obvykle také poskytuje uživatelské rozhraní pro zobrazení nebo správu data. Klíčovou výhodou použití poskytovatele obsahu, je povolení dalších aplikací snadný přístup k zapouzdřené data s využitím klientského objektu zprostředkovatele (nazývá *ContentResolver*). Poskytovateli obsahu a obsahu překladač společně nabízejí konzistentní mezi aplikace API pro přístup k datům, který se snadno vytvářet a využívat. Všechny aplikace můžete zvolit `ContentProviders` interně spravovat data a také ke zveřejnění k ostatním aplikacím.

A `ContentProvider` je také nutný pro vaši aplikaci zajistit návrhy vlastní vyhledávání, nebo pokud chcete zadat možnost Kopírovat komplexní data z vaší aplikace do vložení do jiných aplikací. Tento dokument popisuje, jak pro přístup k a sestavení `ContentProviders` s Xamarin.Android.

Struktura v této části je následující:

- **Jak to funguje** &ndash; přehled co `ContentProvider` je určená pro a jak to funguje.

- **Využívání obsahu zprostředkovatele** &ndash; příklad přístup k seznamu kontaktů.

- **Použití ContentProvider pro sdílení dat** &ndash; zápisy a náročná `ContentProvider` ve stejné aplikaci.

`ContentProviders` a kurzory, které působí na jejich data se často používají k naplnění ListViews. Odkazovat [ListViews a adaptéry Průvodce](~/android/user-interface/layouts/list-view/index.md) Další informace o tom, jak používat tyto třídy.

`ContentProviders` vystavené Android (nebo jiné aplikace) jsou snadný způsob, jak do aplikace zahrnout data z jiných zdrojů. Umožňují přístup a zobrazení dat, jako je například seznamu kontaktů, fotografie nebo kalendář události z v rámci vaší aplikace a umožní uživatelům interakci s daty.

Vlastní `ContentProviders` jsou vhodný způsob k balíčku data pro použití ve vaší vlastní aplikaci nebo pro použití jinými aplikacemi (včetně speciálních používá jako vlastní vyhledávání, kopírování a vkládání).

Témata v této části poskytují jednoduché příklady použití a zápis `ContentProvider` kódu.



## <a name="related-links"></a>Související odkazy

- [Ukázkový ContactsAdapter (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (ukázka)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [Příručka pro vývojáře poskytovatelů obsahu](http://developer.android.com/guide/topics/providers/content-providers.html)
- [Odkaz na ContentProvider – třída](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [Odkaz na ContentResolver – třída](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [Referenční třída ListView](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [Odkaz na CursorAdapter – třída](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [Odkaz na UriMatcher – třída](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [Odkaz na ContactsContract – třída](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
