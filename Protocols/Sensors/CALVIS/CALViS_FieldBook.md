# APPN – CALViS Fieldbook

For a high-level description of the CALViS sensor package and how it sits
alongside the other APPN platforms, see the
[Platforms Overview](../../Background/PlatformsOverview/Platforms_Overview.md).

This fieldbook provides a standardised operational guide for APPN CALViS UAV
deployments, supporting safe flight operations, consistent sensor
configuration, and high-integrity data capture. It is intended for trained
APPN staff conducting hyperspectral, LiDAR, and GNSS-INS data acquisitions,
and promotes repeatability, transparency, and confidence in downstream data
analysis across APPN operations.

> [!IMPORTANT]
> **This protocol must be followed for all standard APPN CALViS UAV flights.**
> Adherence to these procedures is essential to ensure operational safety, data
> integrity, and comparability of datasets across deployments. For any flights
> that **fall outside standard operating procedures**, detailed records must be
> kept documenting all deviations, including the specific settings changed, the
> rationale for those changes, and any anticipated implications for data
> quality or analysis.

---

## Document Structure

1. [Equipment Checklist](#equipment-checklist) — what to bring to
   the field.
2. [Preflight Planning](#preflight-planning) — survey / capture
   polygons, QGroundControl setup, and mission upload.
3. [Onsite Preflight Operations](#onsite-preflight-operations) —
   weather and airspace checks, panel and GCP layout, aircraft setup.
4. [CALViS Sensor Configuration](#calvis-sensor-configuration) —
   sensor power-up and per-flight configuration:
   - [Setting Exposure](#setting-exposure)
   - [Setting Flight Plan](#setting-flight-plan)
   - [Setting Dark Reference](#setting-dark-reference)
   - [Setting Flight Parameters](#setting-flight-parameters)
   - [Completing the Setup](#completing-the-setup)
5. [Flight Operations](#flight-operations) — takeoff, dynamic
   alignment, autonomous mission, and landing.
6. [Post-Flight Sensor Configuration](#post-flight-sensor-configuration) —
   stopping the capture and confirming data on the sensor.
7. [Post-Flight](#post-flight) — pack-down of aircraft, payload, and
   ground reference kit.
8. [Data Integrity Checks](#data-integrity-checks) — expected file
   counts and sizes by sensor.
9. [Data Confirmation & Download](#data-confirmation--download) —
   downloading from the sensor and storing under the APPN folder
   structure:
   - [VNIR, SWIR + LiDAR](#vnir-swir--lidar)
   - [GNSS-INS](#gnss-ins)
   - [Data storage, processing & validation](#data-storage-processing--validation)
10. [Appendix](#appendix) — static IP setup, network reference, and
    Headwall polygon tool workflow.

---

## Equipment Checklist

> [!NOTE]
> Ensure batteries for all equipment are fully charged before heading to the
> field. Ensure charging cables are available for necessary equipment.

> [!WARNING]
> Cables and adaptors (particularly ethernet and USB) are known to fail in
> the field on hot days. Bring spares of all critical cables and adaptors
> wherever possible.

- [ ] **Aircraft**
  - [ ] Inspired Flight IF1200A
  - [ ] Aircraft batteries and spares
  - [ ] Landing gear
  - [ ] Landing pad
  - [ ] Spare parts
  - [ ] Tools
  - [ ] Logbook
- [ ] **Radio Control Transmitter / Ground Control Station**
- [ ] **Headwall CoAligned HP sensor payload**
  - [ ] Headwall coaligned system
  - [ ] GNSS antennae
  - [ ] Dovetail to XT60 + XT60 to XT30 cables/adapter
  - [ ] Dual ethernet cable and adaptor
  - [ ] USB-C to USB-A adaptor (for laptops without a USB-A port, it can
        be a combined ethernet usb dongle).
  - [ ] Quality USB-A to USB-C cable for connecting a PC to the IF1200
        (e.g. [LTT TrueSpec USB Type-A to C](https://global.lttstore.com/products/ltt-truespec-cable-usb-type-a-to-c)).
        A USB-C to USB-C cable will **not** work.
  - [ ] GRYFN Exposure reference panel
  - [ ] Hyperspectral capture polygon exported from the
        [HPI Polygon Tool](http://50.170.92.179/)
- [ ] **Ground reference kit**
  - [ ] 2 x Reflectance calibration panels (11%, 30%, 56%, 82%)
  - [ ] 1 x Calibration validation panels (20%, 45%)
  - [ ] 5 × Propeller Aeropoints or other ground control points (GCPs) and/or RTK GNSS system 
  - [ ] 2 × folding tables to elevate panels
- [ ] **Accessories**
  - [ ] Safety gear (signage and high-vis vests)
  - [ ] Aeronautical radio
  - [ ] Field laptop (with Headwall software installed) and spare batteries
  - [ ] External storage media
  - [ ] Water, food, esky, sunscreen, bug spray, first aid kit, etc.
  - [ ] Spirit bubble, spirit level (or angle measurement) and measuring tape
  - [ ] External power brick (for charging UAV RC)
  - [ ] Anemometer (e.g. [Kestrel 2000 Wind Meter](https://kestrelmeters.com.au/products/kestrel-2000-wind-meter))

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
> when planning flights. Further planning documentation can be found in the
> [IF1200 manual](https://docs.inspiredflight.com/inspired-documentation/products/aircraft/if1200/if1200-manual).

1. Using a GPS survey system (Emlid, Trimble…) or a GIS software, create a
   polygon of the area of interest. Make sure your polygon includes the areas
   where you will place your calibration panels and GCPs, with an additional
   5 m buffer to avoid incomplete data.
2. Save this polygon twice as a KML — once as a *survey* polygon and once as a
   *capture* polygon.
3. If using QGIS, export the polygon as a KML (in Geometry, select *include
   z-dimension*, and ensure the CRS is set to WGS 84). Import the *capture*
   KML into the [HPI Polygon Tool](http://50.170.92.179/) and export. This
   polygon sets the activation of the hyperspectral sensor within Hyperspec3.
4. Open QGroundControl (QGC)
   ([download here](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html))
   on a PC and connect a USB-A cable to the PC and USB-C into the USB-C port
   at the top of the IF1200. Alternatively, connect a Ground Control station
   directly to the IF1200 via the Herelink hotspot
   ([follow steps here](https://docs.inspiredflight.com/inspired-documentation/products/aircraft/if1200/operating-the-aircraft/wireless-connection-to-ground-station)).


> [!IMPORTANT]
> A USB-C to USB-C cable will **not** work for the QGC <-> IF1200 connection
> — you must use a USB-A to USB-C cable.

   - Set up a comm link in QGC by pressing the **Q** in the top left of QGC.
   - Navigate to *Application Settings* → *Comm Links*.
   - Click the **Add** button.
   - Name your comm link and change the settings as displayed below.

      ![QGC comm link settings](CALViS_FieldBook_media/image_e9ed2be4e34c.png)

   - Once this is complete, the IF1200 should now connect to a PC when QGC
     is open, allowing you to upload flight missions over a wired
     connection.

5. Using the *survey* polygon previously created, use QGC to create a survey
   of the area of interest.
   - Navigate to *Flight Plan* by pressing the **Q** at the top left of QGC.

      ![QGC flight plan menu](CALViS_FieldBook_media/image_cab0fe550221.png)

   - Select *Pattern* → *Survey* from the options on the LHS.

      ![QGC pattern survey selection](CALViS_FieldBook_media/image_6e4660fa0f8a.png)

   - Select *Load KML/SHP*. Navigate to and import the previously created
     *survey* polygon.

      ![QGC load KML/SHP](CALViS_FieldBook_media/image_9e03ea164d26.png)

   - Ensure the angle is as close to 0° or 180° as possible, as a
     North–South flight direction is preferred for hyperspectral data
     capture.

      ![QGC survey angle setting](CALViS_FieldBook_media/image_d32c53625edd.png)

      ![QGC survey angle aligned](CALViS_FieldBook_media/image_19e4341ae663.png)

6. Using both QGroundControl and the GRYFN flight calculator, determine the
   speed, altitude, and frame period required to survey the area of interest.
   See the [Standard Mission Parameters](#standard-mission-parameters) table
   below for examples of common flight types.
   - Do not go below 2 m/s or above 8 m/s for stability reasons (GRYFN).
     Speeds greater than 8 m/s lead to excessive aircraft pitch and speeds
     less than 2 m/s accentuate the impacts of wind on aircraft stability and
     cause a visibly less smooth trajectory.
   - Altitude and speed will be tested and recommended from APEx results.
   - Ensure the frame period is at a minimum of 20% oversampling, the side
     overlap is > 30% for the SWIR sensor (> 40% for the VNIR), and the
     *turnaround distance* is 2× flight speed (> 3× at > 6 m/s). See the
     note under the
     [Standard Mission Parameters](#standard-mission-parameters) table
     for why the SWIR sensor is used to set the side overlap.
7. Ensure flight lines are in the direction of planting (GRYFN).

> [!NOTE]
> GRYFN recommends aligning flight lines with the planting direction for
> "operational reasons", as this greatly reduces stitching artefacts
> between adjacent flight lines. Testing flight-line angle is a top
> priority for APEx, and this guidance may be revised before the start of
> the season.

8. Once the flight plan is complete, upload it to the UAV using the wired
   connection from PC to IF1200. Once the UAV is powered and connected to the
   RC, the mission will appear on the RC. If the mission doesn't appear on
   the RC, navigate to *Flight Planning* → *File* → *Vehicle* → *Download*.

### Standard Mission Parameters

| Standard Mission Type | Scenario                                                              | Altitude (m) | Speed (m/s) | Frame Period VNIR        | Frame Period SWIR        | Distance between flight lines — IF1200 (m) |
| :-------------------- | :-------------------------------------------------------------------- | :----------: | :---------: | :----------------------- | :----------------------- | :----------------------------------------: |
| Type 1                | Plant counting / small structures (intra-plot differences)           |      30      |     2.1     | 5.32 (20% oversampling)  | 6.0 (30% oversampling)   |                     8                      |
| Type 2                | Plant breeding experiments (inter-plot differences)                  |      50      |     3.2     | 5.09 (30% oversampling)  | 6.56 (30% oversampling)  |                     13                     |
| Type 3                | Large landscape measurements (strip trials, hyperspectral transects) |      80      |     5.1     | 5.11 (30% oversampling)  | 6.59 (30% oversampling)  |                     21                     |

> [!NOTE]
> **Side overlap is planned from the SWIR sensor.** The SWIR has a
> narrower swath than the VNIR at the same altitude, making it the
> limiting sensor for flight-line spacing — CALViS carries no RGB frame
> camera, so the hyperspectral sensors govern the plan. Setting > 30%
> side overlap on the SWIR in the GRYFN flight calculator automatically
> gives > 40% on the wider VNIR swath. Planning from the VNIR instead
> would leave gaps in SWIR coverage between adjacent flight lines.

---

## Onsite Preflight Operations

1. Conduct airspace and weather checks. No cloud cover. Maximum wind speed
   for quality hyperspectral capture is **6 m/s (~22 km/h)**. Any wind
   speed over **5 m/s (~18 km/h)** should be recorded. Try to ensure the
   sun is ±20° of solar noon, approximately 2 hours before or after noon
   (**however**, this will depend on time of year and latitude — please
   check [here](https://www.suncalc.org/#/-30.3252,149.7855,9/2026.06.10/14:23/1/3) if unsure).

   > [!NOTE]
   > **To be confirmed by in-field testing:** solar noon is a moving target,
   > so a ±20° window effectively permits flying at a lower solar altitude in
   > Adelaide than in Brisbane. Whether to instead adopt the
   > [GRYFN recommendation](https://gryfn.gitbook.io/gryfn-operations/sensing-technologies/hyperspectral)
   > will be evaluated as part of the APEx experimental flights this season,
   > and is tracked in the [future-release backlog](../../../TODO_FUTURE.md).

2. Turn on the aeronautical radio and set to local CTAF (find in
   [ERSA](https://www.airservicesaustralia.com/aip/aip.asp)).

3. Set up reflectance panels, the validation panel, and GCPs following the
   [Standard Flight Procedure](../../FlightDesign/StandardFlight/Standard_Flight.md).
   In summary:

   - Use the [Dual ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#dual-elm-panel-flight)
     layout by default (mandatory for missions > 15 minutes — noting that
     IF1200 missions are typically < 30 minutes, so a calibration panel is
     overflown approximately every 15 minutes).
   - Fall back to the [Single ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#single-elm-panel-flight)
     layout only when a second panel set is unavailable.
   - Deploy the 2-panel validation set with its paired elevated + ground
     GCPs per [Validation Panel Setup](../../FlightDesign/StandardFlight/Standard_Flight.md#validation-panel-setup).
   - Follow the [Panel Setup](../../FlightDesign/StandardFlight/Standard_Flight.md#panel-setup)
     rules (level folding table, panels in brightness order aligned with
     the flight direction, record panel-surface height).
   - Place 5 GCPs per [Ground Control Points](../../FlightDesign/StandardFlight/Standard_Flight.md#ground-control-points)
     — in alternate flight lines, next to the calibration panels.
   - When flying multiple missions, panels and GCPs must be present in
     every flight (panels overflown approximately every 15 minutes).

   ![GRYFN 4-panel ELM reflectance set laid out in brightness order on a levelled folding table in an open, unshaded area](CALViS_FieldBook_media/panel_setup_table.jpg)

   *Figure: Correctly deployed 4-panel ELM reflectance set on a levelled
   folding table. See the
   [Standard Flight Procedure](../../FlightDesign/StandardFlight/Standard_Flight.md#panel-setup)
   for the full panel and GCP layout diagrams.*

   ![Top-down diagram of a dual ELM panel flight: survey polygon with two 4-panel reflectance sets placed at the 1/3 and 2/3 positions along the long axis, validation panel set near the centre, flight lines aligned with the long axis, ~10% edge-of-polygon buffer, and a 5-GCP layout (two centre validation GCPs, two corner GCPs, one off-axis GCP)](CALViS_FieldBook_media/dual_elm_panel_flight_diagram.png)

   *Figure: Dual ELM panel flight layout — default layout for CALViS
   surveys. See the
   [Dual ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#dual-elm-panel-flight)
   section of the Standard Flight Procedure for the full rationale and
   alternative layouts.*

   ![Top-down diagram of a single ELM panel flight: one 4-panel reflectance set deployed near the centre of the survey area, alongside the validation panel set and its paired GCPs, with a 5-GCP layout covering centre, corner and off-axis positions, all inside the ~10% effective capture area inset from the polygon edge](CALViS_FieldBook_media/single_elm_panel_flight_diagram.png)

   *Figure: Single ELM panel flight layout — fallback when a second
   panel set is unavailable. See the
   [Single ELM panel flight](../../FlightDesign/StandardFlight/Standard_Flight.md#single-elm-panel-flight)
   section of the Standard Flight Procedure for full guidance.*

5. Set up the landing pad and UAV in a safe RTH location.

> [!IMPORTANT]
> In dusty environments, an additional tarp must be used under the
> landing pad.

5. Perform all on-ground safety checks for the UAV.

6. Attach the CALViS sensor payload to the aircraft.

> [!CAUTION]
> The IF1200 dovetail has no hot-swap protection, so ensure
> the IF1200 is powered off when attaching or removing the sensor
> ([more details](https://gryfn.gitbook.io/gryfn-hardware/headwall-co-aligned-hp/user-manual/integration)).

   - Connect the 8-inch antenna mast with the Trimble AV18 antenna to the A1
     position (front left).
   - Slide the sensor into the dovetail mount, lock the dovetail latch, and
     clip on the safety wire.
   - Connect the power cable from aircraft to payload dovetail.
   - Connect the A1 GNSS antenna cable.
   - **Remove the lens cap!**
   - Clean the lens with approved cleaner (e.g.,
     [Zeiss Lens Wipes](https://eyesolutions.com.au/products/zeiss-lens-wipes),
     recommended by Gryfn).
   - Leave the LiDAR ethernet cable disconnected until ready for takeoff.

7. Power on the radio controller and/or ground control station.
   - Check radio controller / ground control battery status.

8. Launch QGroundControl on the controller.

9. Review the flight plan. Double-check the area of interest, GCPs, and
   reflectance panels are all within the capture polygon.

10. Power on the aircraft.
    - Ensure the radio controller / ground control station connects to the
      aircraft.
    - Check aircraft battery status.
    - Ensure Remote ID is enabled.

---

## CALViS Sensor Configuration

#### Setting Exposure

1. Place the GRYFN exposure reference panel (82%) under the lens.
   - Point the centerline of the drone at the sun (to reduce shadows).
   - Ensure no shadow is cast on the exposure reference panel.
2. Connect the dual-ethernet cables to the VNIR and SWIR ports.
3. Ensure a static IP is set using the steps in the
   [Appendix](#appendix) of this document.

> [!IMPORTANT]
> For version 1.0 of the protocol we will use Hyperspec III for both
> VNIR and SWIR until issues with HS Insight have been addressed
> with GRYFN. This will change in future versions.

4. Open Hyperspec III.
   - Wait 2–3 minutes for the SWIR sensor to reach capture temperature.
     Hyperspec III won't connect until the SWIR is at temperature
     (listen for the click).
5. Navigate to the VNIR sensor.
   - Open *Live View*.
6. Set the **Frame Period** based on flight parameters using the
   [GRYFN Flight Calculator](https://gryfn.gitbook.io/gryfn-operations/operations/flight-planning/flight-planning-calculator)
   spreadsheet.

> [!IMPORTANT]
> The GRYFN Flight Calculator is the only approved calculator. Other
> calculators (e.g. Headwall) will give incorrect values and result in
> significant amounts of missing data.

7. Adjust the Frame Period for sufficient oversampling.
   - GRYFN recommends adjusting oversample to 20% for standard flights and
     conditions.
8. Adjust **Exposure** until the spectral graph is ~95% saturated at peak
   (dependent on spectra of interest), while not exceeding the frame period
   value.
   - When adjusting exposure, use the lowest gain mode possible while still
     achieving sufficient saturation to boost SNR, without adjusting frame
     period.
   - HyperspecIII displays two graphs of counts — one in the spatial and one
     in the spectral dimension. For exposure setting the spectral graph
     should be the main focus, and the X-hair toggle at the top of the
     screen may be useful for selecting a brightly lit part of the scene.
     For SWIR, counts are on the y-axis with a max of 65,000, so 95% ≈ 61,750 counts. For VNIR, counts are on the y-axis with a max of 4095 so 95% ≈ 3890 counts
     HSinsight works similarly, though it measures in intensity and so ~95%
     can be set directly.
   - [Example](https://www.youtube.com/watch?v=8DAJToJKQhw).

> [!NOTE]
> (GRYFN) Low gain is unlikely to ever be possible with SWIR. They are
> in contact with Headwall to see if more granular gain settings are
> possible. 

> [!NOTE]
> A reference image and notes covering the recommended sensor angle during
> exposure setting will be added in a future revision (tracked in the
> [future-release backlog](../../../TODO_FUTURE.md) and
> [`/IMAGE_TODO.md`](../../../IMAGE_TODO.md)).

9. Toggle Hyperspec III to the SWIR sensor.
10. Repeat the frame period / exposure steps for the SWIR sensor.

#### Setting Flight Plan

1. Open the **GPS** tab.
2. Upload the HSI polygon KML file.
   - Take note of the current ground-level altitude.
3. Open the **Capture** tab.
   - Ensure the number of active polygons shows the correct amount.
4. Update the name of the data collection folder.
5. Ensure the frame period and exposure have carried over properly from the
   Live Video tab sensors.
6. Ensure the number of active polygons shows the correct amount.

#### Setting Dark Reference

1. Place the lens cap on the sensor.
2. Toggle **Dark Reference**.
3. Click **Start**.
   - You should hear two clicks, signalling that the dark reference is
     complete.

#### Setting Flight Parameters

1. After the dark reference is complete, open the **Advanced** tab next to
   polygons.
2. Recall your ground-level altitude from the GPS tab.
3. Set the minimum polygon altitude high enough above ground level to allow
   the alignment procedure.
   - 5–10 m above ground at minimum, preferably at least 20 m below flight
     altitude if possible.
4. Set the maximum polygon altitude high enough above your flight altitude
   to give buffer in case of slight altitude offsets.
   - At least 10 m above flight altitude. Unless the pilot plans on flying
     above the polygon to return home after the mission, this can be set as
     high as you'd like.
5. Press **Start**. Wait until the **Pause** icon appears.

#### Completing the Setup

1. Disconnect the ethernet cable from the VNIR and SWIR ports.
2. Remove the exposure reference panel.
3. **Remove the lens cap!**
4. Connect the LiDAR ethernet cable to the VNIR port.
5. Begin flight operations.

---

## Flight Operations

1. Ensure the aircraft is in **Position** flight mode.
2. Sync the flight plan to the radio controller if applicable.
3. Double-check the flight plan.
4. Clear people/objects away from the UAV.
5. Let crew/observers know you are about to take off.
6. Begin manual takeoff. Do not exceed the minimum polygon altitude until
   mission start.
7. Perform a dynamic alignment procedure outside the polygon (a figure-8 and
   back-and-forth manoeuvre at take-off and landing) at a recommended speed
   of **6 m/s**.
8. Enable autonomous mission.
9. Monitor UAV battery voltage/percentage in flight.
10. Upon completion of mission, return the aircraft to **Position** flight
    mode. The pilot will regain manual control of the aircraft.
11. Lower to alignment altitude and complete the post-mission dynamic
    alignment procedure.
12. Land and disarm the UAV.
13. Leave the UAV, transmitter, and sensor power on; move to post-flight
    checklists.

---

## Post-Flight Sensor Configuration

1. Disconnect the LiDAR ethernet cable from the VNIR port.
2. Connect the dual-ethernet cables to the VNIR and SWIR ports.
3. In Hyperspec III, open the **Capture** tab and press **Stop**.
4. Confirm data file sizes in Hyperspec III for both VNIR and SWIR sensors
   through the **Storage** tab.
   - Should be on a scale of thousands to tens or hundreds of thousands,
     depending on flight length.
   - Also look for LiDAR files for the flight; these are the `.pcap` files in
     the VNIR `captureddata` directory.
5. In a web browser, connect to the APX Web Configuration at
   `10.0.65.51:8080`.
6. After logging in, go to **Data Logging** and disable the session.
7. Turn off the drone and sensor.

---

## Post-Flight

1. Disconnect aircraft batteries.
2. Place the lens cap on the sensor.
3. Disconnect the ethernet cable from the payload.
4. Disconnect the payload GNSS cable.
5. Disconnect the payload power cable.
6. Undo the payload mounting latch, remove the payload, and place it in the
   storage case.
7. Pack up the aircraft.
8. Collect reflectance panels.

---

## Data Integrity Checks

| Sensor type  | File count                                | File size           | Notes                                                                                                |
| ------------ | ----------------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------- |
| **GNSS-INS** | 1 file per minute                         | ~1.5 MB per minute  | First/last files may be smaller depending on start/stop time within the UTC minute.                  |
| **LiDAR**    | 1 file per minute                         | ~113 MB per file    | First/last file may be smaller. Files may be smaller over a low-reflectivity object.                 |
| **VNIR**     | mission time (s) / frame period ≈ N cubes | Several GB / flight | Check that `frameindex`, `imu_gps`, and `settings` files all exist.                                  |
| **SWIR**     | mission time (s) / frame period ≈ N cubes | Several GB / flight | Check that `frameindex`, `imu_gps`, `pps`, and `settings` files all exist.                           |

---

## Data Confirmation & Download

### VNIR, SWIR + LiDAR

1. Connect to the VNIR + SWIR ethernet ports.
2. Open Hyperspec III.
3. Open the **Data Storage** tab.
4. Select the mission folder.
5. Choose a local storage destination.
6. Navigate to the mission date.
7. Select all files.
8. Download all files.

In HyperspecIII, the PNG-creation tab at download can be disabled and PNG
creation completed instead in the Gryfn Processing Tool. This has some
advantages in download speed, as well as producing PNGs with greater
contrast among ground reference panels for easier mapping of panel
reflectance. Download via FTP is also an option for the VNIR+Lidar data,
using the logins provided on the sensor.

### GNSS-INS

1. Connect to the SWIR ethernet port.
2. Open a web browser to `10.0.65.51:8080`.
3. Log in.
4. Open the **Data Logging** tab.
5. Click on **INTERNAL**.
6. Download all files from within that mission window.
7. The files required for GPT will be any with a matching UTC time to the
   VNIR or SWIR `imu_gps` file.

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
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/CALVIS/20250825/run_00/T0_raw/
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
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/CALVIS/20250825/run_00/T0_raw/Vault/
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
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/CALVIS/20250825/FieldNotes.txt
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
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/CALVIS/20250825/RunOverview.csv
   ```

> [!IMPORTANT]
> When data from a failed run is being kept (e.g. debugging with GRYFN),
> the `RunFailed` boolean column of `RunOverview.csv` must be set to
> `True`.

5. The bundled `.graw` should be saved in the same `T0_raw` folder.
6. The `.gpro` should be generated using the APPN GPT Pipeline (see the
   [Processing Pipelines](../../Pipelines/GryfnProcessingPipeline/Gryfn_Processing_Pipeline.md)
   document and the
   [`Calvis_standard_pipeline_v1.0.yaml`](../../Pipelines/GryfnProcessingPipeline/yaml/Calvis_standard_pipeline_v1.0.yaml)
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
   ./USYD_Narrabri/2025_SIFCal/2025IAWatson_F/CALVIS/20250825/run_00/T1_proc/
   ```

7. Standard QA process should be performed following
   [this guide](../../QA/QAprocess/AerialDataQC.md).

---

## Appendix

### Setting a Static IP Address

A static IP address needs to be set on the accessing machine to connect to
the sensors:

1. Open *Network and Internet Settings*.
2. Open *Change Adapter Options*.
3. Open *Properties* for the Ethernet port/adapter.
4. Open *Properties* for *Internet Protocol Version 4 (TCP/IPv4)*.
5. Use the following IP address: `10.0.65.2`
6. Subnet mask: `255.255.255.0`
7. Press **OK**.

### Network & Service Reference

| Service                                 | Address / details                                                                                       |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| APX WebUI (APX-15) — SWIR ethernet port | `10.0.65.51:8080`                                                                                       |
| FTP site                                | IP `10.0.65.50`, port `22`, user `hpi`, password `HeadwallnHP###` (`###` = serial number of the Nano HP) |
| Headwall polygon tool                   | `apps.headwallphotonics.com`                                                                            |

### Headwall Polygon Tool — Workflow

1. Import KML.
   - Must be in Google Earth format (i.e. contain a z-dimension).
2. Export KML.
3. Save KML.
4. In File Explorer, rename the KML as the survey name.
