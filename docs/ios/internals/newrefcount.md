---
title: "Nový odkaz počítání systém"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 723a9c4a052f7f432ba0f32ec501af3221b2696f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="new-reference-counting-system"></a>Nový odkaz počítání systém

Xamarin.iOS 9.2.1 zavedl odkaz na rozšířené počítání systému na všechny aplikace ve výchozím nastavení. Může sloužit k vyloučení mnoho problémů paměti, které by bylo obtížné sledovat a opravit v dřívějších verzích Xamarin.iOS.

## <a name="enabling-the-new-reference-counting-system"></a>Povolení nový odkaz počítání systému

Od verze Xamarin 9.2.1 nové ref počítání systému je povolena a **všechny** ve výchozím nastavení aplikace.

Pokud vyvíjíte existující aplikaci, můžete zkontrolovat soubor .csproj zajistit, aby všechny výskyty `MtouchUseRefCounting` jsou nastaveny na `true`, například níže:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Pokud je nastaven na hodnotu `false` aplikace nebude pomocí nového nástroje.

### <a name="using-older-versions-of-xamarin"></a>Pomocí starší verze Xamarin

Xamarin.iOS 7.2.1 a výše funkce Rozšířené náhled naše nový odkaz počítání systému.

**Klasické rozhraní API:**

Pokud chcete povolit tento nový odkaz počítání systém, zkontrolujte **použít odkaz na rozšíření počítání** políčko Najít v **Upřesnit** karta vašeho projektu **iOS sestavení možnosti** , jak je uvedeno níže: 

[![](newrefcount-images/image1.png "Povolit systém počítání nový odkaz")](newrefcount-images/image1.png#lightbox)

Všimněte si, že tyto možnosti, že byla odebrána v novějších verzích sady Visual Studio for Mac.

 **[Jednotné rozhraní API:](~/cross-platform/macios/unified/index.md)**

 Odkaz na nové počítání rozšíření je potřeba pro unifikované API a by měla být povolená ve výchozím nastavení. Starší verze vaší IDE nemusí mít tuto hodnotu automaticky zkontrolovat a možná budete muset zaškrtněte tímto sami.

    
> [!IMPORTANT]
> Starší verzi této funkce je k dispozici, protože MonoTouch 5.2 ale byla dostupná jenom pro **sgen** jako experimentální preview. Tato verze nové a vylepšené je nyní dostupná také pro **Boehm** systém uvolňování paměti.


Existuje upřednostňovaly dva typy objektů, které spravuje Xamarin.iOS: ty, které byly jenom obálku kolem objekt nativní (objektů na stejné úrovni) a rozšířené nebo začlenit i metodu nové funkce (odvozené objekty) – které obvykle tak stav velmi paměti. Dříve bylo možné, že jsme může objekt sdílené s stavu posílení (například přidáním obslužnou rutinu události C#), ale, že jsme nechat objekt přejděte neregistrované a pak shromáždění. To může způsobit selhání později (například pokud modul runtime jazyka Objective-C zpětné volání do spravovaného objektu).

Nový systém automatické upgrady rovnocenných objektů do objektů, které jsou spravovány modul runtime, když budou ukládat žádné další údaje.

Byl odstraněn různé havárií, ke kterým došlo v situacích, jako je tato:

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

Bez referenční počet rozšíření by tento kód havárií, protože `cell` stane collectible a tak jeho `TouchDown` delegáta, který se přeloží do nepropojená ukazatel.

Referenční počet rozšíření zajišťuje spravovaného objektu zůstává aktivní a brání jeho kolekce zadaný objekt nativní je uchován v portále nativního kódu.

Nový systém také odebírá potřebu, *většina* privátní zálohování poli použitými v vazeb – které je výchozí přístup, aby instance zachování připojení. Spravované linkeru je odebrat všechny takové *nepotřebné* pole z aplikace, které používají nový odkaz počet rozšíření.

To znamená, že každý spravovaný objekt instancí využívat méně paměti, než je před. Řeší také související problém, kde některé základní pole by obsahovat odkazy, které už nebyly vyžaduje modul runtime jazyka Objective-C, provedení pevný získat některé paměť.
