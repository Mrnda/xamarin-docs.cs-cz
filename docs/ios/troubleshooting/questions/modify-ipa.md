---
title: Můžete přidat soubory do nebo odebrat soubory ze souboru soubor IPA po sestavení v sadě Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b8b61ba38491b2085233dd1b30a82bc57d2baaed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777928"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Můžete přidat soubory do nebo odebrat soubory ze souboru soubor IPA po sestavení v sadě Visual Studio?

Ano, je možné, ale bude obvykle vyžadují, aby znovu podepsat `.app` sady po provedení změny.

Všimněte si, že změnou `.ipa` souboru není nutné v běžném provozu. Tento článek poskytuje výhradně pro informační účely.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Příklad: odebrání soubor z `.ipa` archivu

Pro tento příklad předpokládá, že je název projektu Xamarin.iOS `iPhoneApp1` a `generated session id` je `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Sestavení `.ipa` souboru normální ze sady Visual Studio.

2.  Přepněte na Mac sestavení hostitele.

3.  Vyhledání sestavení v `~/Library/Caches/Xamarin/mtbs/builds` složky. Můžete vložit tuto cestu do **vyhledávací > přejděte > přejděte do složky** a vyhledejte složku v hledání. Vyhledejte složku, která odpovídá názvu projektu. V této složce, vyhledejte složku, která odpovídá `generated session id` sestavení. Pravděpodobně bude podsložky, která má čas poslední změny.

4.  Otevřete nový `Terminal.app` okno.

5.  Typ `cd ` do okna Terminal.app a pak přetáhněte & rozevírací `generated session id` složku do `Terminal.app` okno:

    ![](modify-ipa-images/session-id-folder.png "Umístění složky id generovaného relace v hledání")

6.  Zadejte návratové klíč, který chcete změnit adresář, do `generated session id` složky.

7.  Rozbalte `.ipa` souborů do dočasného `old/` složky pomocí následujícího příkazu. Upravit `Ad-Hoc` a `iPhoneApp1` názvy podle potřeby pro konkrétní projekt.

    > kopírování - xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa staré /

8.  Zachovat `Terminal.app` okno otevřít.

9.  Odstranit požadované soubory z `.ipa`. Můžete je přesunout do koše pomocí funkce hledání, nebo je odstranit na pomocí příkazového řádku `Terminal.app`. Chcete-li zobrazit obsah `Payload/iPhone` souboru v Finder klávesou soubor a vyberte **zobrazit obsah balíčku**.

10.  Pomocí stejné obecný přístup jako v kroku 3, najděte soubor protokolu v části `~/Library/Logs/Xamarin/MonoTouchVS/` , má název projektu a `generated session id` v názvu: ![ ] (modify-ipa-images/build-log.png "vyhledejte v protokolu sestavení projektu v hledání")

11.  Otevřete protokolu sestavení z kroku 10, například poklepáním.

12.  Najít řádek, který zahrnuje `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Typ `/usr/bin/codesign ` do okna Terminal.app z kroku 8.

14.  Zkopírujte všechny argumenty začínající `-v` z řádku v kroku 12 a vložte je do okna Terminal.app.

15.  Změnit poslední argument jako `.app` sady umístěné v rámci `old/Payload/` složku a potom spusťte příkaz.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Přejděte do `old/` adresář v terminálu:

```bash
cd old
```

17.  ZIP si obsah adresáře do nového `.ipa` souboru pomocí `zip` příkaz. Můžete změnit `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` argument pro výstup `.ipa` souboru vždy, když chcete:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Běžné chybové zprávy

Pokud se zobrazí `Invalid Signature. A sealed resource is missing or invalid.`, to obvykle znamená, že něco byl změněn v rámci `.app` sady a že `.app` sady nebyl znovu správně podepsaný později. Rovněž Pamatujte, že pokud chcete vytvořit `.ipa` k profilu distribuční jste _musí_ sestavení původní `.ipa` s profil distribuce. V opačném případě `Entitlements.xcent` nesprávné.

Poskytnout konkrétní příklady jak této chyby mohou nastat, pokud spustíte následující `codesign --verify` příkazu v okně terminálu po kroku 9 se zobrazí chyba společně s přesnou příčinu chyby:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

A proces ověření obchod oznámí podobná chybová zpráva:

> Chyba ITMS-90035: "Neplatný podpis. Zapečetěné prostředků je chybějící nebo neplatná. Binární soubor v cestě [iPhoneApp1.app/iPhoneApp1] obsahuje neplatný podpis. Ujistěte se, že jste se zaregistrovali vaší aplikace pomocí certifikátu distribučního, není certifikát ad hoc nebo vývojový certifikát. Ověřte, zda jsou nastavení pro podepisování kódu v Xcode správné na cílové úrovni (který potlačí všechny hodnoty na úrovni projektu). Kromě toho Ujistěte se, že sady, které odesíláte bylo vytvořeno prostřednictvím cílovou verzi v Xcode, není simulátoru cíl. Pokud jste si jisti, že jsou správná nastavení pro podepisování kódu, zvolte "Vyčistit všechny" v Xcode, odstranit adresář "sestavení" v nástroji hledání a znovu sestavte cílových verze. Další informace, přečtěte si [ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
