---
title: "Konfigurace vlastní Linkeru"
ms.topic: article
ms.prod: xamarin
ms.assetid: F8A99E3F-2197-4399-AC81-F1DBAB5729C9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: d5470dfc314677dbe558bca2f3ddfa107354c752
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="custom-linker-configuration"></a>Konfigurace vlastní Linkeru

Pokud výchozí sada možností nestačí, že proces propojení můžete jednotku se souborem XML, který popisuje, co chcete z linkeru.

Můžete zadat další definice linkeru zajistit typ, metody nebo pole nejsou vyloučit z vaší aplikace. V kódu je upřednostňovaný způsob použití `[Preserve]` vlastních atributů, jak bylo popsáno v [propojení na iOS](~/ios/deploy-test/linker.md) a [propojení v systému Android](~/android/deploy-test/linker.md) příručky.
Ale pokud budete potřebovat některé definicemi ze sestavení sady SDK nebo produktu, pak pomocí souboru XML může být nejlepší řešení (oproti přidáte kód, který bude zajištěno, aby že linkeru neodstraní, je nutné).

K tomuto účelu můžete definovat soubor XML s element nejvyšší úrovně <linker> obsahující *sestavení* uzlů, která obsahují *typ* uzlů, která obsahují *–Metoda* a *pole* uzlů.

Až budete mít tento soubor popis linkeru, přidejte do projektu a:

-  **Pro Android** : nastavte **akce sestavení** k **LinkDescription**
-  **Pro iOS** : nastavte **akce sestavení** k **LinkDescription**


Následující příklad ukazuje, jak vypadá soubor XML:

```xml
<linker>
        <assembly fullname="mscorlib">
                <type fullname="System.Environment">
                        <field name="mono_corlib_version" />
                        <method name="get_StackTrace" />
                </type>
        </assembly>
        <assembly fullname="My.Own.Assembly">
                <type fullname="Foo" preserve="fields">
                        <method name=".ctor" />
                </type>
                <type fullname="Bar">
                        <method signature="System.Void .ctor(System.String)" />
                        <field signature="System.String _blah" />
                </type>
                <namespace fullname="My.Own.Namespace" />
                <type fullname="My.Other*" />
        </assembly>
</linker>
```

Ve výše uvedeném příkladu bude číst a použít pokyny v linkeru `mscorlib.dll` (pro Android součástí Mono) a `My.Own.Assembly` sestavení (uživatelského kódu).

V první části pro `mscorlib.dll`, zajistí, že `System.Environment` typu se zachovat jeho pole s názvem `mono_corlib_version` a jeho `get_StackTrace` metoda.
Poznámka: metoda getter a setter metoda názvy se musí použít jako nerozumí vlastnosti jazyka C# a funguje na IL linkeru.

Druhá část pro `My.Own.Assembly.dll`, zajistí, že `Foo` typ zachová všechna její pole (tj `preserve="fields"` atribut) a všechny jeho konstruktorů (tj všechny metody s názvem `.ctor` v IL). `Bar` Typu se zachovat konkrétní podpisy (nejsou to názvy) pro jeden – konstruktor (který přijme parametr jednoho řetězce) a pro konkrétní řetězec pole `_blah`.
`My.Own.Namespace` Obor názvů se zachovat všechny typy, které obsahuje.
Nakonec libovolného typu, jejichž úplný název (včetně oboru názvů) odpovídá vzoru zástupný znak "My.Other\*" bude zachovat všechny jeho polí a metody. Zástupný znak `*` může být zahrnuta vícekrát v rámci vzor "Zadejte úplný název".



## <a name="related-links"></a>Související odkazy

- [Propojení v systému iOS](~/ios/deploy-test/linker.md)
- [Propojení v systému Android](~/android/deploy-test/linker.md)
