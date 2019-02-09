### Thermostat
This service describes a thermostat.

```
{
    "name": "An items name, as shown in Homekit later",
    "type": "thermostat",
    "currentTempItem": "Itemname-within-OpenHAB",
    "targetTempItem": "Itemname-within-OpenHAB",
    "targetCoolingThresholdItem": "Itemname-within-OpenHAB",
    "targetHeatingThresholdItem": "Itemname-within-OpenHAB",
    "currentHumidityItem": "Itemname-within-OpenHAB",
    "targetHumidityItem": "Itemname-within-OpenHAB",
    "targetHeatingCoolingStategItem": "Itemname-within-OpenHAB",
    "currentHeatingCoolingStateItem": "Itemname-within-OpenHAB",
    "tempUnit": "Celsius",
    "thermostatMode": "HEAT_COOL_AUTO"
}
```
* `currentTempItem`: The openHAB item representing the current temperature as measured by the thermostat
  * Needs to be of type `Number` within openHAB
* `targetTempItem`: The openHAB item representing the target temperature inside the room
  * Needs to be of type `Number` within openHAB
* `targetCoolingThresholdItem`*(optional)*: The openHAB item representing the Cooling threshold temperature for auto mode
  * Needs to be of type `Number` within openHAB
* `targetHeatingThresholdItem`*(optional)*: The openHAB item representing the Heating threshold temperature for auto mode
  * Needs to be of type `Number` within openHAB 
* `currentHumidityItem` *(optional)*: The openHAB item representing the current humidity as measured by the thermostat
  * Needs to be of type `Number` within openHAB
* `targetHumidityItem` *(optional)*: The openHAB item representing the target humidity inside the room
  * Needs to be of type `Number` within openHAB
* `targetHeatingCoolingStategItem` : The openHAB item representing the thermostat mode
  * Needs to be of type `Number`, where OFF = 0;HEAT = 1;COOL = 2;AUTO = 3
* `currentHeatingCoolingStateItem` : The openHAB item showing the current action of your heater or air conditioner (cooling in progress, heating, open radiator valve, etc.). This affects the color of the item in the Home App.
  * Needs to be of type `Number`
   * Allowed values: `from 0 to 3`: NACTIVE = 0; IDLE = 1; HEATING = 2; COOLING = 3;
* `thermostatMode` :
  * Allowed values: `HEAT` & `COOL` & `HEAT_COOL` & `HEAT_COOL_AUTO` & `AUTO` 
  */// for example, for `HEAT` - "TargetHeatingCoolingState":{"validValues":[0,1]}
  
* `tempUnit` *(optional)*: Gives the measurement unit of the thermostat, currently does not change anything inside HomeKit
  * Default: `Celsius`
  * Allowed values: `Celsius` & `Fahrenheit`

