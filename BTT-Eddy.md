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

I will refer back to the BTT documenatin for this: [Compiling Firmware](https://github.com/bigtreetech/Eddy?tab=readme-ov-file#compiling-firmware)


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
mesh_min: 15, 31.66 # for N4 Pro
mesh_max: 198.84, 220.00
probe_count: 21, 21 # keeping it an odd number will give you a midpoint.
algorithm: bicubic

[safe_z_home]
home_xy_position: 141,98 
speed: 200
z_hop: 10                 
z_hop_speed: 25
```

# Step 4 - Configuring your Eddy

This step is all on on Big Tree Tech's side. [Follow their documents](https://github.com/bigtreetech/Eddy/blob/master/README.md)


# Notes

## Using z_offset for Your Filament

While it is great that the Eddy always works from a 0 Z offset it doesn't allow for adjusting the "squish" you want when you lay down that first layer. There are several ways to do this. BTT has their own macros, In Ocra Slicer you can add the Z offset the start G-Code... but how do you get there? Here is what I like to use, others have their own methods... and it can be debated ad nauseam.

I start by running a first layer test, and stepping the tool head up or down as needed. I have the option of using either the Elegoo UI with 0.01 steps or Fluidd with 0.005 steps. When you get that perfect "squish" then finish the print and run these commands
```
Z_OFFSET_APPLY_PROBE
SAVE_CONFIG
```
Klipper will restart and your z_offset will be 0 again.

- Do you need to repeat steps continuously? No. Not unless you lost the mapping completely. You only really need to redo your first layer test when you change the filament or your bed moves a bit.
- Do you need to run the tilt screw marco? Only if you think your bed needs to be retramed. Then redo your first layer test, you may have to do the paper trick first if it was off-level a lot.
- Do I need to save the Z_OFFSET? You should, but if you don't when you shut down your printer or restart Klipper it will reset to 0 and lose the baby steps you worked hard to find.

## Adaptive Meshing

Back in January 2024 Klipper brought in the excellent use of KAMP. To use adaptive meshing with Eddy you only need to update your `[PRINT_START]` gcode.
Example Only: Your PRINT_START may be different.
```ini
[gcode_macro PRINT_START]         
gcode:
    SAVE_VARIABLE VARIABLE=was_interrupted VALUE=True
	  G92 E0                                         
    BED_MESH_CLEAR                                     
	  G90             
    BED_MESH_CALIBRATE ADAPTIVE=1 METHOD=scan SCAN_MODE=rapid algorithm=bicubic #PROFILE=6          
    CLEAR_PAUSE
    M117 Printing
```
Note: Elegoo is specifically looking at PROFILE 6 when showing it on the UI. Since we want adaptive meshing `ADAPTIVE=1` we will not load `PROFILE=6`. 
![image](https://github.com/user-attachments/assets/a81073c4-ef20-42e4-b36e-1155fd7602db)




