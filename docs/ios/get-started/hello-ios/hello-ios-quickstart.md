---
title: Hello, iOS
description: Tato příručka dvě části popisuje, jak vytvořit základní aplikace pro Xamarin.iOS pomocí sady Visual Studio pro Mac nebo Visual Studio a pochopili jejich základní informace o vývoj aplikací pro iOS pomocí Xamarin. Zavede nástroje, koncepty a kroky potřebné k sestavení a nasazení aplikace pro Xamarin.iOS.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: e4828f1ae5448a94fa1f52147d1aef3787e95521
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="helloios-quickstart"></a>Hello.iOS Quickstart

Tato příručka popisuje, jak vytvořit aplikaci, která znamená, že alfanumerické telefonní číslo, zadané uživatelem do číselné telefonní číslo a pak zavolá toto číslo. Konečné aplikace vypadá takto:

 [![](hello-ios-quickstart-images/image1.png "Aplikaci Hello.iOS Quickstart")](hello-ios-quickstart-images/image1.png#lightbox)


<a name="Requirements" />

## <a name="requirements"></a>Požadavky

vývoj pro iOS pomocí Xamarinu vyžaduje:

-  Systému Mac spuštěné macOS Sierra (10.12) nebo novější.
-  Nejnovější verzi Xcode a iOS SDK nainstalovat z [obchod](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) .

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.iOS funguje s následující nastavení:

-  Nejnovější verze sady Visual Studio pro Mac, která odpovídá výše uvedené podmínky.

[Průvodce instalací Mac Xamarin.iOS](~/ios/get-started/installation/mac.md) je k dispozici pokyny krok za krokem instalace

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS funguje s následující nastavení:

-  Nejnovější verzi Visual Studio 2015 nebo 2017 Professional nebo novější na systému Windows 7 nebo vyšší, spárována s Mac sestavení hostitele, který nejlépe odpovídá výše uvedené podmínky.

[Průvodce instalací Windows Xamarin.iOS](~/ios/get-started/installation/windows/index.md) je k dispozici pokyny krok za krokem instalace.

-----

Než začnete, stáhněte si [nastavit ikony aplikace Xamarin](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio pro Mac návod

Tento návod popisuje, jak vytvořit aplikaci s názvem Phoneword, který překládá alfanumerické telefonní číslo do číselné telefonní číslo.

1. Spusťte sadu Visual Studio pro Mac z **aplikace** složku nebo **Spotlight** se zprovoznit úvodní obrazovka:

  ![](hello-ios-quickstart-images/image2new.png "Úvodní obrazovka")

Na obrazovce spustit, klikněte na tlačítko **nový projekt...**  k vytvoření nové řešení Xamarin.iOS:

![](hello-ios-quickstart-images/image3new.png "řešení pro iOS")


2. Z **dialogové okno nové řešení**, vyberte **iOS > aplikace > jediné zobrazení aplikace** šablony, zajistíte, že C# je vybraný. Klikněte na tlačítko **Další**:

  ![](hello-ios-quickstart-images/image4new.png "Vyberte jediné zobrazení aplikace")

3. Konfigurace aplikace. Pojmenujte ho **název** `Phoneword_iOS`a nechat všem ostatním jako výchozí. Klikněte na tlačítko **Další**:

  ![](hello-ios-quickstart-images/image5new.png "Zadejte název aplikace")

4. Ponechte projektu a název řešení, jako je. Vyberte umístění projektu tady, nebo zachovat jako výchozí hodnota:

  ![](hello-ios-quickstart-images/image6new.png "Vyberte umístění projektu")

5. Klikněte na tlačítko **vytvořit** aby **řešení**.

6. Otevřete **Main.storyboard** soubor dvojím kliknutím na ho **řešení Pad**. To poskytuje způsob, jak vizuálně k vytvoření uživatelského rozhraní:

  ![](hello-ios-quickstart-images/image7new.png "IOS návrháře")

  Všimněte si, že _velikost třídy_ jsou ve výchozím nastavení povolené. Odkazovat [Unified scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) průvodce Další informace o nich.

8. V **sada nástrojů Pad**, zadejte "Popis" do panelu Hledat a přetáhněte ji **popisek** na návrhovou plochu (oblast v centru):

  ![](hello-ios-quickstart-images/image8new.png "Přetáhněte na návrhovou plochu oblasti v Centru pro štítek")

  > [!NOTE]
  > Lze provést **vlastnosti Pad** nebo **sada nástrojů** kdykoli přechodem na **zobrazení > dotyková zařízení**.

9. Získat popisovačů systému *přetáhněte ovládací prvky* (kroužky okolo ovládacího prvku) a ujistěte se, širší popisek:

  ![](hello-ios-quickstart-images/image9.png "Ujistěte se, širší popisek")


10. S **popisek** vybrané na návrhovou plochu, použijte **vlastnosti Pad** změnit **Text** vlastnost **popisek** k "Enter Phoneword: "

  ![](hello-ios-quickstart-images/image10.png "Popisek k zadání Phoneword sady")

11. Vyhledejte "textové pole" uvnitř sady nástrojů a přetáhněte **textové pole** z **sada nástrojů** na návrh surface a umístěte ji pod **popisek**. Umožňuje upravit šířku až **textové pole** stejnou délku jako **popisek**:

  ![](hello-ios-quickstart-images/image12new.png "Ujistěte se, pole textu šířku stejné jako popisek")


12. S **textové pole** vybrané na návrhovou plochu, změnit **textové pole**na **název** vlastnost v části Identita **vlastnosti Pad** k `PhoneNumberText`a změňte **Text** vlastnost "1-855-XAMARIN":

  ![](hello-ios-quickstart-images/image13new.png "Změňte vlastnost název 1-855-XAMARIN")


13. Přetáhněte **tlačítko** z **sada nástrojů** na návrh surface a umístěte ji pod **textové pole**. Umožňuje upravit šířku proto **tlačítko** je širší jako **textové pole** a **popisek**:

  ![](hello-ios-quickstart-images/image14new.png "Umožňuje upravit šířku tak, aby tlačítko širší jako popisek a textové pole.")


14. S **tlačítko** vybrané na návrhovou plochu, změnit **název** vlastnost v **Identity** části **Pad vlastnosti** na `TranslateButton`. Změna **název** vlastnost "Přeložit":

  ![](hello-ios-quickstart-images/image15new.png "Změňte vlastnost název přeložit")


15. Opakujte předchozí dva kroky a přetáhněte ji **tlačítko** z **sada nástrojů** na návrh surface a umístěte jej do první **tlačítko**. Umožňuje upravit šířku proto **tlačítko** je jako první širší **tlačítko**:

  ![](hello-ios-quickstart-images/image16new.png "Umožňuje upravit šířku tak, aby tlačítko širší jako první tlačítka")


16. S druhou **tlačítko** vybrané na návrhovou plochu, změnit **název** vlastnost v **Identity** části **vlastnosti Pad**k `CallButton`. Změna **název** vlastnost "Hovor":

  ![](hello-ios-quickstart-images/image17new.png "Změňte vlastnost název na volání")

  Uložit změny přechodem na **soubor > Uložit** nebo stisknutím kombinace kláves **⌘ + s**.


17. Některé logiku musí být přidán do aplikace přeložit telefonní čísla z alfanumerické na číselnou. Přidejte nový soubor do projektu kliknutím pravým tlačítkem na **Phoneword_iOS** v projektu **Pad řešení** a výběr **Přidat > Nový soubor...**  nebo stiskněte **⌘ + n**:

  ![](hello-ios-quickstart-images/image18.png "Přidejte do projektu nový soubor")


18. V **nový soubor** dialogovém okně, vyberte **Obecné > prázdná třída** a pojmenujte nový soubor `PhoneTranslator`:

  ![](hello-ios-quickstart-images/image19.png "Vyberte třídu prázdný a název nového souboru PhoneTranslator")


19. Tím se vytvoří nový, prázdný C# třídu pro nás. Odeberte všechny kód šablony a nahraďte ji následujícím kódem:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    Uložit **PhoneTranslator.cs** souboru a zavřete ho.

20. Přidejte kód, který propojit se uživatelské rozhraní. Udělat tento klikněte dvakrát na **ViewController.cs** v **řešení Pad** a otevře se:

  ![](hello-ios-quickstart-images/image20new.png "Přidejte kód, který propojit se uživatelské rozhraní")


21. Začněte tím, že kabeláž až `TranslateButton`. V **ViewController** třídy, vyhledejte `ViewDidLoad` metoda a přidejte následující kód pod `base.ViewDidLoad()` volání:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    Zahrnout `using Phoneword_iOS;` Pokud obor názvů v souboru se liší.

22. Přidejte kód reagovat na uživatel kliknutím na tlačítko druhé, která se nazývá `CallButton`. Vložte následující kód pod kód pro `TranslateButton` a přidejte `using Foundation;` do horní části souboru:

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```


23. Uložte změny a následně vytvořit aplikaci tak, že zvolíte **sestavení > sestavení všechny** nebo stiskněte **⌘ + B**.  Pokud aplikace zkompiluje, zobrazí se zpráva o úspěšném provedení v horní části integrovaného vývojového prostředí:

  ![](hello-ios-quickstart-images/image21.png "Zpráva o úspěšném provedení se zobrazí v horní části rozhraní IDE")

  Pokud nejsou chyby, všechny předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.

27. Nakonec testování aplikace **simulátoru iOS**. V horní části zůstane rozhraní IDE, zvolte **ladění** z první rozevíracího seznamu, a **iPhone 8 Plus verze iOS** z druhé rozevírací seznam a stiskněte klávesu **spustit** (trojúhelníkovou tlačítko, podobá tlačítko Přehrát akci):

  ![](hello-ios-quickstart-images/image27new.png "Stiskněte klávesu Start")

  > [!NOTE]
  > V současné době z důvodu požadavku od společnosti Apple, bude pravděpodobně potřeba mít vývojový certifikát nebo *podepisování identity* sestavení kódu pro zařízení ani simulátor. Postupujte podle kroků v [zřizování zařízení Průvodce](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) chcete nastavit tuto možnost.

28. Tím spustíte aplikaci v simulátoru iOS:

  ![](hello-ios-quickstart-images/image28.png "Aplikace běžící v simulátoru iOS")

  Telefonní hovory nejsou podporovány v simulátoru; iOS Místo toho zobrazí se dialogového okna výstrah při pokusu o volání:

  ![](hello-ios-quickstart-images/image29.png "Dialogového okna výstrah při pokusu o volání")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-walkthrough"></a>Návod pro Visual Studio

Tento návod popisuje, jak vytvořit aplikaci s názvem Phoneword, který překládá alfanumerické telefonní číslo do číselné telefonní číslo.

**Poznámka:**: Tento návod používá Visual Studio Enterprise 2017 na 10 virtuálního počítače s Windows. Vaše nastavení se liší od toho, dokud splňuje požadavky na výše uvedené, ale mějte na paměti, že některé snímky obrazovky se mohou lišit na vaše nastavení.

> [!NOTE]
> Než budete pokračovat v tomto průvodci, musíte mít již připojení k počítači Mac ze sady Visual Studio. To je proto Xamarin.iOS spoléhá na společnosti Apple nástrojů pro sestavení a spusťte iOS Designer a aplikace. Pokud chcete získat nastavit, postupujte podle kroků v [připojení k Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) průvodce.

1. Spusťte sadu Visual Studio z **spustit** nabídky:

  ![](hello-ios-quickstart-images/image001-.png "Na úvodní obrazovce")

  Do vyhledávacího pole v části **nové řešení** zadejte _jediné zobrazení aplikace_a vyberte **jediné zobrazení aplikace (iPhone)** k vytvoření nové řešení Xamarin.iOS:

  ![](hello-ios-quickstart-images/image002-.png "Přidání jediné zobrazení aplikace")


2. Název projektu a řešení `Phoneword`, jak je uvedeno dále:

  ![](hello-ios-quickstart-images/vs-image3.png "Název projektu PhonewordiOS a nového řešení Phoneword")


3. Stiskněte klávesu **OK** k vytvoření nového projektu

4. Potvrďte, že je zelená ikona Xamarin Mac Agent na panelu nástrojů.

    ![Potvrďte, že je zelená ikona Xamarin Mac Agent na panelu nástrojů](hello-ios-quickstart-images/vs-image4.png)

    Pokud tomu tak není, to znamená, že se žádné připojení k hostiteli sestavení Mac, postupujte podle kroků v [Průvodci konfigurací](~/ios/get-started/installation/windows/connecting-to-mac/index.md) můžete spojit.


5. Otevřete **Main.storyboard** souboru v iOS Návrhář dvojitým kliknutím na něm v **Průzkumníku řešení**:

  ![](hello-ios-quickstart-images/vs-image7.png "IOS návrháře")

6. Otevřete **sada nástrojů** kartě, zadejte "Popis" do panelu Hledat a přetáhněte ji **popisek** na návrhovou plochu (oblast v centru):

  ![](hello-ios-quickstart-images/vs-image8.png "Přetáhněte na návrhovou plochu oblasti v Centru pro štítek")


7. V dalším kroku získat popisovačů systému *přetáhněte ovládací prvky* a jmenovku širší:

  ![](hello-ios-quickstart-images/vs-image9.png "Ujistěte se, širší popisek")


8. S **popisek** vybrané na návrhovou plochu, použijte **Windows vlastnosti** změnit **Text** vlastnost **popisek** k "Enter Phoneword: "

  ![](hello-ios-quickstart-images/vs-image10.png "Změňte vlastnost Text popisku, zadat Phoneword.")

  > [!NOTE]
  > Lze provést **vlastnosti** nebo **sada nástrojů** kdykoli přechodem na **zobrazení** nabídky.


9. Vyhledejte "textové pole" uvnitř sady nástrojů a přetáhněte **textové pole** z **sada nástrojů** na návrh surface a umístěte ji pod **popisek**. Umožňuje upravit šířku až **textové pole** stejnou délku jako **popisek**:

  ![](hello-ios-quickstart-images/vs-image12.png "Umožňuje upravit šířku, dokud textové pole je šířku stejné jako popisek")


10. S **textové pole** vybrané na návrhovou plochu, změnit **textové pole**na **název** vlastnost v části Identita **vlastnosti**k `PhoneNumberText`a změňte **Text** vlastnost "1-855-XAMARIN":

  ![](hello-ios-quickstart-images/vs-image13.png "Změnit vlastnosti textu na 1-855-XAMARIN")


11. Přetáhněte **tlačítko** z **sada nástrojů** na návrh surface a umístěte ji pod **textové pole**. Umožňuje upravit šířku proto **tlačítko** je širší jako **textové pole** a **popisek**:

  ![](hello-ios-quickstart-images/vs-image14.png "Umožňuje upravit šířku tak, aby tlačítko širší jako popisek a textové pole.")


12. S **tlačítko** vybrané na návrhovou plochu, změnit **název** vlastnost v **Identity** části **vlastnosti** k `TranslateButton`. Změna **název** vlastnost "Přeložit":

  ![](hello-ios-quickstart-images/vs-image15.png "Změňte vlastnost název přeložit")


13. Opakujte předchozí dva kroky a přetáhněte ji **tlačítko** z **sada nástrojů** na návrh surface a umístěte jej do první **tlačítko**. Umožňuje upravit šířku proto **tlačítko** je jako první širší **tlačítko**:

  ![](hello-ios-quickstart-images/vs-image16.png "Umožňuje upravit šířku tak, aby tlačítko širší jako první tlačítka")


14. S druhou **tlačítko** vybrané na návrhovou plochu, změnit **název** vlastnost v **Identity** části **vlastnosti** na `CallButton`. Změna **název** vlastnost "Hovor":

  ![](hello-ios-quickstart-images/vs-image17.png "Změňte vlastnost název na volání")

  Uložit změny přechodem na **soubor > Uložit vše** nebo stisknutím kombinace kláves **Ctrl + s**.

15. Přidáte kód, který převede telefonní čísla z alfanumerické na číselnou. To pokud chcete udělat, nejprve přidejte nový soubor do projektu kliknutím pravým tlačítkem na **Phoneword** v projektu **Průzkumníku řešení** a výběr **Přidat > novou položku...**  nebo stiskněte **Ctrl + Shift + A**:

  ![](hello-ios-quickstart-images/vs-image18.png "Přidat kód, který převede telefonní čísla z alfanumerické na číselné")


16. V **nový soubor** dialogovém okně, vyberte **Apple > třída** a pojmenujte nový soubor `PhoneTranslator`:

  ![](hello-ios-quickstart-images/vs-image19.png "Přidejte novou třídu s názvem PhoneTranslator")

  > [!IMPORTANT]
  > Ujistěte se, že vyberete šablonu 'class', která obsahuje C# v ikonu. Jinak nebudete moci odkazovat na tato nová třída.


17. Tím se vytvoří novou třídu C#. Odeberte všechny kód šablony a nahraďte ji následujícím kódem:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

  Uložit **PhoneTranslator.cs** souboru a zavřete ho.

18. Dvakrát klikněte na **ViewController.cs** v **Průzkumníku** , otevřete tak, aby logiku mohou být přidány do popisovače interakce s tlačítka:

  ![](hello-ios-quickstart-images/vs-image20.png "Přidat do interakce s tlačítka zpracování logiky")


19. Začněte tím, že kabeláž až `TranslateButton`. V **ViewController** třídy, vyhledejte `ViewDidLoad` metoda. Přidejte následující kód tlačítko uvnitř `ViewDidLoad`, ybrat `base.ViewDidLoad()` volání:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```
    Zahrnout `using Phoneword;` Pokud obor názvů v souboru se liší.

20. Přidejte kód reagovat na uživatel kliknutím na tlačítko druhé, která se nazývá `CallButton`. Vložte následující kód pod kód pro `TranslateButton` a přidejte `using Foundation;` do horní části souboru:

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```


21. Uložte změny a následně vytvořit aplikaci tak, že zvolíte **sestavení > Sestavit řešení** nebo stiskněte **Ctrl + Shift + B**.  Pokud aplikace zkompiluje, zobrazí se zpráva o úspěšném provedení v dolní části integrovaného vývojového prostředí:

  ![](hello-ios-quickstart-images/vs-image21.png "Zpráva o úspěšném provedení se zobrazí v dolní části rozhraní IDE")

  Pokud nejsou chyby, všechny předchozí kroky a opravte chyby, dokud úspěšně sestavení aplikace.

22. Nakonec testování aplikace **používat vzdáleně simulátoru iOS**. Na panelu nástrojů IDE, zvolte **ladění** a **iPhone 8 Plus verze iOS** z rozevírací nabídky a stiskněte klávesu **spustit** (zeleným trojúhelníkem, která se podobá tlačítko Přehrát akci):

  ![](hello-ios-quickstart-images/vs-image27.png "Stiskněte klávesu Start")

23. Tím spustíte aplikaci v simulátoru iOS:

  ![](hello-ios-quickstart-images/vs-image28.png "Aplikace běžící v simulátoru iOS")

  Telefonní hovory nejsou podporovány v simulátoru; iOS Místo toho dialogového okna výstrah se zobrazí při volání:

  ![](hello-ios-quickstart-images/vs-image29.png "Dialogového okna výstrah se zobrazí při volání")

-----

Blahopřejeme k dokončení svoji první aplikaci Xamarin.iOS!

Nyní je čas dissect nástroje a dovednosti uvedené v tomto průvodci v [Hello, iOS podrobné informace](~/ios/get-started/hello-ios/hello-ios-deepdive.md).


## <a name="related-links"></a>Související odkazy

- [Ikony aplikace Xamarin a spuštění bitové kopie (ukázka)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Hello, iOS (ukázka)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS Provisioning Portal](https://developer.apple.com/ios/manage/overview/index.action)
