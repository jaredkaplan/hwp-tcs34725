TCS34725 Hardwarepins Library
=============================

This library provides an interface to the TCS34725 RGB sensor.

* [Product information and data sheet](http://www.ams.com/eng/Products/Light-Sensors/Color-Sensor/TCS34725)

Getting Started
---------------

This library can be imported into Kinoma Studio and added to an application's build path settings. You can then use the TCS34725 module to create an object that polls the sensor's RGB value and returns the result to the application.

The following objects can be created using the TCS34725 module in this library:

* [TCS34725](#tcs34725)

Example
-------

The following example demonstrates how to instantiate a new TCS34725 object and poll the RGB value from the sensor.

```xml
<program xmlns="http://www.kinoma.com/kpr/1">
    <require id="TCS34725" path="TCS34725" />
    <script>
        <![CDATA[
            // create a new TCS34725 instance and specify the 
            // I2C data and clock pins on the device
            var tcs34725 = new TCS34725.TCS34725( 27, 29 );

            // initialize the sensor and start polling
            tcs34725.init();
            tcs34725.poll( 50, "milliseconds", "/tcs34725/value" );
        ]]>
    </script>
    <handler path="/tcs34725/value">
        <behavior>
            <method id="onInvoke" params="handler, message">
                <![CDATA[
                    var data = message.requestObject;

                    if( data != null )
                        trace( data.webcolor + "\n" );
                ]]>
            </method>
        </behavior>
    </handler>
</program>
```

API Reference
-------------

The following reference describes the object interfaces defined in the TCS34725 module.

TCS34725
------

The TCS34725 object interface is used to get RGB values from the TCS34725 sensor.

Creating a new instance of a TCS34725 object:

```javascript
var tcs34725 = new TCS34725.TCS34725( sda, scl, addr, led, integrationTime, gain, config );
```

>The led pin specified in the constructor is used control an optional LED light on the breakout board if it has one (such as the [Adafruit TCS34725 RGB Sensor](http://www.adafruit.com/product/1334)). 
>
>By default, the sensor's integration time is set automatically by to the most appropriate value when polling. The integrationTime can be specified using one of the following constants, specified in the TCS34725 module:
>
>* INTEGRATION_TIME_AUTO
>* INTEGRATION_TIME_2_4MS
>* INTEGRATION_TIME_24MS
>* INTEGRATION_TIME_50MS
>* INTEGRATION_TIME_101MS
>* INTEGRATION_TIME_154MS
>* INTEGRATION_TIME_700MS
>
>The sensor's gain can be controlled by specifying one of the following constants:
>
>* GAIN_1X
>* GAIN_4X
>* GAIN_16X
>* GAIN_60X

Initializing the TCS34725 object instance:

```javascript
tcs34725.init();
```

To request the RGB value from the sensor, you must specify a callback to receive the result. The callback is the name of a handler that is implemented in your program or module:

```javascript
tcs34725.get( callback );
```

To continuously poll the RGB values from the sensor, you must specify the polling interval and time units, as well as the callback handler to receive the temperature value:

```javascript
tcs34725.poll( time, units, callback, skipFirst );
```

>The units parameter must be one of the following string literal values:
>
>* milliseconds
>* seconds
>* minutes
>* microseconds
>* nanoseconds
>* hours
>* days

When the TCS34725 object's callback handler is invoked, the result will be passed in the message requestObject property. The RGB value will be passed in a data structure to the application in the following format:

```javascript
{ 
   raw:{r:82,g:79,b:54,c:221},
   r:94,g:91,b:62,
   color:6183742,
   webcolor:"#5E5B3E"
}
```

Turning the LED light on and off:

```javascript
tcs34725.ledOn( callback );
tcs34725.ledOff( callback );
tcs34725.setLED( enabled, callback );
```
