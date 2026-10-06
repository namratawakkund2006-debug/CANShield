# CANShield

### Lightweight CAN Message Authentication and Replay Protection Gateway

## Problem Statement

CAN (Controller Area Network) is widely used for communication between Electronic Control Units (ECUs) in vehicles.

However, a basic CAN frame does not inherently provide cryptographic authentication of the message origin. An attacker who gains access to the CAN bus may attempt to inject a message using a legitimate-looking CAN identifier.

CAN communication is also vulnerable to replay attacks, where a previously recorded legitimate message is transmitted again.

CANShield addresses these two security concerns:

- Unauthorized message injection
- Replay of previously valid CAN messages

## Proposed Solution

CANShield is a lightweight security gateway designed to verify the authenticity and freshness of CAN messages before allowing them into the trusted network.

The gateway performs two primary checks:

1. **Message Authentication**  
   Verifies whether the message is associated with an authorized ECU possessing the correct cryptographic key.

2. **Freshness Verification**  
   Checks whether the message is new or whether an old valid message is being replayed.

Based on these checks, the gateway either accepts or blocks the message.

## System Overview

```text
Legitimate ECU
      |
      v
   CAN Bus
      |
      v
+----------------------+
|   CANShield Gateway  |
|                      |
| Authentication       |
|        +             |
| Freshness Check      |
+----------+-----------+
           |
      +----+----+
      |         |
      v         v
   ACCEPT     BLOCK

Attack Scenarios
1. Legitimate Message
AUTHENTICATION: VALID
FRESHNESS: VALID

MESSAGE ACCEPTED


2. Message Injection
AUTHENTICATION: INVALID

INJECTION BLOCKED


3. Replay Attack
AUTHENTICATION: VALID
FRESHNESS: INVALID

REPLAY BLOCKED


Project Architecture
ESP32 #1
Legitimate ECU
      |
      v
CAN Transceiver
      |
      +---------------- CAN BUS ----------------+
                                               |
ESP32 #3                                      |
Attacker                                       |
      |                                        |
CAN Transceiver                                |
                                               v
                                    ESP32 #2
                                    CANShield
                                    Security Gateway
                                               |
                                               v
                                         OLED Display


Demonstration

Legitimate Message
        |
        v
   Authentication
        |
        v
    Freshness
        |
        v
       Accept

Fake Message
     |
     v
Authentication
     |
     v
   INVALID
     |
     v
   BLOCK

Replayed Message
       |
       v
 Authentication
       |
       v
     VALID
       |
       v
 Freshness Check
       |
       v
     INVALID
       |
       v
     BLOCK


 Objectives

- Demonstrate CAN message authentication.
- Detect unauthorized CAN message injection.
- Detect replayed CAN messages.
- Implement freshness verification using a counter mechanism.
- Develop a low-cost proof-of-concept security gateway.
- Demonstrate the security decision in real time.

 Technology Stack

- ESP32
- CAN communication
- CAN transceivers
- Embedded C/C++
- Cryptographic authentication
- Freshness / counter mechanism
- OLED display
- Serial debugging
- CAN bus monitoring

Future Scope

The prototype can be extended toward:

- Automotive-grade hardware
- CAN-FD support
- Hardware-based secure key storage
- Advanced key management
- Integration with automotive cybersecurity architectures
- More sophisticated intrusion detection
- Vehicle-level testing and validation

 Project Status

Current Stage: Prototype Development

CANShield is being developed as an educational proof-of-concept to demonstrate CAN message authentication and replay protection using embedded hardware.




