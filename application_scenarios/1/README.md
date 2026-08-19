## NDVI Calculation

### Background

The NDVI is widely used for assessing vegetation coverage and health[^1].
It is derived from red and NIR channels as $\text{NDVI} = (\text{NIR} - \text{R}) / (\text{NIR} + \text{R})$.
In this scenario, the AOI is defined by a bounding box and the aerial imagery is provided by a WMS.

### Implementation

The scenario is realized using the `TilePipeline` as it requires processing a large spatial extent of raster data.
A `Grid` instance is created from the bounding box and a tile size, defining the tiles to be processed.
Since the WMS provides RGB and NIR imagery separately, a `CompositeFetcher` is used to compose two `WMSFetcher` into a single data source, fetching both concurrently and merging their outputs into a single `Tile` instance.
By assigning names only to the relevant channels, the remaining channels returned by the WMS are omitted from the resulting `Tile` instances.
The `TileLoader` then collates the `Tile` instances into a single `Tiles` instance, which is forwarded iteratively to the `SequentialCompositeProcessor`.
To avoid idle time between batches, subsequent tiles can be prefetched during processing.
Next, an `ExpressionProcessor` calculates the NDVI values from the red and NIR channels and assigns them to a new channel, before exporting the results tile by tile as GeoTIFF files using a `RasterExporter`.

The pipeline structure is summarized as follows[^2]:

- `TilePipeline`
  - `CompositeFetcher` → `Tile` (R<sub>R</sub>, NIR<sub>R</sub>)
    - `WMSFetcher` → `Tile` (R<sub>R</sub>)
    - `WMSFetcher` → `Tile` (NIR<sub>R</sub>)
  - `SequentialCompositeProcessor`
    - `ExpressionProcessor` → `Tiles` (R<sub>R</sub>, NIR<sub>R</sub>, NDVI<sub>R</sub>)
    - `RasterExporter`

This scenario demonstrates that the framework is not limited to ML-related tasks, but is equally applicable to general processing.

  [^1]: https://doi.org/10.1016/j.tree.2005.05.011
  [^2]: The arrows indicate the typed output of the components. The subscript denotes—where applicable—the `Channel` implementation: <sub>R</sub> for `RasterChannel`, <sub>V</sub> for `VectorChannel`, and <sub>O</sub> for `ObjectChannel`.
