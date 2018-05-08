---
title: Spusťte obrazovky pro aplikace pro Xamarin.iOS
description: Tento článek vysvětluje, jak vytvořit aplikaci spustit obrazovky pro všechna zařízení s iOS, na jakékoli řešení a orientaci pomocí jednoho Unified scénáře.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: d5a267bfa8655a9b9c6d4dba9d8cf9d16624ba9b
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
---
# <a name="launch-screens"></a>Spusťte obrazovky

_Tento článek vysvětluje, jak vytvořit aplikaci spustit obrazovky pro všechna zařízení s iOS, na jakékoli řešení a orientaci pomocí jednoho Unified scénáře._

Před iOS 8 vyžaduje vytvoření obrazovky spuštění pro aplikaci pro iOS developer zajistit prostředek bitovou kopii pro každý různým velikostem zařízení a řešení, ve kterých by mohl spustit aplikaci. Od verze iOS 8 ale bylo možné vytvořit spusťte obrazovky, který vypadá správné ve všech případech pomocí jednoho Unified scénáře.

Tento stručný postup popisuje, jak vytvořit blokování spustit buď Storyboard, dostupné ve výchozím nastavení do nového projektu nebo s Storyboard, ručně přidat do existujícího projektu. Potom ukazuje, jak používat iOS Návrhář přidat zobrazení obrazu a štítek do scénáře, nastavte omezení těchto zobrazení a ověřte, že vypadá v pořádku pro různá zařízení a orientace scénáři.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Správa spuštění obrazovky s scénářů

V iOS 8 (nebo novější) můžete vytvořit vývojář speciální Unified Storyboard zajistit obrazovce spusťte místo použití jeden nebo více statických spouštěcí bitové kopie. Při vytváření spuštění scénáře v Návrháři iOS, použijte k definování různých rozložení pro prostředí s jiným zobrazením velikost třídy a automatického rozložení. Pomocí třídy velikost a automatického rozložení vývojář může vytvořit jeden úvodní obrazovka, který bude vypadat dobře na všech zařízeních a zobrazit prostředí.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V sadě Visual Studio pro Mac, vytvořte nový projekt tak, že vyberete **soubor > Nový řešení** a pak vyberete **jediné zobrazení aplikace**: 

    ![Okno Nový projekt v jedné aplikaci zobrazení vybrané](launch-screens-images/launch01.png)

    - Ve výchozím nastavení, zahrnuje nový projekt **LaunchScreen.storyboard** soubor, který definuje rozhraní spusťte obrazovky. 
    - Na obrazovce scénáře spusťte místo toho přidat do existujícího projektu, klikněte pravým tlačítkem na název projektu v **řešení Pad** a zvolte **Přidat > Nový soubor...**  a pak vyberte **spusťte obrazovky**:

    ![Okno Nový soubor s iOS obrazovky spustit vybrané](launch-screens-images/launch01b.png)

    - Název souboru **LaunchScreen** nebo jiný název vašeho výběru.

2. Konfigurace projektu pro použití odpovídající scénáře pro jeho spuštění obrazovky:

    - Dvakrát klikněte **Info.plist** souboru v **řešení Pad** otevřete pro úpravy.
    - V **spuštění bitové kopie** část, ujistěte se, že **spusťte obrazovky** je nastavena na název příslušné scénáře:

    ![Selektor spusťte obrazovky v Info.plist](launch-screens-images/launch02.png)

    - Ve výchozím nastavení, nový projekt konfigurován pro použití **LaunchScreen.storyboard** jako jeho spuštění obrazovky.

3. Přidejte bitovou kopii **Assets.xcassets** Asset katalogu, aby bylo k dispozici pro použití na obrazovce spustit. Další informace najdete v tématu [přidání bitových kopií do skupiny pro bitovou kopii Asset Catalog](~/ios/app-fundamentals/images-icons/displaying-an-image.md) části [zobrazení bitovou kopii](~/ios/app-fundamentals/images-icons/displaying-an-image.md) průvodce.

4. Otevřete **LaunchScreen.storyboard** pro úpravy poklepáním v **řešení Pad**.

5. Vyberte zařízení a orientaci na které chcete zobrazit náhled v iOS návrháře obrazovky Storyboard spustit. Otevřete panel výběru zařízení na dolním panelu nástrojů a vyberte **iPhone 4S** a **na výšku**.

    ![Panel nástrojů pro výběr zařízení](launch-screens-images/launch05.png)

    - Upozorňujeme, že výběrem zařízení a orientaci pouze změní jak iOS Návrhář přináší náhled návrhu. Bez ohledu na výběr provedený v tomto poli, nově přidaná omezení se použijí ve všech zařízeních a orientace, pokud **upravit vlastnosti** tlačítko jsou využívány k určení jinak. 

6. Nastavte **pozadí** barva řadiče zobrazení hlavního zobrazení. Vyberte zobrazení kliknutím uprostřed řadiče zobrazení a upravit pomocí Barva pozadí **Pad vlastnosti**:

    ![Jediné zobrazení barvou fialové pozadí](launch-screens-images/launch06.png)

7. Přidat **Image zobrazení** k obrazovce pro spuštění a nastavte její zdroj **Image**:

    - Přetáhněte **Image zobrazení** z **Pad sady nástrojů** do středu zobrazení.
    - S **zobrazení bitové kopie** vybrali, v **pomůcky** části **vlastnosti Pad** nastavit **Image** vlastnost do bitové kopie sady již Přidat do **Assets.xcassets** katalogu Asset Catalog. Změna umístění a velikost **Image zobrazení** podle potřeby:
    
    ![Zobrazení obrazu s nastavenou vlastnost bitové kopie](launch-screens-images/launch07.png)

8. Přidat **popisek** níže **Image zobrazení** a použít **vlastnosti Pad** k nastavení jeho atributy: 

    ![Štítek s nastavenou text a barvy](launch-screens-images/launch08.png)

9. Přepnout na režim úprav omezení pomocí pravého tlačítka v **omezení nástrojů**:
    
    ![Tlačítko režimu úprav omezení](launch-screens-images/launch09.png)

10. Přidat omezení na **Image zobrazení**, nastavení jeho výška a šířka a zarovnání ho vodorovně a svisle:

    ![Zobrazení obrazu s omezeními rozložení](launch-screens-images/launch10.png)

    - Další podrobnosti o tom, jak přidat omezení najdete v tématu [automatického rozložení pomocí návrháře Xamarin pro iOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Přidat omezení pro **popisek**, centrování ho vodorovně, předá výšky a šířky a umístění Pevná vzdálenost svisle **zobrazení bitové kopie**:

    ![Štítek s omezeními rozložení](launch-screens-images/launch11.png)

12. Testovat jiná zařízení a orientace k ověření, že vypadá tak, jak má ve všech scénářích návrhu. V případech, kdy je potřeba provést pro určité zařízení nebo orientaci úpravy, použít **upravit vlastnosti** tlačítko přidáte omezujícími podmínkami pro konkrétní velikost třídy:

    ![Na obrazovce spuštění se vykresluje jako je iPhone X pomocí orientaci na šířku](launch-screens-images/launch12.png)

13. Uložte změny do scénáře. Spusťte aplikaci v simulátoru nebo zařízení a na obrazovce spusťte budou viditelné, jako je spuštění aplikace.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Vytvořte nový projekt. V sadě Visual Studio, vyberte **soubor > Nový > Projekt > Visual C# > iPhone & iPad > iOS aplikace (Xamarin)**:

    ![Okna Nový projekt, s iOS aplikace (Xamarin) vybraná](launch-screens-images/launch01.w157.png)

    Vyberte **jediné zobrazení aplikace** šablony a pak klikněte na tlačítko **OK**:

    ![Jediné zobrazení aplikace šabloně](launch-screens-images/launch01-2.w157.png)

2. Pokud **prostředky > LaunchScreen.xib** existuje v **Průzkumníku řešení**, odstraňte jej tak, že kliknete na soubor pravým tlačítkem a vyberete **odstranit**. Tento soubor se nahradí Storyboard v dalším kroku.

3. Vytvořte Storyboard chcete použít jako obrazovce spustit. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte možnost **Přidat > novou položku...**  následuje **prázdný Storyboard**. Název tohoto scénáře **LaunchScreen.storyboard** a klikněte na tlačítko **přidat**:

    ![Okna Přidat novou položku s prázdnou Storyboard vybrané](launch-screens-images/launch03.w157.png)

4. Konfigurace projektu pro použití **LaunchScreen.storyboard** jako jeho spuštění Storyboard obrazovky:

    - Dvakrát klikněte **Info.plist** souboru v **Průzkumníku řešení** otevřete pro úpravy. 
    - Na **Visual prostředky** nastavte **spusťte obrazovky** k **LaunchScreen**.

    ![Selektor spusťte obrazovky v Info.plist](launch-screens-images/launch04-vs.png)

5. Přidejte bitovou kopii do katalog Asset v projektu, aby bylo k dispozici pro použití na obrazovce spusťte:

    - V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Asset katalogů** a vyberte **přidat katalog Asset**. Název tohoto nového katalogu Asset **prostředky**:

    ![Okna Přidat novou položku s katalogem Asset vybrané](launch-screens-images/launch05.w157.png)

    - Přidání nové bitové kopie sad do **prostředky** katalog Asset, jak je popsáno v [přidání bitových kopií do skupiny pro bitovou kopii Asset Catalog](~/ios/app-fundamentals/images-icons/displaying-an-image.md) části [zobrazení bitovou kopii](~/ios/app-fundamentals/images-icons/displaying-an-image.md) průvodce.

6. Otevřete **LaunchScreen.storyboard** pro úpravy poklepáním v **Průzkumníku řešení**.

    - Upravit soubor Storyboard, Visual Studio musí aktivní připojení k hostiteli Mac sestavení. Najdete v článku [připojení k počítači Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Průvodce podrobnosti.

7. Vyberte zařízení a orientaci na které chcete zobrazit náhled v iOS návrháře obrazovky Storyboard spustit. Otevřete panel výběru zařízení na dolním panelu nástrojů a vyberte **iPhone 4S** a **na výšku**: 
 
    ![Panel nástrojů pro výběr zařízení](launch-screens-images/launch07-vs.png)

    - Upozorňujeme, že výběrem zařízení a orientaci pouze změní jak iOS Návrhář přináší náhled návrhu. Bez ohledu na výběr provedený v tomto poli, nově přidaná omezení se použijí ve všech zařízeních a orientace, pokud **upravit vlastnosti** tlačítko jsou využívány k určení jinak. 

8. Přidat **View Controller** do scénáře přetažením z **sada nástrojů** na návrhovou plochu: 

    ![Prázdnou řadiče zobrazení přidat do návrhové plochy](launch-screens-images/launch08-vs.png)

9. Nastavte **pozadí** barva řadiče zobrazení hlavního zobrazení. Vyberte zobrazení kliknutím uprostřed řadiče zobrazení a upravit pomocí Barva pozadí **vlastnosti – okno**:
    
    ![Jediné zobrazení barvou fialové pozadí](launch-screens-images/launch09-vs.png)

10. Přidat **Image zobrazení** k obrazovce pro spuštění a nastavte její zdroj **Image**:

    - Přetáhněte **Image zobrazení** z **sada nástrojů** do středu zobrazení.
    - S **Image zobrazení** stále vybraným v **pomůcky** části **vlastnosti – okno** nastavit **Image** vlastnost nastavující bitové kopie již přidán do **prostředky** katalogu Asset Catalog. Změna umístění a velikost **Image zobrazení** podle potřeby:
    
    ![Zobrazení obrazu s nastavenou vlastnost bitové kopie](launch-screens-images/launch10-vs.png)

11. Přidat **popisek** níže **bitové kopie zobrazení**:

    - Přetáhněte **popisek** z **sada nástrojů** na návrhovou plochu, jeho umístění níže **Image zobrazení**.
    - Nastavit atributy **popisek** pomocí **vlastnosti – okno**:

    ![Štítek s nastavenou text a barvy](launch-screens-images/launch11-vs.png) 

12. Přepnout na režim úprav omezení pomocí pravého tlačítka v **omezení nástrojů**:
    
    ![Tlačítko režimu úprav omezení](launch-screens-images/launch12-vs.png) 

13. Přidat omezení na **Image zobrazení**, nastavení jeho výška a šířka a zarovnání ho vodorovně a svisle:

    ![Zobrazení obrazu s omezeními rozložení](launch-screens-images/launch13-vs.png) 

    - Informace o přidání omezení najdete v tématu [automatického rozložení pomocí návrháře Xamarin pro iOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Přidat omezení pro **popisek**, centrování ho vodorovně, předá výšky a šířky a umístění Pevná vzdálenost svisle **zobrazení bitové kopie**:
    
    ![Štítek s omezeními rozložení](launch-screens-images/launch14-vs.png) 

15. Testovat jiná zařízení a orientace k ověření, že vypadá tak, jak má ve všech scénářích návrhu. V případech, kdy je potřeba provést pro určité zařízení nebo orientaci úpravy, použít **upravit vlastnosti** tlačítko přidáte omezujícími podmínkami pro konkrétní velikost třídy:

    ![Na obrazovce spuštění se vykresluje jako je iPhone X pomocí orientaci na šířku](launch-screens-images/launch15-vs.png) 

16. Uložte změny do scénáře. Spusťte aplikaci v simulátoru nebo zařízení a na obrazovce spusťte budou viditelné, jako je spuštění aplikace.

-----

> [!NOTE]
> Scénáře, použít jako obrazovky spusťte _musí_ zahrnout pouze jednoduché, integrované prvky uživatelského rozhraní a **nelze** všechny výpočty nemusí být odvozeny od vlastní třídy.

Další informace o vytvoření obrazovky spustit s scénáře se Unified, najdete v tématu [dynamické spusťte obrazovky](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) části [Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) průvodce.

## <a name="migrating-to-launch-screen-storyboards"></a>Ke spuštění obrazovky scénářů migrace

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Při aktualizaci stávající aplikace pro použití scénářů pro jeho spuštění obrazovky, klikněte pravým tlačítkem **název projektu** v **Průzkumníku řešení** a vyberte **přidat**  >  **Nový soubor...** . Vyberte **iOS** > **spusťte obrazovky** a klikněte na tlačítko **nový** tlačítko:

![](launch-screens-images/storyboard02.png "Vyberte iOS spusťte obrazovky")

Potom dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy. V části **spusťte obrazovky**, vyberte nový soubor Storyboard vytvořili výše.

![](launch-screens-images/storyboard09.png "Vyberte nový soubor Storyboard vytvořili výše")


Pokud chcete používat nové scénáře jako úvodní obrazovka, postupujte takto:

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. Přejděte k položce **Universal spuštění bitové kopie** části editoru otevřete **spusťte obrazovky** rozevírací seznam a vyberte název scénáři vytvořili výše: 

    ![](launch-screens-images/storyboard08.png "Nastavení úvodní obrazovka do scénáře")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Klikněte pravým tlačítkem na název projektu v **Průzkumníku řešení** a vyberte **přidat** > **nový soubor...** : 

    ![](launch-screens-images/image012.png "Přidat nový soubor")
2. Zadejte název pro spouštěcí obrazovku a klikněte **přidat** tlačítko: 

    ![](launch-screens-images/image013.png "Zadejte název pro úvodní obrazovka")
3. V **Průzkumníku**, poklikejte na soubor nově vytvořený storyboard otevřete pro úpravy.
4. Ujistěte se, že **velikost třída** je nastaven na **žádné: žádné** a **zobrazení jako** je **Obecné**: 

    ![](launch-screens-images/image016.png "Třída velikost je nastavena na žádné: žádné a zobrazení jako je obecná")
5. Sestavení úvodní obrazovka z tříd velikost, jednoduché prvky uživatelského rozhraní (například `UIImageView`) a bitové kopie, které jste zahrnuli do aplikace sady: 

    ![](launch-screens-images/image017.png "Sestavení úvodní obrazovka v iOS návrháře")
6. Uložte změny do scénáře.

-----

## <a name="related-links"></a>Související odkazy

- [Dynamické spusťte obrazovky (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Sjednocené scénáře](~/ios/user-interface/storyboards/unified-storyboards.md)
- [Základy iOS Designeru](~/ios/user-interface/designer/index.md)
- [Přidání bitové kopie na bitovou kopii katalog Asset nastavit](~/ios/app-fundamentals/images-icons/displaying-an-image.md#asset-catalogs)
- [Automatické rozložení pomocí návrháře Xamarin pro iOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Lidského rozhraní pokyny: Úvodní obrazovka](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
