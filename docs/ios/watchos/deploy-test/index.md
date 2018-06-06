---
title: Nasazení a testování aplikací watchOS pomocí Xamarinu
description: Tento dokument popisuje, jak nasadit a otestovat watchOS aplikace vytvořené s funkcí Xamarin. Ho kontrolní seznam nasazení poskytuje, popisuje explicitní a zástupný znak app ID a se podíváme na skupiny aplikací.
ms.prod: xamarin
ms.assetid: 98257399-E9B3-4BAB-9204-0E89117DEA6D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 778583456e74bb7ed3a85dce96bcdbc487aef57a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790939"
---
# <a name="deploying-and-testing-watchos-apps-with-xamarin"></a>Nasazení a testování aplikací watchOS pomocí Xamarinu

## <a name="deployment-checklist"></a>Kontrolní seznam nasazení

Ať už jsou nasazení do testu sledovat nebo chcete nahrát do obchodu s aplikacemi, musíte provést kroky na této stránce:

- V **iOS Dev Center**:
  - [ID aplikace](#App_IDs) byly vytvořeny.
  - [Skupiny aplikací](#App_Groups) nakonfigurované (v případě potřeby).
  - Zřizování profily distribuce, který je vytvořen

- Ve vašem řešení:

  - Ověřte [ID sady prostředků a odkazy na projekt](~/ios/watchos/get-started/installation.md) jsou nastavené.
  - Zkontrolujte vaše ikony jsou [správně nakonfigurovaná](~/ios/watchos/app-fundamentals/icons.md).
  - Kontrola shody čísla verze sady ve všech projektech.
  - Konfigurace **Entitlements.plist** pro skupiny aplikací (v případě potřeby).

* Potom postupujte podle pokynů:
  - [Nasazení do Apple Watch pro testování](~/ios/watchos/deploy-test/device.md), nebo
  - [Nahrajte do obchodu s aplikacemi](~/ios/watchos/deploy-test/appstore.md).

<a name="App_IDs"/>

## <a name="app-ids"></a>ID aplikace

Jak je popsáno v [pokyny](~/ios/watchos/get-started/installation.md), všechny tři projekty v aplikaci sledování mít relaci ID sady prostředků, jako například:

- Sjednocený projektu Xamarin.iOS- `com.xamarin.WatchKitCatalog`
- Rozšíření WatchKit projekt – `com.xamarin.WatchKitCatalog.watchkitextension`
- Projekt aplikace kukátko – `com.xamarin.WatchKitCatalog.watchkitapp`

Všechny tři projekty vyžadují odpovídající distribuční zřizování profilu, buď pomocí explicitně ID aplikace pro každou nebo zástupný znak ID aplikace.

### <a name="explicit-app-ids"></a>ID explicitní aplikace

Vytvoření **ID aplikace** ID sady každý každého projektu, (který bude vypadat například takto na Centrum vývojářů pro iOS):

![ID sady prostředků v centru vývojářů pro iOS](images/appids-specific-sml.png)

Při vytváření nebo konfigurace ID aplikace, nezapomeňte povolit specifické funkce, které vaše aplikace vyžaduje. To může zahrnovat nabízená oznámení a skupiny aplikací.

Budete muset vytvořit profil pro zřizování distribuce pro každý ID aplikace.

### <a name="wildcard-app-id"></a>ID aplikace zástupný znak

Alternativně můžete vytvořit zástupný znak **ID aplikace** odpovídající všech tří projektů, jako například `com.xamarin.*`.

Všimněte si, že některé funkce nelze použít s zástupný znak ID aplikace (např. nabízená oznámení). Pokud vaše aplikace vyžaduje tyto funkce byste měli vytvořit explicitní ID aplikace.

Pro distribuci jenom musíte vytvořit jeden distribuční profil zřizování pro zástupný znak ID aplikace.

<a name="App_Groups" />

## <a name="app-groups"></a>Skupiny aplikací

Skupinu aplikace můžete použít ke sdílení dat mezi vaší aplikace pro iOS a sledovat rozšíření. Měli ujistit, že má vaše řešení:

- Nakonfigurované **aplikace skupiny** v portálu pro vývojáře Apple **certifikáty, identifikátory a profily** části.

- Povolit **skupin aplikací** (a poskytuje **ID skupiny aplikací**) v *i* aplikace pro iOS a sledovat rozšíření **ID aplikace** a  **Entitlements.plist**.

### <a name="certificates-identifiers--profiles"></a>Certifikáty, identifikátory a profily

Chcete-li použít skupinu aplikací, vytvořte položku v **skupin aplikací** obrazovky. V následujícím příkladu je skupině pojmenovány styl stejné zpětného DNS, který se běžně používá pro ID aplikace, ale s `group.` předpona (což je vyžadována):

![Identifikátor](images/appgroups-new-sml.png)

Skupina aplikace se pak zobrazí v seznamu:

![Seznam identifikátorů](images/appgroups-setup-sml.png)

Jakmile je skupina vytvořená, může být odkazováno v vaše **ID aplikace** konfigurace. Nezapomeňte zahrnout i iOS aplikace a sledovat rozšíření **ID aplikace**.

![K dispozici consifurations](images/appgroups-sml.png)

Proveďte **není** povolit skupiny aplikací v ID Apple Watch aplikace. Není potřeba povolit pro sledování, sám sebe.

### <a name="entitlementsplist"></a>Entitlements.plist

Některé funkce aplikace (např. Skupiny aplikací) vyžadují, abyste nastavit vaše oprávnění.
Dvojitým kliknutím na Upravit **Entitlements.plist** souboru v těchto projektů:

- projekt aplikace iOS
- Kukátko rozšíření projektu

.![Entitlements.plist editor](images/entitlements-plist-sml.png)

Proveďte **není** povolte oprávnění v aplikaci sledování projektu. Není potřeba povolit pro sledování, sám sebe.

## <a name="related-links"></a>Související odkazy

- [Průvodce odeslání WatchKit Apple](https://developer.apple.com/app-store/watch/)
