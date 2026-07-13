======================
Instrument & Sensors
======================

G-LiHT combines six sensors on a single airborne platform, flown together so
that every measurement is collected over the same ground footprint at the same
time.

LiDAR (Light Detection and Ranging)
=====================================

* Current Model:
* Previously Used Models:
* Acquisition Rate:
* Spatial Resolution:
* Field of View: ~60 degrees total (±30 degrees off-nadir) — the widest
  field of view of any G-LiHT sensor, producing a swath a little under
  400 m wide at the nominal ~1000 ft flight altitude.

Imaging Spectrometer (Hyperspectral)
=====================================

* Current Model:
* Previously Used Models:
* Spectral range: 
* Acquisition rate: 
* Spatial Resolution:
* Captures vegetation composition and biophysical/biochemical properties

Irradiance Spectrometer
=========================

* Current Model:
* Previously Used Models:
* Acquisition rate: 
* Measures incoming solar irradiance to support reflectance calibration of the
  imaging spectrometer

Thermal Camera
================

* Current Model:
* Previously Used Models:
* Acquisition rate: 
* Spatial Resolution:
* Measures land surface temperature, used to study vegetation function and
  stress

Phase One Camera
====================

* Current Model:
* Previously Used Models:
* Acquisition rate:
* Bit Depth: 16-bit (not 8-bit), to preserve dynamic range
* Forward overlap: ~75% along a single flight line
* Sidelap: ~30% when flying a mapped area (multiple lines); none on a
  single flight line
* Captures RGB imagery, plus a near-infrared band (RGBI) on datasets from
  the 2022 collection onward — see :doc:`data_products` and
  :doc:`processing`

Sensor Integration & Boresighting
=======================================

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

*** CONFIRMING A FEW THINGS ***


