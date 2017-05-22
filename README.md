# WaterPump
This is a microPython Libary for working with Pumps, Flowmeters, Pressure Sensors, valves, LED's, and buttons.
do the the memory issuse with my code I have had to move to frozen modules. if you try to load the raw files into flash you will have memory errors. 

# Install Instructions
if running a Adafruit Feather HUZZAH with ESP8266 WiFi make sure to alicate more then 8M of the flash.
Adafruit demo use 8M flash_size as images before 1.8.6 fit fine, this is to small for this
example use something like this:
```python
      esptool.py --port /path/to/port --baud 460800 write_flash --flash_size=32m 0 /path/to/image
``` 

For more then about two modules to work you must byte freeze the code. Please see the tutoral from adafruit if you need help on craeting byte frozen code.

https://learn.adafruit.com/micropython-basics-loading-modules/frozen-modules?view=all

## uasync.core
This project is in the process implementing asyncio and coroutines. Example_uasyncio/main.py
is a example file that will implement one reley to contoller the pump, a button to turn on and
off the pump, a tri led to give status of the pump. it also will read a 3 wire pressure sensor. 

The simple server at this time will accept connections but dose not do andthing with them. 

This code has only been tested with Adafruit Feather HUZZAH with ESP8266 WiFi.
*** do to memory issues I install this file with the frozen code from my project
## uasync
you need to install the __init__.py file from this project into lib/uasyncio folded

*** do to memory issues I install this file with the frozen code from my project

## Logging.py
This is required by uasync 
*** do to memory issues I install this file with the frozen code from my project

## server.py - Simple Server
So that you can access pumps and data remotely. This can be either a remote contoller or a
telnet conncection. This is all in devolpement. 


# Modules list

## Leds.py
need text

## pumps.py
This pump module controls a reley to turn on and off the pump. it has events  
### Events  
    pumpNotReadyEvent - event for pump to know it okay to start  
    pumpStartEvent - event for services that need to do some in the startup period. value is the time from epoch that startup period ends. 
    pumpCleanUpEvent - event for pumpMonitor to lauch a pump shutdown  
    pumpFinishEvent - event for services to know to supply data on registered return events  
    pumpRunningEvent - event for pumpOn for service that need to know that the pump is on  
    pumpTimeOnEvent = Event(name='Pump Time On') # client event, running time ie server  
    pumpStatusEvent = Event(name='Pump Status')  

### Register to events  
    registerMonitorEvent(event, func)         
        event - class object  
        func - handle to the function that get lauched on the event being set.
        description - This is used to create the list of things to monitor by the pump.
                      IE no flow, high pressure
        returns - nothing
    registerFinishDataEvent(event, store) 
        event - class object  
        store - name of object
        description - Used to create a list of events to collect data from sensors or
                      change states of objects.
        returns - handle to the finishDataEvent

### init items
    PowerPin - pin for turin rely on and off
    startUpTime - (20) start up period length in seconds.
    name - (Pump) the name of the object. helpful when multiple Pump items defined, repl messages, and debuging issues
    
      

## flowMeters.py
Module for running Hall effect Sensors Flow meters. Constants for Adafruit 1/2 meter, and Digiten G1 meter.
Since the sensors are hall effect sensors, you need to do IRQ interupts on the pin for the meter.
### Events
    noFlowEvent - set when pump is running and there is no flow
    flowFinishData - event for recording total liters pumped on last run. filled when finishEvent is set.
    shutoffDataReturn - dummy event for for calling pumpoff
    currentflowEvent - used to request currentflow data. set to handle of return data event
    

### Register to events
    registerFinishEvent(event)
        event - handle to forgien handle to call for data.
        description - this is used to register a event for calling for finish run data. 
        returns - flowFinishData handle
    
### init items
    flowPin - pin for reading palues.
    flowCount - (0) this is where palses are stored when resetting globle flowCount
    rate - this is the meters Flow rate pulse Characterristics (constances for supporte meters)
    name - (flowMeter) display name for flow meter object
    clicks - (450) number of clicks/pulses per litter.
    
## pressure.py
need text

## buttons.py
need text




