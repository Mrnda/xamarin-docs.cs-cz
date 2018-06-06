---
title: SpriteKit v Xamarin.iOS
description: Tento dokument popisuje SpriteKit společnosti Apple 2D grafické rozhraní, které se integruje s SceneKit, zahrnuje fyziky a animace, zahrnuje podporu pro osvětlení a stínování a další. SpriteKit lze použít k vytvoření 2D hry.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: b74b5a722aab240b55ed96bea2a33b162d7817eb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786766"
---
# <a name="spritekit-in-xamarinios"></a>SpriteKit v Xamarin.iOS

SpriteKit rozhraní grafiky 2D od společnosti Apple, obsahuje některé zajímavé nové funkce v iOS 8 a OS X Yosemite. Mezi ně patří integrace s SceneKit, podporu shaderu, osvětlení, stínů, omezení, normální mapa generování a fyziky vylepšení. Konkrétně nové funkce fyziky usnadňují velmi přidat realistické efekty do hru.

## <a name="physics-bodies"></a>Fyziky těla

SpriteKit zahrnuje 2D, pevné textu fyziky rozhraní API. Každý pohyblivý symbol má k přidružené fyziky tělo (`SKPhysicsBody`), který definuje vlastnosti fyziky například velkokapacitních a třením, jakož i geometrie obsahu na světě fyziky.

## <a name="creating-a-physics-body-from-a-texture"></a>Vytváření fyziky text z texturou
SpriteKit teď podporuje odvozování z jeho texture fyziky text pohyblivý symbol. Díky tomu je snadno implementovat kolize, které podívejte přirozenější.

Všimněte si například v následujícím kolizí jak banánů a opic kolidují téměř na povrchu každé bitové kopie:
 
![](spritekit-images/image13.png "Banánů a opic dojít ke konfliktu téměř na povrchu každé bitové kopie")

SpriteKit umožňuje vytváření fyziky text s jedním řádkem kódu. Jednoduše volání `SKPhysicsBody.Create` s texture a velikost: pohyblivý symbol. PhysicsBody = SKPhysicsBody.Create (pohyblivý symbol. Texture, pohyblivý symbol. Velikost);

## <a name="alpha-threshold"></a>Prahovou hodnotu alfa

Kromě nastavení jednoduše `PhysicsBody` vlastnost přímo na geometrie odvozené od textury aplikace můžete nastavit a řídit, jak je odvozený geometrie alfa prahové hodnoty. 

Definuje prahovou hodnotu alfa minimální hodnotu alfa, které mají být zahrnuty v těle výsledné fyziky musí mít jeden bod. Například následující kód výsledkem mírně odlišné fyziky text:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

Účinek postupně je upravujte prahovou hodnotu alfa takto fine-tunes předchozí kolizí tak, aby opic spadá při ke kolizi s banánů:

![](spritekit-images/image14.png "Opic spadá při ke kolizi s banánů")
 
## <a name="physics-fields"></a>Fyziky pole

Jiné skvělé přidání do SpriteKit je nové pole fyziky podporovat. Ty umožňují přidávat položky, například pole vortex paprskového gravitace pole a pole pružiny pro pojmenování několika.

Pole fyziky jsou vytvořené pomocí SKFieldNode třídu, která se přidá do scény stejně jako libovolný jiný `SKNode`. Existuje mnoho různých metod vytváření na `SKFieldNode` vytvořit různé fyziky pole. Pole pružiny lze vytvořit voláním `SKFieldNode.CreateSpringField()`, pole paprskového gravitace voláním `SKFieldNode.CreateRadialGravityField()`a tak dále.

`SKFieldNode` také obsahuje vlastnosti pro řízení pole atributů, jako jsou třeba intenzita pole, pole oblast a oslabení sil pole.

## <a name="spring-field"></a>Spring pole

Například následující kód vytvoří pružiny pole a přidává ji k scény:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Potom můžete přidat Sprite a nastavte jejich `PhysicsBody` vlastnosti tak, aby pole fyziky ovlivní Sprite, stejně když uživatel dotykem obrazovky následující kód:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

To způsobí, že banány k jeho rozkmitání jako pružiny kolem uzlu pole:

![](spritekit-images/image15.png "Banány jeho rozkmitání jako pružiny kolem uzlu pole")
 
## <a name="radial-gravity-field"></a>Paprskového gravitace pole

Přidávání do jiného pole je podobný. Například následující kód vytvoří paprskového gravitace pole:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Výsledkem je jiné pole force, kde jsou banány vyžádány radiálně o pole:

![](spritekit-images/image16.png "Banány vyjmutí radiálně kolem pole")
