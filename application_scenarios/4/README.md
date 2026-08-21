## Vehicle Detection

### Background

Vehicle detection localizes individual vehicles as oriented bounding boxes.
In this scenario, a YOLO26[^1] model pretrained on the DOTA[^2] dataset is executed with [PyTorch](https://pytorch.org).
The AOI is defined by a GeoJSON file specifying the boundary, and the aerial imagery is provided by a WMS.

### Implementation

Similarly to the previous scenario, it is realized using the `CompositePipeline`.
The `TilePipeline` fetches aerial imagery from the WMS with a buffer, which is passed directly to the `VehicleModel`.
In contrast to the previous scenario, no standardization is required, as it is handled internally by the model.
The `VehicleModel` is a custom `TilesProcessor` that performs inference and assigns the predictions as `Object` instances to a new channel.
After inference, the predictions are clipped back to the original tile extent and exported tile by tile using an `ObjectExporter` by appending to a GeoPackage.
In the subsequent stage, the `VectorPipeline` additionally filters the predictions based on their confidence score using a `QueryProcessor`.

The pipeline structure is summarized as follows:

- `CompositePipeline`
  - `TilePipeline`
    - `WMSFetcher` → `Tile` (R<sub>R</sub>, G<sub>R</sub>, B<sub>R</sub>)
    - `SequentialCompositeProcessor`
      - `VehicleModel` → `Tiles` (V<sub>O</sub>)
      - `RemoveBufferProcessor` → `Tiles` (V<sub>O</sub>)
      - `ObjectExporter` → `Tiles` (—)
      - `GridExporter`
  - `VectorPipeline`
    - `CompositeLoader` → `Vector` (V, AOI)
      - `GPKGLoader` → `Vector` (V)
      - `GeoJSONLoader` → `Vector` (AOI)
    - `SequentialCompositeProcessor`
      - `ClipProcessor` → `Vector` (V, AOI)
      - `QueryProcessor` → `Vector` (V, AOI)
      - `MapFieldProcessor` → `Vector` (V)
      - `VectorExporter`

This scenario demonstrates that a different task, modality, and ML backend are supported through the same consistent pipeline structure as the previous scenarios.

  [^1]: https://doi.org/10.48550/arXiv.2606.03748
  [^2]: https://doi.org/10.1109/CVPR.2018.00418
