# Borehole-Dilution-Test

Link to git repository to get the code:

```
git clone https://github.com/Mohammed-Fadul/Borehole-Dilution-Test.git
```

Link to web page:
```
https://mohammed-fadul.github.io/media.Mohammed-Fadul.github.io
```
## Theoretical Background
The borehole dilution test, or point dilution test, is a single-well test for calculating Darcy velocity in aquifers. A tracer is introduced into a well section and its change in concentration is recorded over time. To achieve the proper mixing conditions, a tracer is introduced into a borehole and mixed there during the testing process. Over time, the dilution of the tracer caused by the entry of fresh water and the outflow of water mixed with tracer from the wellis monitored.<sup>[1][2][3]

The tradiotional method of calculating the Darcy´s velocity as a function of dilution rate is that decreasing concentration of tracer is proportional to both, the apparant velocity in the test section and The Darcy velocity.<sup>[1][2][3]

Assuming that factors related to insufficient mixing (density effects, vertical flow, etc.) can be neglected, the Darcy velocity vf follows the relationship of:

$$ {v_{f}} = -{V \over \alpha.F.t} ln{c \over c_{0}} $$

Alpha is a dimensioless factor that corrects the horizontal flow patterns created in the aquifer by boreholes due to the well screen effect. The well screen effect is caused by convergence of flow field because of differences in hydraulic conductivity between the aquifer and the well.<sup>[1][2][3]


### Purpose and motivation

The flow of groundwater is not measured directly through the sensors. The results obtained from the borehole dilution test are of the change in concentration of the tracer recorder over time, from which the velocity is calculated for each time step to generate the plot of velocity, from which the average Darcys velocity is calculated. The aim of this program is to generate the required plots to enable the user to calculate Darcy's velocity via a graphical solution which is the standard method, and to give instant estimate of the Darcy´s velocity from the data as a 50% percentile range. An example of the file can be seen in later sections.

### Software Requirements

The software requirements to successfully run this program and obtain the Darcy Velocity are:    

1. Python 3.9.16 or more recent versions
2. libraries/modules:
      - numpy
      - pandas
      - os (Miscellaneous operating system interfaces)
      - math
      - typing
      - sklearn
      - matplotlib
      - pathlib
       
### Obtaining Results

Upon meeting software requirements and getting all the python files in the local device, run the main script
```
python main.py
```
to get the estimation of the velocity on the console or the log file. Also, a new folder named (Plots) will appear in the directory where the program is saved that contains graphs generated from the given data. A new excel file contains all the calculated velocities will appear in the directory also a logfile containing the details of your run.

There is already an excel file named (All_Data) in the folder (Input_data_workbook) containing a typical data from a field test as an example of how the user 
should prepare his excel input file for reusing this code for other experiments.


## Code 

The object-oriented codes use custom classes that are called within a `main.py` script which calculates the Darcy´s 
velocity average. The code is based on custom classes and functions that are defined in the following files:

1-logging_parameters (package) that contains:
* `checker.py`: contains logging functions

2-tracer_tests (package) that contains:
* `tracertests.py`: contains a Tracer class to read the basic elements of borehole test from field data, and make the ordinary
calculations needed for the calculation of Darcy´s velocity.

3-file_utiles (package) that contains:
* `base_file(package)`: contains `__init__.py` file which contains class BaseFile to check if data files exist
* `sensor_data_file.py`: contains the DataSheet, Calibration and SensorPairData classes to read the data from the sensors.

4-plot_settings (package) that contains:
* `plotSensors2.py`: contains the functions for the plots calculation - sensors plot, time concentration plot and 
velocity plots;
* `plot_saver.py`: contains a function to create a file to save plots in.

The relation among the classes and functions are according to the following diagram:

![alt text](https://github.com/Mohammed-Fadul/media/blob/a6d5d1b5823e1d292d988a7080570c4075761b29/Untitled%20Diagram.jpg)

### LOGGING FUNCTIONS
```{admonition} Code Requirements
Code Requirements: necessary to import the logging.
```
The `checker.py` is made of two functions:
1. `log_parameters`: contains all the basic configurations of the logging;
2. `logger`: this returns the function wrapper for the main() function that are going to execute logging messages throughout the code.

**Note** - The logging information are going to be saved in the file `logfile.log`.

### FIELD DATA 
```{admonition} 
Code Requirements: necessary to import numpy library, and the logging_parameters function from the checker.py file.
```
The `tracertests.py` file has the variables and the methods which are relevant to Darcy´s velocity calculation. 
In the class `Tracer`, all the constant data taken from the local site test is found. They include: 
* Drilling´s and well´s diameter;
* Well´s head above ground level;
* Top of gravel pack below ground level;
* Top of filter screen below ground level;
* Bottom of well below ground level;
* Depth of filter below well´s head;
* Water table below well´s head;
* Initial concentration of the tracer in the well;
* Permeability of the gravel pack;
* Permeability of the aquifer formation;
* Accuracy of the fluorescence sensor.

By these information, it is possible to execute calculations found in the different functions within the Tracer class.
* Top gravel pack (`top_gravel_pack`): returns the distance from the wellhead to the top of the gravel pack;
* Top filter screen (`top_filter_screen`): returns the distance from the wellhead to the top of the filter screen;
* Water head (`water_head`): returns the profile of the water in the well measured from the bottom of the well;
* Volume in well (`volume_in_well`): returns the volume of the water in the well;
* Area of flow (`area_of_flow`): returns the area perpendicular to the flow direction;
* Calculate alpha (`calculate_alpha`): returns the correction factor borehole horizontal flow rate Qb and aquifer horizontal flow rate Qf;
* Calculate vf (`calculate_vf`): returns the area of the screen filter.

### SENSORS DATA 
```{admonition} 
Code Requirements: necessary to import logging, pandas, numpy, typing and sklearn libraries, and to inherit the file_utils 
from the BaseFile in __init__.py (packet base_files).
```
The provided sensors data is stored in the xlsx workbook **All_Data.xlsx** which contains the following information:

| **dilution time(s)** | **fluorescense** |
|----------------------|------------------|
| 0                    | 2                |
| 5                    | 5                |
| 10                   | 7                |
| ...                  | ...              |


| **Flourescense** | **µg/L Uranine** |
|------------------|------------------|
| -3               | 0                |
| 8                | 10               |
| 23               | 25               |
| ...              | ...              |

In the excel file, the dilution time vs fluorescense table is located in the **Data DC** and **Data FC** sheet, giving information of each 
sensor test. While, Flourescense vs Uranine table is located in the **DC calibration** and **FC calibration** sheet with the information of each sensor´s calibration. 
1. `Basefile`: checks if the file to be opened exists
      * accepts a filepath string as input.
      * Provides several properties to retrieve information about the file such as filename, parent directory, and extension.
      * `__set_filepath()`: verifies that the specified file exists before creating a Path object to represent the file. 

2. `DataSheet` & `CalibrationSheet`: Two small classes are written to check the if the right column names exist in calibration and data files for both sensors.
      * They take in a Pandas DataFrame object (`data_dataframe`) and a list of column names, parameters, that should be present in the DataFrame. 
      * The `__init__` method initializes the object by assigning the input parameters to the class variables `parameters` and `combined_data_dataframe`, and then calls the `__verify_parameters` method to ensure that all the column names specified in parameters are present in combined_data_dataframe.
      * If any of the specified column names are not present, an exception is raised.
      * The `__verify_parameters` method checks if each parameter in parameters is present in`combined_data_dataframe.columns`, and if not, raises an exception with an error message that specifies the invalid column names.

3. `SensorPairData`: After reading from the excel files, the calibration data is checked for linear correlation and the coefficient of determination is calculated
      * inherits from class `BaseFile`
      * The SensorPairData class takes in a name and a filepath as input parameters, along with optional parameters specifying the names of different sheets in the             Excel file that contain data and calibration information. The `__init__` method initializes the object by assigning the input parameters to class             variables and then calling the `__read_sensor_excel` method to read the data and calibration sheets from the Excel file.
      * `__read_sensor_excel`: reads the data and calibration sheets from the Excel file specified by filepath using Pandas read_excel method. It then creates instances of `DataSheet` and `CalibrationSheet` classes to store the data and calibration information, respectively.
      * `check_calibration`: checks the calibration data for both FC and DC sensors by calling the `__check_sub_cal` method with the calibration data as input               * `__check_sub_cal` method calculates the coefficient of determination $R^2$ for the calibration data using linear regression and returns the regression equation for the calibration data in the form **y = mx + b**, where m is the slope and b is the y-intercept.

### PLOTS 
```{admonition} 
Code Requirements: necessary to import logging, matplot and os libraries, and to inherit from the file plot_saver the function 
save_plot.
```

The `plotSensors2.py` contains three functions with different outputs each: 

1. `sensors_plot` - the parameters of this function are the calibration data from each type of sensor, in which returns the plot of flourescence vs uranine;

2. `time_concentration_plot`- function that returs the plot of the Time vs Fluorescence for each sensor. The parameters are the tests results from each sensor.

3. `velocity_plot` - function that returns the plot of Darcy´s Velocity vs Time vs Fluorescence concentration for each sensor. The parameters are also the tests results frome each sensors plus the calculated Darcy´s velocity. 

The `plotSensors2.py` inherits the `plot_saver.py` which contains the function `save_plot` that allows the plot to be correctly saved in the -named given - folder. It also returns logging information in case of exceptions. The arguments for the save_plot are the folder path, plot and the name of the image which are given when calling this function inside of each plot function. 


### DARCY´S VELOCITY CALCULATION 
```{admonition} 
Code Requirements: necessary to import pandas and math libraries, and to inherit functions from the tracertests, sensor_data_file, 
plotSensors2 and checker. 
```
The Darcy´s velocity is calculated in the `main.py` script. In this file we can find: 
* `darcys_velocity_each_second`: this function returns the data frame containing the final darcy´s velocity of each time. 
It receives the field_data, the slope of the sensor´s equation and intercept of the sensor´s equation as parameters. 

* `darcys_velocity_averaged`: this function returns the Darcy´s velocity averaged, and 25% of the lower and upper values 
are excluded giving a 50% of range.

* `main`: the main function is responsible for calling the functions in the correct order: 
1. This fuction sets the document´s information (**All_Data.xlsx**), as parameters to the `sensor_data_file.py`;
2. The instantiation of the objects and attributes are saved in variables which take the information also from 
`sensor_data_file.py` (eg: regression line information, calibration information and sensors information);
3. After having all the necessary inputs, it is called `darcys_velocity_each_second` with the required parameters for each type of sensor;
4. The `darcys_velocity_averaged` is executed with a logging message that allows to know which type of sensor is this Darcy´s velocity result;
5. When successfully runned, the information is saved in a excel file named **Results.xlsx** that looks like this:

|    | Time | Flourescense | Uranine(mg/l) | ln(c/c0) | vf(m/s)   |
|----|------|-------------|---------------|----------|-----------|
| 0  | 0    | 2           | 0,001         |  -13,71  | 837038,04 |
| 1  | 5    | 5           | 0,001         | -13,71   | 0,16      |
| 2  | 10   | 7           | 0,001         | -13,71   | 0,083     |
| 3  | 20   | 39          | 40,07         | -3,11    | 0,012     |
| 4  | 25   | 51          | 52,11         | -2,84    | 0,008     |
| ...| ...  | ...         | ...           | ...      |   ...     |

6. Last, the plots are executed when calling the functions `velocity_plot`, `time_concentration_plot` and `sensors_plot` with the corrected parameters.
and the new files for the results and logging will be generated.


*References*
1. L. Piccinini, P. Fabbri & M. Pola. (2016). Point dilution tests to calculate groundwater velocity: an example in a porous aquifer in northeast Italy, Hydrological Sciences Journal, 1-2
2. Radulović, M.M., Poleksić, S. & Blagojević, M. A modified point dilution test for the assessment of groundwater flux in karst aquifers. Environ Earth Sci 78, 473 (2019). https://doi.org/10.1007/s12665-019-8489-4
3. Zhang, Y., Wang, H., Zhang, X., Dong, H. (2020). Groundwater velocity determination by single-borehole dilution test, IOP Conference Series Earth and Environmental Science 525(1):012175, 1
4. Lecture notes, Field Course Hydrogeology
