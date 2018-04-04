---
title: MonoGame GamePad odkaz
description: GamePad je standardní, napříč platformami třída pro přístup k vstupní zařízení v MonoGame.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 37e6b0a6365b1e93192c0eaad4fd3975c3cbf010
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="monogame-gamepad-reference"></a>MonoGame GamePad odkaz

_GamePad je standardní, napříč platformami třída pro přístup k vstupní zařízení v MonoGame._

`GamePad` můžete použít ke čtení ze vstupního zařízení na více platforem MonoGame vstupní. Tato příručka ukazuje, jak pracovat s třídou GamePad. Vzhledem k tomu, že každé vstupní zařízení se liší v rozložení a počet tlačítka, které poskytuje, tento průvodce obsahuje diagramy, které ukazují různé mapování zařízení.


# <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>GamePad k nahrazení Xbox360GamePad

Poskytuje rozhraní API původní XNA `Xbox360GamePad` třídu pro čtení vstup z herní konzole Xbox 360 nebo počítači. O MonoGame nahradili `GamePad` třídy vzhledem k tomu, že řadiče Xbox 360 nelze použít na většině platforem MonoGame (třeba iOS nebo Xbox jeden). Bez ohledu na změnu názvu, použití `GamePad` třídy je podobná `Xbox360GamePad` třídy.


# <a name="reading-input-from-gamepad"></a>Čtení vstup z GamePad

`GameController` Třída poskytuje standardizovaného způsobu čtení vstupu na jakékoli platformě MonoGame. Poskytuje informace prostřednictvím dvou metod:

 - `GetState` – Vrátí aktuální stav tlačítek, analogovým jsou Flash disky a řídicí kontroleru.
 - `GetCapabilities` – Vrátí informace o možnostech hardwaru, například jestli řadičem má určité tlačítka nebo podporuje vibracím.


## <a name="example-moving-a-character"></a>Příklad: Přesunutí znak

Následující kód ukazuje, jak lze pomocí Flash disk levém jezdec přesunout znak tak, že nastavení jeho `XVelocity` a `YVelocity` vlastnosti. Tento kód předpokládá, že `characterInstance` představuje instanci objektu, který má `XVelocity` a `YVelocity` vlastnosti:


```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```


## <a name="example-detecting-pushes"></a>Příklad: Zjišťování nabízených oznámení

`GamePadState` poskytuje informace o aktuálním stavu řadiče, například zda stisknutí určité tlačítka. Některé akce, jako je například vytváření znak přejít, vyžadovat kontrolu Pokud byla posunuta tlačítko (nebyla dolů poslední snímek, ale je mimo provoz tento snímek) nebo vydané (byl dolů poslední snímek, ale není dolů tento snímek). 

Pro tento typ logiku, místní proměnné, které ukládají předchozí snímek `GamePadState` a aktuální snímek `GamePadState` musí být vytvořen. Následující příklad ukazuje, jak uložit a použít předchozí snímek `GamePadState` implementovat přechod:


```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed = 
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```


## <a name="example-checking-for-buttons"></a>Příklad: Kontrola tlačítka

`GetCapabilities` slouží ke kontrole, pokud má řadič určité hardwaru, jako je konkrétní tlačítko nebo analogovým Flash disk. Následující kód ukazuje, jak zkontrolovat B a Y tlačítek na řadič ve hře, který vyžaduje přítomnost obě tlačítka:


```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```


# <a name="ios"></a>iOS

aplikace pro iOS podporují vstup bezdrátové herní zařízení.

> [!IMPORTANT]
> Balíčky NuGet pro MonoGame 3.5 neobsahují podporu pro bezdrátové herní zařízení. Používání třídy GamePad v systému iOS vyžaduje vytváření MonoGame 3.5 ze zdroje nebo pomocí MonoGame 3.6 NuGet binární soubory. 



## <a name="ios-game-controller"></a>iOS herní řadiče

`GamePad` Třída vrací vlastnosti pro čtení z bezdrátové řadičů. Vlastnosti v `GamePad` pokrytí dobrý pro standardní iOS řadiče hardwaru, jak je znázorněno v následujícím diagramu:

![](input-images/image1.png "Vlastnosti v GamePad pokrytí dobrý pro standardní iOS řadiče hardwaru, jak je znázorněno v tomto diagramu")


# <a name="apple-tv"></a>Apple TV

Hry Apple TV, můžete použít pro vstup Siri vzdálené nebo bezdrátové herní zařízení.


## <a name="siri-remote"></a>Vzdálené Siri

*Vzdálené Siri* je nativní vstupní zařízení Apple TV. I když hodnoty od vzdálených Siri lze číst pomocí události (jak je znázorněno v [Siri vzdálené a řadiče Bluetooth Průvodce](~/ios/tvos/platform/remote-bluetooth.md)), `GamePad` třída může vrátit hodnoty od vzdálených Siri.

Všimněte si, že `GamePad` může jenom číst vstupní z tlačítko Přehrát a touch prostor: 

![](input-images/image2.png "Všimněte si, že GamePad můžete pouze číst vstupní z tlačítko Přehrát akci a touch prostor")

Od stiskem prostor přesun pročtěte `DPad` vlastnost, jsou uvedeny pomocí přesun `ButtonState` – třída. Jinými slovy, hodnoty jsou k dispozici pouze jako `ButtonState.Pressed` nebo `ButtonState.Released`, oproti gesta nebo číselné hodnoty.


## <a name="apple-tv-game-controller"></a>Apple TV herní řadiče

Herní zařízení Apple TV chovají stejně jako na herní zařízení pro aplikace iOS. Další informace najdete v tématu [iOS herní řadič části](#iOS_Game_Controller). 


# <a name="xbox-one"></a>Xbox One

Konzole Xbox jeden podporuje čtení vstup z jednoho Xbox herní zařízení.


## <a name="xbox-one-game-controller"></a>Herní zařízení Xbox jeden

Xbox jedno herní zařízení je nejběžnější vstupní zařízení pro Xbox jeden. `GamePad` Třída poskytuje vstupní hodnoty od hardwaru herní zařízení.

![](input-images/image3.png "Třída GamePad poskytuje vstupní hodnoty od hardwaru herní zařízení")


# <a name="summary"></a>Souhrn

Tato příručka poskytuje přehled na MonoGame `GamePad` třídy, jak implementovat logiku vstup čtení a diagramy běžné `GamePad` implementace.

## <a name="related-links"></a>Související odkazy

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
