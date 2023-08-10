# Introduction to ANT components and CONTAM elements

ANT conponents are organized into 12 categories, each of which is represented by a tab in the ANT toolbar. The 12 categories are:
<!--ts-->
 - [01-Geometry](#01-Geometry)
    - [Zone](#zone)
    - [Door](#door)
    - [Window](#window)
    - [Orifice](#orifice)
    - [Generic opening](#generic-opening)
 - [02-HVAC](#02-HVAC)
    - [Air handling system (AHS)](#air-handling-system-(AHS))
    - [AHS Supply](#AHS-Supply)
    - [AHS Return](#AHS-Return)
 - [03-Filter](#03-Filter)
    - [Filter element](#filter-element)
    - [Filter efficiency data - Constant efficiency](#filter-efficiency-data---constant-efficiency)
    - [Filter efficiency data - Simple gaseous](#filter-efficiency-data---simple-gaseous)
    - [Filter efficiency data - Simple particle](#filter-efficiency-data---simple-particle)
    - [Filter efficiency data - UVGI](#filter-efficiency-data---UVGI)
 - [04-Schedule](#04-Schedule)
    - [Day schedule](#day-schedule)
    - [Week schedule (weekdays & weekends)](#week-schedule-(weekdays-&-weekends))
    - [Week schedule (detailed days)](#week-schedule-(detailed-days))
 - [05-Species](#05-Species)
 - [06-Source/Sink](#06-Source/Sink)
 - [07-Occupancy](#07-Occupancy)
 - [08-Airflow](#08-Airflow)
 - [09-Library](#09-Library)
 - [10-Ambient](#10-Ambient)
 - [11-Simulation](#11-Simulation)
 - [12-Results](#12-Results)
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
            - Number input → Constant temperature over time.
            - Schedule input → Variable temperatures over time.
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
        - Description: A list of texts that represents the day schedule data. Each text must be presented as a text in the format of `time value`. Time and value can be separated by a space` `, a comma`,`, a tab`    `, or a dashline`-`. For example, `0 0.1`, `0,0.1`, and `0:0.1` are all valid time-value pairs.
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
## 06-Source/Sink
![Source/Sink components](./img/icons-src-sink.png)
## 07-Occupancy
![Occupancy components](./img/icons-occupancy.png)
## 08-Airflow
![Airflow path components](./img/icons-airflow-path.png)
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