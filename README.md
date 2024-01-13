# Greenhouse Gas Flux Tower Datalogger Program

This older (pre-EasyFlux&trade;) version of [Campbell Scientific][1] open-path eddy-covariance 
(OPEC) station code was customized to support the <abbr title="greenhouse gas">GHG</abbr>
monitoring towers operated for the [Regional Approaches to Climate Change][2]
project. Some notable differences are outlined below. For new deployments, users are
recommended to use [EasyFlux-DL][3] instead.

* Drops support for ATI anemometers, LI-COR open-path <abbr title="infrared gas
  analyzers">IRGAs</abbr>, and other sensors that were not used.
* Adds support for Decagon 5TM soil probes and <abbr title="Normalized Difference
  Vegetation Index">NDVI</abbr>/<abbr title="Photochemical Reflectance Index">PRI</abbr> 
  sensors, and certain closed-path gas analyzers by Picarro and Los Gatos Research.
* Replaces the original dynamically-generated data tables with statically-defined
  ones (which actually changed many times).
* Records additional program deployment metadata to separate table file (e.g. 
  unique sensor values, compilation signatures, etc).
* Adds custom logger menu and `ConstTable` which allows users to enable/disable 
  sensors and update unique program values.

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

A detailed history of changes can be viewed in the [CHANGELOG](CHANGELOG.md).

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

Basic program configuration is done through user-defined constants at the start
of the program file. Logger serial numbers are used to identify the correct set
of constants.

More recent, undeployed program versions replace the lookup table of constants with a
`ConstTable` and custom menu. The constants table allows users to enable or disable 
optional sensors, while the custom menu exposes the unique program values for
editing. Changes made via both mechanisms are retained after power loss. 

## Acknowledgments

The core of this work was licensed at cost from Campbell Scientific, which also provided key technical documentation.
Additional integration documentation was provided by Decagon Devices and Garmin, Intl.

The majority of new development was funded by the Regional Approaches to Climate Change for Pacific Northwest Agriculture project under National Institute of Food and Agriculture award #2011-68002-30191.

## License

This software is provided with express written permission of Campbell Scientific, which
retains all rights to its original work. You may freely use and modify this program
provided the following copyright notices remain intact at the top of the program file:

````
'Copyright (c) 2011-2015, 2017 Washington State University. Shared with permission.
'Copyright (c) 2002, 2006, 2010 Campbell Scientific, Inc. All rights reserved.
````

## Disclaimer

For complete terms, including software Limited Warranty disclaimer, please refer to [https://www.campbellsci.com/terms][4]. 

Contributions to the original source code are disclaimed as follows (excerpted from
[The MIT License][5]): 

> THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## References

[1]: https://campbellsci.com
[2]: https://reacchpna.org
[3]: https://www.campbellsci.com/easyflux-dl
[4]: https://www.campbellsci.com/terms
[5]: http://www.opensource.org/licenses/MIT

* Campbell Scientific. *CR3000 Micrologger Operator's Manual.* Preliminary for OS v.28: 4/20/15.
  Online: <http://s.campbellsci.com/documents/us/manuals/cr3000.pdf>

* Campbell Scientific. *CSAT3 Three Dimensional Sonic Anemometer Instruction Manual.* Rev 2/15. 
  Online: <http://s.campbellsci.com/documents/us/manuals/csat3.pdf>

* Campbell Scientific. *EC150 CO<sub>2</sub>/H<sub>2</sub>O Open-Path Gas Analyzer Instruction Manual.* Rev 12/15.
  Online: <http://s.campbellsci.com/documents/us/manuals/ec150.pdf>

* Campbell Scientific. *GPS16X-HVS GPS Receiver Instruction Manual.* Rev 4/12.
  Online: <http://s.campbellsci.com/documents/us/manuals/gps16x-hvs.pdf>

* Campbell Scientific. *Model HMP155A Temperature and Relative Humidity Probe.* Rev 4/12.
  Online: <http://s.campbellsci.com/documents/us/manuals/hmp155a.pdf>

* Campbell Scientific. *Model HFP01SC Self-Calibrating Soil Heat Flux Plate.* Rev 8/07.
  Online: <http://s.campbellsci.com/documents/us/manuals/hfp01sc.pdf>

* Campbell Scientific. *LI190SB Quantum Sensor Instruction Manual.* Rev 2/15.
  Online: <http://s.campbellsci.com/documents/us/manuals/li190sb.pdf>

* Campbell Scientific. *Met One 034B Windset Instruction Manual.* Rev 3/12.
  Online: <http://s.campbellsci.com/documents/us/manuals/034b.pdf>

* Campbell Scientific. *NR-LITE2 Net Radiometer Instruction Manual.* Rev 9/10.
  Online: <http://s.campbellsci.com/documents/us/manuals/nr-lite2.pdf>

* Campbell Scientific. *TE525 Tipping Bucket Rain Gage.* Rev 8/16.
  Online: <http://s.campbellsci.com/documents/us/manuals/te525.pdf>

* Decagon Devices. *5TE/5TM Integrator's Guide.* Rev 9. 
  Online: <http://manuals.decagon.com/Integration%20Guides/5TM%20Integrators%20Guide.pdf>

* Decagon Devices. *5TM Water Content and Temperature Sensors.* Version July 10, 2017.
  Online: <http://manuals.decagon.com/Manuals/13441_5TM_Web.pdf>

* Decagon Devices. *SRS CSI Example Program.* 
  Online: <http://www.decagon.com/assets/Uploads/SRS-pair-example-program.CR1.zip>

* Decagon Devices. *SRS NDVI Sensor Integrator Guide.* Version R03.
  Online: <http://manuals.decagon.com/Integration%20Guides/SRS-N%20Integrators%20Guide.pdf>

* Decagon Devices. *SRS PRI Sensor Integrator Guide.* Version R03.
  Online: <http://manuals.decagon.com/Integration%20Guides/SRS-P%20Integrators%20Guide.pdf>

* Decagon Devices. *SRS Spectral Reflectance Sensor Operator's Manual.* Version July 10, 2017.
  Online: <http://manuals.decagon.com/Manuals/14597_SRS_Web.pdf>

* Garmin International. *GPS 16x Technical Specifications.* Revision B, October 2008.
  Online: <http://static.garmincdn.com/pumac/GPS_16x_tech_specs.pdf>

* Garmin International. *GPS 16x Technical Specifications.* Revision C, October 2011.
  Online: <https://static.garmin.com/pumac/GPS_16x_tech_specs.pdf>
