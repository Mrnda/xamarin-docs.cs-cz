---
title: Soubor iTunesMetadata.plist
description: "Tento článek se zabývá iTunesMetadata.plist soubor použitý k zadání informací pro službu iTunes o používání Ad Hoc distribuce pro testování nebo podnikové nasazení aplikace pro iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 70676eba-6a99-4a3a-bccc-84359fe9c2c3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3bdf00a9e50b2bf66f51c825306c2ba8e6365dd2
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="the-itunesmetadataplist-file"></a>Soubor iTunesMetadata.plist

_Tento článek se zabývá iTunesMetadata.plist soubor použitý k zadání informací pro službu iTunes o používání Ad Hoc distribuce pro testování nebo podnikové nasazení aplikace pro iOS._

Vytvoření aplikace pro iOS v iTune Connect (buď pro prodej nebo bezplatnou verzi z obchodu iTunes App) má vývojář můžete zadat informace, jako je aplikace genre, dílčí genre, o autorských právech, zařízení podporovaných iOS a vyžaduje zařízení Možnosti. Pro iOS aplikace, které se dodávají buď testery nebo Enterprise uživatele prostřednictvím ad hoc distribuce tyto informace chybí.

Chcete-li zadat chybějící informace do Ad Hoc distribuce, volitelný `iTunesMetadata.plist` souboru můžete vytvořit a součástí aplikace IPA souboru. Tento soubor plist je speciálně formátovaný soubor XML (viz společnosti Apple [Průvodce programováním v seznamu vlastností](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html) Další informace) obsahující dvojice klíč/hodnota definování informace o aplikaci dané iOS.

<a name="iTunesMetadata_contents" />

## <a name="the-itunesmetadataplist-contents"></a>Obsah iTunesMetadata.plist

Následuje příklad z typických `iTunesMetadata.plist` soubor používá k definování iTunes informace pro Ad Hoc distribuci:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>UIRequiredDeviceCapabilities</key>
    <dict>
        <key>armv7</key>
        <true/>
        <key>front-facing-camera</key>
        <true/>
    </dict>
    <key>artistName</key>
    <string>Company, Inc.</string>
    <key>bundleDisplayName</key>
    <string>App Name</string>
    <key>bundleShortVersionString</key>
    <string>1.5.1</string>
    <key>bundleVersion</key>
    <string>1.5.1</string>
    <key>copyright</key>
    <string>© 2015 Company, Inc.</string>
    <key>drmVersionNumber</key>
    <integer>0</integer>
    <key>fileExtension</key>
    <string>.app</string>
    <key>gameCenterEnabled</key>
    <false/>
    <key>gameCenterEverEnabled</key>
    <false/>
    <key>genre</key>
    <string>Games</string>
    <key>genreId</key>
    <integer>6014</integer>
    <key>itemName</key>
    <string>App Name</string>
    <key>kind</key>
    <string>software</string>
    <key>playlistArtistName</key>
    <string>Company, Inc.</string>
    <key>playlistName</key>
    <string>App Name</string>
    <key>releaseDate</key>
    <string>2015-11-18T03:23:10Z</string>
    <key>s</key>
    <integer>143441</integer>
    <key>softwareIconNeedsShine</key>
    <false/>
    <key>softwareSupportedDeviceIds</key>
    <array>
        <integer>9</integer>
    </array>
    <key>softwareVersionBundleId</key>
    <string>com.company.appid</string>
    <key>subgenres</key>
    <array>
        <dict>
            <key>genre</key>
            <string>Puzzle</string>
            <key>genreId</key>
            <integer>7012</integer>
        </dict>
        <dict>
            <key>genre</key>
            <string>Word</string>
            <key>genreId</key>
            <integer>7019</integer>
        </dict>
    </array>
    <key>versionRestrictions</key>
    <integer>16843008</integer>
</dict>
</plist>

```

Hodnoty pro jednotlivé klíče se budeme rozpis naleznete níže.

### <a name="uirequireddevicecapabilities"></a>UIRequiredDeviceCapabilities

`UIRequiredDeviceCapabilities` Umožňuje iTunes vědět, které zařízení specifické funkce aplikace pro iOS vyžaduje předtím, než se dá nainstalovat na zařízení iOS daný klíč. Je poskytována jako slovník (`<dict>...</dict>`) funkcí (`<key>...</key>`) a logickou hodnotu pro každou funkci. Pokud je hodnota funkce `true`, pak tato funkce musí být přítomen. Pokud je `false` funkce nesmí být v zařízení nenachází. Příklad:

```xml
<key>UIRequiredDeviceCapabilities</key>
<dict>
    <key>armv7</key>
    <true/>
    <key>front-facing-camera</key>
    <true/>
</dict>
```
Určuje, že zařízení s iOS musí podporovat ARM7 instrukce nastavit tak, aby před přístupem fotoaparát před instalací této aplikace na zařízení. Úplný seznam povolených hodnot, najdete v tématu společnosti Apple [UIRequiredDeviceCapabilities](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) dokumentaci.

### <a name="artistname-and-playlistartistname"></a>artistName a playlistArtistName

Použití `artistName` a `playlistArtistName` klíče můžete definovat název společnosti, která vytvořila aplikace systému iOS, která se zobrazí v iTunes. Příklad:

```xml
<key>artistName</key>
<string>Company, Inc.</string>
...
<key>playlistArtistName</key>
<string>Company, Inc.</string>
```

### <a name="bundledisplayname-itemname-and-playlistname"></a>bundleDisplayName, název položky a playlistName

Použití `bundleDisplayName`, `itemName`, a `playlistName` klíče můžete definovat název aplikace iOS, která se zobrazí uvnitř iTunes. Příklad:

```xml
<key>bundleDisplayName</key>
<string>App Name</string>
...
<key>itemName</key>
<string>App Name</string>
...
<key>playlistName</key>
<string>App Name</string>
```

### <a name="bundleshortversionstring-and-bundleversion"></a>bundleShortVersionString a bundleVersion

Použití `bundleShortVersionString` a `bundleVersion` klíče zadat číslo verze aplikace iOS, která se zobrazí v iTunes. Příklad:

```xml
<key>bundleShortVersionString</key>
<string>1.5.1</string>
<key>bundleVersion</key>
<string>1.5.1</string>
```

### <a name="softwareversionbundleid"></a>softwareVersionBundleId

Použití `softwareVersionBundleId` klíč k určení ID sady prostředků pro aplikace systému iOS. Příklad:

```xml
<key>softwareVersionBundleId</key>
<string>com.company.appid</string>
```

### <a name="copyright"></a>Copyright

Použití `copyright` klíč k definování o autorských právech, který se zobrazí v iTunes. Příklad:

```xml
<key>copyright</key>
<string>© 2015 Company, Inc.</string>
```

### <a name="releasedate"></a>releaseDate

Použití `releaseDate` klíče zadejte datum vydání pro aplikace iOS, která se zobrazí v iTunes. Příklad:

```xml
<key>releaseDate</key>
<string>2015-11-18T03:23:10Z</string>
```

### <a name="softwareiconneedsshine"></a>softwareIconNeedsShine

Použití `softwareIconNeedsShine` klíč říct iTunes, pokud vyžaduje ikona aplikace iOS _nám zvýraznění_ pro iOS 6 (a před). Příklad:

```xml
<key>softwareIconNeedsShine</key>
<false/>
```

### <a name="gamecenterenabled-and-gamecentereverenabled"></a>gameCenterEnabled a gameCenterEverEnabled

Použití `gameCenterEnabled` a `gameCenterEverEnabled` klíče říct iTunes je tato aplikace iOS podporuje herní centrum společnosti Apple. Příklad:

```xml
<key>gameCenterEnabled</key>
<false/>
<key>gameCenterEverEnabled</key>
<false/>
```

### <a name="genre-genreid-and-subgenres"></a>genre, genreId a subgenres

Použití `genre` a `genreId` klíče říct iTunes jaké genre, patří aplikace systému iOS. Příklad:

```xml
<key>genre</key>
<string>Games</string>
<key>genreId</key>
<integer>6014</integer>
```

Volitelně můžete `subgenres` klíč lze dále určit až dvě dílčí žánry pro aplikace systému iOS. Příklad:

```xml
<key>subgenres</key>
<array>
    <dict>
        <key>genre</key>
        <string>Puzzle</string>
        <key>genreId</key>
        <integer>7012</integer>
    </dict>
    <dict>
        <key>genre</key>
        <string>Word</string>
        <key>genreId</key>
        <integer>7019</integer>
    </dict>
</array>
```

Pro aplikace pro iOS Apple aktuálně definuje následující žánry a genre ID:

[!include[](~/ios/includes/table-appstore.md)]

Další informace najdete v tématu společnosti Apple [Genre ID příloha](http://www.apple.com/itunes/affiliates/resources/documentation/genre-mapping.html) dokumentaci.

### <a name="softwaresupporteddeviceids"></a>softwareSupportedDeviceIds

Použití `softwareSupportedDeviceIds` klíč iTunes zjistit, jaké zařízení se systémem iOS podporuje tato aplikace iOS. Příklad:

```xml
<key>softwareSupportedDeviceIds</key>
<array>
    <integer>9</integer>
</array>
```

Kde jsou k dispozici následující hodnoty:

- 1 – Iphony classic
- 2 – iPod Touch
- 4 – iPad
- 9 – moderní Iphony

### <a name="standard-keys"></a>Standardní klíče

Tyto klíče jsou zahrnuté ve všech `iTunesMetadata.plist` soubory pro aplikace pro iOS a vždy mají stejné hodnoty:

```xml
<key>drmVersionNumber</key>
<integer>0</integer>
<key>fileExtension</key>
<string>.app</string>
...
<key>kind</key>
<string>software</string>
...
<key>s</key>
<integer>143441</integer>
...
<key>versionRestrictions</key>
<integer>16843008</integer>
```

<a name="iTunesMetadata_creating" />

## <a name="creating-an-itunesmetadataplist-file"></a>Vytvoření souboru iTunesMetadata.plist

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Při práci s `iTunesMetadata.plist` souborů v sadě Visual Studio pro Mac, máte dvě možnosti:

- Vytváření a údržbu soubor pomocí sady Visual Studio pro Mac na editor visual plist.
- Vytváření a údržbu souboru v editoru prostého textu.

 Obě možnosti se bude vztahovat rozpis naleznete níže.

### <a name="using-the-visual-plist-editor"></a>V editoru Visual Plist.

Postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na soubor projektu Xamarin.iOS a vyberte **přidat** > **nový soubor...**
2. Z tohoto dialogového okna nový soubor vyberte **iOS** > **seznam vlastností**:

    ![](itunesmetadata-images/image01.png "Vyberte iOS seznam vlastností")
3. Zadejte `iTunesMetadata` pro **název** a klikněte na **nový** tlačítko.
4. Dvakrát klikněte `iTunesMetadata.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy:

    ![](itunesmetadata-images/image02.png "ITunesMetadata.plist editor")
5. Klikněte na tlačítko se zeleným  **+**  chcete vytvořit novou položku a zadejte `UIRequiredDeviceCapabilities` jako název klíče:

    ![](itunesmetadata-images/image03.png "Vytvořte novou položku a zadejte UIRequiredDeviceCapabilities jako název klíče")
6. Klikněte na **řetězec** hodnota typu a vyberte **slovník** v automaticky otevřeném okně seznamu:

    ![](itunesmetadata-images/image04.png "Vyberte ze seznamu místní slovník")
7. Klikněte na tlačítko turndown v levé části názvu vlastnosti a odhalit položky slovníku je:

    ![](itunesmetadata-images/image05.png "Odhalit položky slovníku")
8. Klikněte na **přidat novou položku** text, pak klikněte na tlačítko zeleným  **+**  přidejte položku do slovníku:

    ![](itunesmetadata-images/image06.png "Přidat položku do slovníku")
9. Zadejte `armv7` název klíče, vyberte typ **Boolean** a zadejte **Ano** jako hodnotu:

    ![](itunesmetadata-images/image07.png "Zadejte název klíče armv7, výběr typu logická hodnota a zadejte jako hodnotu Ano")
10. Opakujte předchozí kroky, dokud jste vyplnili `iTunesMetadata.plist` souboru se všemi dvojic klíč/hodnota vyžaduje (v tématu [iTunesMetadata.plist obsah](#iTunesMetadata_contents) části výše další podrobnosti).

11. Uložte změny do souboru plist.

### <a name="using-a-plain-text-editor"></a>Pomocí editoru prostého textu

Postupujte takto:

1. V editoru prostého textu, vytvořte nový textový soubor s názvem `iTunesMetadata.plist`.
2. Zkopírujte obsah příklad z [obsah iTunesMetadata.plist](#iTunesMetadata_contents) část výše.
3. Umožňuje vložit obsah v souboru a je upravit podle potřeby.
4. Uložte tento soubor a vrátit k sadě Visual Studio for Mac.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na soubor projektu Xamarin.iOS a vyberte **přidat** > **existujících souborů...** .
6. V dialogovém okně Otevřít soubor, vyberte `iTunesMetadata.plist` souboru, který byl vytvořen výše a klikněte **OK** tlačítko.
7. Ponechte **akce sestavení** tohoto souboru nastavit na **žádné**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Modul plug-in Xamarin pro Visual Studio podporuje pouze o vizuální editor pro `Info.plist` a `Entitlement.plist` souborům, takže budete muset vytvořit vaší `iTunesMetadata.plist` souboru v standardního textového editoru a ručně ho zahrňte ve vašem projektu Xamarin.iOS.

Postupujte takto:

1. V editoru prostého textu, vytvořte nový textový soubor s názvem `iTunesMetadata.plist`.
2. Zkopírujte obsah příklad z [obsah iTunesMetadata.plist](#iTunesMetadata_contents) část výše.
3. Umožňuje vložit obsah v souboru a je upravit podle potřeby.
4. Uložte soubor a vraťte se k sadě Visual Studio.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na soubor projektu Xamarin.iOS a vyberte **přidat** > **existujících souborů...** .
6. V dialogovém okně Otevřít soubor, vyberte `iTunesMetadata.plist` souboru, kterou jste vytvořili výše a klikněte na tlačítko **otevřete** tlačítko.
7. Ponechte **akce sestavení** tohoto souboru nastavit na **žádné**.

-----

Později, tuto možnost vyberte, `iTunesMetadata.plist` souboru při přípravě k sestavení váš soubor IPA je v prostředí IDE.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých `iTunesMetadata.plist` soubor, který slouží k říct iTunes o ad hoc doručit aplikace systému iOS. Ho má popsané standardní klíč v souboru plist a jak vytvořit a udržovat souboru v sadě Visual Studio a Visual Studio for Mac.

## <a name="related-links"></a>Související odkazy

- [Distribuce v obchodě App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikování do obchodu App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interní distribuční](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Jednorázová distribuce](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Podpora IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
