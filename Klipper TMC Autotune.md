# Klipper TMC Autotune Notes

Notes on my experimentation with getting the Klipper autotune working on my Elegoo Neptune 4 Pro

## Installation

Read and install: https://github.com/andrewmcgr/klipper_tmc_autotune

## Configuration

Based on what SilentedFrost has discovered these are some of the attributes of the steppers. I can confirm that I am using the BJ42D22-53V04 for the X, assuming the Y will be identical. I will have to find evidence for the others.

![image](https://github.com/user-attachments/assets/75a469f6-d764-46fc-84b8-a697ec7c54df)

Specs on the steppers: https://www.3djake.com/elegoo/stepper-motor-4


```ini
########################################
# Klipper TMC Autotune configuration
########################################

[motor_constants bj42d22-53v04]             ; 1.2A version of the stepper motor
resistance: 2.1
inductance: 0.0046
holding_torque: 0.4
max_current: 1.20
steps_per_revolution: 200

[| autotune_tmc stepper_x]
motor: bj42d22-53v04
tuning_goal: auto

[| autotune_tmc stepper_y]
motor: bj42d22-53v04
tuning_goal: auto
```

## Fixing Packages

### Enum error
https://github.com/andrewmcgr/klipper_tmc_autotune/issues/40

run:
```bash
source ~/klippy-env/bin/activate
pip install enum
deactivate
```

### Signature error

run:
```bash
source ~/klippy-env/bin/activate
pip install funcsigs
deactivate
```

Change this line:
https://github.com/andrewmcgr/klipper_tmc_autotune/blob/main/| autotune_tmc.py#L3
```python
import math, logging, os
from enum import Enum
from inspect import signature
from . import tmc
```
to
```python
import math, logging, os
from enum import Enum
#from inspect import signature
from funcsigs import signature
from . import tmc
```
Probably a better way to do this so it can try the first and fall back on the second

## Performance

Since I only have the XY steppers tuning I will do an X offset print and compare times and the sound.

| Auto | Silent | Performance |
| :--- | :----- | :---------- |
| 5m25s | 5m24s | 5m24s |
| autotune_tmc set stepper_x pwm_freq=2 | autotune_tmc set stepper_x pwm_freq=2 | autotune_tmc set stepper_x pwm_freq=2 |
| autotune_tmc stepper_x ncycles=219 pfdcycles=73 | autotune_tmc stepper_x ncycles=219 pfdcycles=73 | autotune_tmc stepper_x ncycles=219 pfdcycles=73 |
| autotune_tmc set stepper_x tbl=1 | autotune_tmc set stepper_x tbl=1 | autotune_tmc set stepper_x tbl=1 |
| autotune_tmc set stepper_x toff=1 | autotune_tmc set stepper_x toff=1 | autotune_tmc set stepper_x toff=1 | 
| autotune_tmc seting hysteresis based on 24.0 V | autotune_tmc seting hysteresis based on 24.0 V | autotune_tmc seting hysteresis based on 24.0 V |
| dcoilblank = 0.010435, dcoilsd = 0.003278 | dcoilblank = 0.010435, dcoilsd = 0.003278 | dcoilblank = 0.010435, dcoilsd = 0.003278 |
| hysteresis = 0, htotal = 0, hstrt = 1, hend = -1 | hysteresis = 0, htotal = 0, hstrt = 1, hend = -1 | hysteresis = 0, htotal = 0, hstrt = 1, hend = -1 |
| autotune_tmc set stepper_x hstrt=0 | autotune_tmc set stepper_x hstrt=0 | autotune_tmc set stepper_x hstrt=0 |
| autotune_tmc set stepper_x hend=2 | autotune_tmc set stepper_x hend=2 | autotune_tmc set stepper_x hend=2 |
| autotune_tmc set stepper_x sgthrs=40 | autotune_tmc set stepper_x sgthrs=40 | autotune_tmc set stepper_x sgthrs=40 |
| autotune_tmc using max PWM speed 176.661987 | autotune_tmc using max PWM speed 176.661987 | autotune_tmc using max PWM speed 176.661987 |
| autotune_tmc set stepper_x pwm_autoscale=True | autotune_tmc set stepper_x pwm_autoscale=True | autotune_tmc set stepper_x pwm_autoscale=True |
| autotune_tmc set stepper_x pwm_autograd=True | autotune_tmc set stepper_x pwm_autograd=True | autotune_tmc set stepper_x pwm_autograd=True |
| autotune_tmc set stepper_x pwm_grad=15 | autotune_tmc set stepper_x pwm_grad=15 | autotune_tmc set stepper_x pwm_grad=15 |
| autotune_tmc set stepper_x pwm_ofs=33 | autotune_tmc set stepper_x pwm_ofs=33 | autotune_tmc set stepper_x pwm_ofs=33 |
| autotune_tmc set stepper_x pwm_reg=15 | autotune_tmc set stepper_x pwm_reg=15 | autotune_tmc set stepper_x pwm_reg=15 |
| autotune_tmc set stepper_x pwm_lim=4 | autotune_tmc set stepper_x pwm_lim=4 | autotune_tmc set stepper_x pwm_lim=4 |
| autotune_tmc set stepper_x tpwmthrs=1048575 | autotune_tmc set stepper_x tpwmthrs=0 | autotune_tmc set stepper_x tpwmthrs=1048575 |
| autotune_tmc set stepper_x en_spreadcycle=True | autotune_tmc set stepper_x en_spreadcycle=False | autotune_tmc set stepper_x en_spreadcycle=True |
| autotune_tmc set stepper_x tcoolthrs=391(24.0) | autotune_tmc set stepper_x tcoolthrs=391(24.0) | autotune_tmc set stepper_x tcoolthrs=391(24.0) |
| autotune_tmc set stepper_x semin=2 | autotune_tmc set stepper_x semin=2 | autotune_tmc set stepper_x semin=2 |
| autotune_tmc set stepper_x semax=4 | autotune_tmc set stepper_x semax=4 | autotune_tmc set stepper_x semax=4 |
| autotune_tmc set stepper_x seup=3 | autotune_tmc set stepper_x seup=3 | autotune_tmc set stepper_x seup=3 |
| autotune_tmc set stepper_x sedn=2 | autotune_tmc set stepper_x sedn=2 | autotune_tmc set stepper_x sedn=2 |
| autotune_tmc set stepper_x seimin=1 | autotune_tmc set stepper_x seimin=1 | autotune_tmc set stepper_x seimin=1 |
| autotune_tmc set stepper_x iholddelay=12 | autotune_tmc set stepper_x iholddelay=12 | autotune_tmc set stepper_x iholddelay=12 |
| autotune_tmc set stepper_x multistep_filt=True | autotune_tmc set stepper_x multistep_filt=True | autotune_tmc set stepper_x multistep_filt=True |
| autotune_tmc set stepper_y pwm_freq=2 | autotune_tmc set stepper_y pwm_freq=2 | autotune_tmc set stepper_y pwm_freq=2 |
| autotune_tmc stepper_y ncycles=219 pfdcycles=73 | autotune_tmc stepper_y ncycles=219 pfdcycles=73 | autotune_tmc stepper_y ncycles=219 pfdcycles=73 |
| autotune_tmc set stepper_y tbl=1 | autotune_tmc set stepper_y tbl=1 | autotune_tmc set stepper_y tbl=1 |
| autotune_tmc set stepper_y toff=1 | autotune_tmc set stepper_y toff=1 | autotune_tmc set stepper_y toff=1 |
| autotune_tmc seting hysteresis based on 24.0 V | autotune_tmc seting hysteresis based on 24.0 V | autotune_tmc seting hysteresis based on 24.0 V |
! dcoilblank = 0.010435, dcoilsd = 0.004006 | dcoilblank = 0.010435, dcoilsd = 0.004006 | dcoilblank = 0.010435, dcoilsd = 0.004006 |
! hysteresis = -1, htotal = -1, hstrt = 1, hend = -2 | hysteresis = -1, htotal = -1, hstrt = 1, hend = -2 | hysteresis = -1, htotal = -1, hstrt = 1, hend = -2 |
| autotune_tmc set stepper_y hstrt=0 | autotune_tmc set stepper_y hstrt=0 | autotune_tmc set stepper_y hstrt=0 |
| autotune_tmc set stepper_y hend=1 | autotune_tmc set stepper_y hend=1 | autotune_tmc set stepper_y hend=1 |
| autotune_tmc set stepper_y sgthrs=40 | autotune_tmc set stepper_y sgthrs=40 | autotune_tmc set stepper_y sgthrs=40 |
| autotune_tmc using max PWM speed 171.091564 | autotune_tmc using max PWM speed 171.091564 | autotune_tmc using max PWM speed 171.091564 |
| autotune_tmc set stepper_y pwm_autoscale=True | autotune_tmc set stepper_y pwm_autoscale=True | autotune_tmc set stepper_y pwm_autoscale=True |
| autotune_tmc set stepper_y pwm_autograd=True | autotune_tmc set stepper_y pwm_autograd=True | autotune_tmc set stepper_y pwm_autograd=True |
| autotune_tmc set stepper_y pwm_grad=15 | autotune_tmc set stepper_y pwm_grad=15 | autotune_tmc set stepper_y pwm_grad=15 |
| autotune_tmc set stepper_y pwm_ofs=40 | autotune_tmc set stepper_y pwm_ofs=40 | autotune_tmc set stepper_y pwm_ofs=40 |
| autotune_tmc set stepper_y pwm_reg=15 | autotune_tmc set stepper_y pwm_reg=15 | autotune_tmc set stepper_y pwm_reg=15 |
| autotune_tmc set stepper_y pwm_lim=4 | autotune_tmc set stepper_y pwm_lim=4 | autotune_tmc set stepper_y pwm_lim=4 |
| autotune_tmc set stepper_y tpwmthrs=1048575 | autotune_tmc set stepper_y tpwmthrs=0 | autotune_tmc set stepper_y tpwmthrs=1048575 |
| autotune_tmc set stepper_y en_spreadcycle=True | autotune_tmc set stepper_y en_spreadcycle=False | autotune_tmc set stepper_y en_spreadcycle=True |
| autotune_tmc set stepper_y tcoolthrs=391(24.0) | autotune_tmc set stepper_y tcoolthrs=391(24.0) | autotune_tmc set stepper_y tcoolthrs=391(24.0) |
| autotune_tmc set stepper_y semin=2 | autotune_tmc set stepper_y semin=2 | autotune_tmc set stepper_y semin=2 |
| autotune_tmc set stepper_y semax=4 | autotune_tmc set stepper_y semax=4 | autotune_tmc set stepper_y semax=4 |
| autotune_tmc set stepper_y seup=3 | autotune_tmc set stepper_y seup=3 | autotune_tmc set stepper_y seup=3 |
| autotune_tmc set stepper_y sedn=2 | autotune_tmc set stepper_y sedn=2 | autotune_tmc set stepper_y sedn=2 |
| autotune_tmc set stepper_y seimin=1 | autotune_tmc set stepper_y seimin=1 | autotune_tmc set stepper_y seimin=1 |
| autotune_tmc set stepper_y iholddelay=12 | autotune_tmc set stepper_y iholddelay=12 | autotune_tmc set stepper_y iholddelay=12 |
| autotune_tmc set stepper_y multistep_filt=True | autotune_tmc set stepper_y multistep_filt=True | autotune_tmc set stepper_y multistep_filt=True |

