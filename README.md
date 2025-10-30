# Firmware (cleaning_bot .ino) — WaterCleaningRobot

This README documents the Arduino firmware (.ino) used for the WaterCleaningRobot: purpose, wiring (pin mapping), build/upload steps, runtime behavior, configuration points in the sketch, serial/debugging interface, and quick troubleshooting.

Important
- Replace <FIRMWARE.ino> with the actual .ino filename in this repo.
- Update pin numbers, sensor types, and any project-specific notes below to match your hardware.

Overview
- Purpose: low-level motor and sensor control for the WaterCleaningRobot.
- Platform: Arduino-compatible board (e.g., Arduino Uno / Nano / Mega). Confirm board and voltage before connecting motors.
- Sketch file: <FIRMWARE.ino>

Required hardware (example — update to your BOM)
- Microcontroller: Arduino Uno / Nano / Mega
- Motor driver: L298N / TB6612 / similar
- Motors: 2x DC motors (left/right) or thrusters
- Ultrasonic sensor: HC-SR04 (optional)
- IMU: (optional)
- Power: battery (with regulator if needed)
- Wiring connectors, switches, and fuses as appropriate

Pin mapping (update this block to match the sketch)
- LEFT_MOTOR_PWM: D5
- LEFT_MOTOR_DIR: D4
- RIGHT_MOTOR_PWM: D6
- RIGHT_MOTOR_DIR: D7
- ULTRASONIC_TRIG: D8
- ULTRASONIC_ECHO: D9
- STATUS_LED: D13
- SERIAL BAUD: 115200

(If your sketch uses different pin names or additional peripherals, list them here and update the sketch defines.)

Configuration (inside the .ino)
- Motor pins and sensor pins are declared as #define or const variables at the top of the sketch. Example:
  - #define LEFT_MOTOR_PWM 5
  - #define SERIAL_BAUD 115200
- PID or controller gains (if present) are in a configuration section near the top for easy tuning.
- Safety limits: check for constants like MAX_PWM, BATTERY_VOLTAGE_CUTOFF, and EMERGENCY_STOP_PIN.

Build & upload

Arduino IDE
1. Open Arduino IDE.
2. File → Open → select <FIRMWARE.ino>.
3. Tools → Board → select the appropriate board (e.g., Arduino Uno).
4. Tools → Port → select your device port.
5. Click Upload.

PlatformIO (VS Code)
1. Create platformio.ini with the correct board (e.g., uno).
2. Place <FIRMWARE.ino> under src/ (rename to main.cpp if needed).
3. Run: pio run --target upload

Serial / Debugging
- Serial baud: 115200 (or as defined in the sketch).
- Typical output:
  - Startup banner (firmware version)
  - Sensor readings (distance, IMU)
  - Motor command acknowledgements
  - Error/status messages
- Simple serial commands (examples — verify actual commands in your sketch):
  - "STATUS" → prints current status
  - "MOTOR L 100" → set left motor PWM to 100
  - "STOP" → emergency stop

Runtime behavior
- On power-up, the sketch initializes serial, I/O pins, sensors, and performs a short self-check (LED blink + serial banner).
- Safety: the firmware includes an emergency-stop condition when a sensor reads out-of-range or on command.
- Control loop frequency: typically 10–50 Hz depending on sensors used.

Troubleshooting
- Upload fails:
  - Verify board and port in IDE.
  - Disconnect motor power during upload if ground loops or noise cause resets.
- Motors don't spin:
  - Check motor driver EN pins and power rails.
  - Verify direction pin wiring and PWM pin mapping.
- Sensors return garbage:
  - Confirm wiring (Vcc, GND, TRIG/ECHO or I2C/SPI lines).
  - Ensure correct voltage levels (use level shifter if necessary).
- Serial shows no output:
  - Confirm correct baud rate.
  - If using USB-serial adapter, ensure drivers are installed.

Safety notes
- Never connect motors directly to Arduino power pins.
- Place fuses between battery and motor driver.
- Test motors and propellers out of water first.
- Use caution with LiPo batteries — follow safe charging/storage procedures.

Where to change behavior
- Motor control functions: look for functions like setMotorSpeed(), drive(), stopMotors().
- Sensor reading: functions such as readUltrasonic(), readIMU(), or similar.
- PID: tune constants in the PID section (Kp, Ki, Kd).

What I need from you to finalize
- The exact .ino filename and the top-of-file pin defines (or paste the top ~50 lines of the sketch).
- Confirm the board type (Uno/Nano/Mega/Pro Micro).
- Any special serial command set implemented in the sketch.

License and credits
- Keep the repository LICENSE file updated (e.g., MIT / Apache-2.0).
- Author: mamatakhatke

If you want, I can:
- Generate a minimal README filled with exact pin mapping and example serial commands if you paste the .ino header or the pin/define section here.
