# Odirve config guide
## Calibrating the odrive 
1. Connect odrive to pc via usb
2. Enter odrive utility. if odrive is connected correctly, you should see some OK messages
```bash
odrivetool
```
3. Now you can start entering commands to configure odrive.
4. To start calibration sequence run:
```bash
# for axis 0
dev0.axis0.requested_state=AXIS_STATE_FULL_CALIBRATION_SEQUENCE

#for axis 1
dev0.axis1.requested_state=AXIS_STATE_FULL_CALIBRATION_SEQUENCE
```
6. Save the configuration
```bash
dev0.save_configuration()
```


## Getting errors
- To check for errors on odrive, run:
```bash
dump_errors(dev0)
```
- To clear errors run:
```bash
dev0.clear_errors()
```

## To Get/Set configuration of specific device
- get encoder configuration
```bash
dev0.axis0.encoder
```
- get motor configuration
```bash
dev0.axis0.motor
```
- Set encoder configuration
```bash
dev0.axis0.encoder.config.[value of the seting to change] = [value]
```
- Set motor configuration
```bash
dev0.axis0.motor.config.[value of the seting to change] = [value]
```

## Load configuration from json file
- connect the odive to pc
- Load configuration from json file
```bash
odrivetool restore-config config.json 
```
- Enter odrive utility
```bash
odrivetool
```
- Save the configuration
```bash
dev0.save_configuration()
```
## Save configuration to json file
- connect the odive to pc
- backup the configuration to json file
```bash
odrivetool backup-config
```


# Problemns
### MotorError errors
After calibreting odrive without saving the configuration, the motors seem to be working and after saving the configuration the motors are not working anymore. The errors are usually:
```bash
axis: Error(s):
    AxisError.MOTOR_FAILED
  motor: Error(s):
    MotorError.UNKNOWN_PHASE_ESTIMATE
``` 
1. Calibrate the engine
```bash
dev0.axis1.requested_state=AXIS_STATE_FULL_CALIBRATION_SEQUENCE
```
2. Change the encoder.config.pre_calibrated to True (because it is not loaded correctly from backup of odrive config) 
```bash
# for example set this for axis 1
dev0.axis1.encoder.config.pre_calibrated = True
```
3. Save the configuration
```bash
dev0.save_configuration()
```
4. Now it should work correctly

## WARNING
- sometimes configuration is not loaded correctly from json backup. [ if there is some weard errors try to check the config to see if values in odrve are different form the backup. Then set the values manually and save the configuration]


# Helpfull links
- [Odrive geting started](https://docs.odriverobotics.com/v/0.5.4/getting-started.html#install-odrivetool)

- [Odrive firmware install docs](https://docs.odriverobotics.com/v/0.5.4/odrivetool.html?highlight=elf#device-firmware-update)

- [Odrive firmware](https://github.com/odriverobotics/ODrive/releases)