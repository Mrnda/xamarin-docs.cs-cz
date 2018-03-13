---
title: "Testování v sledování zařízení"
description: "Nasazení aplikací k testování na vaše Apple Watch"
ms.topic: article
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 211f8c7066e86dac3a8351b913da0185093dcb70
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="testing-on-watch-devices"></a>Testování v sledování zařízení

Po provedení [kroky nasazení](~/ios/watchos/deploy-test/index.md) k vytvoření ID aplikace a skupin aplikací (v případě potřeby), postupujte podle pokynů na této stránce:

- [Nastavení zařízení](#devices) ve vývojářském centru Apple a
- [Vytvořit vývojový profily zřizování](#profiles), pak
- [Nasaďte a otestujte](#testing) na Apple Watch.

<a name="devices" />

## <a name="devices"></a>Zařízení

Testování aplikací pro iOS na skutečné iPhone nebo iPad vždy vyžaduje zařízení k registraci na webu Dev Center. V seznamu zařízení vypadá takto (kliknutím na znaménko plus  **+**  pro přidání nové zařízení):

![](device-images/devices-sml.png "V seznamu zařízení bude mít tento tvar")

Sleduje se neliší – nyní je třeba přidat zařízení Apple Watch před nasazením aplikace k němu. Najít pomocí sledování UDID **Xcode** (**Windows > zařízení** seznamu). Při připojení spárované telefonu sledovat informace se zobrazí také:

[![](device-images/xcode-devices-sml.png "Informace o spárované sledování")](device-images/xcode-devices.png#lightbox)

Pokud víte, sledovat UDID, přidejte do seznamu zařízení na webu Dev Center:

![](device-images/devices-watch-sml.png "Sledování UDID v seznamu zařízení")

Po přidání zařízení sledovat, ujistěte se, že je vybraný v jakékoli nové nebo existující vývoj nebo ad-hoc zřizovacích profilů, které vytvoříte:

![](device-images/devices-provisioning.png "Seznam dostupných zařízení")

Nezapomeňte, pokud upravíte existující profil zřizování, stáhněte a nainstalujte ji znovu!

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>Vývoj profily zřizování

K sestavení pro testování na vašem zařízení, je nutné vytvořit **profil zřizování vývoj** pro daná ID aplikace ve vašem řešení.

Pokud máte zástupný znak ID aplikace, *jenom jeden profil zřizování se bude vyžadovat*; ale pokud máte samostatnou ID aplikace pro každý projekt, pak pro každý ID aplikace budete potřebovat profil pro zřizování:

![](device-images/provisioningprofile-development.png "Profil zřizování vývoj")

Po vytvoření všechny tři profily, se objeví v seznamu. Mějte na paměti, stáhněte a nainstalujte každé z nich:

![](device-images/provisioningprofiles.png "K dispozici vývoj profily zřizování")

Můžete ověřit v profilu pro zřizování **možnosti projektu** výběrem **sestavení > iOS podepisování sady** obrazovky a výběrem **verze** nebo **Ladění iPhone** konfigurace.

**Profil zřizování** seznamu se zobrazí všechny profily odpovídající – byste měli vidět odpovídající profily, které jste vytvořili v tomto rozevíracím seznamu:

![](device-images/options-selectprofile.png "V seznamu profil zřizování")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>Testování na sledování zařízení

Po konfiguraci zařízení, ID aplikace a profily zřizování, jste připraveni k testování.

1. Zajistěte, aby váš iPhone je napájený ze sítě a hodinek již byl spárován s iPhone.

2. Zkontrolujte konfiguraci je nastavená na **verze** nebo **ladění**.

3. Zkontrolujte, zda že je zařízení připojené iPhone je vybraný v seznamu cíl.

4. Klikněte pravým tlačítkem na projekt aplikace pro iOS (ne hodinek nebo rozšíření) a zvolte **nastavit jako spouštěný projekt**.

5. Klikněte na tlačítko **spustit** tlačítko (nebo zvolte **spustit** možnost z **spustit** nabídky).

6. Řešení bude sestavte a aplikaci pro iOS se nasadí do iPhone.
  Pokud není správně nastavené aplikace pro iOS nebo zřizování rozšíření sledovat nasazení pro iPhone se nezdaří.

7. Po úspěšném dokončení nasazení pro iPhone se automaticky pokusí odeslat aplikace sledovat spárované sledovat. Ikona vaše aplikace se zobrazí na obrazovce sledovat s kruhový *instalaci* indikátor průběhu.

8. Pokud sledování aplikace je úspěšně nainstalovaná, zůstane na ikonu na obrazovce kukátko – touch tak, aby testování vaší aplikace!


## <a name="troubleshooting"></a>Poradce při potížích

Pokud dojde k chybě během používání nasazení **zobrazení > dotyková zařízení > zařízení protokolu** zobrazíte další informace o této chybě. Níže jsou uvedeny některé chyby a jejich příčin:

### <a name="error-mt3001-could-not-aot-the-assembly"></a>Chyba MT3001: Může AOT sestavení

K tomu může dojít při sestavování v režimu ladění k nasazení na zařízení Apple Watch.

K *dočasně* tento problém obejít, zakažte **přírůstková sestavení** v rozšíření sledovat **možnosti projektu > sestavení > watchOS sestavení** okno:

[![](device-images/disable-incremental-sml.png "Zaškrtávací políčko přírůstková sestavení")](device-images/disable-incremental.png#lightbox)

Tento problém bude vyřešený v příští verzi, po jejímž uplynutí přírůstkové sestavení se dá znovu povolit využívat výhod sestavení rychlejší.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Podívejte se, že aplikace se nepodaří spustit při ladění na zařízení

Jakmile se zobrazí při pokusu o ladění aplikace sledovat na fyzické zařízení, ikona & načítání Číselník (a nakonec časový limit). To bude vyřešen v budoucí verzi; alternativní řešení je spustit sestavení pro vydání (který neumožní ladění).


### <a name="invalid-application-executable-or-application-verification-failed"></a>Spustitelný soubor neplatný aplikace nebo aplikace se nezdařilo ověření

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "Výstraha neplatné spustitelný soubor aplikace")

Pokud tyto zprávy se zobrazují *na obrazovce sledovat* po aplikaci se pokusil nainstalovat, může být několik problémy:

- Kukátko samotné zařízení není přidaný jako zařízení ve vývojářském centru Apple. Postupujte podle pokynů a [nakonfigurovat zařízení správně](#devices).

- Vývoj zřizovacích profilů používá pro testování neměl sledování zařízení zahrnuté; nebo po hodinek byla přidána do zřizovacích profilů nebyly znovu stáhnout a nainstalovat znovu. Postupujte podle pokynů a [správně nakonfigurovat profily zřizování](#profiles).

- Pokud **iOS zařízení protokolu** obsahuje `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` pak sledování aplikace **Info.plist** má chybnou **MinimumOSVersion** hodnotu.
  Měl by být **8.2** – Pokud jste nainstalovali Xcode 6.3, možná budete muset ručně upravit zdroj pro vložení, nastavte ji na 8.2.

- Sledování aplikace **Entitlements.plist** nesprávně nárok povolil (například skupin aplikací), by neměl mít.

- Sledování aplikace **ID aplikace** nesprávně má nárok povolené (například skupin aplikací) na webu Dev Center, který by neměl mít.



### <a name="install-never-finished"></a>Nikdy dokončení instalace

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

Tato chyba může znamenat nepotřebné (a neplatný) klíče v aplikaci sledování **Info.plist** souboru. Nesmí obsahovat klíče určená pro rozšíření iOS aplikace nebo sledovat v aplikaci sledování.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>"čekat na připojení ladicího"

Pokud **výstupu aplikace** okno vázne zobrazující

```csharp
waiting for debugger to connect
```

Zkontrolujte, pokud žádné z NuGets, který byl zahrnut ve vašem projektu jsou závislé na **Microsoft.Bcl.Build**. Tím se automaticky přidá s některé knihovny publikovaná Microsoft včetně oblíbených [knihovny klienta Http Microsoft](http://www.nuget.org/packages/Microsoft.Net.Http/).

**Microsoft.Bcl.Build.targets** soubor, který je přidán do **.csproj** může narušovat balení iOS rozšíření během nasazení. Můžete sledovat [chyb](https://bugzilla.xamarin.com/show_bug.cgi?id=29912).
Možným řešením je upravit souboru .csproj a ručně přesunout **Microsoft.Bcl.Build.targets** být posledním prvkem.

