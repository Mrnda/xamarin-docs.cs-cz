---
title: Můžete přidat soubory do nebo odebrat soubory ze souboru IPA po sestavení v sadě Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 366308774a7302e54b0d47753256638e89d97b82
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350979"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Můžete přidat soubory do nebo odebrat soubory ze souboru IPA po sestavení v sadě Visual Studio?

Ano, je možné, ale bude obvykle vyžadovat, aby znovu podepsat `.app` sady po provedení změny.

Všimněte si, že změna `.ipa` souboru není nutné při běžném používání. Tento článek slouží výhradně k informačním účelům.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Příklad: odstranění souboru z `.ipa` archivu

V tomto příkladu se předpokládá, že je název projektu Xamarin.iOS `iPhoneApp1` a `generated session id` je `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Sestavení `.ipa` souboru ze sady Visual Studio jako obvykle.

2.  Přejít k hostiteli buildu Mac.

3.  Najít sestavení `~/Library/Caches/Xamarin/mtbs/builds` složky. Můžete vložit tuto cestu do **Finder > Přejít > přejděte do složky** k vyhledání složky ve Finderu. Vyhledejte složku, která odpovídá názvu projektu. V rámci dané složky, vyhledejte složku, která odpovídá `generated session id` sestavení. Pravděpodobně bude podsložku, která má čas poslední změny.

4.  Otevřete novou `Terminal.app` okna.

5.  Typ `cd ` do okna Terminal.app a pak přetáhněte & rozevírací `generated session id` do složky `Terminal.app` okno:

    ![](modify-ipa-images/session-id-folder.png "Umístění složky id generované relace ve Finderu.")

6.  Zadejte klávesu return, chcete-li změnit adresář na `generated session id` složky.

7.  Rozbalte `.ipa` souborů do dočasného `old/` složky pomocí následujícího příkazu. Upravit `Ad-Hoc` a `iPhoneApp1` pojmenuje podle potřeby pro konkrétní projekt.

    > kopírování - xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa staré /

8.  Zachovat `Terminal.app` otevřené okno.

9.  Odstranit z požadované soubory `.ipa`. Můžete je přesunout do koše pomocí hledání, nebo je odstranit na příkazovém řádku pomocí `Terminal.app`. Chcete-li zobrazit obsah `Payload/iPhone` souboru ve Finderu kliknutí s klávesou Control soubor a vyberte **zobrazit obsah balíčku**.

10.  Pomocí stejný obecný postup jako v kroku 3, najděte soubor protokolu v části `~/Library/Logs/Xamarin/MonoTouchVS/` , který má název projektu a `generated session id` v názvu: ![](modify-ipa-images/build-log.png "vyhledejte v protokolu sestavení projektu ve Finderu.")

11.  Otevřete protokol sestavení z kroku 10, například poklepáním.

12.  Vyhledejte řádek, který zahrnuje `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Typ `/usr/bin/codesign ` do okna Terminal.app z kroku 8.

14.  Zkopírujte všechny argumenty začínající `-v` z řádku v kroku 12 a vložte je do okna Terminal.app.

15.  Změňte poslední argument `.app` sada nachází v rámci `old/Payload/` složku a pak příkaz spusťte.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Přejděte do `old/` adresář, v terminálu:

```bash
cd old
```

17.  Zazipovat si obsah adresáře do nového `.ipa` soubor pomocí `zip` příkazu. Můžete změnit `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` argument pro výstup `.ipa` soubor bez ohledu na to vás:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Běžné chybové zprávy

Pokud se zobrazí `Invalid Signature. A sealed resource is missing or invalid.`, která obvykle znamená, že něco změnil v rámci `.app` svazku a že `.app` sady nebyl znovu podepsán správně později. Všimněte si také, pokud chcete vytvořit `.ipa` pomocí distribučního profilu, můžete _musí_ sestavení původní `.ipa` pomocí distribučního profilu. V opačném případě `Entitlements.xcent` bude nepřesná.

Poskytnout konkrétní příklad, jak k této chybě může dojít, pokud spustíte následující `codesign --verify` příkaz v okně terminálu po kroku 9, zobrazí se chyba spolu s přesnou příčinu chyby:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

A proces ověření App Store budou hlásit podobná chybová zpráva:

> Chyba ITMS-90035: "Neplatný podpis. Zapečetěné prostředek je chybějící nebo neplatný. Binární soubor v cestě [iPhoneApp1.app/iPhoneApp1] obsahuje neplatný podpis. Ujistěte se, že jste si zaregistrovali aplikace pomocí certifikátu distribučního, není certifikát ad hoc nebo certifikát pro vývoj. Ověřte, zda jsou nastavení podepisování kódu v prostředí Xcode správné na cílové úrovni (který přepsat všechny hodnoty na úrovni projektu). Kromě toho Ujistěte se, že sady, které nahráváte bylo vytvořeno prostřednictvím cílové verze v Xcode, ne cíl simulátoru. Pokud jste si jisti, že jsou správné nastavení pro podepisování kódu, zvolte "Vyčistit vše" v prostředí Xcode, odstranit adresář "sestavení" v Finder a znovu sestavte vaší cílovou verzi. Další informace, obraťte se prosím [ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
