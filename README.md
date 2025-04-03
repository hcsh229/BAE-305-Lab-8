# BAE-305-Lab-8
# There’s an App for That

Group members: Heath Shewmaker and Terence Redford

Date Submitted: 4/2/25

# Introduction:
This lab introduced the fundamentals of app development by enabling us to build a mobile app capable of controlling a robot car via serial and Bluetooth communication. The aim was to learn how to develop an Android app, using MIT App Inventor, that interfaces with an Arduino-powered robot to execute motion commands. The app would act as a remote control, replacing traditional USB communication with a wireless solution. This exercise combined elements of mobile interface design, embedded systems, and hardware-software integration. Through the development and testing of the app, we gained insights into practical Bluetooth communication and the nature of software testing in physical applications.
# Methods:
Step 1:
The robot car was assembled using the breadboard and base, motors, and motor driver circuit from prior lab work. Wheels were attached to the DC motors, which were placed on the bottom of the breadboard base to emulate a car. The breadboard was connected to the computer through USB, and a battery pack was also installed for later use. The circuit can be seen below, but do not worry about the ultrasonic sensor as it is not needed for this lab. 
 
 ![image](https://github.com/user-attachments/assets/95c9d8fc-dbcb-4115-b8f4-fc2703c7feeb)

Figure 1: Robot Car circuit constructed

Step 2: 
Using MIT App Inventor, we created a user interface consisting of directional buttons (Forward, Backward, Left, Right) and a speed control interface. An .apk file was generated and installed on an Android phone using the provided QR code. The app was tested initially over serial communication, via USB. Make sure to have your code/app tested by the lab instructor at this point. 
 
 ![image](https://github.com/user-attachments/assets/f9cef1fd-080e-482c-8562-6c37f11ad94f)

Figure 2: App Inventor blocks given in lab manual, needed for serial communication

Step 3:
Before moving on it was important to verify that the App commands that were being sent from the code were being correctly received and interpreted by the code. We verified this by using a direct connection from the mobile device to the RedBoard and testing the buttons.

Step 4: 
The HC-05 Bluetooth module was connected on a breadboard using a 1kΩ-2kΩ voltage divider for the TX-RX line to step the voltage down from 5V to 3.3V. Pins 2 and 3 on the Arduino were designated as SoftwareSerial RX and TX. 

 ![image](https://github.com/user-attachments/assets/28cab096-b4cc-4045-b620-c6d9f2959174)

Figure 3: HC-05 BT Module Wiring Diagram (http://exploreembedded.com/wiki/Setting_up_Bluetooth_HC-05_with_Arduino)

Step 5: 
Following this, additions were made to the arduino program which facilitated the Bluetooth connection. These additions, seen below, are initializations of the serial Bluetooth connection, and associated variables and arrays that will be used to facilitate data handling. 

![image](https://github.com/user-attachments/assets/f2704802-ae51-4eb3-8f02-dc955cb811c2)

Figure 4: Given code for BT which can be found in Lab manual

Step 6: 
This block of code in the main loop of the program calls two user-made functions, called “recvWithEndMarker” and “parseData”, in order to read in the data sent from the android app Bluetooth connection, and then to translate that data into the variables our code expects to handle.

 ![image](https://github.com/user-attachments/assets/c32b23d8-66b5-4fc4-9116-66f52e11f09d)

Figure 5: Sample code, for receiving data from the app, given in Lab manual

Step 7: 
The two user-made functions that can be seen in the code below are used to ensure that the Bluetooth data transfer is handled correctly. The function “recvWithEndMardker” listens for the “\n” character to decide when a message from the Bluetooth connection has concluded. Until it receives this character, it continues to save each incoming character into an array. If the characters exceed the array length, the oldest received character is discarded. If the end character is received, it then signals this by changing the “newData” variable to true, indicating that the data received can now be used. Finally,“parseData” ensures that the character data received is correctly formatted to be used elsewhere in the code.

  ![image](https://github.com/user-attachments/assets/aab430ff-8751-493b-85d9-61838fac6e9e)

Figure 6: Sample code given in Lab manual

Step 8: 
Once the HC-05 Bluetooth chip is correctly wired it was necessary to modify the application to support Bluetooth connection instead of serial connection.  This required us to add a Bluetooth Client object to the application, as well as a list picker object which will contain the options for available Bluetooth devices for the user to pick from. It was also necessary to install an extension called “GetApiLevel”, which allowed us to add the GetApiLevel object, which would allow us to refine how the Bluetooth is handled.
The associated code blocks we used to achieve this can be seen below:

![image](https://github.com/user-attachments/assets/6d9b55e1-7e64-420c-945c-b38633d7cc93)

Figure 7: App Inventor blocks given in lab manual, needed for BT communication

In order to get this to function correctly it was also necessary to slightly modify the blocks that were present in each of the buttons which would send the corresponding input to the RedBoard code to drive the motors in the desired way. The changes were identical for each of the buttons, so only one block is included to display the changes below:

![image](https://github.com/user-attachments/assets/09c1e82b-dd8a-4f4d-80ca-4a599709abc9)

Figure 8: App inventor blocks modified to send Bluetooth signals

Step 9: To verify the function of the application we used an android device to connect to the HC-05 using the app and tested the functionality of each of the buttons.
# Results:
Steps 1-4: 
We reused an Arduino sketch from a previous project which allowed serial control of the robot. The program defined motor pins, accepted serial character input, and translated it into directional movement commands (‘f’ for forward, ‘b’ for backward). Motor speed and direction were set through digital writes and PWM signals.

Step 5-9:
When we tested the application we developed, we were unable to use the app to run the motors. However, we later identified the issue in our Arduino code, where we had initialized the Tx and Rx pins incorrectly. We had initialized the Tx and Rx pins on the RedBoard in such a way that the Tx of the RedBoard connected to the Tx of the HC-05. This explains why even though we could get the direct connection to drive the motors, we couldn’t receive the correct data from Bluetooth. Despite this, the application functioned as desired, connecting to the HC-05 and displaying the connection status. The user interface we designed was simple and focused on including all the functions as simply as possible. We added buttons with names representing the response that the car should have when they were pressed. The “BT connection?” label is a marker displaying whether a successful Bluetooth connection was made, and would turn green if this happened. The final design can be seen below:

![image](https://github.com/user-attachments/assets/cf475d76-0998-4f68-9606-fd12f5c1f7bd)

Figure 9: Android application user interface design

# Discussion:
Part 1: In Lab 6 we found out what was the minimum speed that will move the motors. What is the minimum speed that will move the complete car?
-	When testing this, we were seeing that our car wouldn’t move at any motor speed under 125. This is reflected in our code. However, after further testing we realized that we were not using the binder clip in the most efficient orientation. After fixing this, we found that the minimum motor speed to move the car is closer to about 100.
***Make sure to put your binder clip in a position that creates the least amount of contact area***
# Conclusion:
This lab introduced us to mobile app development as a tool for controlling embedded systems through the construction of an Android app used to operate a robot car via serial and Bluetooth communication. We successfully reused a previously developed Arduino sketch to control the robot through serial commands. The robot responded accurately to character inputs from the Serial Monitor and with a direct connection. We were able to establish a Bluetooth connection between the app and the robot using the HC-05 module, however we were unable to get the Bluetooth module to communicate with the RedBoard. We discovered our error and realized that it is critical to note that, when wiring Tx and Rx connections, they represent transmission and reception terminals. Therefore, the transmission terminal from one component must be connected to the receiving terminal of the other component, and vice versa. This is why we could not get our HC-05 to communicate with our RedBoard, despite being able to establish the Bluetooth connection with our mobile device and verifying the function of the app with the RedBoard directly. Nonetheless, this lab provided valuable insight into system integration, debugging wireless communication, and the importance of aligning code logic across both hardware and software interfaces.

