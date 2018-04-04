---
title: Oprávnění v Xamarin.Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: b8a8005c69c8aaee5d92bdabb3429bd52fc76b4a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="permissions-in-xamarinandroid"></a>Oprávnění v Xamarin.Android


## <a name="overview"></a>Přehled

Spouštění aplikací pro Android v vlastní izolovaného prostoru a zabezpečení důvodů nemají přístup k některým systémové prostředky nebo hardwaru na zařízení. Uživatel musí explicitně udělit oprávnění k aplikaci, než ho může použít tyto prostředky. Například aplikace nebudou mít přístup GPS na zařízení bez explicitního oprávnění od uživatele. Android vyvolá výjimku `Java.Lang.SecurityException` Pokud se aplikace pokusí o přístup k chráněnému prostředku bez oprávnění.

Oprávnění jsou deklarované v **AndroidManifest.xml** podle vývojář aplikace, když je aplikace vyvinuté. Android má dva různé pracovní postupy pro získání souhlasu uživatele pro tato oprávnění:
 
* Pro aplikace, které cílí Android 5.1 (API úrovně 22) nebo nižší oprávnění žádosti došlo k chybě při instalaci aplikace. Pokud uživatel neudělí oprávnění, nebude aplikace nainstaluje. Jakmile je aplikace nainstalovaná, neexistuje žádný způsob, jak odvolat oprávnění kromě odinstalací aplikace.
* Počínaje systémem Android 6.0 (úroveň rozhraní API 23), uživatelé byly zadány větší kontrolu nad oprávněními; mohou udělit nebo odvolat oprávnění, dokud aplikace se nainstaluje na zařízení. Tento snímek obrazovky ukazuje nastavení oprávnění pro aplikaci Google kontakty. Zobrazí seznam různých oprávnění a umožňuje uživatelům povolit nebo zakázat oprávnění:

![Ukázka oprávnění obrazovky](permissions-images/01-permissions-check.png) 

Aplikace pro Android musíte zkontrolovat, při spuštění zobrazit, pokud mají oprávnění k přístupu k chráněnému prostředku. Pokud aplikace nemá oprávnění, je třeba provést požadavky pomocí nových rozhraní API poskytované Android SDK pro uživatele k udělení oprávnění. Oprávnění jsou rozdělené do dvou kategorií:

* **Normální oprávnění** &ndash; jedná se o oprávnění, které představují malé bezpečnostní riziko pro zabezpečení nebo ochranu osobních údajů uživatele. Android 6.0 automaticky udělí normálními oprávněními v době instalace. Informujte se v systému Android dokumentaci [úplný seznam normálními oprávněními](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Nebezpečná oprávnění** &ndash; normálními oprávněními, na rozdíl od nebezpečná oprávnění jsou ty, které chrání zabezpečení nebo ochranu osobních údajů uživatele. Uživatel musí mít explicitně uděleno. Odesílání nebo přijímání zpráva SMS je příklad akce nutnosti nebezpečná oprávnění.

> [!IMPORTANT]
> Kategorii, která patří oprávnění může časem změnit.  Je možné, že oprávnění, která byla kategorizována jako "normální" oprávnění může být zvýšena v budoucnosti úrovně rozhraní API na nebezpečná oprávnění.

Nebezpečná oprávnění další dílčí dělí do [ _oprávnění skupiny_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Oprávnění skupiny bude obsahovat oprávnění, které logicky souvisejí. Když uživatel uděluje oprávnění na jeden člen skupiny oprávnění, Android automaticky udělí oprávnění u všech členů této skupiny. Například [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) skupině oprávnění obsahuje i `WRITE_EXTERNAL_STORAGE` a `READ_EXTERNAL_STORAGE` oprávnění. Pokud uživatel uděluje oprávnění k `READ_EXTERNAL_STORAGE`, pak se `WRITE_EXTERNAL_STORAGE` oprávnění je automaticky přiděleno ve stejnou dobu.

Před vyžaduje jeden nebo více oprávnění, je osvědčeným postupem zadejte odůvodnění, proč aplikace vyžaduje oprávnění před vyžádáním oprávnění. Jakmile uživatel nerozumí odůvodnění, aplikace může požádat o oprávnění od uživatele. Porozuměním odůvodnění, může uživatel provést informované rozhodnutí, pokud chtějí udělit oprávnění a pokud ne, pochopit má. 

Celý pracovní postup kontroly a vyžadování oprávnění se označuje jako _běhu oprávnění_ zkontrolujte a může být souhrnu v následujícím diagramu: 

[![Vývojový diagram kontrola běhu oprávnění](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Knihovna pro Android podporu backports některé z nových rozhraní API Správce oprávnění pro starší verze systému Android. Tyto přeneseny zpět rozhraní API automaticky zkontroluje verzi Androidu na zařízení, takže není nutné provádět kontrolu úrovně rozhraní API pokaždé, když.  

Tento dokument popisuje postup přidání oprávnění k aplikaci Xamarin.Android a jak aplikace, které cílí na Android 6.0 (úroveň rozhraní API 23) nebo vyšší musí provést kontrolu oprávnění běhu.


> [!NOTE]
> Je možné, že oprávnění pro hardware může mít vliv na filtrování aplikace pomocí služby Google Play. Například pokud aplikace vyžaduje oprávnění pro fotoaparát, pak Google Play nebudou zobrazovat aplikace Google Play Storu na zařízení, která nemá nainstalovanou kameru.


<a name="requirements" />

## <a name="requirements"></a>Požadavky

Důrazně doporučujeme, zahrnují Xamarin.Android projekty [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) balíček NuGet. Toto oprávnění backport bude balíček, který konkrétní rozhraní API pro starší verze systému Android, poskytuje jeden běžné rozhraní bez nutnosti neustále zkontrolujte verzi systému Android, která aplikace běží na.


## <a name="requesting-system-permissions"></a>Požaduje oprávnění systému

Prvním krokem při práci s Androidem oprávnění je deklarovat, že oprávnění v Android manifest souboru. To je potřeba bez ohledu na úrovni rozhraní API, které se budou zaměřovat je aplikace.

Aplikace, které cílí na Android 6.0 nebo vyšší nelze předpokládat, že vzhledem k tomu, že uživatel oprávnění v určitém okamžiku v minulosti, že oprávnění budou platné při příštím. Aplikace, která je cílena Android 6.0 musí vždy provést kontrolu oprávnění modulu runtime. Není potřeba provést kontrolu oprávnění spuštění aplikace, které cílí na Android 5.1 nebo nižší.

> [!NOTE]
> Aplikace by měla pouze požádat o oprávnění, která vyžadují.


### <a name="declaring-permissions-in-the-manifest"></a>Deklarování oprávnění v manifestu

Oprávnění jsou přidány do **AndroidManifest.xml** s `uses-permission` elementu. Například pokud aplikace odkazuje na vyhledejte pozici zařízení, se vyžaduje bez problémů a kurzu umístění oprávnění. Následující dva elementy se přidají k manifestu: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Je možné deklarovat oprávnění pomocí podpory nástroje integrovaná v sadě Visual Studio:

1. Klikněte dvakrát na **vlastnosti** v **Průzkumníku řešení** a vyberte **Android Manifest** kartě v okně vlastností:

    [![Požadovaná oprávnění na kartě Android Manifest](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Pokud aplikace již nemá AndroidManifest.xml, klikněte na tlačítko **ne AndroidManifest.xml nalezen. Klikněte na tlačítko Přidat jeden** jak je uvedeno níže:

    [![Žádná zpráva AndroidManifest.xml](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Vyberte všechna oprávnění, aplikace musí z **požadovaná oprávnění** seznamu a uložte:

    [![Příklad FOTOAPARÁT oprávnění vybrané](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Je možné deklarovat oprávnění pomocí podpory nástroje součástí sady Visual Studio pro Mac:

1. Dvakrát klikněte na projekt **řešení Pad** a vyberte **možnosti > sestavení > aplikace pro Android**:

    [![Povinná sekce oprávnění vidět](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Klikněte **přidat Android Manifest** tlačítko Pokud projekt již nemá **AndroidManifest.xml**:

    [![Chybí projektu manifestu systému Android.](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Vyberte všechna oprávnění, aplikace musí z **požadovaná oprávnění** seznamu a klikněte na tlačítko **OK**:

    [![Příklad FOTOAPARÁT oprávnění vybrané](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android automaticky přidají některá oprávnění v čase vytvoření buildu do sestavení pro ladění. Bude ladění aplikace jednodušší. Zejména dvě významné oprávnění jsou `INTERNET` a `READ_EXTERNAL_STORAGE`. Tato oprávnění automaticky set nezobrazí povolení v **požadovaná oprávnění** seznamu. Verze sestavení, ale, použijte jenom oprávnění, která jsou explicitně nastavené **požadovaná oprávnění** seznamu. 

Pro aplikace, které cílí na Android 5.1 (API úrovně 22) nebo nižší není nic jiného, který je potřeba udělat. Aplikace, které poběží na Android 6.0 (API 23 úroveň 23) nebo vyšší musí pokračovat k další části o tom, jak provést běhu ověří oprávnění. 


### <a name="runtime-permission-checks-in-android-60"></a>Ověří oprávnění Runtime v Android 6.0

`ContextCompat.CheckSelfPermission` – Metoda (k dispozici s knihovnou Android podporu) se používá k ověření, pokud byla udělena zvláštní oprávnění. Tato metoda vrátí [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) výčtu, která obsahuje jednu ze dvou hodnot:

* **`Permission.Granted`** &ndash; Byla udělena oprávnění k zadané.
* **`Permission.Denied`** &ndash; Zadané oprávnění nebylo uděleno.

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

Je osvědčeným postupem informovat o tom, proč je oprávnění nezbytná pro aplikaci tak, aby můžete provést informované rozhodnutí o udělení oprávnění uživatele. Příkladem tohoto může být aplikaci, která přebírá fotky a geografická značky je. Je zřejmé, uživateli, je nutné oprávnění fotoaparát, ale nemusí být jasné, proč aplikace také musí umístění zařízení. Logický základ hlediska by měl zobrazit zprávu pomoct pochopit, proč oprávnění umístění je žádoucí a že není vyžadováno oprávnění fotoaparát uživateli.

`ActivityCompat.ShouldShowRequestPermissionRational` Metoda se používá k určení, pokud by měl být vysvětlení zobrazí uživateli. Tato metoda vrátí `true` Pokud má být zobrazena důvody dané oprávnění. Tento snímek obrazovky ukazuje příklad Snackbar, zobrazí aplikace, která vysvětluje, proč aplikace musí znát umístění zařízení:

![Důvody tohoto umístění](permissions-images/07-rationale-snackbar.png) 

Pokud uživatel udělí oprávnění, `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` metoda by měla být volána. Tato metoda vyžaduje následující parametry:

* **aktivita** &ndash; jedná o aktivitu, která požaduje oprávnění a je informováni Android výsledky.
* **oprávnění** &ndash; seznam oprávnění, které jsou požadovány.
* **requestCode** &ndash; celočíselná hodnota, která se používá k porovnání výsledků žádosti o oprávnění na `RequestPermissions` volání. Tato hodnota by měla být větší než nula.

Tento fragment kódu je příkladem dvě metody, které byly popsané. Nejprve se provede kontrola k určení, pokud by se měly zobrazit odůvodnění oprávnění. Pokud odůvodnění se má zobrazit, se zobrazí Snackbar s odůvodnění. Pokud uživatel klikne na **OK** v Snackbar, potom aplikace bude požadovat oprávnění. Pokud uživatel nepřijímá odůvodnění, aplikace by neměl pokračovat s žádostí o oprávnění. Pokud není zobrazen vysvětlení a bude požadovat aktivity oprávnění:

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

`RequestPermission` je možné volat i v případě, že uživatel už má uděleno oprávnění. Následující volání nejsou potřebné, ale poskytují možnost potvrďte (nebo ji odvolat) oprávnění uživatele. Když `RequestPermission` je volána, řízení je předávána operačního systému, který se zobrazí uživatelské rozhraní pro přijetí oprávnění:  

![Dialogové okno Permssion](permissions-images/08-location-permission-dialog.png)

Po dokončení uživatele Android se vrátí výsledky aktivity prostřednictvím metody zpětného volání, `OnRequestPermissionResult`. Tato metoda je součástí rozhraní `ActivityCompat.IOnRequestPermissionsResultCallback` které musí být implementována pomocí aktivity. Toto rozhraní obsahuje jedinou metodu, `OnRequestPermissionsResult`, který bude vyvolán Android k informování aktivitu volby uživatele. Pokud uživatel má uděleno oprávnění, můžete aplikaci pokračujte a použít k chráněnému prostředku. Příklad, jak implementovat `OnRequestPermissionResult` jsou uvedeny níže: 

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

Tato příručka popsané, jak přidat a zkontrolujte oprávnění v zařízení se systémem Android. Rozdíly v fungování oprávnění mezi původním aplikace pro Android (rozhraní API na úrovni < 23) a nové aplikace pro Android (rozhraní API na úrovni > 22). Je popsané, jak provádět kontroly oprávnění běhu v Android 6.0.


## <a name="related-links"></a>Související odkazy

- [Seznam normálními oprávněními](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Modul runtime oprávnění ukázkové aplikace](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Zpracování oprávnění v Xamarin.Android](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest)
