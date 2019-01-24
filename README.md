# Homebridge Plugin for OpenHAB2 - Complete Edition

This [homebridge](https://github.com/nfarina/homebridge) plugin for [openHAB](https://www.openhab.org) has the expectation to fully support all Services offered by Apple's Homekit Accessory Protocol (HAP), as far as it is feasible based on the Item types offered by OpenHAB. In opposite to the existing [openHAB homebridge plugin](https://www.npmjs.com/package/homebridge-openhab2) or the native [openHAB Homekit Plugin](https://www.openhab.org/addons/integrations/homekit/) this plugin requires explicit declaration of accessories in the homebridge configuration and does not use openHAB's tagging system, which leads to a little more effort during configuration, but should prove more reliable and functional in more complex installations. See [Comparisson](#comparison) below.

## Installation

*Note: Please install [homebridge](https://www.npmjs.com/package/homebridge) first.*

```
npm install -g homebridge-openhab2-complete
```

## Configuration

This is a platform plugin, that will register all accessories within the Bridge provided by homebridge. The following shows the general homebridge configuration (`config.json`), see the [Supported HAP Services below](#supported-hap-services), in order to get the detailed configuration for each Service.

```
{
    "bridge": {
        ...
    },

    "accessories": [
        ...
    ],

    "platforms": [
        {
            "platform": "openHAB2-Complete",
            "host": "http://192.168.0.100",
            "port": "8080",
            "accessories": [
                {
                    "name": "An items name, as shown in Homekit later",
                    "type": "switch",
                    "habItem": "Itemname-within-OpenHAB"
                },
                ...
            ]
        },
        ...
    ]
}
```
* `platform` has to be `"openHAB2-Complete"`
* `host`: The IP or hostname of your openHAB instance. The Protocol specifier (`http://`) is optional, defaults to `http://` (independent of the port)
* `port`: Optional if not the default port of the protocol specified in `host`
* `accessory`: An array of accessories exposed to HomeKit, see the next chapter for available services and their configurations.

## Supported HAP Services
The following is a list of all Services that are currently supported and which values are required within the accessory configuration. 

### Switch

This service describes a binary switch.

```
{
    "name": "An items name, as shown in Homekit later",
    "type": "switch"
    "habItem": "Itemname-within-OpenHAB"
}
```
* `habItem` is expected to be of type `Switch` within openHAB

### Lightbulb

This service describes a lightbulb.

```
{
    "name": "An items name, as shown in Homekit later",
    "type": "light"
    "habItem": "Itemname-within-OpenHAB"
}
```
* `habItem` is expected to be of type `Switch`, `Dimmer` or `Color` within openHAB (This changes the functionality withtin HomeKit)

### Temperature Sensor

This service describes a temperature sensor.
```
{
    "name": "An items name, as shown in Homekit later",
    "type": "temp"
    "habItem": "Itemname-within-OpenHAB"
    "habBatteryItem": "Itemname-within-OpenHAB"
    "habBatteryItemStateWarning": "ON"
}
```
* `habItem` is expected to be of type `Number` within openHAB 
* `habBatteryItem` (optional) defines a openHAB item of type `Switch` that represents a battery warning for the service
* `habBatteryItemStateWarning` (optional, default: `ON`) state of the `habBatteryItem` that triggers a warning


## Additional Services & Notes from the Developer

Obviously the aim of this project is a full coverage of the HAP specification. Due to the limitations of smart devices in my home I can only test a subset and would love to have your feedback and input for this project.

Due to the very limited documentation on homebridge plugin development I have not implemented a dynamic platform (there is only [this partly complete wiki entry](https://github.com/nfarina/homebridge/wiki/On-Programming-Dynamic-Platforms)). If anyone of you knows how to do it, please contact me directly!

If you have feedback or suggestions how to better represent the Services as openHAB Items, feel free to open an [issue](https://github.com/steilerDev/homebridge-openhab2-complete/issues).

If you would like to contribute just send me a pull request. In order to add a new service you have to modify/add the following parts:
1. `./index.js`: 
    * `SerialNumberPrefixes` at the beginning need to get a key (your `type` name) and value
    * `this._factories` at the end of the `OpenHABComplete` constructor needs the same key and a reference to a factory method
    * Create your factory method at the end of the file, returning your accessory instance. The configuration block dedicated to your accessory will be passed to this function as JSON
2. Create your own accessory class:
    * Create a new file for your accessory within the `./accessory` folder and include it within `./index.js`
    * The only *required* functions are `getServices()` (returning an array of `HAP.Service` with attached `HAP.Characteristic`) and `identify()` (which does not need to do anything). Those are implemented in the `Accessory` super class and don't need to be overridden.
    * See the `./accessory/Switch.js` accessory for a simple Service and use it as a skeleton


## Comparision

| [OH homebridge plugin](https://www.npmjs.com/package/homebridge-openhab2) | openHAB2 - Complete Edition
--- | --- 
Verly little configuration within homebridge/openHAB, only tags within `*.items` files and inclusion within sitemap | Explicit declaration within `config.json` not requiring instable openHAB `Metadata Provider` (removes items if state is `NULL`) and de-couples homebridge configuration from openHAB
Support only 1:1 mappings between Items and HomeKit Services | Supports composite items (e.g. Thermostat)
No documentation to support extension | Simple concept for extending functionality
Uses `SSE` to receive push notifications from openHAB about state changes | Polling of states through REST interface
Battery Warnings not supported | Battery Warnings supported

Concluding, I personally would use the [OpenHAB homebridge plugin](https://www.npmjs.com/package/homebridge-openhab2) in smaller, less diverse installations. However my own installation has a magnitude of different devices, that I want to fully include in HomeKit, therefore this plugin is the only feasible way for me and everyone alike.