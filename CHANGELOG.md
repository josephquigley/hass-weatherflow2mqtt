# Home Assistant WeatherFlow2MQTT Changelog

## Version 2.0.3

**Release Date**: NOT RELEASED YET

### Changes in release 2.0.3

`NEW`: A new sensor called `wf_visibility`has been added. This sensor shows the distance to the horizon, in either km or nautical miles, depending on the unit_system.

## Version 2.0.2

**Release Date**: June 18th, 2021

### Changes in release 2.0.2

**Please make a backup of `weatherflow2mqtt.db` before upgrading. Just in case anything goes wrong.**

`FIX`: If the forecast data from WeatherFlow is not available, the program will now just skip the update, and wait for the next timely update, instead of crashing the Container.

`CHANGED`: Attributes for each sensors are now moved from the event topics, to each individual sensor, so that we can add sensor specific attributes. This will have no impact on a running system.

`NEW`: Started the work on creating Sensors for High and Low values. A new table is created and daily high/low will be calculated and written to this table. Currently only day high and low plus all-time high and low values are calculated. The values are written as attributes to each individual sensor where I believe it is relevant to have these values. **Note** It takes 10 min before the daily max and min values are shown, and all-time values are first shown the following day after upgrading, or on the first run of this program.

| Attribute Name   | Description   |
| --- | --- |
| `max_day` | Maximum value for the current day. Reset at midnight. |
| `max_day_time` | UTC time when the max value was recorded. Reset at midnight. |
| `min_day` | Minimum value for the current day. Reset at midnight. |
| `min_day_time` | UTC time when the min value was recorded. Reset at midnight. |
| `max_all` | Maximum value ever recorded. Updated at midnight every day. |
| `max_all_time` | UTC time when the all-time max value was recorded. Updated at midnight every day. |
| `min_all` | Minimum value ever recorded. Updated at midnight every day. |
| `min_all_time` | UTC time when the all-time min value was recorded. Updated at midnight every day. |

The following sensors are displaying Max and Min values:

| Sensor ID   | Max Value   | Min Value   |
| --- | --- | --- |
| `air_temperature` | Yes | Yes |
| `dewpoint` | Yes | Yes |
| `illuminance` | Yes | No |
| `lightning_strike_count_today` | Yes | No |
| `lightning_strike_energy` | Yes | No |
| `rain_rate` | Yes | No |
| `rain_duration_today` | Yes | No |
| `relative_humidity` | Yes | Yes |
| `sealevel_pressure` | Yes | Yes |
| `solar_radiation` | Yes | No |
| `uv` | Yes | No |
| `wind_gust` | Yes | No |
| `wind_lull` | Yes | No |
| `wind_speed_avg` | Yes | No |

## Version 2.0.1

**Release Date**: June 15th, 2021

### Changes in release 2.0.1

* `FIX`: Fixing the AIR Density value, when using Imperial Metrics

## Version 2.0.0

**Release Date**: June 15th, 2021

### Changes in release 2.0.0

* `NEW`: There is now a new sensor called `pressure_trend`. This sensor monitors the Sea Level pressure changes. The Pressure Trend state is determined by the rate of change over the past 3 hours. It can be one of the following: `Steady`, `Falling` and `Rising`. Please note we will need to gather 3 hours of data, before the returned value will be correct. Until then the value will be `Steady`.
* `NEW`: The simple storage system, that used two flat files, has now been rewritten, to use a SQLite Database instead. This will make future developments easier, and is also the foundation for the new Pressure Trend sensor. If you are already up and running with this program, your old data will automatically be migrated in to the new SQLite Database. And once you are confident that all is running, you can safely delete `.storage.json` and `.lightning.data`.
* `BREAKING CHANGE`: To better seperate the sensors created by this Integration from other Weather related sensors, this version now prefixes all sensor names with `wf_` and all Friendly Names with `WF`. As each sensor has a Unique ID that does not change, the sensors will keep the old Entity Id, and just change the name, and only the Friendly Name will change after this upgrade. But if you delete the Integration, and re-add it, then all the sensors will have the `wf_` as a prefix. The same goes for new sensors that might be added in the future. So if you want to avoid any future issues, I recommend deleting the `WeatherFlow2MQTT` device from the MQTT Integration, and then restart the Docker Container, to get all the sensors added again, with the new naming convention.