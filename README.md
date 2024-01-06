# Greenhouse Gas Flux Tower Datalogger Program

This (older, pre-EasyFlux&trade;) version of [Campbell Scientific](https://campbellsci.com) 
eddy-covariance station code was customized to support the <abbr title="greenhouse gas">GHG</abbr>
monitoring towers operated for the [Regional Approaches to Climate Change](reacchpna.org)
project. Some notable differences are outlined below. For new deployments,
users are recommended to use [EasyFlux-DL](https://www.campbellsci.com/easyflux-dl) instead.

* Drops support for ATI anemometers, LI-COR open-path <abbr title="infrared gas
  analyzers">IRGAs</abbr>, and other sensors that were not used.
* Adds support for Decagon 5TM soil probes and <abbr title="Normalized Difference
  Vegetation Index">NDVI</abbr>/<abbr title="Photochemical Reflectance Index">PRI</abbr> 
  sensors, and certain closed-path gas analyzers by Picarro and Los Gatos Research.
* Adds custom logger menu which allows users to enable/disable sensors and update
  unique values such as sonic azimuth or sensitivity factors.
* Replaces the original dynamically-generated data tables with statically-defined
  ones (which actually changed many times).
* Records additional program deployment metadata to separate table file (e.g. 
  unique sensor values, compilation signatures, etc).

The conditionally-compiled data tables were replaced by static definitions with
the intent of insulating post-processing scripts from changes in data table
structure, namely from the addition or removal or sensors (i.e. instead of
removing the column, it remains present but is filled with NANs). However, in
practice, the structure of the tables still changed fairly often and it was 
necessary to write a script for translating older data files into newer formats
for input to EddyPro. That script, along with required column translation metadata,
are provided in the *Tower Logger Post Processing* repository linked below.

## What's Here

* **default.cr3** - Program file for CR3000 data logger (CRBasic)
* **wiring_details.ods** - Printable table of wiring connections (OpenDocument spreadsheet)
* **data_file_key.ods** - Spreadsheet with details of each data table, including
  column labels, units, variable descriptors, calculation method, and relevant 
  notes on individual fields (OpenDocument spreadsheet)

## Related Works

Please refer to these other repositories for a more complete understanding of
tower operations and data management:

* [Tower Logger Post Processing](https://github.com/wsular/reacchpna-eddyflux-processing)
* [Biomass Sampling Data and Records](https://github.com/wsular/reacchpna-eddyflux-biomass)
* [Field Timelapse Image Management](https://github.com/wsular/reacchpna-eddyflux-timelapse)

## Requirements

This program runs on a Campbell Scientific CR3000-series micrologger with firmware
versions **CR3000.Std.28.02** or earlier. More recent firmware versions may fail to 
compile the program file due to conflicts between defined variable units and units
applied to field names in data tables.

## Program Usage

Most of the core configuration is done through user-defined constants at the start
of the program file. The `ConstTable` instruction was introduced after initial
program development and is only incorporated into unreleased versions. For most
of the program history, logger serial numbers are used to identify the correct
default constants used at compilation time, and then users are permitted to update
sensor-specific values using a custom logger menu.

### ConstTable

> *This feature was introduced in newer, unreleased program versions.*

The constants table allows users to completely enable or disable particular sensors,
which affects whether or not certain ancillary data tables are generated. When
a particular sensor is enabled, the custom logger menu will expose its sensor-specific
parameters for editing.

| Constant | Sensor | Data table(s) |
|:-|:-|:-|
| `SHFP_ENABLED` | Self-calibrating soil heat flux plates (Huskeflux HFP01-SC) | When disabled, relevant columns in 5- and 30-min statistics tables will contain *NAN*. |
| `DEC_SRS_ENABLED` | NDVI/PRI (Decagon SRS series) | When enabled, data table `stats5_srs` becomes active. |
| `LGR_N2O_ENABLED` | N<sub>2</sub>O/CO/H<sub>2</sub>O analyzer (Los Gatos Research Inc, s/n 10-0038) | When enabled, data table `tsdata_extra` becomes active.
| `PIC_CH4_ENABLED` | CH<sub>4</sub>/CO<sub>2</sub>/H<sub>2</sub>O analyzer (Picarro Inc, G2301-f) | When enabled, data table `tsdata_extra` becomes active.


### Custom Logger Menu

> *Note 1: The description below applies to the unreleased version of the program file.
> Most versions of the program do not expose sensor-specific values in a user-editable way;
> instead, relying on the logger serial number and a lookup table defined with program constants.
> In such programs, the menu options are labeled slightly differently and only options 
> for ancillary sensors are shown (Decagon SRS, Picarro CH<sub>4</sub> and Los Gatos Research N<sub>2</sub>O)*
> 
> *Note 2: Unlike deployed program versions (which selected unique parameters
> using a lookup table), this unreleased version does not assign any unique
> parameter values by default. Instead, users must define the sonic azimuth,
> sensitivity values, etc, after starting the program but prior to start of data
> collection. Pay close attention to units of calibration factors as some values
> are converted prior to storage in final data tables.*

Users are able to modify certain sensor configuration and update unique program
values using a menu on the logger display. After applying changes, settings get
stored in a non-volatile settings file on the CPU and the program reloads with
the new configuration. A condensed version of menu options is provided below.

* Initial Setup
  * Soil heat flux plate > Enable? *{No, Yes}*
  * Decagon NDVI/PRI > Enable *{No, Yes}*
    * NDVI up-facing SDI-12 address
    * NDVI down-facing SDI-12 address
    * PRI up-facing SDI-12 address
    * PRI down-facing SDI-12 address
  * LGR N2O/CO > Enable? *{No, Yes}*
  * Picarro CO2/CH4 > Enable *{No, Yes}*
  * Apply now? *{No, Apply}*
* Settings
  * Sonic azimuth (degrees East of True North)
  * NR sensitivity (&micro;V/W/m<sup>2</sup>)
  * PAR sensitivity (&micro;A/mmol/s/m<sup>2</sup>)
  * Soil moisture probes >
    * Enable 5? *{Yes, No}*
    * Enable 6? *{Yes, No}*
    * Enable 7? *{Yes, No}*
    * Enable 8? *{Yes, No}*
    * Enable 9? *{Yes, No}*
  * Soil heat flux plate > *(if enabled in the ConstTable)*
    * Plate #1 (DF 6) Sensitivity (&micro;V/W/m<sup>2</sup>)
    * Plate #1 (DF 6) Resistance (Ohms)
    * Plate #1 (DF 6) Area (m<sup>2</sup>)
    * Plate #2 (DF 7) Sensitivity (&micro;V/W/m<sup>2</sup>) 
    * Plate #2 (DF 7) Resistance (Ohms)
    * Plate #2 (DF 7) Area (m<sup>2</sup>)
  * LGR N2O/CO/H2O > *(if enabled in the ConstTable)*
    * N2O Mult. (units/mV)
    * N2O Offset (units@0mV)
    * CO Mult. (units/mV)
    * CO Offset (units@0mV)
    * H2O Mult. (units/mV)
    * H2O Offset (units@0mV)
  * Picarro CO2/CH4/H2O > *(if enabled in the ConstTable)*
    * CO2 Mult. (units/mV)
    * CO2 Offset (units@0mV)
    * CH4 Mult. (units/mV)
    * CH4 Offset (units@0mV)
    * H2O Mult. (units/mV)
    * H2O Offset (units@0mV)
  * Save changes > Save now? *{Cancel, Yes}*

## Acknowledgments

The core of this work was licensed at cost from Campbell Scientific, who also provided key technical documentation and guidance.

The majority of development work was funded by the Regional Approaches to Climate Change for Pacific Northwest Agriculture project under National Institute of Food and Agriculture award #2011-68002-30191.

## License

This software is provided with express written permission of Campbell Scientific, who
retains all rights to its original work. You may freely use and modify this program
provided the following copyright notices remain intact at the top of the program file:

````
'Copyright (c) 2011-2015, 2017 Washington State University. Shared with permission.
'Copyright (c) 2002, 2006, 2010 Campbell Scientific, Inc. All rights reserved.
````

## Disclaimer

For complete terms, including software Limited Warranty disclaimer, please [see here](https://www.campbellsci.com/terms). 

Contributions to the original source code are similarly disclaimed as follows (excerpted from
[The MIT License]()): 

> THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
