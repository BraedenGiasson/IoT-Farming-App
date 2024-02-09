# Welcome to IoT Farming App! :seedling:
## :bookmark_tabs: Table of Contents

- **:page_with_curl: [Project Description](#page_with_curl-project-description)**
- **:iphone: [Mobile Application](#iphone-mobile-application)**
- **:pencil2: [Application Setup](#pencil2-application-setup)**
- **:triangular_ruler: [Python Script Setup](#triangular_ruler-python-script-setup)**
- **:no_entry: [Authentication Setup](#no_entry-authentication-setup)**
- **:rocket: [Application Functionality](#rocket-application-functionality)**
- **:camera: [Application Screenshots](#camera-application-screenshots)**
  
- **:vertical_traffic_light: [Controlling Actuators](#vertical_traffic_light-controlling-actuators)**
- **:cloud: [Sensor Readings](#cloud-sensor-readings)**
- **:wrench: [Hardware & Connections](#wrench-hardware--connections)**
- **:chart_with_upwards_trend: [Potential Future Work](#chart_with_upwards_trend-potential-future-work)**
- **:key: [Contributions](#key-contributions)** 

## :page_with_curl: Project Description

The project is an **IoT product** that consists of a **container farm**, which is a stand-alone farming system for growing food inside a shipping container, and a **mobile app** which is used to view and control information about the container. The container farm, which uses the Raspberry Pi reTerminal as a computing device, has different **sensors and actuators** used for the purpose of this project.

## :iphone: Mobile Application

The mobile application has two user roles: **Farm Technician**, and **Fleet Owner**.

#### Farm Technician

The Farm Technician **manages all the plants, adjusts its environment, and monitors the growing conditions for the farming container.** 

They have access to the Temperature, Humidity, Soil Moisture, and Water Level readings inside the container. 

Additionally, a Farm Technician can control the state of the lights, fan, and door lock inside the container. 

Finally, they can view graphs of Temperature and Humidity readings of the container over a period of 24 hours.  

#### Fleet Owner

The Fleet Owner **manages multiple container farms**. 

They can view all deployed containers, monitor individual container locations, assure its proper installation, and monitor container security.

They can also view container locations through a map.

## :pencil2: Application Setup

:warning: Before setting anything up, see the **:wrench:[Hardware & Connections](#wrench-hardware--connections)** section to ensure you have all the **hardware** and **connections** needed to run the application properly. 

Upon verification that you meet the application setup prerequisites, you will need to complete the following steps.

#### Modify IoT Hub Connection Strings

These **connections strings** need to be modified inside the **'appsettings.json'** file in the app project folder. Replace the strings with the values from your own IoT Hub. 

If you have trouble locating these connection strings, refer below for how to acquire them.

```json
{
    "Settings": 
    {
      "EventHubConnectionString": "{your own EventHubConnectionString}",
      "EventHubName": "{your own EventHubName}",
      "ConsumerGroup": "{your own ConsumerGroup}",
      "StorageConnectionString": "{your own StorageConnectionString}",
      "BlobContainerName": "{your own BlobContainerName}",  
      "HubConnectionString": "{your own HubConnectionString}",
      "DeviceId": "{your own DeviceId}",
    }
}
```

#### Get *EventHubName*, *ConsumerGroup* & *EventHubConnectionString*

1. Sign in to Azure and navigate to your IoT Hub.

2. Select “Built-in endpoints” from the resource menu, under “Hub setting”.
3. Get the EventHubName from the section “Event Hub-compatible name”, under “Event Hub Details”.
4. Get the ConsumerGroup from the section “Consumer Groups”, under “Event Hub Details” (default name is $Default).
5. Get the EventHubConnectionString from the section “Event Hub compatible endpoint”.

#### Get *StorageConnectionString* & *BlobContainerName*

1. Sign in to Azure.

2. Create a storage account if not done already.

3. Navigate to your storage account.
4. Select “Access Keys” from the resource menu, under “Security + networking”.
5. Get the StorageConnectionString from under “Key1”.
6. Select “containers” from the resource menu, under “Data storage”.
7. Create a container if not done already.
8. Get the BlobContainerName, which is the name of your container.

#### Get *HubConnectionString* & *DeviceId*

1. Sign in to Azure and navigate to your IoT Hub.

2. Select “Shared access policies” from the resource menu, under “Security Settings”.
3. Select “service” as the policy name.
4. Get the HubConnectionString from the section “Primary connection string”.
5. Select “Devices” from the resource menu, under “Device management”.
6. Get the DeviceId of the device to be used (create if not already), which is the name of the device.

## :triangular_ruler: Python Script Setup

To **modify the Python script** to use a custom Azure IoT Hub, modify the **IOTHUB_DEVICE_CONNECTION_STRING** to the device connection string for your IoT Hub device. See the **.env.example** file in the **\farm** folder for how your .env file should look.

Then, in the **\farm** folder of the project in a terminal, run the following command:

```
python3 farm.py
```

\* *This command only works with python 3.*

#### Get IOTHUB_DEVICE_CONNECTION_STRING
1. Sign in to Azure and navigate to your IoT Hub.
2. Select “Devices” from the resource menu, under “Device management”.
3. Select the specific device.
4. Get the IOTHUB_DEVICE_CONNECTION_STRING from the section “Primary connection string”.

## :no_entry: Authentication Setup

The authentication process for the application is managed by **Firebase**.

In order to be **properly authenticated**, you need to modify the Firebase Configuration inside the **'ResourceStrings.cs'** file inside the **'Config'** folder of the application.

```C#
static public string AuthorizedDomain { get; set; } = "{your own Firebase Auth Domain}";
```
```C#
public static string Firebase_ApiKey { get; set; } = "{your own Firebase API Key}";
```


## :rocket: Application Functionality

The application has many functionalities in terms of views for both the **Farm Technician** and **Fleet Owner**.

### Login Page

This view **provides functionality** to log in to the application as either a Farm Technician, or a Fleet Owner. This is essential to determine what views to display next.
### Farm Technician Devices Page

This view **displays the devices** (sensors & actuators) associated with the container for the Farm Technician.

The devices include sensor readings for the container temperature & humidity, as well as the soil moisture and water levels. 

Additionally, a Farm Technician can control the state of the container light, fan, and door lock.   

### Farm Technician Charts Page

This view displays **historical data** over a 24 hour period for temperature and humidity readings for a Farm Technician container.

A **graph** is shown for a seamless, user-friendly display of the data. Additionally, a scrollable section with specific data with allocated time readings. 

### Fleet Owner Page

This view **displays a list** of the **containers** associated with the **Fleet Owner**.

The Fleet Owner can **navigate** between the **Geo-Location** and **Security** tabs, to view the address, or any issues of a specific container.

### Geo-Location Information Page

This view **displays the information** related to the **Geo-Location** subsystem of a specific container associated with a **Fleet Owner**.

The Fleet Owner can view the container **address**, as well as the readings from the **pitch, roll angle**, and **vibration** of a container. 

Additionally, the **buzzer** actuator can be controlled by the Fleet Owner.  

### Security Information Page

This view **displays the information** related to the **Security** subsystem of a specific container associated with a **Fleet Owner**.

The Fleet Owner can view the readings from the **noise level, luminosity, motion detector**, and **door** of a container. 

Additionally, the **buzzer** and **door lock** actuators can be controlled by the Fleet Owner.  

### Fleet Owner Map Page

This view **displays an interactable map containing address** markers of all the containers associated with the Fleet Owner.  

### Settings Page

This view **displays figures** that can be changed for the purpose of notifications.

The user can alter threshold limits, meaning for a specific sensor, if the reading is out of the limits of the threshold amounts, a notification will be sent to the user indicating a threshold boundary alert.


## :camera: Application Screenshots

### Login Page

<table>
  <tr>
    <td>
      <img src="assets\Login Page.png" alt="Login Page"/>
    </td>
  </tr>
</table>

### Farm Technician 
<table>
  <tr>
    <td>
      <img src="assets\Farm Technician - Devices.png" alt="Farm Technician - Devices"/>
    </td>
     <td>
      <img src="assets\Farm Technician - Graphs.png" alt="Farm Technician - Devices"/>
    </td>
  </tr>
</table>

### Fleet Owner
<table>
  <tr>
    <td>
      <img src="assets\Fleet Owner - Containers_GeoLocation.png" alt="Farm Technician - Devices"/>
    </td>
      <td>
      <img src="assets\Fleet Owner - Containers_Security.png" alt="Farm Technician - Devices"/>
    </td>
       <td>
      <img src="assets\Fleet Owner - GeoLocation.png" alt="Farm Technician - Devices"/>
    </td>
  </tr>
  <tr>
    <td>
      <img src="assets\Fleet Owner - Security.png" alt="Farm Technician - Devices"/>
    </td>
      <td>
      <img src="assets\Fleet Owner - Map.png" alt="Farm Technician - Devices"/>
    </td>
  </tr>
</table>

### Settings
<table>
  <tr>
    <td>
      <img src="assets\Settings.png" alt="Farm Technician - Devices"/>
    </td>
    <td>
      <img src="assets\Menu.png" alt="Farm Technician - Devices"/>
    </td>
  </tr>
</table>

## :vertical_traffic_light: Controlling Actuators

To control the actuators, there were 3 options, Device Twins, Direct Method, C2D Messages. 

After careful thought, we decided to choose **Device Twins**. 

We **chose this approach** because this strategy allows the IoT Hub to know if the state of the actuator was changed after requesting it, which is not available for D2C messages. Additionally, direct methods return a response to the IoT Hub, however, in this case, it is better for us to use **device twins**, as you can keep track of the current **state** of all the actuators with the **reported twin properties**, which can be helpful in tracking the state of the actuators in the application.

### Buzzer (Geo-Location)

##### Turn buzzer on
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"geolocationBuzzer": "on"}'
```
##### Turn buzzer off
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"geolocationBuzzer": "off"}'
```
### Buzzer (Security)

##### Turn buzzer on
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"securityBuzzer": "on"}'
```
##### Turn buzzer off
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"securityBuzzer": "off"}'
```
### Fan

##### Turn fan on
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsFan": "on"}'
```
##### Turn fan off
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsFan": "off"}'
```
### RGB LED Stick

##### Turn RGB LED Stick on
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsLED": "lights-on"}'
```
##### Turn RGB LED Stick off
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsLED": "lights-off"}'
```
### Micro Servo (Door Lock)

##### Turn door lock on
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"securityDoorLock": "lock"}'
```
##### Turn door lock off
```
az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"securityDoorLock": "unlock"}'
```

## :cloud: Sensor Readings

For **fast, reliable sensor readings** on Azure IoT Hub, we decided to choose **C2D Messages**.

We **chose this approach** because C2D messages are fast and can easily be sent to the Azure IoT Hub. Because the sensor doesn't need a big payload back, for example, to control an actuator, this strategy offers **seamless interaction** between the sensors and the Azure IoT Hub.

### Plant Subsystem
#### Temperature

```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"temperature":{\"value":20,\"unit":"C"\}\}\]\}
```

#### Humidity
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"humidity":{\"value":50,\"unit":"%HR"\}\}\]\}
```
#### Soil Moisture
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"soil-moisture":{\"value":20,\"unit":""\}\}\]\}
```
#### Water Level
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"water-level-sensor":{\"value":20,\"unit":"% submerged"\}\}\]\}
```
### Geo-Location Subsystem
#### Location
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"location"{\"value":"",\"unit":"loc"\}\}\]\}
```
#### Pitch
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"pitch":{\"value":10,\"unit":"°"\}\}\]\}
```
#### Roll Angle
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"roll-angle":{\"value":10,\"unit":"°"\}\}\]\}
```
#### Vibration
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"vibration":{\"value":20,\"unit":"m/s2"\}\}\]\}
```
### Security Subsystem
#### Door State
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"door":{\"value":"open",\"unit":""\}\}\]\}
```
#### Noise Level
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"noise":{\"value":100,\"unit":""\}\}\]\}
```
#### Motion Detection
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"motion":{\"value":"detected",\"unit":""\}\}\]\}
```
#### Luminosity Level
```
az iot device send-d2c-message -n {iothub_name} -d {device_id} --data {\"sensors":[\{\"luminosity":{\"value":30,\"unit":""\}\}\]\}
```


## :wrench: Hardware & Connections

For the project to **function** as intended, you will need the following **hardware**:

- **[Raspberry Pi reTerminal](https://wiki.seeedstudio.com/reTerminal/)**
- **[Raspberry Pi Power Supply](https://www.raspberrypi.com/products/type-c-power-supply/)**
- **[Grove Base Hat for Raspberry Pi](https://wiki.seeedstudio.com/Grove_Base_Hat_for_Raspberry_Pi/)**

### Grove Base Hat GPIO Connections

|                           Hardware                           |       Sensor/Actuator        | Grove Hat Pin | Bus  |      Subsystem(s)      |
| :----------------------------------------------------------: | :--------------------------: | :-----------: | :--: | :--------------------: |
| [GPS (Air530)](https://wiki.seeedstudio.com/Grove-GPS-Air530/) |         GPS Location         |     UART      |      |      Geo-Location      |
| [reTerminal’s built-in accelerometer](https://wiki.seeedstudio.com/reTerminal/) | Pitch, Roll Angle, Vibration |   Built-in    |      |      Geo-Location      |
| [reTerminal’s built-in buzzer](https://wiki.seeedstudio.com/reTerminal/) |            Buzzer            |   Built-in    |      | Geo-Location, Security |
| [AHT20 Temperature & Humidity Sensor](https://wiki.seeedstudio.com/Grove-AHT20-I2C-Industrial-Grade-Temperature&Humidity-Sensor/) |    Temperature, Humidity     |      26       | 0x38 |         Plants         |
| [Soil Moisture Sensor](https://wiki.seeedstudio.com/Grove-Capacitive_Moisture_Sensor-Corrosion-Resistant/) |        Soil Moisture         |      A2       |      |         Plants         |
| [Water Level Sensor](https://wiki.seeedstudio.com/Grove-Water-Level-Sensor/) |         Liquid Level         |      A4       |      |         Plants         |
| [RGB LED Stick](https://wiki.seeedstudio.com/Grove-RGB_LED_Stick-10-WS2813_Mini/) |             LED              |      18       |      |         Plants         |
|     [Cooling Fan](https://www.adafruit.com/product/3368)     |             Fan              |      22       |      |         Plants         |
| [PIR Motion Sensor](https://wiki.seeedstudio.com/Grove-PIR_Motion_Sensor/) |            Motion            |      D16      |      |        Security        |
| [Sound Sensor/ Noise Detector](https://wiki.seeedstudio.com/Grove-Sound_Sensor/) |            Noise             |      A0       | 0x04 |        Security        |
| [Magnetic door sensor reed switch](https://wiki.seeedstudio.com/Grove-Magnetic_Switch/) |             Door             |      D5       |      |        Security        |
| [MG90S 180° Micro Servo](https://wiki.seeedstudio.com/Grove-Servo/) |          Door Lock           |      PWM      |      |        Security        |
| [reTerminal's built in light sensor](https://wiki.seeedstudio.com/reTerminal-hardware-interfaces-usage/) |          Luminosity          |   Built-in    |      |        Security        |

## :chart_with_upwards_trend: Potential Future Work

- Store the UUID and permission level on a database (ex: Firebase).
- Ability to sign up to the application.
- Implementation of dark and light mode theme.
- Expand 24 data to soil moisture and water levels.
- If connection is lost to the IoT Hub, a notification will appear on the application until re-connection.

## :key: Contributions 

| **[Braeden Giasson](https://www.linkedin.com/in/braedengiasson/)** | **[Sarim Syed](https://www.linkedin.com/in/sarim-syed-1010b3211/)** | **[Payal Rathod](https://github.com/Payal-Rathod)**          |
| ------------------------------------------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Braeden worked on the Geo-Location subsystem. He was primarily responsible for the frontend of the application, designing and coding the UI of the pages. He also helped when needed on the backend of the application, and coded the geo-location portion of the Python script. | Sarim worked on the Plant subsystem. He was primarily responsible for the functionality and coding the Python script, while also contributing to the UI for the plant subsystem. | Payal worked on the Security subsystem. She was primarily responsible for the functionality of the backend of the application, while also contributing to the frontend of the application, and and coded the security portion of the Python script. |
































# Grove Base Pi Hat Pins



|    Sensor     | Actuator  |     Grove Hat Pin      |  Subsystem   |
| :-----------: | :-------: | :--------------------: | :----------: |
| GPS (Air530)  |           |          UART          | Geo-Location |
|     Pitch     |           | Built-in accelerometer | Geo-Location |
|  Roll Angle   |           | Built-in accelerometer | Geo-Location |
|   Vibration   |           | Built-in accelerometer | Geo-Location |
|               |  Buzzer   | Built-in accelerometer | Geo-Location |
|   Humidity    |           |           26           |    Plants    |
|  Temperature  |           |           26           |    Plants    |
| Soil Moisture |           |           A2           |    Plants    |
| Liquid Level  |           |           A4           |    Plants    |
|               |    LED    |           18           |    Plants    |
|               |    Fan    |           22           |    Plants    |
|  Luminosity   |           | Built-in Raspberry Pi  |   Security   |
|    Motion     |           |          D16           |   Security   |
|     Noise     |           |           A0           |   Security   |
|     Door      |           |           D5           |   Security   |
|               | Door Lock |          PWM           |   Security   |
|               |  Buzzer   | Built-in accelerometer |   Security   |

# Controlling Actuators
## Fan

- Communication Strategy
    - Device Twins
    - Device twin was used for this actuator because this strategy allows the IoT Hub to know if the state of the actuator was changed after requesting it which is not available for D2C messages. Direct methods returns a response to the IoT Hub however,       it is better to use device twins since you can keep track of the current state of all the actuator with the reported twin properties which can be helpful in tracking the state of the actuators in the app.
- Code snippet
  - Set plantsFan to on
      ```
      az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsFan": "on"}'
      ```
   - Set plantsFan to off
      ```
      az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsFan": "off"}'
      ```

## Light

- Communication Strategy
    - Device Twins
    - Device twin was used for this actuator because this strategy allows the IoT Hub to know if the state of the actuator was changed after requesting it which is not available for D2C messages. Direct methods returns a response to the IoT Hub however, it is better to use device twins since you can keep track of the current state of all the actuator with the reported twin properties which can be helpful in tracking the state of the actuators in the app.
- Code snippet
  - Set plantsLED to on
      ```
      az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsLED": "lights-on"}'
      ```
   - Set plantsLED to off
      ```
      az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"plantsLED": "lights-off"}'
      ```

## Buzzer (Geo-Location & Security)

- Communication Strategy

  - Device Twins
  - Device twin was used for this actuator because this strategy allows the IoT Hub to know if the state of the actuator was changed after requesting it which is not available for D2C messages. Direct methods returns a response to the IoT Hub however, it is better to use device twins since you can keep track of the current state of all the actuator with the reported twin properties which can be helpful in tracking the state of the actuators in the app.

- Code snippet
  - Set geolocationBuzzer to on

    ```
    az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"geolocationBuzzer": "on"}'
    ```

  - Set geolocationBuzzer to off

    ```
    az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"geolocationBuzzer": "off"}'
    ```

    

## Door Lock

- Communication Strategy
  - Device Twins
  - Device twin was used for this actuator because this strategy allows the IoT Hub to know if the state of the actuator was changed after requesting it which is not available for D2C messages. Direct methods returns a response to the IoT Hub however, it is better to use device twins since you can keep track of the current state of all the actuator with the reported twin properties which can be helpful in tracking the state of the actuators in the app.
  
- Code snippet

  - Set securityDoorLock to lock
    ```
    az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"securityDoorLock": "lock"}'
    ```

  - Set securityDoorLock to unlock

    ```
    az iot hub device-twin update -n {iothub_name} -d {device_id} --desired '{"securityDoorLock": "unlock"}'
    ```

    

