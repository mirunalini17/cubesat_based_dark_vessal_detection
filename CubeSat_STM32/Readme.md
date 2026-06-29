# CubeSat-Based Dark Vessel Detection System - STM32

# Overview
This project presents a prototype of a CubeSat-Based Dark Vessel Detection System designed to identify vessels that stop transmitting their 
status signals 
while operating at sea.Illegal fishing, unauthorized maritime activities, vessel hijacking, and sinking incidents often involve the intentional 
or accidental 
loss of communication with monitoring authorities.Traditional geofence-based monitoring systems only generate alerts when a vessel crosses 
predefined 
boundaries and may not provide continuous real-time communication between vessels and shore stations.To address this limitation, our system 
introduces a 
heartbeat-based vessel monitoring mechanism using a CubeSat communication concept.

# Problem Statement
Maritime monitoring systems like Geofence alert systems primarily depend on AIS (Automatic Identification System) Geofencing and Radar 
surveillance.
Here the Limitations are 
            AIS can be manually switched OFF.
            Geofence alerts only trigger after boundary violations.
            Continuous vessel-to-shore communication is not always available.
            Sinking vessels may lose communication completely.

As a result, vessels engaged in illegal activities may become invisible to authorities.

# Proposed Solution
A heartbeat-based CubeSat monitoring system where:

Vessel continuously transmits heartbeat packets.
CubeSat receives heartbeat packets.
CubeSat forwards vessel status to ground station.
Missing heartbeat packets indicate a dark vessel event.
Ground station receives real-time alerts.

# Hardware Used  

STM32 Blue pill STM32F103C8T6
Arduino UNO	
MPU6050 IMU Sensor	
nRF24L01 Wireless Module	

# Connections 

# Arduino and nrf module (vessal and ground station)

nRF24L01	Arduino UNO
VCC	        3.3V
GND	        GND
CE	        D9
CSN	        D10
SCK	        D13
MOSI	    D11
MISO	    D12

# STM32 to nRF module

Note : Configure I2C and SPI and USART
where nrf module uses SPI 

nRF24L01	STM32
VCC	        3.3V
GND	        GND
CE	        PA0
CSN	        PA4
SCK	        SPI2_SCK
MOSI	    SPI2_MOSI
MISO	    SPI2_MISO

MPU6050 use I2C

MPU6050	     STM32
VCC	        3.3V
GND	        GND
SDA	I       2C SDA
SCL	        I2C SCL

# Communication address 
vessel to satellite         00001
satellite to ground station 00002

# Working 
The proposed system is designed as a three-node communication architecture consisting of a Vessel Node (Arduino UNO), a Satellite Node (STM32 
with MPU6050), and a Ground Station (Arduino UNO), all interconnected using nRF24L01 wireless transceivers. Initially, the Vessel Node 
continuously transmits heartbeat packets (e.g., HB:1, HB:2, etc.) to the satellite using the wireless address 00001. These heartbeat packets 
represent the presence of an active vessel within the communication range. The STM32-based satellite configures its nRF24L01 module in Receive 
Mode and continuously listens for these heartbeat packets over a fixed observation period of 5 seconds. During this interval, every received 
heartbeat packet is processed, and if at least one valid packet is detected, the satellite identifies the target as a Vessel Detected; 
otherwise, after the timeout expires without receiving any heartbeat, it classifies the target as a Dark Vessel Detected, indicating the 
absence of communication from the vessel.

Once the vessel status has been determined, the satellite temporarily stops wireless reception and proceeds to acquire orientation data from 
the onboard MPU6050 Inertial Measurement Unit (IMU) through the I²C interface. The accelerometer readings are used to calculate the Pitch and 
Roll angles using trigonometric relationships, while the gyroscope's Z-axis angular velocity is integrated over time to estimate the Yaw angle. 
These three orientation parameters represent the instantaneous attitude of the satellite and provide valuable telemetry information. After 
completing the calculations, the STM32 combines the vessel detection result and the computed orientation values into a formatted telemetry 
packet containing the vessel status, pitch, roll, and yaw values.

The satellite then switches its nRF24L01 module from Receive Mode to Transmit Mode by reconfiguring the radio parameters and changing the 
communication address from 00001 to 00002, which is dedicated to communication with the Ground Station. The formatted telemetry packet is 
transmitted wirelessly to the Ground Station through the nRF24L01 module. Once the transmission is completed successfully, the STM32 clears the 
radio status flags, flushes the communication buffers if required, and reinitializes the nRF24L01 module back into Receive Mode to begin 
monitoring the vessel again. This receive–process–transmit cycle repeats continuously, allowing the satellite to periodically monitor vessel 
activity while simultaneously forwarding processed telemetry.

At the Ground Station, the Arduino UNO continuously operates in Receive Mode on address 00002, waiting for telemetry packets from the 
satellite. Upon receiving a packet, it extracts the transmitted information, including the vessel status, pitch, roll, and yaw values, using 
string parsing techniques. The decoded information is then displayed on the Serial Monitor in a user-friendly format, enabling the operator to 
observe the vessel's communication status and the satellite's orientation in real time. If the received telemetry indicates a valid heartbeat, 
the Ground Station displays "Vessel Detected"; otherwise, it reports "Dark Vessel Detected", along with the corresponding orientation values.

Overall, the system operates as a complete wireless telemetry chain, where the Arduino-based Vessel Node periodically transmits heartbeat 
signals, the STM32-based Satellite Node receives the heartbeat, determines vessel presence, calculates its onboard orientation using the 
MPU6050 sensor, packages all the information into a telemetry frame, and forwards it to the Arduino-based Ground Station. This architecture 
demonstrates reliable multi-node wireless communication, onboard data processing, real-time attitude estimation, and telemetry transmission, 
making it a practical prototype for CubeSat-inspired maritime monitoring and dark vessel detection applications.

# Project Features

    Real-time vessel heartbeat monitoring

    Dark vessel detection

    Dual-mode nRF24L01 operation (RX/TX)

    Satellite orientation estimation

    Wireless telemetry transmission

    Multi-node communication

    Low-cost CubeSat-inspired architecture

    Reliable packet forwarding using dedicated RF addresses

# Technologies Used

    STM32CubeIDE

    Embedded C

    Arduino IDE

    SPI Communication

    I²C Communication

    nRF24L01 RF Communication

    MPU6050 IMU

    Serial Communication

# Future Enhancements

    Integrate GPS on the vessel node for real-time location tracking.

    Replace the simulated heartbeat with actual sensor data from a marine vessel.

    Add bidirectional communication to allow commands from the ground station to the satellite or vessel.

    Implement packet acknowledgment and retransmission for improved reliability.

    Incorporate data logging on an SD card or cloud platform for mission analysis.

    Develop a graphical ground station interface to visualize telemetry in real time.

    Apply sensor fusion algorithms such as a complementary or Kalman filter for more accurate orientation estimation.

    Introduce encryption and authentication to secure wireless communication.

    Expand the architecture to support multiple vessel nodes communicating through a single satellite.

    Integrate long-range communication technologies (e.g., LoRa or satellite modems) for extended operational coverage.

# Conclusion

This project successfully demonstrates a CubeSat-inspired wireless communication system capable of detecting vessel activity, computing onboard 
orientation, and transmitting telemetry to a ground station. By combining an Arduino-based vessel node, an STM32 satellite controller with 
MPU6050, and an Arduino-based ground station, the system illustrates a practical implementation of embedded systems, wireless networking, and 
telemetry exchange. The modular design provides a strong foundation for future research and development in autonomous maritime monitoring and 
small satellite communication systems.
