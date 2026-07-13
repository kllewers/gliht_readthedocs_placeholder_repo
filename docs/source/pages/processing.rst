==================================
Data Processing Methodology
==================================

This page describes how raw G-LiHT sensor data is processed into the
delivered LiDAR and imaging spectrometer products described in
:doc:`data_products`.

Overall Processing Order
=============================

Processing follows a fixed order, since later steps depend on earlier ones:

1. **Applanix trajectory data** is processed first — every other product
   depends on having a valid trajectory solution.
2. **LiDAR data** is processed second. It is both the most commonly
   requested product on its own, and a required input for orthorectifying
   the other sensors (e.g., hyperspectral and Phase One imagery need a
   DTM/DSM to orthorectify against).
3. All other sensor data is processed after that.

LiDAR Processing
====================

Datums
--------

All G-LiHT LiDAR data are stored in projected units referenced to:

* **Horizontal datum:** WGS84
* **Vertical datum:** EGM96 (a geoid, not the WGS84 ellipsoid)

Because heights are geoid heights rather than ellipsoidal heights,
negative elevation values are valid and expected in some locations — this
is not an error.

Acquisition and Processing Software
----------------------------------------

LiDAR data are acquired in flight using RIEGL's **RiAcquire** software, and
converted from raw data into LAS point cloud files using RIEGL's
**RiProcess** software.

RiProcess bins returns into height/distance "zones." The standard setting
is a fixed **zone one**, which works well when the aircraft is closely
terrain-following (nominal terrain-following altitude is 1100 ft AGL). Over
hilly terrain, where the aircraft cannot terrain-follow as closely, returns
can fall into zone two — if processing is fixed to zone one, those returns
become unclassified "floaters" and are effectively lost, producing visible
data gaps in the DTM over valleys. For very hilly terrain, processing is
instead run with **automatic zone detection**, which recovers most of that
data. Automatic zone detection can introduce some floaters of its own, but
these are filtered out by the downstream IDL raster-generation code, so it
is used selectively rather than as the default.

Point Classification
------------------------

Point classification is intentionally simple: the processing only
classifies **ground returns**. Buildings, power lines, towers, and other
structures are not classified, so buildings can sometimes be
misclassified as ground.

Above-ground-level (AGL) height is calculated for each point and stored
in the LAS **point source ID** field.

Reflectance Calibration
----------------------------

G-LiHT LiDAR returns are delivered as range-corrected **apparent
reflectance** (factory-calibrated), not raw intensity. This calibration:

* Does **not** correct for off-nadir angle
* Does **not** account for the BRDF of the target
* Does **not** account for partial beam interception (e.g., a beam
  partially intercepted by a leaf, or hitting a leaf at an angle)

Only fully-intercepted returns are used, in order to partially mitigate
these limitations.

Known Limitations
----------------------

* Scan angle was readable in LAS versions 1.0/1.1, but is **not**
  readable in LAS 1.4, because RIEGL repurposed that field in the extra
  bytes for their own mirror-characteristic data. This is a known,
  currently-unresolved limitation that requires new LAS read/write code
  to address.
* LAS 1.4 has become fragmented industry-wide, as vendors (including
  RIEGL) increasingly define their own custom extra-byte formats rather
  than adhering to a shared standard.

LiDAR Surface Model Products
=================================

* **Digital Terrain Model (DTM)** — bare-earth elevation, built from a
  TIN interpolated to 1 m spatial resolution. Aspect and slope are also
  calculated. A Google Earth overlay quicklook is generated for the DTM.
* **Canopy Height Model (CHM)** — maximum canopy height (the "outer
  shell" of the canopy, not every internal gap/pit), interpolated to 1 m
  resolution from an average of ~12 shots/m². Includes a Google Earth
  overlay quicklook.
* **Mean Canopy Height Model** — the same concept as the CHM, but using
  the mean height of returns within each 1 m pixel instead of the
  maximum.
* **Digital Surface Model (DSM)** — height above the vertical datum
  (EGM96 geoid); equivalent to DTM + CHM.

Plot-Scale Metrics
-----------------------

Topographic and LiDAR metrics that require neighborhood statistics are
computed at **plot scale**, defined as a 13 m grid cell — chosen to match
the size of a US Forest Service Forest Inventory and Analysis (FIA)
subplot, since much of G-LiHT's use case ties back to forest inventory
work.

LiDAR metrics describe how points are distributed vertically within a
profile. There are roughly 60 standard metrics computed, addressing the
three properties foresters generally care about: canopy height, the
height profile within the canopy, and the density profile within the
canopy. Metrics include quartile means, standard deviation (rugosity),
skewness, kurtosis, fractional tree cover, interquartile range, pulse
density, and number of returns per pulse, among others.

These metrics require a reasonably large, well-sampled area (generally
100+ points) to be statistically meaningful, which is why they are
computed at 13 m plot scale rather than 1 m. Metrics are also restricted
to points within approximately ±15 degrees of nadir (a 30-degree full
field of view), since wider viewing angles bias metrics like fractional
cover (a beam looking further off-nadir is less likely to reach the
ground even with equivalent canopy structure).

Change Products
--------------------

For any site visited more than once, change layers are generated
automatically: for every LiDAR surface model and metric layer, an
absolute-difference layer is computed relative to a chosen reference
acquisition. The reference acquisition is user-specified — a common
approach is to always difference against the *first* acquisition of a
site, so that errors do not compound additively across a time series,
though the processing supports differencing against any prior
acquisition.

Imaging Spectrometer (VNIR) Processing
===========================================

Instrument Generations
---------------------------

G-LiHT has flown two generations of imaging spectrometer:

* **Model A** — the original instrument. G-LiHT-specific IDL processing
  code exists only for this instrument, using a back-trajectory
  geo-registration calculation based on the LiDAR data. This approach
  was developed because, at the time, Headwall did not yet provide their
  own processing software.
* **Microhyperspec** — a newer instrument with a TE-cooled Science-CMOS
  detector (Headwall). No G-LiHT-specific IDL processing code has been
  developed for this instrument; the recommended approach is to use
  Headwall's own geo-registration software, which now outperforms the
  older back-trajectory method.

Calibration and Geo-registration Workflow
----------------------------------------------

The recommended (Headwall-based) workflow uses:

* A directory of RadioCal calibration files, provided by Headwall, for
  spectrometer calibration.
* Applanix POSPac processing for the DSM/trajectory solution.
* PDAL to generate the DSM from the resulting point cloud.

**Known issues:**

* Deliverables are geo-registered using a data cube plus a geographic
  lookup table (GLT) file, similar to the AVIRIS format. The GLT
  co-registration step has a known bug that can produce vertical-line
  artifacts across the swath; generating an ortho photo instead avoids
  this issue.
* The DSM used for geo-registration should cover a larger footprint than
  the spectrometer swath itself — if the DSM is too tightly cropped,
  edge effects from the SBET (smoothed best estimate of trajectory) can
  cause warping near the swath edges.

Radiance, Reflectance, and BRDF
------------------------------------

Two spectral products are generated:

* **Radiance** — at the individual swath level.
* **Reflectance** — calculated as the ratio of upwelling to downwelling
  irradiance. This is **not** a full atmospheric correction; a full
  atmospheric correction is deliberately not applied because it is not
  well-optimized for the low flight altitudes G-LiHT typically operates
  at, and has not performed reliably at those altitudes.

G-LiHT flight lines are generally not flown within the solar principal
plane, so BRDF effects are not controlled for by flight geometry. An
empirically-derived BRDF correction is instead applied post-hoc to help
account for this.

Vegetation Indices and Ancillary Data
-------------------------------------------

* Vegetation indices are derived from the reflectance data and delivered
  as GeoTIFFs (much smaller than the full ~100+ band hyperspectral
  cube).
* An ancillary data file is delivered alongside the spectral products,
  containing acquisition time, aircraft location, and sun-sensor
  geometry (e.g., solar/sensor azimuth and related angles).

Phase One RGB/RGBI Orthomosaic Processing
===============================================

Phase One camera imagery is photogrammetrically processed into orthomosaics
using **Correlator3D**, then orthorectified against the existing
**LiDAR-derived DTM** rather than a photogrammetric elevation model — this
is faster, and avoids the lower-quality elevation products that
photogrammetry from a single flight line would otherwise produce. This
pipeline does not generate any point-cloud or elevation product.

Geo-registration is designed to be fully automated and hands-off, given
the volume of G-LiHT imagery: manual ground control point (GCP) placement
is supported but deliberately avoided as standard practice, in favor of
post-processed camera position/orientation data.

This is harder than typical UAS photogrammetric surveys, because G-LiHT
imagery has limited overlap compared to a typical UAS mapping grid:

* Forward overlap along a single flight line is high (~75%).
* Sidelap only exists when multiple lines are flown for a mapped area, and
  is typically only ~30% — well below the ~70-80% sidelap common in UAS
  surveys. A single flight line has no sidelap at all.

Traditional vs. True Orthophoto
-------------------------------------

G-LiHT's orthomosaics are **traditional orthophotos**, not **true
orthophotos**. Both use a surface model, but the distinction matters for
how features appear:

* A traditional orthophoto corrects using the bare-earth DTM only.
  Objects like trees and buildings still lean radially outward from the
  image's nadir point — only features directly at nadir appear perfectly
  top-down.
* A true orthophoto additionally corrects this lean so objects appear to
  "stand up straight." Producing one requires imagery of the object from
  *all* sides (impossible from a single flight-line transect, since only
  one side of a tree or building is ever imaged) plus a DSM with
  resolution high enough to drape over individual objects.

Correlator3D supports true-ortho processing as a setting, but it is not
used as standard practice for G-LiHT products — even mapped areas with
multi-angle coverage from several flight lines are currently delivered as
traditional orthophotos.

Data Tiling & Mosaicking
=============================

For large mapped areas (as opposed to a single flight line), G-LiHT
delivers data in 1 km x 1 km UTM tiles rather than one continuous mosaic:

* The LiDAR data's extent sets the bounding corners of the tile grid,
  since it has the widest field of view of any G-LiHT sensor (see
  :doc:`instrument`). Tiles extend outward from a corner in exact 1 km
  increments, aligned to whole UTM easting/northing values — there are no
  fractional-kilometer tiles or fractional-meter tile origins.
* The UTM zone used for a given tile set is whatever zone the acquisition
  started in.
* Mapped-area tiles are typically built from many separate flight lines.
  Before tiling, flight lines are first co-aligned to each other (using
  tie points), then cut and distributed into their respective tiles.
* Each tile is processed with a buffer around its edges to avoid edge
  artifacts in outputs like the DTM or CHM; the buffer is trimmed off in
  the final output step.
