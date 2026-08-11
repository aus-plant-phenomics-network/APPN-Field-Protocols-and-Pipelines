# APPN – GOBI M350 Fieldbook

For a high-level description of the GOBI sensor package and how it sits
alongside the other APPN platforms, see the
[Platforms Overview](../../Background/PlatformsOverview/Platforms_Overview.md).

For the GOBI fieldbook covering the Inspired Flight IF1200A, see
[GOBI IF1200 Fieldbook](../GOBI/GOBI_IF1200_FieldBook.md).

This fieldbook provides a standardised operational guide for APPN GOBI UAV
deployments **on the DJI M350**, supporting safe flight operations,
consistent sensor configuration, and high-integrity data capture. It is
intended for trained APPN staff conducting hyperspectral, LiDAR, RGB, and
GNSS-INS data acquisitions, and promotes repeatability, transparency, and
confidence in downstream data analysis across APPN operations.

> [!IMPORTANT]
> **This protocol must be followed for all standard APPN GOBI M350 UAV
> flights.** Adherence to these procedures is essential to ensure operational
> safety, data integrity, and comparability of datasets across deployments.
> For any flights that **fall outside standard operating procedures**,
> detailed records must be kept documenting all deviations, including the
> specific settings changed, the rationale for those changes, and any
> anticipated implications for data quality or analysis.

---

## Document Structure

1. [Equipment Checklist](#equipment-checklist) — what to bring to
   the field.
2. [Preflight Planning](#preflight-planning) — survey / capture
   polygons and DJI Pilot 2 flight planning.
3. [Onsite Preflight Operations](#onsite-preflight-operations) —
   weather and airspace checks, panel and GCP layout, aircraft setup.
4. [GOBI Sensor Configuration](#gobi-sensor-configuration) — sensor
   power-up, exposure, dark reference, and mission start.
5. [Flight Operations](#flight-operations) — takeoff, dynamic
   alignment, autonomous mission, and landing.
6. [Post-Flight Sensor Configuration](#post-flight-sensor-configuration) —
   stopping the mission and confirming data on the sensor.
7. [Data Confirmation](#data-confirmation) — expected file counts and
   sizes by sensor.
8. [Post-Flight](#post-flight) — pack-down of aircraft, payload, and
   ground reference kit.
9. [Offload Data](#offload-data) — downloading from the sensor and
   storing under the APPN folder structure:
   - [Data storage, processing & validation](#data-storage-processing--validation)
10. [Appendix](#appendix) — FTP & service reference, Headwall polygon
    tool workflow, static IP setup, and IP address reference.

---

## Equipment Checklist

> [!NOTE]
> Ensure batteries for all equipment are fully charged before heading to the
> field. Ensure charging cables are available for necessary equipment.

- [ ] **Aircraft**
  - [ ] DJI M350
  - [ ] Aircraft batteries and spares
  - [ ] Landing gear
  - [ ] Landing pad
  - [ ] Spare parts
  - [ ] Tools
  - [ ] Logbook
- [ ] **Radio Control Transmitter / Ground Control Station**
- [ ] **GRYFN Gobi**
  - [ ] Gobi system (power once each month
        [to charge the RGB camera internal battery](https://gryfn.gitbook.io/gryfn-hardware/support/gobi-user-manual/maintenance/sony-ilx-lr1))
  - [ ] Power cables, chargers
  - [ ] Ethernet cable (and dongle if needed)
  - [ ] SD card
  - [ ] *Capture* and *survey* polygon files
- [ ] **Ground reference kit**
  - [ ] 2 x Reflectance calibration panels (11%, 30%, 56%, 82%)
  - [ ] 1 x Calibration validation panels (20%, 45%)
  - [ ] 5 × Propeller Aeropoints or other ground control points (GCPs) and/or RTK GNSS system 
  - [ ] 2 × folding tables to elevate panels
  - [ ] *If over 30 km from a single CORS base station*, a portable RTK base station
        ([link to GRYFN gitbook](https://gryfn.gitbook.io/gryfn-operations/operations/base-station-availability))
- [ ] **Accessories**
  - [ ] Safety gear (signage and high-vis vests)
  - [ ] Aeronautical radio
  - [ ] Field laptop and spare batteries
  - [ ] External storage media
  - [ ] Water, food, esky, sunscreen, bug spray, first aid kit, etc.
  - [ ] Spirit bubble, spirit level (or angle measurement) and measuring tape
  - [ ] Portable fan (for cooling GOBI during data offload)
  - [ ] External power brick (for charging UAV RC)

> [!NOTE]
> The **Ground reference kit** above assumes the
> [Standard Dual ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#dual-elm-panel-flight)
> design. Other flight designs may require additional equipment (e.g.
> extra reflectance panel sets, additional GCPs and folding tables for
> multi-flight captures, or extra targets for validation flights). See
> the [Standard Flight Procedure](../../FlightDesign/StandardFlight/Standard_Flight.md)
> and the
> [Validation Flight Procedure](../../FlightDesign/ValidationFlight/Validation_Flight.md)
> for the full equipment requirements of each design.

---

## Preflight Planning

> [!IMPORTANT]
> Ensure that you apply for UAV flight approvals for locations and dates of
> flights well in advance. Always use the **latest version** of the
> [GRYFN Flight Calculator](https://gryfn.gitbook.io/gryfn-operations/operations/flight-planning/flight-planning-calculator)
> when planning flights. Further planning documentation for the GOBI can
> be found in the
> [GRYFN operations gitbook](https://gryfn.gitbook.io/gryfn-operations/operations).

1. Using a GPS survey system (Emlid, Trimble…) or GIS software, create a
   polygon with a 5 m margin around the area of interest. Make sure the
   polygon includes the areas where you will place your calibration panels
   and GCPs, with an additional 5 m buffer to avoid incomplete data.
2. Create two polygons — a *survey* polygon and a *capture* polygon. Ensure
   the *survey* polygon is 2× the flight speed larger than the *capture*
   polygon in the direction of travel (as per the figure below). Save both
   polygons uniquely as KML files using the convention
   `YYYYMMDD_XXXX_capture` and `YYYYMMDD_XXXX_survey`
   (`YYYY` = year; `MM` = month; `DD` = day, must be 2 digits;
   `XXXX` = a short reference or abbreviation for the job).

   ![Survey vs capture polygon layout](GOBI_M350_FieldBook_media/image_318468665ad6.png)

   > [!NOTE]
   > Buffer the *capture* polygon **perpendicular to the flight direction**
   > by at least one flight-line spacing on each side. When a flight line
   > runs immediately adjacent to the capture-polygon boundary, the aircraft
   > can drift out of the capture region and hyperspectral triggering stops,
   > losing data along that edge. A perpendicular buffer provides margin for
   > trajectory variation and ensures full capture on the outermost lines.
   > **Increase the buffer further when flight lines cross the polygon
   > boundary at an angle** (e.g. north–south APEx flights): the aircraft
   > enters and exits the polygon at an angle, so a triggered frame can fall
   > half inside and half outside the capture region.

3. *If using QGIS*, import the *capture* KML into Google Earth and
   immediately export it again. This is due to the required formatting
   Google Earth supplies, versus QGIS.
4. Import the *capture* KML into the
   [HPI Polygon Tool](http://50.170.92.179/) and export. This polygon sets
   the activation bounds of the hyperspectral sensor.
5. Load the *survey* KML onto a USB stick or microSD card.
6. Power on the controller. Open DJI Pilot 2 and select
   **Flight Route → + Import Route (KMZ/KML)**. Alternatively, you can
   create an Area Route on the controller. *Note that you do not need the
   UAV powered on for this, just the controller.*
7. Using both DJI Pilot 2 and the GRYFN flight calculator, determine the
   speed, altitude, and frame period required to survey the area of interest.
   See the [Standard Mission Parameters](#standard-mission-parameters) table
   below for examples of common flight types.
8. Set the frame period to **> 20% oversampling (30% is our default)**.
   Frame period sets the along-track sampling and is separate from side
   overlap. Then ensure **side overlap is driven by the VNIR
   (hyperspectral) requirement, not the RGB camera** (see the important
   note under [Standard Mission Parameters](#standard-mission-parameters)):
   for hyperspectral missions (Types 1–3) plan at **80% side overlap from
   the RGB custom camera**, which the GRYFN flight calculator resolves to
   approximately **37% VNIR side overlap**. RGB-only missions (Type 4)
   have no VNIR requirement and may use 75%.

### Standard Mission Parameters

| Standard Mission Type | Scenario                                                              | Altitude (m) | Speed (m/s) | Frame Period (Hz)        | Side Overlap % (DJI)\* |
| :-------------------- | :-------------------------------------------------------------------- | :----------: | :---------: | :----------------------- | :--------------------: |
| Type 1                | Plant counting / small structures (intra-plot differences)           |      30      |     2.1     | 5.32 (20% oversampling)  |          80%           |
| Type 2                | Plant breeding experiments (inter-plot differences)                  |      50      |     3.2     | 5.09 (30% oversampling)  |          80%           |
| Type 3                | Large landscape measurements (strip trials, hyperspectral transects) |      80      |     5.1     | 5.11 (30% oversampling)  |          80%           |
| Type 4                | Surveys using LiDAR and RGB only (ecosystem measurements, forestry)  |     100      |      9      | N/A                      |          75%           |

> \* Select *custom camera* in DJI Pilot 2 flight planning and enter the
> **RGB camera** parameters per the
> [GRYFN custom camera settings guide](https://gryfn.gitbook.io/gryfn-operations/uas-quick-start/ground-control-software/custom-camera-settings).
> The percentages above are the **RGB** side overlap entered into the
> flight planner.

> [!IMPORTANT]
> **Overlap must be driven by the VNIR (hyperspectral) data-quality
> requirement — RGB overlap alone is not a sufficient measure of
> acceptable sidelap.** Historically the standard 75% RGB setting was
> assumed to guarantee adequate VNIR sidelap; in practice it produces
> *very low* VNIR overlap and has caused missing pixels, linear data
> holes, and stronger edge/directional effects in recent flights.
>
> For hyperspectral missions (Types 1–3) plan at **80% RGB side
> overlap**. The GRYFN flight calculator resolves this to approximately
> **37% VNIR side overlap** at these altitudes. VNIR side-overlap targets:
>
> - **~37%** — delivered by the 80% RGB standard; treat as the practical
>   minimum.
> - **≥ 40%** — preferred, for robustness under wind and trajectory
>   variation.
> - **≥ 50%** — every ground point is imaged by **at least two flight
>   lines**, greatly increasing redundancy and resilience to trajectory
>   error. Increase RGB overlap further where this level of redundancy is
>   required.
>
> RGB frame cameras remain the limiting factor for the orthomosaic —
> GRYFN recommends an absolute minimum of 70% overlap/sidelap, with 80%
> giving better feature matching — so planning at 80% RGB keeps RGB
> products well within spec while lifting VNIR sidelap to an acceptable
> level. **RGB-only missions (Type 4)** have no VNIR requirement and may
> use 75%.

> [!NOTE]
> Frame-period values may differ slightly between the **GRYFN Flight
> Calculator** and the **Gryfn WebUI**. The WebUI uses each sensor's
> *calibrated* focal length, whereas the calculator uses the
> manufacturer's nominal value (VNIR ~12.6 mm, RGB ~24 mm). Calibrated
> focal lengths are typically slightly shorter, marginally reducing the
> computed frame rate and overlap, but the difference is negligible
> (~0.01 ms in frame period, ~0.02% in sidelap) and is absorbed by the
> oversampling and sidelap margins. Either source is acceptable; the
> values in the table above are from the flight calculator.

> [!NOTE]
> **Type 1 (30 m) uses 20% oversampling, not the 30% used for Types 2–3.**
> At 30 m the finer ground sampling requires a higher frame rate, so 30%
> oversampling pushes the VNIR frame rate close to the sensor's maximum,
> which GRYFN does not recommend. 20% oversampling keeps a safe margin
> below the maximum frame rate while still meeting the minimum oversampling
> requirement.

---

## Onsite Preflight Operations

1. Conduct airspace and weather checks.
   - No cloud cover.
   - Maximum wind speed for quality hyperspectral capture is 6 m/s
     (22 km/h). Any wind speed over 5 m/s (18 km/h) should be recorded.
   - Do not operate in conditions below 0 °C or above 40 °C.
   - Try to ensure the sun is within ±20° of solar noon (approximately
     2 hours before or after noon). **However**, *this depends on time of
     year and latitude — check
     [NOAA solar calculator](https://gml.noaa.gov/grad/solcalc/) if
     unsure.*
2. Turn on the aeronautical radio and set to local CTAF (find in
   [ERSA](https://www.airservicesaustralia.com/aip/aip.asp)).
3. *Optional, if over 50 km from a CORS station:* set up the Emlid RTK
   (install a peg or permanent GCP below the base station for recurring
   flights), let it run for at least 15 minutes, and start recording the
   RINEX file before flying.
4. Set up reflectance panels, the validation panel, and GCPs following the
   [Standard Flight Procedure](../../FlightDesign/StandardFlight/Standard_Flight.md).
   In summary:

   - Use the [Dual ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#dual-elm-panel-flight)
     layout by default (mandatory for missions > 15 minutes).
   - Fall back to the [Single ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#single-elm-panel-flight)
     layout only when a second panel set is unavailable or for short
     missions (≤ 10 minutes) under stable illumination.
   - Deploy the 2-panel validation set with its paired elevated + ground
     GCPs per [Validation Panel Setup](../../FlightDesign/StandardFlight/Standard_Flight.md#validation-panel-setup).
   - Follow the [Panel Setup](../../FlightDesign/StandardFlight/Standard_Flight.md#panel-setup)
     rules (level folding table, panels in brightness order aligned with
     the flight direction, record panel-surface height).
   - Place 5 GCPs per [Ground Control Points](../../FlightDesign/StandardFlight/Standard_Flight.md#ground-control-points)
     — in alternate flight lines, next to the calibration panels.
   - When flying multiple missions, panels and GCPs must be present in
     every flight (panels overflown approximately every 15 minutes).

   ![GRYFN 4-panel ELM reflectance set laid out in brightness order on a levelled folding table in an open, unshaded area](GOBI_M350_FieldBook_media/panel_setup_table.jpg)

   *Figure: Correctly deployed 4-panel ELM reflectance set on a levelled
   folding table. See the
   [Standard Flight Procedure](../../FlightDesign/StandardFlight/Standard_Flight.md#panel-setup)
   for the full panel and GCP layout diagrams.*

   ![Top-down diagram of a dual ELM panel flight: survey polygon with two 4-panel reflectance sets placed at the 1/3 and 2/3 positions along the long axis, validation panel set near the centre, flight lines aligned with the long axis, ~10% edge-of-polygon buffer, and a 5-GCP layout (two centre validation GCPs, two corner GCPs, one off-axis GCP)](GOBI_M350_FieldBook_media/dual_elm_panel_flight_diagram.png)

   *Figure: Dual ELM panel flight layout — default layout for GOBI
   M350 surveys. See the
   [Dual ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#dual-elm-panel-flight)
   section of the Standard Flight Procedure for the full rationale and
   alternative layouts.*

5. Set up the landing pad and UAV in a safe RTH location.
   - In dusty environments an additional tarp should be used under the
     landing pad.
6. On the controller, set up a safe UAV RTH location, RTH altitude, and
   other geo-fencing settings.
7. Attach the GOBI payload to the aircraft:
   - Check that all lenses and sensors are clean. If not, use professional
     lens cleaning wipes to clean them.
   - Connect the power cable from aircraft to payload (non-standard
     payload bus only).
   - Connect both GNSS antenna cables to the correct ports (match A1 and
     A2).
   - **Remove RGB and Nano HP lens caps.**
   - Insert the RGB SD card.
   - Connect the C-type power cable to power the gimbal (make sure the
     B side of the cable is toward the outer side of the drone and the
     upper side of the light sensor).
   - *Optional, in warm ambient conditions:* activate the battery-powered
     fan. Avoid leaving GOBI powered on in stagnant air or high heat
     (> ~35–38 °C).
8. Power on the radio controller; check battery status.
9. Launch DJI Pilot 2 on the M350 controller.
10. Review the flight plan, checking operational height and double-checking
    that area of interest, GCPs, and reflectance panels are all within the
    capture polygon.
11. Power on the aircraft; confirm connection to RC/GCS and battery status.
12. If you are using RTK, check that you are receiving GNSS corrections.
    There should be more than 8 satellites for good correction (check the
    RTK fix in the M350 controller settings).

---

## GOBI Sensor Configuration

1. Place the 80% exposure reference panel under the Nano HP lens; avoid
   casting shadows.
   - Place the drone legs upon exposure-angle cones, ensuring a fixed
     angle is achieved each exposure setting.
   - The maximum distance from the VNIR HS sensor to the GOBI reflectance
     panel is 40 cm. This is calculated based on the FOV and a 20 cm panel
     size.
2. Connect to GOBI Wi-Fi or connect the ethernet cable to the payload.
   *If using ethernet, ensure a static IP address for the sensor at*
   `10.0.65.2`.
3. Navigate to the GOBI WebUI at `10.0.65.50`.
4. Ensure a valid altitude is shown in the top-right corner of the WebUI
   (~10–20 m error is acceptable). (accurate position/elevation should be within ~30-60 seconds from power on)
5. Name the mission, using the convention `YYYYMMDD_XXXX`
   (`YYYY` = year; `MM` = month; `DD` = day, must be 2 digits;
   `XXXX` = a short reference or abbreviation for the job).
6. Ensure the correct UAV (M350) is selected in the dropdown box.
   **This is very important.**

> [!CAUTION]
> Selecting the incorrect drone will render all data captured unrecoverable

7. Scroll down to polygon settings. Ensure triggering is set by
   **Polygon** in the dropdown menu. Set min and, optionally, max altitude
   triggers (recommended to set a minimum of at least 20 m to prevent
   unnecessary data capture).
8. Import the polygon KML or select from previously flown missions using
   the dropdown menu. Inspect the map to ensure it aligns with
   expectations.
9. Return to the top of the WebUI, open the Flight Calculator, and update
   flying height / flight speed (default 40 m height, default speed
   3 m/s); adjust the oversampling buffer (recommended setting is 30%).
   Frame period will be automatically updated.
10. Adjust exposure until ~95% spectral saturation at the **peak of the
    90th percentile line** without exceeding the frame period. Use the
    lowest gain mode that still provides sufficient saturation.
11. Place the lens cap on the Nano HP.
12. Click **Collect Dark Reference**; wait to finish.
13. **Remove the lens cap.**
14. Remove the exposure reference panel and return it to its case.
15. Press **Start Mission**.
16. Remove the ethernet cable (if using) and begin flight operations.

---

## Flight Operations

1. Ensure the aircraft is in **Position** flight mode.
2. Clear people/objects away from the UAV.
3. Notify crew/observers that takeoff is beginning.
4. Begin manual takeoff, check stick controls work, and then fly to ~12 m
   AGL; **do not exceed** the trigger altitude (20 m default) before
   mission start.
5. Perform dynamic alignment (one to two figure-8 patterns) at a lower
   height than capture height and outside of the capture polygon area.
6. Enable autonomous mission.
7. Monitor UAV battery voltage/percentage in flight.
8. After mission, switch back to **Position** mode to regain manual
   control.
9. Lower to below capture height and perform post-mission dynamic
   alignment (figure-8).
10. Land and disarm UAV. Be careful when landing the M350 — downward
    sensors are blocked when using GOBI, so bring it in slowly.
11. Leave the UAV, transmitter, and sensor powered on; begin post-flight
    checks. Ensure data transfer is finished before turning off the sensor.

> [!WARNING]
> **There is no hot-swap protection on the GOBI.** Do not mate or de-mate
> the Smart Dovetail while the aircraft is powered.

> [!WARNING]
> Hot-swapping batteries is **not recommended** with any high-end GNSS
> solution. Static data can cause IMU drift and degrade post-processed
> trajectory quality. **Treat each flight as a new flight.**

---

## Post-Flight Sensor Configuration

1. Reconnect to GOBI Wi-Fi or ethernet cable.
2. Open a browser to `10.0.65.50` or refresh the UI.
3. Press **Stop Mission**.
4. Replace the Nano HP and RGB lens caps.
5. In the WebUI, ensure all data has captured correctly (see the
   [Data Confirmation](#data-confirmation) table below).

---

## Data Confirmation

| Sensor type   | File count                                                   | File size                                  | Notes                                                                                                  |
| :------------ | :----------------------------------------------------------- | :----------------------------------------- | :----------------------------------------------------------------------------------------------------- |
| **GNSS-INS**  | 1 file                                                       | ~1.5 MB per minute                         | A new file will be created if time rolls over the hour (UTC time, not capture time).                   |
| **LiDAR**     | 1 per minute                                                 | ~175 MB per file                           | First & last file may be smaller. Files may be smaller if over a low-reflectivity object.              |
| **VNIR**      | Mission time (s) / Frame Period value ~ number of data cubes | Several GB even for short flights.         | Check that `frameindex`, `imu_gps`, and `settings` files all exist.                                    |
| **RGB**       | One image every 2 s\* (by default)                           | ~30–70 MB per image                        | Check the event file vs the number of images.                                                          |

---

## Post-Flight

1. Power off the drone, then the controller. Inspect the drone for any
   damage and report for maintenance if necessary.
2. Disconnect the ethernet cable from the payload, if using.
3. Disconnect the aircraft batteries.
4. Disconnect the payload power cable.
5. Disconnect the payload GNSS cables.
6. Undo the mounting clamp, remove the payload, and place it in its case.
7. Pack up the drone and all gear from site, double-checking against the
   checklist at the start of this document to ensure everything is
   accounted for.

---

## Offload Data

1. Open WinSCP, FileZilla, or similar FTP software.
2. Connect to the GRYFN Gobi via `gryfn@10.0.65.50` (password: `gryfn`).
3. Raw GNSS & LiDAR PCAP location: `/data/{mission name}`.
4. VNIR data location: `/data/capturedData/captured/{dateTime}`.
5. GOBI logs location: `/data/gryfn.log.{date}`.
6. RGB images are stored on the camera SD card.
7. Download GNSS, LiDAR, logs, and RGB after each mission; download
   hyperspectral data later due to its size.
8. Clear data directories after download and backup to avoid filling the
   500 GB SSD.

- Gryfn Processing Tool v1.94 and greater can query the sensor directly with
  the new firmware and so can be used to offload the data. In some cases
  where the drone is powered off too quickly after stopping the mission the
  GNSS data may be absent. This can be recovered by connecting directly to
  the SBG via FTP — instructions for connection below.

### Data storage, processing & validation

All paths below follow the
[APPN folder structure](https://github.com/ArdenB/APPN_GenricFileStorage/wiki/Folder-Structure).
Formal paths use the wiki's placeholder syntax; an example follows each.

1. Downloaded data should be stored in the correct `T0_raw` folder.

   Formal path:

   ```
   ./{Node}/
     {YYYY_ProjectDesc[_I|E][_Researcher][_org]}/
     {YYYYSiteName[_F|C]}/
     {SensorPlatform}/{YYYYMMDD}/run_XX/T0_raw/
   ```

   Example:

   ```
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/GOBI/20250825/run_00/T0_raw/
   ```

2. Data from the GCP points should be saved in the `T0_raw/Vault` folder
   (location may change).

   Formal path:

   ```
   ./{Node}/
     {YYYY_ProjectDesc[_I|E][_Researcher][_org]}/
     {YYYYSiteName[_F|C]}/
     {SensorPlatform}/{YYYYMMDD}/run_XX/T0_raw/Vault/
   ```

   Example:

   ```
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/GOBI/20250825/run_00/T0_raw/Vault/
   ```

3. Important information and issues should be recorded in the
   `FieldNotes.txt` file.

   Formal path:

   ```
   ./{Node}/
     {YYYY_ProjectDesc[_I|E][_Researcher][_org]}/
     {YYYYSiteName[_F|C]}/
     {SensorPlatform}/{YYYYMMDD}/FieldNotes.txt
   ```

   Example:

   ```
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/GOBI/20250825/FieldNotes.txt
   ```

4. Per-run information (e.g. APEx test conditions) should be stored in
   `RunOverview.csv`.

   Formal path:

   ```
   ./{Node}/
     {YYYY_ProjectDesc[_I|E][_Researcher][_org]}/
     {YYYYSiteName[_F|C]}/
     {SensorPlatform}/{YYYYMMDD}/RunOverview.csv
   ```

   Example:

   ```
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/GOBI/20250825/RunOverview.csv
   ```

   > [!IMPORTANT]
   > When data from a failed run is being kept (e.g. debugging with GRYFN),
   > the `RunFailed` boolean column of `RunOverview.csv` must be set to
   > `True`.

5. The bundled `.graw` should be saved in the same `T0_raw` folder.
6. The `.gpro` should be generated using the APPN GPT Pipeline (see the
   [Processing Pipelines](../../Pipelines/GryfnProcessingPipeline/Gryfn_Processing_Pipeline.md)
   document and the
   [`Gobi_standard_pipeline_v1.0.yaml`](../../Pipelines/GryfnProcessingPipeline/yaml/Gobi_standard_pipeline_v1.0.yaml)
   in the adjacent
   [`yaml/`](../../Pipelines/GryfnProcessingPipeline/yaml/) folder) and
   stored in the adjacent `T1_proc` folder.

   Formal path:

   ```
   ./{Node}/
     {YYYY_ProjectDesc[_I|E][_Researcher][_org]}/
     {YYYYSiteName[_F|C]}/
     {SensorPlatform}/{YYYYMMDD}/run_XX/T1_proc/
   ```

   Example:

   ```
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/GOBI/20250825/run_00/T1_proc/
   ```

7. Standard QA process should be performed following
   [this guide](../../QA/QAprocess/AerialDataQC.md).

   > [!NOTE]
   > Missing pixels along VNIR image edges can result from insufficient
   > overlap, trajectory irregularities, overly restrictive KML
   > boundaries, or a restrictive processing extent. Where practical,
   > process with no fixed extent (or against a generously buffered
   > capture polygon) so valid edge data is not clipped.

---

## Appendix

### FTP & Service Reference

| Service           | Protocol | IP                           | Port | User       | Password |
| ----------------- | -------- | ---------------------------- | ---- | ---------- | -------- |
| GOBI              | FTP      | `10.0.65.50`                 | 22   | `gryfn`    | `gryfn`  |
| SBG               | FTP      | `10.0.65.100`                | 21   | `operator` | *(none)* |
| Headwall polygon  | HTTP     | `apps.headwallphotonics.com` | —    | —          | —        |

### Headwall Polygon Tool — Workflow

1. Import KML.
   - Must be in Google Earth format.
2. Export KML.
3. Save KML.
4. In File Explorer, rename the KML as the survey name.

### Setting a Static IP Address

If connecting over an ethernet connection, a static IP address will need to
be set:

1. Open *Network and Internet Settings*.
2. Open *Change Adapter Options*.
3. Open *Properties* for the Ethernet port/adapter.
4. Open *Properties* for IPv4.
5. Set IP address to `10.0.65.2`.
6. Set subnet mask to `255.255.255.0`.
7. Press **OK**.

### IP Address Reference

| Service      | Address              |
| ------------ | -------------------- |
| GRYFN WebUI  | `10.0.65.50`         |
| HSInsight    | `10.0.65.50:8080`    |
| Ouster       | `10.0.65.128`        |
| SBG          | `10.0.65.100`        |
