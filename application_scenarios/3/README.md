## Land Cover Segmentation

### Background

Land cover segmentation classifies each pixel into land cover categories such as buildings, pervious and impervious surfaces, different vegetation types, and water bodies.
In this scenario, a model from the FLAIR-HUB[^1] benchmark, previously converted to the [ONNX](https://onnx.ai) format, is executed with [ONNX Runtime](https://onnxruntime.ai).
The model is hosted on [Hugging Face](https://huggingface.co) and downloaded at runtime.
The AOI is defined by a GeoJSON file specifying the boundary, and the aerial imagery is provided by a WMS.

### Implementation

The scenario is realized using the `CompositePipeline` as the processing involves two distinct stages: tile-based inference followed by postprocessing of the vectorized predictions.
A `Grid` instance is created from the boundary and a tile size, defining the tiles to be processed.
Since the AOI is large, the `Grid` instance can be partitioned to enable scalable processing across compute resources.
The `TilePipeline` fetches aerial imagery from the WMS with a buffer to provide additional spatial context for the model.
Three `StandardizeProcessor` standardize each channel in accordance with the model's requirements, before the `LandCoverModel` performs inference and assigns the predictions to a new channel.
The `LandCoverModel` is a custom `TilesProcessor` that conforms to the corresponding component interface and is registered as a plugin with an associated configuration schema.
Small artifacts in the predictions are removed using a `SieveProcessor`, after which the predictions are clipped back to the original tile extent using a `RemoveBufferProcessor`.
The predictions are then vectorized using a `VectorizeProcessor` and exported tile by tile using a `VectorExporter` by appending to a GeoPackage.
A `GridExporter` exports the coordinates of processed tiles to a JSON file after each batch, enabling the pipeline to be resumed if interrupted.
In the subsequent stage, the `VectorPipeline` loads the vectorized predictions and the AOI using a `CompositeLoader`, composing a `GPKGLoader` and a `GeoJSONLoader`.
A `ClipProcessor` clips the predictions to the spatial extent of the AOI boundary, and a `MapFieldProcessor` maps the predicted values to semantic category names, before exporting the final results as a GeoPackage using a `VectorExporter`.

The pipeline structure is summarized as follows:

- `CompositePipeline`
  - `TilePipeline`
    - `WMSFetcher` → `Tile` (R<sub>R</sub>, G<sub>R</sub>, B<sub>R</sub>)
    - `SequentialCompositeProcessor`
      - `StandardizeProcessor` → `Tiles` (R<sub>R</sub>, G<sub>R</sub>, B<sub>R</sub>)
      - `StandardizeProcessor` → `Tiles` (R<sub>R</sub>, G<sub>R</sub>, B<sub>R</sub>)
      - `StandardizeProcessor` → `Tiles` (R<sub>R</sub>, G<sub>R</sub>, B<sub>R</sub>)
      - `LandCoverModel` → `Tiles` (LC<sub>R</sub>)
      - `SieveProcessor` → `Tiles` (LC<sub>R</sub>)
      - `RemoveBufferProcessor` → `Tiles` (LC<sub>R</sub>)
      - `VectorizeProcessor` → `Tiles` (LC<sub>V</sub>)
      - `VectorExporter` → `Tiles` (—)
      - `GridExporter`
  - `VectorPipeline`
    - `CompositeLoader` → `Vector` (LC, AOI)
      - `GPKGLoader` → `Vector` (LC)
      - `GeoJSONLoader` → `Vector` (AOI)
    - `SequentialCompositeProcessor`
      - `ClipProcessor` → `Vector` (LC, AOI)
      - `MapFieldProcessor` → `Vector` (LC, AOI)
      - `VectorExporter`

This scenario demonstrates the integration of a model as a custom component within a multi-stage pipeline, including postprocessing of the predictions.

  [^1]: https://doi.org/10.1016/j.isprsjprs.2026.04.017
