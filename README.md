# ShotSpotter Sample Live Fire Test Data #

This dataset comprises gunshot audio and supporting data released as part of [ShotSpotter Tech Note 098, "Precision and accuracy of acoustic gunshot location in an urban environment"](./TN%20098-Accuracy%20of%20Acoustic%20Gunshot%20Location.pdf).

The data derive from a series of live fire tests of the ShotSpotter Respond gunshot location system conducted in Pittsburgh, PA on December 18th, 2018 by the Pittsburgh Bureau of Police. ShotSpotter uses live fire tests to validate that the deployed sensor density is appropriate for the community in question, and to 
ensure the system is ready for production use.

Source data from the Pittsburgh tests is comparable to what is submitted as evidence in a criminal trial. These materials comprise time-stamped acoustic recordings, time- and location-stamped output of the pulse processing algorithms, and tagged pulse sets suitable for acoustic multilateration. These data permit other investigators to review or extend the current work.

This dataset is released under the Creative Commons [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.

For more about ShotSpotter, see [https://www.shotspotter.com](https://www.shotspotter.com).

# Data Released #

The dataset comprises audio files recorded by ShotSpotter's acoustic sensors, metadata that describe audio files, and metadata associated with the test itself such as the surveyed location of the firing position. Audio files from up to six reporting sensors are provided for each test.

Both the sensor positions and the firing position have been anonymized by converting location of each sensor from latitude-longitude-elevation to an arbitrary Cartesian x-y-z coordinate system. Each firing position has a different arbitrary origin.

## Primary Data ##
Data provided on each test is as follows:

### Sensor Audio: ```test_{xxx}.audio.{sensor serial number}.wav``` ###

These files are 12 kHz 16-bit mono uncompressed audio files in WAV format. The timestamp of the first sample of the file is given in the associated metadata.

### Sensor Metadata: ```test_{xxx}.audio.{sensor serial number}.json``` ###

Metadata associated with audio files, as follows:

| field | description |
|-------|-------------|
| serial_number | sensor serial number |
| timestamp | UTC timestamp of the start of the first sample |
| location | xyz sensor location in arbitrary coordinate system (meters) |
| description | human-readable description of the audio |
| original_file_md5 | checksum of the original (non-anonymized) file in ShotSpotter's private data repository. |

### Survey Location: ```test_{xxx}.survey.json``` ###

Surveyed location of the firing test, as determined by a GPS-enabled smartphone. Accuracy of the survey location is approximately 2.5 meters.

| field | description |
|-------|-------------|
| test_id | test id, sequential from start of test |
| firing_position | each firing position has a different arbitrary origin |
| num_rounds | number of shots fired in the test |
| firearm | type of firearm (one of three) |
| approximate_time_utc | UTC timestamp recorded by the program manager. Not an accurate discharge time |
| survey_location | xyz discharge location in arbitrary coordinate system (meters) 
| description | human-readable description of test |

### Weather: ```test_{xxx}.weather.json``` ###


Weather data, reported from the nearest available NWS METAR station.

| field | description |
|-------|-------------|
| fluid | always "air" |
| temperature | in deg C |
| speed | windspeed in m/s |
| direction | wind direction, as a from azimuth |
| observation_time | as reported by METAR station |
| source | reporting station |

## Derived Data ##

The data above comprise the primary dataset; it is possible to analyze the provided audio files, identify the gunshot pulses, and compare the computed locaton with the survey location using only the data above. 

The dataset also includes two additional files describe the output of ShotSpotter's signal processing and pulse grouping algorithms. These may be useful to researchers who are primarily interested in multilateration accuuracy.

### Pulse Data: ```test_{xxx}.allpulses.json``` ###

Pulse data reported by ShotSpotter sensors. This file includes pulses from sensors for which audio data is not provided.

| field | description |
|-------|-------------|
| pulse_uuid | uuid |
| serial_number | serial number of the reporting sensor. There are not audio files associated with all pulses in this file. |
| arrival_time_utc | pulse arrival time, zero-crossing point at start of pulse |
| location | xyz sensor location in arbitrary coordinate system (meters) |
| location_error | sensor's self-described position error (meters) |
| signal_to_noise_dB | ratio of peak value of gunshot pulse to pre-pulse noise |
| power_dB_SPL | peak power of the pulse, in dB SPL |

### Pulse Selection Data: ```test_{xxx}.nlos-tuples.json``` ###

Sets of pulses from ```test_{xxx}.allpulses.json``` associated with each individual shots, as selected by ShotSpotter algorithms.

| field | description |
|-------|-------------|
| shot_index | index within test, (0..2) |
| pulse_uuids | foreign keys to ```test_{xxx}.allpulses.json```|

# Tests Conducted #

There were a total of nine firing positions. Eighteen tests were conducted at each firing position, for a total of 162 tests. 

Tests were conducted in the following sequence at each firing position:

### .45 caliber automatic handgun ###
-------------------------------------
* 3 rounds
* 3 rounds
* 3 rounds
* 1 round 
* 1 round 
* 1 round

### .40 caliber automatic handgun ###
-------------------------------------
* 3 rounds
* 3 rounds
* 3 rounds
* 1 round 
* 1 round 
* 1 round

### 9 mm automatic handgun ###
------------------------------
* 3 rounds
* 3 rounds
* 3 rounds
* 1 round 
* 1 round 
* 1 round

# Misc #

Comments or questions can be directed to ```rcalhoun@shotspotter.com```.

Last update 29 July 2021.
