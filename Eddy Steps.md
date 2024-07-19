- Update to the latest elegoo firmware
- Replace Macros, (see snippets)
- Comment/Remove `max_accel_to_decel` in `printer` section of `printer.cfg`
- Replace `Probe` section in config, as above
- Replace `moonraker.conf` (see snippets) >> probably means to remove the deprecated sections
- Update klipper to master >> This is probably not correct for everyone. Build the firmware normal.
  - `cd ~/klipper`
  - make menuconf
  - Follow the recommendations for the mcu
  - plugin the BTT Eddy while holding the boot button
  - run `lsusb` to make sure the Eddy has an address
  - `make clean`
  - `make flash FLASH_DEVICE=2e8a:0003`
  - find device `ls /dev/serial/by-id/*`
  - Take note of the name, it's not the same for some. You will use this in the printer.cfg
- Update klipper with eddy and n4 support
  - `git remote add n4.klipper https://git.krakjoe.dev/n4.klipper.git`
  - `git fetch n4.klipper`
  - `git checkout n4/eddy`
- Update moonraker to master with n4 support
  - `cd ~/moonraker`
  - `git remote add n4.moonraker https://git.krakjoe.dev/n4.moonraker.git`
  - `git fetch n4.moonraker`
  - `git reset --hard`
  - `git checkout n4/master`
  - Replace `deb http://deb.debian.org/debian buster-backports main contrib non-free` in `/etc/apt/sources.list` with `deb http://archive.debian.org/debian buster-backports main contrib non-free`
  - `./scripts/data-path-fix.sh`
- Update fluidd
  - Use `kiauh` (`kiauh` itself requires and will prompt for an update first, this is safe)
- Restart system services
  - `systemctl restart klipper`
  - `systemctl restart moonraker`

Before you run levelling using procedure documented in eddy repo, you must edit elegoo_conf.ini:

Under `[printer_offset]`
```
z_offset                       = 0.00
```
Under `[level_mode]`
```
level_points                   = 6
```

Will ensure your G29 (from snippets) macro works; it will cause the elegoo firmware to load the profile named 6 whenever it loads a profile.

This profile should represent the entire bed, do not use this name for adaptive meshing.

The screen must not be used for probe calibration or meshing, the procedure outlined by BTT must now be followed

Here is my probe section, all changes required by eddy are contained in this section, this should replace the section with the same header in configuration:

### Probe section

 ```ini
#####################################################################
#     Probe
#####################################################################

[mcu eddy]
serial: /dev/serial/by-id/usb-Klipper_rp2040_eddy-if00

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
mesh_min: 21,21
mesh_max: 300.75,300.75
probe_count: 25, 25
algorithm: bicubic

[safe_z_home]
home_xy_position: 189.25,144.55 
speed: 200
z_hop: 10                 
z_hop_speed: 25

[screws_tilt_adjust]
screw1_name: front left screw
screw1: 55,10
screw2_name: rear left screw
screw2: 55,180
screw3_name: rear right screw
screw3: 225,180
screw4_name: front right screw
screw4: 225,10
horizontal_move_z: 10
speed: 200
screw_thread: CW-M4  # Use CW for Clockwise and CCW for Counter Clockwise

#[probe]
#pin:^PA11
#x_offset: -24.25
#y_offset: 20.45
#z_offset: 0.0
#speed: 10.0
#samples: 2
#samples_result: median
#sample_retract_dist: 3.0
#samples_tolerance: 0.05
#samples_tolerance_retries: 5

#[bed_mesh]
#speed:500                
#horizontal_move_z:10     
#mesh_min:15,21           
#mesh_max:210,211        
#probe_count:6,6          
#algorithm:bicubic
#bicubic_tension:0.2
#mesh_pps: 2, 2   
#fade_start:5.0
#fade_end:30.0  
```
This is correct for plus, using the other configurations as a base, you can copy or derive the correct values for mesh_min/max etc ...
Once you have switched branches, the "virtual mcu" (which is a linux process), must be recompiled using the normal make flash workflow, you may either go through menuconfig and configure the linux process build (and document how to do this), or you may use the virtualmcu.config from the ON project (and document how to do that) ...

So far, elegoo have included klipper and moonraker in their update packages, so installing elegoo provided updates over this mod will wipe out the support.
If they don't, and continue to make changes to klipper and moonraker, users will have to apply the update and set (or reset) branches to branches with those changes ported ... (edited)
afaict I'm licensed to repackage the updates contained in ELEGOO_UPDATE_DIR (just not screen firmware blobs), that's also an option ...
