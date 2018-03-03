---
title: PhotoKit
description: "Fotografie Kit umožňuje aplikacím pro dotazování knihovna obrázků systému a vytvářet vlastní uživatelské rozhraní k zobrazení a změna jeho obsahu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 6f6858f36c30f45225417c78225926906481eb8d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="photokit"></a>PhotoKit

_Fotografie Kit umožňuje aplikacím pro dotazování knihovna obrázků systému a vytvářet vlastní uživatelské rozhraní k zobrazení a změna jeho obsahu._

Fotografie Kit je nové rozhraní, které umožňuje aplikacím pro dotazování knihovna obrázků systému a vytvořit vlastní uživatelská rozhraní zobrazovat a měnit jeho obsah. Obsahuje několik tříd, které představují bitové kopie a video prostředky, jakož i kolekcí prostředků, jako je například alb a složky.

## <a name="model-objects"></a>Objekty modelu
Fotografie Kit představuje tyto prostředky v volá objekty modelu. Model objektů, které představují fotky a videa, sami jsou typu `PHAsset`. A `PHAsset` obsahuje metadata, jako je například typ média assetu a jeho data vytvoření.
Podobně `PHAssetCollection` a `PHCollectionList` třídy obsahovat metadata o asset kolekce a seznamy kolekce v uvedeném pořadí. Asset kolekce jsou skupiny prostředků, jako je například fotky a videa pro zadaný rok. Podobně kolekce seznamy jsou skupiny asset kolekcí, jako je například fotografiím a videím seskupené podle roku.

## <a name="querying-model-data"></a>Dotazování na Data modelu
Fotografie Kit usnadňuje data modelu dotazu prostřednictvím řady různých metod načítání. Například pokud chcete načíst všechny Image, by volat `PFAsset.Fetch`, předejte `PHAssetMediaType.Image` typ média.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

`PHFetchResult` Instance by obsahovat, pak všechny `PFAsset` instance reprezentující bitové kopie. Pokud chcete sami bitové kopie, použijte `PHImageManager` (nebo ukládání do mezipaměti verzi, `PHCachingImageManager`) vytvořte žádost pro bitovou kopii voláním `RequestImageForAsset`. Například následující kód načte bitovou kopii pro každý prostředek v `PHFetchResult` zobrazíte v buňce zobrazení kolekce:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

Výsledkem je mřížka bitových kopií, jak je uvedeno níže:

![](photokit-images/image4.png "Spuštěné aplikaci zobrazení mřížky bitových kopií")
 
## <a name="saving-changes-to-the-photo-library"></a>Uložení změn do knihovny fotografie

Postupy: zpracování dotazování a čtení dat se. Změny je také možné zapsat zpět do knihovny. Vzhledem k tomu, že moci pracovat s knihovnou fotografií systému více také zajímat, aplikace se můžete zaregistrovat pozorovatel k upozornění na změny pomocí PhotoLibraryObserver. Poté Pokud změny mohou, vaše aplikace můžete aktualizovat odpovídajícím způsobem. Zde je například jednoduchou implementaci načtením výše uvedené kolekce zobrazení:

    class PhotoLibraryObserver : PHPhotoLibraryChangeObserver
    {
        readonly PhotosViewController controller;
        public PhotoLibraryObserver (PhotosViewController controller)
        
        {
            this.controller = controller;
        }
    
        public override void PhotoLibraryDidChange (PHChange changeInstance)
        {
            DispatchQueue.MainQueue.DispatchAsync (() => {
            var changes = changeInstance.GetFetchResultChangeDetails (controller.fetchResults);
            controller.fetchResults = changes.FetchResultAfterChanges;
            controller.CollectionView.ReloadData ();
            });
        }
    }
    
Ve skutečnosti zapsat zpět změny z vaší aplikace, vytvořte žádost o změnu. Všechny třídy modelu má třídu žádosti přidružené změny. Například pokud chcete změnit PHAsset, vytvoříte PHAssetChangeRequest. Postup provedení změny, které jsou zapsány zpět do knihovny fotografie a odesílají do pozorovatelů podobný jako výše jsou:

-   Provedení operace úprav.
-   Data filtrovaná image uložte do PHContentEditingOutput instance.
-   Ujistěte se, žádost o změnu k publikování formuláře změny úpravy výstup.

Tady je příklad, který zapíše změnu zpět na obrázek, který použije filtr noir základní Image:

    void ApplyNoirFilter (object sender, EventArgs e)
    {
            
            Asset.RequestContentEditingInput (new PHContentEditingInputRequestOptions (), (input, options) => {
            
        // perform the editing operation, which applies a noir filter in this case
            var image = CIImage.FromUrl (input.FullSizeImageUrl);
            image = image.CreateWithOrientation((CIImageOrientation)input.FullSizeImageOrientation);
            var noir = new CIPhotoEffectNoir {
                Image = image
            };
            var ciContext = CIContext.FromOptions (null);
            var output = noir.OutputImage;
            var uiImage = UIImage.FromImage (ciContext.CreateCGImage (output, output.Extent));
            imageView.Image = uiImage;
        //
        // save the filtered image data to a PHContentEditingOutput instance
            var editingOutput = new PHContentEditingOutput(input);
            var adjustmentData = new PHAdjustmentData();
            var data = uiImage.AsJPEG();
            NSError error;
            data.Save(editingOutput.RenderedContentUrl, false, out error);
            editingOutput.AdjustmentData = adjustmentData;
        //
        // make a change request to publish the changes form the editing output
            PHPhotoLibrary.GetSharedPhotoLibrary.PerformChanges (() => {
                PHAssetChangeRequest request = PHAssetChangeRequest.ChangeRequest(Asset);
                request.ContentEditingOutput = editingOutput;
          },
          (ok, err) => Console.WriteLine ("photo updated successfully: {0}", ok));
      });
    }
    
Když uživatel vybere tlačítko, se filtr použije:

![](photokit-images/image5.png "Příklad použití filtru")
 
A díky PHPhotoLibraryChangeObserver, změny se projeví v zobrazení kolekce když uživatel přejde zpět:

![](photokit-images/image6.png "Změny se projeví v zobrazení kolekce, když uživatel přejde zpět")
