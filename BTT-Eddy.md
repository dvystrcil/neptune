# How to install BTT Eddy for the Elegoo Neptune 4 Series 3D Printer

## Notes for now


### Requirements

- Own the BTT Eddy
- Update your printer with the krakjoe's Klipper and Moonraker. This is all in the README.md
- Watch the [BTT Eddy installation guide](https://drive.google.com/file/d/1uXiymZxoWhvRIwTwojOh0fGKmjitoytf/view)
- Read the [BTT Eddy instllation docs](https://github.com/bigtreetech/Eddy/blob/master/README.md)

Note: Ignore the steps to use Kiauh from the BTT instructions! You did these steps following krakjoe's Klipper and Moonraker method.

# Step 1 - Install the Eddy

There are a couple ways to do this and there are several YouTubes out there that cover it. 

I used the BTT-Eddy_Adapter_v05.stl mounting bracket for my printer:
https://www.printables.com/model/928061-neptune-4-btt-eddy-adapter

# Step 2 - Compiling the firmware


# Step 3 - Updating your printer.cfg

Add these marcos to your printer.cfg.
- Remove the old `[probe]` section
- You will need to modify the `[bed_mesh]` section to account for the new position of the probe. Remember the Eddy is chucky. The original stock probe is not. The settings bellow I found are good for my Neptune 4 Pro AND the mount I chose to use.  

```ini
#####################################################################
# 	Probe
#####################################################################

[mcu eddy]
serial: /dev/serial/by-id/usb-Klipper_rp2040_45474E621B0483CA-if00

[temperature_sensor btt_eddy_mcu]
sensor_type: temperature_mcu
sensor_mcu: eddy
min_temp: 10
max_temp: 100

[probe_eddy_current btt_eddy]
sensor_type: ldc1612
z_offset: 1.0
#i2c_address:
i2c_mcu: eddy
i2c_bus: i2c0f
# x_offset: -24.25
# y_offset: 20.45
# new Probe values:
#Eddy 0Deg x=-36.16 y=31.66
x_offset: -36.16
y_offset: 31.66
data_rate: 500

[temperature_probe btt_eddy]
sensor_type: Generic 3950
sensor_pin: eddy:gpio26
horizontal_move_z: 2

[bed_mesh]
horizontal_move_z: 1
speed: 400
mesh_min:15,32           
mesh_max: 197.94, 203.44 #185,211 
probe_count: 21, 21
algorithm: bicubic

[safe_z_home]
home_xy_position: 141,98 
speed: 200
z_hop: 10                 
z_hop_speed: 25
```

# Step 4 - Configuring your Eddy

This step is all on on Big Tree Tech's side. [Follow their documents](https://github.com/bigtreetech/Eddy/blob/master/README.md)