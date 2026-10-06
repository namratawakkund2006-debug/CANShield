CAN (Controller Area Network) is widely used for communication between Electronic Control Units (ECUs) in vehicles.

However, a basic CAN frame does not inherently provide cryptographic authentication of the message origin. An attacker who gains access to the CAN bus may attempt to inject a message using a legitimate-looking CAN identifier.

CAN communication is also vulnerable to replay attacks, where a previously recorded legitimate message is transmitted again.

CANShield addresses these two security concerns:

- Unauthorized message injection
- Replay of previously valid CAN messages
