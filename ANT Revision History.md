# ANT Revision History
The history of major enhancements and fixes to ANT is provided below.

## Version 0.2.3
*Release Date: December 18, 2023*
Updates: 
 - The Project component now will create a JSON file to store link flow information (LFI), e.g. location & direction, for visualizing airflows. 
 - A new Vector component is added to viz airflows. 
 - The Contour and Plot components now allow for customizing text color and size.

## Version 0.2.2 
*Release Date: November 30, 2023*

Updates:
 - The algorithm for determining interior surfaces has been changed to make it compatible for concave geometries.

## Version 0.2.1
*Release Date: November 20, 2023*

Updates: 
 - The issue with outputting flow status (pressure, temperature, air density) from projects without contaminants using the result component has been resolved.

## Version 0.2.0
*Release Date: November 9, 2023*

Updates: 
 - Issues with some airflow elements (stairwell, two-way openings, etc.) have been fixed.
 - Buttons to select airflow element type for windows and doors have been added to the window and door components.
 - Issues with the azimuth angle for exterior openings have been fixed.

## Version 0.1.2
*Release Date: November 1, 2023*

Updates: 
 - Zone component can take a string of number as the "temperature" input.
 - The order of adding elements to project in the project component is reorganized for adding all elements properly.

## Version 0.1.1
*Release Date: October 31, 2023*

Updates: 
 - The result component can output temperature, pressure and air density data. Set the "_element" input of the result component to text "temperature", "pressure", or "density" to enable the function. 

## Version 0.1.0
*Release Date: October 30, 2023*

Updates:
 - The latest 3.5 version of CONTAM APIs (ContamP and ContamX APIs) and simulation engine (contamx3.exe) is being used. But the same airflow element issues still exist. 
 - To align with 3.5 version ContamP API, some elements were assigned by names, including AHS supply and return elements. A default name will be assigned and visible as the message of the component when the component is created. 
 - CtmTool will require a valid key for usage.

## Version 0.0.3
*Release Date: October 11, 2023*

Updates: 
 - A probability distribution component is added for parametric analysis.
 - simread.exe is used to read simulation results from the SIM files instead of reading SQLite files. SQLite files are no longer used or generated.
 - The boolean input "_update" on the results processing components (Results and Occupancy Exposure) is removed. The results will be updated automatically when the simulation is completed since the results are read from the SIM files.

## Version 0.0.2
*Release Date: September 26, 2023*

Updates: 
 - The buttons on project and simulation components are replaced with the boolean inputs "_run". The simulation will be run automatically when the boolean input "_run" is set to "True".
 - A boolean input of "_update" is added to the results processing components (Results and Occupancy Exposure) to allow users to update the results without re-connecting the simulation to the results processing components.

## Version 0.0.1
*Release Date: September 20, 2023*

Updates:
- The units of removal rate for constant coefficient source and sink elements are added to the component menu.
- The options of using 1/2 and 1/3 of surface area as the multiplier for airflow path settings are added to the component menu.

## Version 0.0.0 and revisions
*Release Date: August 8, 2023*

The initial release of ANT was named by *Version 1.0.0*. However, the version number was later changed to *Version 0.0.0* to indicate that it is still a beta version. The later versions were continued after *Version 0.0.0*. 

Some minor revisions were made to fix the issues in the initial release. But the revisions were not recorded by changing the version number. Major updates are listed below.

Updates: 
 - The option of selecting surface area as multiplier for airflow path settings is added to the component menu. (*August 17, 2023*)
 - The option of selecting room surface area as the multiplier for deposition sink is added to the component menu. (*August 18, 2023*)
 - The surfaces that are directly connected with the ground will be assigned as exterior floor/wall surfaces by default. (*August 23, 2023*)
 - The results component can read not only species concentrations but also temperatures and pressures of the zones by specifying "temperature" or "pressure" in "_element" input (changed from "_species"). (*August 24, 2023*)
 - The loop component is updated with an option to reset the loop from the first item. (*August 28, 2023*)
 - A static dictionary is created for those components that require a unique name, including AHS and Source/Sink, to store the index for every instance of the component class based on the individual Guid for each instance. This update is to fix the issue that duplicating a abovementioned component will create a new component with the same name as the original one. (*August 29, 2023*)
 - The project component will read WTH and CTM files specified to the project file to assign the start/end simulation date and time for the project, if they are not specified separately. Some earlier issues with this feature are fixed. (*August 31, 2023*)
 - The AHS and source/sink components will show their individual names in the message when they are created. (*September 5, 2023*)

