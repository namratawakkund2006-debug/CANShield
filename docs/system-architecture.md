# CANShield - System Architecture

## Overview

CANShield is designed as a lightweight security gateway placed between CAN communication nodes.

The gateway evaluates incoming CAN messages before allowing them to proceed as trusted messages.

The prototype consists of:

- Legitimate ECU
- CANShield Security Gateway
- Attacker Node
- CAN Bus
- Display for security status

## Basic Architecture

```text
        Legitimate ECU
             |
             v
        +----------+
        |  CAN Bus |
        +----------+
             |
             v
     +----------------+
     |   CANShield    |
     | Security       |
     | Gateway        |
     +----------------+
             |
       +-----+-----+
       |           |
       v           v
 Authentication  Freshness
     Check          Check
       |           |
       +-----+-----+
             |
       +-----+-----+
       |           |
       v           v
    ACCEPT       BLOCK


Legitimate Communication
A legitimate ECU generates a CAN message and transmits it onto the CAN bus.

The CANShield gateway receives the message and performs security verification.
Legitimate ECU
      |
      v
  CAN Message
      |
      v
Authentication
      |
      v
Freshness Check
      |
      v
   ACCEPT


Unauthorized Message
An attacker node may attempt to inject a CAN message using a legitimate-looking CAN identifier.

CANShield verifies the message authentication information.

If authentication fails, the message is rejected.

Attacker Node
      |
      v
 Fake CAN Message
      |
      v
Authentication
      |
      v
   INVALID
      |
      v
    BLOCK


Gateway Decision
CANShield makes the final security decision based on authentication and freshness.

Incoming CAN Message
          |
          v
   Authentication
       Check
          |
     +----+----+
     |         |
   VALID     INVALID
     |         |
     v         v
 Freshness    BLOCK
   Check
     |
 +---+---+
 |       |
VALID   INVALID
 |       |
 v       v
ACCEPT  BLOCK


Prototype Nodes
The prototype can use multiple low-cost embedded nodes:

ESP32 #1
Legitimate ECU
      |
      v
CAN Transceiver
      |
      v
   CAN Bus
      |
  +---+----------------+
  |                    |
  v                    v
CANShield Gateway   Attacker Node
  |                    |
  v                    v
Security Check     Injection /
  |                Replay Test
  v
ACCEPT/BLOCK


Security Functions

The prototype focuses on two main security functions:
1. Message Authentication

Checks whether the received message contains valid authentication information associated with an authorized sender.
Authentication = VALID
        AND
Freshness = VALID
        |
        v
      ACCEPT

2. Freshness Verification
Checks whether the message is new or whether it is an older message being replayed.
Authentication = INVALID
        OR
Freshness = INVALID
        |
        v
      BLOCK




