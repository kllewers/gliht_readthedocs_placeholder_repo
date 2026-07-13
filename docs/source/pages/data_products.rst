==============
Data Products
==============

Each G-LiHT flight produces a coincident set of data products derived from its
five sensors. Because the sensors are co-registered and fly together, products
from a given flight line can be fused at the pixel level for combined
structure/composition/temperature analysis.

See :doc:`processing` for how these products are generated, including
calibration methodology and known processing limitations.

Product Categories
====================

Lidar Products
----------------

* Canopy height models and digital terrain/surface models
* Point cloud data describing 3D canopy and terrain structure

Hyperspectral Products
-------------------------

* Calibrated surface reflectance imagery
* Vegetation indices derived from the reflectance data, supporting vegetation
  composition and biophysical/biochemical analysis
* A separate ancillary metadata file (acquisition time, aircraft location,
  sun-sensor geometry) — see :doc:`processing`

Thermal Products
-------------------

* Calibrated land surface temperature imagery

Phase One RGB / RGBI Orthomosaic Products
-----------------------------------------------

* **RGBI orthomosaics** — 4-band (RGB + near-infrared), co-registered,
  photogrammetrically processed imagery from G-LiHT's Phase One cameras.
  This is the standard product starting with the 2022 collection.
* **RGB-only orthomosaics** — for earlier campaigns (e.g. 2018, 2019) that
  lack a near-infrared band.
* These are traditional orthophotos, not true orthophotos — tall features
  (trees, buildings) lean radially away from image center rather than
  appearing top-down. See :doc:`processing` for why.
* **Anaglyph (stereo 3D) imagery**, also derived from the Phase One data.
* Imagery is delivered as 16-bit (not 8-bit) to preserve dynamic range.
* Thermal orthomosaic processing (using the same photogrammetric
  pipeline) is a work in progress — it currently outputs raw digital
  numbers rather than calibrated temperature values.
* No point-cloud or elevation product is produced from this pipeline —
  see :doc:`processing` for why.

Derived / Fused Products
----------------------------

* Combined lidar + hyperspectral + thermal products supporting biomass and
  carbon stock estimation, species composition mapping, and vegetation
  stress/function studies

Spatial Resolution & Format
==============================

LiDAR surface model products (DTM, CHM, mean CHM, DSM) are delivered at 1 m
spatial resolution. LiDAR metrics and topographic metrics that require
neighborhood statistics are computed at 13 m "plot scale" instead — see
:doc:`processing` for why.

No-Data Values
==================

G-LiHT does not use a single fixed no-data/fill value convention across
all products — check each product's metadata rather than assuming. Some
products use ``NaN``, while others use a fill value such as ``-9999``.
Watch for this when doing calculations: an unfiltered ``-9999`` fill value
can silently corrupt statistics, and ``0`` is generally not used as a flag
value either, since some products can legitimately contain small negative
or zero values.

Where Products Are Archived
==============================

G-LiHT data products are archived and distributed at
`glihtdata.gsfc.nasa.gov/files/G-LiHT/ <https://glihtdata.gsfc.nasa.gov/files/G-LiHT/>`_.
See :doc:`data_access` for how to browse and download them.

