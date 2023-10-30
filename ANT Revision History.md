# ANT Revision History
The history of major enhancements and fixes to ANT is provided below.

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

## Version 0.0.0 and revisions
*Release Date: August 8, 2023*

The initial release of ANT was named by *Version 1.0.0*. However, the version number was later changed to *Version 0.0.0* to indicate that it is still a beta version. The later versions were continued after *Version 0.0.0*. 

Some minor revisions were made to fix the issues in the initial release. But the revisions were not recorded by changing the version number. Major updates are listed below.

Updates: 
 - A static dictionary is created for those components that require a unique name, including AHS and Source/Sink, to store the index for every instance of the component class based on the individual Guid for each instance. This update is to fix the issue that duplicating a abovementioned component will create a new component with the same name as the original one. (*August 29, 2023*)
