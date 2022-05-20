# Light Monitoring system using Z-score Analysis
This project was inspired by BOLT IOT, IOT and ML course.
## Story behind this project:
To learn the nitty gritty details about z score analysis and learn how to integrate integromat and twilio services.

## Things used in the project
### Hardware Components
-  Bolt IOT module
- Light Dependent resistor(LDR)
- Resistor(10K ohm)
- Connecting wires



### Software and Online services

| Twilio  |  Execution |
| ------------ | ------------ |
|![](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fmobilemarketingmagazine.com%2Fwp-content%2Fuploads%2Ftwilio.jpg&f=1&nofb=1) | ![WhatsApp Image 2022-05-20 at 6 00 51 PM (1)](https://user-images.githubusercontent.com/75200693/169530378-5c901d9a-06a6-41af-ade8-9a8147770ee3.jpeg)|

|Integromat   | Execution   |
| ------------ | ------------ |
|![image](https://user-images.githubusercontent.com/75200693/169534091-bce487e5-1186-4686-abe2-fa1edf9cd476.png)   |![int](https://user-images.githubusercontent.com/75200693/169575207-df186f45-3ab2-4ae7-bf22-21c35be50ab2.PNG)   |
|![wh](https://user-images.githubusercontent.com/75200693/169534704-5ed8d4f9-3011-4f1d-925b-d58ae2143db7.PNG)   |![wh2](https://user-images.githubusercontent.com/75200693/169534721-e76ded45-ad96-4965-85bf-bc4a7dfd736b.PNG)   

## Hardware setup

1. Connect the Ground pin and A0 pin of Bolt module to resistor.
2. Next connect the A0 pin and 3V pin to LDR

| image 1  | image 2  |
| ------------ | ------------ |
|![WhatsApp Image 2022-05-20 at 10 13 47 PM](https://user-images.githubusercontent.com/75200693/169574501-93e31887-c98c-49c7-9957-b079228d1c91.jpeg)   | ![WhatsApp Image 2022-05-20 at 10 13 46 PM](https://user-images.githubusercontent.com/75200693/169574506-5d752306-e17b-4711-b464-236619f9edca.jpeg)  |

## Software Programming

Main features of the code:
1) Importing the requierd libraries
2) Calculating Z factor, Lower and Upper bounds.

``` .py
import conf, json, time, math, statistics
from boltiot import Sms, Bolt

def compute_bounds(history_data,frame_size,factor):
    if len(history_data)<frame_size :
        return None

    if len(history_data)>frame_size :
        del history_data[0:len(history_data)-frame_size]
    Mn=statistics.mean(history_data)
    Variance=0
    for data in history_data :
        Variance += math.pow((data-Mn),2)
    Zn = factor * math.sqrt(Variance / frame_size)
    High_bound = history_data[frame_size-1]+Zn
    Low_bound = history_data[frame_size-1]-Zn
    return [High_bound,Low_bound]`
 ```
## Conclusion

This project allows us to dynamically set the threshold points to detect any anamoly and to send alert messages to us using different online services.

### Theory

What is Z Score analysis?
Theoretical answer: A z-score describes the position of a raw score in terms of its distance from the mean, when measured in standard deviation units. The z-score is positive if the value lies above the mean, and negative if it lies below the mean.

Basically Z score is a value that defines a dynamic threshold for a data set. If the data set has some anamoly it will detect the unusual behaviour easily.

In the below picture Mn is mean Vi are the values of data set. r is the frame size and c is multiplication factor
 Tn is the z score for ith value V.
Frame Size: These are the number of previous data points that will be used to predict the trend of the data.

 
![z](https://user-images.githubusercontent.com/75200693/169545511-9cc3c34d-9a9a-4bff-80d4-8307eec3716c.PNG)

So how does this system help us know if someone turned on or off the lights?

Well throughout the day, the light in the room will change with the rising and setting of the sun. This change will be slow, and the bounds for the system will change to match this change. But when someone turns on or turns off the lights in the room, the intensity of light in the room will change suddenly. Because of this, the system will detect an anomaly and quickly alert you that someone is in your room.


#### Output

|Collecting data to calculate initial Z value   | When light intensity changes suddenly   |
| ------------ | ------------ |
|![WhatsApp Image 2022-05-20 at 10 23 33 PM](https://user-images.githubusercontent.com/75200693/169576033-0e8f9ec6-c1ae-4b1b-aa8b-a19e5c21a9b1.jpeg)   | ![WhatsApp Image 2022-05-20 at 10 22 52 PM](https://user-images.githubusercontent.com/75200693/169576065-1f2237e9-5f0b-4cad-8a2d-0ddaf35769d4.jpeg)  |
