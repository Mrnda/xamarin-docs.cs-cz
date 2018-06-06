---
title: Propojení aplikace Xamarin.iOS
description: Tento dokument popisuje linkeru Xamarin.iOS, který se používá k odstranění nepoužívaných kód z aplikace pro Xamarin.iOS kvůli snížení jeho velikost.
ms.prod: xamarin
ms.assetid: 3A4B2178-F264-0E93-16D1-8C63C940B2F9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/24/2017
ms.openlocfilehash: f80faa961fe4bef45df33c411d914ba80e605c75
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785577"
---
# <a name="linking-xamarinios-apps"></a>Propojení aplikace Xamarin.iOS

Při vytváření aplikace, Visual Studio pro Mac nebo Visual Studio volá nástroj nazvaný **mtouch** linkeru pro spravovaný kód, který obsahuje. Slouží k odebrání knihovny tříd funkce, které aplikace nepoužívá. Cílem je ke snížení velikosti aplikaci, které se dodávají spolu s pouze nezbytné služby bits.

Linkeru používá k určení cesty jiný kód, které vaše aplikace je ohrožena útoky založenými na postupujte podle statické analýzy. Jedná se trochu těžká, jako má projít všechny podrobnosti každé sestavení, abyste měli jistotu, že nic zjistitelný odebrána. Ve výchozím nastavení v simulátoru sestavení není povolena pro urychlení čase vytvoření buildu při ladění. Ale vzhledem k tomu, že vytváří menší aplikace ho můžete urychlit AOT kompilace a odesílání do zařízení, všechny *zařízení (verze) sestavení* ve výchozím nastavení používají linkeru.

Linkeru je nástroj statické, nelze označit pro zahrnutí typy a metody, které jsou volány prostřednictvím reflexe, nebo dynamicky vytvoření instance. Chcete-li vyřešit několik možností existuje toto omezení.

<a name="Linker_Behavior" />

## <a name="linker-behavior"></a>Chování linkeru

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Proces propojení lze přizpůsobit pomocí rozevíracího seznamu chování linkeru v **možnosti projektu**. Pro přístup k této dvakrát klikněte na projekt pro iOS a přejděte do **iOS sestavení > Možnosti Linkeru**, jak je uvedeno dále:

[![](linker-images/image1.png "Možnosti linkeru")](linker-images/image1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Proces propojení lze přizpůsobit pomocí rozevíracího seznamu linkeru chování v **vlastnosti projektu** v sadě Visual Studio.

Postupujte takto:

1. Klikněte pravým tlačítkem na **název projektu** v **Průzkumníku řešení** a vyberte **vlastnosti**:

    ![](linker-images/linking01w.png "Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte vlastnosti")
2. V **vlastnosti projektu**, vyberte **IOS sestavení**:

    ![](linker-images/linking02w.png "Vyberte IOS sestavení")
3. Postupujte podle pokynů níže změnit možností propojení.

-----

Nabízí tři hlavní možnosti jsou popsány níže:


### <a name="dont-link"></a>Nemáte propojení

Zakázání propojení bude Ujistěte se, že jsou upraveny žádné sestavení. Z důvodů výkonu je výchozí nastavení, když vaše IDE cílem pro simulátoru iOS. Pro sestavení zařízení mělo by být použito pouze jako alternativní řešení vždy, když linkeru obsahuje chybu, která zabrání spuštění vaší aplikace. Pokud vaše aplikace pracuje pouze s *- nolink*, odešlete prosím [Sestava chyb](http://bugzilla.xamarin.com).

To odpovídají *- nolink* možnost při použití mtouch nástroj příkazového řádku.

<a name="Link_SDK_assemblies_only" />

### <a name="link-sdk-assemblies-only"></a>Odkaz sestavení SDK pouze

V tomto režimu linkeru ponechá vaše sestavení nezměněný a sníží velikost sestavení sady SDK (tj. co je součástí Xamarin.iOS) odebráním všechno, co se vaše aplikace nebude používat. Toto je výchozí nastavení při vaší IDE cílem zařízení s iOS.

Toto je nejjednodušší možnost, jak nevyžaduje žádné změny v kódu. Rozdíl oproti propojení všechno, co je, že linkeru nelze v tomto režimu tak, aby byl kompromis mezi práce potřebné k propojení všechno a velikost konečné aplikace provést několik optimalizací.

To odpovídají *- linksdk* možnost při použití mtouch nástroj příkazového řádku.

<a name="Link_all_assemblies" />

### <a name="link-all-assemblies"></a>Odkaz ve všech sestaveních

Při propojování vše, linkeru můžete použít celou sadu jeho optimalizace zpřístupnění aplikace co nejmenší. Upraví ho uživatelského kódu, který může rozdělit vždy, když kód používá funkce způsobem, který nelze zjistit statické analýzy linkeru. V takových případech, například webovým službám, reflexe nebo serializace, bude pravděpodobně nutné některé adjustements ve vaší aplikaci propojení vše.

To odpovídají *- linkall* při pomocí nástroje příkazového řádku **mtouch**.

<a name="Controlling_the_Linker" />

## <a name="controlling-the-linker"></a>Řízení Linkeru

Při použití linkeru ji někdy se odebere kód, který může mít například dynamicky, ani nepřímo. Pro tyto případy linkeru nabízí několik funkce a možnosti a umožní vám větší kontrolu na jeho akce.

<a name="Preserving_Code" />

### <a name="preserving-code"></a>Zachování kódu

Při použití linkeru můžete někdy odebrat kód, který vám může mít například dynamicky buď pomocí System.Reflection.MemberInfo.Invoke, nebo pomocí export vaše metody pomocí jazyka Objective-C `[Export]` atribut a poté ručně vyvolání modulu pro výběr .

V takových případech můžete určit, aby linkeru vzít v úvahu buď celý třídy mají být používá nebo jednotlivé členy nutné zachovat použitím `[Xamarin.iOS.Foundation.Preserve]` atribut buď na úrovni třídy nebo na úrovni člena. Každý člen, který není staticky propojený aplikací je podléhají odeberou. Proto tento atribut slouží k označení členy, které nejsou staticky odkazuje, ale který stále potřeba vaší aplikací.

Například pokud vytvoříte instanci typy dynamicky, můžete zachovat výchozí konstruktor vašich typů. Pokud používáte serializace XML, můžete zachovat vlastnosti vaší typů.

Na každý člen typu nebo na vlastní typ, můžete použít tento atribut. Pokud chcete zachovat celý typ, můžete použít syntaxi `[Preserve
(AllMembers = true)]` na typu.

Někdy budete chtít zachovat určité členy, ale pouze, pokud byla zachována nadřazeného typu. V takových případech použít `[Preserve (Conditional=true)]`

Pokud nechcete provést závislost na knihovny Xamarin-Řekněme například, že vytváříte knihovny přenosných tříd křížové platformy (PCL) – když můžete nadále používat tento atribut.

Uděláte to tak, by měl právě PreserveAttribute třídu deklarovat a použít ho v kódu, například takto:

```csharp
public sealed class PreserveAttribute : System.Attribute {
    public bool AllMembers;
    public bool Conditional;
}
```

Není důležité skutečnosti ve které obor názvů je definována, linkeru vypadá tento atribut název typu.

 <a name="Skipping_Assemblies" />

### <a name="skipping-assemblies"></a>Přeskočení sestavení

Je možné zadat sestavení, které mají být vyloučeny z procesu linkeru současně ostatních sestavení propojení normálně. To je užitečné, pokud používáte `[Preserve]` na některá sestavení, není možné (například kód 3. stran) nebo jako dočasné řešení pro chyby.

To odpovídají `--linkskip` možnost při použití mtouch nástroj příkazového řádku.

Při použití **odkaz ve všech sestaveních** možnost, pokud chcete zjistit linkeru chcete přeskočit celý sestavení, zadejte následující příkaz **mtouch další argumenty** možnosti vašeho nejvyšší úrovně sestavení:

```csharp
--linkskip=NameOfAssemblyToSkipWithoutFileExtension
```

Pokud chcete, aby linkeru tak, aby přeskočil více sestavení, můžete zahrnout více `linkskip` argumenty:

```csharp
--linkskip=NameOfFirstAssembly --linkskip=NameOfSecondAssembly
```

Neexistuje žádné uživatelské rozhraní pro tuto možnost použijte, ale jeho lze zadat v sadě Visual Studio pro dialogové okno Možnosti projektu Mac nebo v podokně vlastností projektu sady Visual Studio v **mtouch další argumenty** textové pole. (Např.) *--linkskip = mscorlib* nebude propojit mscorlib.dll ale by propojit ostatních sestavení v řešení).

<a name="Disabling_Link_Away" />

### <a name="disabling-link-away"></a>Zakázání "Link rychle"

Linkeru odebere kód, který je velmi pravděpodobně k použití na zařízeních, například nepodporovaný nebo zakázána. Ve výjimečných případech je možné, že aplikace nebo knihovna závisí na tento kód (práce nebo ne). Linkeru může být nastavena tak, aby přeskočil optimalizace, protože Xamarin.iOS – 5.0.1.

To odpovídají *- nolinkaway* možnost při použití mtouch nástroj příkazového řádku.

Neexistuje žádné uživatelské rozhraní pro tuto možnost použijte, ale jeho lze zadat v sadě Visual Studio pro dialogové okno Možnosti projektu Mac nebo v podokně vlastností projektu sady Visual Studio v **mtouch další argumenty** textové pole. (Např.) *--nolinkaway* nebude odebrat další kód (přibližně 100 kb)).

### <a name="marking-your-assembly-as-linker-ready"></a>Označení vaší sestavení jako připravené Linkeru

Uživatelé mohou vybrat pouze odkaz sestavení sady SDK a není to žádné propojení svůj kód.  To také znamená, že všechny knihovny třetích stran, které nejsou součástí základní pro Xamarin, které nebudou propojené sady SDK.

To obvykle způsobeno nechtějí ručně přidat `[Preserve]` atributy pro svůj kód.  Vedlejším účinkem je, že nebudou propojené knihovny třetích stran a jde obecně dobrý výchozí, protože není možné zjistit, zda třetích stran knihovny linkeru popisný nebo ne.

Pokud máte ve vašem projektu knihovny nebo vývojáři opakovaně použitelné knihoven a chcete linkeru do považovat za vaše sestavení korelovat, stačí je přidat atribut úrovně sestavení [ `LinkerSafe` ](https://developer.xamarin.com/api/type/Foundation.LinkerSafeAttribute/), podobné výjimky:

```csharp
[assembly:LinkerSafe]
```

Vaše knihovna tak, aby odkazovaly na Xamarin knihovny pro tuto nemusí ve skutečnosti.  Například pokud vytváříte Přenosná knihovna tříd, které poběží na jiných platformách, stále můžete `LinkerSafe` atribut.
Vyhledá linkeru Xamarin `LinkerSafe` atribut name, nikoli jeho skutečným typem.  To znamená, že napíšete tento kód a taky bude fungovat:

```csharp
[assembly:LinkerSafe]
// ... assembly attribute should be at top, before source
class LinkerSafeAttribute : System.Attribute {
    public LinkerSafeAttribute : System.base {}
}
```

## <a name="custom-linker-configuration"></a>Konfigurace vlastní Linkeru

Postupujte podle [pokyny pro vytvoření konfiguračního souboru linkeru](~/cross-platform/deploy-test/linker.md).


## <a name="related-links"></a>Související odkazy

- [Vlastní konfigurace linkeru](~/cross-platform/deploy-test/linker.md)
- [Propojení v systému Mac](~/mac/deploy-test/linker.md)
- [Propojení v systému Android](~/android/deploy-test/linker.md)
