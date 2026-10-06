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
