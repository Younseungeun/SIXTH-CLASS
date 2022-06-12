# SIXTH-CLASS

## TOPIC
- PM sensor
### CODE
1. #include <PMsensor.h>          //PM sensor header file
2. #include <Wire.h>              
3. #include <LiquidCrystal_I2C.h>   //LCD header file
4. PMsensor PM;
5. LiquidCrystal_I2C lcd(0x27,16,2);  
6. int redPin = 3;    //connect RGB's red led data to port 3 (left of longest one)
7. int greenPin = 9   //connect RGB green led data to port 9 (right of longest one)
8. int bluePin = 5;      //connect RGB's blue led data to port 5 (the other one)
9. void setup() {
10. lcd.init();                      
11. lcd.backlight();                   
12. Serial.begin(9600);                //start serial
13. pinMode(redPin, OUTPUT);          //I'll use the red pin as the result
14. pinMode(greenPin, OUTPUT);        //I'll use the green pin as the result 
15. pinMode(bluePin, OUTPUT);         //I'll use the blue pin as the result
16. /////(infrared LED pin, sensor pin)  /////infrared sensor. Fine dust sensor pin
17. PM.init(2, A0);                             //I'll get the infrared sensor data on port 2, and I'll get the fine dust sensor pin as A0
18. }
19. void loop() {
20. Serial.println("=================================");         // mark ================================= on the cereal
21. Serial.println("Read PM2.5");                                  //pm 2.5 dust size is 2.5 micro size
22. float filter_Data = PM.read(0.1);         
23. //float=real, filter value is the new sensor value multiplied by 0.1 and the nofilter value before the finely accepted nofilter value multiplied by 0.9 (you can increase or decrease accuracy by changing the number 0.1)
24. float noFilter_Data = PM.read();
25. //the nofilter value is read from the pm sensor (fine dust sensor), so accuracy can be increased or reduced)
26. Serial.print("Filter : ");         //enter "Filter:" in the serial
27. Serial.println(filter_Data);        //Enter filter value in serial
28. Serial.print("noFilter : ");           //enter "noFilter:" in the seria
29. Serial.println(noFilter_Data);         //Enter noFilter_Data value in serial
30. lcd.setCursor(2,0);                        // LCD coordinates (2,0) 
31. lcd.print("filter:");                     //Enter "filter:
32. lcd.setCursor(10,0);                       // LCD coordinates (10,0)
33. lcd.print(filter_Data);                    //filter_Data값 입력
34. lcd.setCursor(0,1);                           // LCD coordinates (0,1)
35. lcd.print("nofilter:");                              //Enter "nofilter:"
36. lcd.setCursor(10,1);                                // LCD coordinates (10,1)
37. lcd.print(noFilter_Data);                               /Enter noFilter_Data
38.  if (filter_Data<=30)                                     //If the filter value is less than 30,
39.  { setColor(0,0,255); delay(100);}                         //blue LED
40.  if  (filter_Data<=80 && filter_Data>30 )                  
41.  //If the filter value is greater than 30 seconds and less than 80 (initially 30<filter_Data<=80), the conditional statement was written, but the combination of blue and green was questionable. Then, the problem was solved using && by using the concept of "logical operator" learned in information time. I also told other friends.
42.   { setColor(0,255,0); delay(100);}               //green LED
43.   if  (filter_Data>80)                    //If the filter value is greater than 80
44.   { setColor(255,0,0);  delay(100);}          //red LED
45.   delay(1000);                                   //1000 microsecond delay
46.   }
47.   void setColor(int red, int green, int blue)     //specify the set color function (RGB led can control the intensity of light by utilizing analog signals rather than digital signals that are 0 and 1 on and off)
48.   {
49.   analogWrite(redPin, red);               //the value of the red pin as the red value
50.   analogWrite(greenPin, green);           //the value of the green pin as the green value
51.    analogWrite(bluePin, blue);             //the value of the blue pin as the blue value
52.    }
