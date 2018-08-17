############
The Pipeline
############

SExtractor
==========

`Guide to SExtractor <astroa.physics.metu.edu.tr/MANUALS/sextractor/Guide2source_extractor.pdf>`_


SCAMP
=====

Filter Chain
============

All filter steps are optional and can be controlled via the `pipeline setup file`.

Filter by Number of Detections - `FILTER_DETEC`
-----------------------------------------------
All sources with a number of detections equal to the numbers specified in the `DETECTIONS` parameter are removed. By default, `DETECTIONS` is `123`, removing all sources with fewer than 4 detections. Removing sources with only 1 or 2 detections is always recommended, as their motion cannot be judged. It is, however, not enforced by the pipeline.

As a rule of thumb, artifacts such as random CR associations tend to have fewer detections than SSOs, which in turn have fewer detections than stars and galaxies. Increasing the required number of detections is an effective way to clean the sample, though at the cost of possibly losing faint SSOs and SSOs in the edge regions of the images.

Filter by Proper Motion Range - `FILTER_PM`
-------------------------------------------
All sources with proper motions lower than `PM_LOW` and larger than `PM_UP` are rejected. Furthermore, the lower limit of the signal-to-noise ratio (SNR) of the proper motion measurement performed by SCAMP can be set using `PM_SNR`. SCAMP performs a linear fit of the source coordinates over time to determine the proper motion. Large uncertainties signal sources which do not move with constant proper motions, as expected from SSOs.

Effectively, the SNR lower limit introduces a lower limit on the proper motion as well. If the proper motion of an SSO over the exposure time is within order of the seeing conditions, it will exhibit large fluctuations in position and therefore be assigned a large error in the proper motion measurement by SCAMP.

Filter by Bad Pixel
-------------------
If all detections of a single source fall within the same pixel `DELTA_PIXEL` range (both `X_IMAGE` and `Y_IMAGE` parameters), the source is rejected.
Bad CCD pixel can be falsely interpreted as sources by SExtractor and SCAMP. Due to the dithering patterns, they appear to move perfectly linear and with a constant proper motion. SExtractor parameters like `DETECT_MINAREA` can be used to clean these sources, but increasing the minimum pixel area per source can also reject faint SSOs. The filter chain therefore also offers this rudimentary bad pixel rejection.

Filter by Motion
----------------
The motion filter is the most effective and strictest filter. A linear fit is applied to both the RA and the DEC coordinates against observation epochs. If the `R^2` goodness-of-fit parameter of both fits is equal or larger than the user-defined `R_SQU_M` parameter (0 <= R^2 <= 1), the source is accepted. If either fit is not within the limit, the source is rejected. If `R_SQU_M` is between 0.95 and 1, this imposes very strict rules on the motion. Slow moving SSOs (proper motion in the order of seeing) might be missed if `R_SQU_M` is too big, while a lower setting will increase the number of artifacts surviving the pipeline.

The filter is effective in sorting out stars and galaxies from the sample, as they are stationary over the period of time, and the centroid position found by SExtractor will randomly fluctuate within the order of the seeing.

Problems arise when the observations span multiple hours or nights. If the survey images for example cover one area of the sky for the whole night with 50 exposures, it may occur that an SSO is observed in the first and the last 5 exposures. Such a long baseline with no observations in between will almost always yield a perfect linear fit. The same is true for sources randomly associated by stars, e.g. two stars close together or a star and several CRs. Again, the linear motion filter will be fooled by the large baseline of observations.
To tackle this problem, the `IDENTIFY_OUTLIER` option was introduced. If `True`, the motion filter starts by detecting outliers in epoch-space within the detections of one source. This is achieved using the **Median Absolute Deviation** (MAD) of the observation epochs *E*.

.. math::

   \mathrm{MAD} = \mathrm{median}(|E_{i} - \mathrm{median}(E)|)

This calculates the median duration between one observation and the median observation epoch. The median is not affected by outliers, therefore it can be used to identify jumps in the epochs. If the time difference between any two observations is larger than `MAD*OUTLIER_THRESHOLD`

Filter by Trail Size
--------------------

Filter by Trail Consistency
---------------------------

Filter by Star Region
---------------------

Optional Analyses
=================

Output Files
============