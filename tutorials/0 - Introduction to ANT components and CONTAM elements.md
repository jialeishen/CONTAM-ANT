# Introduction to ANT components and CONTAM elements

ANT conponents are organized into 12 categories, each of which is represented by a tab in the ANT toolbar. The 12 categories are:
<!--ts-->
 - [01-Geometry](#01-geometry)
    - [Zone](#zone)
    - [Door](#door)
    - [Window](#window)
    - [Orifice](#orifice)
    - [Generic opening](#generic-opening)
 - [02-HVAC](#02-hvac)
    - [Air handling system (AHS)](#air-handing-system-ahs)
    - [AHS Supply](#ahs-supply)
    - [AHS Return](#ahs-return)
 - [03-Filter](#03-filter)
    - [Filter element](#filter-element)
    - [Filter efficiency data - Constant efficiency](#filter-efficiency-data---constant-efficiency)
    - [Filter efficiency data - Simple gaseous](#filter-efficiency-data---simple-gaseous)
    - [Filter efficiency data - Simple particle](#filter-efficiency-data---simple-particle)
    - [Filter efficiency data - UVGI](#filter-efficiency-data---uvgi)
 - [04-Schedule](#04-schedule)
    - [Day schedule](#day-schedule)
    - [Week schedule (weekdays & weekends)](#week-schedule-weekdays--weekends)
    - [Week schedule (detailed days)](#week-schedule-detailed-days)
 - [05-Species](#05-species)
    - [Contaminant/Species](#contaminantspecies)
    - [Edit contaminant/species](#edit-contaminantspecies)
    - [Reactant-Product (R-P) pair](#reactant-product-r-p-pair)
    - [Reaction](#reaction)
    - [Particle distribution calculator](#particle-distribution-calculator)
 - [06-Source/Sink](#06-sourcesink)
    - [Source - Constant coefficiency](#source---constant-coefficiency)
    - [Source - Burst mass](#source---burst-mass)
    - [Source - Cutoff concentration](#source---cutoff-concentration)
    - [Source - Decaying source](#source---decaying-source)
    - [Source - Pressure driven](#source---pressure-driven)
    - [Source - Peak model (NRCC)](#source---peak-model-nrcc)
    - [Source - Power-law model (NRCC)](#source---power-law-model-nrcc)
    - [Sink - Constant coefficiency](#sink---constant-coefficiency)
    - [Sink - Deposition rate](#sink---deposition-rate)
    - [Sink - Deposition velocity](#sink---deposition-velocity)
    - [Sink - Deposition with resuspension emission](#sink---deposition-with-resuspension-emission)
    - [Sink - Boundary layer diffusion model](#sink---boundary-layer-diffusion-model)
 - [07-Occupancy](#07-occupancy)
 - [08-Airflow](#08-airflow)
 - [09-Library](#09-library)
 - [10-Ambient](#10-ambient)
 - [11-Simulation](#11-simulation)
 - [12-Results](#12-results)
<!--te-->
CONTAM elements are essential for creating CONTAM projects (PRJ) and are associated with ANT components and created by ANT components. Detailed introductions of CONTAM elements can be found in [CONTAM User Guide](https://www.nist.gov/publications/contam-user-guide-and-program-documentation-version-34). Some elements are introduced here for better understanding of ANT components.

## 01-Geometry
![geometry components](./img/icons-geometry.png)
### Zone
Create zones.\
Zones with default names of `_zone_?` are created, where `?` is the number of zone created.
 - **Inputs**:
    - **_zone** [required]:
        - Type: Brep geometry [List]
        - Default: None
        - Description: Zone geometry (brep) acquired from Rhino.
    - **temperature**: 
        - Type: Number / Week Schedule (temperature) [Item]
        - Default: 20 °C (constant)
        - Description: Zone temperature. Same input for all specified zones (in **_zone**). Unit can be changed by right-clicking the component and selecting the desired one.
            - Number input: Constant temperature over time.
            - Schedule input: Variable temperatures over time.
    - **init ctm conc**: 
        - Type: Number [List]
        - Default: 0 for all contaminants
        - Description: Initial concentrations of simulated contaminants (specified in [Project](#project) component). Must match the size of contaminant inputs in [Project](#project) component. Same input for all specified zones (in **_zone**).
    - **srcs/sinks**: 
        - Type: Source/Sink [List]
        - Default: None
        - Description: Source/Sink elements. Same input for all specified zones (in **_zone**).
    - **ahs sup/ret**: 
        - Type: Airflow path (AHS supply/return) [List]
        - Default: None
        - Description: Airflow paths of air handling system (AHS) supply/return elements that work in the zones. Same input for all specified zones (in **_zone**).
- **Outputs**:
    - **zones**:
        - Type: Brep geometry - [List]
        - Description: Brep geometry with updated settings.

### Door
Create doors.\
Each door by default contains an airflow path for the door (*two opening model* in two-way opening models) and an airflow path for the undercut (*orifice model* in one-way flow using powerlaw models).
- **Inputs**:
    - **_openings** [required]:
        - Type: Brep/surface geometry [List]
        - Default: None
        - Description: Door geometry (brep/surface) acquired from Rhino.
    - **wpp\:door**:
        - Type: Wind pressure profile [Item]
        - Default: None
        - Description: Wind pressure profile (WPP) for doors. Same input for all specified doors (in **_openings**). WPP is available only on exterior surfaces.
    - **ucut area**:
        - Type: Number [Item]
        - Default: Door width × default undercut thickness (1 in or 25.4 mm)
        - Description: Door undercut area. Same input for all specified doors (in **_openings**). 
    - **wpp:ucut**:
        - Type: Wind pressure profile [Item]
        - Default: None
        - Description: Wind pressure profile (WPP) for door undercuts. Same input for all undercuts of specified doors (in **_openings**).
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the airflow paths doors. Same input for all specified doors (in **_openings**). Only available for airflow paths of doors (not working on undercut airflow paths).
- **Outputs**:
    - **openings**:
        - Type: Brep/surface geometry [List]
        - Description: Door geometry (brep/surface) with updated settings.
        
### Window
Create windows.\
Each window by default contains an airflow path. If the window is an openable window (with input on **sched**), a *two opening model* in two-way opening models is applied to the window. If the window is a fixed window (without input on **sched**), an *one opening model* in two-way opening models is applied.
- **Inputs**:
    - **_openings** [required]:
        - Type: Brep/surface geometry [List]
        - Default: None
        - Description: Window geometry (brep/surface) acquired from Rhino.
    - **wpp**:
        - Type: Wind pressure profile [Item]
        - Default: None
        - Description: Wind pressure profile (WPP) for windows. Same input for all specified windows (in **_openings**). WPP is available only on exterior surfaces.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the airflow paths of windows. Same input for all specified windows (in **_openings**). Only available for airflow paths of openable windows (not working on fixed windows).
- **Outputs**:
    - **openings**:
        - Type: Brep/surface geometry [List]
        - Description: Window geometry (brep/surface) with updated settings.

### Orifice
Create orifices.\
Each orifice by default contains an airflow path of the orifice (*orifice model* in one-way flow using powerlaw models).
- **Inputs**:
    - **_openings** [required]:
        - Type: Brep/surface geometry [List]
        - Default: None
        - Description: Orifice geometry (brep/surface) acquired from Rhino.
    - **wpp**:
        - Type: Wind pressure profile [Item]
        - Default: None
        - Description: Wind pressure profile (WPP) for orifices. Same input for all specified orifices (in **_openings**). WPP is available only on exterior surfaces.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the airflow paths of orifices. Same input for all specified orifices (in **_openings**).
- **Outputs**:
    - **openings**:
        - Type: Brep/surface geometry [List]
        - Description: Orifice geometry (brep/surface) with updated settings.

### Generic opening
Create generic openings.\
Each generic opening contains an airflow path defined in **_path**. For openings with *fan and forced-flow models*, airflow direction is visualized and can be changed by right-clicking the component and reverse the flow direction.
- **Inputs**:
    - **_openings** [required]:
        - Type: Brep/surface geometry [List]
        - Default: None
        - Description: Generic opening geometry (brep/surface) acquired from Rhino.
    - **_path** [required]:
        - Type: Airflow path [Item]
        - Default: None
        - Description: Airflow path of generic openings. Same input for all specified generic openings (in **_openings**).
- **Outputs**:
    - **openings**:
        - Type: Brep/surface geometry [List]
        - Description: Generic opening geometry (brep/surface) with updated settings.

## 02-HVAC
![HVAC system components](./img/icons-hvac.png)
### Air handing system (AHS)
Create air handling system (AHS).\
A simple AHS with a default name of `_ahs_?` is created, where `?` is the number of AHS created (AHS components that created).
- **Inputs**:\
    ── Out Air ──
    - **min OA**:
        - Type: Number [Item]
        - Default: 0
        - Description: Minimum outdoor air introduced to the AHS supply flow (mass flow rate). Unit can be changed by right-clicking the component and selecting the desired one.
    - **intake loc**:
        - Type: Number [List]
        - Default: 0, 0, 0
        - Description: Outdoor air intake location. Must be a list of 3 numbers, i.e., X, Y, Z coordinates, respectively. Required only when using a WPC file.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the outdoor air intake.

    ── Supply ──
    - **volume**:
        - Type: Number [Item]
        - Default: 0
        - Description: Volume of the supply sub-system (implicit return zone) of the simple AHS (e.g., supply-side duct work etc.). This volume is similar to a zone volume and will be used in the simulation of contaminant transport.
    - **init ctm conc**: 
        - Type: Number [List]
        - Default: 0 for all contaminants
        - Description: Initial concentrations of simulated contaminants (specified in [Project](#project) component) for the supply sub-system of the simple AHS. Must match the size of contaminant inputs in [Project](#project) component.
    
    ── Return ──
    - **volume**:
        - Type: Number [Item]
        - Default: 0
        - Description: Volume of the return sub-system (implicit return zone) of the simple AHS (e.g., return-side duct work etc.). This volume is similar to a zone volume and will be used in the simulation of contaminant transport.
    - **init ctm conc**: 
        - Type: Number [List]
        - Default: 0 for all contaminants
        - Description: Initial concentrations of simulated contaminants (specified in [Project](#project) component) for the return sub-system of the simple AHS. Must match the size of contaminant inputs in [Project](#project) component.
    
    ── Filters ──
    - **OA filter**:
        - Type: Filter element [Item]
        - Default: None
        - Description: Outdoor air filter element which affects the air brought in by the simple AHS from outside the building.
    - **Rec filter**:
        - Type: Filter element [Item]
        - Default: None
        - Description: Recirculation air filter element which affects the return air being circulated back through the simple AHS.
- **Outputs**:
    - **ahs**:
        - Type: Air handling system [Item]
        - Description: Air handling system (AHS) with updated settings.

### AHS Supply
Create AHS supply.
- **Inputs**:
    - **_ahs** [required]:
        - Type: Air handling system [Item]
        - Default: None
        - Description: AHS to which the supply is connected.
    - **flow**:
        - Type: Airflow path [Item]
        - Default: 0
        - Description: Design flow rate of supply air (mass flow rate). Unit can be changed by right-clicking the component and selecting the desired one.
    - **sched**: 
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the supply air.
    - **filter**:
        - Type: Filter element [Item]
        - Default: None
        - Description: Filter element that affects the supply air.
- **Outputs**:
    - **supply**:
        - Type: Airflow path (AHS supply) [Item]
        - Description: Airflow path of AHS supply element with updated settings.

### AHS Return
Create AHS return.
- **Inputs**:
    - **_ahs** [required]:
        - Type: Air handling system [Item]
        - Default: None
        - Description: AHS to which the return is connected.
    - **flow**:
        - Type: Airflow path [Item]
        - Default: 0
        - Description: Design flow rate of return air (mass flow rate). Unit can be changed by right-clicking the component and selecting the desired one.
    - **sched**: 
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the return air.
    - **filter**:
        - Type: Filter element [Item]
        - Default: None
        - Description: Filter element that affects the return air.
- **Outputs**:
    - **return**:
        - Type: Airflow path (AHS return) [Item]
        - Description: Airflow path of AHS return element with updated settings.

## 03-Filter
![Filter components](./img/icons-filter.png)
### Filter element
Create filter element.\
Four types of filter elements are available: constant efficiency, simple gaseous, simple particle, and ultraviolet germicidal irradiation (UVGI). The type of filter element can be selected by clicking the option buttons on the component.
 - **Inputs**:
    - **_name** [required]:
        - Type: Text [Item]
        - Default: None
        - Description: A unique name used to identify the filter element.
    - **desc**:
        - Type: Text [Item]
        - Default: None
        - Description: Detailed description of the filter element.
    - **area**:
        - Type: Number [Item]
        - Default: 1 m²
        - Description: Face area of the filter element.
    - **depth**:
        - Type: Number [Item]
        - Default: 0.1 m
        - Description: Depth of the filter element along the axis of airflow.
    - **density**:
        - Type: Number [Item]
        - Default: 100 kg/m³
        - Description: Density of the filter element. Unit can be changed by right-clicking the component and selecting the desired one.
    - **_data** [required]:
        - Type: Filter efficiency data [List]
        - Default: None
        - Description: A list of filter efficiency data that defines the contaminant removel efficiency by this filter element. Must match the type of filter element.
- **Outputs**:
    - **filter**:
        - Type: Filter element [Item]
        - Description: Filter element with updated settings.

### Filter efficiency data - Constant efficiency
Create filter efficiency data for constant efficiency filter element.
- **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: The contaminant species that is filtered by the filter element.
    - **eff**:
        - Type: Number [Item]
        - Default: 0
        - Description: The efficiency of the filter element to filter the given contaminant species. Value must be between 0 and 1.
- **Outputs**: 
    - **filter data**:
        - Type: Filter efficiency data (constant efficiency) [Item]
        - Description: Filter efficiency data for constant efficiency filter element with updated settings.

### Filter efficiency data - Simple gaseous
Create filter efficiency data for simple gaseous filter element.
- **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: The contaminant species that is filtered by the filter element.
    - **brk thru**: 
        - Type: Number [Item]
        - Default: 0
        - Description: The breakthrough concentration of the contaminant species for the simple gaseous filter element. This value will be used during simulation to report if and when the filter efficiency drops below this value. 
    - **_effs** [required]:
        - Type: Text [List]
        - Default: None
        - Description: A list of texts that represents the loading-efficiency pairs of the filter element. The loading-efficiency pair defines the efficiency of the filter element at the given loading. Each pair must be presented as a text in the format of `loading efficiency`. Loading and efficiency can be separated by a space` `, a comma`,`, a tab`    `, a colon`:`, or a dashline`-`. For example, `0.1 0.9`, `0.1,0.9`, and `0.1:0.9` are all valid loading-efficiency pairs.
- **Outputs**:
    - **filter data**:
        - Type: Filter efficiency data (simple gaseous) [Item]
        - Description: Filter efficiency data for simple gaseous filter element with updated settings.

### Filter efficiency data - Simple particle
Create filter efficiency data for simple particle filter element.
- **Inputs**:
    - **_size_** [required]:
        - Type: Number [Item]
        - Default: None
        - Description: The size of the particle, i.e. mean diameter, that is filtered by the filter element. Unit is in micrometer (μm).
    - **eff**:
        - Type: Number [Item]
        - Default: 0
        - Description: The efficiency of the filter element to filter particles of the given particle size. Value must be between 0 and 1.
- **Outputs**:
    - **filter data**:
        - Type: Filter efficiency data (simple particle) [Item]
        - Description: Filter efficiency data for simple particle filter element with updated settings.
### Filter efficiency data - UVGI
Create filter efficiency data for ultraviolet germicidal irradiation (UVGI) filter element.
- **Inputs**:
    - **survivability**:
        - Type: Number [Item]
        - Default: 0.01
        - Description: Desired survivability of the filter. Value must be between 0 and 1. The single-pass filter inactivation efficiency is equal to: 1 - survivability.
    - **velocity**:
        - Type: Number [Item]
        - Default: 4 m/s
        - Description: Design maximum Velocity expected through the filter flow region. Unit can be changed by right-clicking the component and selecting the desired one.
    - **mass flow**:
        - Type: Number [Item]
        - Default: 0.5 kg/s
        - Description: Design maximum airflow rate expected through the filter flow region (mass flow rate). Unit can be changed by right-clicking the component and selecting the desired one. This will be used along with the **velocity** to calculate the cross-sectional flow area. 
    - **const**:
        - Type: Number [Item]
        - Default: 0.02172
        - Description: Design minimum microorganism-specific rate constant. Unit is in m²/J. This will be used to determine the design UV dose.

    ─── TU ───
    - **C0**:
        - Type: Number [Item]
        - Default: 5.79
        - Description: TU constant C0. Used to determine the output as a percentage of design based on current conditions including flow velocity and temperature. 
    - **C1**:
        - Type: Number [Item]
        - Default: 5.66
        - Description: TU constant C1. Used to determine the output as a percentage of design based on current conditions including flow velocity and temperature. 
    - **C2**:
        - Type: Number [Item]
        - Default: -20.3
        - Description: TU constant C2. Used to determine the output as a percentage of design based on current conditions including flow velocity and temperature. 
    - **C3**:
        - Type: Number [Item]
        - Default: -0.0701
        - Description: TU constant C3. Used to determine the output as a percentage of design based on current conditions including flow velocity and temperature. 
    - **C4**:
        - Type: Number [Item]
        - Default: 4.01
        - Description: TU constant C4. Used to determine the output as a percentage of design based on current conditions including flow velocity and temperature. 

    ─── Age ───
    - **K0**:
        - Type: Number [Item]
        - Default: 0
        - Description: Age constant K0. Used to determine the percentage of output based on the lamp age function. 
    - **K1**:
        - Type: Number [Item]
        - Default: 0
        - Description: Age constant K1. Used to determine the percentage of output based on the lamp age function. 
- **Outputs**:
    - **filter data**:
        - Type: Filter efficiency data (UVGI) [Item]
        - Description: Filter efficiency data for UVGI filter element with updated settings.

## 04-Schedule
![Schedule components](./img/icons-schedule.png)
### Day schedule
Create a day schedule.\
Three types of day schedules are available: dimensionless, temperature, and occupancy. The type of day schedule can be selected by clicking the option buttons on the component. The interpretion shape of the data can be changed by right-clicking the component and selecting the desired one (rectangular or trapezoidal).
- **Inputs**:
    - **_name_** [required]:
        - Type: Text [Item]
        - Default: None
        - Description: A unique name used to identify the day schedule.
    - **desc**:
        - Type: Text [Item]
        - Default: None
        - Description: Detailed description of the day schedule.
    - **_data** [required]:
        - Type: Text [List]
        - Default: None
        - Description: A list of texts that represents the day schedule data. Each text must be presented as a text in the format of `time value`. Time and value can be separated by a space` `, a comma`,`, a tab`    `, or a dashline`-`. For example, `0 0.1`, `0,0.1`, and `0-0.1` are all valid time-value pairs.
            - `time`: needs to be in the format of `H:mm:ss`, `H:mm`, or simply `H`. For example, `6`, `6:00`, and `6:00:00` are all valid time formats for 6AM. A valid day schedule must be started with time 0 (`0`, `0:00`, or `0:00:00`) and ended with time 24 (`24`, `24:00`, or `24:00:00`). 
            - `value`: 
                - Dimensionless day schedule: `value` needs to be a number between 0 and 1.
                - Temperature day schedule: `value` can be any number in °C or °F. Unit can be changed by right-clicking the component and selecting the desired one.
                - Occupancy day schedule: `value` needs to be a string of zone name. Zone names can be visualized by right-clicking the component and clicking the *visualize zone names* item. For example, `6, _zone_1` represents the occupant is in *_zone_1* at 6AM.
 - **Outputs**:
    - **day sched**:
        - Type: Day schedule [Item]
        - Description: A day schedule element with updated settings.

### Week schedule (weekdays & weekends)
Create a week schedule by defining the day schedules for weekdays and weekends.
 - **Inputs**:
    - **_name_** [required]:
        - Type: Text [Item]
        - Default: None
        - Description: A unique name used to identify the week schedule.
    - **desc**:
        - Type: Text [Item]
        - Default: None
        - Description: Detailed description of the week schedule.
    - **_weekdays** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for all weekdays  (Monday through Friday).
    - **_weekends** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for all weekends (Saturday and Sunday) and extra days (day type 8 - 12, defined for days with non-typical schedules, such as holidays).
 - **Outputs**: 
    - **sched**:
        - Type: Week schedule [Item]
        - Description: A week schedule element with updated settings.

### Week schedule (detailed days)
Create a week schedule by defining the day schedules for each day of the week and extra days.
 - **Inputs**:
    - **_name_** [required]:
        - Type: Text [Item]
        - Default: None
        - Description: A unique name used to identify the week schedule.
    - **desc**:
        - Type: Text [Item]
        - Default: None
        - Description: Detailed description of the week schedule.
    - **_Sun** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for Sunday.
    - **_Mon** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for Monday.
    - **_Tue** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for Tuesday.
    - **_Wed** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for Wednesday.
    - **_Thu** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for Thursday.
    - **_Fri** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for Friday.
    - **_Sat** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for Saturday.
    - **_8** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for day type 8.
    - **_9** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for day type 9.
    - **_10** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for day type 10.
    - **_11** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for day type 11.
    - **_12** [required]:
        - Type: Day schedule [Item]
        - Default: None
        - Description: A day schedule element for day type 12.
- **Outputs**:
    - **sched**:
        - Type: Week schedule [Item]
        - Description: A week schedule element with updated settings.

## 05-Species
![Species components](./img/icons-species.png)
### Contaminant/Species
Create a contaminant/species element.\
Contaminant/species concentration unit can be changed by right-clicking the component and selecting the desired one. All contaminants/species are assumed to be trace contaminants/species by default.
 - **Inputs**:
    - **_name_** [required]:
        - Type: Text [Item]
        - Default: None
        - Description: A unique name used to identify the contaminant/species. Must NOT be the same as the species names defined in the system species library, including PM2.5, CO2, CO, NO2, Ozone, SO2, IV1. 
    - **desc**:
        - Type: Text [Item]
        - Default: None
        - Description: Detailed description of the contaminant/species.
    - **molar mass**:
        - Type: Number [Item]
        - Default: 0
        - Description: Molar mass of the contaminant/species in g/mol (kg/kmol).
    - **Dm**:
        - Type: Number [Item]
        - Default: 0.00002 m²/s
        - Description: Diffusion coefficient of the contaminant/species. Unit can be changed by right-clicking the component and selecting the desired one.
    - **Cp**:
        - Type: Number [Item]
        - Default: 1000 J/(kgK)
        - Description: Specific heat capacity of the contaminant/species. Unit can be changed by right-clicking the component and selecting the desired one. Only used when the *variable junction temperatures* method is selected (invalid in the current version of ANT).
    - **mean diam**:
        - Type: Number [Item]
        - Default: 0
        - Description: Mean diameter of the contaminant/species if this species is to be a particulate type species. Unit is micrometer (μm). Used when converting between particle count units and particle mass and volume units, and to determine filter efficiency of *simple particle filter elements* for this size particle.
    - **eff dens**:
        - Type: Number [Item]
        - Default: 0
        - Description: Effective density of the contaminant/species that is considered to be a particulate type species. Unit can be changed by right-clicking the component and selecting the desired one. Only used when converting between particle count units and particle mass and volume units.
    - **decay**: 
        - Type: Number [Item]
        - Default: 0
        - Description: Exponential decay constant based upon the half-life of the radioactive species. Unit is in 1/s.
    - **Kuv**:
        - Type: Number [Item]
        - Default: 0
        - Description: Species-dependent rate constant used to determine UVGI filter effectiveness for a specific contaminant. Unit is m²/J.
    - **default conc**:
        - Type: Number [Item]
        - Default: 0
        - Description: Default concentration of the contaminant/species. Unit can be changed by right-clicking the component and selecting the desired one. This value will be applied as the default initial concentration for each zone. Initial concentration for individual zones can be revised in [Zone](#zone) component per requirements. This value will also be used as the ambient contaminant concentration during simulations when no external ambient contaminant data file (CTM or WPC) is provided.
 - **Outputs**:
    - **species**:
        - Type: Species [Item]
        - Description: A contaminant/species element with updated settings.

### Edit contaminant/species
Edit existing contaminant/species.
Contaminant/species settings will show up when the component is connected to an existing contaminant/species.
 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A contaminant/species element to be edited.
    - *species settings*: Same as the inputs of [Contaminant/Species](#contaminantspecies).
 - **Outputs**:
    - **species**:
        - Type: Species [Item]
        - Description: A contaminant/species element with updated settings.

### Reactant-Product (R-P) pair
Create a reactant-product (R-P) pair to create a kinetic reaction between two species.
 - **Inputs**:
    - **_reac** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: The reactant contaminant/species element in the R-P pair.
    - **_prod** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: The product contaminant/species element in the R-P pair.
    - **_coef** [required]:
        - Type: Number [Item]
        - Default: None
        - Description: The reaction coefficiency from the reactant to the product.
 - **Outputs**:
    - **R-P pair**:
        - Type: R-P pair [Item]
        - Description: A reactant-product (R-P) pair element with updated settings.

### Reaction
Create a kinetic reaction between contaminants/species.
 - **Inputs**: 
    - **_name** [required]:
        - Type: Text [Item]
        - Default: None
        - Description: A unique name used to identify the reaction.
    - **desc**:
        - Type: Text [Item]
        - Default: None
        - Description: Detailed description of the reaction.
    - **_R-P pairs** [required]:
        - Type: R-P pair [List]
        - Default: None
        - Description: A list of R-P pairs that define the reaction.
 - **Outputs**:
    - **reaction**:
        - Type: Reaction [Item]
        - Description: A kinetic reaction element with updated settings.

### Particle distribution calculator
Calculate particle distribution and generate a species library (LB0) of particles with desired distributions and a CTM file of particles.\
The original calculator is a web application on CONTAM website: [Particle Distribution Calculator](https://www.nist.gov/el/energy-and-environment-division-73200/nist-multizone-modeling/software/contam-particle). Number of modes can be selected by clicking the *number of modes* button on the component. Distribution type can be selected by clicking the *distribution type* button on the component. Click *Calculate* button on the component to generate desired files (LB0 and CTM). 
 - **Inputs**:
    - **bins**: 
        - Type: Integer [Item]
        - Default: 20
        - Description: Number of bins to divide the particle size range into.
    - **min diam**:
        - Type: Number [Item]
        - Default: 0.001 µm
        - Description: Smallest particle diameter. Unit is in micrometer (µm).
    - **max diam**:
        - Type: Number [Item]
        - Default: 10 µm
        - Description: Largest particle diameter. Unit is in micrometer (µm).
    - **air dens**:
        - Type: Number [Item]
        - Default: 1.2041 kg/m³
        - Description: Air density. Unit is in kg/m³.
    - **pm dens**:
        - Type: Number [Item]
        - Default: 1200 kg/m³
        - Description: Particle density. Unit is in kg/m³.
    - **user-defined bins**:
        - Type: Text [List]
        - Default: None
        - Description: A list of texts that represents the user-defined bins. If this input is not empty, the inputs of **bins**, **min diam**, **max diam**, and *mode settings* will be ignored. The size of list represents the number of bins. Each text must be presented as a text in the format of `diameter[µm]  concentration[#/cm³] (concentration[#/cm³] concentration[#/cm³])` (mode 1: `concentration[#/cm³]` × 1; mode 2: `concentration[#/cm³]` × 2; mode 3: `concentration[#/cm³]` × 3). Variables can be separated by a space` `, a comma`,`, a tab`    `, or a dashline`-`. For example, `0.1 100`, `0.1,100`, and `0.1-100` are all valid user-defined bins. The selection of *number of modes* needs to match the size of each text of user-defined bin (e.g., if mode = 3, the bin text should be `diameter, conc 1, conc 2, conc 3`). For example, `0.1, 100, 200, 300` represents the particle distribution has 3 modes, the concentration of particles in 0.1 µm diameter is 100 #/cm³, 200 #/cm³, and 300 #/cm³ for mode 1, 2, and 3, respectively. 
        
    ── mode 1 ──
    - **µ_log** (or **µ** for normal distribution):
        - Type: Number [Item]
        - Default: 0.014 µm
        - Description: Mean (µ) of particle diameters. Unit is in micrometer (µm).
    - **σ_log** (or **σ** for normal distribution):
        - Type: Number [Item]
        - Default: 1.8 µm
        - Description: Standard deviation (σ) of particle diameters. Unit is in micrometer (µm).
    - **total N**:
        - Type: Number [Item]
        - Default: 10600 #/cm³
        - Description: Total particle number concentration. Unit is in #/cm³.

    ── mode 2 ──
    - **µ_log** (or **µ** for normal distribution):
        - Type: Number [Item]
        - Default: 0.054 µm
        - Description: Mean (µ) of particle diameters. Unit is in micrometer (µm).
    - **σ_log** (or **σ** for normal distribution):
        - Type: Number [Item]
        - Default: 2.16 µm
        - Description: Standard deviation (σ) of particle diameters. Unit is in micrometer (µm).
    - **total N**:
        - Type: Number [Item]
        - Default: 32000 #/cm³
        - Description: Total particle number concentration. Unit is in #/cm³.

    ── mode 3 ──
    - **µ_log** (or **µ** for normal distribution):
        - Type: Number [Item]
        - Default: 0.86 µm
        - Description: Mean (µ) of particle diameters. Unit is in micrometer (µm).
    - **σ_log** (or **σ** for normal distribution):
        - Type: Number [Item]
        - Default: 2.21 µm
        - Description: Standard deviation (σ) of particle diameters. Unit is in micrometer (µm).
    - **total N**:
        - Type: Number [Item]
        - Default: 5.4 #/cm³
        - Description: Total particle number concentration. Unit is in #/cm³.
 - **Outputs**:
    - **pm LB0**:
        - Type: Text [Item]
        - Description: A relative file path of the generated species library file (LB0).
    - **pm CTM**:
        - Type: Text [Item]
        - Description: A relative file path of the generated CTM file of particles.

## 06-Source/Sink
![Source components](./img/icons-src.png)
### Source - Constant coefficiency
Create a contaminant/species source with a constant generation/removal rate.\
A constant coefficiency source with a default name of `_SrcCsCcf_?` is created, where `?` is the number of constant coefficiency source created.\
The generation rate is calculated by:
$$S_\alpha(t) = mult \cdot ctrl \cdot G - mult \cdot ctrl \cdot R \cdot C_\alpha(t)$$
$S_\alpha(t)$ = Source strength at time $t$ [$kg_\alpha /s$]\
$G$ = Generation rate [$kg_\alpha /s$]\
$R$ = Effective removal rate [$kg_{air} /s$]\
Determine by multiplying the first-order removal rate of the contaminant [$1/s$] by air density [$kg_{air}/m^3$] and volume [$m^3$] of the zone in which the source is to be located.\
$C_\alpha(t)$ = Concentration of contaminant $\alpha$ in the zone at time $t$ [$kg_\alpha / kg_{air}$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the source.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the source.
    - **gen rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The rate at which the contaminant/species is introduced into the zone ($G$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **remov rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The rate at which the contaminant/species is removed from the zone ($R$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the source strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **src**:
        - Type: Source [Item]
        - Description: A source element with updated settings.
        
### Source - Burst mass
Create a contaminant/species source, which releases instantaneous mass of contaminant into the zone.\
A burst mass source with a default name of `_SrcCsBrs_?` is created, where `?` is the number of burst mass source created.
 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the source.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the source.
    - **mass added**:
        - Type: Number [Item]
        - Default: 0
        - Description: A value for the instantaneous mass release into the zone. The addition of mass is controlled by a schedule; when the schedule changes from zero to a non-zero value, mass is added to the zone in one simulation time setp. The amount of mass added is equal to the mass added to zone value times the schedule value. A single schedule can initiate several events at different times. Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the source strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **src**:
        - Type: Source [Item]
        - Description: A source element with updated settings.

### Source - Cutoff concentration
Create a contaminant/species source with a reducing emission as the concentration within the zone approaching a specified cutoff concentration.\
A cutoff concentration source with a default name of `_SrcCsCut_?` is created, where `?` is the number of cutoff concentration source created.\
This model may be appropriate for some sources of volatile organic compounds (VOCs).\
The generation rate is calculated by:
$$S_\alpha(t) = mult \cdot ctrl \cdot G \cdot (1 - C_\alpha(t)/C_{cut})$$
$S_\alpha(t)$ = Source strength at time $t$ [$kg_\alpha /s$]\
$G$ = Generation rate [$kg_\alpha /s$]\
$C_\alpha(t)$ = Concentration of contaminant $\alpha$ in the zone at time $t$ [$kg_\alpha / kg_{air}$]\
$C_{cut}$ = Cutoff concentration at which emission ceases [$kg_\alpha / kg_{air}$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the source.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the source.
    - **gen rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The rate at which the contaminant/species is introduced into the zone as a function of concentration ($G$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **cutoff conc**:
        - Type: Number [Item]
        - Default: 0
        - Description: The concentration at which the source emission is reduced to zero ($C_{cut}$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the source strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.

### Source - Decaying source
Create a contaminant/species source with exponentially decaying with time according to a user-defined time constant.\
A decaying source with a default name of `_SrcCsEds_?` is created, where `?` is the number of decaying source created.\
This model may be appropriate for some sources of volatile organic compounds (VOCs).\
The generation rate is calculated by:
$$S_\alpha(t) = mult \cdot ctrl \cdot G_0 \cdot e^{-\Delta t/\tau _c}$$
$S_\alpha(t)$ = Source strength at time $t$ [$kg_\alpha /s$]\
$G_0$ = Initial generation rate [$kg_\alpha /s$]\
$\Delta t$ = Elapsed time since the start of emission [$s$]\
$\tau _c$ = Time constant [$s$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the source.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the source.
    - **gen rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: Initial generation rate ($G_0$). Contaminant generation is controlled by a schedule. Contaminant generation begins when the schedule changes from a zero to a non-zero value (between 0 and 1). The initial generation is equal to the schedule value times the initial generation rate. A single scheudle may be used to initiate several emissions at different times. Unit can be changed by right-clicking the component and selecting the desired one.
    - **decay const**:
        - Type: Number [Item]
        - Default: 0
        - Description: The time at which the generation rate decays by 0.368 of the original rate ($\tau_c$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the source strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **src**:
        - Type: Source [Item]
        - Description: A source element with updated settings.

### Source - Pressure driven
Create a contaminant/species source that is governed by the pressure differences between interior and exterior zones, e.g., radon or soil gas entry.\
A pressure driven source with a default name of `_SrcCsPrs_?` is created, where `?` is the number of pressure driven source created.\
The generation rate is calculated by:
$$S_\alpha(t) = mult \cdot ctrl \cdot G (\Delta P)^n$$
$S_\alpha(t)$ = Source strength at time $t$ [$kg_\alpha /s$]\
$G$ = Generation rate coefficient [$kg_\alpha /Pa^n \cdot s$]\
$\Delta P$ = Pressure difference (ambient pressure - zone pressure) [$Pa$]\
$n$ = Pressure exponent [no units]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 
 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the source.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the source.
    - **gen rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The rate at which the contaminant/species is introduced into the zone as a function of pressure difference ($G$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **press expon**:
        - Type: Number [Item]
        - Default: 0
        - Description: Pressure exponent ($n$). It describe the dependence on pressure of the contaminant entry.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the source strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **src**:
        - Type: Source [Item]
        - Description: A source element with updated settings.

### Source - Peak model (NRCC)
Create a contaminant/species source with a constant emission rate until specified time at which the peak emission rate is obtained after which time the emission rate decays according to the peak model relationship.\
A peak model source with a default name of `_SrcCsPkm_?` is created, where `?` is the number of peak model source created.\
This source is provided for compatibility with the Material Emissions Database developed by the National Research Council Canada (NRCC) that is largely based on material emission test chamber data.\
The generation rate is calculated by:
$$S_\alpha(t) = mult \cdot a \cdot e ^ {-0.5[(1/b) \cdot ln(t/t_p)]^2}$$
$S_\alpha(t)$ = Source strength at time $t$ [$kg_\alpha /s$]. It will increase up until time $t_p$ at which point it will begin to decay.\
$a$, $b$, and $t_p$ = Empirical coefficients typically determined from emission chamber measurements.\
$mult$ = Source/sink multiplier [no units]

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the source.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the source.
    - **peak gen**:
        - Type: Number [Item]
        - Default: 0
        - Description: Peak emission rate ($a$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **fit param**:
        - Type: Number [Item]
        - Default: 0
        - Description: The fitting parameter ($b$).
    - **time**:
        - Type: Number [Item]
        - Default: 0
        - Description: The time at which the peak emission rate is obtained ($t_p$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the source strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **src**:
        - Type: Source [Item]
        - Description: A source element with updated settings.

### Source - Power-law model (NRCC)
Create a contaminant/species source with a constant emission rate until specified time after which time the emission rate decays according to the power law relationship.\
A power-law model source with a default name of `_SrcCsPlm_?` is created, where `?` is the number of power-law model source created.\
This source is provided for compatibility with the Material Emissions Database developed by the National Research Council Canada (NRCC) that is largely based on material emission test chamber data.\
The generation rate is calculated by:\
For $t	\leqslant t_p$ the initial emission rate is:
$$S_{\alpha,o}(t) = mult \cdot a \cdot t_p^{-b}$$
and for $t > t_p$:
$$S_\alpha(t) = mult \cdot a \cdot t^{-b}$$
$S_{\alpha,o}(t)$ = Intitial source strength before time $t_p$ [$kg_\alpha /s$].\
$S_\alpha(t)$ = Source strength at time $t$ [$kg_\alpha /s$].\
$a$ and $b$ = Empirical coefficients typically determined from emission chamber measurements.\
$t_p$ = Power-law model time [$s$].\
$mult$ = Source/sink multiplier [no units]

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the source.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the source.
    - **init gen**:
        - Type: Number [Item]
        - Default: 0
        - Description: Initial emission rate ($a$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **expon**:
        - Type: Number [Item]
        - Default: 0
        - Description: The power law exponent ($b$).
    - **time**:
        - Type: Number [Item]
        - Default: 0
        - Description: Power-law model time ($t_p$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the source strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **src**:
        - Type: Source [Item]
        - Description: A source element with updated settings.

![Sink components](./img/icons-sink.png)
### Sink - Constant coefficiency
Create a contaminant/species sink with a constant generation/removal rate.\
A constant coefficiency sink with a default name of `_SinkCsCcf_?` is created, where `?` is the number of constant coefficiency sink created.\
The removal rate is calculated by:
$$R_\alpha(t) = mult \cdot ctrl \cdot G - mult \cdot ctrl \cdot R \cdot C_\alpha(t)$$
$R_\alpha(t)$ = Sink strength at time $t$ [$kg_\alpha /s$]\
$G$ = Generation rate [$kg_\alpha /s$]\
$R$ = Effective removal rate [$kg_{air} /s$]\
Determine by multiplying the first-order removal rate of the contaminant [$1/s$] by air density [$kg_{air}/m^3$] and volume [$m^3$] of the zone in which the source is to be located.\
$C_\alpha(t)$ = Concentration of contaminant $\alpha$ in the zone at time $t$ [$kg_\alpha / kg_{air}$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the sink.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the sink.
    - **gen rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The rate at which the contaminant/species is introduced into the zone ($G$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **remov rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The rate at which the contaminant/species is removed from the zone ($R$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the sink strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **sink**:
        - Type: Sink [Item]
        - Description: A sink element with updated settings.
        
### Sink - Deposition rate
Create a contaminant/species sink with a deposition rate.\
This model is dependent on the mass of air in the zone in which the source/sink is located and will behave similar to that of a kinetic reaction having the same reaction rate.\
A deposition rate sink with a default name of `_SinkCsDrs_?` is created, where `?` is the number of deposition rate sink created.\
The removal rate is calculated by:
$$R_\alpha(t) = mult \cdot ctrl \cdot k_d \cdot V_z \cdot \rho_{air}(t) \cdot C_\alpha(t)$$
$R_\alpha(t)$ = Sink strength (removal rate by deposition) at time $t$ [$kg_\alpha /s$]\
$V_z$ = Zone volume [$m^3$]\
$k_d$ = Deposition rate [$1/s$]\
$\rho_{air}(t)$ = Density of air in the sink zone at time $t$ [$kg_{air}/m^3$]\
$C_\alpha(t)$ = Concentration of contaminant $\alpha$ in the zone at time $t$ [$kg_\alpha / kg_{air}$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the sink.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the sink.
    - **dep rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The deposition rate of the contaminant/species ($k_d$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the sink strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **sink**:
        - Type: Sink [Item]
        - Description: A sink element with updated settings.

### Sink - Deposition velocity
Create a contaminant/species sink with a deposition velocity.\
This model differs from the [Deposition Rate Sink Model](#sink---deposition-rate) in that it is not dependent on the mass of air in the zone in which it is located.\
A deposition velocity sink with a default name of `_SinkCsDvs_?` is created, where `?` is the number of deposition velocity sink created.\
The removal rate is calculated by:
$$R_\alpha(t) = mult \cdot ctrl \cdot v_d \cdot A_s \cdot \rho_{air}(t) \cdot C_\alpha(t)$$
$R_\alpha(t)$ = Sink strength (removal rate by deposition) at time $t$ [$kg_\alpha /s$]\
$v_d$ = Deposition velocity [$m/s$]\
$A_s$ = Deposition surface area [$m^2$]\
$\rho_{air}(t)$ = Density of air in the sink zone at time $t$ [$kg_{air}/m^3$]\
$C_\alpha(t)$ = Concentration of contaminant $\alpha$ in the zone at time $t$ [$kg_\alpha / kg_{air}$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the sink.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the sink.
    - **dep vel**:
        - Type: Number [Item]
        - Default: 0
        - Description: The deposition velocity of the contaminant/species ($v_d$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **dep area**:
        - Type: Number [Item]
        - Default: 0
        - Description: The deposition surface area of the contaminant/species ($A_s$).
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the sink strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **sink**:
        - Type: Sink [Item]
        - Description: A sink element with updated settings.

### Sink - Deposition with resuspension emission
Create a contaminant/species that provides contaminant removal from the zone air by deposition to a surface with source emission via resuspension from the surface.\
The total deposition surface area remains constant, but the resuspension rate and area can be modified by a schedule or control value.\
A deposition with resuspension emission sink with a default name of `_SinkCsDvr_?` is created, where `?` is the number of deposition with resuspension emission sink created.\
The removal rate is calculated by:
$$R_\alpha(t) = v_d \cdot (mult \cdot A_s) \cdot \rho_{air}(t) \cdot C_\alpha(t)$$
The resuspension rate is calculated by:
$$S_\alpha(t) = (ctrl \cdot r \cdot A_r) \cdot L_\alpha(t)$$
$R_\alpha(t)$ = Sink strength (removal rate by deposition) of contaminant $\alpha$ from zone air to surface at time $t$ [$kg_\alpha /s$]\
$v_d$ = Deposition velocity [$m/s$]\
$A_s$ = Deposition surface area [$m^2$]\
$\rho_{air}(t)$ = Density of air in the sink zone at time $t$ [$kg_{air}/m^3$]\
$C_\alpha(t)$ = Concentration of contaminant $\alpha$ in the zone at time $t$ [$kg_\alpha / kg_{air}$]\
$S_\alpha(t)$ = Source strength (resuspension) of contaminant $\alpha$ from surface to zone air at time $t$ [$kg_\alpha /s$]\
$r$ = Resuspension rate [$1/s$]\
$A_r$ = Resuspension surface area [$m^2$]\
$L_\alpha(t)$ = Concentration of contaminant $\alpha$ on the deposition surface at time $t$ [$kg_\alpha / m^2$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units] 

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the sink.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the sink.
    - **dep vel**:
        - Type: Number [Item]
        - Default: 0
        - Description: The deposition velocity of the contaminant/species ($v_d$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **dep area**:
        - Type: Number [Item]
        - Default: 0
        - Description: The deposition surface area of the contaminant/species ($A_s$).
    - **resusp rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The resuspension rate of the contaminant/species ($r$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **resusp area**:
        - Type: Number [Item]
        - Default: 0
        - Description: The resuspension surface area of the contaminant/species ($A_r$). The resuspension surface area may be different from that of the deposition surface area, e.g., a shoe impacting the deposition surface. Once associated with a source/sink, the resuspension rate can be varied using a schedule or control also associated with the source/sink.  
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the sink strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
    - **sink**:
        - Type: Sink [Item]
        - Description: A sink element with updated settings.

### Sink - Boundary layer diffusion model
Create a contaminant/species sink with a boundary layer diffusion model.\
The boundary layer diffusion controlled reversible sink/source model with a linear sorption isotherm. The boundary layer refers to the region above the surface of a material through which a concentration gradient exists between the near-surface concentration and the air-phase concentration.\
A boundary layer diffusion sink with a default name of `_SinkCsBls_?` is created, where `?` is the number of boundary layer diffusion sink created.\
The removal rate is calculated by:
$$R_\alpha(t) = ctrl \cdot h \cdot d \cdot A_s \cdot [C_\alpha(t) - C_s(t) / k]$$
$R_\alpha(t)$ = Sink strength of contaminant $\alpha$ from zone air transferred onto a surface at time $t$ [$kg_\alpha /s$]\
$ctrl$ = Schedule or control value [no units]\
$h$ = Film mass transfer coefficient over the sink [$m/s$]\
$d$ = Film density of air [$kg_{air}/m^3$]\
$A_s$ = Surface area of the adsorbent [$m^2$]\
$C_\alpha(t)$ = Concentration of contaminant $\alpha$ in the zone air at time $t$ [$kg_\alpha / kg_{air}$]\
$C_s(t)$ = Concentration of contaminant $\alpha$ in the adsorbent at time $t$\
$k$ = Henry adsorption constant or the partition coefficient [$kg_{air}/kg_{surface}$]\
$mult$ = Source/sink multiplier [no units]\
$ctrl$ = Schedule or control value [no units]

 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is associated with the sink.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the sink.
    - **film trf coef**:
        - Type: Number [Item]
        - Default: 0
        - Description: The film mass transfer coefficient over the sink ($h$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **film dens**:
        - Type: Number [Item]
        - Default: 0
        - Description: The film density of air ($d$). Unit can be changed by right-clicking the component and selecting the desired one.
    - **srf mass**:
        - Type: Number [Item]
        - Default: 0
        - Description: Surface mass of absorbed contaminant/species. unit can be changed by right-clicking the component and selecting the desired one.
    - **partition coef**:
        - Type: Number [Item]
        - Default: 0
        - Description: The Henry adsorption constant or the partition coefficient ($k$).
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the sink strength will be multiplied during simulation. With this feature you could define a source/sink element having a source strength per unit area then use the multiplier as the area of the zone for each source/sink that uses the per unit area source/sink element.
 - **Outputs**:
        - **sink**:
            - Type: Sink [Item]
            - Description: A sink element with updated settings.

## 07-Occupancy
![Occupancy components](./img/icons-occupancy.png)
### Occupant
Create an occupant element.
 - **Inputs**:
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: The multiplier to simulate multiple occupants with the same characteristics. 
    - **occ sched**: 
        - Type: Week schedule (occupancy) [Item]
        - Default: None
        - Description: Week schedule that controls the location of this occupant.
    - **occ ctm gens**:
        - Type: Occupant contaminant generation [List]
        - Default: None
        - Description: A list of occupant contaminant generation elements which control the contaminants generated by the occupant.
 - **Outputs**:
    - **occ**:
        - Type: Occupant [Item]
        - Description: An occupant element with updated settings.

### Occupant contaminant generation
Create an occupant contaminant generation element.
 - **Inputs**:
    - **_species** [required]:
        - Type: Species [Item]
        - Default: None
        - Description: A species element that is generated by the occupant.
    - **gen rate**:
        - Type: Number [Item]
        - Default: 0
        - Description: The maximum rate of the contaminant to generate per time step. Unit can be changed by right-clicking the component and selecting the desired one.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the contaminant generation.
 - **Outputs**:
    - **occ ctm gen**:
        - Type: Occupant contaminant generation [Item]
        - Description: An occupant contaminant generation element with updated settings.

### Occupancy DALY factor library
Select disability-adjusted life years ([DALYs](https://www.who.int/data/gho/indicator-metadata-registry/imr-details/158)) factors (∂DALY/∂intake for cancer and non-cancer effects) from a library of DALYs factors for different species. The library is established using the data in [this paper](https://doi.org/10.1897/2004-007R.1).\
Factors can be selected by clicking the *DALY Factor* button on the component and selecting the desired species. ∂DALY/∂intake for cancer effects and ∂DALY/∂intake for non-cancer effects are exported as outputs. 

![DALY factor library](/tutorials/img/daly-factor-2.png)
Factors can be searched by species name or CAS number in the *Search* bar. After selecting the desired species, click *Update Selection* to close the window and export the factors as outputs.

 - **Outputs**:
    - **f_cancer**:
        - Type: Number [Item]
        - Description: ∂DALY/∂intake factor for cancer effects of the selected species.
    - **f_non-cancer**:
        - Type: Number [Item]
        - Description: ∂DALY/∂intake factor for non-cancer effects of the selected species.

### Occupancy DALY factors
Create an occupant disability-adjusted life years ([DALYs](https://www.who.int/data/gho/indicator-metadata-registry/imr-details/158)) factors element. 
 - **Inputs**:
    - **_f_cancer** [required]:
        - Type: Number [Item]
        - Default: 0
        - Description: ∂DALY/∂intake factor for cancer effects of the selected species. The factor can be acquired from the [Occupancy DALY factor library](#occupancy-daly-factor-library) component.
    - **_f_non-cancer** [required]:
        - Type: Number [Item]
        - Default: 0
        - Description: ∂DALY/∂intake factor for non-cancer effects of the selected species. The factor can be acquired from the [Occupancy DALY factor library](#occupancy-daly-factor-library) component.
    - **ADAF**:
        - Type: Number [Item]
        - Default: 1.6
        - Description: Cancer age-dependent adjustment factor (ADAF) for the selected species. Typically, ADAF = 10 for groups with age < 2 years old, ADAF = 3 for groups with age 2 - 16 years old, ADAF = 1 for groups with age > 16 years old. In average, ADAF ≈ 1.6 for overall population.
 - **Outputs**:
    - **DALY factors**:
        - Type: DALY factors [Item]
        - Description: An element of occupant DALY factors with updated settings.

### Occupancy exposure
Read simulation results and calculate occupancy exposure and health impacts to specified contaminants over a certain period of time.\
The exposure is calculated based on contaminant concentrations in zones and occupant schedule, breathing rate, and filtration by personal protective equipment (PPE) such as masks. The health impacts include 1) disability-adjusted life years ([DALYs](https://www.who.int/data/gho/indicator-metadata-registry/imr-details/158)) for general contaminants and 2) infection risk for infectious respiratory diseases. The calculated metrics can be switched by clicking the metrics option button on the component.\
Mortality does not give a complete picture of the burden of disease borne by individuals in different populations. The overall burden of disease is assessed using the disability-adjusted life year (DALY), a time-based measure that combines years of life lost due to premature mortality (YLLs) and years of life lost due to time lived in states of less than full health, or years of healthy life lost due to disability (YLDs). One DALY represents the loss of the equivalent of one year of full health. For health issues related to the air contaminants, DALYs can be calculated based on the inhalation exposure and DALYs factors through the Intake-DALY (ID) approach. DALYs are usually expressed per 100,000 population per year. Thus, it is suggested to evaluate DALYs for full-year simulations in ANT. For those air contaminants with sufficient epidemiological data (e.g., PM2.5, CO, NO2, Ozone, and SO2), the Intake-Incidence-DALY (IND) approach can be applied. However, it is not yet ready in the current version of ANT. The calculation of DALYs through the ID approach can be represented as:
$$DALYs = \frac {\partial DALY}{\partial intake} \times intake$$
$$\frac {\partial DALY}{\partial intake} = (\frac {\partial DALY}{\partial intake})_{cancer} \times ADAF + (\frac {\partial DALY}{\partial intake})_{non-cancer}$$
$$intake = \int_{t1}^{t2} (1 - \eta _{PPE} \cdot ctrl_{PPE} ) \cdot C_{\alpha}(t) \cdot Q_{intake} \cdot ctrl_{intake} \cdot dt$$
$\frac {\partial DALY}{\partial intake}$ = DALY factor per intake.\
$(\frac {\partial DALY}{\partial intake})_{cancer}$ and $(\frac {\partial DALY}{\partial intake})_{non-cancer}$ = DALY factors for cancer and non-cancer effects, respectively.\
$ADAF$ = Cancer age-dependent adjustment factor (ADAF).\
$intake$ = Intake of the target contaminant.\
$\eta _{PPE}$ = Filtration efficiency of PPE [dimensionless].\
$ctrl_{PPE}$ = Schedule or control PPE filtration efficiency [no units].\
$C_{\alpha}(t)$ = Contaminant concentration of contaminant $\alpha$ in the zone where the susceptible occupant is located at time $t$.\
$Q_{intake}$ = Breathing/inhalation rate.\
$ctrl_{intake}$ = Schedule or control breathing/inhalation rate [no units].\
$dt$ = Time step.\
$t1$ and $t2$ = Start and end time of the exposure period. For DALYs calculation, $t1$ is the input start time and $t2$ is the input end time. It means that accumulated exposure during the whole exposure period is calculated.

Infection risk for infectious respiratory diseases is calculated based on the exposure through the [Wells-Riley model](https://doi.org/10.1111/j.1600-0668.2009.00621.x), which depends on the inhaled dose of infectious pathogen. In ANT, contaminants with number concentrations are required for infection risk estimation, i.e., #/m³, #/cm³, #/ft³, or #/lb. It requires particulate types of contaminants to be defined as the infetious pathogen. The infection risk is calculated based on the "quanta" of the infectious pathogen inhaled, which is the number of infectious pathogen required to cause infection in 63% of susceptible occupants. Unit "quanta" cannot be directly defined in CONTAM or ANT. Therefore, the number concentrations (#/m³, #/cm³, #/ft³, or #/lb) are used as the alternatives for "quanta" units of the infectious pathegon in ANT. In this case, #/m³ equals quanta/m³, for example. Other units related to the number concentrations (#) are changed accordingly. For example, the contaminant generation rate #/h is equivalent to quanta/h. Quanta generation rates of some typical respiratory diseases (including COVID-19) can be found in a [research paper](https://doi.org/10.1016/j.buildenv.2021.107926) and a [review paper](https://doi.org/10.1080/23744731.2021.1977693). Typically, a generation rate of 100 quanta/h is very commonly applied for SARS-CoV-2 (particularly the Omicron variant). The calculation of infection risk (the Wells-Riley model) can be represented as:

$$P = \frac{N_C}{N_S} = 1 - e^{-intake}$$
$$intake = \int_{t1}^{t2} (1 - \eta _{PPE} \cdot ctrl_{PPE} ) \cdot C_{\alpha}(t) \cdot Q_{intake} \cdot ctrl_{intake} \cdot dt$$

$P$ = Infection probability [dimensionless].\
$N_C$ = Number of occupants infected [person(s)].\
$N_S$ = Number of susceptible occupants [person(s)].\
$intake$ = Inhaled dose of infectious pathogen [quanta].\
$\eta _{PPE}$ = Filtration efficiency of PPE [dimensionless].\
$ctrl_{PPE}$ = Schedule or control PPE filtration efficiency [no units].\
$C_{\alpha}(t)$ = Contaminant concentration of contaminant $\alpha$ in the zone where the susceptible occupant is located at time $t$ [quanta/m³, quanta/cm³, quanta/ft³, or quanta/lb].\
$Q_{intake}$ = Breathing/inhalation rate.\
$ctrl_{intake}$ = Schedule or control breathing/inhalation rate [no units].\
$dt$ = Time step.\
$t1$ and $t2$ = Start and end time of the exposure period. For infection risk calculation, there are three modes of exposure duration can be selected, i.e., 1) daily, 2) receding, and 3) accumulation. ***Daily*** exposure duration means the inhalation exposure and infection risk are calculated seperately for each single day (i.e., $t1$ = the start of a day, `00:00:00`; $t2$ = the end of the day, `24:00:00`). The exposure inhaled by the occupant during the previous day is not considered by the exposure of the next day. ***Receding*** exposure duration means the inhalation exposure and infection risk are only calculated for the moments within the current receding period (i.e., $t1$ = the start moment of the receding period, $t-\Delta t$ ($t$ is the current moment at time $t$, $\Delta t$ is the length of the receding period); $t2$ = the current moment at time $t$). Any exposure during the moments before the receding period is not considered. ***Accumulation*** exposure duration means the inhalation exposure and infection risk are calculated for the whole exposure period (i.e., $t1$ = the start moment of the exposure period; $t2$ = the end moment of the exposure period).

![occupant exposure modes](./img/occ_exposure_results.png)

 - **Inputs**:
    - **_results** [required]:
        - Type: Text [Item]
        - Default: None
        - Description: A text contains the relative file path of the simulation result (.sim and .sqlite3 files are required).
    - **date & time**:
        - Type: Text [List]
        - Default: None
        - Description: A list of texts that represents the start and end date/time of the desired period. Each text must be presented as a text in the format of `date time`. Date and time can be separated by a space` `, a comma`,`, or a tab`    `. For example, `1/1 12` and `1/1,12` are both valid date-time pairs. `date time` needs to be in the format of `M/d H:mm:ss`, `M/d H:mm`, or simply `M/d H`. For example, `1/1 12`, `1/1 12:00`, and `1/1 12:00:00` are all valid formats for January 1st 12PM. If no input is provided, the full period in the result will be used.
    - **_species** [required]:
        - Type: Text / Species [Item]
        - Default: None
        - Description: A species element or a text of the species name that will be calculated for the exposure. The species needs to be defined and calculated in the simulation.
    - **_occ sched** [required]:
        - Type: Week schedule (occupancy) [Item]
        - Default: None
        - Description: A week schedule element that is used to describe the schedule of the susceptible occupant (the occupant being exposed to the contaminant).
    - **breath rate**: 
        - Type: Number [Item]
        - Default: 0.5
        - Description: The breathing rate of the occupant. Unit can be changed by right-clicking the component and selecting the desired one.
    - **breath sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: A week schedule element that controls the breathing rate.
    - **PPE eff**:
        - Type: Number [Item]
        - Default: 0
        - Description: The filtration efficiency of the personal protective equipment (PPE) such as masks. 
    - **PPE sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: A week schedule element that controls the use of PPE (the filtration efficiency of PPE).
    - **DALY factors**:
        - Type: DALY factors [Item]
        - Default: None
        - Description: DALY factors that are used to calculate DALYs.
 - **Outputs**:
    - **exposure** / **DALYs** / **infect risk**:
        - Type: Number [Tree]
        - Description: A tree (list or item) of numbers that represents the exposure to a specific contaminant ($intake$), the DALYs for this contaminant ($DALYs$), or the infection risk of an infectious pathogen ($P$) of the susceptible occupant (the occupant being exposed to the contaminant). For exposure metrics mode, when the input *DALY factors* has a valid input, DALYs are calculated. Otherwise, exposure ($intake$ without calculating DALYs) is calculated and exported. The exposure and DALYs metrics are expressed by an item of a final number. The infection risk is expressed by a list of numbers representing the infection probability over time (for daily or receding durations), or an item of accumulated infection risk. The unit of the exposure is expressed in $kg_{\alpha}$. The unit of DALYs is DALYs per 100,000 population per exposure duration. The unit of infection risk is dimensionless.
    - **conc seq**:
        - Type: Number [Tree]
        - Description: A tree (list) element of contaminant concentration sequence that represents the time-sequence concentration level the occupant is exposed to (depending on the occupant's schedule of locations).
    - **breath seq**:
        - Type: Number [Tree]
        - Description: A tree (list) element of breathing rate sequence, which depends on the occupant's breathing rate schedule.
    - **PPE eff seq**:
        - Type: Number [Tree]
        - Description: A tree (list) element of personal protective equipment (PPE) filtration efficiency sequence.
    - **loc seq**:
        - Type: Text [Tree]
        - Description: A tree (list) element of occupant location sequence (zone names), which depends on the occupant's schedule of locations.
    - **time seq**:
        - Type: Text [Tree]
        - Description: A tree (list) element of time sequence (text). The text of each item is in the format of `date time type`, more specifically `M/d HH:mm:ss ddd`, e.g., `1/1 12:00:00 Fri`. The time sequence depends on the input start and end *data & time* and the time sequence in the result files.

## 08-Airflow
![Airflow path components](./img/icons-airflow-path.png)
### Airflow path
Create an airflow path element.
 - **Inputs**:
    - **_afe** [required]:
        - Type: Airflow element [Item]
        - Default: None
        - Description: An airflow element that is associated with the airflow path.
    - **elev**:
        - Type: Number [Item]
        - Default: None
        - Description: The elevation of the airflow path. If not specified, the opening elevation will be used if this airflow path is associated with an opening airflow element, or a default elevation will be applied based on the wall geometry information if this airflow path is associated with a wall airflow path setting.
    - **mult**:
        - Type: Number [Item]
        - Default: 1
        - Description: Multiplier. A constant value by which the airflow path will be multiplied during simulation. The multiplier is a constant value allowing you, for example, to use a flow element description of one window and a multiplier of 5 to describe 5 windows in a particular wall. If you have specified a "leakage area" flow element, complete the description here by giving the reference area, length, or number of units. 
    - **wpp**:
        - Type: Number / Wind pressure profile [Item]
        - Default: None
        - Description: The wind pressure profile (WPP) that applies to the path. It is only valid for exterior surfaces (invalid for interior surfaces). If not specified, no WPP will be applied to the path. The WPP can be defined by a number or a wind pressure profile element. If a number is given, a constant pressure [Pa] will be applied to the airflow paths. If a wind pressure profile element is given, the wind pressure profile will be applied to the airflow paths. The wind pressure profile element can be created by the [wind pressure profile](#wind-pressure-profile) component.
    - **modifier**:
        - Type: Number [Item]
        - Default: 0.36
        - Description: The modifier for differences between wind velocity profiles at the building site and that at a measurement location, e.g., an airport weather station.
    - **sched**:
        - Type: Week schedule (dimensionless) [Item]
        - Default: None
        - Description: Week schedule that controls the airflow path.
    - **filter**:
        - Type: Filter element [Item]
        - Default: None
        - Description: A filter element used for the airflow path.
 - **Outputs**:
    - **path**:
        - Type: Airflow path [Item]
        - Description: An airflow path element with updated settings.

### Airflow path settings
Create airflow paths for different types of building surfaces. 
 - **Inputs**:
    - **ext wall**:
        - Type: Airflow path [List]
        - Default: None
        - Description: A list of airflow path elements for all exterior walls.
    - **int wall**:
        - Type: Airflow path [List]
        - Default: None
        - Description: A list of airflow path elements for all interior walls.
    - **int ceil/floor**:
        - Type: Airflow path [List]
        - Default: None
        - Description: A list of airflow path elements for all interior ceilings and floors.
    - **ext floor**:
        - Type: Airflow path [List]
        - Default: None
        - Description: A list of airflow path elements for all exterior floors.
    - **roof**:
        - Type: Airflow path [List]
        - Default: None
        - Description: A list of airflow path elements for all (exterior) roofs.
 - **Outputs**:
    - **path settings**:
        - Type: Airflow path settings (a JSON file) [Item]
        - Description: An airflow path settings element with updated settings.

![Airflow element components](./img/icons-airflow-one-way-powerlaw.png)
![Airflow element components](./img/icons-airflow-one-way-quadratic.png)
![Airflow element components](./img/icons-airflow-two-way-flow.png)
![Airflow element components](./img/icons-airflow-backdraft.png)
![Airflow element components](./img/icons-airflow-fan.png)
![Airflow element components](./img/icons-airflow-cubic-spline.png)

## 09-Library
![Library components](./img/icons-library.png)
## 10-Ambient
![Ambient components](./img/icons-ambient.png)
## 11-Simulation
![Simulation components](./img/icons-simulation.png)
### Project
### Simulation parameters
### Simulation
### Help
## 12-Results
![Results components](./img/icons-results.png)