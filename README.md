# AppAdapter
Official Library for supporting App control of the HTL Robot

## How to use AppAdapter
  This library is written for the Arduino Framework. It works as an Interface for the [CircleController](https://github.com/codingmiracle/circleController) App.  
       

## Setup
clone the Repository into the `/lib` folder of your PlatformIO Project:
```
git clone https://github.com/codingmiracle/AppAdapter.git
```
Open the `main.c` or `main.cpp` file in your code editor and include the library:
```
#include "../lib/AppAdapter/AppAdapter.h"
```
Note: there are many ways to include the Library so use the method, which works for you best   
    
## Usage
You can use the AppAdapter similar to the BluetoothSerial Library:  
  
```cpp
#include <Arduino.h>
#include "../lib/AppAdapter/AppAdapter.h"

AppAdapter appAdapter;

void setup() {
    Serial.begin(115200);
    appAdapter.begin("HTL-Robot");
    Serial.println("The Roboter started, now you can pair it with bluetooth!");
}

void loop() {
    if (appAdapter.dataAvailable()) {
        if(appAdapter.isMsg()) {
            Serial.println(appAdapter.readMsg());
        } else if(appAdapter.isSpeed()) {
            Serial.println(appAdapter.readSpeed().toString());
        } else if(appAdapter.isColor()) {
            CRGB col = appAdapter.readColor();
            Serial.printf("r:%d g:%d b:%d\n",col.r, col.g, col.b);
        } else {
            Serial.println("Unknown Object: " + String(appAdapter.readBuffer()));
            appAdapter.finish_data_handling();
        }
    }
    if (Serial.available()) {
        appAdapter.write(Serial.read());
    }
}
```
In this Demo Program the data received will be logged in the Serial Monitor.    
   
* With `appAdapter.dataAvailable()` we check if there is any new data from the app.
* With the `appAdapter.is...()` methods you can chech the type of data, which can be a Speed, Message or Color.  
Notice that currently only the Speed data is supported by the App, but more will come soon.   
* Finally you can get the data with the `appAdapter.read...()` methods, which automaticaly will finish the handling of the data. If you don't always end with one of these methods after checking for available data, the data won't be refreshed and you will delay the communication.    
In such a case call `appAdapter.finish_data_handling()` to reset specificly.


## Contribution & Issues
Check [CONTRIBUTING.md](CONTRIBUTING.md) to know proper contribution guidelines.   
For Issues also check out **2. Issues** of the Contribution guidelines and stick to the **5. coding ethics**
