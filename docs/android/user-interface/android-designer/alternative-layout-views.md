---
title: "Alternativní rozložení zobrazení"
description: "Toto téma vysvětluje, jak rozložení může být verzí pomocí kvalifikátory prostředků. Například může existovat verzi rozložení, který se používá pouze, když je zařízení v režimu na šířku a rozložení verzi, která je jen pro režim na výšku."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: c2df60a79ea3b5a0ff226cfaade0440db13fd5ea
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="alternative-layout-views"></a>Alternativní rozložení zobrazení

_Toto téma vysvětluje, jak rozložení může být verzí pomocí kvalifikátory prostředků. Například může existovat verzi rozložení, který se používá pouze, když je zařízení v režimu na šířku a rozložení verzi, která je jen pro režim na výšku._


## <a name="creating-alternative-layouts"></a>Vytváření alternativní rozložení

Když kliknete **alternativní rozložení zobrazení** ikona (nalevo od **zařízení**), otevře se podokno náhledu seznam alternativní rozložení, které jsou k dispozici ve vašem projektu. Pokud neexistují žádné alternativní rozložení **výchozí** zobrazení se zobrazí: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![V podokně zobrazení alternativní rozložení](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "podokno zobrazení alternativní rozložení")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![V podokně zobrazení alternativní rozložení](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

-----

Když kliknete na zelený symbol plus vedle **novou verzi**, **vytvořte variace rozložení** otevře se dialogové okno, ve kterém můžete vybrat kvalifikátory prostředků pro tuto variantu rozložení: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vytvořte variace rozložení](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "vytvořit variantu rozložení")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vytvořte variace rozložení](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

-----


V následujícím příkladu, kvalifikátor prostředků pro **orientace obrazovky** je nastaven na **na šířku**a **velikost obrazovky** se změní na **velké**. Tím se vytvoří nová verze rozložení s názvem **velké krajině**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Velké krajině variace](alternative-layout-views-images/vs/03-large-land-sml.png "velké krajině variace")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Velké krajině variace](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

-----


Všimněte si, že zobrazí v podokně náhledu na levé straně důsledky výběr kvalifikátor prostředků. Kliknutím na tlačítko **přidat** vytvoří alternativní rozložení a návrháři k přepnutí do tohoto rozložení. **Alternativní rozložení zobrazení** podokno náhledu určuje, jaké rozložení byste je načten do návrháře prostřednictvím malé správné ukazatel, jak je uvedeno v následující snímek obrazovky: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Indikátor načíst rozložení](alternative-layout-views-images/vs/04-new-layout-sml.png "indikátor načíst rozložení")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Indikátor načíst rozložení](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

-----



## <a name="editing-alternative-layouts"></a>Úpravy alternativní rozložení

Když vytvoříte alternativní rozložení, je často žádoucí, aby jediné změny, která se vztahuje na všechny forked verze rozložení. Můžete například změnit text tlačítka na žlutý ve všech rozložení. Pokud máte velký počet rozložení, údržby rychle stát náročná a náchylný Pokud potřebujete rozšířit jediné změny pro všechny verze. 

Pro zjednodušení správy více verzí rozložení, poskytuje návrháře **více upravit** režimu, které šíří změny mezi jeden nebo více rozložení. Když se nachází více než jeden rozložení **více upravit** , zobrazí se ikona: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ikonu pro úpravu více](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "ikonu pro úpravu více")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Ikona více úpravy](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

-----


Když kliknete **více upravit** ikonu, se zobrazí řádky, které označují, že jsou rozložení propojení (jak je znázorněno níže); to znamená, když změníte jeden rozložení, tato změna se rozšíří do žádné propojené rozložení. Kliknutím na ikonu v kroužku uvedené v následující snímek obrazovky můžete odpojit všechny rozložení: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zrušit propojení všech rozložení](alternative-layout-views-images/vs/06-multi-linked-sml.png "zrušit propojení všech rozložení")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zrušit propojení všech rozložení](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

-----


Pokud máte více než dvě rozložení, můžete selektivně přepínat na tlačítko Upravit vlevo od jednotlivých verzí preview rozložení k určení rozložení, které jsou spojeny dohromady. Pokud chcete, aby jediné změny, které šíří například první a poslední tři rozložení je by nejprve zrušit střední rozložení jak je vidět tady: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zrušit propojení střední rozložení](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "střední rozložení zrušit propojení")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zrušit propojení střední rozložení](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)
 
-----
 

V tomto příkladu ke změně buď **výchozí** nebo **dlouho** rozložení se rozšíří na jiné rozložení, ale nikoli k **velké krajině** rozložení. 



### <a name="multi-edit-example"></a>Příklad více úpravy 

Obecně platí když změníte jeden rozložení, tato změna stejné rozšíří do všech ostatních propojené rozložení. Například přidávání nové `TextView` pomůcka k **výchozí** rozložení a změna jeho textového řetězce na `Portrait` způsobí stejné změnu provést všechny propojené rozložení. Zde je, jak vypadá **výchozí** rozložení: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Přidat TextView](alternative-layout-views-images/vs/08-add-textview-sml.png "přidat TextView")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Přidat TextView](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)
 
-----
 

`TextView` Je taky přidaný ke **velké krajině** rozložení zobrazit, protože je propojena **výchozí** rozložení: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Na šířku TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "šířku TextView")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Na šířku TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
-----
 

Ale co když budete chtít provést změnu, která je místní pro pouze jedno rozložení (to znamená, že nechcete, aby změny mohly rozšířit do žádné jiné rozložení)? Chcete-li to provést, je nutné zrušit rozložení, který chcete změnit před zahájením úprav, jak je popsáno dále. 



### <a name="making-local-changes"></a>Provedení místní změn 

Předpokládejme, že chceme obou rozložení tak, aby měl přidaném `TextView`, ale chceme také změnit textového řetězce v **velké krajině** rozvržení `Landscape` místo `Portrait`. Pokud jsme Ujistěte se, aby se provedená změna **velké krajině** při obou rozložení jsou propojené, změna rozšíří zpět **výchozí** rozložení. Proto jsme musíte nejprve zrušit propojení dvě rozložení před jsme proveďte požadovanou změnu. Když jsme upravit text v **velké krajině** k `Landscape`, Návrhář označí tuto změnu s červeným rámečkem k označení, že je místní pro změnu **velké krajině** rozložení a je *není* rozšíří zpět **výchozí** rozložení: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Místní změny](alternative-layout-views-images/vs/10-local-change-sml.png "místní změny")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Místní změny](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)
 
-----
 

Když kliknete **výchozí** rozložení zobrazení, `TextView` textový řetězec je stále nastavené `Portrait`. 



## <a name="handling-conflicts"></a>Zpracování konfliktů 

Pokud se rozhodnete změnit barvu textu v **výchozí** rozložení na zelenou, zobrazí se ikona upozornění se zobrazí na propojené rozložení. Klepnutím na tomto rozložení otevřete rozložení a odhalit konflikt. Zvýrazní pomůcky, který způsobil konflikt s červeným rámečkem a zobrazí se následující zpráva: *nedávné změny způsobit konflikty v tomto alternativní rozložení*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konfliktní změny](alternative-layout-views-images/vs/11-conflicting-change-sml.png "konfliktní změny")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Konfliktní změny](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)
 
-----
 

A *konflikt pole* se zobrazí na pravé straně widgetu vysvětlit konflikt: 

[![Konflikt upozornění](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

Pole konflikt zobrazí seznam vlastností, které se změnily a vypíše jejich hodnot. Kliknutím na tlačítko **Ignorovat konflikt** změnit vlastnost se vztahuje pouze na této pomůcky. Kliknutím na tlačítko **použít** změnit vlastnost se vztahuje na tato pomůcka také tak, aby widgetu protějškem v propojenou **výchozí** rozložení. Pokud jsou použity všechny změny vlastností, je automaticky zahodí konflikt. 


### <a name="view-group-conflicts"></a>Zobrazení skupiny konfliktu 

Změny vlastností nejsou jediný zdroj je v konfliktu. Při vkládání nebo odebrání pomůcky můžete zjistil konflikt. Například, když **velké krajině** rozložení je odpojení od **výchozí** rozložení a `TextView` v **velké krajině** rozložení přetáhnout výše `Button`, Návrhář označí přesunutý pomůcka k označení konflikt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zobrazení skupiny konfliktu](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "zobrazit skupiny konfliktu")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Zobrazení skupiny konfliktu](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
-----
 

Neexistuje však žádný značky na `Button`. I když pozici `Button` došlo ke změně `Button` zobrazuje žádné použité změny, které jsou specifické pro **velké krajině** konfigurace. 

Pokud `CheckBox` je přidán do **výchozí** rozložení, je generován jiný konfliktu a ikona upozornění se zobrazí nad **velké krajině** rozložení: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zaškrtávací políčko konflikt](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "konflikt zaškrtávací políčko")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Konflikt zaškrtávací políčko](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
-----
 

Kliknutím **velké krajině** rozložení se zobrazí konflikt. Zobrazí se následující zpráva: *nedávné změny způsobit konflikty v tomto alternativní rozložení*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ALT rozložení konflikt](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "konflikt Alt rozložení")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Konflikt ALT rozložení](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
-----
 

Kromě toho pole konflikt zobrazí následující zprávu:

[![Zpráva o konfliktu](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Přidávání `CheckBox` způsobuje konflikt v podobě, protože **velké krajině** rozložení obsahuje změny `LinearLayout` který ji obsahuje. Ale v takovém případě pole konflikt zobrazuje pomůcky, který byl právě vložen do **výchozí** rozložení ( `CheckBox`).

Pokud kliknete na tlačítko **Ignorovat konflikt**, návrháře vyřeší konflikt, povolení widgetu zobrazen v poli konflikt k přetahování do rozložení, kde chybí widgetu (v takovém případě **velké krajině**  rozložení): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vyřešit konflikt skupiny](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "vyřešit konflikt skupiny")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vyřešit konflikt skupiny](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
-----
 

Jak je vidět v předchozím příkladu s `Button`, `CheckBox` nemá značku red změnit, protože pouze `LinearLayout` obsahuje změny, které byly použity v **velké krajině** rozložení.



### <a name="conflict-persistence"></a>Trvalost konflikt

Konflikty zůstávají v souboru rozložení jako komentáře XML, jak je vidět tady:

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

Proto když na projekt se zavře a znovu otevřít, všechny konflikty, i nadále existovat &ndash; i ty, které byly ignorovány.

