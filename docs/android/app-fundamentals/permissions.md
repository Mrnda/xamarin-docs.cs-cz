---
title: Oprávnění v Xamarin.Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 3e12aa47404d8ee4e52ddada3d99f91250e6c54d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242261"
---
# <a name="permissions-in-xamarinandroid"></a>Oprávnění v Xamarin.Android


## <a name="overview"></a>Přehled

Spouštění aplikací pro Android v jejich vlastním sandboxu a zabezpečení důvodů nemají přístup k některým systémovým prostředkům nebo hardwaru na zařízení. Předtím, než může použít tyto prostředky, musí uživatel explicitně udělit oprávnění k aplikaci. Například aplikace nemá přístup k GPS zařízení bez explicitní oprávnění od uživatele. Android vyvolá výjimku `Java.Lang.SecurityException` Pokud se aplikace pokusí o přístup k chráněnému prostředku bez oprávnění.

Oprávnění jsou deklarovány v **AndroidManifest.xml** vývojář aplikace, když se aplikace vyvíjí. Android má dva různé pracovní postupy pro získání souhlas uživatele pro tato oprávnění:
 
* Pro aplikace, které jsou cíleny 5.1 Android (úroveň rozhraní API 22) nebo nižší žádost o oprávnění došlo k chybě při instalaci aplikace. Pokud uživatel neudělí oprávnění, by aplikace nainstalovaná. Jakmile je aplikace nainstalovaná, neexistuje žádný způsob, jak odvolání oprávnění s výjimkou při odinstalování aplikace.
* Od verze Android 6.0 (úroveň rozhraní API 23), se uživatelům zobrazí větší kontrolu nad oprávněními; jsou-li udělit nebo odvolat oprávnění, za předpokladu, aplikace se nainstaluje na zařízení. Tento snímek obrazovky ukazuje nastavení oprávnění pro aplikace kontakty Google. Obsahuje seznam různých oprávnění a umožňuje uživatelům povolit nebo zakázat oprávnění:

![Ukázka obrazovky oprávnění](permissions-images/01-permissions-check.png) 

Aplikace pro Android musí zkontrolovat v době běhu, pokud chcete zobrazit, pokud mají oprávnění pro přístup k chráněnému prostředku. Pokud aplikace nemá oprávnění, pak musí vytvořit požadavky pomocí nových rozhraní API poskytovaných sady Android SDK pro daného uživatele udělit oprávnění. Oprávnění jsou rozdělená do dvou kategorií:

* **Normální oprávnění** &ndash; jedná se o oprávnění, které představují malé bezpečnostní riziko pro zabezpečení nebo ochranu osobních údajů uživatele. Android 6.0 automaticky udělí normální oprávnění v době instalace. V dokumentaci Android pro [úplný seznam normálními oprávněními](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Nebezpečná oprávnění** &ndash; na rozdíl od normální oprávnění nebezpečných oprávnění jsou ty, které chrání zabezpečení nebo ochranu osobních údajů uživatele. Explicitně ty musí být udělena uživatelem. Odesílání a příjem zpráva SMS je příklad akce vyžadující nebezpečných oprávnění.

> [!IMPORTANT]
> Kategorie, které patří oprávnění může v průběhu času měnit.  Je možné, že oprávnění, která byla kategorizována jako "normální" oprávnění může být zvýšena v budoucnu úrovně rozhraní API na nebezpečných oprávnění.

Nebezpečná oprávnění se dále dílčí dělí do [ _skupiny oprávnění_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Oprávnění skupiny bude obsahovat oprávnění, která jsou logicky souvisí. Když uživatel udělí oprávnění pro jednoho člena skupiny oprávnění, Android automaticky udělí oprávnění u všech členů této skupiny. Například [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) oprávnění skupiny obsahuje i `WRITE_EXTERNAL_STORAGE` a `READ_EXTERNAL_STORAGE` oprávnění. Pokud uživatel udělí oprávnění k `READ_EXTERNAL_STORAGE`, pak bude `WRITE_EXTERNAL_STORAGE` automaticky uděleno oprávnění ve stejnou dobu.

Než požádáte o jedno nebo více oprávnění, je vhodné zajistit důvody, proč aplikace vyžaduje oprávnění než požádáte o oprávnění. Jakmile uživatel nerozumí důvody, aplikace může požádat o oprávnění uživatele. Pochopením důvody může uživatel provést informované rozhodnutí, když budou chtít udělit oprávnění a pochopit má, pokud tomu tak není. 

Celý pracovní postup kontroly a vyžadování oprávnění se označuje jako _oprávnění za běhu_ zkontrolujte a můžeme shrnout v následujícím diagramu: 

[![Vývojový diagram kontrola oprávnění za běhu](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Knihovna pro Android podporu zpětné některé z nových rozhraní API pro oprávnění ke starším verzím systému Android. Tyto přeneseny zpět rozhraní API automaticky vyhledala verzi Androidu na zařízení, takže není nutné provádět kontrolu úrovně rozhraní API pokaždé, když.  

Tento dokument popisuje přidání oprávnění do aplikace Xamarin.Android a jak aplikace, které cílí na Android 6.0 (úroveň rozhraní API 23) nebo vyšším byste provést kontrolu oprávnění za běhu.


> [!NOTE]
> Je možné, že oprávnění pro hardwaru může ovlivnit, jak aplikace se filtrují podle Google Play. Například pokud aplikace vyžaduje oprávnění pro fotoaparátu/kamery, pak Google Play nezobrazí aplikaci ve Store Google Play na zařízení, který nemá nainstalovanou kameru.


<a name="requirements" />

## <a name="requirements"></a>Požadavky

Důrazně doporučujeme, že projekty Xamarin.Android zahrnout [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) balíček NuGet. Tento balíček bude backport oprávnění, které konkrétní rozhraní API pro starší verze systému Android, poskytuje jednu společnou rozhraní bez nutnosti neustále zkontrolujte verzi Androidu aplikace je spuštěná na.


## <a name="requesting-system-permissions"></a>Vyžadování oprávnění systému

Chcete-li deklarovat, že soubor manifestu oprávnění v Androidu je prvním krokem při práci s oprávnění Androidu. To je nutné provést bez ohledu na úroveň rozhraní API, že aplikace je zaměřen.

Aplikace, které cílí na Android 6.0 nebo vyšší nelze předpokládat, protože uživatel oprávnění v určitém okamžiku v minulosti, že oprávnění budou platit při příštím. Aplikace, která cílí na Android 6.0 vždycky musí provádět kontrolu oprávnění za běhu programu. Aplikace, které cílí na Android 5.1 nebo nižší, není potřeba provést kontrolu oprávnění za běhu.

> [!NOTE]
> Aplikace by měl pouze požádat o oprávnění, která vyžadují.


### <a name="declaring-permissions-in-the-manifest"></a>Deklarování oprávnění v manifestu

Oprávnění jsou přidány do **AndroidManifest.xml** s `uses-permission` elementu. Například pokud aplikace, je nalezení polohu zařízení, se vyžaduje bez problémů a kurzu oprávnění umístění. Následující dva prvky jsou přidány do manifestu: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Je možné deklarovat oprávnění s využitím podpory nástroje integrované do sady Visual Studio:

1. Dvakrát klikněte na panel **vlastnosti** v **Průzkumníka řešení** a vyberte **Manifest v Androidu** karty v okně Vlastnosti:

    [![Požadovaná oprávnění na kartě Manifest pro Android](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Pokud aplikace již nemá souboru AndroidManifest.xml, klikněte na tlačítko **ne AndroidManifest.xml nalezen. Klikněte a přidejte ho** jak je znázorněno níže:

    [![Žádná zpráva o souboru AndroidManifest.xml](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Vyberte všechna oprávnění, které vaše aplikace potřebuje z **požadovaná oprávnění** seznamu a uložte:

    [![FOTOAPARÁT oprávnění příklad vybrali](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Je možné deklarovat oprávnění s využitím podpory nástroje integrované do sady Visual Studio pro Mac:

1. Dvakrát klikněte na projekt v **oblasti řešení** a vyberte **možnosti > sestavení > aplikace pro Android**:

    [![Požadovaná oprávnění část uvedenou](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Klikněte na tlačítko **přidat Manifest Android** tlačítko, pokud projekt již nemá **AndroidManifest.xml**:

    [![Chybí manifest Android tohoto projektu](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Vyberte všechna oprávnění, které vaše aplikace potřebuje z **požadovaná oprávnění** seznamu a klikněte na tlačítko **OK**:

    [![FOTOAPARÁT oprávnění příklad vybrali](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android automaticky přidá některé oprávnění v okamžiku sestavení k sestavení pro ladění. Díky tomu budou ladění aplikace snazší. Konkrétně se dvě důležité oprávnění jsou `INTERNET` a `READ_EXTERNAL_STORAGE`. Tato oprávnění automaticky sada nebude zobrazovat v **požadovaná oprávnění** seznamu. Verze sestavení, ale, použijte pouze oprávnění, která jsou explicitně nastaveny **požadovaná oprávnění** seznamu. 

Pro aplikace, které cílí 5.1 Android (úroveň rozhraní API 22) nebo nižší není nic jiného, který je potřeba udělat. Aplikace, které poběží na Android 6.0 (úroveň rozhraní API 23 23) nebo novějším by mělo pokračovat k další části o tom, jak provádět běhu kontroluje oprávnění. 


### <a name="runtime-permission-checks-in-android-60"></a>Oprávnění za běhu se změnami s Androidem 6.0

`ContextCompat.CheckSelfPermission` – Metoda (k dispozici s knihovnou Android podporu) se používá ke kontrole, pokud byla udělena zvláštní oprávnění. Tato metoda vrátí [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) výčtu, která má jednu ze dvou hodnot:

* **`Permission.Granted`** &ndash; Zadané oprávnění bylo uděleno.
* **`Permission.Denied`** &ndash; Nebylo uděleno oprávnění k zadané.

Tento fragment kódu je příklad toho, jak zkontrolovat oprávnění fotoaparát v aktivitě: 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

Je osvědčeným postupem je informovat uživatele, proč je oprávnění nutná pro aplikaci tak, že můžete provést informované rozhodnutí udělit oprávnění. Příklad tohoto by aplikace, která přijímá fotografie a geo-tags je. Je uživateli zřejmé, že oprávnění fotoaparát je nezbytné, ale nemusí být jasné, proč se umístění zařízení musí aplikace. Důvody by se zobrazit zpráva umožňující uživateli snáze pochopit, proč oprávnění umístění je žádoucí a že není vyžadováno oprávnění k fotoaparátu.

`ActivityCompat.ShouldShowRequestPermissionRational` Metoda se používá k určení, pokud by měl být důvody zobrazí uživateli. Tato metoda vrátí `true` Pokud by se mělo zobrazit důvody pro dané oprávnění. Tento snímek obrazovky ukazuje příklad Snackbar zobrazí aplikace, která vysvětluje, proč se aplikace potřebuje znát umístění zařízení:

![Důvody pro umístění](permissions-images/07-rationale-snackbar.png) 

Pokud uživatel udělí oprávnění, `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` by měla být volána metoda. Tato metoda vyžaduje následující parametry:

* **aktivita** &ndash; jedná o aktivitu, která žádá o oprávnění a je informován androidem výsledků.
* **oprávnění** &ndash; seznam oprávnění, které jsou požadovány.
* **requestCode** &ndash; celočíselnou hodnotu, která slouží k porovnání výsledků žádost o oprávnění k `RequestPermissions` volání. Tato hodnota by měla být větší než nula.

Tento fragment kódu je příkladem dvě metody, které nebyly popsány. Nejprve se provede kontrola k určení, pokud by se zobrazit důvody oprávnění. Pokud důvody je zobrazený, se zobrazí Snackbar s důvody. Pokud uživatel klikne **OK** v Snackbar, potom aplikace požádá o oprávnění. Pokud uživatel nepřijímá důvody, aplikace by neměl pokračujte požádat o oprávnění. Pokud není uvedené důvody, aktivita bude požadovat oprávnění:

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission` je možné volat i v případě, že uživatel už udělil oprávnění. Následná volání nejsou nezbytné, ale poskytují uživateli možnost potvrzení (nebo ji odvolat) oprávnění. Když `RequestPermission` je volána, ovládací prvek je předáno do operačního systému, který se zobrazí uživatelské rozhraní pro příjem oprávnění:  

![Dialogové okno Permssion](permissions-images/08-location-permission-dialog.png)

Po dokončení uživatele Androidu výsledky vrátí do aktivity prostřednictvím metody zpětného volání, `OnRequestPermissionResult`. Tato metoda je součástí rozhraní `ActivityCompat.IOnRequestPermissionsResultCallback` který musí implementovat aktivitou. Toto rozhraní obsahuje jedinou metodu, `OnRequestPermissionsResult`, který bude vyvolán s Androidem a informovat aktivity volby uživatele. Pokud uživatel udělil oprávnění, můžete aplikace pokračujte a použijte chráněnému prostředku. Příklad toho, jak implementovat `OnRequestPermissionResult` je uveden níže: 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permision_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  


## <a name="summary"></a>Souhrn

Tato příručka probírali přidání a zkontrolujte oprávnění v zařízení s Androidem. Rozdíly ve fungování oprávnění mezi původní aplikace pro Android (API úrovně < 23) a nové aplikace pro Android (úroveň rozhraní API > 22). Ho popsané postupy provádět kontroly oprávnění za běhu v Android 6.0.


## <a name="related-links"></a>Související odkazy

- [Seznam běžných oprávnění](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Ukázková aplikace oprávnění modulu runtime](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Zpracování oprávnění v Xamarin.Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
