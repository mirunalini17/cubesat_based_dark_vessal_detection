# CubeSat-Based Dark Vessel Detection System

# Overview
This project presents a prototype of a CubeSat-Based Dark Vessel Detection System designed to identify vessels that stop transmitting their status signals 
while operating at sea.Illegal fishing, unauthorized maritime activities, vessel hijacking, and sinking incidents often involve the intentional or accidental 
loss of communication with monitoring authorities.Traditional geofence-based monitoring systems only generate alerts when a vessel crosses predefined 
boundaries and may not provide continuous real-time communication between vessels and shore stations.To address this limitation, our system introduces a 
heartbeat-based vessel monitoring mechanism using a CubeSat communication concept.

# Problem Statement
Maritime monitoring systems like Geofence alert systems primarily depend on AIS (Automatic Identification System) Geofencing and Radar surveillance.
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

# Hardware Components

# Vessel Node

Arduino UNO
NRF24L01 Transceiver Module (act as TX)

Function:

Periodically sends heartbeat packets.

# CubeSat Node

Arduino UNO
NRF24L01 Module X 2 (one act as Tx and one act as Rx)
MPU6050 Gyroscope + Accelerometer

Functions:

Receives heartbeat packets.
Monitors communication timeout.
Detects dark vessel condition.
Sends status to ground station.
Measures CubeSat orientation (Roll, Pitch, Yaw).

# Ground Station

Arduino UNO
NRF24L01 Module

Function:

Receives vessel status messages from CubeSat.

# Working Principle

      The proposed CubeSat-Based Dark Vessel Detection System operates using a heartbeat communication mechanism between a vessel, a CubeSat, and a ground 
      station. The vessel node, equipped with an Arduino UNO and an NRF24L01 transceiver, continuously transmits heartbeat signals at regular intervals. These 
      heartbeat messages indicate that the vessel is active and operating normally. The CubeSat node receives these signals through its NRF24L01 module and 
      verifies the presence of the vessel. As long as heartbeat packets are received within the expected time period, the CubeSat forwards a status message 
      such as "VESSEL ALIVE" to the ground station, providing continuous monitoring of vessel activity. Additionally, the CubeSat prototype uses an MPU6050 
      sensor to determine its orientation parameters, including roll, pitch, and yaw, simulating the attitude monitoring function of an actual satellite.

      If the CubeSat does not receive heartbeat signals from a vessel(means microcontroller at vessal side is switched off) for a predefined timeout period, it 
      assumes that communication with the vessel has been lost. This loss of communication may be due to illegal activities such as unauthorized fishing, 
      intentional disabling of the transmitter, equipment failure, or even a vessel sinking incident. In such cases, the CubeSat classifies the vessel as a 
      Dark Vessel and immediately transmits an alert message to the ground station. The ground station then displays a warning indicating that the vessel is no 
      longer transmitting its heartbeat signal and may require further investigation. By continuously monitoring heartbeat transmissions rather than relying 
      solely on geofence violations, the proposed system enables near real-time detection of dark vessels and improves maritime surveillance, safety, and 
      security.

# Circuit Connections
# Vessel Node

NRF24L01 → Arduino UNO
NRF24L01	Arduino UNO
VCC	        3.3V
GND	        GND
CE	        D9
CSN	        D10
SCK	        D13
MOSI	    D11
MISO	    D12

# CubeSat Node
NRF24L01 → Arduino UNO
NRF24L01	Arduino UNO
VCC	        3.3V
GND	        GND
CE	        D9
CSN	        D10
SCK	        D13
MOSI	    D11
MISO	    D12

MPU6050 → Arduino UNO
MPU6050	Arduino UNO
VCC	    5V
GND	    GND
SDA	    A4
SCL	    A5

# Ground Station
NRF24L01 → Arduino UNO
NRF24L01	Arduino UNO
VCC	        3.3V
GND	        GND
CE	        D9
CSN	        D10
SCK	        D13
MOSI	    D11
MISO	    D12

# Example Outputs
# Normal Condition

Ground Station:

CubeSat Status

Heartbeat Received
VESSEL ALIVE

Roll  : 2.8
Pitch : 1.3
Yaw   : 32.4
Dark Vessel Condition

# Ground Station:

WARNING

No heartbeat received

ALERT: DARK VESSEL DETECTED

#  Future Enhancements
Long-range LoRa communication.
AIS integration.
GPS-enabled vessel localization.
Multiple vessel monitoring.
Machine learning-based anomaly detection.
Actual CubeSat deployment.
Cloud-based maritime dashboard.

# Conclusion
This project demonstrates a CubeSat-inspired maritime monitoring system capable of detecting dark vessels through heartbeat signal monitoring.
By continuously verifying vessel activity and generating alerts when communication is lost, the system provides a low-cost and scalable approach for enhancing 
maritime safety, surveillance, and illegal activity detection.




