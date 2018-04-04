---
title: Propojení aplikace v Android
description: Tato příručka popisuje jak Android 6.0 podporuje aplikace propojení, technika, který umožňuje mobilní aplikace reagovat na adresy URL na webech. Bude popisují, jaké aplikace propojení je, jak implementovat aplikaci propojení v aplikaci Android 6.0 a jak nakonfigurovat web, který chcete udělit oprávnění k mobilní aplikaci pro doménu.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2ef6b8044387d759e26d05c1468caaad7efb9bdc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="app-linking-in-android"></a>Propojení aplikace v Android

_Tato příručka popisuje jak Android 6.0 podporuje aplikace propojení, technika, který umožňuje mobilní aplikace reagovat na adresy URL na webech. Bude popisují, jaké aplikace propojení je, jak implementovat aplikaci propojení v aplikaci Android 6.0 a jak nakonfigurovat web, který chcete udělit oprávnění k mobilní aplikaci pro doménu._

## <a name="app-linking-overview"></a>Přehled aplikace propojení

Mobilní aplikace, které už existují v sila &ndash; v mnoha případech jsou důležitou součástí své firmy, spolu s jejich webu. Je žádoucí, aby podniky bezproblémově připojit jejich přítomnosti webové a mobilní aplikace, s odkazy na webu spuštěním mobilní aplikace a zobrazení souvisejícího obsahu v mobilní aplikaci. *Propojení aplikace* (také označuje jako *propojení přímým*) je jedna metoda, která umožňuje mobilního zařízení a reagovat na identifikátor URI a spustit mobilní aplikace, která odpovídá na tento identifikátor URI.

Android zpracovává aplikace propojení prostřednictvím *záměrné systému* &ndash; když uživatel klikne na odkaz v prohlížeč pro mobilní zařízení, prohlížeč pro mobilní zařízení bude dispatch záměrem, která budou delegovat Android k registrované aplikaci. Například kliknutím na odkaz na vaření webu by otevřít mobilní aplikace, který je přidružený tento web a zobrazit konkrétní recepturách uživateli. Pokud je pro zpracování této záměr registrována více než jednu aplikaci, pak Android vyvolá, která se označuje jako *rozlišení více tras dialogu* , požádá uživatele vyberte aplikaci, která by měla řídit záměr, pro jaké aplikace Příklad:

![Příklad snímek obrazovky dialogového okna rozlišení více tras](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 zlepšuje v tomto pomocí automatické propojení zpracování. Je možné pro Android na automatickou registraci aplikace jako výchozí obslužnou rutinu pro identifikátor URI &ndash; aplikace se automaticky spustí a přejít přímo na příslušné aktivity. Jak se systémem Android 6.0 rozhodne zpracování a identifikátor URI klikněte závisí na následujících kritérií:

1. **Stávající aplikace je již přidružen identifikátor URI** &ndash; uživatel může mít již přidružené stávající aplikace s identifikátorem URI. Android se v takovém případě bude nadále používat tuto aplikaci.
2. **Žádné existující aplikace je přidružený identifikátor URI, ale instalaci podpůrných aplikace** &ndash; v tomto scénáři uživatel nebyl zadán existující aplikaci, tak Android použije nainstalovaná podporující aplikace pro zpracování požadavku.
3. **Žádné existující aplikace je přidružený identifikátor URI, ale mnoho podpůrné aplikace jsou nainstalované** &ndash; vzhledem k tomu, že existuje více aplikací, které podporují identifikátor URI, zobrazí se dialogové okno rozlišení více tras a uživatel musí vybrat, které aplikace budou zpracování identifikátor URI.

Pokud má uživatel nainstalované žádné aplikace podporující identifikátor URI, a následně je nainstalován, pak Android nastaví tuto aplikaci jako výchozí obslužnou rutinu pro identifikátor URI po ověření přidružení k webu, který je přidružený identifikátor URI.

Tato příručka popisuje postupy pro konfiguraci aplikace Android 6.0 a postup vytvoření a publikování souboru digitální Asset odkazů pro podporu aplikace propojení v Android 6.0.

## <a name="requirements"></a>Požadavky

Tato příručka vyžaduje Xamarin.Android 6.1 a aplikace, která cílí Android 6.0 (úroveň rozhraní API 23) nebo vyšší.

Propojení aplikace je možné v dřívějších verzích systému Android pomocí [balíček Rivets NuGet](https://www.nuget.org/packages/Rivets/) z úložišti součástí Xamarin. Balíček nýty není kompatibilní s aplikací propojení v Android 6.0; nepodporuje propojování aplikace Android 6.0.

## <a name="configuring-app-linking-in-android-60"></a>Konfigurace propojení aplikace v systému Android 6.0

Nastavení aplikace – odkazy v Android 6.0 zahrnuje dva hlavní kroky:

1. **Přidání jednoho nebo více záměr filtrů pro web URI** &ndash; záměrné filtry Průvodce Android v určuje způsob zpracování, klikněte adresu URL v prohlížeč pro mobilní zařízení.
2. **Publikování *digitální JSON odkazy Asset* soubor na webu** &ndash; jedná o soubor, který je odeslán na web a Android používá k ověření vztahu mezi domény webu a mobilní aplikace. Bez tohoto Android nelze nainstalovat aplikaci jako výchozí popisovač pro URI; Uživatel musí to provést ručně.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Konfigurace záměrné filtru

Je nutné konfigurovat záměrné filtr, který se mapuje identifikátor URI (nebo možné sadu identifikátory URI) z webu na aktivitu v aplikaci Android. V Xamarin.Android, je tento vztah zřízena adorning aktivitu [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Záměrné filtru musí deklarovat následující informace:

* **`Intent.ActionView`** &ndash; To se registrují záměrné filtr reagovat na požadavky zobrazení informací o
* **`Categories`** &ndash;  Záměrné filtr měli zaregistrovat i **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)** a **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)** moct správně zpracování webu identifikátor URI.
* **`DataScheme`** &ndash; Je třeba deklarovat záměrné filtr `http` nebo `https`. Jsou to jenom dva platná schémata.
* **`DataHost`** &ndash; Toto je doména, která identifikátory URI budou pocházet z.
* **`DataPathPrefix`** &ndash; Toto je volitelná cesta k prostředkům na webu.
* **`AutoVerify`** &ndash; `autoVerify` Atribut informuje Android ověření vztahu mezi aplikací a webu. To bude možné uvedeny další níže.

Následující příklad ukazuje, jak používat [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) pro zpracování odkazy z `https://www.recipe-app.com/recipes` a z `http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe"),
              AutoVerify=true]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android ověří, že každý hostitel, která je identifikovaná záměrné filtry proti digitálnímu souboru prostředků na webu před registrací aplikace jako výchozí obslužnou rutinu pro identifikátor URI. Všechny záměrné filtry musí projít ověření, než Android můžete vytvořit aplikace jako výchozí obslužnou rutinu.

### <a name="creating-the-digital-assets-link-file"></a>Vytvoření souboru odkaz digitálním materiálům

Android 6.0 propojení aplikace vyžaduje, aby Android ověřili přidružení mezi aplikací a web před nastavením aplikace jako výchozí obslužnou rutinu pro identifikátor URI. Toto ověření se stane při první instalaci aplikace. *Digitální odkazy prostředky* soubor je soubor JSON, který je hostitelem příslušných webdomain(s).

> [!NOTE]
> `android:autoVerify` Musí být nastaven záměrné filtr &ndash; jinak nebude Android provádět ověření.

Správce webového serveru domény do umístění vloží soubor **https://domain/.well-known/assetlinks.json**.

Soubor digitální Asset obsahuje nezbytné metadata pro Android ověření přidružení. **Assetlinks.json** soubor má následující páry klíč hodnota:

* `namespace` &ndash; obor názvů aplikace pro Android.
* `package_name` &ndash; Název balíčku aplikace pro Android (deklarované v manifestu aplikace).
* `sha256_cert_fingerprints` &ndash; SHA256 otisky prstů podepsanou aplikaci. Naleznete v průvodci [hledání MD5 nebo SHA1 podpis vašeho úložiště klíčů](~/android/deploy-test/signing/keystore-signature.md) Další informace o tom, jak získat otisk prstu SHA1 aplikace.

Následující fragment kódu je příkladem **assetlinks.json** s jednu aplikaci uvedené:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

Je možné registrovat víc než jeden otisk prstu SHA256 pro podporu různých verzí nebo sestavení vaší aplikace. Tato další **assetlinks.json** soubor je příklad registrace více aplikací:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

[Webu Google digitální Asset odkazy](https://developers.google.com/digital-asset-links/tools/generator) má online nástroj, který může pomoct s vytváření a testování soubor digitálních materiálů.

### <a name="testing-app-links"></a>Testování aplikace odkazy

Po implementaci odkazů na aplikace, musí být různých částí testovány zajistit, že fungují podle očekávání.

Je možné potvrdit, že soubor digitálních materiálů je správně naformátován a hostované pomocí Google digitální Asset odkazy rozhraní API, jak je uvedeno v následujícím příkladu:

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Existují dva testy, které lze provést k zajištění, že záměrné filtry byly správně nakonfigurovány a zda je aplikace nastavena jako výchozí obslužnou rutinu pro identifikátor URI:

1.  Digitální souboru prostředku je hostován správně, jak je popsáno výše. První test bude dispatch záměrem, které by se měla přesměrovat mobilní aplikace Android. Aplikace pro Android by měla spustit a zobrazit aktivity zaregistrovaný pro adresu URL. Na příkazovém řádku zadejte:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  Zobrazte stávající odkaz zpracování zásady pro aplikace nainstalované na daném zařízení. Následující příkaz bude dump seznam odkaz zásady pro každého uživatele v zařízení s následujícími informacemi. V příkazovém řádku zadejte následující příkaz:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; Název balíčku aplikace.
    * **`Domain`** &ndash; Domén (oddělených mezerami), jejíž webové odkazy budou aplikace zpracovávat
    * **`Status`** &ndash; Toto je aktuální stav zpracování odkaz pro aplikaci. Hodnota **vždy** znamená, že aplikace má `android:autoVerify=true` deklarovaný a byla úspěšná ověření systému. Následuje o hexadecimální číslo představující záznam předvolba systému Android.

    Příklad:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Souhrn

Tato příručka popsané, jak aplikaci propojení funguje Android 6.0. Zahrnuté pak postupy pro konfiguraci aplikace Android 6.0 podporu a reagovat na odkazy v aplikaci. Jsou zde popsány i testování aplikace propojení v aplikaci pro Android.


## <a name="related-links"></a>Související odkazy

- [Hledání MD5 nebo SHA1 podpis vašeho úložiště klíčů](~/android/deploy-test/signing/keystore-signature.md)
- [Aktivity a záměry](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Digitální Google prostředky odkazy](https://developers.google.com/digital-asset-links/)
- [Příkaz seznamu generátor a testování](https://developers.google.com/digital-asset-links/tools/generator)
