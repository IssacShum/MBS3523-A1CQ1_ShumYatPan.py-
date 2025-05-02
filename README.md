Paste your Python code here: 

import serial 
import cv2 
import time 
 
# Set up serial connection (adjust 'COM8' to your Arduino port) 
arduino = serial.Serial('COM6', 9600) 
time.sleep(2)  # Wait for the connection to establish 
 
# Set up webcam 
cap = cv2.VideoCapture(0) 
 
while True: 
    # Read temperature and humidity from Arduino 
    if arduino.in_waiting > 0: 
        data = arduino.readline().decode('utf-8').strip() 
        print(data) 
 
    # Capture frame from webcam 
    ret, frame = cap.read() 
    if not ret: 
        break 
 
    # Display the sensor readings on the frame 
    cv2.putText(frame, data, (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 0, 0), 2) 
    cv2.imshow('Webcam Stream', frame) 
 
    # Break the loop on 'q' key press 
    if cv2.waitKey(1) & 0xFF == ord('q'): 
        break 
 
cap.release() 
cv2.destroyAllWindows() 






Paste your Arduino code here: 

 

#include <DHT.h> 

 

#define DHTPIN 2     // Pin where the DHT22 is connected 

#define DHTTYPE DHT22   // Define the type of sensor 

 

DHT dht(DHTPIN, DHTTYPE); 

 

void setup() { 

  Serial.begin(9600); 

  dht.begin(); 

} 

 

void loop() { 

  delay(2000); // Wait a few seconds between measurements 

 

  float temperature = dht.readTemperature(); 

  float humidity = dht.readHumidity(); 

 

  if (isnan(temperature) || isnan(humidity)) { 

    Serial.println("Failed to read from DHT sensor!"); 

    return; 

  } 

 

  Serial.print(" Indoor temperature degree is: "); 

  Serial.print(temperature); 

  Serial.print(" C, Humidity: "); 

  Serial.print(humidity); 

  Serial.println(" %"); 

} 

 
