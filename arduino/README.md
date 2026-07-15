# Arduino Sketch(es)

Put the Arduino sketch used to run live inference here (typically `nano_ble33_sense_accelerometer.ino` or similar, generated/adapted from the Edge Impulse example sketch bundled with the deployment library in `../edge-impulse/deployment/`).

## Target hardware
- Arduino Nano 33 BLE Sense
- Onboard accelerometer + gyroscope (LSM9DS1)

## Running it
1. Install the Edge Impulse Arduino library from `../edge-impulse/deployment/`.
2. Open the sketch in the Arduino IDE, select the Nano 33 BLE Sense board.
3. Flash the sketch.
4. Run `edge-impulse-run-impulse` from the CLI to view live good/slouch predictions and probabilities.

## Notes
- BLE output (`ArduinoBLE`) was attempted for wireless posture reporting but was not completed due to Bluetooth library issues. The sketch in this repo outputs to serial/CLI only.
