======================
Instrument & Sensors
======================

G-LiHT combines six sensors on a single airborne platform, flown together so
that every measurement is collected over the same ground footprint at the same
time.

Instrument Generations
=========================

Instrument models have changed several times over G-LiHT's history.
Specifications below are cross-checked against official per-campaign
metadata PDFs from the `data archive
<https://glihtdata.gsfc.nasa.gov/files/G-LiHT/>`_, sampled across 13
campaign years (2011-2022, 2025). Two things worth knowing about the
archive itself:

* **No campaign folders appear in the archive for 2023 or 2024** — it's
  not clear whether this reflects a gap in flights or a gap in processing
  and publishing data from those years, so don't assume either.
* 2017 was a full system overhaul year: the GPS-INS, LiDAR count, digital
  camera, thermal camera, and VNIR spectrometer all changed at once. Most
  per-sensor "current vs. previous" splits below land on 2017 as the
  dividing line.

LiDAR (Light Detection and Ranging)
=====================================

* Current Model (2017-present): 2x RIEGL VQ-480i — one "old" unit
  (the original VQ-480, upgraded), one "new" unit purchased in 2017. Each
  point in the delivered point cloud is tagged with which unit it came
  from (LAS "User" field: 1 = old, 2 = new).
* Previously Used Models (2011-2016): single RIEGL VQ-480
* A separate **profiling lidar** (RIEGL LD90-3100/LD321-A40, nadir-only)
  was also flown intermittently alongside the scanning lidar in 2011,
  2012, and 2016 — but was absent from other years in that range (e.g.
  2013-2015), and fully discontinued once the dual-scanning-lidar
  configuration became standard in 2017.
* Laser wavelength: 1550 nm
* Pulse repetition frequency: 300 kHz (2011-2016 campaigns also
  supported 150 kHz)
* Scan rate: 150 lines/second (2017-present); 100 lines/second
  (2011-2016)
* Maximum returns per pulse: 8
* Spatial Resolution: nominal 1 m for surface model products, 13 m
  "plot scale" for LiDAR metrics — see :doc:`processing`
* Field of View: ~60 degrees total (±30 degrees off-nadir) — the widest
  field of view of any G-LiHT sensor, producing a swath a little under
  400 m wide at the nominal 335 m (~1100 ft) AGL flight altitude.
* Post-processing software: RIEGL RiProcess

VNIR Imaging Spectrometer
=====================================

* Current Model (2017-present): Headwall Microhyperspec E Series
  (Headwall Photonics), camera PCO Edge 5.5 — this is the "microhyperspec"
  instrument referenced elsewhere in this documentation
* Previously Used Models (2011-2016): Headwall Hyperspec model
  1002A-00451, camera Adimec RA1000m/D_DFG
* Spectral range: 400-1000 nm sensor range (2017-present); 417-1008 nm
  (2011-2016). Delivered data products are resampled to 418-918 nm at
  4.5 nm sampling in both eras.
* Acquisition rate: 75 Hz (2017-present); 50 Hz (2011-2016)
* Quantization: 16-bit (2017-present); 12-bit (2011-2016)
* Spatial Resolution: nominal 1 m (orthorectified)
* Captures vegetation composition and biophysical/biochemical properties

SWIR Imaging Spectrometer
=====================================

* Current Model (2019-present): Headwall Microhyperspec SWIR (Headwall
  Photonics)
* Spectral range: 890-2526 nm
* Acquisition rate: 75 Hz
* Quantization: 16-bit
* Flown alongside the VNIR imaging spectrometer to extend G-LiHT's
  spectral coverage into the shortwave infrared. Not present before 2019
  (no G-LiHT SWIR data exists prior to that year). As of the 2025
  campaigns, delivered SWIR data products were still under development
  even though the sensor itself was flying — check data availability
  per campaign rather than assuming SWIR products exist for every
  2019+ flight.

Irradiance Spectrometer
=========================

Three generations of downwelling irradiance instrument have been used:

* Current Model (2019-present): Spectral Evolution SR-3500 — spectral
  range 350-2500 nm, 16-bit, variable integration time (15/30/30 ms per
  band)
* 2017-2018: a two-spectrometer pair, Ocean Optics FLAME-T
  (344-1037 nm) + QEPro (650-811 nm)
* Previously Used (2011-2016): Ocean Optics USB-4000, 380-1100 nm,
  16-bit
* Field of view: 180 degrees (cosine diffusor)
* Measures incoming solar irradiance to support reflectance calibration of the
  imaging spectrometer

Thermal Camera
================

* Current Model (2017-present): Xenics Gobi-640 (GigE Vision)
* Previously Used Models (2011-2016): Xenics Gobi-384
* Focal plane array: 640 x 480 on 17 µm pixels (2017-present); 384 x 288
  on 25 µm pixels (2011-2016)
* Acquisition rate: 50 Hz (2017-present); 25 Hz (2011-2016)
* Sensitivity: 8-14 µm
* Quantization: 16-bit
* Spatial Resolution: nominal 1 m
* Measures land surface temperature, used to study vegetation function and
  stress

Digital Camera (RGB / RGBI)
=================================

Three generations, tracking the field-of-view/overlap and bit-depth
figures below across all of them once a digital camera was flown:

* 2011-2013: none — G-LiHT flew no digital camera at all in its earliest
  years.
* 2014-2016: Nikon D7100 DSLR, 24 megapixels, 8-bit, JPEG.
* 2017-2020: Phase One iXU1000-R, ~101 megapixels, 16-bit, 1 Hz — a
  single-body camera producing RGB-only imagery.
* Current Model (2021-present): two camera bodies flown together —
  Phase One iXM-RS100F-RS (RGB) and iXM-RS100F-Acr (NIR), each
  ~101 megapixels, 16-bit, 1 Hz. This pairing is what enables the 4-band
  RGBI product; it was first flown as a test in 2021 and became the
  standard collection starting in 2022 — see :doc:`data_products` and
  :doc:`processing`.
* Image format: IIQ (Phase One-era cameras)
* Forward overlap: ~75% along a single flight line
* Sidelap: ~30% when flying a mapped area (multiple lines); none on a
  single flight line

Historical / Retired Instruments
=======================================

* **FIREFLY Fluorescence Spectrometer** (Headwall Photonics, 670-780 nm,
  37.5 Hz, 16-bit) — appears in campaign metadata from 2017 through 2020,
  but is absent from metadata for 2021, 2022, and 2025 campaigns. It is
  not part of the current six-sensor payload and may have been retired;
  this is based on its disappearance from metadata documents rather than
  an explicit retirement notice, so treat as provisional.
* See the LiDAR section above for the intermittently-flown profiling
  lidar (2011-2012, 2016; discontinued by 2017).

Sensor Integration & Boresighting
=======================================

The GPS/IMU system (current, 2017-present: Applanix POS AV V6, 200 Hz;
previously, 2011-2016: Oxford Technical Solutions RT-4041, 250 Hz) provides
the trajectory solution every other sensor is georeferenced against. This
switch happened in the same 2017 system overhaul as the LiDAR, camera, and
thermal camera upgrades (see "Instrument Generations" above).

Each sensor (each LiDAR unit, the IMU, and the GPS antenna) has its own
physical coordinate origin. The offset between these origins is called the
**lever arm**, measured directly on the aircraft and entered into the
processing software. **Boresighting** is the process of aligning these
coordinate systems so that returns from every sensor register to a common
reference frame.

Lever-arm corrections are applied at multiple points in the processing
chain (e.g., in Applanix trajectory processing and in the RIEGL LiDAR
processing), so a missing or incorrect lever-arm correction file (e.g., a
missing ``.ini`` in Headwall processing) can introduce significant
geolocation error if any one of those steps is skipped.

Instrument Computers
--------------------------

G-LiHT's sensors run across two onboard mission computers:

* One computer runs the LiDAR units, cameras, and GPS.
* The other runs the Headwall hyperspectral imaging instruments, locked to
  that computer due to hardware interface constraints.
* The thermal camera can run on either computer.

Common Gridded Products Info
=============================

*** CONFIRMING WITH BRUCE — content below is drafted from the public data
archive's file listings, not yet reviewed ***

File Formats
----------------

Format is not uniform across product types:

* LiDAR-derived rasters (DTM, CHM, DSM, LiDAR metrics, apparent
  reflectance): **GeoTIFF**
* VNIR/SWIR hyperspectral cubes: **ENVI format**, not GeoTIFF
* Point clouds: **ASPRS LAS 1.1**
* Quicklooks: **Google Earth KML** overlays (available for CHM, DTM,
  thermal, and the NIR band of hyperspectral imagery)
* Trajectory: 3D KML plus ASCII at the GPS/IMU sampling rate

Folder Structure and Naming
--------------------------------

Each campaign in the archive follows the same folder skeleton:
``hyperspec/{kml, swath}``, ``lidar/{geotiff/{lidar_refl, metrics}, kml,
las, shp}``, ``metadata/``, ``trajectory/``.

* LiDAR products are packaged as ``<campaign>_<producttype>.tar.gz``
  (e.g. ``AK_10Jul2014_CHM.tar.gz``).
* LiDAR metrics and reflectance are split by return-class subset: ``all_``
  (all returns), ``tree_``, ``ground_``, ``shrub_``, plus a separate
  ``nmbu_tree_`` decile family (a distinct tree-metric algorithm from the
  Norwegian University of Life Sciences, bundled alongside the standard
  "tree" metric set).
* Hyperspectral swaths are packaged as
  ``<campaign>_l<line#>s<swath#>_radiance.tar.gz`` and
  ``..._refl_VIs.tar.gz`` — reflectance and vegetation indices are
  bundled together per swath, while radiance is packaged separately.

Phase One / RGBI Products Not Yet in the Public Archive
--------------------------------------------------------------

At least as of the 2022 campaigns checked, Phase One/RGBI orthomosaic
output does not appear in the public archive's per-campaign folders
(only ``lidar/``, ``metadata/``, and ``trajectory/`` are present) —
consistent with :doc:`processing`'s note that this pipeline is still
under active development.

Related Reference
----------------------

The archive root has a ``Documents/`` folder containing
``Transforming_between_vertical_coordinate_systems_and_datums.docx``,
relevant to the WGS84/EGM96 datum discussion in :doc:`processing`.


