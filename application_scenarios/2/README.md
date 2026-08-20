## Training Dataset Creation

### Background

Datasets for training ML models for semantic segmentation consist of paired image tiles and corresponding label masks.
In this scenario, the AOI is defined by a bounding box, the aerial imagery is provided locally as a VRT file, and the corresponding labels are provided as a GeoPackage, containing building footprints.

### Implementation

Similarly to the previous scenario[^1], it is realized using the `TilePipeline`.
A `VRTFetcher` and `GPKGFetcher` fetch the RGB imagery and building footprints, which are merged into a single `Tile` instance that aggregates both raster and vector data.
The RGB imagery is first exported as GeoTIFF files, then the building footprints are rasterized using a `RasterizeProcessor` and exported as GeoTIFF files.

The pipeline structure is summarized as follows:

- `TilePipeline`
  - `CompositeFetcher` → `Tile` (R<sub>R</sub>, G<sub>R</sub>, B<sub>R</sub>, BF<sub>V</sub>)
    - `VRTFetcher` → `Tile` (R<sub>R</sub>, G<sub>R</sub>, B<sub>R</sub>)
    - `GPKGFetcher` → `Tile` (BF<sub>V</sub>)
  - `SequentialCompositeProcessor`
    - `RasterExporter` → `Tiles` (BF<sub>V</sub>)
    - `RasterizeProcessor` → `Tiles` (BF<sub>R</sub>)
    - `RasterExporter`

This scenario demonstrates that the framework can complement training-focused ML frameworks and libraries by facilitating the creation of training datasets in advance.

  [^1]: For the sake of brevity, aspects identical to the previous scenario are omitted.
