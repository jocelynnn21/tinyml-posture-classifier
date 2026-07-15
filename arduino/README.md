# Arduino Sketch

Put the Arduino sketch used to run live inference here (typically `nano_ble33_sense_accelerometer.ino` or similar, generated/adapted from the Edge Impulse example sketch bundled with the deployment library in `../edge-impulse/deployment/`).

## Target hardware
- Arduino Nano 33 BLE Sense
- Onboard accelerometer + gyroscope (LSM9DS1)

## Running it
1. Flash the script according to your system
2. Run terminal command `edge-impulse-run-impulse` to view live good/slouch predictions and probabilities.

## Notes
- BLE output (`ArduinoBLE`) was attempted for wireless posture reporting but was not completed due to Bluetooth library issues. The sketch in this repo outputs to serial/CLI only.
